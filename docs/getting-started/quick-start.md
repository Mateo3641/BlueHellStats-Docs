# 🚀 Quick Start Guide

This guide walks you through installing and configuring BlueHellStats on your Paper 1.21+ server.

---

## 📋 Requirements

* **Server Platform:** [Paper](https://papermc.io), Purpur, or **Folia** (`folia-supported: true`) running **Minecraft 1.21 or 1.21.1+**.
* **Java Runtime:** **Java 21** or newer (OpenJDK / Temurin 21 recommended).
* **Optional Integrations:**
  * **[PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/)** (for chat, scoreboard, and tablist placeholders)
  * **[Citizens](https://www.spigotmc.org/resources/citizens.13812/)** or **[FancyNpcs](https://modrinth.com/plugin/fancynpcs)** (for in-world 3D player podium NPCs)
  * **[DecentHolograms](https://www.spigotmc.org/resources/96927/)** (for floating spawn leaderboard holograms)
  * **[WorldGuard](https://dev.bukkit.org/projects/worldguard)** (for region-specific stat tracking like PvP arenas)
  * **[Vault](https://www.spigotmc.org/resources/vault.34315/)** (for economy commands in seasonal rewards and milestones)

---

## 📦 Installation

1. **Deploy:** Place the downloaded `.jar` file into your server's `plugins/` directory.
2. **Start Server:** Start your server to generate the default configuration files:
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
3. **Storage Setup:** By default, BlueHellStats uses high-speed **SQLite** (`bluehell.db`), requiring zero initial configuration. For multi-server proxy networks, see [Database Configuration](database.md).
4. **Live Reload:** Modify your YAML files and run `/bhe reload` to apply updates instantly without restarting your server.
