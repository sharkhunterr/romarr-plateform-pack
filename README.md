# romarr-community-packs

Platform pack complet pour [Romarr](https://github.com/sharkhunterr/romarr) — mirror du builtin **plus** 6 plateformes additionnelles.

## Contenu

| Fichier | Version | Plateformes |
|---|---|---|
| `packs/platform-pack-community.yaml` | `2026.07.100` | 60 (54 builtin + 6 additions) |

### Ce qui est ajouté par rapport au builtin `2026.05.002`

**Gen 8-9 home consoles** (le builtin s'arrête à la gen 7-8) :

- `ps4` — PlayStation 4 · Sony · 2013
- `ps5` — PlayStation 5 · Sony · 2020
- `xbox-one` — Xbox One · Microsoft · 2013
- `xbox-series` — Xbox Series X/S · Microsoft · 2020

**Retro Japonais** (souvent absents des DAT packs occidentaux) :

- `x68000` — Sharp X68000 · 1987
- `pc98` — NEC PC-9800 Series · 1982

Chaque plateforme embarque les IDs IGDB, ScreenScraper et MobyGames pour que le scraper métadonnées les trouve automatiquement.

## Installation dans Romarr

**Settings → Platforms → Pack sources** → Add source :

- **Nom** : `Community pack`
- **URL** (dir) : `https://github.com/sharkhunterr/romarr-plateform-pack/tree/main/packs`
  
  ou **URL** (raw single-file) : `https://raw.githubusercontent.com/sharkhunterr/romarr-plateform-pack/main/packs/platform-pack-community.yaml`

Puis **Preview** → **Apply now**. Les 6 nouveaux slugs apparaissent en `+ inserted`, les 54 existants en `~ updated` ou `= skipped` selon les métadonnées.

## Auto-sync

Active le job `PackSourcesSync` dans **Settings → Tasks** (cron `0 5 * * *` par défaut). Les updates poussés sur ce repo landent le lendemain matin sans intervention.

## Bumper le pack

Pour publier un update :

1. Éditer `packs/platform-pack-community.yaml`
2. Incrémenter `pack_version` (ex : `2026.07.100` → `2026.08.100`, format `YYYY.MM.NNN`)
3. Commit + push

Romarr calcule un diff au prochain sync et n'applique que ce qui a changé. Même version + même hash → skip idempotent. Downgrades rejetés.

## Contribuer une plateforme

Ajouter une entry dans `platforms:` en respectant le schéma :

- **`slug`** : `^[a-z0-9]+(-[a-z0-9]+)*$` — ne doit pas déjà exister (voir le [builtin actuel](https://github.com/sharkhunterr/romarr/blob/main/romarr/src/romarr/builtin_packs/builtin-2026.05.002.yaml))
- **`name`**, **`manufacturer`** : non-vides
- **`formats[].extension`** : commence par `.`
- **`formats[].format_type`** : `cartridge` | `disc` | `compressed` | `archive` | `package`

Bump `pack_version` dans la même PR.

## Schéma complet

JSON Schema Draft 2020-12 : [`romarr/src/romarr/platform_packs/schema.py`](https://github.com/sharkhunterr/romarr/blob/main/romarr/src/romarr/platform_packs/schema.py).
