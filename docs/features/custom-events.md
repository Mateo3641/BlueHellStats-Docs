# 🔌 Zero-Overhead Dynamic Custom Event Engine

BlueHellStats features an event interception engine that allows you to hook and track events emitted by **any 3rd-party plugin** (e.g. custom turrets, slimefun machines, jobs, custom bosses) without having to add compile-time Maven dependencies.

---

## 🚀 Configuration Syntax (`stats.yml`)

Set `event: CUSTOM` and declare the `custom_event_data` block:

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
| `event` | The fully-qualified Java class name of the event. | `"dt.ajneb97.api.TurretPlaceEvent"` |
| `player_variable` | Method chain returning the `Player` performing the event. | `"getPlayer()"` or `"getEntity()"` |
| `variables_to_capture` | List of `<placeholder>;<getter_method_path>`. | `"%world%;getLocation().getWorld().getName()"` |
| `conditions` | Logic checks comparing captured placeholders. | `"%world% equals world"`, `"%level% >= 5"` |

---

## ⚡ Technical Architecture: Why is it Lag-Free?

Traditional dynamic event reflection in Bukkit plugins uses `java.lang.reflect.Method.invoke()`, which causes significant overhead and garbage allocation.

BlueHellStats eliminates this:
1. **Boot Compilation via `MethodHandleChain`:**
   During server startup or `/bhe reload`, the method paths (e.g. `getLocation().getWorld().getName()`) are parsed and compiled into high-performance `java.lang.invoke.MethodHandle`s linked directly to the JVM call site.
2. **Native Bukkit `EventExecutor`:**
   The event listener registers directly into Bukkit's `HandlerList` via `pluginManager.registerEvent()`, bypassing heavy listener proxying.
3. **Bytecode Invocation ($O(1)$):**
   When the event fires, methods are invoked through pre-bound `MethodHandle.invokeExact()` / `invoke()`, running at near-native compiled Java speed.

