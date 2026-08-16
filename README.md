<div align="center">

<h2>TgProBot</h2>

<b>Hepsi Bir Arada Telegram Grup Botu</b><br>
Müzik yayını, moderasyon, koruma, ekonomi, oyunlar, federasyonlar ve çok daha fazlası — 13 dil desteğiyle.

<a href="LICENSE">
    <img src="https://img.shields.io/badge/Lisans-MIT-blue?style=for-the-badge" alt="Lisans"/>
</a>
<a href="https://www.python.org/">
    <img src="https://img.shields.io/badge/Geliştirme%20Dili-Python-blue?style=for-the-badge&logo=python" alt="Python"/>
</a>
<a href="https://docs.pyrogram.org/">
    <img src="https://img.shields.io/badge/Pyrogram%20ile-Geliştirildi-blue?style=for-the-badge" alt="Pyrogram"/>
</a>
<br>

<img src="anonx.jpg" width="720" height="auto">

TgProBot, tam donanımlı bir Telegram grup yönetim botudur. Grup görüntülü sohbetlerinde müzik yayını yapar, grubunuzu yönetir ve korur, sanal borsa içeren bir coin ekonomisi sunar, mini oyunlar düzenler ve birden fazla grubu federasyon sistemiyle birbirine bağlar. Tüm özellikler sohbet bazında ayrı ayrı yapılandırılabilir ve 13 dil desteğine sahiptir. <br><br>

<a href="https://t.me/pamukmusicbot?startgroup=true">
    <img src="https://img.shields.io/badge/➕_Beni_Grubuma_Ekle-4CAF50?style=for-the-badge&logoColor=white" alt="Gruba ekle"/>
</a>
<a href="https://t.me/pamukmusicbot">
    <img src="https://img.shields.io/badge/💬_Yardım-17A2B8?style=for-the-badge&logoColor=white" alt="Yardım"/>
</a>
<a href="<your-repo-url>">
    <img src="https://img.shields.io/badge/🔗_Kaynak_Kod-2C3E50?style=for-the-badge&logo=github&logoColor=white" alt="Kaynak kod"/>
</a>
</div>

<hr>

<h2>🔥 Özellikler</h2>

* 🎵 **Müzik** — YouTube, Spotify, Apple Music, SoundCloud ve daha birçok platformdan grup görüntülü sohbetlerine ses veya video yayını yapar; sıra, döngü ve ileri-geri sarma özelliklerini destekler.
* 🛡 **Moderasyon** — Susturma, yasaklama, gruptan atma, süreli yasaklama, yapılandırılabilir limitlere sahip uyarı sistemi, mesaj temizleme ve günlük yasaklama limitleri.
* 🔒 **Koruma** — Yasaklı kelime filtreleri, bağlantı ve reklam koruması, yeni üyeler için CAPTCHA doğrulaması, flood koruması ve mesaj düzenleme koruması.
* 👋 **Karşılama ve gece modu** — Özelleştirilebilir katılma/ayrılma mesajları ve sohbet bazında zamanlanabilen gece modu.
* 💰 **Ekonomi** — XP ve seviye sistemi, seri bonusları içeren günlük ödüller, coin marketi, kişisel kasa ve gerçek sohbet etkinliğine göre hareket eden sanal borsa.
* 🎮 **Oyunlar** — Zar, matematik yarışları, kelime zinciri, doğruluk mu cesaret mi, bomba paslama, emoji bilmeceleri ve daha fazlası.
* 💬 **Sosyal** — Uyumluluk testleri, burçlar, günlük fallar ve diğer eğlenceli sohbet komutları.
* 📢 **Etiketleme** — Hız kontrolü ve kara liste desteği bulunan, flood korumalı toplu etiketleme sistemi.
* ⏰ **Zamanlayıcı** — Sohbet bazında tekrarlanan veya tek seferlik mesajlar zamanlama.
* 🗒 **Notlar** — Telegram’ın kayıtlı filtre sistemine benzer şekilde, `#kısayol` tetikleyicileriyle not kaydetme ve çağırma.
* 🌐 **Federasyonlar** — Birden fazla grubu ortak yasaklama listesi ve moderasyon günlüğü altında birbirine bağlama.
* 📊 **İstatistikler** — Sohbet ve kullanıcı bazında etkinlik sıralamaları.
* ⚙️ **Panel** — Her sohbet için satır içi ayar paneli ve kategorilere ayrılmış bot içi `/help` menüsü.
* 🌍 **13 dil** — `ar de en es fr hi ja my pa pt ru tr zh`; yukarıdaki tüm özellikler tamamen çevrilmiştir.

