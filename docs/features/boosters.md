# 🚀 Global & Stat-Specific Boosters

Reward your community with timed **Happy Hours** or monetize global multipliers cleanly through **Tebex** and **CraftingStore**. BlueHellStats includes built-in support for both server-wide (`all`) and single-statistic boosters.

---

## 💡 Key Highlights

* **Zero Configuration Required:** Ready to trigger straight from the console or in-game.
* **Automatic Expiration:** Boosters automatically expire once their scheduled duration ends.
* **Tebex / Webstore Friendly:** Supports sponsor attribution (`[sponsor]`) so buyers are celebrated across the entire server.
* **Smart Precedence:** When both a global booster (`all`) and a stat-specific booster (e.g., `kills`) are active simultaneously, BlueHellStats applies the **highest active multiplier** rather than compounding infinitely.

---

## 🎮 Usage & Tebex Store Examples

### Server-Wide Happy Hour (All Statistics)
Grants a **2.0x** multiplier across every tracked stat for **30 minutes** credited to `Steve`:
```text
bhe booster start all 2.0 30m {username}
```

### Category-Specific Booster
Grants a **1.5x** multiplier strictly for `diamonds_mined` for **1 hour**:
```text
bhe booster start diamonds_mined 1.5 1h {username}
```

### Managing Active Boosters
* `/bhe booster list` — View all running boosters, sponsors, and remaining countdowns.
* `/bhe booster stop <stat|all>` — Cancel a specific or global booster early.
* `/bhe booster clear` — Immediately clear all running boosters.

---

## 🎨 Live BossBar & Audio Feedback (`config.yml`)

Active boosters automatically display a synchronized Adventure **BossBar** countdown to online players and play an activation sound, configured cleanly in `config.yml`:

```yaml
boosters:
  bossbar:
    enabled: true
    color: "YELLOW"
    overlay: "PROGRESS"
    title: "<gold><bold>⚡ %multiplier% BOOSTER</bold></gold> <gray>(%stat% • Sponsored by <yellow>%sponsor%</yellow>)</gray> <aqua>%remaining%</aqua>"

sounds:
  booster-start: "BLOCK_BEACON_ACTIVATE"
```

---

## 📑 Scoreboard & Chat Placeholders

Display live booster announcements on scoreboards, action bars, or holograms via PlaceholderAPI:

* `%bluehell_booster_active%` — `true` if any booster is currently running.
* `%bluehell_booster_multiplier_kills%` — Current effective multiplier (e.g. `2.0x` or `1.0x`).
* `%bluehell_booster_remaining_kills%` — Time remaining until expiration (e.g. `29m 45s` or `None`).
* `%bluehell_booster_sponsor_kills%` — Name of the player or webstore sponsor (`Steve` or `None`).

