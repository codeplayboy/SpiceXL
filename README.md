# SpiceXL

SpiceXL is a responsive streaming catalogue client with a Capacitor-powered Android shell and an Electron Windows target. It is designed around fast, state-preserving navigation, resilient catalogue hydration, and explicit playback-source diagnostics.

SpiceXL only consumes metadata and playback routes returned by an authorized backend. It does not include scraping, decryption, DRM bypassing, provider credentials, or embedded private APIs.

## Official release — SpiceXL Android v1.1.1

This is the finalized official SpiceXL v1.1.1 Android build. It includes the Home Anime editorial rail update and the latest progressive-loading fix.

### What is fixed

- The Home page has a dedicated **Anime** editorial rail containing high-signal titles across action, shonen, thriller, psychological, fantasy, science fiction, romance, anime films, sports, comedy, and genre classics.
- The editorial Anime rail is isolated from **Recent Anime**. Recent Anime continues to use the latest verified release feed and is not randomized or replaced.
- Stale catalogue refreshes can no longer overwrite the Home Anime editorial selection with the newest-release feed.
- The editorial selection is cached independently and deterministically shuffled per three-day cycle. It remains stable during a cycle and changes when the next cycle begins.
- Anime cards render progressively: the first verified search batch is committed immediately while the remaining curated queries continue in the background.
- The Home rail updater replaces stale cached Anime cards instead of append-only hydration, preventing recent titles from remaining at the front of the editorial rail.
- The official Android APK was rebuilt, installed on the connected TECNO KI7 test device, and relaunched successfully.

### What remains backend-dependent

- A title can still be unavailable when the authorized backend returns no metadata, no artwork, or no playback route. The client cannot manufacture a provider route safely.
- Provider names and advertised language labels are not proof of the actual stream language. Verified stream metadata remains authoritative; mislabeled upstream routes require backend correction.
- Playback quality and audio-track availability depend on the selected provider's manifest. The client preserves explicit user choices but cannot add a missing 720p, 1080p, 4K, or native-language track that the provider does not expose.
- The current phone validation was an install/relaunch verification. A full three-day timer cycle and every provider route cannot be measured in a single session.
- The APK is the official finalized project build and uses development signing for direct installation. It is not Play Store production-signed.

### Editorial refresh model

The main Anime rail uses a separate local cache key and a three-day editorial window:

1. Load the last valid editorial selection immediately when available.
2. Render the first verified batch without waiting for the full query set.
3. Continue collecting and deduplicating the remaining title metadata in the background.
4. Shuffle the completed pool using the current three-day cycle seed.
5. Persist the completed selection and schedule the next refresh.

The Recent Anime rail does not share this cache, timer, shuffle, or replacement path.

## Build verification

The official Android build was checked with `node --check app.js`, synchronized with Capacitor, assembled with Gradle, installed through ADB, force-relaunched, and started with package `com.spicexl.app`.

APK output:

`android/app/build/outputs/apk/debug/app-debug.apk`

### Official distributables on GitHub

- `SpiceXL-Android-v1.1.1.apk` — official Android package for direct installation.
- `SpiceXL Setup 1.1.1.exe` — the already finalized official Windows installer.

Both distributables are published together in the repository so the Android and Windows downloads remain easy to identify as the same SpiceXL release line.

The build still emits existing Gradle warnings about `flatDir`, SDK XML compatibility, and deprecated Gradle features. They do not prevent installation, but should be cleaned up for a future store submission.

## Local development

```bash
npm install
npm start
```

Run the checks with:

```bash
npm run check
npm test
```

## Configuration and secrets

Copy `.env.example` only for server/build tooling. Do **not** ship provider credentials, cookies, API keys, or tokens in a web build or APK—those values are extractable. Keep all secrets in the API service and expose only a public HTTPS API gateway to the client.

For a hosted web build, set a public `window.SPICEXL_CONFIG` object before loading `app.js`:

```html
<script>window.SPICEXL_CONFIG = { apiBases: ['https://api.example.com'] };</script>
```

## Provider reliability

Each API endpoint is health-ranked and temporarily cooled down after a failure. Playback sources are identity-validated, sorted by health/quality, and fail over only to another route supplied by the configured backend. Playback failures are recorded locally for the session; no source is retried while it is in cooldown.

## Android APK

Prerequisites: Node 20+, Android Studio with Android SDK, and a JDK compatible with the Android Gradle Plugin installed by Capacitor.

```bash
npm run android:prepare
npm run android:add       # first time only
npm run android:sync
npm run android:open
```

Use Android Studio's **Build > Generate Signed Bundle / APK** for a release. Do not commit signing keys or `key.properties`; use CI secrets or Android Studio's protected signing flow.

The Android shell uses the same adaptive layout, touch targets, safe-area spacing, native back behavior, and app background as the web version. Network traffic requires HTTPS; mixed content is disabled.

## Windows app

The Windows target is a separate Electron application. It starts its own local catalogue proxy, so it does not rely on a browser tab or the Android Capacitor shell.

```bash
npm run desktop:dev
npm run desktop:build
```

The installer is written to `release/windows/`. It creates Start Menu and optional desktop shortcuts. The Android project remains under `android/` and continues to be built only with the `android:*` commands.
