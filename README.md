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

## Notlar

Bu repo **public**. Buraya e-posta adresi, API anahtarı veya kişisel veri koyma — alıcı adresi gibi yapılandırma bilgileri rutin promptunda tutulur.
