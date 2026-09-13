# Frame3D — releases

Build artifacts for the **Frame3D Spatial Browser** (Meta Quest and Android phone), and the
manifest the app reads to update itself.

The source code is not here.

## For the headset

In the app: **Settings → About → Update address**, paste this once —

```
https://raw.githubusercontent.com/subscriptions-dot/frame3d-releases/main/frame3d-latest.json
```

Then **Check for updates → Install**, and confirm when the system asks. That address never
changes; only the file behind it does.

## What the manifest is

```json
{
  "versionCode": 195,
  "versionName": "1.84-rc62",
  "url": "https://github.com/…/releases/download/<tag>/Frame3D-quest.apk",
  "sha256": "…",
  "sizeBytes": 380483740,
  "notes": ""
}
```

The app refuses the download unless it hashes to exactly `sha256`, and the platform refuses to
install it unless it is signed by the same key as the build already on the device. Both checks
matter and neither replaces the other.

## Publishing a new build

From the source tree:

```bash
./tools/publish-update.sh v1.84-rc63
```

It builds both flavours, hashes the Quest APK, writes the manifest, creates the release and
updates the copy of the manifest on this branch.

`versionCode` in `app/build.gradle.kts` has to go up first. Android refuses to install a package
whose code is not higher than the installed one, so a forgotten bump makes the update do nothing
at all — silently.
