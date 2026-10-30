# Rewards

A quest lists its rewards in `quests.yml`. They are paid, in order, when the player claims the quest
from the menu. Each quest is paid **at most once**, even if the player clicks twice at the same moment.

```yaml
rewards: ["xp:100", "item:minecraft:diamond:2", "command:give %player% minecraft:golden_apple 1"]
```

Put every reward in double quotes.

## Reward types

| Reward | Syntax | Notes |
|---|---|---|
| Money | `"money:500"`, `"money:12.5"` | Needs [Vault](https://www.spigotmc.org/resources/vault.34315/) and an economy plugin. Amount above 0. |
| Experience | `"xp:100"` | Experience **points**, not levels. Whole number, 1 or more. |
| Item | `"item:minecraft:diamond:2"` | Item id and an optional amount (default 1). `"item:diamond"` and `"item:DIAMOND:2"` also work. One item reward is capped at 6400 items. |
| Command | `"command:say %player% did it"` | Runs from the console. `%player%` is replaced by the player's name. A leading `/` is optional. |

### Money

If Vault or an economy plugin is missing, `money:` rewards are skipped with a console warning (the
other rewards of the quest are still paid). Either install them, or leave money rewards out of your `quests.yml`. The default quest pack ships money rewards commented
out for this reason; remove the `#` to turn them on.

### Items

- Items that do not fit in the inventory are **dropped at the player's feet**. A full inventory never
  blocks a claim.
- An unknown item id is reported in the console and ignored: the player still gets the other
  rewards of that quest. Check the console after `/errands reload`.

### Commands

Command rewards run as the console, after the other rewards of the same claim. On Folia they run on
the global region, as Folia requires for console commands.

Be careful with what a command can do. Only use commands you would run yourself as an operator.
Examples:

```yaml
"command:crate give key %player% common 1"
"command:lp user %player% permission settemp my.perk true 1d"
```

## Messages

When a claim succeeds the player sees `quest.claimed` with a short summary like
`500 money, 100 XP, 2x Diamond`. If items were dropped they see `quest.inventory-full-dropped`.
Both texts are in `lang/<language>.yml`.

## Unclaimed rewards are lost at the reset

Quests are replaced when their pool resets (daily or weekly). A quest that is finished but not
claimed at that moment is **gone with its reward**. The quest menu warns players about this. Mention
it on your server too, for example in the reset broadcast.

## Please do not sell quest rewards for real money

Please do not sell quest rewards, quests or extra rerolls for real money. See
[Commands and Permissions](Commands-and-Permissions.md#please-do-not-sell-rerolls-or-rewards-for-real-money).
