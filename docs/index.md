# ⚡ BlueHellStats Documentation

Welcome to the official documentation for **BlueHellStats**, the premium statistics tracking, leaderboard visualization, in-world 3D podium, and automated seasonal tournament engine engineered exclusively for modern Paper 1.21+ Minecraft servers and networks.

---

## 🌟 Key Features

* **⚡ Ultra-Low Latency & Zero Tick Lag:** Engineered from the ground up for massive player counts. All database queries, score updates, and leaderboard sorting run strictly in the background without affecting your server's 20.0 TPS.
* **🗿 In-World 3D Podiums:** Showcase your top players right in your spawn or lobby using vanilla ArmorStands, Citizens NPCs, or FancyNpcs with rotating floating billboards.
* **🏆 Automated Seasons & Tournaments:** Schedule monthly or custom seasons with automatic resets, reward queuing for winners, and an interactive Hall of Fame browser.
* **🔌 Dynamic Custom Event Engine:** Track custom actions from *any* 3rd-party plugin (custom items, bosses, skills, minigames) directly from `stats.yml` without writing a single line of Java code.
* **🎨 Fully Customizable Menus:** Modern declarative YAML menus with freeform slot positioning, pagination, player profile inspection, and military-grade anti-dupe security.
* **🌐 Network-Ready Storage:** Native support for SQLite (single server) as well as MySQL, MariaDB, and PostgreSQL with connection pooling for multi-server BungeeCord and Velocity networks.
* **📑 Extensive PlaceholderAPI Integration:** Instantly show personal stats, rankings, top lists, and season countdowns on scoreboards, chat, and tablists with zero lag.

---

## 🧭 Navigation Guide

Explore our comprehensive guides below or use the navigation tabs above:

| Section | Description |
| :--- | :--- |
| **[Quick Start](getting-started/quick-start.md)** | Installation, prerequisites, and first-time server boot guide. |
| **[Database Configuration](getting-started/database.md)** | Setup SQLite for single servers or MySQL/MariaDB/PostgreSQL for networks. |
| **[Statistics Configuration](features/stats.md)** | Declare stat categories, condition filters, WorldGuard regions, and milestones. |
| **[Dynamic Custom Events](features/custom-events.md)** | Hook and track events from 3rd-party plugins without coding. |
| **[In-World 3D Podiums](features/podiums.md)** | Create physical podium statues with ArmorStands, Citizens, or FancyNpcs. |
| **[GUI Customization](features/guis.md)** | Customize menus, layouts, pagination, and player profile inspection. |
| **[Seasons & Tournaments](features/seasons.md)** | Schedule tournaments, configure winner rewards, and view the Hall of Fame. |
| **[Commands & Permissions](reference/commands.md)** | Complete reference table of all player and administrator commands. |
| **[PlaceholderAPI Reference](reference/placeholders.md)** | Full list of placeholders for scoreboards, chat, and menus. |
| **[FAQ & Troubleshooting](reference/faq.md)** | Answers to common setup questions and troubleshooting tips. |

---

## 💡 Built for Production Servers

BlueHellStats is crafted to give server owners peace of mind:
1. **Uncompromising Performance:** Player stat changes are batched safely in memory and saved asynchronously in the background. Disk and database operations will never freeze your main tick loop.
2. **Rock-Solid Data Safety:** Even during sudden disconnects or server restarts, all pending player stats are committed cleanly to storage so progress is never lost.
3. **Hardened Exploitation Shields:** Virtual menus enforce comprehensive anti-dupe checks that cancel illicit clicks, hotbar number swaps, off-hand swaps, and fast-click macro spam.
4. **Seamless Multi-Server Scalability:** Scale from a single survival server to a multi-proxy network with real-time leaderboard synchronization across all your server nodes.
