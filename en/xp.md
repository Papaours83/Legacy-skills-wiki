# Earning XP

[Home](../README.md) · [Guide](guide.md) · [Skill trees](trees.md) · [Server and datapacks](server.md) · [Français](../fr/xp.md)

Every number on this page is a default value. A server can change all of them, see [Server and datapacks](server.md).

## Sources

### Combat

| Source | Raw XP |
|---|---|
| Killing a hostile creature | `min(max health, 200) × your share`. Your share is the damage you dealt divided by the larger of the total damage dealt by players and the creature's max health. A zombie (20 health) killed alone gives 20 XP |
| A creature from a monster spawner | A tenth of the above. Trial spawners are not affected |
| A hit from a hostile creature really blocked with a shield | `min(blocked damage, 10) × 0.5`, so 5 XP at most |

Damage only counts up to the creature's remaining health. Arrows, tridents, TNT you lit and splash potions count as your damage. A player's credit on a creature is lost 120 seconds after their last hit. Creatures in the `legacyskills:no_kill_xp` tag give nothing (empty by default).

### Crafting

| Source | Raw XP |
|---|---|
| Repairing at the anvil | `30 × share of durability restored`: a full repair gives 30 |
| Crafting an item from the `legacyskills:durable_goods` tag | 6 per item. The tag holds iron, gold and diamond tools and armor, shields, bows, crossbows, fishing rods and the like. Wooden and stone tools are not in it |
| Taking an item from a smithing table | 25 per item |
| Taking items out of a furnace by hand | 0.5 per item |

Shift-clicking gives exactly the same XP as normal clicks.

### Adventure

| Source | Raw XP |
|---|---|
| Sprinting, swimming and climbing | 8 per 100 m |

Diminishing returns for movement are counted per 64 by 64 block area, so running in circles in the same place quickly stops paying. Each area pays in full 3 times, then recovers slowly.

### Advancements

Fifteen vanilla advancements give XP, once per player, even if an operator takes the advancement away and gives it back. They skip the inactivity filter, diminishing returns and the budget.

| Advancement | Discipline | XP |
|---|---|---|
| Monster Hunter (kill a hostile mob) | Combat | 50 |
| Not Today, Thank You (deflect a projectile with a shield) | Combat | 50 |
| Postmortal (use a Totem of Undying) | Combat | 150 |
| Monsters Hunted (kill every hostile mob) | Combat | 500 |
| Free the End (kill the Ender Dragon) | Combat | 500 |
| Acquire Hardware (smelt an iron ingot) | Crafting | 50 |
| Isn't It Iron Pick (craft an iron pickaxe) | Crafting | 50 |
| Enchanter (enchant an item) | Crafting | 100 |
| Crafting a New Look (trim an armor) | Crafting | 100 |
| Cover Me in Debris (full netherite armor) | Crafting | 300 |
| We Need to Go Deeper (enter the Nether) | Adventure | 100 |
| Eye Spy (follow an Eye of Ender) | Adventure | 150 |
| The End? (enter the End) | Adventure | 200 |
| Hot Tourist Destinations (explore all Nether biomes) | Adventure | 300 |
| Adventuring Time (discover every biome) | Adventure | 500 |

No XP in creative or spectator mode, or for fake players (machines from other mods).

## What happens to a gain

Each gain goes through these steps, in this order:

1. **Inactivity filter** (creature kills, shield blocks and movement only).
2. **Diminishing returns** for that exact kind of gain.
3. **Hourly budget** of the discipline.
4. **Multipliers** set by the server (all 1 by default).

### Inactivity

The mod watches four kinds of signals: looking around (at least 2° in a second), moving (at least 1 block outside a vehicle), the inventory (opening or closing a menu, changing the selected slot, dropping an item) and the world (breaking or placing a block, right-clicking, jumping).

| Silence | XP |
|---|---|
| Up to 120 s | ×1 |
| More than 120 s | ×0.5 |
| More than 300 s | ×0 |

Attacking, chatting, taking damage and picking items up don't count as activity. Movement can't reset the silence for movement XP itself: a water current alone doesn't keep you active.

### Diminishing returns

Each kind of gain has its own load: kills of one creature type, shield blocks against one creature type, crafts or repairs of one item, movement in one terrain area. The load goes up by 1 per gain or per item (by your share for a kill, 0.25 per item at the furnace) and goes down by 2 per minute.

| Load | XP |
|---|---|
| Under 20 | ×1 |
| Above 20 | -5% per unit of load |
| Floor | ×0.1 |

Switching activities is always better than repeating one. The load is saved with your profile, so restarting the server doesn't reset it.

### The hourly budget

Each discipline has its own budget: 4,000 raw XP per hour of play, with a reserve of 1,000 XP. The reserve fills up as you play, and a gain can't take more than what's in it. What goes past the budget is lost. Time counts as play time only: time spent logged out doesn't fill the reserve.

## Levels

The cost from level L to level L+1 is `50 + 15 L + 1.2 L²` XP.

| Level | Total XP | Next level costs |
|---|---|---|
| 1 | 50 | 67 |
| 5 | 436 | 155 |
| 10 | 1,517 | 320 |
| 15 | 3,543 | 545 |
| 20 | 6,814 | 830 |
| 30 | 18,291 | 1,580 |
| 40 | 38,348 | 2,570 |
| 50 | 69,385 | maximum |

XP keeps adding up past level 50: if the server raises the maximum level, it counts straight away.

Ranks are only shown on screen and give nothing: rank I from level 0, II from 5, III from 15, IV from 30, V at 50.

## Points

Points come from the sum of your levels in all disciplines (cumulative levels). The gap between two points grows by 1 every 5 points.

| Points | Cumulative levels needed |
|---|---|
| 1 to 5 | 2, 4, 6, 8, 10 |
| 6 to 10 | 13, 16, 19, 22, 25 |
| 11 to 15 | 29, 33, 37, 41, 45 |
| 16 to 20 | 50, 55, 60, 65, 70 |
| 21 to 24 | 76, 82, 88, 94 |

Levels give 24 points at most. Jack of All Trades adds 3 above that cap, so 27 at most, for 35 points of nodes in the trees.

If you lose points (the cap goes down, data changes), the last nodes you bought are refunded and set as pending. They come back by themselves, with no new cost, as soon as they fit again.
