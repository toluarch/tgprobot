# Telegram Music & Video Streaming Bot

Windows Server 2019/2022/2025 üzerinde çalışacak şekilde tasarlanmış, production seviyesinde bir Telegram müzik/video/canlı yayın/radyo botu. PyTgCalls + FFmpeg + yt-dlp üzerine kurulu; grup başına bağımsız kuyruk, çoklu assistant desteği, MongoDB tabanlı kalıcı ayarlar/yetkiler, detaylı loglama ve Windows'a özel kurulum/servis betikleriyle gelir.

## İçindekiler

1. [Mimari Özeti](#mimari-özeti)
2. [Gereksinimler](#gereksinimler)
3. [Adım Adım Kurulum](#adım-adım-kurulum)
4. [.env Yapılandırması](#env-yapılandırması)
5. [Botu Başlatma](#botu-başlatma)
6. [Komutlar](#komutlar)
7. [Oyun Sistemi](#oyun-sistemi)
8. [Windows Service Olarak Çalıştırma](#windows-service-olarak-çalıştırma)
9. [Sorun Giderme](#sorun-giderme)
10. [Test Çalıştırma](#test-çalıştırma)
11. [Proje Yapısı](#proje-yapısı)

---

## Mimari Özeti

Sistem iki ayrı Telegram istemcisiyle çalışır:

- **Bot Client** (`BOT_TOKEN`): Komutları, butonları ve kullanıcı mesajlarını yönetir.
- **Assistant Client** (`ASSISTANT_SESSION`): Normal bir Telegram kullanıcı hesabı olarak grubun sesli/görüntülü sohbetine katılır ve PyTgCalls üzerinden yayın yapar. Birden fazla assistant tanımlanabilir (`ASSISTANT_SESSION_1`, `ASSISTANT_SESSION_2`, ...); sistem her grup için en az yüklü assistant'ı otomatik seçer.

Medya çözümleme merkezi bir `MediaResolver` üzerinden yapılır: YouTube, Spotify, Apple Music, SoundCloud, doğrudan medya linkleri, HLS/DASH, internet radyoları, Telegram'a yüklenmiş dosyalar ve yt-dlp'nin desteklediği yüzlerce diğer site otomatik olarak denenir. Spotify/Apple Music bağlantıları resmi metadata API'leri üzerinden okunur (DRM bypass **yapılmaz**); gerçek ses kaynağı çalma anında YouTube araması ile bulunur.

## Gereksinimler

- Windows Server 2019 / 2022 / 2025 (veya Windows 10/11 - geliştirme için)
- Python 3.10 veya üzeri
- FFmpeg + FFprobe
- Bir Telegram hesabı (assistant için) ve bir bot token (BotFather)
- (Opsiyonel ama önerilir) MongoDB - kalıcı ayarlar/yetkiler için
- (Opsiyonel) Spotify Developer hesabı - Spotify linklerinin metadata'sını okumak için

## Adım Adım Kurulum

### 1. Python Kurulumu

https://www.python.org/downloads/windows/ adresinden Python 3.10+ indirin. Kurulum sırasında **"Add python.exe to PATH"** kutucuğunu mutlaka işaretleyin.

Doğrulama:

```powershell
python --version
```

### 2. Telegram API ID / API HASH Alma

1. https://my.telegram.org adresine giriş yapın.
2. "API Development Tools" bölümüne gidin.
3. Yeni bir uygulama oluşturun; size verilen `api_id` ve `api_hash` değerlerini not edin.

### 3. BotFather ile Bot Oluşturma

1. Telegram'da [@BotFather](https://t.me/BotFather) ile konuşun.
2. `/newbot` komutunu gönderin, adını ve kullanıcı adını belirleyin.
3. Size verilen **bot token**'ı not edin.
4. Botunuzun grup mesajlarını okuyabilmesi için BotFather'da `/setprivacy` ile **Disable** seçeneğini seçin (aksi halde `/play` gibi komutları göremez).

### 4. Assistant Hesabı için String Session Oluşturma

Assistant, sesli sohbete fiziksel olarak katılacak normal bir Telegram hesabıdır (BotFather hesabı DEĞİLDİR). Ayrı bir numara kullanmanız önerilir.

```powershell
.\venv\Scripts\python.exe scripts\generate_session.py
```

Sırasıyla API_ID, API_HASH, telefon numarası, gelen doğrulama kodu ve (varsa) 2FA şifrenizi girin. Çıktıda verilen String Session değerini `.env` dosyanızdaki `ASSISTANT_SESSION` alanına yapıştırın. **Bu değeri kimseyle paylaşmayın** - bir şifre gibidir.

### 5. FFmpeg Kurulumu

1. https://www.gyan.dev/ffmpeg/builds/ adresinden "release full" build'i indirin.
2. Zip dosyasını örneğin `C:\ffmpeg` klasörüne çıkarın.
3. `.env` dosyanızda şu değerleri ayarlayın:
   ```
   FFMPEG_PATH=C:\ffmpeg\bin\ffmpeg.exe
   FFPROBE_PATH=C:\ffmpeg\bin\ffprobe.exe
   ```
   (FFmpeg'i sistem PATH'ine eklerseniz bu iki değeri boş bırakabilirsiniz.)

### 6. MongoDB Bağlantısı (Opsiyonel ama Önerilir)

- Yerel kurulum: https://www.mongodb.com/try/download/community
- Veya ücretsiz bulut: https://www.mongodb.com/cloud/atlas

Bağlantı adresini `.env` dosyasındaki `MONGO_URI` alanına yazın. Boş bırakırsanız bot yine çalışır, ancak sudo listesi, grup ayarları ve blacklist gibi kalıcı veriler saklanmaz.

### 7. .env Dosyasını Oluşturma

```powershell
Copy-Item .env.example .env
notepad .env
```

Tüm alanları doldurun (bkz. [.env Yapılandırması](#env-yapılandırması)).

### 8. Bağımlılıkların Kurulumu

Tüm adımları otomatikleştiren kurulum betiğini çalıştırın:

```powershell
.\install_windows.ps1
```

Bu betik: Python/pip sürümünü kontrol eder, `venv` sanal ortamı oluşturur, `requirements.txt`'i kurar, FFmpeg/FFprobe'u arar, `.env` yoksa `.env.example`'dan oluşturur, gerekli klasörleri (`data`, `cache`, `downloads`, `temp`, `logs`) oluşturur ve yazma izinlerini test eder.

### 9. Botu Başlatma

```powershell
.\start.ps1
```

veya çift tıklanabilir `start.bat` dosyasını kullanın. Başlangıçta bir öz-test tablosu (Python/FFmpeg/FFprobe/yt-dlp/Database) görürsünüz.

### 10. Gruba Bot ve Assistant Ekleme

1. Bot hesabınızı Telegram grubunuza **admin** olarak ekleyin (mesaj silme/sabitleme gerekmez, sadece mesaj gönderebilmesi yeterlidir).
2. Assistant hesabınızı da aynı gruba **normal üye** olarak ekleyin (admin olması gerekmez, ancak grubun "sadece adminler mesaj yazabilir" kısıtlaması yoksa böyle kalabilir).

### 11. Admin İzinleri

`/authmode admin` komutuyla oynatma kontrolünü (pause/skip/stop/...) sadece grup adminleriyle sınırlayabilirsiniz; varsayılan `everyone` modudur.

### 12. Sesli/Görüntülü Sohbeti Açma

Telegram grubunda sesli sohbeti (veya video sohbeti) manuel olarak başlatın (grup adı üzerinden "Voice Chat" / "Video Chat" seçeneği). Bot bu sohbete `/play` komutuyla otomatik katılır; grup içinde henüz açık bir sesli sohbet yoksa bot size açık bir hata mesajı gösterir.

### 13. İlk Test

Grupta:

```
/play Ceza Suspus
```

yazın. Bot arama yapıp sesli sohbete katılacak ve çalmaya başlayacaktır.

## .env Yapılandırması

Tüm değişkenlerin tam listesi ve açıklamaları için `.env.example` dosyasına bakın. En kritik olanlar:

| Değişken | Açıklama |
|---|---|
| `API_ID`, `API_HASH` | my.telegram.org'dan alınan uygulama kimlik bilgileri |
| `BOT_TOKEN` | BotFather bot token'ı |
| `ASSISTANT_SESSION` | scripts/generate_session.py ile üretilen String Session |
| `OWNER_ID` | Bot sahibinin Telegram kullanıcı ID'si |
| `MONGO_URI` | MongoDB bağlantı adresi (boş = kalıcı veri yok, bot yine çalışır) |
| `MAX_VIDEO_HEIGHT` | Varsayılan 720; sunucu performansı için düşürülebilir |
| `FFMPEG_PATH` / `FFPROBE_PATH` | PATH'te değillerse tam dosya yolu |
| `MAX_ACTIVE_GAMES_PER_CHAT` | Varsayılan 1; bir grupta aynı anda kaç oyunun aktif olabileceği (bkz. [Oyun Sistemi](#oyun-sistemi)) |

## Botu Başlatma

| Betik | Amaç |
|---|---|
| `start.ps1` / `start.bat` | Botu başlatır |
| `restart.ps1` | Çalışan süreci durdurup yeniden başlatır |
| `update.ps1` | Kodu (varsa git) ve bağımlılıkları (özellikle yt-dlp'yi) günceller |

## Komutlar

### Oynatma

| Komut | Açıklama |
|---|---|
| `/play <şarkı adı\|link>` (`/p`) | Müzik çalar; medyaya reply ile Telegram dosyası da çalınabilir |
| `/vplay <link>` (`/vp`) | Video çalar |
| `/stream <m3u8/mpd>` | Canlı yayın oynatır |
| `/radio <link>` | İnternet radyosu çalar |

### Kontrol

`/pause` `/resume` `/skip` (`/s`) `/stop` (`/end`) `/seek 01:30` `/replay` `/queue` (`/q`) `/clearqueue` `/shuffle` `/loop off\|track\|queue` `/volume 0-200` `/nowplaying` (`/np`)

### Ayarlar (Grup Admini)

`/authmode everyone\|admin` `/auth @kullanıcı` `/unauth @kullanıcı` `/authlist` `/lang tr\|en` `/blacklist @kullanıcı` `/unblacklist @kullanıcı` `/log enable\|disable` `/spam enable\|disable` `/reload` (önbellek temizle)

### Owner / Sudo

`/sudoadd` `/sudodel` `/sudolist` `/gban` `/ungban` `/broadcast <mesaj>` `/stats` `/ping` `/activevc` `/health` `/cleanup` `/debug` `/resolve <url>`

## Oyun Sistemi

Müzik/video sisteminden bağımsız, ayrı bir modül olarak çalışan oyun/eğlence
sistemi (`app/games/`) - bir grupta müzik çalarken de oyun oynanabilir, ikisi
birbirini etkilemez. `/games` menüsünden erişilen 14 oyun (faz 1):

Hızlı Matematik, Bilgi Oyunu, Bayrak Oyunu, Başkent Tahmin, Plaka Oyunu,
Doğru/Yanlış, Sayı Oyunu, Kelime Sarmalı, Boşluk Doldurma, Emoji Bilmece,
Şifre Kırma, Sıcak Soğuk, XOX ve Düello - tam XP/seviye sistemi, liderlik
tablosu (global/grup, günlük/haftalık/tüm-zamanlar), oyuncu profili,
başarımlar, cooldown/anti-spam ve callback-sahibi doğrulamalı anti-cheat ile
birlikte.

| Komut | Açıklama |
|---|---|
| `/games` (`/oyun`, `/oyunlar`, `/game`) | Oyun menüsünü açar |
| `/gameprofile` (`/profile`) | Seviye/XP/kazanma-kaybetme profilini gösterir |
| `/leaderboard` (`/top`, `/gametop [oyun_id]`) | Liderlik tablosu |
| `/gamehelp` | Her oyunun kurallarını gösteren menü |
| `/cancelgame` (`/oyuniptal`) | Grubun aktif oyununu iptal eder (başlatan kişi ya da admin) |
| `/mygamestats` | Kendi detaylı oyun istatistiklerin |
| `/gamesettings` (Grup Admini) | Oyun/XP/liderlik açık-kapalı, `/gameenable`, `/gamedisable <oyun_id>` |
| `/gameban`, `/gameunban`, `/gamereset`, `/gamedebug`, `/gamestats` (Owner) | Oyun sistemi yönetimi - `/gameban` sadece oyunlara erişimi keser, müzik/video komutlarını etkilemez |

Faz 2'de eklenecekler (henüz yok): Kelime Anlatma, Casus Kim, Sudoku, Kelime
Zinciri, Hafıza Şimşeği, Fark Bulmaca, Bul Bakalım, Pi, Doğruluk/Cesaret,
Buton Oyunu, Eser Yazar ve Günlük Görevler.

## Windows Service Olarak Çalıştırma

Botun sunucu yeniden başladığında otomatik açılması ve çökme durumunda otomatik yeniden başlaması için [NSSM](https://nssm.cc/download) kullanılması önerilir:

```powershell
# nssm.exe'yi indirip PATH'e ekledikten sonra:
.\scripts\install_service.ps1

Start-Service TelegramMusicBot
```

NSSM kullanmak istemezseniz betik size Windows Task Scheduler ile aynı sonucu elde etmenin adımlarını gösterir (Trigger: "At startup", Action: `venv\Scripts\python.exe main.py`).

## Sorun Giderme

| Hata | Çözüm |
|---|---|
| `'ffmpeg' is not recognized` | FFmpeg PATH'te değil. `.env` dosyasında `FFMPEG_PATH`/`FFPROBE_PATH` tam yolunu belirtin veya FFmpeg'i sistem PATH'ine ekleyin. |
| `Python was not found` | Python kurulurken "Add to PATH" işaretlenmemiş. Python'u PATH'e ekleyecek şekilde yeniden kurun veya `python.exe`'yi manuel olarak PATH'e ekleyin. Windows'un kendi "python" kısayolu Microsoft Store'u açıyorsa bu gerçek bir Python kurulumu değildir - python.org'dan indirin. |
| `ModuleNotFoundError` | `venv` aktif değil veya bağımlılıklar kurulmamış. `.\install_windows.ps1` betiğini tekrar çalıştırın. |
| `Microsoft Visual C++ 14.0 or greater is required` (TgCrypto kurulurken) | TgCrypto, Pyrogram için isteğe bağlı bir hız hızlandırıcısıdır ve derlenmesi bir C++ derleyicisi gerektirir. `requirements.txt` içinde varsayılan olarak **kurulmaz** - bot onsuz da tam işlevsel çalışır (sadece şifreleme biraz daha yavaştır). İsterseniz [Visual C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) kurup ardından `pip install TgCrypto` çalıştırabilirsiniz. |
| `WinError 2` | Belirtilen bir dosya yolu (genellikle FFmpeg) bulunamıyor. `FFMPEG_PATH`/`FFPROBE_PATH` değerlerinin doğru olduğundan emin olun. |
| `WinError 10054` | Uzak sunucu bağlantıyı kapattı (ağ/ISP kesintisi). Bot otomatik yeniden bağlanmayı dener; sık tekrarlanıyorsa sunucunun internet bağlantısını kontrol edin. |
| Telegram `FloodWait` | Telegram API'ye çok hızlı istek gönderildi. Bot bunu otomatik olarak bekleyip tekrar dener; sürekli oluyorsa `/broadcast` gibi toplu işlemleri yavaşlatın. |
| `Assistant cannot join voice chat` / `No active group call` | Grupta henüz bir sesli/görüntülü sohbet açılmamış. Önce grup arayüzünden sesli sohbeti başlatın, sonra `/play` yazın. |
| `yt-dlp extractor error` | Site yapısı değişmiş olabilir. `.\update.ps1` ile yt-dlp'yi güncelleyin (`pip install -U yt-dlp`). |
| `cookies expired` | `data/cookies.txt` dosyasını güncelleyin veya `COOKIE_REFRESH_ENABLED=true` + `YTDLP_COOKIES_FROM_BROWSER` ayarlayın. |
| `MongoDB connection refused` | MongoDB servisi çalışmıyor veya `MONGO_URI` hatalı. Bot yine de bellek-içi kuyruklarla çalışmaya devam eder; kalıcı ayarlar/yetkiler o oturum için devre dışı kalır. |

Detaylı hata kayıtları için `logs/errors.log`, oynatma geçmişi için `logs/playback.log`, yapılandırılmışsa Telegram logger grubunuza bakın.

## Test Çalıştırma

```powershell
.\venv\Scripts\python.exe -m pip install -r requirements-dev.txt
.\venv\Scripts\python.exe -m pytest
```

> **Not:** Bu proje bu ortamda (Python kurulu olmayan bir sandbox) yazılmış ve derinlemesine statik olarak gözden geçirilmiştir (import/attribute tutarlılığı, PyTgCalls'ın gerçek GitHub kaynak koduna karşı API doğrulaması, race-condition analizi). Ancak gerçek bir Python yorumlayıcısıyla `pytest` çalıştırılarak doğrulanmamıştır - kurulumdan sonra yukarıdaki komutla test paketini çalıştırmanız ve karşılaştığınız hataları bildirmeniz önerilir.

## Proje Yapısı

```
app/
    core/        - config, logging, exceptions, security (secret redaction), context, self-test
    clients/      - bot.py (BotFather client), assistant.py (AssistantManager)
    calls/        - manager.py (PyTgCalls orchestration), ffmpeg.py (process manager)
    media/        - resolver.py, models.py, providers/ (youtube, spotify, apple_music, soundcloud,
                     direct, hls, radio, telegram, generic, ytdlp_engine)
    queue/        - manager.py (QueueRegistry/ChatQueue), models.py (QueueItem, PlayerSession)
    handlers/     - play, controls, callbacks, admin, owner, settings, general, games_bridge.py
                     (imports app/games/handlers/* so Pyrogram's plugin scanner finds them)
    services/     - database, cookies, cache, retry, statistics, permissions, rate_limiter,
                     spam_guard, event_bus, i18n, health, telegram_logger, now_playing, playback_log
    database/     - models.py, repositories.py, game_models.py, game_repositories.py
    locales/      - tr.json, en.json
    games/        - independent games/eğlence module (see "Oyun Sistemi" above)
                     core/     - BaseGame, GameSession/GameSessionManager, GameRegistry,
                                 scoring, cooldown, timers, matchmaking, permissions, text, dataset_loader
                     games/    - one file per game (quick_math.py, tic_tac_toe.py, duel.py, ...)
                     data/     - JSON datasets (trivia, countries, turkey_plates, words, ...)
                     handlers/ - commands.py, callbacks.py, messages.py
                     services/ - leaderboard.py, statistics.py, rewards.py
scripts/          - generate_session.py, install_service.ps1
tests/            - pytest test suite
main.py           - entry point
install_windows.ps1 / start.ps1 / restart.ps1 / update.ps1 / start.bat
```
