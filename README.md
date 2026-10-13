# Simple Challenges

[English](README.md) | [Deutsch](README.de.md)

A Minecraft challenge plugin for **Paper 26.3**, made to play with friends.

Inspired by **BastiGHG**'s videos, I wanted to play Minecraft challenges with my own friends. I built this plugin for those sessions and am sharing it so others can enjoy them too.

## Challenges

| Mode | How it works |
| --- | --- |
| **Random Items · Solo** | Collect random items before time runs out. Most points wins. |
| **Random Items · Duo** | Teams of up to two share targets, points, skips and a backpack. |
| **Achievement Challenge** | Complete as many Minecraft advancements as possible within the time limit. Recipe unlocks do not count. |
| **Duo Bingo** | Teams share a 20-task card. Complete the configured number of tasks first; a row is not required. You can join a team alone. |
| **Skyblock Bingo** | Start on your own island and complete a full row, column or diagonal on the shared Bingo card. |
| **Mob Battle · Solo - New in 1.1.0** | Hunt random mob targets. Kills and skips earn points; most points wins. |
| **Mob Battle · Duo - New in 1.1.0** | Teams share targets, points, skips and a backpack. Friendly fire is enabled. |
| **XP Battle - New in 1.1.0** | Reach the highest XP level before time runs out. Deaths and spending levels do not reduce your record. |
| **Chunkbound · Co-op - New in 1.1.0** | Unlock neighbouring chunks for 1, 2, 3 … levels and defeat the Ender Dragon together. The Nether also uses unlockable chunks; portal arrival chunks are free and the End is normal. |

Random Items, Mob Battle and Achievements default to **two hours**. XP Battle has a configurable time limit. Bingo and Chunkbound have no time limit.

All modes use separate worlds and support pause/resume across server restarts. Your lobby inventory and player state are restored when you return. All original participants must be online to resume.

While a round prepares, a bossbar shows the current step, waiting time and progress where available. Random Items and Mob Battle also show player direction markers above the hotbar; disable them with `locator-bar: false` under the relevant mode.

## Getting started

1. Use **Java 25** and a **Paper 26.3** server.
2. Put `SimpleChallenges-1.1.0.jar` in `plugins/` and restart the server.
3. Run `/challenge` to choose a mode. Starting and managing rounds requires OP or `simplechallenges.challenge.admin`.

The menu starts with **Competitive** and **Cooperative**. Competitive contains **Challenges** (Random Items, Mob Battle, Achievements and XP Battle) and **Bingos** (Duo Bingo and Skyblock Bingo). Cooperative contains **Chunkbound**. Solo and Duo variants stay together; use **Back** to return to the previous menu.

When upgrading, replace the old JAR and keep its data folder for automatic import. Custom permissions now use `simplechallenges.*`.

| Command | Purpose |
| --- | --- |
| `/challenge` | Open the mode selection menu. |
| `/challenge start <mode> [seed]` | Start `randomitems`, `randomitemsduo`, `mobbattle`, `mobbattleduo`, `achievements`, `duobingo`, `skyblock`, `xpbattle` or `chunkbound`. |
| `/challenge pause` / `/challenge resume` | Save or continue the current round. |
| `/challenge stop` | End the active round and return to the lobby. |
| `/challenge discard` | Discard a paused round. |
| `/bp` | Open your Random Items, Mob Battle or Duo Bingo backpack. |
| `/bingo` | Open your Duo Bingo or Skyblock card. |
| `/result` | Reveal the next result after a round. |
| `/result overview [player]` | View an individual result; Duo Bingo uses the team color instead. |
| `/skyblock reset` | Restore your own central island chunk and starter chest, once every three minutes of active play. Inventory and Bingo progress are kept. |

For Bingo and Skyblock, another `/result` after the final card ends the result phase and returns everyone to the lobby.

## World seeds · New in 1.1.0

Add a number or text after the mode name to replay or share the same world:

```text
/challenge start randomitems 123456789
/challenge start chunkbound Our shared world
```

Text always converts to the same numeric seed; case and spaces matter. Leave out the seed for a random world. Targets and Bingo tasks stay random, and Skyblock keeps its island setup. Use the same Minecraft version and world settings for matching terrain. The menu starts random worlds; resumed rounds keep their existing world.

## Configuration

Settings live in **`plugins/SimpleChallenges/`**. Edit the YAML files, restart the server and start a new round to use updated task pools.

### General settings — `config.yml`

| Section | What you can change |
| --- | --- |
| `challenge` | Default countdown before play begins. |
| `world` | Lobby world and chunk preload radius. |
| `players` | Clear inventories at the start; lobby inventories are saved. |
| `modes.achievements` | Countdown and round duration. |
| `modes.randomitems` / `modes.randomitemsduo` | Countdown, duration, skip count, token material and name. |
| `modes.mobbattle` / `modes.mobbattleduo` | Countdown, duration and skip tokens for Mob Battle Solo and Duo. |
| `modes.xpbattle` | Countdown and round duration. |
| `modes.chunkbound` | PvP, starting distance from the stronghold and chunk-unlock speed. |
| `modes.duobingo` | Countdown, PvP, team spawn spacing, and tasks, win condition and card opener under `bingo`. |
| `modes.skyblock` | Countdown, PvP, island spacing, starter chest, fishing loot, and card, tasks and opener under `bingo`. |

