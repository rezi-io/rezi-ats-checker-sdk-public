# Rezi ATS Checker SDK (Public CDN)

This is the public CDN distribution repository for the Rezi ATS Resume Checker SDK.

> **Do not edit files in this repo directly.** Source lives in the monorepo at `libs/ats-checker-sdk/`.

## CDN Usage

### Latest version
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/rezi-io/rezi-ats-checker-sdk-public/rezi-ats-checker.css">
<script src="https://cdn.jsdelivr.net/gh/rezi-io/rezi-ats-checker-sdk-public/rezi-ats-checker.min.js"></script>
```

### Pinned to a specific commit
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/rezi-io/rezi-ats-checker-sdk-public@{COMMIT_HASH}/rezi-ats-checker.css">
<script src="https://cdn.jsdelivr.net/gh/rezi-io/rezi-ats-checker-sdk-public@{COMMIT_HASH}/rezi-ats-checker.min.js"></script>
```

## Global API

The SDK exposes a global `ReziATSChecker` namespace after loading.

## Development

Source code lives in the monorepo: `libs/ats-checker-sdk/`

Build and deploy:
```bash
nx build-umd ats-checker-sdk
bash libs/ats-checker-sdk/build-for-distribution.sh
```