<hr>

<h2>☁️ Manuel Kurulum</h2>

<h3>✔️ Gereksinimler</h3>

* Sisteminizde <a href="https://www.python.org">Python 3.10+</a> kurulu olmalıdır.
* Sisteminizde <a href="https://deno.com/">Deno</a> ve <a href="https://ffmpeg.org/">FFmpeg</a> kurulu olmalıdır.
* Bir MongoDB veritabanı gereklidir. Örneğin <a href="https://cloud.mongodb.com">MongoDB Atlas</a> kullanılabilir.
* <a href="../sample.env">sample.env</a> dosyasında belirtilen gerekli değişkenler hazırlanmalıdır.

<details>
    <summary>
        <h3>Yerel Bilgisayar / VPS Kurulumu</h3>
    </summary>

<h4>🐧 Linux/macOS</h4>

```bash
git clone <your-repo-url> && cd TgProBot

# uv paket yöneticisini yükleyin
curl -Ls https://astral.sh/uv/install.sh | sh
export PATH="$HOME/.local/bin:$PATH"

# Bağımlılıkları yükleyin
uv sync --frozen

# Ortam değişkenleri dosyasını yeniden adlandırın ve yapılandırın
mv sample.env .env
# .env dosyasını bilgilerinizle düzenleyin

# Botu başlatın
bash start
```

<h4>🪟 Windows (PowerShell)</h4>

Projeyle birlikte gelen kurulum betiğini çalıştırın. Bu betik `ffmpeg`, `deno`, `uv` ve gerekli Python bağımlılıklarını yükler. Ardından gerekli bilgileri sorarak `.env` dosyasını otomatik olarak oluşturur:

```powershell
git clone <your-repo-url> && cd TgProBot

powershell -ExecutionPolicy Bypass -File .\setup.ps1

# Botu başlatın
.\start.ps1
```

