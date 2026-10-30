# Changelog

All notable changes to Errands are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions follow
[Semantic Versioning](https://semver.org/).

## [1.0.0] - 2026-10-30

First public release. Two editions are built from the same code: the paid **Errands** and the free **Errands Lite**.

### Added
- Daily and weekly quest pools. Every player gets random quests from the pools, drawn by configurable weights,
  and sees them in a menu (`/quests`). Pools reset on a schedule in your server's time zone; daylight-saving
  changes are handled.
- 12 objective types: break block, place block, kill mob, fish, craft, smelt, walk distance, playtime, eat,
  breed, enchant and trade with villagers.
- Rewards: items, XP, Vault money and console commands. Claiming twice never pays twice.
- Rerolls (limit per pool, extra rerolls through the `errands.reroll.extra.<n>` permission) and streak bonuses
  for completing quests on consecutive resets.
- A default pack of 40 ready-made quests (30 daily, 10 weekly) with XP and item rewards. Money rewards are
  included but commented out, because they need Vault and an economy plugin.
- World filter: an optional `worlds:` list per quest and a global `disabled-worlds:` setting.
- Update checker: asks the Modrinth API (api.modrinth.com, asynchronously) whether a newer version exists and tells
  players with `errands.admin` when they join. Modrinth sees the server's IP address; no player data is sent.
  Turn it off with `update-checker: false`. It contacts Modrinth and nothing else.
- Anti-farm protection: blocks placed by players are marked and do not count for block-breaking quests.
- Storage: SQLite out of the box; MySQL/MariaDB in the paid edition. Data loads asynchronously on join and saves
  with a debounce, on quit and when the server stops.
- PlaceholderAPI expansion (`%errands_...%`) and Vault support, both optional.
- English and Dutch language files; messages use MiniMessage.
- Developer API: `QuestService` and the events `QuestProgressEvent`, `QuestCompleteEvent`, `QuestClaimEvent`
  and `QuestsResetEvent`.
- Folia supported (tested on Folia 1.21.11): all tasks run on region, entity or global schedulers.
- Paper 1.21.4 to 26.3 (tested on 1.21.11, 26.2 and 26.3) on Java 21 or newer (Paper 26.x needs Java 25 to run).
- **Errands Lite** (free): daily pool only, 3 quest slots, 6 objective types, SQLite storage.

### Notes
- Paper only. Spigot and Bukkit servers are not supported.
- No license key. The only network access is the optional update checker: it contacts api.modrinth.com, which
  sees your server's IP address; no player data is sent. Turn it off with `update-checker: false` in `config.yml`.
