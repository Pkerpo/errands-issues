# Configuration (`config.yml`)

`plugins/Errands/config.yml` holds the technical settings. Quests, pools and reset times are in
[`quests.yml`](Quests.md); all texts are in `lang/<language>.yml`. After editing, run `/errands reload`.
Storage changes need a **server restart**.

## Settings

```yaml
config-version: 1          # do not change

language: en_US            # en_US or nl_NL

storage:
  type: sqlite             # sqlite or mysql
  mysql:
    host: localhost
    port: 3306
    database: errands
    username: errands
    password: change-me
    use-ssl: false
    pool-size: 4
    table-prefix: "errands_"
  save-interval-seconds: 60

anti-farm:
  placed-blocks: true

disabled-worlds: []

update-checker:
  enabled: true
  modrinth-project-id: errands
  changelog-url: ""
```

| Setting | Meaning |
|---|---|
| `language` | Which file in `lang/` is used. Both languages ship with both editions. Unknown values fall back to `en_US`. You can copy a file, translate it and set `language` to its name (letters, digits, `_` and `-` only). |
| `storage.type` | `sqlite` keeps progress in `plugins/Errands/data.db`, with no setup. `mysql` uses a MySQL or MariaDB server (paid edition; lite uses SQLite and logs a warning). |
| `storage.mysql.*` | Connection settings, used only when `type` is `mysql`. If the database cannot be reached at startup, Errands **disables itself** instead of switching to SQLite, because that would give players fresh quests to claim again. |
| `storage.mysql.table-prefix` | Table names are `<prefix>player`. **One prefix per server**, see below. |
| `storage.save-interval-seconds` | How often changed data is written (minimum 10). Data is also saved when a player leaves, after every claim, and when the server stops. |
| `anti-farm.placed-blocks` | When `true`, blocks placed by players do not count for `BREAK_BLOCK` quests. Marks follow pistons. |
| `disabled-worlds` | Worlds where **no** quest progress counts. See below. |
| `update-checker.enabled` | Asks Modrinth for the newest Errands version. See below. |
| `update-checker.modrinth-project-id` | Which Modrinth project to check. Leave it as it is. |
| `update-checker.changelog-url` | Link to the changelog, shown in the update message. |

## MySQL: one prefix per server

Quest progress is **not synced between servers**. Two servers that use the same database *and* the
same `table-prefix` will overwrite each other's progress. If several servers share one database, give
each its own prefix:

```yaml
table-prefix: "survival_"     # server 1
table-prefix: "skyblock_"     # server 2
```

A player then has separate quests on each server. Network-wide sync is not part of 1.0.

## Language files

`lang/en_US.yml` and `lang/nl_NL.yml` contain every text, including quest names and the menu.
They use [MiniMessage](https://docs.advntr.dev/minimessage/format.html). You can edit them freely; the
`<prefix>` tag inserts the `prefix` text, and each key has a comment that lists the `<placeholders>` it supports.

When you update Errands, keys that your copy lacks are added at the end of their section from
the new version (a dated backup is made first). Your own changes, comments and extra keys stay.

## Worlds

```yaml
disabled-worlds:
  - "creative"
  - "minigames"
```

Progress for **every** quest is ignored in these worlds (world names as in your server folder, case does
not matter), for both editions. Players can still open the menu and claim. To limit one quest to
certain worlds instead, use `worlds:` on that quest, see [Quests](Quests.md#worlds).

## Update checker

When `update-checker.enabled` is `true`, Errands asks `api.modrinth.com` for the newest Errands release when
the server starts and every 12 hours. If a newer version exists, it prints one line to the console and tells
players with `errands.admin` when they join. Premium servers are told to download the update from where
they bought Errands, together with the `changelog-url` link.

- The request contains only the project id and the Errands version (in the `User-Agent`).
  **No player data is sent.** Like any web request, it does reveal your server's **IP address** to
  `api.modrinth.com`.
- The check runs in the background and never delays the server. If it fails, nothing happens.
- It is the only network request Errands makes. Turn it off with `enabled: false`, or the short form
  `update-checker: false`, and Errands never contacts anyone.
- The messages `update.available` and `update.available-premium` in the language files control what admins see.

Because the check contacts a third party, mention it in your privacy notice if you operate a server
for players in the EU.
