# Journally 📚

A calm, private journal for your desktop. Write one entry a day, in as many journals as you like. Everything is encrypted on your computer. No account, no server, no cloud.

Built with Electron, Svelte 5 and TypeScript.

> **Status:** Works on Windows. Phases 0–5 are done (encryption, timeline and search, backup and restore, multiple journals). Export and the first public release are still to come.

---

## Why Journally

- **Private by default.** Entries, settings and photos are encrypted on disk. Someone who copies your data folder without your password learns nothing, not even dates, tags or moods.
- **Calm to write in.** Opening the app puts you straight into today's page. No prompts, no clutter, no animation while you type.
- **No guilt.** Missing a day never resets anything. There are no streaks.
- **Your data, your copies.** Make a backup as a single encrypted `.zip` file, and keep it wherever you like, including Google Drive or OneDrive. A reminder appears when your last backup is 14 days old.

## Features

**Writing ✒**
- One entry per day, per journal. Autosaves as you type.
- Optional mood (5 colours), tags, a ⭐ "memorable" star, and photos (drag, paste or pick).
- Photos lose their location data (EXIF/GPS), are shrunk to 2000 px, turned upright and encrypted.
- Past days open read-only, with an Edit button.

**Journals 📔**
- Keep up to 20 named journals (for example School, Dreams, Private), each with its own icon.
- A journals shelf shows each journal and when it was last written in.
- Timeline, calendar and search cover all journals, with chips to narrow them to one.

**Accessing your Journals 🔍**
- Timeline: a list of entries, newest first.
- Calendar: a month view with a mood dot on each written day.
- Search across text, with highlighted results.
- Filters: tag, mood, starred, has photos, date range. You can also type them, for example `#tag:family`, `#mood:good`, `#starred` or `#date:01/01/26-31/03/26`.

**Security and privacy 🛡**
- Encryption with XChaCha20-Poly1305. Keys are derived from your password with Argon2id.
- A 12-word recovery key is shown once at setup, so you can still get in if you forget the password.
- Auto-lock after 1–60 minutes idle, on sleep, and when Windows locks.
- Electron hardening: context isolation, sandbox, no Node access in the interface, strict Content Security Policy, no outside navigation.

**Personalization 🎨**
- Custom accent colors
- Light / Dark / Match Windows theme

## Important: read this before you start

**Your data is encrypted. If you lose both your password and your recovery key, nobody can get your entries back, including the author of this app.**

- **Write the recovery key down** and keep it somewhere safe, offline. It is shown only once.
- **Keep `keyfile.json` with your data.** It holds the locked keys. Without it, neither the password nor the recovery key can open anything. Every backup includes it.
- **Back up regularly.** Settings → Backup creates one `.zip` file. Restore it from Settings, or from the first-run screen.
- Anyone who has your password (or recovery key) and your data folder can read everything. Journally does not protect against malware or a keylogger on a computer while the journal is open.

## Install

Download the Windows installer from the [Releases](../../releases) page (once published), or build it yourself (see below).

## Build from source

You need [Node.js](https://nodejs.org/) (LTS) and Git.

```powershell
git clone <this repository's URL>
cd journally
npm install
npm run dev
```

### Scripts

| Command | What it does |
|---|---|
| `npm run dev` | Starts the app in development mode. Data goes to `%APPDATA%\Journally-dev`. |
| `npm run dev:samples` | Same, plus made-up entries for past days (for testing only). |
| `npm run build:win` | Builds the Windows installer. |
| `npm run build:unpack` | Builds an unpacked app folder, without the installer. |
| `npm test` | Runs the tests (Vitest). |
| `npm run test:speed` | Times unlocking with 3,650 encrypted entries. |
| `npm run typecheck` | Checks the TypeScript and Svelte types. |

The installed app stores its data in `%APPDATA%\journally\journal`.

macOS and Linux build scripts exist (`build:mac`, `build:linux`), but only Windows has been tested.

## How it is built

```
src/core/      Portable core (plain TypeScript + Node): crypto, storage, keys,
               search, backup, restore, journals. No Electron, no Svelte.
src/main/      Electron main process: window, security, auto-lock, IPC to the core.
src/preload/   Exposes a small named API to the interface (window.journally).
src/renderer/  Svelte 5 interface: screens and components.
tests/         Vitest tests for the core and the interface.
```

- **Crypto** lives only in `src/core/crypto.ts`, using [libsodium](https://libsodium.gitbook.io/doc/) (`libsodium-wrappers-sumo`). No custom cryptography.
- **Storage** is one encrypted file per entry, plus encrypted settings and photos. There is no database.
- **Keys** never leave the main process. The interface only receives decrypted entries through the preload API.

The full design, file formats and security reasoning are in [PRD.md](PRD.md). What is built so far is listed in [FEATURES.md](FEATURES.md).

## Credits

- Recovery words use the [EFF Large Wordlist](https://www.eff.org/dice) by the Electronic Frontier Foundation, licensed under [CC BY 3.0 US](https://creativecommons.org/licenses/by/3.0/us/). Copied unchanged (dice numbers removed) into `src/core/wordlist.ts`.
- Interface font: [Figtree](https://fonts.google.com/specimen/Figtree), licensed under the [SIL Open Font License 1.1](https://openfontlicense.org/). Bundled with the app, never loaded from the internet.
- Built on [Electron](https://www.electronjs.org/), [electron-vite](https://electron-vite.org/), [Svelte](https://svelte.dev/) and [libsodium](https://libsodium.org/).

## License

Not yet chosen. (Add a `LICENSE` file before publishing, so others know what they may do with the code.)
