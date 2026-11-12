# SpiceXL changelog

## 1.1.1 — Official SpiceXL Android release

### Added

- Genre-balanced Home Anime editorial pool featuring major action, shonen, thriller, fantasy, science-fiction, romance, film, sports, comedy, and classic titles.
- Dedicated three-day editorial cache and deterministic cycle shuffle.
- Progressive first-batch rendering for the Home Anime rail.
- Release verification notes covering Android build, ADB installation, and relaunch.

### Fixed

- Prevented the recent-release Anime feed from appearing in the main Anime editorial rail.
- Prevented catalogue refreshes from overwriting the editorial Home Anime state.
- Replaced stale cached Anime cards when the editorial selection becomes available.
- Kept Recent Anime on its existing latest-release data path.

### Known limitations

- Provider routes, language tracks, artwork, and quality variants remain dependent on the authorized backend response.
- The official finalized APK was verified through Android installation and launch. It uses development signing for direct installation and is not Play Store production-signed.
- Three-day rotation behavior is implemented but cannot be observed end-to-end without waiting for the refresh window or using a controlled clock in a test harness.

### Privacy and security

- No API keys, cookies, provider credentials, or private route data are documented here.
- Playback routes remain backend-supplied and are not hardcoded into the application.
