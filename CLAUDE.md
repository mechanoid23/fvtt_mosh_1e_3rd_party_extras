# CLAUDE.md — fvtt_mosh_1e_3rd_party_extras

Module ID: `fvtt_mosh_1e_3rd_party_extras`

## Format

NeDB compendium packs — one JSON document per line in `.db` files under `packs/`. Edit directly as text. No scripts; this module is data-only.

## Pack contents

| Pack file | Type | Label | Content |
|---|---|---|---|
| `journals_gear_spacefarer.db` | JournalEntry | Gear For A Spacefarer | 8-page journal: intro, entry requirements, and one table page per item category |
| `rolltables_gear_spacefarer.db` | RollTable | Gear For A Spacefarer - Contracts | 1d10 contract table (Cadwal Trade Depot) |
| `items_gear_spacefarer.db` | Item | Gear For A Spacefarer - Items | 26 utility/leisure/medical/drug/electronic/misc items |
| `rolltables_disciples_cracked_skull.db` | RollTable | Disciples of a Cracked Skull | Wounds - Chemical Weapons (1d10-1, results 0–9) |
| `items_disciples_cracked_skull.db` | Item | Disciples of a Cracked Skull | 12 weapons with custom images |
| `journals_disciples_cracked_skull.db` | JournalEntry | Disciples of a Cracked Skull | Weapon reference table (Name/Cost/Range/Damage/Shots/Critical/Special) |

## ID conventions

All NeDB `_id` values are exactly **16 characters**, alphanumeric (`[a-zA-Z0-9]`). IDs must be unique within each `.db` file.

Prefixes in use:

| Prefix | Used for |
|---|---|
| `GearSpacefarer` | Gear For A Spacefarer journal entry / pages |
| `GearSpacePage` | Gear For A Spacefarer journal pages |
| `GfsContracts` | Gear For A Spacefarer contracts rolltable |
| `GfsContract` | Individual contract rolltable results |
| `GfsPageUtils`, `GfsPageLeisure`, etc. | Gear For A Spacefarer item-category journal pages |
| `GfsAirlock`, `GfsMolotov`, `Gfs…` | Gear For A Spacefarer item entries |
| `GfsChemWndsTbl` | Disciples Chemical Wounds rolltable |
| `GfsChemWR` | Individual Chemical Wounds results |
| `DcsJournal`, `DcsJournalPage` | Disciples journal entry / page |
| `Dcs…` | Disciples weapon items |

Always verify ID length with a Python assertion before writing: `assert len(id_) == 16`.

## Images

Weapon images for Disciples of a Cracked Skull live in `images/` as `weapon_<kebab-name>.png`. The `img` field in each item document uses the path `modules/fvtt_mosh_1e_3rd_party_extras/images/<filename>`.

## Rolltable formulas

| Formula | Range | Use case |
|---|---|---|
| `1d10` | 1–10 | Standard ten-item tables |
| `1d10-1` | 0–9 | Zero-indexed tables (e.g. wound severity starting at 0) |

## Release process

1. Make all content changes to `.db` files and `module.json`
2. Bump `version` and `download` URL in `module.json`
3. Commit and push
4. Build zip from the updated source: `cd /tmp && cp -r <module-path> . && zip -r <module>.zip <module> -x "*.git*"`
5. Verify zip contains correct version: `unzip -p <zip> <module>/module.json | python3 -c "import sys,json; print(json.load(sys.stdin)['version'])"`
6. Create GitHub release: `gh release create v<version> <zip> --title "..." --notes "..."`
