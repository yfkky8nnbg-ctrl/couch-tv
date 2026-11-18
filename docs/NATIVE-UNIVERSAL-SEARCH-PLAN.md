# Native Universal Search — implementation brief

Target device: TCL 65QM6K Pro (Google TV), Couch TV on-TV mode. Development branch: `feature/native-universal-search`.

## Goal
Add a first-class search entry on the Couch TV television home screen. Search movies and TV programs with the TCL remote, display title information plus subscription/rent/buy provider options, and hand selected titles to the corresponding streaming app when possible.

## Requirements
1. Keep all current home features, phone remote, weather, settings, accessibility/Home-watch behavior and Google Assistant remote microphone button unchanged.
2. Add a visible, remote-focusable search entry and a TV-friendly full-screen search/results experience; retain Back and directional navigation.
3. Use the platform Android TV text input method/keyboard. Support voice-to-text when the keyboard exposes dictation; do **not** claim the hardware microphone button can be intercepted while an input is focused.
4. Reuse existing search/provider lookup infrastructure if appropriate, but inspect actual code and data-license/terms before using a provider source. Display free/subscription, rental, and buy offers only when confirmed by reliable available data; show region and offer timestamp when available.
5. Gracefully handle absent offers, unavailable apps, errors, rate limits and provider deep links that only launch an app homepage.
6. Preserve developer's upstream update path and allow later merging of ambient-weather improvements.
7. Provide build/test instructions, signing guidance, rollback, and safe migration from the installed public APK. **No TV deployment or removal without explicit user approval.**

## Source architecture reconnaissance
- `tv-home/web/app.js`, `tv-home/web/app.css`, and `tv-home/web/index.html`: likely home UI and focus/navigation.
- `tv-home/android/`: Android TV home shell/keyboard integration.
- `on-tv/android/`, `on-tv/web/shim.js`: on-TV mode/bridge integration.
- `remote/` and `bridge/`: existing phone remote, lookup and launch mechanisms; inspect before reusing.
- Read `AGENTS.md` and `on-tv/README.md` first. `AGENTS.md` is a safety/setup procedure for TV device changes, not authorization to interact with this user's TV.

## Milestones (save each as its own commit)
1. Inventory current search endpoints, home navigation, JS-to-Android bridge, and tests. Document design/limitations.
2. Implement search affordance, focus flow, results layout and keyboard input with local mock data first.
3. Integrate provider lookup through an allowed data source and clear subscription/rent/buy availability.
4. Deep-link opening with graceful fallback, tests and accessibility checks.
5. Build APK, package safe deployment and rollback instructions. Leave deployment to a separately approved step.

## Acceptance criteria
- D-pad/Select/Back can operate every search state without a phone.
- Keyboard dictation is possible if the installed Android TV keyboard offers it; physical mic still invokes Google Assistant outside our UI.
- Subscription, rental and purchase options appear only when supported by returned data, never invented.
- No regression in Couch TV startup, Home button, weather or phone remote.
- Automated tests pass; build is documented, reproducible and free of accidentally committed signing keys or user credentials.
