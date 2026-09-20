# CostAm — tester site

One static page for people sent a link to install CostAm, the Android app that
works out what a batch of baking actually costs to make.

Live: <https://costam-green.vercel.app>

## What is here

```
index.html        the whole page
style.css         the whole stylesheet
assets/           app icon, favicon, four screenshots
vercel.json       the /costam.apk redirect, and cache headers
.vercelignore     keeps internal docs out of the deployment
```

No framework, no build step, no npm dependencies. Fonts come from Google Fonts;
nothing else is fetched from outside. The whole page is about 172 KB.

## Where the download actually comes from

The current build is **111,455,364 bytes (106 MiB)**. That is over two hard limits:

- GitHub rejects any file over 100 MiB in a push, so `costam.apk` is in `.gitignore`
- Vercel rejects any deployment file over 100 MB, so it cannot be a static asset either

So the APK is published as a **release asset** on this repository, and `vercel.json`
redirects the fixed path `/costam.apk` to it. The page still links to `/costam.apk`
and always will; only the redirect target changes between versions. GitHub serves
release assets with `Content-Disposition: attachment` and the correct
`application/vnd.android.package-archive` type, with no sign-in wall.

To publish a new build:

```bash
eas build:list --platform android        # confirm package + version FIRST
curl -L -o costam.apk "<artifact url>"
gh release create vX.Y.Z costam.apk --repo oramastudiosng/costam-site
```

Then update by hand and redeploy with `vercel --prod`:

- the `destination` in `vercel.json` (point it at the new tag)
- the file size in install step 1 of `index.html`
- the version and date in the footer of `index.html`

If a future build comes in under 100 MB, drop the redirect and let the file sit in
the repo root — Vercel will serve it directly, which is what the brief wanted.

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
