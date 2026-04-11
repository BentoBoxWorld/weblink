# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**weblink** is a JSON-based configuration and data repository for the [BentoBox](https://github.com/BentoBoxWorld) Minecraft server framework ecosystem. It serves as a central registry consumed programmatically by BentoBox plugins and web interfaces. There is no build process, test framework, or runtime — all content is static JSON.

## Repository Structure

The repository follows a **catalog index + library** pattern: each category has a `catalog.json` index and a `library/` directory with the actual data files.

- **`catalog/`** — Main registry of game modes (BSkyBlock, AcidIsland, AOneBlock, SkyGrid, CaveBlock, Boxed) and addons (Level, Challenges, Warps, Biomes, etc.). Includes multilingual `tags.json` and `topics.json`.
- **`challenges/`** — Challenge definitions for various game modes. Each challenge has an ID, type (INVENTORY, ISLAND_TYPE, OTHER_TYPE), requirements, rewards, and repeatability settings. Contains serialized Bukkit ItemStack objects (YAML embedded in JSON strings).
- **`mcg/`** — Magic Cobblestone Generator tier configurations with block spawn probabilities, treasure drop rates, and unlock requirements.
- **`downloads/`** — `config.json` maps addon version compatibility across Minecraft versions. `thirdparty.json` is a registry of community plugins.

## Consumption model

Files are fetched by BentoBox plugins at stable paths (raw GitHub URLs). **Renaming or moving a file is a breaking change** for every server that references it — treat paths as a public API. Add new files alongside existing ones rather than restructuring.

## Conventions

- **Multilingual files**: English uses `filename.json`, translations use `filename-LANG.json` (e.g., `default-ru.json`, `askyblock-challenges-zh-CN.json`). Language codes: en, ru, de, zh-CN, fr, lv.
- **Translation parity**: translated challenge/library files must keep the same keys, IDs, and structure as the English source — only display strings change. Item serialization, requirements, and rewards stay identical.
- **Tags/topics**: Use ISO language codes as object keys for multilingual support.
- **Slot numbers**: Used in catalog entries for UI positioning in BentoBox inventory GUIs. Slots must be unique within a given menu, and should avoid the border slots used by the GUI frame.
- **Version format**: Semantic versioning (Major.Minor.Patch) in `downloads/config.json`.

## Editing Guidelines

- Validate JSON syntax before committing — there is no automated linting or CI.
- Challenge files use specific Bukkit item serialization format (YAML embedded in JSON strings); preserve the existing structure when editing item stacks.
- Block chance values in MCG files are percentage-based probabilities that should be internally consistent within a tier.

## Skyblock challenge library (`challenges/library/skyblock.json`)

This is the flagship modern challenge set and the template for future ports to other game modes. When editing or extending it, preserve the design principles documented in `README.md`:

- **Hardcore starting assumption**: a verified path must exist from the minimal starting kit (lava, water, one tree, seeds, mushrooms, sugar cane, milk, cobble generator) through to Legend rank using only earned rewards. Don't introduce challenges that presume items the player has no way to obtain yet.
- **Rewards deliver otherwise-unobtainable content**: ores, rare saplings, spawn eggs for uncraftable mobs, decorative blocks, enchanted books, modern items. This is the *point* of the rewards — reducing a reward to something the player can already farm breaks the progression.
- **Reasonable quantities**: challenges should feel like achievements, not grinds. Prefer small meaningful amounts over stack-count busywork.
- **Rank waivers**: each rank has a waiver count (how many challenges can be skipped); Legend has 0 and requires all. When adding challenges, keep each rank with more challenges than strictly required so players have route choice.

## AcidIsland challenge library (`challenges/library/acidisland.json`)

Companion to the Skyblock library, ported to the AcidIsland game mode. Same design principles apply (hardcore starter path, otherwise-unobtainable rewards, reasonable quantities, route-choice waivers), with the following AcidIsland-specific adjustments:

