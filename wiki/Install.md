# Install

## Requirements

| | |
|---|---|
| Server software | **Paper** 1.21.4–26.3 (tested on 1.21.11, 26.2 and 26.3). Folia is supported (tested on Folia 1.21.11). Not Spigot, not CraftBukkit. |
| Java | 21 or newer. Paper 26.1 and later need Java 25 at runtime. |
| Optional | [Vault](https://www.spigotmc.org/resources/vault.34315/) and an economy plugin (for `money:` rewards), PlaceholderAPI (paid edition) |

Errands needs no other plugin. Quest data is stored in a local SQLite file by default.

## Steps

1. Stop the server.
2. Put the jar in the `plugins` folder. Use **one** of the two jars:
   - `Errands-<version>.jar`: the paid edition.
   - `Errands-Lite-<version>.jar`: the free edition.

   Never put both in the folder.
3. Start the server. Errands creates its files in `plugins/Errands/`.
4. Optional: edit `config.yml` and `quests.yml`, then run `/errands reload`.
5. Players open the menu with `/quests`. By default every player may use it and reroll.

## Files

| File | Purpose |
|---|---|
| `plugins/Errands/config.yml` | Language, storage, anti-farm. See [Configuration](Configuration.md). |
| `plugins/Errands/quests.yml` | Pools, reset times and every quest. See [Quests](Quests.md). |
| `plugins/Errands/lang/en_US.yml`, `nl_NL.yml` | All texts, in English and Dutch. Edit freely. |
| `plugins/Errands/data.db` | Player progress (SQLite). Back this up with the rest of your server. |

## Updating

Replace the jar and restart. Errands never overwrites your `config.yml` or `quests.yml`.
New texts in an update are added to your language files automatically (the old file is
saved first as `en_US.yml.bak-<date>-<time>`); your own edits and extra keys stay.
Read the changelog for new settings you may want to copy into your own files.

## Checking that it works

Run `/errands info` as an operator. It shows the version, the edition (premium or lite),
the number of quests loaded and the storage in use. The console prints the same edition at startup.

If something is wrong, see the [FAQ](FAQ.md).
