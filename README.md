# flutter-apk-builder — Repo → Signed APK, as a REST API

**Live API + docs:** https://princekauz-flutter-apk-builder.hf.space/docs

Send a Flutter project (zip upload, GitHub repo import, or file-by-file via API), get back a
signed release APK (or split per-ABI / AAB mode). No Android Studio, no local SDK, no Gradle
setup, no CI YAML. Full Android toolchain runs server-side; you get a download link.

## Quickstart

```bash
# 1. Import your project (zip)
curl -X POST https://princekauz-flutter-apk-builder.hf.space/api/projects/import \
  -F "file=@my-app.zip"

# 2. Kick off a full release build
curl -X POST "https://princekauz-flutter-apk-builder.hf.space/api/project/my-app/build?mode=full"

# 3. Poll
curl https://princekauz-flutter-apk-builder.hf.space/api/status/{job_id}

# 4. Download
curl -o my-app-release.apk "https://princekauz-flutter-apk-builder.hf.space/api/download/my-app/full"
```

## Why this exists

Building a Flutter APK outside a full local setup is a known circle of hell:

- Android Studio is a multi-GB install that owns your machine.
- GitHub Actions / Codemagic / Bitrise all want YAML pipelines and account plumbing
  before the first artifact.
- FlutterFlow's free tier doesn't let you export an APK at all.
- Old projects die of Gradle/AGP/Kotlin version drift, and fixing that locally
  is an evening you don't get back.

This service is the boring fix: hand over the project, receive the artifact.

## Modes

| mode | output |
|---|---|
| `full` | universal release APK |
| `split` | per-ABI APKs (smaller downloads) |
| `appbundle` | AAB for Play Store upload |

## Build service (paid)

Self-serve builds are the API above. If you'd rather not touch the API at all:

- **First build free** — verify quality before paying.
- **$12 per build** (or 3 for $29) — you send a repo link, we return the APK.
- **Legacy rescue** — "it used to build, now it doesn't": $49–99 flat, includes a
  diff of exactly what we changed and why.
- **Build credits** — 10 prepaid builds for $79, no expiry, API access included.

Request a build or ask anything: **newmark00010@gmail.com** — replies within a day.

## Tech

Runs on Hugging Face Spaces (Docker). Gradle 9.x / AGP 9.x / Kotlin 2.x KTS pipeline,
`compileSdk 36` override at the root for modern target requirements, per-project isolation.
REST API documented via OpenAPI at `/docs`.

## Fair use

This is a shared free-tier service — don't hammer it, don't upload malware, don't build
things you don't have the rights to. Builds of projects you don't own will be removed.
