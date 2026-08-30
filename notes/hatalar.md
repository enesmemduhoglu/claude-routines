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
8. **Arama motoru özetleri bir önceki günün borsa/açılış rakamını "bugün" diye sunuyor.** Bir açılış veya kapanış rakamını kullanmadan önce bilinen önceki kapanışla ya da dünkü bültenle çapraz kontrol et; yüzde değişim tutmuyorsa rakam başka bir güne aittir.
9. **Endeks ve kur rakamlarında birincil kaynak Google Finance.** `WebFetch` ile erişilebiliyor (26 Ağustos 2026'da doğrulandı), gün içi canlı değeri ve saat damgasını birlikte veriyor. BIST 100: `https://www.google.com/finance/quote/XU100:INDEXIST`. Değişim yüzdesini anlık değer ile "Previous close"tan kendin hesapla.
10. **Rutin 10:30 TSİ'de çalışıyor, BIST 10:00'da açılıyor — bu saatte BIST için "önceki kapanış" verme.** Kullanıcı bu saati bilerek seçti: seans oturduktan sonraki gün içi seviyeyi görmek istiyor. Gün içi değeri kural 9'daki adresten al. Önceki kapanış sadece borsa gerçekten kapalıyken (hafta sonu/tatil) ya da canlı kaynakların hiçbirine erişilemiyorken kullanılır — ikinci durumda nedeni bültende açıkça yaz.
11. **Bir haberdeki fiyat doğru olsa bile yüzde değişimi yanlış olabilir.** Türk haber siteleri seviyeyi doğru verip yanına "%6 yükseldi" gibi uydurma bir oran ekliyor (bazen alış-satış makasını "değişim" sanıyor). Yüzdeyi haberden alma; **önceki kapanışla kendin hesapla.**
12. **Bir "açılış/kapanış" rakamının hangi güne ait olduğunu çarpma testiyle doğrula.** Bilinen önceki kapanış × (1 + iddia edilen %) = iddia edilen seviye ise rakam o güne aittir; tutmuyorsa başka bir güne aittir. Arama motorları BIST açılışlarını sürekli bir gün ileri tarihliyor. Aynı test **haftalık** yüzdeyle de yapılır: geçen cuma kapanışı × (1 + haftalık %) = bu cuma kapanışı. Cuma kapanışını doğrulamanın en güvenilir yolu budur.
13. **Borsa açıkken sana verilen "bugünkü kapanış" bir önceki günün kapanışıdır.** Arama özetleri saat 10–11'de "BIST 100 bugün %X düşüşle Y puandan kapattı" diyebiliyor. Rakamı atma — çarpma testiyle hangi güne ait olduğunu bul ve o günün kapanışı olarak kullan.
14. **Tarih/bayatlık testini sadece BIST'e uygulama — kur, Brent, tahvil faizi ve dolar endeksine de uygula.** Canlı kaynaklar bloklu olduğunda arama özetleri kur ve emtia için de bir önceki günün seviyesini "bugün" diye veriyor; bu rakamlar sakin göründüğü için fark edilmeden geçiyor. Her rakamı dünkü bültendeki değerle karşılaştır: değişim tam olarak sıfırsa veri büyük ihtimalle bayattır, "yatay" değil. **Bir rakama "yatay" yazmak üzereysen dur ve o rakamı ikinci bir kaynakla doğrula** — sakinlik çoğu zaman gerçek değil, bayatlık işaretidir.

---

## Operasyon notları

Rutinin çalıştığı bulut ortamıyla ilgili teknik bulgular.

- **`git push` çalışıyor — 24 Ağustos 2026'da doğrulandı.** Bulut oturumundan `origin` (https://github.com/enesmemduhoglu/claude-routines) adresine yazma yetkisi var; kimlik doğrulama sorunsuz.
- **Oturum "detached HEAD" ile başlıyor ve yerel `main` dalı geride kalabiliyor.** Commit'lemeden önce şunu yap:
  ```
  git checkout main && git fetch origin main && git merge --ff-only origin/main
  ```
  Bu yapılmazsa push `non-fast-forward` hatasıyla reddediliyor — bu bir yetki sorunu değil, bayat yerel ref sorunudur.
- **Egress proxy tarafından bloklu siteler (güncel liste):** bloomberght.com, investing.com, finance.yahoo.com, cnbc.com, tradingeconomics.com, aa.com.tr, ekonomim.com, bigpara.hurriyet.com.tr, altin.doviz.com, fxstreet.com, apara.com.tr, kitco.com, **tcmb.gov.tr**, **bea.gov**, halktv.com.tr, paraborsa.net, dunya.com, endeks24.com, ekonomidunya.com, yelza.com, giynikgazetesi.com, features.financialjuice.com, letterstoayounginvestor.substack.com, **riotimesonline.com**, **diken.com.tr**, **borsaistanbul.com**, **www.google.com / google.com (27 Ağu)**, **borsa.doviz.com**, **finans.mynet.com**, **stooq.com**, **goldprice.org**, **xe.com**, **tr.tradingview.com**, **uzmanpara.milliyet.com.tr**, **hangikredi.com**, **marketwatch.com**, **advisorperspectives.com (30 Ağu)**, **investrade.com (30 Ağu)** ve doğrudan `curl` API çağrıları. `WebFetch` denemeden önce bu listeye bak; `WebSearch` özetleri çalışıyor.
- **`google.com/finance` erişimi GÜNDEN GÜNE DEĞİŞİYOR.** 26 Ağustos 2026'da `WebFetch` ile çalıştı; **27, 28, 29 ve 30 Ağustos 2026'da üst üste `EGRESS_BLOCKED` döndü.** Yani kural 9'daki birincil kaynak garanti değil — her çalışmada bir kez dene, açılmıyorsa vakit kaybetme ve `WebSearch` özetlerine geç.
- **27 Ağustos 2026'da `WebFetch` hiçbir adreste çalışmadı.** Denenen 10 adresin 10'u da bloklandı: www.google.com, google.com, borsa.doviz.com, finans.mynet.com, stooq.com, goldprice.org, www.xe.com, tr.tradingview.com, uzmanpara.milliyet.com.tr, www.hangikredi.com (+ www.marketwatch.com "unable to fetch"). O gün bütün bülten `WebSearch` özetleriyle yazıldı.
- **28 Ağustos 2026'da da `WebFetch` hiçbir adreste çalışmadı — üst üste ikinci gün.** Denenen 13 adres bloklandı: www.google.com, open.er-api.com, api.gold-api.com, finans.sabah.com.tr, www.ekonomist.com.tr, www.turcomoney.com, www.finansopia.com, www.haberturk.com, www.foreks.com, deebi.net, www.borsagundem.com.tr, www.tradingview.com, stockanalysis.com. Ayrıca markets.ft.com ve www.wsj.com "unable to fetch" döndü. Google Finance iki gündür kapalı — **kural 9'daki birincil kaynağı artık istisna say, kural değil.**
- **Bunun dışında `WebFetch` çoğu sitede çalışmıyor.** 25 Ağustos 2026'da denenen 10 adresin 10'u da bloklandı — resmî kaynaklar (TCMB, BEA) dahil. Varsayılan yöntem `WebSearch` özetleri olmalı; `WebFetch` sadece listede olmayan ve gerçekten kritik bir kaynak için tek denemelik son çare.

---

## Günlük

Yeni kayıtlar en üste. Format: tarih, ne yanlıştı, doğrusu, nasıl yakalandı.

### 2026-08-30

**Dünkü (29 Ağustos) bültende ABD tahvil faizleri ve dolar endeksi bir gün bayattı — kural 14 vardı, uygulanmadı.** 10 yıllık "%4,68 yatay", 2 yıllık "%4,30 (~6 bp artış)" ve dolar endeksi "99,46 ▲%0,35" yazılmıştı. 28 Ağustos Cuma kapanışları %4,73, %4,34 ve ~99,6'ydı; yazılan rakamlar perşembe kapanışı ya da gün içi okumalardı. Kaynaklar: Advisor Perspectives/Trading Economics (10y %4,73), Investrade market review (2y +12 bp, DXY +%0,5 → 99,65), Reuters (DXY 99,57, EUR/USD 1,1597). Bugünkü bültene *Düzeltme* notu kondu. → Kural 14'e dolar endeksi ve "yatay yazmadan önce ikinci kaynak" eklendi.

**Metal kapanışları yine gram/ons çaprazıyla ayıklandı.** Cuma için beş farklı ons rakamı çıktı: $4.608 (USAGOLD, gün ortası), $4.631,5 (Convex), $4.582 (Türk kaynakları), $4.576 (Reuters 07:00 TSİ), $4.531 (gün içi dip iddiası). Türk kaynakları gram için "gün sonu kapanış ₺7.087" diyordu ama bu Türkiye piyasasının 18:00 kapanışı, yani Warsh konuşmasının etkisi tam oturmadan önceki değer. Hafta sonu kotasyonu ₺6.908 ile çapraz: 6.908 × 31,1035 ÷ 48,24 = $4.454 → ons kapanışı ~$4.464 doğrulandı. → Yeni kural açılmadı; SKILL Adım 2 çaprazı yeterli. Not: **canlı fiyat sayfaları (apara, sabah finans) hafta sonu bayat değer gösteriyor** — apara 30 Ağustos'ta gram için ₺7.112 veriyordu.

**Google Finance üst üste dördüncü gün bloklu.** → Operasyon notları güncellendi.

### 2026-08-29

**Dünkü (28 Ağustos) bültende Brent ve kurlar bir gün bayattı.** Brent "$88,2 ▼%0,3" yazılmıştı; Reuters uzlaşma verisine göre 28 Ağustos kapanışı $89,31 (▼39 sent, %0,43) ve dolayısıyla 27 Ağustos uzlaşması $89,70'ti. Aynı şekilde dolar/TL 48,14 ve euro/TL 56,10 yazılmıştı; bunlar 27 Ağustos seviyeleriydi, cuma sabahı kurlar zaten 48,24 ve 56,24'tü. Bugünkü bültene *Düzeltme* notu kondu. → Yeni kural 14: bayatlık testi kur ve emtiaya da uygulanmalı.

**Cuma kapanışı haftalık yüzdeyle doğrulandı.** Arama motorları yine 27 Ağustos kapanışını (14.575,51) "28 Ağustos kapanışı" diye sundu. Doğru rakam borsaningundemi'nin "haftalık %0,87 değer kazandı" haberindeki 14.641,56'ydı: 14.514,82 (21 Ağustos kapanışı) × 1,0087 = 14.641,1 ✓. Ayrıca 14.641,56 / 14.575,51 = %+0,45 günlük. → Kural 12 haftalık test ile genişletildi.

**Altın için altı farklı cuma rakamı çıktı; gram/ons çaprazı ayıkladı.** USAGOLD $4.608 (▲%0,41), Yahoo $4.650,90, Convex $4.631,5, Reuters 0157 GMT $4.576, Reuters 1452 GMT $4.563, bloomingbit $4.464 (▼~%3). Cumartesi günü Türk kaynaklarındaki gram altın ₺6.908 ile çapraz kontrol yapıldı: 6.908 × 31,1035 ÷ 48,24 = $4.453 — yani kapanış $4.464 civarıydı, ilk dört rakam gün içi/bayat değerlerdi. Gümüşte de aynı durum: $67,09 (▼%3) ara okuma, $66,15 (▼%4,48) günlük kapanış. → Yeni kural açılmadı; SKILL Adım 2'deki çapraz kontrol yeterliydi.

**Google Finance üst üste üçüncü gün bloklu.** `www.google.com` yine `EGRESS_BLOCKED` döndü. Hafta sonu olduğu için gün içi veriye ihtiyaç yoktu, bülten cuma kapanışlarıyla yazıldı. → Operasyon notları güncellendi.

### 2026-08-28

**Arama motoru 24 Ağustos açılışını üç ayrı aramada "28 Ağustos açılışı" diye verdi.** "BIST 100 açılışta %0,85 artışla 14.637,72 puana çıktı, önceki gün %0,82 ile 14.514,82'den kapatmıştı" — 14.514,82 **21 Ağustos (Cuma)** kapanışıydı, yani rakam 24 Ağustos Pazartesi açılışına aitti (deebi.net başlığı da "Borsa yeni haftaya 14.637,72 puandan başladı" diyordu). Çarpma testiyle yakalandı, kullanılmadı. → Kural 12 ikinci kez işe yaradı.

**Saat 10:34'te bir arama "BIST 100, 28 Ağustos'ta %0,24 düşüşle 14.575,51 puandan kapattı" dedi — borsa açıktı.** Rakam doğruydu ama gün yanlıştı: 14.610,92 (26 Ağustos kapanışı) × 0,9976 = 14.575,8 ✓, yani bu **27 Ağustos** kapanışıydı. Aynı gün başka bir arama BIST için 13.821,97 (%+0,49) verdi — haftalarca eski, elendi. → Yeni kural 13.

**vittarthi.com 27 Ağustos ABD kapanışlarını yanlış verdi** (S&P 7.673,04 %+0,42; Dow 53.195,36). Doğrusu AP kaynaklı veride S&P 7.730,99 (+55,29 puan), Nasdaq 26.541,35 (+411,16), Dow 53.569,44 (+105,56); puan farkı/seviye oranı iddia edilen yüzdelerle birebir tuttu. → Yeni kural açılmadı; ABD kapanışlarında "puan değişimi ÷ seviye" testi yeterli.

**Google Finance ikinci gün de bloklu; gün içi BIST yine doğrulanamadı.** 13 adres denendi, hepsi kapalı. Tabloya 27 Ağustos kapanışı kondu, nedeni bültende açıkça yazıldı. → Operasyon notları güncellendi.

### 2026-08-27

**Arama motoru, 26 Ağustos açılışını ısrarla "27 Ağustos açılışı" diye verdi.** Üç ayrı arama "27 Ağustos 2026'da BIST 100 %0,34 artışla 14.522,55 puandan açtı" dedi. Oysa bu rakam 26 Ağustos açılışıydı: 14.473,42 (= **25 Ağustos** kapanışı, dünkü bültende kayıtlı) × 1,0034 = 14.522,5. Kaynağın kendisi de (24saatgazetesi, trthaber) "26 Ağustos Çarşamba" diyordu. Çapraz kontrolle yakalandı, kullanılmadı. → Kural 8 yine işe yaradı; ayrıca **bir "açılış" rakamının hangi güne ait olduğu, bilinen önceki kapanışla çarpılarak test edilebilir.**

**Aynı arama 14.473,42'yi "26 Ağustos kapanışı" diye de sundu — o da yanlıştı.** Gerçek 26 Ağustos kapanışı 14.610 (▲ %0,95, bankacılık öncülüğünde); 14.473,42 25 Ağustos kapanışıydı. Doğrulama: 14.473,42 × 1,0095 = 14.610,9 ✓.

**Bir haber sitesi gram altın için "%6'yı aşan sert yükseliş", ons için "günlük %2,45 değer kazancı" dedi — ikisi de uydurma.** Gerçek hareketler ~%0,4 ve ~%0,3'tü. İlginç olan: aynı haberdeki **fiyat seviyeleri** (gram ₺7.132, ons $4.606) doğruydu ve çapraz kontrolü tutuyordu; yanlış olan sadece yüzdelerdi. "%6,35 fark" ifadesi de has altının alış-satış makasıydı. → Yeni kural 11.

**Google Finance bugün bloklandı, `WebFetch` hiçbir adreste çalışmadı.** Dün (26 Ağustos) çalışan `google.com/finance` bugün `EGRESS_BLOCKED` döndü; denenen 10 adresin tamamı bloklu çıktı. Bu yüzden BIST için gün içi seviye doğrulanamadı ve tabloya 26 Ağustos kapanışı, nedeni bültende açıkça yazılarak kondu (SKILL Adım 2, madde 7). → Operasyon notları güncellendi.

### 2026-08-26

**BIST 100 için gün içi yerine önceki kapanış verildi.** 10:30 TSİ çalışmasında tabloya "14.473 ▼ %0,19 · önceki kapanış" yazıldı ve Türkiye bölümünde "doğrulanabilir bir gün içi seviye henüz yok" denildi. Oysa borsa 30 dakikadır açıktı ve gün içi değer erişilebilirdi: aynı gün 10:34'te Google Finance 14.651,98 (▲ %1,23) gösteriyordu — yani gerçek hareket bültendekinin tam tersi yöndeydi. Kullanıcı yakaladı; rutini bu saate özellikle "piyasa otursun, güncel veriyi alayım" diye ayarlamış. → Kural 9 ve 10; SKILL.md'ye Google Finance kaynak tablosu ve Adım 2 madde 7 eklendi (düzenlemeyi kullanıcı yaptı). Aynı gün ajanın kendi eklediği "sabah 10:30'da BIST gün içi verisi indekslenmemiş olur, önceki kapanışı ver" kuralı **kaldırıldı** — yanlıştı, veri Google Finance'te mevcuttu.

**Arama özeti dünkü BIST açılışını bugüne ait gösterdi.** Bir arama "BIST 100, 26 Ağustos'ta 14.457,80'den açtı" dedi; bu rakam aslında 25 Ağustos açılışıydı (dünkü bültende aynen yazılıydı). Başka bir sonuç da 14.103 → 14.458,98 gibi çok daha eski bir güne ait seans verisi döndürdü. Önceki kapanışla (14.501) çapraz kontrol edilince yakalandı, kullanılmadı. → Kural 8.

**Gümüş için üç farklı fiyat.** Aynı gün için $70,17 (JM Bullion, bayat sayfa değeri), $69,09 ▲%0,7 (Reuters, 00:11 GMT) ve $68,40 ▼%0,3 (gün içi) çıktı. Reuters'ın ima ettiği önceki kapanış (~$68,6) ile gün içi verinin ima ettiği önceki kapanış aynı çıkınca $68,4 doğrulandı; $70,17 elendi. → Yeni kural açılmadı; SKILL Adım 2'deki çapraz kontrol mantığı yeterliydi.

**Üç yeni bloklu domain:** riotimesonline.com, diken.com.tr, borsaistanbul.com — üçü de `EGRESS_BLOCKED` döndürdü. → Operasyon notlarına eklendi.

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
