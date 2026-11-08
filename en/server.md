# Server and datapacks

[Home](../README.md) · [Guide](guide.md) · [Earning XP](xp.md) · [Skill trees](trees.md) · [Français](../fr/serveur.md)

## Configuration

Each world has its own file: `serverconfig/legacyskills-server.toml` in the world folder (in single player: `saves/<world>/serverconfig/`). It's created on the first start, with a comment on each line. Default values:

| Section | Key | Default | What it does |
|---|---|---|---|
| `progression` | `max_level` | 50 | Maximum level of a discipline. XP keeps adding up above it |
| `progression` | `point_cap` | 24 | Cap on points from levels, before Jack of All Trades |
| `budget` | `xp_per_hour` | 4000 | Raw XP per hour of play, per player and discipline. 0 turns the budget off |
| `budget` | `reserve` | 1000 | Maximum reserve |
| `diminishing_returns` | `full` | 20 | Load under which a gain stays at full rate |
| `diminishing_returns` | `recovery_per_minute` | 2 | Load lost per minute |
| `diminishing_returns` | `step_per_unit` | 0.05 | Drop per load unit above the full rate |
| `diminishing_returns` | `floor` | 0.1 | Lowest coefficient |
| `kills` | `max_base` | 200 | Most XP a kill is worth, shared between players |
| `kills` | `credit_expiry_seconds` | 120 | A player's credit on a creature is lost this long after their last hit |
| `multipliers` | `global`, `combat`, `crafting`, `adventure` | 1 | Multipliers applied after the budget |
| `multipliers` | `others` | empty | Other disciplines from datapacks: `"namespace:path=multiplier"` |
| `activity` | `enabled` | true | Inactivity filter |
| `activity` | `half_after_seconds` | 120 | Silence before ×0.5 |
| `activity` | `zero_after_seconds` | 300 | Silence before ×0 |
| `activity` | `look_degrees` | 2 | Smallest rotation that counts in a second |
| `activity` | `move_blocks` | 1 | Smallest movement that counts in a second |
| `sources.kill` | `enabled`, `spawner_multiplier` | true, 0.1 | Kills, and the share for monster spawner creatures |
| `sources.shield_block` | `enabled`, `xp_per_damage`, `damage_cap` | true, 0.5, 10 | Real shield blocks |
| `sources.anvil` | `enabled`, `xp_per_full_repair` | true, 30 | Anvil repairs |
| `sources.crafting` | `enabled`, `xp_per_item` | true, 6 | Crafting durable items |
| `sources.smithing` | `enabled`, `xp_per_item` | true, 25 | Smithing table |
| `sources.smelting` | `enabled`, `xp_per_item`, `returns_weight_per_item` | true, 0.5, 0.25 | Furnace output taken by hand |
| `sources.movement` | `enabled`, `xp_per_100m`, `cell_size` | true, 8, 64 | Sprinting, swimming, climbing |
| `sources.movement` | `returns_full`, `returns_recovery_per_minute` | 3, 0.3 | Diminishing returns per terrain area |
| `sources.advancement` | `enabled` | true | Advancements declared in datapacks |
| `nodes` | `salvage_chance` | 0.15 | Salvage chance |
| `respec` | `base_cost`, `cost_per_node`, `max_cost` | 5, 1, 30 | Respec cost in vanilla levels (the first respec is free) |
| `respec` | `delay_hours` | 2 | Play time between two respecs |
| `actives` | `rally_cry_cooldown`, `rally_cry_rampart_cooldown` | 120, 60 | Rally Cry cooldown, in seconds |
| `actives` | `rally_cry_duration`, `rally_cry_radius` | 6, 8 | Resistance I duration and radius |
| `actives` | `field_repair_cooldown`, `field_repair_share` | 90, 0.15 | Field Repair |
| `actives` | `dash_cooldown`, `dash_scout_cooldown`, `dash_strength` | 12, 6, 1.1 | Dash |
| `journal` | `gains_csv` | false | Writes every XP gain to `legacyskills_gains.csv` in the world folder |

### Help us balance the mod

The XP values are still design estimates. If you run a server, set `gains_csv = true`: each line of `legacyskills_gains.csv` is one XP attempt, with the source, the raw XP and what was left after each step. Sharing that file in the CurseForge comments helps a lot.

## Commands

