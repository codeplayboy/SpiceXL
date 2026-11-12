<div align="center">
  <img src="docs/assets/spicexl-hero.svg" alt="SpiceXL - watch what moves you" width="100%">
  <br><br>
  <a href="https://github.com/codeplayboy/SpiceXL/releases/tag/v1.1.1"><img src="https://img.shields.io/github/v/release/codeplayboy/SpiceXL?style=flat-square&color=28d7f2&label=official%20release" alt="Official release"></a>
  <a href="https://github.com/codeplayboy/SpiceXL/releases/tag/v1.1.1"><img src="https://img.shields.io/github/downloads/codeplayboy/SpiceXL/v1.1.1/total?style=flat-square&color=ff895c&label=downloads" alt="Downloads"></a>
  <a href="SECURITY.md"><img src="https://img.shields.io/badge/security-no%20secrets%20published-28d7f2?style=flat-square" alt="No secrets published"></a>
  <p><strong>A cinematic catalogue client built for fast discovery, calm navigation, and confident playback.</strong></p>
  <p>Android + Windows · dark interface · responsive rails · resilient playback diagnostics</p>
</div>

## The experience

SpiceXL is a polished streaming catalogue interface for discovering movies, series, anime, TV, and kids content. It keeps the visual language cinematic while the interaction model stays quick: cached sections return immediately, rails hydrate progressively, and playback diagnostics make source selection legible instead of mysterious.

<p align="center"><img src="docs/media/motion-preview.gif" alt="SpiceXL animated catalogue preview" width="760"></p>

The repository intentionally publishes the official distributables and the product story without publishing backend implementation secrets, provider credentials, or private infrastructure details.

## What ships in official v1.1.1

- A dedicated Home Anime editorial rail with genre-balanced, high-signal titles.
- Recent Anime remains a separate verified release feed and is never replaced by the editorial rail.
- Independent three-day editorial caching with deterministic cycle shuffling.
- Progressive first-batch rendering so the Home rail becomes useful before background enrichment finishes.
- Stable catalogue and detail-page presentation across Android and Windows.
- Compact Next Episode behavior, real playback-ended transitions, preserved fullscreen state, and no duplicate episode changes.
- Provider diagnostics that distinguish runtime availability from advertised language labels.
- An official Android APK and the already-finalized Windows installer in the same release line.

## A small visual tour

| Home discovery | Featured catalogue | Playback surface |
|:---:|:---:|:---:|
| <img src="docs/media/home-featured.webp" alt="SpiceXL Home discovery" width="250"> | <img src="docs/media/movies.webp" alt="SpiceXL movie catalogue" width="250"> | <img src="docs/media/player.webp" alt="SpiceXL playback screen" width="250"> |

| Series | Anime | Kids |
|:---:|:---:|:---:|
| <img src="docs/media/series.webp" alt="SpiceXL series catalogue" width="250"> | <img src="docs/media/anime.webp" alt="SpiceXL anime catalogue" width="250"> | <img src="docs/media/kids.webp" alt="SpiceXL kids catalogue" width="250"> |

More product captures are available in [`docs/media`](docs/media/), including search, detail, brand, and motion previews.

## Product principles

| Principle | How SpiceXL approaches it |
| --- | --- |
| Immediate discovery | Reuse cached catalogue snapshots and preserve browsing state when returning to a section. |
| Quiet loading | Keep already-rendered content visible and use progressive hydration instead of blocking the whole page. |
| Honest playback | Treat actual route and stream metadata as authoritative; provider labels are never proof of language or quality. |
| Motion with purpose | Use short, lightweight transitions and a restrained animated brand layer instead of decorative UI noise. |
| Device respect | Adapt the layout to Android safe areas, touch targets, orientation, and the Windows desktop shell. |

## Downloads

### Official v1.1.1 release

- [Android APK](https://github.com/codeplayboy/SpiceXL/releases/download/v1.1.1/app-debug.apk) - direct install package for Android testing devices.
- [Windows installer](https://github.com/codeplayboy/SpiceXL/releases/download/v1.1.1/SpiceXL.Setup.1.1.1.exe) - the already-finalized official Windows build.
- [Release notes and checksums](https://github.com/codeplayboy/SpiceXL/releases/tag/v1.1.1)

The Windows installer was preserved as requested. It was not replaced by the repository presentation work.

The Android package is the official finalized project build for direct installation and uses development signing. It is not Play Store production-signed.

## Security boundary

This public repository contains no `.env` files, API keys, provider credentials, session cookies, access tokens, private signing keys, or unpublished infrastructure details. See [`SECURITY.md`](SECURITY.md) for the audit boundary and contribution rules.

SpiceXL consumes metadata and playback routes returned by an authorized HTTPS backend. It does not include scraping, decryption, DRM bypassing, or embedded private APIs. A client cannot safely create a route that the configured backend does not return.

## Known limitations

- Artwork, playback quality, audio tracks, and route availability remain dependent on upstream metadata and provider manifests.
- Advertised language labels may be incorrect upstream. Verified stream metadata is the authority; a mislabeled source requires backend correction.
- The direct-install Android artifact is not a store-signed production release.
- A full provider matrix and a complete three-day refresh cycle require runtime testing against the authorized backend, not a static repository inspection.

## Build verification

The Android release line was checked with JavaScript syntax validation, Capacitor synchronization, Gradle assembly, ADB installation, force relaunch, and package startup on the connected TECNO KI7 test device. Existing Gradle warnings about `flatDir`, SDK XML compatibility, and deprecated Gradle features do not prevent installation.

```bash
npm install
npm run check
npm test
npm run android:sync
npm run android:build
```

For Windows development:

```bash
npm run desktop:dev
npm run desktop:build
```

Do not commit `key.properties`, signing keys, `.env` files, or generated unpacked installers. Use protected CI secrets or Android Studio's signing flow for a store release.

## Documentation

- [`CHANGELOG.md`](CHANGELOG.md) - official release history and known limitations.
- [`docs/INSTALLATION.md`](docs/INSTALLATION.md) - Android and Windows installation notes.
- [`docs/PLAYBACK_STATUS.md`](docs/PLAYBACK_STATUS.md) - source, language, and quality behavior.
- [`docs/RELEASE_CHECKSUMS.txt`](docs/RELEASE_CHECKSUMS.txt) - published artifact checksums.
- [`SECURITY.md`](SECURITY.md) - repository security boundary.

<div align="center">
  <sub>SpiceXL · watch what moves you</sub>
</div>
