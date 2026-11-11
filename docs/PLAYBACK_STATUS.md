# SpiceXL playback status

This is the public troubleshooting boundary for playback reports. It intentionally describes behavior without publishing private service addresses, credentials, cookies, tokens, or raw media URLs.

## Language selection

The app now follows this order:

1. An explicit language choice made by the user.
2. The title's original language preference.
3. Verified audio-track metadata exposed by the loaded media manifest.
4. A provider route only when its structured metadata supports the requested language.

Provider/server display names are labels, not evidence. A route named “Korean”, “English”, or another language can still contain the wrong audio if the upstream metadata is wrong. The app no longer silently treats that label as truth. If the manifest exposes no reliable audio-track language, the app cannot guarantee the audible language without guessing; that requires a provider-side correction or a manifest with track metadata.

## Finding native-language and quality variants

When English is not the preferred language, inspect the complete server/quality list instead of selecting only the highest-rated entry. Some alternate or lower-rated routes expose the native-language audio and more quality choices, including variants such as 720p or 1080p, while a highly rated route may start in English or publish fewer variants.

Recommended steps:

1. Open the server and quality selector for the title.
2. Try alternate entries, including lower-rated entries, when the first route is English or lacks the desired quality.
3. Select the route whose advertised language and quality match your preference.
4. Confirm the actual audio in playback before continuing; a server name alone is not proof of language.
5. Keep the subtitle preference unchanged unless you intentionally want a different subtitle track.

This is a practical provider-availability workaround and is now documented so users do not assume that the highest rating means “best native-language option.” It cannot guarantee a result when the upstream service has not published the desired audio or quality variant.

## Server and quality problems

### Fixed in the client

- Normal internal buffering and recovery no longer produce a false repeated user-seek state.
- Recovery is bounded and duplicate transitions are guarded.
- A real media failure can move to another available route without changing subtitle preferences.
- The original-language preference is applied before generic route labels.
- Server discovery no longer truncates the provider list; every discovered route is retained for selection and fallback while the established priority order remains intact.
- Episode transitions preserve the selected logical server when the same route is returned for the next episode, even when the provider rotates the underlying URL.
- The player uses real media completion for episode advancement, while the Next Episode control appears in the configured pre-finish window.

### Not fixable solely in the client

- A missing 720p, 1080p, 4K, dubbed, or native-language variant must exist upstream before the app can offer it.
- A route that works at start but fails after seeking may reflect upstream range support, expiration, throttling, or network delivery—not a player quality switch.
- A provider can return a mislabeled route or a manifest without language metadata. The app can avoid claiming certainty, but it cannot reconstruct the missing truth.
- Availability can vary by title, region, network, and time.

## Subtitles and preferences

The language-routing correction does not rewrite subtitle files or override a manual subtitle choice. Manual playback preferences remain user-controlled. Automatic selection is conservative when upstream metadata is incomplete.

## Verification for 1.1.0

The release was checked with:

```text
node --check app.js
node --test tests/playback-discovery.test.mjs tests/playback-quality-seek-regression.test.mjs tests/player-preferences.test.mjs
```

The focused playback suite passed 38/38 tests. A release APK was built, installed on a connected Android device, launched, and verified as package `com.spicexl.app`, version `1.1.0`, versionCode `8`.

Automated tests can verify routing decisions and metadata handling; they cannot prove the audible language of a provider stream when the upstream manifest itself omits or falsifies that metadata. That limitation is recorded here instead of being hidden behind a server label.
