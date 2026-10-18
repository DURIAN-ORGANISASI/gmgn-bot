<div align="center">

<img src="docs/logo.svg" alt="GM/GN Bot" width="84" height="84">

# GM/GN Bot

<p>
  <img src="https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat&logo=python&logoColor=white" alt="Python 3.10+">
  <img src="https://img.shields.io/badge/Discord-Automation-5865F2?style=flat&logo=discord&logoColor=white" alt="Discord automation">
  <img src="https://img.shields.io/badge/Deploy-Railway-0B0D0E?style=flat&logo=railway&logoColor=white" alt="Railway">
  <img src="https://img.shields.io/badge/Container-Docker-2496ED?style=flat&logo=docker&logoColor=white" alt="Docker">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-2DAD87?style=flat" alt="MIT License"></a>
</p>

<p><a href="README.md">🇮🇩 Bahasa Indonesia</a> · <a href="README.en.md">🇬🇧 English</a></p>

<p><strong>Scheduled Good Morning &amp; Good Night messages for multiple Discord channels.</strong><br>
Manage targets and send times through chat commands, with persistent configuration on a Railway Volume.</p>

<p>
  <a href="#try-it-now">Quick start</a> ·
  <a href="#features">Features</a> ·
  <a href="#architecture-at-a-glance">Architecture</a> ·
  <a href="#usage">Usage guide</a> ·
  <a href="#input-format">Commands</a> ·
  <a href="#privacy">Privacy &amp; security</a> ·
  <a href="#development">Development</a>
</p>

</div>

---

![GM/GN Bot illustration](docs/hero.en.svg)

<p align="center"><sub>SVG illustrations for documentation — not live screenshots from Discord or Railway.</sub></p>

> [!WARNING]
> **Important — Discord Terms of Service risk.** This repository uses a user-account token through `discord.py-self` (self-bot). Automating a user account violates Discord's Terms of Service and may lead to account restrictions or bans. Do not use your main account or share the token. For an official-bot-based approach, see [GM/GN n8n Agent](https://github.com/DURIAN-ORGANISASI/gmgn-n8n-agent); this repository's code does not itself use the official bot API.

## Try it now

Three steps from zero to a running bot:

1. **Deploy** this repo to Railway with **Deploy from GitHub repo** ([railway.app/new](https://railway.app/new)).
2. Set three **Variables**: `DISCORD_USER_TOKEN`, `MONITOR_CHANNEL_ID`, `TIMEZONE`, then add a **Volume** mounted at `/data`.
3. Open the monitor channel and type:

```
!set 1234567890 0987654321
!time gm:07.00, gn:19.00
!list
```

That's it: the bot sends `gm` and `gn` to the target channels at the configured times. Full walkthrough in [Usage](#usage).

## Features

**Message delivery**

- **Weighted message variety**: short messages (`gm` / `gn`) are selected more often, with alternative phrasings to reduce repetition.
- **Multi-channel**: one bot for many target channels at once.
- **Variable delay between sends** to spread delivery and reduce request bursts; always respect each server's rules and platform limits.
- **Separate GM and GN schedules**, with a configurable timezone via `TIMEZONE` (e.g. `Asia/Jakarta`, `Asia/Makassar`).

**Control and operational safety**

- **Text-command control panel**: `!menu`, `!set`, `!time`, `!list`, `!stop`, `!test`. No code changes or redeploys needed to change targets or schedule.
- **Locked monitor channel**: commands are only answered in the channel set by `MONITOR_CHANNEL_ID`.
- **Persistent configuration** in `/data/config.json` (Railway Volume), kept across restarts and redeploys.

**Code quality**

- Code split by module (settings, storage, messages, scheduler, commands, client).
- Code style checks with **Ruff**; pytest is available for regression tests contributed to the project.
- Production-ready with a **Dockerfile** and **Procfile**.

## Architecture at a glance

![GM/GN Bot workflow](docs/architecture.en.svg)

Commands are accepted only in the monitor channel. Targets and schedules are stored in `/data/config.json`, and the worker sends GM/GN messages using the configured timezone. A Railway Volume is required to retain configuration when the container is recreated.

## Usage

> [!NOTE]
> The bot replies in Indonesian. Images below are English-labeled documentation mockups using fictional sample values; real bot replies will differ.

**1. Deploy and set environment variables.** Follow [Deploy to Railway](#deploy-to-railway). Once running, the bot posts its status in the monitor channel.

![Environment variables on Railway](docs/screenshots/en/railway.svg)

**2. List the commands** with `!menu` in the monitor channel.

![Help menu](docs/screenshots/en/menu.svg)

**3. Add target channels** with `!set` followed by one or more channel IDs, separated by spaces.

![Set targets](docs/screenshots/en/set.svg)

**4. Set the send times** with `!time`. GM and GN times go in a single command.

![Set schedule](docs/screenshots/en/time.svg)

**5. Check the configuration** with `!list` to see the GM and GN times, the target count, and the list of active target IDs.

![Configuration list](docs/screenshots/en/list.svg)

**6. The bot sends messages** to the target channels at the configured times.

![Sent messages](docs/screenshots/en/result.svg)

**7. Remove a target** any time with `!stop 1234567890`.

## Input format

### Commands

All commands **only work in the monitor channel**.

| Command | Purpose | Example |
| ------- | ------- | ------- |
| `!menu` | Show the help guide | `!menu` |
| `!set` | Add one or more target channel IDs | `!set 1234567890 0987654321` |
| `!time` | Set the automatic GM and GN send times | `!time gm:07.00, gn:19.00` |
| `!list` | Show the active configuration and target IDs | `!list` |
| `!stop` | Remove a channel ID from the targets | `!stop 1234567890` |
| `!test` | Send a manual GM or GN test to all targets | `!test gm` |

Quick rules:

- Channel IDs are separated by **spaces** in `!set`.
- Times use a **dot** (`07.00`), and the `gm:` and `gn:` pairs are separated by a **comma** in `!time`.
- To get a channel ID: enable **Developer Mode** in Discord (Settings > Advanced), right-click the channel, choose **Copy Channel ID**.

### Environment variables

A full example is in `.env.example`.

| Variable | Example | Description |
| -------- | ------- | ----------- |
| `DISCORD_USER_TOKEN` | `MTI3...` | Token of the Discord account the bot runs on (**secret**) |
| `MONITOR_CHANNEL_ID` | `123456789012345678` | Channel where the bot posts status and receives commands |
| `TIMEZONE` | `Asia/Jakarta` | Reference timezone for the schedule |

### Storage

Targets and schedule are stored in `/data/config.json`. Make sure `/data` is a **Railway Volume** so the file survives container rebuilds.

## Privacy

![Privacy and security boundaries](docs/security.en.svg)

- The token is stored **only** in Railway environment variables. Never commit a token; `.env.example` only contains placeholders.
- The bot exposes no web service, uses no database, and has no analytics. The only stored data is `config.json`, containing target channel IDs and the schedule.
- A token is equivalent to your account password. If it leaks, **change the account password** to reset the token.
- Use a **dedicated account**, not your main one.
- This bot automates a user account (self-bot), which **violates Discord's Terms of Service**. The account may be limited or banned. Use at your own risk.

## Development

Requires Python and pip. Run from the repo root.

### Run locally

```
pip install -r requirements.txt
pip install -e .
```

Linux / macOS:

```
export DISCORD_USER_TOKEN=your_token
export MONITOR_CHANNEL_ID=123456789012345678
export TIMEZONE=Asia/Jakarta
python -m gmgn_bot
```

Windows (CMD):

```
set DISCORD_USER_TOKEN=your_token
set MONITOR_CHANNEL_ID=123456789012345678
set TIMEZONE=Asia/Jakarta
python -m gmgn_bot
```

### Lint and formatting

Run style checks from the repository root:

```
pip install -r requirements-dev.txt
ruff check .
ruff format --check .
```

Pytest is included for contributors who add regression tests.

### Run with Docker

```
docker build -t gmgn-bot .
docker run --env-file .env -v gmgn-data:/data gmgn-bot
```

### Deploy to Railway

1. Create a new GitHub repository and push this folder.
2. Open <https://railway.app/>, create a new project, choose **Deploy from GitHub repo**, and connect that repository.
3. In **Variables**, add `DISCORD_USER_TOKEN`, `MONITOR_CHANNEL_ID`, and `TIMEZONE`.
4. Add a **Volume** and mount it at `/data` so `config.json` persists.
5. Railway builds with the `Dockerfile` and runs the worker defined in the `Procfile`.

### Tech stack

- **Python** with a `src/` layout, run via `python -m gmgn_bot`
- **Dockerfile** and **Procfile** for Railway
- **pytest** for behavior tests, **Ruff** for lint and formatting
- Persistent config as a single JSON file, no database

### Folder structure

```
src/gmgn_bot/
  __init__.py           Public package exports
  __main__.py           Entry point: python -m gmgn_bot
  settings.py           Read and validate environment variables
  storage.py            Read/write config.json
  messages.py           Message pool + weighted selection
  scheduler.py          Scheduling and periodic delivery
  commands.py           Handlers for !menu/!set/!time/!list/!stop/!test
  client.py             Discord client and event handlers
  logging_setup.py      Logging configuration
docs/
  logo.svg
  hero.id.svg / hero.en.svg
  architecture.id.svg / architecture.en.svg
  social-preview.svg
  screenshots/
    railway.svg · menu.svg · set.svg · time.svg · list.svg · result.svg
    en/                 English-labeled illustrations
.env.example            Environment template without real credentials
pyproject.toml          Ruff and pytest configuration
requirements.txt        Runtime dependencies
requirements-dev.txt    Development dependencies
Dockerfile              Docker/Railway container
Procfile                Worker start command
```

### Known limitations

- **Commands only work in the monitor channel**; the bot ignores other channels.
- **Without a Volume, the configuration is lost** when the container is recreated.
- **Account risk**: automated sending through a user account can trigger Discord restrictions, and some servers forbid automated messages. Follow each server's rules.

## Contributing

Contributions are welcome.

1. **Fork** this repo and create a new branch, e.g. `feature/your-feature`.
2. Install dev dependencies: `pip install -r requirements-dev.txt`.
3. Change code in `src/gmgn_bot/` and **add or update tests** in `tests/`.
4. Make sure everything passes before opening a PR:

   ```
   pytest
   ruff check .
   ruff format --check .
   ```

5. Open a **Pull Request** with a short description of what changed and why.

For bugs or feature requests, open an **Issue** with reproduction steps and logs (without tokens).

This project is not affiliated with Discord Inc. Released under the [MIT](LICENSE) license.
