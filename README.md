# Simple Challenges

[English](README.md) | [Deutsch](README.de.md)

A Minecraft challenge plugin for **Paper 26.3**, made to play with friends.

Inspired by **BastiGHG**'s videos, I wanted to play Minecraft challenges with my own friends. I built this plugin for those sessions and am sharing it so others can enjoy them too.

## Challenges

| Mode | How it works |
| --- | --- |
| **Random Items · Solo** | Collect randomly assigned items before time runs out. Complete targets to score points and receive a new target. Most points wins. |
| **Random Items · Duo** | The same challenge in teams of up to two. Teammates share targets, points, skip tokens and a backpack. |
| **Achievement Challenge** | Complete as many Minecraft advancements as possible within the time limit. Recipe unlocks do not count. |
| **Duo Bingo** | Teams share a 20-task card and compete to finish the configured number of tasks first. You can also enter a team alone. A completed row is not required. |
| **Skyblock Bingo** | Each player starts on their own island in a void world. Complete a consecutive row, column or diagonal on a shared Bingo card to win. |

Random Items and Achievements default to **two hours**. Duo Bingo and Skyblock have no time limit; their timer counts up until someone wins.

Challenges use separate temporary worlds. You can pause and resume across server restarts, and your lobby inventory and player state are restored when you return. Resuming requires all original participants.

## Getting started

1. Use **Java 25** and a **Paper 26.3** server.
2. Put `SimpleChallenges-1.0-SNAPSHOT.jar` in `plugins/` and restart the server.
3. Run `/challenge` to choose a mode. Starting and managing rounds requires OP or `simplechallenges.challenge.admin`.

When upgrading, replace the old JAR and keep its data folder for automatic import. Custom permissions now use `simplechallenges.*`.

| Command | Purpose |
| --- | --- |
| `/challenge` | Open the mode selection menu. |
| `/challenge start <mode>` | Start `randomitems`, `randomitemsduo`, `achievements`, `duobingo` or `skyblock`. |
| `/challenge pause` / `/challenge resume` | Save or continue the current round. |
| `/challenge stop` | End the active round and return to the lobby. |
| `/challenge discard` | Discard a paused round. |
| `/bp` | Open your Random Items or Duo Bingo backpack. |
| `/bingo` | Open your Duo Bingo or Skyblock card. |
| `/result` | Reveal the next result after a round. |
| `/result overview [player]` | View an individual result; Duo Bingo uses the team color instead. |
| `/skyblock reset` | Restore your own central island chunk and starter chest, once every three minutes of active play. Inventory and Bingo progress are kept. |

For Bingo and Skyblock, another `/result` after the final card ends the result phase and returns everyone to the lobby.

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
| `modes.duobingo` | Countdown before the Bingo round. |
| `duo_bingo` | Task pool, winning task count, card shuffling, PvP and distance between team spawns. |
| `skyblock` | Island spacing, inventory clearing, starter chest contents, fishing loot and Bingo settings. |

Times are in **seconds**: `7200` means two hours.

### Add or edit Bingo tasks

Add, remove or edit entries in the task lists. Customize requirements, `title` and `icon`. Use uppercase materials/entities and advancement keys such as `minecraft:story/mine_diamond`.

**Duo Bingo:** add these under `duo_bingo.tasks`:

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

Keep enough tasks for a **20-task card**, with at most four biomes. `win_count` sets the number needed to win.

**Skyblock:** add entries under `skyblock.bingo.tasks`:

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

Set `board-size` and `in-row-to-win` for the card and winning line. With `strict-no-duplicates: true`, a 5×5 card needs **25 eligible tasks**. Use `min-players: 2` for multiplayer-only tasks.

Under `skyblock.generator.starter-chest.items`, use entries such as `LAVA_BUCKET:1` or `ICE:2`. Fishing drops use `item`, `min`, `max` and a relative `weight`; `keep-vanilla-percent` controls how often the normal catch is kept. The `skyblock_template.json` file defines the island used for new rounds and island resets.

### Random Items filters — `blacklist-config.yml`

The server supplies the **current item catalog** automatically. Both Random Items modes apply these filters:

| Section | How it filters items |
| --- | --- |
| `material-blacklist.items` | Excludes exact material names, such as `NETHER_STAR`. |
| `name-filters.patterns` | Excludes names containing a substring, such as `_SPAWN_EGG`. These are not wildcard patterns. |
| `colored-variants.categories` | Excludes colored versions of listed categories, such as `WOOL`, using `color-prefixes`. |
| `options.debug-export` | Generates CSV reference lists when the Random Items pool loads. |
| `options.warn-unknown-materials` | Logs blacklist entries that do not exist in the running version. |

Each filter group has an `enabled` switch. Use uppercase names and patterns. To allow an item, remove **all** matching exclusions. Patterns match substrings: `LIGHT` also excludes `LIGHT_BLUE_WOOL`.

With CSV export enabled, the plugin writes:

- **`randomitem_pool.csv`** — allowed targets, with `name`, `isBlock` and `isEdible` columns.
- **`randomitem_all_items.csv`** — the current item catalog, with an exclusion reason or an empty reason for allowed items.

Edit **`blacklist-config.yml`**, not the CSVs. Exports are regenerated when a Random Items mode loads. The skip-token material is always excluded from targets.
