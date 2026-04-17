# Lookup Anything — SVE Extension

An override pack that teaches [Pathoschild's Lookup Anything](https://www.nexusmods.com/stardewvalley/mods/541)
about [Stardew Valley Expanded](https://www.nexusmods.com/stardewvalley/mods/3753) content.

## Why an "override pack" and not a real SMAPI mod?

Lookup Anything loads two files — `assets/data.json` and `i18n/default.json` — directly
from its **own** mod folder via `SMAPI.Helper.Data.ReadJsonFile`. Content Patcher cannot
target these files, and the only other extension hook is the C# `ILookupProvider` API
(which requires a compiled DLL).

So the mechanically correct way to add SVE metadata to Lookup Anything is to edit its
own data files. This pack ships drop-in replacements for those two files.

## What this pack adds

- **Location labels** — SVE locations (Crimson Badlands, Highlands, Sprite Spring,
  Fable Reef, Shearwater Bridge, Forest West, Blue Moon Vineyard, Aurora Vineyard,
  Grampleton Fields, Junimo Woods, Adventurer Summit, Castle Village, Grenville Falls,
  etc.) now show friendly names when a lookup mentions them (e.g. in fish spawn info).
- **Character descriptions** — the non-villager NPCs SVE adds (Apples and Peaches
  the pet dragons, Dusty the dog, Charlie the chicken, the Highlands Dwarf, the
  Henchman, and the generic Adventurers) get short descriptions, matching how
  vanilla handles Pet / Horse / Junimo / Trash Bear.
- **Shops** — category buy-lists for SVE shops: Sophia (Blue Moon Vineyard wine
  stand), Martin (Stardrop Saloon bakery), Isaac & Camilla (Fairhaven Farm produce),
  Alesia (First Slash Adventurer's Guild), Gunther's expedition shop, the Joja
  Emporium, and the Go-Goat Ranch.
- **Ignored "fake" fishing locations** — extra entries for SVE scratch maps
  (`Custom_Void`, `GaldoranVoid`, warp rooms) so Lookup Anything doesn't try to
  show fish spawn info for them.

This pack does **not** add per-fish spawn rules for SVE fish, because SVE assigns
those via its C# code rather than through `Data/Fish`, and the specific object IDs
change between SVE versions. Lookup Anything will still show SVE fish data that SVE
puts into `Data/Fish` / `Data/Locations` automatically.

## Install

1. Back up `Mods/LookupAnything/assets/data.json` and `Mods/LookupAnything/i18n/default.json`.
2. Copy the two files from this pack into the Lookup Anything mod folder,
   overwriting the originals:

   ```
   LookupAnything SVE Extension/assets/data.json   →   Mods/LookupAnything/assets/data.json
   LookupAnything SVE Extension/i18n/default.json  →   Mods/LookupAnything/i18n/default.json
   ```
3. Launch Stardew Valley. You can sanity-check by opening the in-game console and
   running `patch summary` — Lookup Anything's version is unchanged, but the next
   time you press F1 over an SVE NPC / shop / location the new strings appear.

## When Lookup Anything updates

If Pathoschild releases a new version of Lookup Anything, let the new file overwrite
this pack's version, then re-diff. The SVE additions in this pack are all isolated:

- In `assets/data.json`, additions sit inside `Shops`, `Characters`, and
  `IgnoreFishingLocations`, each marked with a `// SVE:` comment.
- In `i18n/default.json`, additions are grouped under a single
  `/* SVE locations / shops / NPCs */` banner near the end of the file.

The reference-only JSON in `additions-only/` contains just the SVE additions in
the same shape as the full file, so you can re-merge by copy-paste into a fresh
Lookup Anything release if you prefer.

## Not a SMAPI mod

The `manifest.json` in this folder is for the mod-pack catalog only. SMAPI will
log `no .dll file found` if it tries to load this folder — the folder is fine to
keep in `Mods/`, or you can move it out once you've copied the files into Lookup
Anything. To silence the SMAPI warning, delete `manifest.json` or move this whole
folder out of `Mods/`.

## Credits

- Pathoschild — *Lookup Anything*
- FlashShifter & the SVE team — *Stardew Valley Expanded*
