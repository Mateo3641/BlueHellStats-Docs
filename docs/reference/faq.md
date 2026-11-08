# ❓ Frequently Asked Questions & Troubleshooting

Find answers to common questions and troubleshooting advice for BlueHellStats.

---

## ⚡ Performance & Server Load

### Will BlueHellStats cause lag on high player-count servers?
**No.** BlueHellStats is built specifically for high-scale production networks:
* All database interactions (queries, updates, and schema migrations) execute asynchronously in the background.
* Player stat modifications are accumulated in memory and periodically saved in batches, minimizing disk I/O.
* Leaderboard sorting and placeholder evaluations operate on pre-cached data, guaranteeing $O(1)$ instant lookups without blocking the server's main tick loop.

---

## 🌐 Multi-Server & Network Setup

### How do I sync statistics across my network?
1. Open `config.yml` on each of your server instances.
2. Set `database.type` to `MYSQL`, `MARIADB`, or `POSTGRESQL`.
3. Provide the connection credentials for your shared database.
4. Leave `sync.server-id` empty so each server generates its own unique ID, or give each server a friendly name (e.g., `survival-1`, `lifesteal-1`).
5. All connected servers will sync player data and update leaderboards automatically.

---

## 🗿 3D Podiums & Holograms

### Which podium provider should I use?
* **`ARMOR_STAND`**: Best if you want zero external dependencies. Works directly on vanilla Paper servers with custom player heads and tier armor.
* **`FANCY_NPCS`**: Recommended for optimal performance on large servers. Uses packet-based rendering with zero server-side entity overhead and supports custom poses (`sitting`, `crouching`, etc.).
* **`CITIZENS`**: Best if your server already utilizes Citizens and you want full NPC trait integration.

### How do I remove or move an existing podium?
To remove a podium:
```bash
/bhe podium remove <id|stat>
```
To reposition, simply stand at the new coordinates and execute:
```bash
/bhe podium create <stat> <rank>
```

---

## 🔌 Custom Events & 3rd-Party Plugins

### Can I track custom events from plugins without coding?
**Yes!** Using the Dynamic Custom Event Engine in `stats.yml`, you only need the event class name and getter methods from the plugin's documentation. BlueHellStats hooks the event automatically at startup without requiring custom code or recompilation.

---

## 🏆 Seasons & Reward Distribution

### What happens if a player is offline when a season ends?
Pending rewards are safely queued in storage. When the winning player logs into the server, the queued rewards and commands are automatically executed (`claim-on-join: true`), or the player can run `/season claim`.

---

## 🔧 Troubleshooting

### My placeholders are displaying `N/A` or empty values
* Make sure **PlaceholderAPI** is installed on your server.
* If testing a new statistic, ensure the player has triggered the stat at least once so their profile is registered.
* Check your `stats.yml` to verify that the stat ID matches the placeholder query (e.g. `%bluehell_stat_diamonds_mined%`).

### Database connection fails on startup
* Verify that your MySQL / MariaDB / PostgreSQL server is accessible from your Minecraft server host.
* Check that the user has `CREATE`, `SELECT`, `INSERT`, `UPDATE`, and `DELETE` privileges on the target database.
* Ensure firewall rules permit traffic on the configured port (default `3306` for MySQL, `5432` for PostgreSQL).

### The podium NPC isn't wearing the top player's skin
* Skins are fetched asynchronously from the Mojang API. On newly created podiums or servers in offline mode, ensure the server has an active internet connection to contact Mojang skin servers.
