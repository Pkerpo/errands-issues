# Upgrading from Lite to Premium

The lite and paid editions use the same plugin name, the same data format and the same files, so
upgrading is a jar swap. Your quests, settings and players' progress stay.

## Steps

1. Stop the server.
2. Delete `Errands-Lite-<version>.jar` from `plugins/`.
3. Put `Errands-<version>.jar` (the paid jar) in `plugins/`. Never keep both jars.
4. Start the server. Check `/errands info`: the edition should say **premium**.

Do not delete `plugins/Errands/`. It holds your settings and `data.db`.

## What changes

| | Lite | Premium |
|---|---|---|
| Pools | Daily only | Daily and weekly |
| Daily slots | 3 at most | Any number |
| Objective types | 6 | All 12 |
| Rerolls | No | Yes |
| Streak bonuses | No | Yes |
| MySQL | No (SQLite only) | Yes |
| PlaceholderAPI | No | 7 placeholders |

## After the swap

- **Weekly quests and the six paid objective types** that lite skipped in your `quests.yml` are now
  loaded. Check the `weekly:` section (slots, reset day) in `quests.yml`, then run `/errands reload`.
  Players receive weekly quests at their next access.
- **Rerolls:** set `daily.rerolls` (and `weekly.rerolls`) in `quests.yml`. Players need `errands.reroll`
  (given to everyone by default).
- **Streaks:** set `streaks.enabled: true` and your bonus rewards in `quests.yml`.
- **MySQL:** to move to MySQL, change `storage.type` and restart. Existing progress in `data.db`
  is **not** copied over; players start with fresh quests on the new database.
- **Daily slots above 3:** after `/errands reload`, players are topped up to the new number of quests.

## Downgrading

Going back to lite works the same way. Weekly quests and paid types are skipped with a console notice;
your `quests.yml` is not changed.
