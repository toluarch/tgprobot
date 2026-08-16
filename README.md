<div align="center">

<h2>TgProBot</h2>

<b>All-in-One Telegram Group Bot</b><br>
Music streaming, moderation, protection, economy, games, federations, and more — in 13 languages.

<a href="../LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="License"/>
</a>
<a href="https://www.python.org/">
    <img src="https://img.shields.io/badge/Written%20in-Python-blue?style=for-the-badge&logo=python" alt="Python"/>
</a>
<a href="https://docs.pyrogram.org/">
    <img src="https://img.shields.io/badge/Built%20with-Pyrogram-blue?style=for-the-badge" alt="Pyrogram"/>
</a>
<br>

<img src="anonx.jpg" width="720" height="auto">

TgProBot is a full-featured Telegram group management bot: it streams music into video chats, moderates and protects your group, runs a coin economy with a virtual stock market, hosts mini-games, and links groups together into federations — all configurable per-chat, in 13 languages.
<br><br>

<a href="https://t.me/pamukmusicbot?startgroup=true">
    <img src="https://img.shields.io/badge/➕_Beni_Grubuma_Ekle-4CAF50?style=for-the-badge&logoColor=white" alt="Add to group"/>
</a>
<a href="https://t.me/pamukmusicbot">
    <img src="https://img.shields.io/badge/💬_Yardım-17A2B8?style=for-the-badge&logoColor=white" alt="Help"/>
</a>
<a href="<your-repo-url>">
    <img src="https://img.shields.io/badge/🔗_Kaynak-2C3E50?style=for-the-badge&logo=github&logoColor=white" alt="Source"/>
</a>
</div>

<hr>

<h2>🔥 Features</h2>

- 🎵 **Music** — stream audio/video into group video chats (YouTube, Spotify, Apple Music, SoundCloud, and more), with queueing, loop, and seek
- 🛡 **Moderation** — mute/ban/kick/tban, warning system with configurable limits, message purge, daily ban limits
- 🔒 **Protection** — forbidden-word filters, link/ad protection, captcha verification for new members, anti-flood, edit protection
- 👋 **Welcome & night mode** — custom join/leave messages, scheduled night mode per chat
- 💰 **Economy** — XP/leveling, daily rewards with streak bonuses, a coin market, a personal vault, and a virtual stock market driven by real chat activity
- 🎮 **Games** — dice, math races, word chains, truth-or-dare, bomb-pass, emoji riddles, and more
- 💬 **Social** — compatibility checks, horoscopes, daily fortunes, and other lighthearted chat commands
- 📢 **Tagging** — flood-safe mass mentions with speed control and a blacklist
- ⏰ **Scheduler** — schedule recurring or one-off messages per chat
- 🗒 **Notes** — save and recall notes with `#shortcut` triggers, mirroring Telegram's saved-filter pattern
- 🌐 **Federations** — link multiple groups under a shared ban list and moderation log
- 📊 **Stats** — per-chat and per-user activity leaderboards
- ⚙️ **Panel** — inline settings panel per chat, with a categorized in-bot `/help` menu
- 🌍 **13 languages** — `ar de en es fr hi ja my pa pt ru tr zh`, fully translated including all of the above

<hr>

<h2>☁️ Manual Deployment</h2>

<h3>✔️ Prerequisites</h3>

- <a href="https://www.python.org">Python 3.10+</a> installed
- <a href="https://deno.com/">deno</a> & <a href="https://ffmpeg.org/">ffmpeg</a> installed on your system
- A MongoDB database (e.g. <a href="https://cloud.mongodb.com">MongoDB Atlas</a>)
- Required variables mentioned in <a href="../sample.env">sample.env</a>

<details>
    <summary>
        <h3>Local / VPS Setup</h3>
    </summary>

<h4>🐧 Linux/macOS</h4>

```bash
git clone <your-repo-url> && cd TgProBot

# Install uv
curl -Ls https://astral.sh/uv/install.sh | sh
export PATH="$HOME/.local/bin:$PATH"

# Install dependencies
uv sync --frozen

# Rename and configure environment variables
mv sample.env .env
# Edit .env with your credentials

# Start the bot
bash start
```

<h4>🪟 Windows (PowerShell)</h4>

