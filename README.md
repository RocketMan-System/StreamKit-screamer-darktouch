# StreamKit Effect Simple Template

This repository is a minimal static template for a RocketMan StreamKit extension (effect). The package is a simple archive containing a `manifest.json` and media files that the host application will use directly — no build system, no Node.js, and no runtime frameworks are required.

Key files

- [`manifest.json`](manifest.json) — primary configuration file for the extension.
- [`logo.svg`](logo.svg) — recommended icon for the extension (SVG preferred; `logo.png` is acceptable).
- [`media/`](media) — folder containing media assets referenced by the manifest (for example [`media/audio.mp3`](media/audio.mp3), [`media/video.webm`](media/video.webm)).

Overview

The package is intended to be a static bundle: manifest, optional icon, and optional media files. The host reads `manifest.json` and plays the referenced media according to the manifest parameters.

Manifest fields

The `manifest.json` contains metadata and runtime parameters. Common fields:

- `name` — package name. May be a string or a localized object.
- `description` — optional; may be a string or a localized object.
- `author` — package author.
- `version` — package version.
- `type` — either `effect` or `screamer`.
- `params` — optional block that configures media content. Can include `audio` and/or `video` entries; using both is optional.
- `settings` — effect settings (implementation details are not described here).

Example `params` section:

```JSON
"params": {
  "video": {
    "path": "media/video.webm",
    "hideEnd": true
  },
  "sound": {
    "path": "media/audio.mp3"
  }
}
```

Notes on `params`:

- Each `path` is a relative path inside the package (for example `media/audio.mp3`).
- Either `video` or `sound` (or both) may be omitted if the effect uses only one type of media.

Packaging and release

The package should include `manifest.json`, optional icon, and any referenced media files. The CI workflow in [`.github/workflows/build-and-release.yml`](.github/workflows/build-and-release.yml) demonstrates the packaging steps used in this repository; it creates a `build` directory, copies the manifest, icon and `media/` folder, then creates an archive named `main.zip`.

Packaging commands from CI:

```bash
mkdir build
cp manifest.json build/
cp logo.* build/ 2>/dev/null || true
cp -r media build/ 2>/dev/null || true
cd build
zip -r ../main.zip .
```

What to include in the archive

- [`manifest.json`](manifest.json:1) — required and must be valid.
- Icon file (`logo.svg` or `logo.png`) — recommended if available.
- `media/` folder with files referenced by `params` (for example [`media/audio.mp3`](media/audio.mp3:1), [`media/video.webm`](media/video.webm:1)).

Recommendations and security

- Do not include secrets (API keys, tokens, passwords) in the package.
- Keep `manifest.json` minimal and include only the parameters required by the effect.
- Ensure that paths declared in `params` match the files included in the archive.

Author and license

Author: RocketMan

This template is provided as-is. Adapt `manifest.json` and the contents of the `media/` folder to match the needs of your effect and the host application.
