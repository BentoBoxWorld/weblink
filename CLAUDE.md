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

## Conventions

- **Multilingual files**: English uses `filename.json`, translations use `filename-LANG.json` (e.g., `default-ru.json`, `askyblock-challenges-zh-CN.json`). Language codes: en, ru, de, zh-CN, fr, lv.
- **Tags/topics**: Use ISO language codes as object keys for multilingual support.
- **Slot numbers**: Used in catalog entries for UI positioning in BentoBox interfaces.
- **Version format**: Semantic versioning (Major.Minor.Patch) in `downloads/config.json`.

## Editing Guidelines

- Validate JSON syntax before committing — there is no automated linting or CI.
- Challenge files use specific Bukkit item serialization format; preserve the existing structure when editing item stacks.
- Block chance values in MCG files are percentage-based probabilities that should be internally consistent within a tier.
