# Arsenal — working rules

Arsenal ships in TWO formats from two repos. Every change to any part of Arsenal
must land in both, in the same session.

| Format | Repo | File | Publishes via |
|---|---|---|---|
| iOS / Android app (Capacitor) | `getarsenal/arsenal-app` | `www/index.html` | push to `main` triggers the Codemagic iOS build (App Store) |
| Website — https://getarsenal.app | `getarsenal/getarsenal.github.io` | `index.html` | push to `main` deploys via GitHub Pages |

## Rules
- **Always merge and publish.** When a change is done and verified, merge it to
  `main` in BOTH repos without asking first.
- The two `index.html` files are NOT identical. The app build has native-only
  code (Capacitor plugins, `rc-bundle.js` / RevenueCat) that the web build lacks.
  Never copy one file over the other. Port the change: e.g.
  `git diff` the app change and `git apply -p2` it onto the web repo, or make
  the same edit by hand, then check that it applied cleanly.
- New assets (images, SVGs, etc.) go in `arsenal-app/www/` AND the root of
  `getarsenal.github.io`.
- Native-only features (camera/barcode scan, purchases, share, filesystem) must
  degrade gracefully on the web (`window._isCapacitor` is false there).
- Verify both builds render before publishing (headless Chromium at desktop
  ~1500px and phone widths; dismiss the PIN overlay with
  `document.getElementById('pin').classList.add('gone')`, then `go('<screenId>')`).
- Docs-only commits to `arsenal-app` `main`: add `[skip ci]` to the commit
  message so they don't start an App Store build.
