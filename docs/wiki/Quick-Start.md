# 🚀 Quick Start Guide

This guide walks you through installing and configuring BlueHellStats on a Paper 1.21+ server.

---

## 📋 Prerequisites

* **Server Platform:** [Paper](https://papermc.io), Purpur, or any modern Paper fork running **Minecraft 1.21.1+**.
* **Java Runtime:** **Java 21** or newer (OpenJDK / Temurin 21 recommended).
* **Optional Integrations:**
  * **[PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/)** (for chat, scoreboard, and tablist placeholders)
  * **[Citizens](https://www.spigotmc.org/resources/citizens.13812/)** or **[FancyNpcs](https://modrinth.com/plugin/fancynpcs)** (for in-world 3D player podium NPCs)
  * **[DecentHolograms](https://www.spigotmc.org/resources/96927/)** (for floating spawn leaderboard holograms)
  * **[WorldGuard](https://dev.bukkit.org/projects/worldguard)** (for region-specific stat tracking like PvP arenas)
  * **[Vault](https://www.spigotmc.org/resources/vault.34315/)** (for economy commands in seasonal rewards and milestones)

---

## 📦 Installation Steps

1. **Download:** Obtain the latest `BlueHellEssence-1.3.0.jar` from [Releases](https://github.com/Mateo3641/BlueHellStats/releases).
2. **Deploy:** Place the `.jar` into your server's `plugins/` folder.
3. **Boot Server:** Start your server once to allow BlueHellStats to create its configuration folder and default files:
   ```
   plugins/BlueHellEssence/
   ├── config.yml
   ├── seasons.yml
   ├── stats.yml
   ├── podiums.yml
   ├── messages/
   │   ├── messages_en.yml
   │   └── messages_es.yml
   └── menus/
       ├── hub.yml
       ├── top.yml
       └── profile.yml
   ```
4. **Choose Database:** By default, BlueHellStats runs on **SQLite** (`bluehell.db`), requiring zero setup. If you run a network, see [[Database-Configuration]].
5. **Reload Configs:** Run `/bhe reload` to apply any changes in real time without restarting the server.

