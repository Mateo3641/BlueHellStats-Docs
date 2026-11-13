# 📑 PlaceholderAPI Reference

BlueHellStats provides a full expansion registered with **PlaceholderAPI** under the `%bluehell_...%` namespace.

All placeholders read directly from concurrency-safe RAM caches ($O(1)$) and **never execute blocking queries on the main server thread**.

---

## 📊 Player & Stat Placeholders

| Placeholder | Description | Output Example |
| :--- | :--- | :--- |
| `%bluehell_stat_<stat_id>%` | Raw integer stat value of the player. | `1450` |
| `%bluehell_stat_formatted_<stat_id>%` | Comma-formatted score with thousands separators. | `1,450` |
| `%bluehell_rank_<stat_id>%` | Player's placement in the leaderboard (or `N/A`). | `#3` |
| `%bluehell_has_<stat_id>_<min_score>%` | Boolean check whether player's score is $\ge$ minimum. | `true` or `false` |
| `%bluehell_req_<requirement_id>%` | Boolean check against composite multi-stat requirement. | `true` or `false` |

---

## 🏆 Leaderboard Position Placeholders

| Placeholder | Description | Output Example |
| :--- | :--- | :--- |
| `%bluehell_top_<stat_id>_<pos>_name%` | Player name at leaderboard placement `<pos>`. | `Steve` |
| `%bluehell_top_<stat_id>_<pos>_value%` | Raw score of player at placement `<pos>`. | `500` |
| `%bluehell_top_<stat_id>_<pos>_value_formatted%` | Comma-formatted score at placement `<pos>`. | `500` |

---

## 🗓️ Season & Tournament Placeholders

| Placeholder | Description | Output Example |
| :--- | :--- | :--- |
| `%bluehell_season_current_id%` | Numerical ID of the active season. | `1` |
| `%bluehell_season_current_name%` | Display name of the active season. | `Season 1 - Genesis` |
| `%bluehell_season_time_remaining%` | Formatted countdown until season ends. | `14d 05h 22m` |
| `%bluehell_history_<season>_<stat>_<rank>_name%` | Winner name of past season podium placement. | `Alex` |
| `%bluehell_history_<season>_<stat>_<rank>_score%` | Final score of past season podium placement. | `2,500` |
| `%bluehell_history_<season>_<stat>_<rank>_score_formatted%` | Formatted final score of past season podium placement. | `2,500` |

---

## 🚀 Active Booster Placeholders

| Placeholder | Description | Output Example |
| :--- | :--- | :--- |
| `%bluehell_booster_active%` | Whether any global or stat booster is currently running. | `true` or `false` |
| `%bluehell_booster_multiplier_<stat_id>%` | Current effective multiplier applying to `<stat_id>`. | `2.0x` |
| `%bluehell_booster_remaining_<stat_id>%` | Remaining duration of the governing booster (or `None`). | `29m 14s` |
| `%bluehell_booster_sponsor_<stat_id>%` | Sponsor name attached to the governing booster (or `None`). | `Steve` |

