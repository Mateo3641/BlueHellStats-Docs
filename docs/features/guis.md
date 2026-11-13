# 🎨 GUI Customization & Anti-Dupe Protection

All user interfaces in BlueHellStats are declarative, customizable via YAML files inside `plugins/BlueHellEssence/menus/`, and built with enterprise-grade anti-dupe security.

---

## 🛡️ Anti-Dupe Security & Protection

Virtual menus in Minecraft can be susceptible to item duplication and ghost item glitches if poorly designed. BlueHellStats employs a comprehensive security shield:

1. **Strict Interaction Cancellation:** All inventory clicks, drags, and item movements are strictly intercepted and cancelled before they can affect inventories.
2. **Hotbar Swap & Injection Shield:** Number key swaps (1-9), shift-clicks, off-hand swaps (`F`), and double-clicks are completely blocked.
3. **Macro & Autoclicker Debouncing:** An internal per-player debouncer prevents rapid autoclickers and packet flood race conditions during menu transitions.
4. **Tamper-Proof Action Metadata:** Menu buttons store navigation actions in persistent item metadata, ensuring menus cannot be exploited by item renaming, anvil tampering, or custom resource packs.

---

## 📁 Menu Configurations

### 1. Hub Menu (`menus/hub.yml`)
The main category selector opened via `/top`:
```yaml
title: "<dark_gray>Hall of Fame - Categories</dark_gray>"
rows: 4
items:
  diamonds_category:
    slot: 10
    material: DIAMOND
    action: OPEN_STAT:diamonds_mined
    name: "<aqua><bold>Diamonds Mined</bold></aqua>"
    lore:
      - "<gray>Your Score: <aqua><score_formatted></aqua> (<gold><rank></gold>)</gray>"
      - "<progress_bar> <gray>(<progress_percent>)</gray>"
      - "<yellow>▶ Click to open leaderboard</yellow>"
  claim_rewards:
    slot: 24
    material: CHEST_MINECART
    action: CLAIM_REWARDS
    name: "<green><bold>Claim Season Rewards</bold></green>"
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
    lore:
      - "<gray>Your score: <yellow><score_formatted></yellow></gray>"
      - "<gray>Next milestone: <aqua><next_milestone></aqua></gray>"
      - "<progress_bar> <gray>(<progress_percent>)</gray>"
```

### 3. Detailed Player Profile (`menus/profile.yml`)
Shows all statistics, scores, milestone progress, and network ranks for a specific player:
```yaml
title: "<dark_gray>Profile - <player></dark_gray>"
rows: 5
player_card:
  slot: 4
  material: PLAYER_HEAD
  name: "<gold><bold><player></bold></gold>"
stat_slots: [10, 11, 12, 13, 14, 15, 16, 19, 20, 21, 22, 23, 24, 25, 28, 29, 30, 31, 32, 33, 34]
stat_entry:
  name: "<yellow><bold><stat_display></bold></yellow>"
  lore:
    - "<gray>Score: <aqua><score_formatted></aqua></gray>"
    - "<gray>Ranking: <gold><rank></gold></gray>"
    - "<gray>Next Milestone: <yellow><next_milestone></yellow></gray>"
    - "<progress_bar> <gray>(<progress_percent>)</gray>"
items:
  prev_page:
    slot: 37
    material: ARROW
    action: PREVIOUS_PAGE
  claim_rewards:
    slot: 40
    material: CHEST_MINECART
    action: CLAIM_REWARDS
  next_page:
    slot: 43
    material: ARROW
    action: NEXT_PAGE
```

* **Interactive Navigation:** Clicking any player head in `/top <stat>` opens their multi-stat profile.
* **Automatic Pagination:** Automatically distributes categories across multiple pages if you configure more stats than available slots.

---

## 🎭 Custom Resource Packs & Head Textures

BlueHellStats seamlessly integrates with popular custom item and resource-pack plugins without requiring any manual setup or hard dependencies:

| Format | Example | Description |
| :--- | :--- | :--- |
| **ItemsAdder** | `itemsadder:bluehell:gold_trophy` (or `ia:...`) | Resolves a custom ItemsAdder stack directly in any menu slot. |
| **Oraxen** | `oraxen:emerald_crown` (or `oxn:...`) | Resolves a custom Oraxen item directly in any menu slot. |
| **Nexo** | `nexo:ruby_badge` | Resolves a custom Nexo item directly in any menu slot. |
| **Base64 Heads** | `basehead-<base64>` (or `head:<base64>`) | Renders custom textured player heads without needing online player profiles. |
| **CustomModelData** | `custom-model-data: 2025` | Supported on static buttons as well as dynamic templates (`entry`, `player_card`, and `stat_entry`). |

### Example Custom-Textured Item
```yaml
items:
  hall_of_fame_btn:
    slot: 22
    material: "itemsadder:server_ui:trophy_gold"
    custom-model-data: 1050
    action: OPEN_HOF
    name: "<gold><bold>Hall of Fame</bold></gold>"
    lore:
      - "<gray>Inspect past tournament winners!</gray>"
```

---

## 📊 Visual Progress Bars & GUI Actions

Any statistic card in `menus/hub.yml`, `menus/top.yml` (`my_stats`), or `menus/profile.yml` (`stat_entry`) supports dynamic milestone progress placeholders:

* `<progress_bar>` — Renders a 10-segment MiniMessage progress bar (`■■■■■□□□□□`) toward the next milestone.
* `<progress_percent>` — Renders percentage progress (e.g. `50%` or `100%`).
* `<next_milestone>` — Displays the next threshold value formatted with commas (or `MAX` once all milestones are unlocked).

### Supported Button `action` Values
* `OPEN_STAT:<stat_id>` — Opens the Top 10 leaderboard menu for `<stat_id>`.
* `OPEN_PROFILE` / `PLAYER_STATS` — Opens the player's profile grid.
* `OPEN_HOF` — Opens the Seasonal Hall of Fame browser.
* `CLAIM_REWARDS` — Closes the menu and claims all pending seasonal Top-3 rewards with configured sound feedback (`sounds.reward-claim` in `config.yml`).
* `COMMAND:<cmd>` — Runs `<cmd>` as the clicking player (`%player%` supported).
* `BACK`, `PREVIOUS_PAGE`, `NEXT_PAGE`, `CLOSE` — Built-in navigation controls (`sounds.gui-click` & `sounds.gui-page-turn` in `config.yml`).
