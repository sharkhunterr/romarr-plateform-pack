# romarr-plateform-pack

Test platform packs pour valider l'intégration `Pack Sources` de [Romarr](https://github.com/sharkhunterr/romarr).

Ce dépôt public sert de source de packs de plateformes : il permet de récupérer (« get ») des plateformes de test directement dans Romarr via le mécanisme des **Pack sources**.

## Contenu

| Fichier | pack_version | Plateforme testée |
|---|---|---|
| `packs/test-arcade.yaml` | `2026.07.100` | `test-arcade-cabinet` (cabinet fictif, gen 3) |
| `packs/test-handheld.yaml` | `2026.07.101` | `test-pocket-console` (handheld fictif, gen 4) |

Les slugs sont préfixés `test-*` pour garantir aucune collision avec les packs builtin.

## Comment tester dans Romarr

Ouvre **Settings → Platforms → Pack sources** puis ajoute une source.

### Test A — dossier complet (walker GitHub API)

- **Nom** : `Test packs (dir)`
- **URL** : `https://github.com/sharkhunterr/romarr-plateform-pack/tree/main/packs`
- Auto-détecté comme `github_dir` → walk tous les `*.yaml` du dossier

Attendu au **Preview** :
- 2 YAMLs listés
- Chacun avec action `would_apply` (fresh slugs)
- Diff par plateforme : `+ test-arcade-cabinet`, `+ test-pocket-console`

### Test B — fichier unique (raw)

- **Nom** : `Test arcade only`
- **URL** : `https://raw.githubusercontent.com/sharkhunterr/romarr-plateform-pack/main/packs/test-arcade.yaml`
- Auto-détecté comme `raw`

Attendu au **Preview** :
- 1 YAML listé, `would_apply`

### Test C — idempotence

Après un premier `Sync now` réussi, re-clique **Preview** → chaque YAML doit apparaître avec action `would_skip` (même hash, déjà en DB).

### Test D — auto-sync programmé

**Settings → Tasks** → active `PackSourcesSync`, ajuste le cron (ex : `*/5 * * * *` pour toutes les 5 min) → la row `pack_sources` dans **Settings → Platforms** doit se re-stamper avec un nouveau `last_synced_at`.

## Format du pack

Voir le schéma dans `romarr/src/romarr/platform_packs/schema.py` (Draft 2020-12). Champs obligatoires :

- `pack_version` : format `YYYY.MM.NNN` (ex `2026.07.100`)
- `schema_version` : `1`
- `platforms[].slug` : `kebab-case`
- `platforms[].name`, `manufacturer` : chaînes non-vides
- `platforms[].formats[].extension` : commence par `.`
- `platforms[].formats[].format_type` : `cartridge` | `disc` | `compressed` | `archive` | `package`

## Nettoyage

Pour retirer les plateformes tests de Romarr, va dans **Settings → Platforms**, sélectionne les slugs `test-*` et override-les manuellement (le pack builtin ne les recréera pas).
