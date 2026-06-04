# AGENTS.md

## Cursor Cloud specific instructions

### Repository layout

The git repo tracks `blockops-mobile.zip` at the workspace root. The app source lives in `blockops-mobile/` after unzip. There is **no backend** in this repository; the mobile client talks to `https://tasker.blockops.au/api` (see `blockops-mobile/lib/api.ts`).

### Install & refresh dependencies

From the workspace root:

```bash
unzip -qo blockops-mobile.zip -d .
cd blockops-mobile
npm install --legacy-peer-deps
```

NativeWind v4’s Babel pipeline also needs packages that are not listed in the committed `package.json` inside the zip. Install them after `npm install` (use `--no-save` if you must not change `package.json`):

```bash
npm install expo-font@~12.0.10 react-native-worklets@0.9.1 --legacy-peer-deps --no-save
```

### Placeholder assets (required for Metro / web)

`app.json` references `./assets/icon.png`, `splash.png`, `adaptive-icon.png`, and `favicon.png`, but the zip only ships `assets/README.md`. Without those PNGs, `expo start --web` fails with `ENOENT` on `favicon.png`. Create minimal placeholders once per workspace (see `blockops-mobile/assets/README.md` for brand specs), or add real branded assets before store builds.

### Running the app (Cloud VM / Linux)

| Goal | Command (from `blockops-mobile/`) |
|------|-----------------------------------|
| Dev server (QR / interactive) | `npm start` |
| **Web** (best on Linux cloud VMs) | `npm run web` or `npx expo start --web --port 8081` |
| Android | `npm run android` (emulator/device required) |
| iOS | `npm run ios` (macOS + Xcode only) |

Use `CI=1` and `EXPO_NO_TELEMETRY=1` in non-interactive environments. Prefer **tmux** for long-running `expo start` sessions.

### Lint & typecheck

```bash
cd blockops-mobile
npm run lint          # may auto-install eslint on first run
npx tsc --noEmit      # known implicit-any issues in dashboard/jobs screens
```

`npm run lint` currently reports one ESLint error in `metro.config.js` (`__dirname` / `no-undef`) plus unused-import warnings; `tsc` reports a few implicit-`any` parameters. These do not block Metro web bundling once dependencies and assets are in place.

### Tests

No unit/integration test script is defined in `package.json`. Validate changes via lint, `npx tsc --noEmit`, and manual web or device testing against the hosted API.

### API / E2E behavior

- No `.env` is required; API base URL is hardcoded.
- Auth uses `x-session-token` stored in AsyncStorage (`blockops_token`).
- Public smoke test: `curl https://tasker.blockops.au/api/jobs` should return HTTP 200.
- Full login/job flows need valid credentials on the hosted API (not documented in-repo).

### Production builds

EAS builds need a real `eas.projectId` in `app.json` and branded assets. See `blockops-mobile/README.md`.
