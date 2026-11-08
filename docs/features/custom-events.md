# 🔌 Zero-Overhead Dynamic Custom Event Engine

BlueHellStats features an event interception engine that allows you to hook and track events emitted by **any 3rd-party plugin** (such as custom turrets, custom enchants, slimefun items, custom bosses, or jobs) directly from your configuration, **without needing custom addons or programming**.

---

## 🚀 Configuration Syntax (`stats.yml`)

To track an external plugin's event, set `event: CUSTOM` and configure the `custom_event_data` section:

```yaml
stats:
  turrets_placed:
    display-name: "<yellow>Turrets Placed</yellow>"
    event: CUSTOM
    increment: 1
    custom_event_data:
      event: "dt.ajneb97.api.TurretPlaceEvent"
      player_variable: "getPlayer()"
      variables_to_capture:
        - "%world%;getLocation().getWorld().getName()"
        - "%turret_type%;getTurretType()"
      conditions:
        - "%world% equals world"
        - "%turret_type% equals LASER"
```

---

## ⚙️ Configuration Properties

| Property | Description | Example |
| :--- | :--- | :--- |
| `event` | The full event class name provided by the 3rd-party plugin. | `"dt.ajneb97.api.TurretPlaceEvent"` |
| `player_variable` | The method path returning the player responsible for the event. | `"getPlayer()"` or `"getEntity()"` |
| `variables_to_capture` | List of placeholders and their getter method chains. | `"%world%;getLocation().getWorld().getName()"` |
| `conditions` | Logic checks comparing captured variables before incrementing. | `"%world% equals world"`, `"%level% >= 5"` |

---

## 🔍 How to Find Event Names for Any Plugin

Most plugin developers publish their API event names in their documentation or GitHub repositories.

1. **Check the plugin's documentation:** Look for an "API" or "Developer" section (for example: `com.sk89q.worldguard.protection.events.DisallowedPVPEvent`).
2. **Identify the player getter:** Typically `getPlayer()`, `getEntity()`, or `getDamager()`.
3. **Capture contextual values:** You can chain standard Java getter methods, such as `getItem().getType().name()` or `getLocation().getWorld().getName()`.

---

## ⚡ Performance Guarantee

Tracking dynamic events in other plugins often causes severe lag spikes if implemented poorly. 

BlueHellStats solves this completely:
* **Pre-Compiled at Startup:** Method chains are resolved and compiled into high-speed executable handles during server startup and `/bhe reload`.
* **Zero Runtime Overhead:** When events fire in game, conditions are checked at near-native compiled speed with zero memory allocation or CPU overhead.
* **Safe Fallbacks:** If a configured plugin is not installed or the event class is not found, BlueHellStats logs a clean warning in console without crashing your server.