- **Starter kit is different from Skyblock**: AcidIsland players start with chest contents that include a lava bucket, iron pickaxe, leather armor, water breathing + night vision potions, bread, melon, pumpkin, sugar cane, an acacia sapling, and a fishing rod (temple variant only). Reference schematics live at `~/tmp/island.json` and `~/tmp/temple.json`. The main `island.json` lacks a fishing rod, mushrooms, and seeds — rank-1 challenges must work without those, or the rank-1 reward chain must hand them out.
- **The sea and rain are acidic**: any challenge that involves swimming, diving, or extended water contact is dangerous without water breathing. Sprinkle `water_breathing` (rank 1–2) and `long_water_breathing` (rank 3+) potions through dive/swim/fish challenges as rewards so players can keep venturing deeper. Splash variants are appropriate for combat-in-water challenges (e.g. guardians).
- **Ranks**: 7 nautical ranks — Shipwrecked → Survivor → Mariner → Navigator → Captain → Leviathan → Admiral. Admiral has waiverAmount 0 (must complete all). Keep each rank with more challenges than the strict minimum after waivers.
- **Captain's Log lore series**: each rank-completion reward includes one volume of a 7-book "Captain's Log" written-book series. The arc deliberately leaves the nature of the acid sea ambiguous (pollution? future Earth? past Earth? a prison seal around something older?). When adding or rewriting volumes, raise questions rather than answer them, and let later volumes contradict earlier guesses.
- **Reward diversity**: avoid reusing the same "redstone + iron ore + X" reward template across multiple challenges in a rank — each challenge should have a distinctive themed reward (enchanted books, spawn eggs, rare saplings, decorative blocks, etc.).

## Dependency Source Lookup

When you need to inspect source code for a dependency (e.g., BentoBox, addons):

1. **Check local Maven repo first**: `~/.m2/repository/` — sources jars are named `*-sources.jar`
2. **Check the workspace**: Look for sibling directories or Git submodules that may contain the dependency as a local project (e.g., `../bentoBox`, `../addon-*`)
3. **Check Maven local cache for already-extracted sources** before downloading anything
4. Only download a jar or fetch from the internet if the above steps yield nothing useful

Prefer reading `.java` source files directly from a local Git clone over decompiling or extracting a jar.

In general, the latest version of BentoBox should be targeted.

## Project Layout

Related projects are checked out as siblings under `~/git/`:

**Core:**
- `bentobox/` — core BentoBox framework

**Game modes:**
- `addon-acidisland/` — AcidIsland game mode
- `addon-bskyblock/` — BSkyBlock game mode
- `Boxed/` — Boxed game mode (expandable box area)
- `CaveBlock/` — CaveBlock game mode
- `OneBlock/` — AOneBlock game mode
- `SkyGrid/` — SkyGrid game mode
- `RaftMode/` — Raft survival game mode
- `StrangerRealms/` — StrangerRealms game mode
- `Brix/` — plot game mode
- `parkour/` — Parkour game mode
- `poseidon/` — Poseidon game mode
- `gg/` — gg game mode

**Addons:**
- `addon-level/` — island level calculation
- `addon-challenges/` — challenges system
- `addon-welcomewarpsigns/` — warp signs
- `addon-limits/` — block/entity limits
- `addon-invSwitcher/` / `invSwitcher/` — inventory switcher
- `addon-biomes/` / `Biomes/` — biomes management
- `Bank/` — island bank
- `Border/` — world border for islands
- `Chat/` — island chat
- `CheckMeOut/` — island submission/voting
- `ControlPanel/` — game mode control panel
- `Converter/` — ASkyBlock to BSkyBlock converter
- `DimensionalTrees/` — dimension-specific trees
- `discordwebhook/` — Discord integration
- `Downloads/` — BentoBox downloads site
- `DragonFights/` — per-island ender dragon fights
- `ExtraMobs/` — additional mob spawning rules
- `FarmersDance/` — twerking crop growth
- `GravityFlux/` — gravity addon
- `Greenhouses-addon/` — greenhouse biomes
- `IslandFly/` — island flight permission
- `IslandRankup/` — island rankup system
- `Likes/` — island likes/dislikes
- `Limits/` — block/entity limits
- `lost-sheep/` — lost sheep adventure
- `MagicCobblestoneGenerator/` — custom cobblestone generator
- `PortalStart/` — portal-based island start
- `pp/` — pp addon
- `Regionerator/` — region management
- `Residence/` — residence addon
- `TopBlock/` — top ten for OneBlock
- `TwerkingForTrees/` — twerking tree growth
- `Upgrades/` — island upgrades (Vault)
- `Visit/` — island visiting
- `weblink/` — web link addon
- `CrowdBound/` — CrowdBound addon

**Data packs:**
- `BoxedDataPack/` — advancement datapack for Boxed

**Documentation & tools:**
- `docs/` — main documentation site
- `docs-chinese/` — Chinese documentation
- `docs-french/` — French documentation
- `BentoBoxWorld.github.io/` — GitHub Pages site
- `website/` — website
- `translation-tool/` — translation tool

Check these for source before any network fetch.

## Key Dependencies (source locations)

- `world.bentobox:bentobox` → `~/git/bentobox/src/`
