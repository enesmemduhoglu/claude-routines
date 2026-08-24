# Hata Defteri — Günlük Ekonomi

Bu dosyayı `gunluk-ekonomi` skill'ini çalıştıran ajan tutar. Amaç: aynı hatanın iki kez yapılmaması.

**Ajan için:** Her çalışmada bu dosyayı oku (Adım 0a). Hata tespit edersen günlüğe ekle, ders kalıcıysa yukarıdaki kurallara tek satır olarak taşı, commit'leyip push'la (Adım 4). **`SKILL.md`'yi düzenleme** — orası kullanıcının sözleşmesi.

---

## Kalıcı kurallar

Her bültende geçerli. En fazla 15 madde — doluysa eskiyen bir maddeyi çıkar.

1. **"Rekor" kelimesini haberden kopyalama.** Türk haber siteleri "altın rekor kırdı" başlığını, fiyat zirvenin çok altındayken bile atıyor. Rekor demeden önce ATH'yi bul ve bugünkü fiyatla karşılaştır. Emin değilsen "X ayın zirvesi" de.
2. **Altın zirveleri (29 Ocak 2026):** ons **$5.602**, gram **₺7.797**. Ons ve gram ayrı ayrı kontrol edilir — kur etkisiyle biri rekor kırarken diğeri kırmayabilir.
3. **Neden-sonuç cümlelerini yazdıktan sonra bir kez daha oku.** Özellikle "güven / güvensizlik", "risk iştahı / kaçış", "alım / satım baskısı" gibi ters çevrilmesi kolay ifadeleri. Cümlenin anlamı kastettiğinin tersi olmasın.
4. **Faiz yükselirken altın da yükseliyorsa** bunun okuması "ABD varlıklarına **güven kaybı**, yatırımcı ek getiri talep ediyor" yönündedir — "güven" değil.
5. **Bulut ortamında bazı siteler egress proxy tarafından bloklu.** Güncel liste aşağıdaki "Operasyon notları" bölümünde. Zaman kaybetme — `WebSearch` sonuçlarındaki özetlerle çalış, `WebFetch`'i sadece bloklu olmayan sitelerde dene.
6. **Bir kapanış rakamını ancak doğrulayabiliyorsan "kapanış" diye yaz.** Rutin 18:00'den önce çalışıyorsa BIST kapanışı henüz yayınlanmamış olur; son doğrulanan gün içi değeri saatiyle ver ve kapanışın doğrulanamadığını belirt.

---

## Operasyon notları

Rutinin çalıştığı bulut ortamıyla ilgili teknik bulgular.

- **`git push` çalışıyor — 24 Ağustos 2026'da doğrulandı.** Bulut oturumundan `origin` (https://github.com/enesmemduhoglu/claude-routines) adresine yazma yetkisi var; kimlik doğrulama sorunsuz.
- **Oturum "detached HEAD" ile başlıyor ve yerel `main` dalı geride kalabiliyor.** Commit'lemeden önce şunu yap:
  ```
  git checkout main && git fetch origin main && git merge --ff-only origin/main
  ```
  Bu yapılmazsa push `non-fast-forward` hatasıyla reddediliyor — bu bir yetki sorunu değil, bayat yerel ref sorunudur.
- **Egress proxy tarafından bloklu siteler (güncel liste):** bloomberght.com, investing.com, finance.yahoo.com, cnbc.com, tradingeconomics.com, aa.com.tr, ekonomim.com, bigpara.hurriyet.com.tr, altin.doviz.com, fxstreet.com, apara.com.tr ve doğrudan `curl` API çağrıları. `WebFetch` denemeden önce bu listeye bak; `WebSearch` özetleri çalışıyor.

---

## Günlük

Yeni kayıtlar en üste. Format: tarih, ne yanlıştı, doğrusu, nasıl yakalandı.

### 2026-08-24

**Dört yeni bloklu domain.** `bigpara.hurriyet.com.tr`, `altin.doviz.com`, `fxstreet.com` ve `apara.com.tr` `WebFetch` ile denenince `EGRESS_BLOCKED` döndü. Dördü de kural 5'teki listede yoktu; boşa dört çağrı harcandı. → Liste "Operasyon notları"na taşındı ve genişletildi.

**Doğrulanamayan BIST kapanışı.** Çalışma 17:50 TSİ'de yapıldı, borsa 18:00'de kapanıyor. Arama sonuçlarından biri "14.396,54 · %0,43 düşüş" verdi ama Cuma kapanışı 14.514,82 ile bu rakam tutarsız (%0,82 düşüşe denk geliyor) — başka bir güne ait olduğu anlaşıldı ve kullanılmadı. Bültende 16:50 gün içi değeri saatiyle verildi. → Kural 6.

**Gram altın için "rekor" denmesi.** Bültende gram ₺7.207 "rekor" olarak yazıldı. Gerçek zirve 29 Ocak 2026'da ₺7.797 — bugünkü seviye onun ~%7,5 altında. Kaynak: Türk haber sitelerinin "gram altın rekorla başladı" başlıkları doğrulanmadan alınmıştı. Ertesi çalışmada ATH araştırılınca yakalandı. → Kural 1 ve 2.

**Ters kurulmuş neden-sonuç cümlesi.** "Faizler yükselirken altının da yükselmesi, ABD varlıklarına duyulan güvene işaret ediyor" yazıldı; kastedilen güven **kaybı**ydı. Cümle olduğu gibi okununca anlamı tersine dönüyor. → Kural 3 ve 4.

**Tek kaynaktan alınan takvim bilgisi.** İlk bültende Jackson Hole 24–26 Ağustos (doğrusu 27–29) ve çekirdek PCE beklentisi %3,6 (doğrusu %3,2–3,3) yazılmıştı. Her ikisi de tek kaynağa dayanıyordu. → SKILL.md Adım 2, madde 3'te zaten kurallı.
