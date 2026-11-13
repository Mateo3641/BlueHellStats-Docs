# 🧩 Developer API

Third-party plugins (custom gamemodes, guild systems, buffs, or Discord integrations) can interact with BlueHellStats through the official **`BlueHellStatsAPI`** facade and custom Bukkit events.

---

## 🔌 Accessing the API

Add `BlueHellEssence` to your `plugin.yml` (`depend` or `softdepend`), then retrieve the active API instance either directly or via Bukkit's `ServicesManager`:

```java
import com.dev.bluehell.api.BlueHellStatsAPI;
import org.bukkit.Bukkit;

if (BlueHellStatsAPI.isAvailable()) {
    BlueHellStatsAPI api = BlueHellStatsAPI.getInstance();
    long kills = api.getStat(player.getUniqueId(), "kills");
}
```

Or via `ServicesManager`:
```java
var registration = Bukkit.getServicesManager().getRegistration(BlueHellStatsAPI.class);
if (registration != null) {
    BlueHellStatsAPI api = registration.getProvider();
}
```

---

## ⚙️ Common API Operations

```java
// Read cached stat synchronously for online players
long blocks = api.getStat(player.getUniqueId(), "diamonds_mined");

// Modify stats safely (queued automatically for persistence)
api.addStat(player.getUniqueId(), "kills", 5L);

// Activate a 2x global booster for 30 minutes
api.activateBooster("all", 2.0, Duration.ofMinutes(30), "GuildEvent");

// Query the current active season and effective booster multiplier
api.getActiveSeason().ifPresent(season -> {
    String name = season.name();
});
double multiplier = api.getEffectiveBoosterMultiplier("kills");
```

---

## 📡 Custom Bukkit Events

### `PlayerStatIncrementEvent` (`Cancellable`)
Fired before a player's statistic is incremented. You can cancel tracking under custom conditions or adjust the final increment amount (for example, guild perks or custom boosters):

```java
@EventHandler
public void onStatIncrement(PlayerStatIncrementEvent event) {
    if (event.getStatId().equals("kills") && hasCustomPerk(event.getPlayer())) {
        event.setIncrementAmount(event.getIncrementAmount() * 2);
    }
}
```

### `SeasonRolloverCompleteEvent` (Async)
Fired asynchronously whenever a season rollover completes successfully, providing direct access to archived podium winners and reward queue totals:

```java
@EventHandler
public void onSeasonRollover(SeasonRolloverCompleteEvent event) {
    int finishedSeason = event.getArchivedSeasonId();
    int newSeason = event.getNewSeasonId();
    List<PodiumEntry> winners = event.getPodiums();
}
```
