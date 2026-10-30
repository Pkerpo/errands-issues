# Quests (`quests.yml`)

`plugins/Errands/quests.yml` holds the reset times, the number of quest slots and every quest.
After editing, run `/errands reload`. A quest with an error is skipped and the console names it; all
other quests still load.

Always put ids, rewards and times in **double quotes**. YAML can misread `minecraft:stone` inside a
list, and reads an unquoted `00:00` as a number.

## The default quest pack

The `quests.yml` that ships with Errands has **40 quests**: 30 daily and 10 weekly, covering all 12
objective types, with XP and item rewards. Every quest has a commented-out `money:` line: remove the `#`
once Vault and an economy plugin are installed. In the lite edition only the daily quests with the six
lite objective types load. The streak section is included too (paid edition).
Use the pack as it is, or as a template for your own quests.

## Time zone and pools

```yaml
timezone: Europe/Amsterdam      # any IANA zone id, e.g. America/New_York, UTC

daily:
  slots: 3                      # quests per player at a time (lite: at most 3)
  reset: "00:00"                # HH:mm in the time zone above
  rerolls: 1                    # free rerolls per player per day (paid only)

weekly:                         # paid edition only
  slots: 2
  reset-day: MONDAY             # MONDAY ... SUNDAY (required)
  reset: "00:00"                # optional, default "00:00"
  rerolls: 0                    # optional, default 0
```

| Setting | Meaning |
|---|---|
| `slots` | How many quests each player holds from that pool. `0` switches the pool off. Raising it adds quests on reload; lowering it applies from the next reset. |
| `reset` | When the pool refreshes. Daylight saving changes are handled. Missed resets (server offline) are caught up on the next access. |
| `rerolls` | Rerolls a player gets per reset period. `errands.reroll.extra.<n>` adds more, see [Commands and Permissions](Commands-and-Permissions.md). |

A quest is drawn at random from its pool, weighted by `weight`. A player never holds the same quest twice at once.

## A quest

```yaml
quests:
  miner_1:
    pool: daily
    type: BREAK_BLOCK
    targets: ["minecraft:stone", "minecraft:deepslate"]
    amount: 256
    weight: 10
    icon: "minecraft:iron_pickaxe"
    rewards: ["xp:100", "item:minecraft:iron_ingot:8"]
```

| Field | Required | Meaning |
|---|---|---|
| id (`miner_1`) | yes | Letters, digits, `_` and `-`. Do not rename an id while players hold that quest: their progress would be lost. |
| `pool` | yes | `daily` or `weekly`. |
| `type` | yes | One of the 12 objective types below. |
| `targets` | no | Which blocks, items or mobs count. Leave it out (or `[]`) for "anything". Not allowed for `WALK_DISTANCE` and `PLAYTIME_MINUTES`. Short names work: `"stone"` and `"STONE"` become `minecraft:stone`. |
| `amount` | yes | How many are needed (1 or more). |
| `weight` | no | Chance of being drawn, relative to the other quests in the same pool. Default 1. `0` = never drawn. |
| `icon` | no | Item id shown in the menu. Default `minecraft:paper`. |
| `rewards` | no | See [Rewards](Rewards.md). |
| `name-key`, `lore-key` | no | Language keys for the name and description. Default `quests.<id>.name` and `quests.<id>.lore`. |

Unknown block, item or mob ids are reported in the console and ignored.

## The 12 objective types

Lite edition: the first six only. A quest with a paid-only type is skipped in lite with a console notice.

