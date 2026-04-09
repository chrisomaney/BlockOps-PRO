# AGENTS.md

## Cursor Cloud specific instructions

### Project layout

The app source is at `blockops-mobile/blockops-mobile/` (nested directories). All `npm` and `npx expo` commands must run from that directory.

### Running the app (web)

```bash
cd blockops-mobile/blockops-mobile
npx expo start --web --port 8081
```

The app opens at `http://localhost:8081`. There is no local backend; the API is remote at `https://tasker.blockops.au/api` (hardcoded in `lib/api.ts`). Pages that fetch data (e.g. Jobs) will show errors if the API is unreachable — this is expected.

### Lint

```bash
cd blockops-mobile/blockops-mobile
npx expo lint
```

The first run auto-installs `eslint` and `eslint-config-expo`. One pre-existing `no-undef` error on `__dirname` in `metro.config.js` (CommonJS file) is a false positive.

### Key gotchas

- **Missing placeholder assets**: The `assets/` directory ships empty. The first `npm install` run creates placeholder PNG files (`icon.png`, `splash.png`, `adaptive-icon.png`, `favicon.png`) required by `app.json`. Without them, the web bundler fails with `ENOENT: ./assets/favicon.png`.
- **`react-native-worklets`**: NativeWind v4's `react-native-css-interop` Babel plugin references `react-native-worklets/plugin`. This package must be installed with `--legacy-peer-deps` because the latest version requires React Native 0.81+ while the project pins 0.74.5. This is handled by the update script.
- **`expo-font`**: The static rendering path (`"output": "static"` in `app.json`) requires `expo-font/build/server`. Install `expo-font` with `--legacy-peer-deps` if not already present.
- **No `.env` file needed**: The API base URL and auth token key are hardcoded constants.
- **No automated test suite**: The project has no unit/integration tests. Only `expo lint` is available for CI checks.
