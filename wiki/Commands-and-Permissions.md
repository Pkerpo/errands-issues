# Commands and permissions

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/quests` | `errands.use` | Opens the quest menu. Players only. |
| `/errands reload` | `errands.admin` | Re-reads `config.yml`, `quests.yml` and the language files. Storage changes need a restart. |
| `/errands info` | `errands.admin` | Shows the version, edition, quests loaded, storage in use and players loaded. |
| `/errands reset <player> [daily\|weekly\|all]` | `errands.admin` | Draws new quests for the player and resets their rerolls. Default `all`. |
| `/errands progress <player> <questId> <amount>` | `errands.admin` | Adds progress to the player's unclaimed quest with that id, whatever its type. Useful for testing. |

A missing permission gives a clear message, not "unknown command".

## Permissions

| Node | Default | Meaning |
|---|---|---|
| `errands.use` | everyone | Open the menu and claim rewards. |
| `errands.reroll` | everyone | May reroll quests (paid edition). |
| `errands.reroll.extra.<n>` | nobody | Adds `<n>` extra rerolls per pool and per reset period on top of the `rerolls` setting. If a player has several, the **highest** `n` counts. Example: `errands.reroll.extra.2`. |
| `errands.admin` | operators | The `/errands` commands. |

LuckPerms examples:

```
/lp group default permission set errands.use true
/lp group vip permission set errands.reroll.extra.2 true
```

## Using the menu

- **Left-click** a finished quest to claim it.
- **Shift + right-click** an unfinished quest to reroll it for a new one from the same pool.
  A claimed or finished quest cannot be rerolled. Lite has no rerolls.
- The info item shows the time until each reset and the rerolls left.

## Please do not sell rerolls or rewards for real money

Errands is built so that server owners **do not** have to turn quests into a shop, and we ask that you
do not: please do not sell extra rerolls (`errands.reroll.extra.<n>`), quests or quest rewards for
real money.

As we understand Mojang's [commercial usage guidelines](https://www.minecraft.net/article/commercial-usage-guidelines-updated),
a server must not sell gameplay advantages (pay-to-win) to players for real money. Extra rerolls and quest
rewards (items, XP, in-game money) are gameplay advantages. Granting
`errands.reroll.extra.<n>` through a paid rank or a store package is selling them. Give extra rerolls
for things that cost nothing: playtime, events, staff roles or votes. Cosmetic items are a different
matter; read the guidelines yourself, because they can change and the final responsibility for your server
is yours. This page is not legal advice.
