# ⌨️ Commands & Permissions Reference

All commands support tab-completion, permission checks, and localization messages.

---

## 👥 Player Commands

| Command | Permission | Description |
| :--- | :--- | :--- |
| `/top` | `bluehell.top` | Opens the main categories leaderboard hub (`menus/hub.yml`). |
| `/top <stat>` | `bluehell.top` | Directly opens the leaderboard for a specific category (`menus/top.yml`). |
| `/top profile [player]` | `bluehell.profile` | Opens the multi-stat detail profile GUI for yourself or another player. |
| `/halloffame` (alias `/hof`) | `bluehell.hof` | Opens the historical browser of past seasons and podium winners. |
| `/season info` | `bluehell.season.info` | View active season duration and remaining time. |
| `/season claim` | `bluehell.season.claim` | Manually claim pending seasonal rewards. |

---

## 🛡️ Administrator Commands (`/bhe`)

Master permission: `bluehell.admin` (granted to operators by default).

| Command | Description |
| :--- | :--- |
| `/bhe add <player> <stat> <amount>` | Increment a player's score by the given amount. |
| `/bhe set <player> <stat> <amount>` | Set a player's score to an exact number. |
| `/bhe reset <player> <stat>` | Reset a player's stat to 0. |
| `/bhe profile [player]` | View any player's multi-stat profile GUI as administrator. |
| `/bhe reload` | Hot-reload all configurations, menus, and stats without server reboot. |

### 🗿 Podium Sub-commands (`/bhe podium`)

| Sub-command | Description |
| :--- | :--- |
| `/bhe podium create <stat> <1\|2\|3> [id]` | Spawn a 3D in-world podium statue at your exact location and yaw. |
| `/bhe podium remove <id\|stat>` | Remove a specific podium statue by ID or all podiums of a stat. |
| `/bhe podium list` | List all active 3D podiums and coordinates. |
| `/bhe podium refresh [id]` | Force immediate visual re-render of podium statues and billboards. |

### 🪧 Hologram Sub-commands (`/bhe holo`)

| Sub-command | Description |
| :--- | :--- |
| `/bhe holo create <stat> [rows] [name]` | Spawn a DecentHolograms floating leaderboard at your position. |
| `/bhe holo remove <name\|stat>` | Remove a floating leaderboard hologram. |

---

## 🏆 Season Admin Commands (`/season`)

Permission: `bluehell.season.admin`

| Command | Description |
| :--- | :--- |
| `/season status` | Display active season details, start/end dates, and barrier state. |
| `/season start <number> <name> [duration]` | Force start a new season (e.g. `/season start 2 "Winter Cup" 30d`). |
| `/season end` | Force terminate the active season. |
| `/season rollover [number] [name] [duration]` | Execute atomic Two-Phase Rollover Barrier immediately. |
| `/season resetall confirm` | Master administrative reset back to Season #1 (wipes history). |

