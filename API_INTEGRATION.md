# ATH License API integration

The project uses the ATH License Worker API through `ThreeOneOSFive/helpers/APONLicenseSDK.swift`. The iOS client calls `POST /api/license/mobile-verify` with only the license key and a per-install Keychain device ID. The app fails closed for patch application until the server response explicitly returns `allow_inject: true`; restore operations remain available.

The mobile endpoint is intentionally used because the API HMAC secret must remain server-side and must not be embedded in the public iOS app.

## API contract

- Base URL: `https://ath-license-bot.failrased.workers.dev`
- Endpoint: `POST /api/license/mobile-verify`
- Request JSON: `{ "key": "ATH-...", "device_id": "..." }`
- Active response requires `valid: true`, `status: "active"`, and `allow_inject: true`.
- The server binds a one-device key to the first valid device.
- The server applies its own mobile rate limit.

The app sends no API HMAC secret. HMAC signing is reserved for authorized server-to-server calls to `/api/license/verify`.

## Local macOS build

```bash
bash ./build_unsigned.sh
```

## Backend deployment

Deploy the Worker and configure its D1 database and secrets according to the backend project's `SETUP.md`. Keep `API_HMAC_SECRET` only as a Cloudflare Worker secret; never place it in the iOS target or a GitHub artifact.

The API remains authoritative for license status and whether injection is allowed.

## Build validation

Build on macOS with Xcode. The sandbox is Linux and does not include `xcodebuild`, so the archive/IPA must be compiled and tested on macOS. A successful unsigned build still needs to be signed using the intended authorized deployment method before installation.
