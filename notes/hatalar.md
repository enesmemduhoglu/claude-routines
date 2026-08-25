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
6. **Bir veriyi "bugün açıklandı" diye yazmadan önce kurumun yayım tarihini doğrula.** Haber siteleri TÜİK/TCMB verilerini günler sonra yeniden servis ediyor; aylık anket verilerinde (RKGE, KKO, tüketici güveni) haber tarihi ≠ yayım tarihi.
7. **Bir kapanış rakamını ancak doğrulayabiliyorsan "kapanış" diye yaz.** Rutin 18:00'den önce çalışıyorsa BIST kapanışı henüz yayınlanmamış olur; son doğrulanan gün içi değeri saatiyle ver ve kapanışın doğrulanamadığını belirt.

---

## Operasyon notları

Rutinin çalıştığı bulut ortamıyla ilgili teknik bulgular.

- **`git push` çalışıyor — 24 Ağustos 2026'da doğrulandı.** Bulut oturumundan `origin` (https://github.com/enesmemduhoglu/claude-routines) adresine yazma yetkisi var; kimlik doğrulama sorunsuz.
- **Oturum "detached HEAD" ile başlıyor ve yerel `main` dalı geride kalabiliyor.** Commit'lemeden önce şunu yap:
  ```
  git checkout main && git fetch origin main && git merge --ff-only origin/main
  ```
  Bu yapılmazsa push `non-fast-forward` hatasıyla reddediliyor — bu bir yetki sorunu değil, bayat yerel ref sorunudur.
- **Egress proxy tarafından bloklu siteler (güncel liste):** bloomberght.com, investing.com, finance.yahoo.com, cnbc.com, tradingeconomics.com, aa.com.tr, ekonomim.com, bigpara.hurriyet.com.tr, altin.doviz.com, fxstreet.com, apara.com.tr, kitco.com, **tcmb.gov.tr**, **bea.gov**, halktv.com.tr, paraborsa.net, dunya.com, endeks24.com, ekonomidunya.com, yelza.com, giynikgazetesi.com, features.financialjuice.com, letterstoayounginvestor.substack.com ve doğrudan `curl` API çağrıları. `WebFetch` denemeden önce bu listeye bak; `WebSearch` özetleri çalışıyor.
- **Pratikte `WebFetch` neredeyse hiç çalışmıyor.** 25 Ağustos 2026'da denenen 10 adresin 10'u da bloklandı — resmî kaynaklar (TCMB, BEA) dahil. Varsayılan yöntem `WebSearch` özetleri olmalı; `WebFetch` sadece listede olmayan ve gerçekten kritik bir kaynak için tek denemelik son çare.

---

## Günlük

Yeni kayıtlar en üste. Format: tarih, ne yanlıştı, doğrusu, nasıl yakalandı.

### 2026-08-25

**`WebFetch` bloklarında büyük genişleme.** Denenen 10 adresin tamamı `EGRESS_BLOCKED` döndü: kitco.com, tcmb.gov.tr, bea.gov, halktv.com.tr, paraborsa.net, dunya.com, endeks24.com, ekonomidunya.com, yelza.com, giynikgazetesi.com (+ features.financialjuice.com, letterstoayounginvestor.substack.com). SKILL.md'nin birinci sıradaki güvenilirlik kaynakları (TCMB, BEA) doğrudan erişilemiyor. → Operasyon notlarına işlendi; varsayılan yöntem `WebSearch` özeti oldu.

**Kaynak yine yanlış "rekor" dedi — kural 1 yakaladı.** Bugün gzt.com/halktv "ons altın 4.600 doları aşarak tarihi zirvesini yeniledi" başlığını attı; gerçek zirve 29 Ocak 2026'da $5.602, bugünkü seviye $4.635. Kullanılmadı, bültende ATH farkı açıkça yazıldı. Kural 1 ve 2 zaten kayıtlı — yeni kural açılmadı.

**Yanlış tarihlendirilecekti: TCMB reel kesim güveni.** Arama sonuçları ağustos RKGE (102,4) ve KKO (%73,5) verilerini bugüne aitmiş gibi gösterdi; ikinci aramada yayım tarihinin 21.08.2026 olduğu görüldü. Türkiye bölümüne "bugün açıklandı" diye girmedi. → Ders: TCMB/TÜİK anket verilerinde haber tarihi değil, kurumun yayım tarihi esas alınmalı.

### 2026-08-24

**Dört yeni bloklu domain.** `bigpara.hurriyet.com.tr`, `altin.doviz.com`, `fxstreet.com` ve `apara.com.tr` `WebFetch` ile denenince `EGRESS_BLOCKED` döndü. Dördü de kural 5'teki listede yoktu; boşa dört çağrı harcandı. → Liste "Operasyon notları"na taşındı ve genişletildi.

**Doğrulanamayan BIST kapanışı.** Çalışma 17:50 TSİ'de yapıldı, borsa 18:00'de kapanıyor. Arama sonuçlarından biri "14.396,54 · %0,43 düşüş" verdi ama Cuma kapanışı 14.514,82 ile bu rakam tutarsız (%0,82 düşüşe denk geliyor) — başka bir güne ait olduğu anlaşıldı ve kullanılmadı. Bültende 16:50 gün içi değeri saatiyle verildi. → Kural 6.

**Gram altın için "rekor" denmesi.** Bültende gram ₺7.207 "rekor" olarak yazıldı. Gerçek zirve 29 Ocak 2026'da ₺7.797 — bugünkü seviye onun ~%7,5 altında. Kaynak: Türk haber sitelerinin "gram altın rekorla başladı" başlıkları doğrulanmadan alınmıştı. Ertesi çalışmada ATH araştırılınca yakalandı. → Kural 1 ve 2.

**Ters kurulmuş neden-sonuç cümlesi.** "Faizler yükselirken altının da yükselmesi, ABD varlıklarına duyulan güvene işaret ediyor" yazıldı; kastedilen güven **kaybı**ydı. Cümle olduğu gibi okununca anlamı tersine dönüyor. → Kural 3 ve 4.

**Tek kaynaktan alınan takvim bilgisi.** İlk bültende Jackson Hole 24–26 Ağustos (doğrusu 27–29) ve çekirdek PCE beklentisi %3,6 (doğrusu %3,2–3,3) yazılmıştı. Her ikisi de tek kaynağa dayanıyordu. → SKILL.md Adım 2, madde 3'te zaten kurallı.