Both Bingos always start with fresh inventories. Shared options and automatic migration of old configs: [Bingo configuration (German)](../docs/BINGO_CONFIG.md).

Times are in **seconds**: `7200` means two hours.

### Add or edit Bingo tasks

Add, remove or edit entries in the task lists. Customize requirements, `title` and `icon`. Use uppercase materials/entities and advancement keys such as `minecraft:story/mine_diamond`.

**Duo Bingo:** add these under `modes.duobingo.bingo.tasks`:

```yaml
- type: OBTAIN
  item: DIAMOND
  amount: 8
  title: "Find eight diamonds"
  icon: DIAMOND

- type: KILL
  mob: ZOMBIE
  count: 10
  icon: IRON_SWORD
```

Task types: `OBTAIN`, `CRAFT`, `KILL`, `BIOME`, `STRUCTURE_FIND`, `ADVANCEMENT`, `VILLAGER_TRADE`, `BLOCK_PLACE`, `EXPERIENCE`, `DISTANCE_TRAVEL`, `FOOD_EAT`, `ENCHANT_ITEM`, `POTION_BREW`, `ANIMAL_BREED`, `SLEEP_BED`.

Keep enough tasks for a **20-task card**, with at most four biomes. `bingo.win-count` sets the number needed to win.

**Skyblock:** add entries under `modes.skyblock.bingo.tasks`:

```yaml
- type: INVENTORY_HAS
  item: COBBLESTONE
  amount: 64
  title: "Collect a stack of cobblestone"
  icon: COBBLESTONE

- type: FISH
  amount: 5
  icon: FISHING_ROD
```

Task types: `INVENTORY_HAS`, `CRAFT_ITEM`, `SMELT_ITEM`, `PLACE_BLOCK`, `KILL_ENTITY`, `KILL_MOBS`, `ADVANCEMENT`, `COBBLE_GEN`, `REACH_HEIGHT`, `BREED_ANIMAL`, `FISH`, `BUILD_GOLEM`, `HARVEST_CROPS`, `GROW_TREES`, `TRADE_COUNT`, `GRASS_SPREAD`, `SHEAR`, `EAT_ITEM`, `SIGN_NAME`, `FALL_SURVIVE`, `ZOMBIE_VILLAGER_CATCH`, `COMPOST_PRODUCE`, `HIT_PLAYER_ARROW`.

Set `bingo.board-size` for the card. A winning line always spans the full board size. With `strict-no-duplicates: true`, a 5×5 card needs **25 eligible tasks**. Use `min-players: 2` for multiplayer-only tasks.

Under `modes.skyblock.generator.starter-chest.items`, use entries such as `LAVA_BUCKET:1` or `ICE:2`. Fishing drops use `item`, `min`, `max` and a relative `weight`; `keep-vanilla-percent` controls how often the normal catch is kept. The `skyblock_template.json` file defines the island used for new rounds and island resets.

### Random Items filters — `blacklist-config.yml`

The server supplies the **current item catalog** automatically. Both Random Items modes apply these filters:

| Section | How it filters items |
| --- | --- |
| `material-blacklist.items` | Excludes exact material names, such as `NETHER_STAR`. |
| `name-filters.patterns` | Excludes names containing a substring, such as `_SPAWN_EGG`. These are not wildcard patterns. |
| `colored-variants.categories` | Excludes colored versions of listed categories, such as `WOOL`, using `color-prefixes`. |
| `options.debug-export` | Generates CSV reference lists when the Random Items pool loads. |
| `options.warn-unknown-materials` | Logs blacklist entries that do not exist in the running version. |

Each filter group has an `enabled` switch. Names and patterns are case-insensitive. To allow an item, remove **all** matching exclusions. Patterns match substrings: `LIGHT` also excludes `LIGHT_BLUE_WOOL`.

With CSV export enabled, the plugin writes:

- **`randomitem_pool.csv`** — allowed targets, with `name`, `isBlock` and `isEdible` columns.
- **`randomitem_all_items.csv`** — the current item catalog, with an exclusion reason or an empty reason for allowed items.

Edit **`blacklist-config.yml`**, not the CSVs. Exports are regenerated when a Random Items mode loads. The skip-token material is always excluded from targets.

### Mob Battle filters — `mob-blacklist-config.yml`

The current server entity catalog supplies mob targets automatically. Use `entity-blacklist.entities` for exact exclusions and `name-filters.patterns` for case-insensitive substring filters. Each group has an `enabled` switch. `PLAYER` can be a target even when playing alone. Mob Battle Duo enables friendly fire. Its icon is `PLAYER_HEAD`; add `PLAYER` to the blacklist to exclude it. Other non-mob entities never become targets.

With `options.debug-export: true`, new rounds generate `mobbattle_pool.csv` (allowed mobs and icons) and `mobbattle_all_entities.csv` (all entity types and exclusion reasons). Edit the YAML, not the CSVs. Changes apply to new rounds; resumed rounds retain their original pool.

Targets use their spawn eggs. For mobs without an egg, `icons` accepts a unique alternative material. Difficult mobs are excluded by default and can be allowed by editing the filters.
