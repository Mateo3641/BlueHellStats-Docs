# ⚡ BlueHellStats Documentation

Welcome to the official documentation for **BlueHellStats** (also known as *BlueHellEssence*), the enterprise-grade statistics tracking, leaderboard visualization, in-world 3D podium, and automated seasonal tournament engine designed exclusively for Paper 1.21+ Minecraft servers.

---

## 🧭 Navigation Guide

Explore our comprehensive guides below or use the navigation tabs above:

| Section | Description |
| :--- | :--- |
| **[Quick Start](getting-started/quick-start.md)** | Prerequisites, installation, and first-time server boot guide. |
| **[Database Configuration](getting-started/database.md)** | Setup SQLite for single servers or MySQL/MariaDB/PostgreSQL for multi-server proxy networks. |
| **[Statistics Configuration](features/stats.md)** | Declare custom stat categories, tracking events, WorldGuard regions, multipliers, and milestones. |
| **[Dynamic Custom Events](features/custom-events.md)** | Zero-overhead dynamic event interception engine powered by precompiled JVM `MethodHandle` chains. |
| **[In-World 3D Podiums](features/podiums.md)** | Setup physical podium statues using Paper ArmorStands, Citizens2, or FancyNpcs with floating billboards. |
| **[GUI Customization](features/guis.md)** | Freeform slot mapping, hardened anti-dupe security, and player profile inspection menus (`menus/`). |
| **[Seasons & Tournaments](features/seasons.md)** | Automated season duration, Two-Phase Rollover Barrier, reward queuing, and Hall of Fame browsing. |
| **[Commands & Permissions](reference/commands.md)** | Complete reference of administrator, player, season, and podium commands and permission nodes. |
| **[PlaceholderAPI Reference](reference/placeholders.md)** | Complete table of all $O(1)$ memory-cached placeholders for scoreboards, chat, and menus. |
| **[Architecture & Governance](reference/governance.md)** | Deep architectural guidelines, clean architecture layering, and write-behind concurrency patterns. |
| **[Guía Paso a Paso (Español)](reference/walkthrough-es.md)** | Tutorial detallado de configuración, despliegue y personalización en español. |

---

## 💡 Architecture & Philosophy

BlueHellStats is engineered following **Pragmatic Clean Architecture**:
1. **The Bukkit Tick Loop (20 TPS) is Sacred:** All disk I/O, network communication, database reads/writes, dirty delta flushes, and rank sortings are strictly offloaded to asynchronous background worker pools.
2. **Immutability by Default:** All domain entities are represented by immutable **Java 21 Records** (`StatDefinition`, `PlayerProfile`, `LeaderboardEntry`, `PodiumConfig`).
3. **Hardened Security:** Anti-dupe GUI pipelines cancel events at `HIGHEST` priority, blocking hotbar swapping, item injection, drag duplication, and macro spam.
4. **Resilient Network Synchronization:** Two-phase distributed lock barrier prevents race conditions and duplicate reward deliveries across multi-server proxy environments.

