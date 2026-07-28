<div align="center">

# romarr-community-packs

**A complete platform pack for [Romarr](https://github.com/sharkhunterr/romarr)** — a mirror of the builtin pack, plus 6 additional platforms.

</div>

---

## Overview

| File | Version | Platforms |
|------|---------|-----------|
| `packs/platform-pack-community.yaml` | `2026.07.201` | 60 (54 builtin + 6 additions) |

Every platform ships with its IGDB, ScreenScraper, and MobyGames IDs so the metadata scraper can match them automatically. Every platform also accepts the universal archive extensions `.zip`, `.7z`, and `.rar`, since ROMs are almost always distributed archived.

## What's added over builtin `2026.05.002`

**Gen 8–9 home consoles** — the builtin stops at gen 7–8:

| Slug | Platform | Manufacturer | Year |
|------|----------|--------------|------|
| `ps4` | PlayStation 4 | Sony | 2013 |
| `ps5` | PlayStation 5 | Sony | 2020 |
| `xbox-one` | Xbox One | Microsoft | 2013 |
| `xbox-series` | Xbox Series X/S | Microsoft | 2020 |

**Japanese retro** — often missing from Western DAT packs:

| Slug | Platform | Manufacturer | Year |
|------|----------|--------------|------|
| `x68000` | Sharp X68000 | Sharp | 1987 |
| `pc98` | NEC PC-9800 Series | NEC | 1982 |

## Installation

In Romarr, go to **Settings → Platforms → Pack sources → Add source**:

- **Name:** `Community pack`
- **URL (directory):**
  ```
  https://github.com/sharkhunterr/romarr-plateform-pack/tree/main/packs
  ```
- **URL (raw, single file):**
  ```
  https://raw.githubusercontent.com/sharkhunterr/romarr-plateform-pack/main/packs/platform-pack-community.yaml
  ```

Then click **Preview → Apply now**. The 6 new slugs appear as `+ inserted`, and the 54 existing ones as `~ updated` or `= skipped` depending on their metadata.

## Auto-sync

Enable the `PackSourcesSync` job under **Settings → Tasks** (default cron `0 5 * * *`). Updates pushed to this repository are applied automatically the next morning — no manual action required.

## Publishing an update

1. Edit `packs/platform-pack-community.yaml`.
2. Bump `pack_version` (e.g. `2026.07.100` → `2026.08.100`, format `YYYY.MM.NNN`).
3. Commit and push.

On the next sync, Romarr computes a diff and applies only what changed. Same version and same hash results in an idempotent skip. Downgrades are rejected.

## Contributing a platform

Add an entry under `platforms:` that follows the schema:

| Field | Rule |
|-------|------|
| `slug` | Must match `^[a-z0-9]+(-[a-z0-9]+)*$` and not already exist ([current builtin](https://github.com/sharkhunterr/romarr/blob/main/romarr/src/romarr/builtin_packs/builtin-2026.05.002.yaml)) |
| `name`, `manufacturer` | Non-empty strings |
| `formats[].extension` | Must start with `.` |
| `formats[].format_type` | One of `cartridge`, `disc`, `compressed`, `archive`, `package` |

Bump `pack_version` in the same pull request.

## Schema

Full JSON Schema (Draft 2020-12): [`romarr/src/romarr/platform_packs/schema.py`](https://github.com/sharkhunterr/romarr/blob/main/romarr/src/romarr/platform_packs/schema.py).