Run the bundled setup script — it installs `ffmpeg`, `deno`, `uv`, the Python dependencies, and prompts for your credentials to generate `.env`:

```powershell
git clone <your-repo-url> && cd TgProBot

powershell -ExecutionPolicy Bypass -File .\setup.ps1

# Start the bot
.\start.ps1
```

> ⭐ If you'd rather set things up by hand: `winget install ffmpeg`, install [deno](https://deno.com/) and [uv](https://docs.astral.sh/uv/getting-started/installation/), copy `sample.env` to `.env`, then run `uv run python -m tgprobot` (not `python3` — Windows/uv virtual environments don't ship a `python3` executable). Git Bash or WSL also work with `bash start`.

</details>

<details>
    <summary>
        <h3>Deploy to Heroku</h3>
    </summary>

This repo includes an `app.json` and `heroku.yml`, so it can be deployed directly from your own fork via Heroku's "Deploy from GitHub" flow.
</details>

<hr>

<h2>⚙️ Configuration</h2>

Edit <code>.env</code> (or set variables in your hosting environment):
<details>
    <summary>Here's an example of the .env file</summary>

```env
# from my.telegram.org/apps
API_ID=123456
API_HASH=abcdef1234567890

# from @BotFather on telegram
BOT_TOKEN=123456:ABC-DEF
LOGGER_ID=-1001234567890

# mongo url from cloud.mongodb.com
MONGO_URL=mongodb+srv://

OWNER_ID=123456789

# pyrogram session from @StringFatherBot on telegram
SESSION=BQgfh...AA

# default bot language for new chats (see tgprobot/locales for available codes)
LANG_CODE=tr
```

> 📝 Check <a href="../config.py">config.py</a> for all available options.
</details>

<hr>

<h2>🧐 Usage</h2>

1. Add the bot to your Telegram group.
2. Promote it to <b>admin</b> — grant invite, restrict, delete-message, and pin permissions for full functionality.
3. Send `/help` in the chat (or in DM) for a categorized, in-bot command menu — every command below (and more) is listed there with its required permission level.

<details>
    <summary>Command overview by category</summary>
    <pre>
🎵 Music      /play /vplay /pause /resume /skip /stop /seek /queue /loop
🛡 Moderation /mute /unmute /ban /unban /kick /tban /warn /warnlist /del
🔒 Protection /yasakkelime /linkkoruma /reklamkoruma /captcha /filter
👋 Welcome    /hosgeldin /hoscakal /gecemodu
💰 Economy    /seviye /gunluk /coin /coinaktar /market /borsa /kasam
🎮 Games      /oyun /zar /matematik /kelimezinciri /kura /bomba
💬 Social     /fal /burc /askolcer /tokat
📢 Tagging    /herkes /adminlercagir /karaliste
⏰ Scheduler  /zamanla /zamanlar /zamaniptal
🗒 Notes      /not_ekle /not /notlar
🌐 Federation /newfed /joinfed /fedstat /fedchats
📊 Stats      /istatistik /liderlik /combotsira
⚙️ Panel      /panel /lang
    </pre>
</details>

A curated, BotFather-ready command list (for `/setcommands`) is included at <a href="../botfather_commands.txt">botfather_commands.txt</a> in the repo root.

<hr>

<h2>❤️ Contributing</h2>

Contributions are welcome!

1. Fork the repository.
2. Create your branch: <code>git checkout -b feature/new</code>.
3. Commit changes: <code>git commit -m 'New feature'</code>.
4. Push: <code>git push origin feature/new</code>
5. Open a Pull Request.

<hr>

<h2>🗒️ License</h2>

This project is licensed under the <b>MIT License</b> — see <a href="../LICENSE">LICENSE</a> for details.

<hr>

<h2>👀 Acknowledgements</h2>

- Originally forked from the open-source <b>AnonXMusic</b> Telegram music bot, then substantially extended with moderation, protection, economy, games, social, tagging, scheduling, notes, federation, and stats systems.
- Built with <a href="https://docs.pyrogram.org/">Pyrogram</a> and <a href="https://github.com/pytgcalls/pytgcalls">Py-TgCalls</a>.

<hr>

<div align="center">

⭐ If this project is useful to you, consider starring the repo!

</div>
