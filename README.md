# Snap Issue

A Chrome extension (Manifest V3) that captures and annotates screenshots, then uses an AI model to turn them into a GitHub issue. See `.scratch/snap-issue/spec.md` for the full spec.

## Packages

| Package              | Role                                                                    |
| -------------------- | ----------------------------------------------------------------------- |
| `packages/extension` | WXT + React + TypeScript extension (popup, editor, options, background) |
| `packages/helper`    | Node + TypeScript native messaging host for the local AI CLIs           |
| `packages/shared`    | Zod schemas and logic shared by the other two packages                  |

## Dev setup

Requirements: Node 22 or newer, and pnpm via Corepack (`corepack enable`). The pnpm version is pinned in `package.json`.

```sh
pnpm install     # also runs `wxt prepare` to generate extension types
pnpm build       # builds every package
pnpm test        # Vitest in every package
pnpm typecheck   # tsc in every package
pnpm lint        # ESLint + Prettier check (`pnpm format` to fix)
pnpm dev         # WXT dev mode with hot reload (opens a Chrome instance)
```

## Load the unpacked extension

1. Run `pnpm build`.
2. Open `chrome://extensions` and turn on **Developer mode**.
3. Click **Load unpacked** and choose `packages/extension/dist/chrome-mv3`.
4. Click the toolbar icon for the popup. Open the options page from the extension's **Details** page. The editor page is at `chrome-extension://<extension-id>/editor.html`.

After a rebuild, click the reload icon on the extension's card.

## Team image host

Screenshots are uploaded to an S3-compatible bucket. To ship the team's bucket inside the build, so nobody has to set it up:

1. Copy `packages/extension/.env.example` to `packages/extension/.env`. The `.env` file is git-ignored.
2. Fill in the `WXT_S3_*` values.
3. Run `pnpm build`. The build bakes the values in and grants the endpoint and public URL as host permissions.

Anyone who has the built extension can read these credentials. Use a token scoped to that one bucket, with object read and write only, and share the build only within the team. **Options › Image host** still overrides any field.

The Chrome Web Store build (`pnpm --filter @snap-issue/extension zip:store`) leaves these values out. See [docs/chrome-web-store.md](docs/chrome-web-store.md).

## Sign in to GitHub

Snap Issue files issues through a GitHub App with the device flow. The app's Client ID is committed in `packages/extension/lib/github/config.ts`.

- **Using the team's app:** open the options page and click **Sign in with GitHub**. The app must be installed on the repos you want to file into.
- **Creating your own app:** follow [docs/github-app-setup.md](docs/github-app-setup.md) (exact permissions, installing the app on repos), put the new Client ID in the config file and rebuild. With an empty Client ID the options page shows "Client ID not configured".

## Local helper (Claude Code and Codex with your own login)

The "Claude (local)" and "Codex (local)" AI providers run the `claude` and `codex` CLIs on your computer through a native messaging helper (`com.snapissue.helper`), so they use your Claude subscription or ChatGPT login instead of an API key. macOS and Linux only. You need at least one of the two CLIs.

1. Install the CLIs you want and sign in once:
   - **Claude Code**: install [Claude Code](https://docs.claude.com/en/docs/claude-code) and run `claude` in a terminal to sign in.
   - **Codex**: install the Codex CLI, or the ChatGPT desktop app, which bundles it. Run `codex login` and sign in with ChatGPT; `codex login status` should then say `Logged in using ChatGPT`.
2. Find the extension's ID: open `chrome://extensions`, turn on **Developer mode**, and copy the ID on the Snap Issue card (32 letters a–p). An unpacked extension's ID depends on the folder it was loaded from, so reinstall the helper if you load it from somewhere else.
3. Run, from the repo root:

   ```sh
   pnpm helper:install --extension-id <extension-id>
   ```

   This builds the helper, copies it to `~/.snap-issue/helper/` with a launcher that uses the absolute `node` path, records the absolute paths of `claude` and `codex` in `config.json` there, and writes the host manifest for Chrome (`~/Library/Application Support/Google/Chrome/NativeMessagingHosts/` on macOS, `~/.config/google-chrome/NativeMessagingHosts/` on Linux). Add `--browser chromium` or `--browser chrome-for-testing` (repeatable) for other browsers.

   Each CLI is looked up on your `PATH`, then in the usual install locations: for `claude`, `~/.local/bin`, `~/.claude/local`, `/opt/homebrew/bin` and `/usr/local/bin`; for `codex`, the ChatGPT app (`/Applications/ChatGPT.app/Contents/Resources/codex-cli/bin/codex`), `~/.codex/plugins/.plugin-appserver/codex-cli/bin/codex`, `/opt/homebrew/bin` and `/usr/local/bin`. A path you put in `config.json` by hand is kept when you reinstall.
4. Reload the extension. The options page's **Status** panel shows whether the helper is reachable and, for each CLI, whether it was found and is signed in (from `claude auth status` and `codex login status`).

Run the command again after pulling helper changes or installing a CLI somewhere new. Each generate runs in a temporary directory that is deleted afterwards and keeps no session: `claude -p` with no tools, or `codex exec` in a read-only sandbox. It is stopped after 120 seconds or when you click Cancel.