> ⭐ Kurulumu elle yapmak isterseniz `winget install ffmpeg` komutuyla FFmpeg’i yükleyin, ardından [Deno](https://deno.com/) ve [uv](https://docs.astral.sh/uv/getting-started/installation/) kurulumlarını tamamlayın. `sample.env` dosyasını `.env` adıyla kopyalayın ve `uv run python -m tgprobot` komutunu çalıştırın. Windows/uv sanal ortamlarında `python3` çalıştırılabilir dosyası bulunmadığı için `python3` kullanmayın. Git Bash veya WSL kullanıyorsanız botu `bash start` komutuyla da başlatabilirsiniz.

</details>

<details>
    <summary>
        <h3>Heroku’ya Dağıtım</h3>
    </summary>

Bu depo `app.json` ve `heroku.yml` dosyalarını içerir. Bu nedenle kendi fork’unuz üzerinden Heroku’nun “Deploy from GitHub” özelliği kullanılarak doğrudan dağıtılabilir.

</details>

<hr>

<h2>⚙️ Yapılandırma</h2>

<code>.env</code> dosyasını düzenleyin veya değişkenleri kullandığınız barındırma platformunun ortam değişkenleri bölümüne ekleyin:

<details>
    <summary>Örnek .env dosyası</summary>

```env
# my.telegram.org/apps adresinden alınır
API_ID=123456
API_HASH=abcdef1234567890

# Telegram üzerindeki @BotFather hesabından alınır
BOT_TOKEN=123456:ABC-DEF
LOGGER_ID=-1001234567890

# cloud.mongodb.com adresinden alınan MongoDB bağlantısı
MONGO_URL=mongodb+srv://

# Bot sahibinin Telegram kullanıcı kimliği
OWNER_ID=123456789

# Telegram üzerindeki @StringFatherBot aracılığıyla alınan Pyrogram oturumu
SESSION=BQgfh...AA

# Yeni sohbetler için varsayılan bot dili
# Kullanılabilir dil kodları tgprobot/locales dizininde bulunur
LANG_CODE=tr
```

> 📝 Kullanılabilir tüm yapılandırma seçeneklerini görmek için <a href="../config.py">config.py</a> dosyasını inceleyin.

</details>

<hr>

<h2>🧐 Kullanım</h2>

1. Botu Telegram grubunuza ekleyin.
2. Botu <b>yönetici</b> yapın. Tüm özelliklerin çalışabilmesi için kullanıcı davet etme, kullanıcıları kısıtlama, mesaj silme ve mesaj sabitleme izinlerini verin.
3. Kategorilere ayrılmış bot içi komut menüsünü açmak için grupta veya botun özel mesajlarında `/help` komutunu gönderin. Aşağıdaki komutların tamamı ve daha fazlası, gerekli yetki seviyeleriyle birlikte bu menüde listelenir.

<details>
    <summary>Kategorilere göre komut özeti</summary>
    <pre>
🎵 Müzik       /play /vplay /pause /resume /skip /stop /seek /queue /loop
🛡 Moderasyon  /mute /unmute /ban /unban /kick /tban /warn /warnlist /del
🔒 Koruma      /yasakkelime /linkkoruma /reklamkoruma /captcha /filter
👋 Karşılama   /hosgeldin /hoscakal /gecemodu
💰 Ekonomi     /seviye /gunluk /coin /coinaktar /market /borsa /kasam
🎮 Oyunlar     /oyun /zar /matematik /kelimezinciri /kura /bomba
💬 Sosyal      /fal /burc /askolcer /tokat
📢 Etiketleme  /herkes /adminlercagir /karaliste
⏰ Zamanlayıcı /zamanla /zamanlar /zamaniptal
🗒 Notlar      /not_ekle /not /notlar
🌐 Federasyon  /newfed /joinfed /fedstat /fedchats
📊 İstatistik  /istatistik /liderlik /combotsira
⚙️ Panel       /panel /lang
    </pre>
</details>

`/setcommands` bölümünde kullanılmak üzere hazırlanmış BotFather uyumlu komut listesi, projenin ana dizinindeki <a href="../botfather_commands.txt">botfather_commands.txt</a> dosyasında bulunmaktadır.

<hr>

<h2>❤️ Katkıda Bulunma</h2>

Projeye yapılacak katkılar memnuniyetle karşılanır!

1. Depoyu fork’layın.
2. Yeni dalınızı oluşturun: <code>git checkout -b feature/new</code>
3. Değişikliklerinizi commit’leyin: <code>git commit -m 'Yeni özellik'</code>
4. Değişiklikleri deponuza gönderin: <code>git push origin feature/new</code>
5. Bir Pull Request açın.

<hr>

<h2>🗒️ Lisans</h2>

Bu proje <b>MIT Lisansı</b> ile lisanslanmıştır. Ayrıntılı bilgi için <a href="../LICENSE">LICENSE</a> dosyasını inceleyin.

<hr>

<h2>👀 Teşekkürler</h2>

* Proje başlangıçta açık kaynaklı <b>AnonXMusic</b> Telegram müzik botundan fork’lanmış, daha sonra moderasyon, koruma, ekonomi, oyunlar, sosyal komutlar, etiketleme, zamanlama, notlar, federasyon ve istatistik sistemleriyle kapsamlı biçimde geliştirilmiştir.
* <a href="https://docs.pyrogram.org/">Pyrogram</a> ve <a href="https://github.com/pytgcalls/pytgcalls">Py-TgCalls</a> kullanılarak geliştirilmiştir.

<hr>

<div align="center">

⭐ Bu proje işinize yaradıysa GitHub deposuna yıldız vermeyi düşünebilirsiniz!

</div>