| Type | Counts | Targets | Amount is | Example |
|---|---|---|---|---|
| `BREAK_BLOCK` | Blocks the player breaks | Block ids | Blocks | `targets: ["minecraft:diamond_ore", "minecraft:deepslate_diamond_ore"]`, `amount: 16` |
| `PLACE_BLOCK` | Blocks the player places | Block ids | Blocks | `amount: 128` (any block) |
| `KILL_MOB` | Mobs the player kills | Mob ids | Kills | `targets: ["minecraft:zombie", "minecraft:skeleton"]`, `amount: 20` |
| `FISH` | Things caught while fishing | Id of the caught item | Catches | `targets: ["minecraft:cod", "minecraft:salmon"]`, `amount: 10` |
| `CRAFT` | Items crafted | Id of the crafted item | Items | `targets: ["minecraft:torch"]`, `amount: 64` |
| `SMELT` | Items taken out of a furnace, smoker or blast furnace | Id of the result | Items | `targets: ["minecraft:iron_ingot"]`, `amount: 32` |
| `WALK_DISTANCE` | Distance walked | none | Blocks | `amount: 2000` |
| `PLAYTIME_MINUTES` | Time online | none | Minutes | `amount: 600` |
| `EAT` | Food or drink consumed | Id of the food | Times | `targets: ["minecraft:bread"]`, `amount: 10` |
| `BREED` | Animals bred | Mob ids | Animals bred | `targets: ["minecraft:cow", "minecraft:sheep"]`, `amount: 5` |
| `ENCHANT` | Items enchanted at an enchanting table | Id of the enchanted item | Items | `targets: ["minecraft:diamond_sword"]`, `amount: 3` |
| `TRADE_VILLAGER` | Trades completed with villagers | Id of the item received | Trades | `targets: ["minecraft:emerald"]`, `amount: 30` |

Paid only: `WALK_DISTANCE`, `PLAYTIME_MINUTES`, `EAT`, `BREED`, `ENCHANT`, `TRADE_VILLAGER`.

Rules worth knowing:

- **Placed blocks do not count** for `BREAK_BLOCK` (controlled by `anti-farm.placed-blocks` in `config.yml`).
  Players cannot place a block and break it again to farm progress.
- **Creative and spectator** players never make progress.
- **Walking** ignores vehicles, flying, gliding, swimming and riptide, and large jumps (teleports).
- **Playtime** counts one minute for each minute online, per player.
- Progress is capped at the goal, and a finished quest stays finished until claimed.

## Streaks (paid edition)

```yaml
streaks:
  enabled: true
  rewards:                      # streak length in days -> rewards
    3: ["xp:200"]
    7: ["item:minecraft:diamond:3"]
```

A day counts when a player claims **every** daily quest of that daily period. Missing a period resets
the streak. A bonus is paid when the streak reaches exactly a listed length. The current streak is
available as `%errands_streak%` ([PlaceholderAPI](PlaceholderAPI.md)).

## Names and descriptions

Every quest id needs a name and a description in the language files
(`plugins/Errands/lang/en_US.yml` and `nl_NL.yml`):

```yaml
quests:
  miner_1:
    name: "Quarry Work"
    lore: ["<gray>Mine <white><goal><gray> blocks of stone", "<gray>or deepslate."]
```

`<goal>` is replaced by the `amount`. Texts use [MiniMessage](https://docs.advntr.dev/minimessage/format.html)
(`<red>`, `<bold>` and so on). A quest without a name is shown with its id ("miner_1" becomes "Miner 1").
To write quests of your own, add them to `quests.yml` and add `name` and `lore` for the new ids to
your language files.

## Worlds

Limit a quest to certain worlds with `worlds:`. Progress only counts while the player is in one of them
(names as in your server folder, case does not matter):

```yaml
  nether_hunter:
    pool: daily
    type: KILL_MOB
    worlds: ["world_nether"]
    amount: 15
```

A quest without `worlds:` (or with an empty list) counts in every world. To switch quests off in some
worlds altogether (a creative or minigame world), use `disabled-worlds` in
[`config.yml`](Configuration.md#worlds). Both filters apply to all 12 objective types, and `PLAYTIME_MINUTES`
does not tick in excluded worlds. `/errands progress` ignores them.

Tip: a quest limited to a world that a player cannot reach can still be drawn for that player. Only
list worlds that everyone can visit.
