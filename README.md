# TapDose site

Static site for https://med.techviv.us (GitHub Pages, deployed by Actions, no Jekyll).
Serves the universal link / App Link verification files and the pages for NFC sticker links (`/t/*` falls through to `404.html`).

## Owner steps
1. GoDaddy DNS: add a `CNAME` record from `med` to `developervivus-sys.github.io`.
2. Repo Settings > Pages: source = GitHub Actions, custom domain = `med.techviv.us`, turn on Enforce HTTPS.
3. Replace `<TEAM_ID>` in `.well-known/apple-app-site-association` (Apple Developer > Membership details).
4. Add SHA-256 fingerprints to `.well-known/assetlinks.json`:
   - EAS upload/build key: `eas credentials -p android`
   - Play App Signing key: Play Console > App signing
   - Debug keystore: `keytool -list -v -keystore android/app/debug.keystore -alias androiddebugkey -storepass android -keypass android`
5. Verify:
   - `curl -sI https://med.techviv.us/.well-known/apple-app-site-association` (expect 200, no redirect; note the Content-Type)
   - `curl -s https://app-site-association.cdn-apple.com/a/v1/med.techviv.us` (Apple's cached copy, within about 24 h)
   - `adb shell pm get-app-links us.techviv.tapdose` (expect `verified`)
