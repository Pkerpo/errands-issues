# PlaceholderAPI

The paid edition registers an expansion named `errands` when [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/)
is installed. There is nothing to download with `/papi ecloud`; it is built in and loads on startup.
The lite edition has no placeholders.

| Placeholder | Value |
|---|---|
| `%errands_daily_completed%` | Daily quests whose goal the player has reached (claimed or not) |
| `%errands_daily_total%` | Daily quests the player holds |
| `%errands_weekly_completed%` | Weekly quests whose goal the player has reached |
| `%errands_weekly_total%` | Weekly quests the player holds |
| `%errands_streak%` | The player's current daily streak, in days |
| `%errands_reset_daily%` | Time until the next daily reset, e.g. `23h 59m` |
| `%errands_reset_weekly%` | Time until the next weekly reset, e.g. `6d 4h` |

Players whose data is not loaded yet (just joined) get `0` for the number placeholders.
The two reset placeholders are the same for every player.

## Examples

Scoreboard or tab list line:

```
Quests: %errands_daily_completed%/%errands_daily_total%   Streak: %errands_streak%
```

Hologram or chat text: `Next daily reset in %errands_reset_daily%`.

If a placeholder shows up as plain text, check that PlaceholderAPI is installed and enabled,
that you run the **paid** jar, and that `/papi list` shows `errands`.
