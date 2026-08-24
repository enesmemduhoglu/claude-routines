# claude-routines

Claude Code bulut rutinlerinin (routines) okuduğu skill tanımları.

Bulut ajanları yerel `~/.claude/skills/` dizinine erişemez. Bu repo, rutinlerin skill'i tek bir kaynaktan okuyabilmesi için var: rutin promptu kısa kalır, davranış değişikliği için sadece bu repodaki dosya güncellenir.

## Skills

| Skill | Ne yapar | Ne zaman |
|---|---|---|
| [`gunluk-ekonomi`](skills/gunluk-ekonomi/SKILL.md) | Günlük ekonomi/piyasa bültenini araştırır ve e-posta olarak gönderir | Her gün 09:00 TSİ |

## Nasıl çalışıyor

Rutin bu repoyu klonlar, promptunda ilgili `SKILL.md` dosyasını okumasını söyler ve talimatları uygular.

Skill'i değiştirmek için bu repoya push yapmak yeterli — **rutini güncellemeye gerek yok.**

## İki dosya, iki rol

| Dosya | Kim yazar | Ne içerir |
|---|---|---|
| `skills/*/SKILL.md` | **Sadece insan** | Kullanıcının sözleşmesi: format, uzunluk sınırları, ton, kurallar |
| [`notes/hatalar.md`](notes/hatalar.md) | **Ajan** (append-only) | Yapılmış hatalar ve bunlardan çıkan dersler |

Ajan her çalışmada ikisini de okur, ama yalnızca `notes/hatalar.md`'ye yazar. Bu ayrım bilinçli: skill'i ajana açarsan zamanla şişer ve kullanıcının koyduğu kurallar aşınır; hata defteri ise en kötü ihtimalle uzar, bülteni bozmaz.

Defterdeki bir ders kalıcılaşmışsa insan onu `SKILL.md`'ye taşıyabilir.

## Notlar

Bu repo **public**. Buraya e-posta adresi, API anahtarı veya kişisel veri koyma — alıcı adresi gibi yapılandırma bilgileri rutin promptunda tutulur.