All commands need operator level 2.

| Command | What it does |
|---|---|
| `/legacyskills info <player>` | XP, level, rank and reserve per discipline, free points, nodes, specialization, current activity |
| `/legacyskills xp <player> <discipline> get` | XP of a discipline |
| `/legacyskills xp <player> <discipline> add <n>` or `set <n>` | Adds or sets XP, with no filter. Works for an offline player too |
| `/legacyskills xp <player> <discipline> gain <n> <key>` | A gain that goes through diminishing returns and the budget. Online players only |
| `/legacyskills node <player> add <node>` or `remove <node>` | Adds a node following the purchase rules, or removes it |
| `/legacyskills spec <player> set <id>` or `clear` | Sets or clears the specialization |
| `/legacyskills reset <player>` | Erases the profile |
| `/legacyskills definitions` | Accepted disciplines, nodes, specializations and XP sources, refusals with their reason, and a hash |

## Saves

Profiles are saved in `data/legacyskills_players.dat` in the world folder, by player UUID. An unreadable file is renamed instead of being overwritten, and a file written by a newer version of the mod is never rewritten.

## Datapacks

Everything in the trees is JSON, reloaded with `/reload`. NeoForge conditions (`neoforge:conditions`) work. An unknown key gets the file refused. One refused file never stops the others from loading, and every refusal is logged and shown by `/legacyskills definitions`. If a node disappears, players who owned it get their points back.

| Folder | Content |
|---|---|
| `data/<namespace>/legacyskills/discipline/` | `icon`, `order` |
| `data/<namespace>/legacyskills/node/` | `discipline`, `icon`, `cost` (1 to 10), `required_level`, `requires` (8 nodes at most), `branch`, `x`, `y`, `exclusive_group`, `effects` |
| `data/<namespace>/legacyskills/specialization/` | `icon`, `bonus_points` (0 to 10), `requires` (`levels`, `nodes`), `effects` |
| `data/<namespace>/legacyskills/xp_source/` | XP sources, see below |

Node and specialization effects:

- `legacyskills:flag`: a flag read by Legacy Skills or by another mod through `legacyskills:unlocks`.
- `legacyskills:active`: an active skill (`legacyskills:rally_cry`, `legacyskills:field_repair`, `legacyskills:dash`).
- `legacyskills:attribute`: an attribute, an amount and an operation. Damage, attack speed, armor, health and luck are refused.

A node, for example:

```json
{
  "discipline": "legacyskills:adventure",
  "icon": "minecraft:feather",
  "cost": 1,
  "required_level": 3,
  "branch": "body",
  "x": 0,
  "y": 0,
  "effects": [
    {"type": "legacyskills:attribute", "attribute": "minecraft:generic.safe_fall_distance", "amount": 2.0, "operation": "add_value"}
  ]
}
```

XP sources:

```json
{"type": "legacyskills:advancement", "discipline": "legacyskills:combat", "advancement": "minecraft:adventure/kill_a_mob", "xp": 50}
```

```json
{"type": "legacyskills:external", "discipline": "legacyskills:crafting", "max_per_call": 100}
```

`xp` goes from 1 to 10000, `max_per_call` from 1 to 1000.

Tags: `legacyskills:durable_goods` (items that give crafting XP), `legacyskills:recoverable` (ingredients Salvage can give back), `legacyskills:no_kill_xp` (creatures that give no kill XP, empty by default).

## For modders

Legacy Skills exposes three entity capabilities, made only of JDK types: your mod doesn't need to depend on it. If Legacy Skills isn't installed, you get `null`.

```java
EntityCapability<Predicate, Void> UNLOCKS = EntityCapability.createVoid(ResourceLocation.parse("legacyskills:unlocks"), Predicate.class);
Predicate<ResourceLocation> unlocks = player.getCapability(UNLOCKS);
```

| Capability | Type | Use |
|---|---|---|
| `legacyskills:unlocks` | `Predicate<ResourceLocation>` | Is this flag, active skill, node or specialization in effect for the player? |
| `legacyskills:levels` | `ToIntFunction<ResourceLocation>` | Level of a discipline |
| `legacyskills:xp` | `ObjIntConsumer<ResourceLocation>` | Server only: gives XP in the name of a `legacyskills:external` source, capped per call, then the usual steps |

On the client, only the local player answers, from what the server sent.
