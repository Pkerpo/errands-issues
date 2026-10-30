# FAQ and troubleshooting

## Basics

**Does Errands work on Spigot or CraftBukkit?**
No. Errands is a **Paper-only** plugin (it uses Paper's command and scheduler APIs). Use Paper
1.21.4–26.2 (tested on 1.21.11 and 26.2). On Spigot it will not load.

**Which Java version do I need?**
Java 21 or newer. Paper 26.1 and later need Java 25; that is Paper's requirement, not ours.

**Is there a license key?**
No. There is no license key, no phone-home and no DRM.

**Does Errands contact the internet?**
Only through the update checker, if enabled. It asks `api.modrinth.com` for the newest version every 12 hours.
Modrinth, like any web server, sees your server's IP address; no player data is sent. Turn it off with `update-checker: false`; see [Configuration](Configuration.md#update-checker).

## Rewards and resets

**A player says their reward vanished.**
Quests are replaced when their pool resets (daily or weekly). A quest that is finished but **not
claimed** by then is lost, with its reward. The menu shows a warning on finished quests. Tell your
players to claim before the reset; the time left is shown in the menu and in `%errands_reset_daily%`.

**Money rewards do nothing.**
Install [Vault](https://www.spigotmc.org/resources/vault.34315/) **and** an economy plugin (for example
EssentialsX). Without them the money part of a reward is skipped, the other rewards are still paid, and the console
logs a warning that names the reason. The default quest pack has money rewards commented
out; remove the `#` once Vault works.

**A player's inventory was full.**
Items that do not fit are dropped at the player's feet and the quest counts as claimed.

**Players asked to buy rerolls or rewards.**
Please do not sell them; see [Commands and Permissions](Commands-and-Permissions.md#please-do-not-sell-rerolls-or-rewards-for-real-money).

## Quests

**Progress does not go up.**
- Blocks the player placed do not count for `BREAK_BLOCK` (anti-farm).
- Creative and spectator players do not make progress.
- Walking does not count in vehicles, while flying, gliding or swimming, or for teleports.
- The block, item or mob must match `targets` exactly; check the id spelling in `quests.yml`. Unknown
  ids are listed in the console on `/errands reload`.
- In lite, only six objective types work.

**"That quest is gone" when claiming.**
The quest was replaced (reset, reroll, or the quest was removed from `quests.yml`). Open the menu again.

**A player just joined and sees no quests.**
Player data loads in the background when they join. It takes a moment. `/errands info` shows how many
players are loaded.

**I changed `quests.yml` and nothing happened.**
Run `/errands reload`. Read the console: a quest with an error is skipped and named in the log.

**I get a YAML error.**
Put ids, rewards and times in double quotes, and use spaces, not tabs.

**I renamed a quest id and players lost progress.**
Do not rename ids while players hold the quest. The old quest is replaced at the player's next access.

**Quest names show as "Miner 1" instead of a title.**
There is no `quests.<id>.name` in your language file. See [Quests](Quests.md#names-and-descriptions).

## Storage and servers

**Can two servers share one MySQL database?**
Yes, but each server needs its own `storage.mysql.table-prefix`. Quests are **not** synced between
servers; two servers on the same prefix overwrite each other's progress.

**The plugin disabled itself at startup with a MySQL error.**
Errands refuses to fall back to SQLite, because players would get fresh quests and could claim twice.
Fix the `storage.mysql` settings (host, port, user, password) and restart.

**Where is the player data?**
`plugins/Errands/data.db` (SQLite) or the `<prefix>player` table (MySQL). Back it up.

## Other

**Progress does not count in one of my worlds.**
Check `disabled-worlds` in `config.yml` and the `worlds:` list of the quest; see [Quests](Quests.md#worlds).

**Does Errands work on Folia?**
Yes, tested on Folia 1.21.11. Errands is built on Paper's region, entity and global schedulers.
Report any problem in the issue tracker.

**Does `/reload` work?**
Do not use the vanilla/Bukkit `/reload`; it breaks many plugins. Use `/errands reload` for Errands.

**Where do I report a bug?**
https://github.com/pkerpo/errands-issues/issues. Include `/version`, `/errands info` and the console log.
