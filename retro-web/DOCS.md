# Home Assistant Community App: Retro Web

Retro Web provides a private browser-based library for legally acquired retro
games. The app is intentionally self-hosted: ROMs, BIOS files, covers,
profiles, and saves remain under `/data`.

## Installation

The installation of this app is pretty straightforward and not different in
comparison to installing any other Home Assistant app.

1. Click the Home Assistant My button below to open the app on your Home
   Assistant instance.

   [![Open this app in your Home Assistant instance.][addon-badge]][addon]

1. Click the "Install" button to install the app.
1. Start the "Retro Web" app.
1. Open Retro Web from the Home Assistant sidebar.

## Configuration

External access and external administration are disabled by default.

**Note**: _Remember to restart the app when the configuration is changed._

Example app configuration:

```yaml
log_level: info
external_access: false
external_username: ""
external_password: ""
allow_external_rom_upload: false
external_admin_password: ""
session_timeout_hours: 12
max_upload_mb: 4096
profile_limit: 8
enable_arcade: true
rom_index_interval_minutes: 60
enable_cover_indexing: true
```

### Option: `log_level`

Controls the level of log output. Possible values are:

- `trace`: Show every detail.
- `debug`: Show detailed debug information.
- `info`: Show normal operational events.
- `warning`: Show exceptional occurrences that are not errors.
- `error`: Show runtime errors.
- `fatal`: Show fatal errors that make the app unusable.

The default and recommended level is `info`.

Retro Web logs use the Home Assistant timestamp format:

```text
[14:13:47] INFO: Retro Web listening on ingress port 8099
[14:13:47] WARNING: Indexing is already running
[14:13:47] ERROR: Indexing failed durationMs=123 errorFiles=1
```

The index worker reports start, ROM and BIOS phases, new, changed and deleted
files, cover lookup, completion, and failures. At `debug` or `trace` level it
also reports platform and per-cover progress.

### Option: `external_access`

Enables the separate external web listener on port `8098`. The listener remains
protected by `external_username` and `external_password`.

### Options: `external_username` and `external_password`

Credentials required by external users before selecting a profile. The app
refuses to start with `external_access` enabled if either value is empty.

### Option: `allow_external_rom_upload`

Enables administration from the external listener. It is disabled by default
and requires `external_admin_password`.

### Option: `external_admin_password`

Separate password used to elevate an authenticated external session into
administrator mode.

### Option: `session_timeout_hours`

Sets the external session lifetime in hours. Accepted range: `1` to `168`.

### Option: `max_upload_mb`

Maximum ROM, BIOS, or cover upload size in megabytes. Accepted range: `16` to
`16384`.

### Option: `profile_limit`

Maximum number of local player profiles. Accepted range: `1` to `20`.

### Option: `enable_arcade`

Enables uploading Arcade ROMs to FinalBurn Neo.

### Option: `rom_index_interval_minutes`

Interval in minutes for automatic ROM and BIOS folder scanning. Accepted range:
`1` to `1440`. The default is `60`.

### Option: `enable_cover_indexing`

Enables automatic cover lookup from Libretro DAT and Thumbnail sources.

## First start

1. Open **Retro Web** from the Home Assistant sidebar.
2. Create one or more profiles.
3. Open **Manage** and upload a ROM.
4. Select the matching platform.
5. Optionally upload or search for a cover.
6. Return to the library and select the game.

The app creates a default `Player 1` profile on first start.

## ROM upload

ROM upload is available inside Home Assistant Ingress. Supported formats:

| Platform         | Extensions     | Default core       |
| ---------------- | -------------- | ------------------ |
| NES              | `.nes`, `.fds` | `fceumm`           |
| SNES             | `.sfc`, `.smc` | `snes9x`           |
| Game Boy / Color | `.gb`, `.gbc`  | `gambatte`         |
| Game Boy Advance | `.gba`         | `mgba`             |
| Nintendo 64      | `.z64`, `.n64` | `mupen64plus_next` |
| Arcade           | `.zip`, `.7z`  | `fbneo`            |

