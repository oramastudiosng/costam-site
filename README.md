# CostAm — tester site

One static page for people sent a link to install CostAm, the Android app that
works out what a batch of baking actually costs to make.

Live: (add the Vercel URL once deployed)

## What is here

```
index.html        the whole page
style.css         the whole stylesheet
assets/           app icon, favicon, four screenshots
vercel.json       content-type and cache headers for the APK
costam.apk        the build people download  (NOT in git — see below)
```

No framework, no build step, no npm dependencies. Fonts come from Google Fonts;
nothing else is fetched from outside. The page is about 172 KB excluding the APK.

## The APK is deliberately not in this repository

The current build is **111,455,364 bytes (106 MiB)**, which is over GitHub's hard
100 MiB per-file limit — a push containing it is rejected outright. So `costam.apk`
is listed in `.gitignore` and uploaded as part of the Vercel deployment instead.

**This means a Git-integration deploy would ship the site without the download.**
Deploy from a local folder that has the APK in it:

```bash
vercel --prod
```

To refresh the APK after a new build:

```bash
eas build:list --platform android          # confirm package + version first
curl -L -o costam.apk "<artifact url>"
```

Then update three things by hand and redeploy:

- the file size in install step 1 of `index.html`
- the version and date in the footer of `index.html`
- `filename` in the `Content-Disposition` header in `vercel.json`

## Current build

| | |
|---|---|
| Version | 2.0.0 (versionCode 1) |
| Package | `com.orama.costam` |
| Size | 111,455,364 bytes / 106 MiB |
| Expo project | `@oramastudiosngltd/costam` |
| Build ID | `61255735-e399-4299-9f82-870016487b71` |
| Signing | APK Signature Scheme v2 |

Nearly half that size is `x86` and `x86_64` native libraries (47 MiB), which only
Android emulators use. A per-ABI build or an app bundle would cut the download
roughly in half for real phones.

## No tracking

There is no analytics, no tracking pixel, no cookie banner, no contact form and
no newsletter signup on this page, by design. Feedback goes to WhatsApp or email.
