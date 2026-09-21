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

## Where the download comes from

`costam.apk` is a plain static file in this folder, served straight from
`/costam.apk`. It is **not in git**: binaries bloat a repo permanently and each
release would add another ~59 MB, so `.gitignore` excludes it and `.vercelignore`
deliberately does not.

**That means the deploy has to run from a working folder that has the APK in it.**
A Git-integration deploy would ship the site without the download:

```bash
vercel --prod
```

To publish a new build:

```bash
eas build:list --platform android      # confirm package + version FIRST
curl -L -o costam.apk "<artifact url>"
unzip -l costam.apk | grep '^.*lib/'   # must be arm-only, no x86
```

Then update by hand and redeploy:

- the file size in install step 1 of `index.html`
- the version and date in the footer, and the `download` attribute on the button
- the `Content-Disposition` filename in `vercel.json`

### If a build ever exceeds 100 MB again

Vercel rejects any deployment file over 100 MB and GitHub rejects any pushed file
over 100 MiB. The 2.0.0 build was 106 MiB and hit both, and was served through a
GitHub release asset with a redirect in `vercel.json` (see the `v2.0.0` release,
kept for history). 2.1.0 is ARM-only and fits, so the redirect is gone. The real
fix is keeping the build ARM-only rather than reinstating the workaround.

## Current build

| | |
|---|---|
| Version | 2.1.0 (versionCode 2) |
| Package | `com.orama.costam` |
| Size | 61,820,884 bytes / 59 MiB |
| ABIs | `arm64-v8a`, `armeabi-v7a` (no x86 — that is what halved it) |
| Expo project | `@oramastudiosngltd/costam` |
| Build ID | `d1b7e2c9-bae8-45a9-b8cc-caa80e5844bf` |
| Signing | APK Signature Scheme v2 |

## No tracking

There is no analytics, no tracking pixel, no cookie banner, no contact form and
no newsletter signup on this page, by design. Feedback goes to WhatsApp or email.