ROM names are not used as filesystem paths. Retro Web identifies uploaded
content by SHA-256 hash and stores an editable display title separately.

Selecting a ROM generates the title from its filename. The generated title
remains editable before upload.

## Manual ROM and BIOS folders

Administrators can place files directly below `/data`:

```text
/data/roms/<platform>/<file>
/data/bios/<platform>/<file>
```

The platform folder name selects the EmulatorJS core mapping in
`app/platforms.js`. For example:

```text
/data/roms/n64/Zelda OoT.z64
/data/bios/arcade/neogeo.zip
```

Use **Reindexar ahora** in the administration screen or wait for the automatic
interval. The index stores SHA-1, CRC32, size, modification time, version, and
deletion state. Files are only hashed again when size or modification time
changes.

## Index endpoints

Administrative users can inspect or start indexing:

| Method | Route                     | Response                                                |
| ------ | ------------------------- | ------------------------------------------------------- |
| `GET`  | `/api/admin/index/status` | Running state and latest execution details              |
| `POST` | `/api/admin/index/run`    | `200` with the latest execution, or `409` while running |

In external mode these routes require an elevated external administration
session. In Ingress mode they are available to the local administrator.

## Progress endpoint

`POST /api/games/:gameId/progress` marks a game as recently played for the
active profile. It requires the active Ingress or external profile session and
returns `204` on success.

## Covers

The administrator can upload a PNG, JPEG, or WebP cover and preview it before
upload. Malformed, SVG, oversized, or excessive-dimension images are rejected.

Retro Web can also search public box art in Libretro Thumbnails. Searches run
only when requested, and the selected image is copied into `/data/covers`.
Future use is local and does not depend on the provider remaining online.

## Language

The interface supports Spanish and English. The initial language follows the
browser, and the active profile stores later selections.

## BIOS upload

BIOS files are managed separately from ROMs. Only upload BIOS files that you
are legally authorized to use.

Nintendo 64 games normally run without BIOS. An optional BIOS may support
features such as 64DD. Arcade ROM sets may require a matching BIOS bundle.

## Profiles and saves

Each native save and save state belongs to:

- Profile.
- Game.
- Emulator core.
- Save type.
- Slot.

Profiles cannot read or replace another profile's files. Writes are atomic and
the previous slot is retained as `.previous`.

Native saves come from the game's own save function and synchronize
automatically. Save states use the selected slot and **Save State** button.

## External access

To enable external access:

1. Set `external_access` to `true`.
2. Configure `external_username` and `external_password`.
3. Expose container port `8098`.
4. Put the port behind an HTTPS reverse proxy.

The external listener exposes gameplay and profiles only. Administrative
routes remain unavailable unless external administration is explicitly enabled.

## Backups

Back up the complete `/data` directory. It contains:

- `app.db`.
- ROMs and BIOS files.
- Cover images.
- Profiles.
- Native saves and save states.

Restore the directory while the app is stopped.

## Changelog & Releases

This repository keeps a change log using [GitHub's releases][releases]
functionality.

Releases are based on [Semantic Versioning][semver] and use `MAJOR.MINOR.PATCH`.

## Support

Got questions?

You could also [open an issue here][issue] on GitHub.

## Authors & contributors

The original setup of this repository is by [Jhon Coronel][jhoncb].

For a full list of all authors and contributors,
check [the contributor's page][contributors].

## License

MIT License

Copyright (c) 2026 Jhon Coronel

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[addon-badge]: https://my.home-assistant.io/badges/supervisor_addon.svg
[addon]: https://my.home-assistant.io/redirect/supervisor_addon/?addon=4dd71ed5_retro-web&repository_url=https%3A%2F%2Fgithub.com%2Fioncb-hassio-addons%2Frepository
[contributors]: https://github.com/ioncb-hassio-addons/app-retro-web/graphs/contributors
[issue]: https://github.com/ioncb-hassio-addons/app-retro-web/issues
[jhoncb]: https://github.com/jhoncb
[releases]: https://github.com/ioncb-hassio-addons/app-retro-web/releases
[semver]: http://semver.org/spec/v2.0.0.html
