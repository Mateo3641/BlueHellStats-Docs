# 🎨 GUI Customization & Anti-Dupe Architecture

All user interfaces in BlueHellStats are declarative, customizable via YAML files inside `plugins/BlueHellEssence/menus/`, and built with enterprise-grade anti-dupe security.

---

## 🛡️ Anti-Dupe Security & HCI Design

Virtual menus in Minecraft can be susceptible to item duplication and ghost item glitches if poorly designed. BlueHellStats employs a multi-tiered security model:

1. **Priority Cancellation:** All `InventoryClickEvent` and `InventoryDragEvent` occurrences in any plugin menu are cancelled at `EventPriority.HIGHEST`.
2. **Hotbar Swap & Injection Shield:** Number key swaps (1-9), shift-clicks, off-hand swaps (`F`), and double-clicks are completely blocked.
3. **Macro Debouncing:** An internal 150ms per-player debouncer prevents autoclicker race conditions during menu transitions.
4. **PDC Navigation Routing:** Buttons store routing metadata in Paper's `PersistentDataContainer` (PDC), never relying on mutable item display names or lore.

---

## 📁 Menu Configurations

### 1. Hub Menu (`menus/hub.yml`)
The main category selector opened via `/top`:
```yaml
title: "<dark_gray>Hall of Fame - Categories</dark_gray>"
rows: 4
items:
  diamonds_category:
    slot: 11
    material: DIAMOND
    action: OPEN_STAT:diamonds_mined
    name: "<aqua><bold>Diamonds Mined</bold></aqua>"
    lore:
      - "<gray>Click to view leaderboard rankings!</gray>"
```

### 2. Leaderboard Menu (`menus/top.yml`)
Displays the top players for a category with freeform slot mapping:
```yaml
title: "<dark_gray>Top - <stat_display></dark_gray>"
rows: 5

top_slots:
  1: 13  # First place (center top)
  2: 21  # Second place (left)
  3: 23  # Third place (right)
  4: 29
  5: 30
  6: 31
  7: 32
  8: 33
  9: 39
  10: 41

items:
  my_stats:
    slot: 40
    material: PLAYER_HEAD
    action: PLAYER_STATS # Opens personal profile
    name: "<gold>Your Rank: <rank></gold>"
```

### 3. Detailed Player Profile (`menus/profile.yml`)
Shows all statistics, scores, and network ranks for a specific player:
```yaml
title: "<dark_gray>Profile: <player></dark_gray>"
rows: 6
stat_slots: [10, 11, 12, 13, 14, 15, 16, 19, 20, 21, 22, 23, 24, 25, 28, 29, 30, 31, 32, 33, 34]
previous_page_slot: 45
next_page_slot: 53
stat_item:
  material: BOOK
  name: "<yellow><stat_display></yellow>"
  lore:
    - "<gray>Score:</gray> <white><score></white>"
    - "<gray>Leaderboard Rank:</gray> <gold><rank></gold>"
```
* **Interactive Navigation:** Clicking any player head in `/top <stat>` opens their multi-stat profile.
* **Pagination:** Automatically distributes categories across pages if the server configures more stats than slots.

