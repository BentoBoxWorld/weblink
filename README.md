# weblink

Data repository for the [BentoBox](https://github.com/BentoBoxWorld) Minecraft server framework. Hosts JSON catalogs and libraries consumed by BentoBox plugins and addons:

- `catalog/` — registry of game modes and addons
- `challenges/` — challenge libraries for the Challenges addon
- `mcg/` — tier configurations for the Magic Cobblestone Generator
- `downloads/` — addon version compatibility matrix and third-party plugin registry

## Challenge libraries

### Skyblock (`challenges/library/skyblock.json`)

A modern challenge set for BSkyBlock built around vanilla 1.21+ content. 60 challenges spread across 7 ranks, designed so early rewards meaningfully unlock later challenges rather than asking players to grind in place.

**Ranks** (waiver amounts in parentheses — how many challenges per rank can be skipped):

1. **Castaway** (1) — first-night survival: bread, stew, cobble, glass, paper, a basic shelter
2. **Settler** (2) — farming, fishing, mob drops, animal husbandry; `Craftsman` at island level 50 awards the 10 obsidian needed to frame a nether portal
3. **Artisan** (2) — iron tools and armor, dyes, emeralds, bookshelves; `Enchanter` awards the enchanting table
4. **Voyager** (1) — nether access, brewing, villagers; `Alchemist` awards a Silk Touch book
5. **Architect** (2) — decoration, music discs, advanced potions; `Gem Collector` awards Mending
6. **Mythic** (1) — diamond and netherite tier, wither fight, oceans, archaeology, sniffer
7. **Legend** (0, all required) — elytra, full netherite, dragon egg, island level 5000

**Design principles**

- **Multiple paths, not one grind.** Each rank has more challenges than strictly required so players can pick routes that suit their playstyle (builder / farmer / combat / explorer).
- **Rewards are the delivery mechanism for otherwise unobtainable items.** Ores, unusual saplings (cherry, mangrove, dark oak, jungle, acacia), spawn eggs for rare animals (wolf, fox, parrot, axolotl, frog, sniffer), uncraftable decorative blocks (mycelium, ice, moss, sculk, calcite, deepslate), enchanted books, and modern items (spyglass, echo shards, recovery compass, pottery sherds, brushes, goat horn) are handed out as challenge rewards.
- **Reasonable quantities.** 21 bread, 64 cobble, 32 emeralds — not 256 wool or 164 iron blocks. Challenges should feel like achievements, not chores.
- **Hardcore skyblock assumption.** Starting resources are just a lava bucket, infinite water, one tree, seeds, mushrooms, sugar cane, milk, and a cobblestone generator. A verified path exists from those starting items all the way to Legend using only earned challenge rewards.
- **Some aspirational endgame goals.** A few late-rank challenges require rare drops (ghast tear, wither skeleton skulls, nautilus shell, heart of the sea, enchanted golden apple) as stretch goals — never as early progression gates.
- **Base for other game modes.** This library is intended as the template for future ports to AcidIsland, CaveBlock, and other game modes with different starting resources.

### Other libraries

- **`default.json`** — the original BSkyBlock challenge set (kept for backwards compatibility).
- **`askyblock-challenges.json`** — legacy challenges imported from the original ASkyBlock plugin.
- **`poseidon.json`** — ocean-themed challenges for the Poseidon game mode.

Translations for the default and ASkyBlock sets are available in `challenges/library/` (suffixes: `-ru`, `-de`, `-zh-CN`).

## Contributing

Edit JSON files directly and open a pull request. There is no build or automated validation — JSON syntax and gameplay balance are reviewed manually. See [`CLAUDE.md`](./CLAUDE.md) for format conventions and editing guidelines.
