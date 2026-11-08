# 📊 Statistics Configuration (`stats.yml`)

BlueHellStats allows you to declare unlimited dynamic statistics with granular condition filtering, permission multipliers, milestone rewards, and seasonal prizes.

---

## 🛠️ YAML Structure & Options

```yaml
stats:
  <stat_id>:
    display-name: "<color>Display Name</color>"
    event: <EVENT_TYPE>
    increment: 1
    cooldown-ms: 0
    conditions:
      materials: [DIAMOND_ORE, DEEPSLATE_DIAMOND_ORE]
      entities: [PLAYER, ZOMBIE]
      game-modes: [SURVIVAL]
      ignore-silk-touch: true
      world-mode: WHITELIST # Options: WHITELIST or BLACKLIST
      worlds: [world, world_nether]
      regions: [arena_pvp] # WorldGuard region IDs
    multipliers:
      "bluehell.multiplier.vip": 2.0
      "bluehell.multiplier.mvp": 3.0
    milestones:
      - threshold: 100
        message: "<green>You reached 100 diamonds!</green>"
        broadcast: "<gold><player> reached 100 diamonds!</gold>"
        commands:
          - "give %player% diamond 5"
    season-rewards:
      1:
        - "eco give %player% 50000"
        - "broadcast <gold>[SEASON]</gold> <yellow>%player%</yellow> won Top 1!"
      2:
        - "eco give %player% 25000"
      3:
        - "eco give %player% 10000"
```

---

## 🎯 Supported Native Event Types

| Event Type | Bukkit Trigger Event | Supported Condition Filters |
| :--- | :--- | :--- |
| `BLOCK_BREAK` | `BlockBreakEvent` | `materials`, `ignore-silk-touch`, `game-modes`, `worlds`, `regions` |
| `BLOCK_PLACE` | `BlockPlaceEvent` | `materials`, `game-modes`, `worlds`, `regions` |
| `ENTITY_KILL` | `EntityDeathEvent` | `entities` (e.g. `PLAYER`, `ENDER_DRAGON`), `game-modes`, `worlds`, `regions` |
| `ITEM_CRAFT` | `CraftItemEvent` | `materials`, `game-modes`, `worlds` |
| `ITEM_CONSUME` | `PlayerItemConsumeEvent`| `materials`, `game-modes`, `worlds` |
| `PLAYER_FISH` | `PlayerFishEvent` | `materials` (caught item), `game-modes`, `worlds` |
| `CUSTOM` | Any Bukkit Event | See [[Custom-Event-Engine]] for details |

---

## 🧩 Composite Boolean Requirements

Define cross-stat requirements that can be evaluated dynamically in PlaceholderAPI (`%bluehell_req_<id>%`):

```yaml
requirements:
  nether_champion:
    pvp_kills: 50
    diamonds_mined: 500
    blaze_kills: 100
```
When a player reaches or exceeds all threshold values, `%bluehell_req_nether_champion%` returns `true`.

