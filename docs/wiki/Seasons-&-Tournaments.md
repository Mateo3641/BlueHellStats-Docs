# 🏆 Seasons & Tournaments Engine

BlueHellStats includes a seasonal competition engine that automates tournament schedules, historical archiving, and safe reward distribution.

---

## ⚙️ Season Configuration (`seasons.yml`)

```yaml
season:
  enabled: true
  auto-rollover: true
  rollover-interval-days: 30
  barrier-delay-ms: 1000 # Distributed lock barrier coordination delay

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

## 🔒 Two-Phase Rollover Barrier

When a season expires (either automatically or via `/season rollover`):
1. **Phase 1: Freeze & Distributed Lock:**
   The coordinating node acquires a database-level distributed lock. Active score increments are temporarily frozen in RAM to ensure an uncorrupted snapshot.
2. **Phase 2: Archive & Reward Queuing:**
   The top players for each stat are immortalized into the `bh_hall_of_fame` table. Reward commands are queued into the `bh_pending_rewards` queue for Top 1, 2, and 3 players.
3. **Phase 3: Activation & Resumption:**
   A new active season ID is generated, stats for the new season are initialized, and live score increments resume.

---

## 🎁 Reward Delivery & Historical Browser

* **Safe Delivery:** Pending rewards are delivered via console commands when players log in (`claim-on-join: true`) or manually via `/season claim`.
* **Hall of Fame (`/halloffame`):** Players can open an interactive GUI to browse winners, podium standings, and final scores of past seasons.

