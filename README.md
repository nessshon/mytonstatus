# 📊 My TON Status

[![TON](https://img.shields.io/badge/TON-grey?logo=TON&logoColor=40AEF0)](https://ton.org)
[![Gatus](https://img.shields.io/badge/Gatus-v5.36.0-blue)](https://github.com/TwiN/gatus)

Self-hosted [Gatus](https://github.com/TwiN/gatus) uptime dashboard for TON
services — frontends, backends and APIs — with Telegram alerts on incidents.

## Quick Start

```bash
cp .env.example .env
docker compose up -d
```

Then open the dashboard in your browser.

## Configuration

All `*.yaml` under `config/` merge into one — one subject per file:

| File | Defines |
|------|---------|
| `global.yaml` | title, logo, favicon, buttons, layout |
| `storage.yaml` | history backend (SQLite) |
| `alerts.yaml` | alert providers (Telegram) |
| `endpoints/*.yaml` | checks, one file per service |

**UI** — under `ui:` in `global.yaml`:

- `title`, `header` — page title and top-bar text
- `logo`, `favicon` — branding, served locally from `./assets`
- `footer` — footer text
- `buttons` — links shown in the top bar
- `hide-controls` — hide the search / filter / sort bar
- `default-sort-by` — `group`, `name` or `health`
- `default-expanded` — open all groups by default

**Assembling configs**

- One file per service under `endpoints/` — names are for humans; all files merge regardless.
- Keep each scalar key in a single file: the same non-map key in two files breaks the merge.
- Reference brand assets by path (`/assets/...`), not external URLs — served locally, no flicker on load.

## Telegram Alerts

A customised Gatus `telegram` provider — styled message, premium emoji, several
recipients (comma-separated ids). Configured in `alerts.yaml`, credentials in `.env`:

```env
TELEGRAM_ALERT_BOT_TOKEN=123456789:AA-...
TELEGRAM_ALERT_CHAT_ID=123456789,987654321
```

Opt in per endpoint with `alerts: [- type: telegram]`; `default-alert` fires
after 3 consecutive failures and resolves after 2 successes.

## Environment

Copy `.env.example` → `.env` and fill in (`.env` is gitignored — never commit real values):

| Variable | Description |
|----------|-------------|
| `GATUS_PORT` | Host port for the dashboard (default `8080`) |
| `TELEGRAM_ALERT_BOT_TOKEN` | Bot token from @BotFather — sends the alerts |
| `TELEGRAM_ALERT_CHAT_ID` | Chat id(s) to notify, comma-separated for several |
| `<BOT>_HEARTBEAT_TOKEN` | Per-bot shared secret; must match the bot's external-endpoint token |
