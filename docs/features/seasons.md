# 🏆 Seasons & Tournaments Engine

BlueHellStats includes a seasonal competition engine that automates tournament schedules, historical archiving, and safe reward distribution.

---

## ⚙️ Season Configuration (`seasons.yml`)

```yaml
season:
  enabled: true
  auto-rollover: true
  rollover-interval-days: 30
  barrier-delay-ms: 1000 # Synchronization delay across multi-server networks

current-season:
  id: 1
  name: "Season 1 - Genesis"
  start-time: "2026-11-01T00:00:00Z"
  end-time: "2026-11-30T23:59:59Z"

rewards:
  claim-on-join: true
  fallback-podium-rewards:
    1:
      - "eco give %player% 50000"
      - "broadcast <gold>[SEASON]</gold> <yellow>%player%</yellow> conquered Season #%season%!"
    2:
      - "eco give %player% 25000"
    3:
      - "eco give %player% 10000"
```

---

## 🔒 Safe Tournament Rollovers

When a season expires (either automatically based on duration or manually via `/season rollover`):

1. **Snapshot & Lock:**
   The server takes a synchronized snapshot of all leaderboard standings to ensure rankings are captured accurately without mid-reset race conditions.
2. **Historical Archiving & Reward Distribution:**
   Top winners for every active category are permanently archived in the Hall of Fame. Winning reward commands are queued safely for offline and online players.
3. **Season Activation & Reset:**
   The new season starts automatically with fresh leaderboards, while past season achievements remain permanently viewable in the Hall of Fame.

---

## 🎁 Reward Delivery & Hall of Fame Browser

* **Offline Reward Safety:** If a player is offline when the season ends, their rewards remain safely queued until their next login (`claim-on-join: true`) or until they execute `/season claim`.
* **Interactive Hall of Fame (`/halloffame`):** Players can open an interactive GUI at any time to browse past tournament seasons, inspect podium winners, and see final scores.
