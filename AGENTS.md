# AGENTS.md

## Cursor Cloud specific instructions

### Project overview

BlockOps Pro is a React Native / Expo mobile app (frontend only) for a B2B verified trades marketplace. The source code lives in `blockops-mobile/blockops-mobile/`. There is no backend in this repo — the app talks to a remote API at `https://tasker.blockops.au/api`.

### Running the app

- **Web mode** (recommended for Cloud agents): `npx expo start --web --port 8081` from `blockops-mobile/blockops-mobile/`
- iOS/Android emulators are not available on Linux VMs; use web mode for testing.
- The app requires placeholder asset PNGs in `blockops-mobile/blockops-mobile/assets/` (`icon.png`, `splash.png`, `adaptive-icon.png`, `favicon.png`). If missing, the Expo web bundler will fail. The update script generates them automatically.

### Linting

Run `npx expo lint` from `blockops-mobile/blockops-mobile/`. The `metro.config.js` `__dirname` error is a false positive (CommonJS file in ESLint's default ESM scope).

### Key gotchas

- `react-native-worklets` must be installed as a peer dependency of `react-native-css-interop` (used by NativeWind v4). The update script handles this via `--legacy-peer-deps`.
- `expo-font` must be installed for web rendering (required by `expo-router` static rendering). The update script handles this.
- No `.env` file is needed — the API base URL is hardcoded in `lib/api.ts` and `lib/constants.ts`.
- No automated test suite is included in this repo (no test runner configured in `package.json`).
