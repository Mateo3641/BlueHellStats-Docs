# ⚙️ Database Configuration

BlueHellStats provides a high-performance, asynchronous database engine powered by enterprise-grade **HikariCP** connection pooling and automatic schema management.

---

## 🗄️ Supported Database Engines

Configure your storage engine in `plugins/BlueHellEssence/config.yml`:

### 1. SQLite (Default — Single Server / Local)
Ideal for single-server setups, survival servers, or testing environments. Requires zero setup or external services.

```yaml
locale: "en"

database:
  type: SQLITE
  sqlite:
    file: "bluehell.db"
```

### 2. MySQL / MariaDB (Multi-Server Network / Bungee / Velocity)
Ideal for networks hosted across multiple nodes, shared dedicated servers, or managed via phpMyAdmin/Pterodactyl.

```yaml
locale: "en"

database:
  type: MYSQL # or MARIADB
  mysql:
    host: "127.0.0.1"
    port: 3306
    database: "network_stats"
    username: "minecraft"
    password: "your_strong_password"
    ssl: false

sync:
  server-id: "" # Leave empty to auto-generate a unique server ID per node
  poll-interval-ms: 1000
  flush-interval-seconds: 20
  leaderboard-refresh-seconds: 60
  leaderboard-top-size: 10
```

### 3. PostgreSQL (Enterprise High-Performance)
Recommended for massive network scales requiring ultra-low latency, robust connection pooling, and heavy concurrent player traffic.

```yaml
locale: "en"

database:
  type: POSTGRESQL
  postgresql:
    host: "10.0.0.5"
    port: 5432
    database: "network_stats"
    username: "postgres_user"
    password: "your_strong_password"
    ssl: false
```

---

## ⚡ Performance & Data Safety Guarantees

To ensure that your server runs smoothly with consistent 20.0 TPS even during high player activity:

* **Asynchronous Execution:** Database operations, connection handshakes, and queries never run on your main server thread. Your gameplay and tick rates remain completely unaffected.
* **Smart Memory Caching:** Stat increments are tracked instantly in memory and batched before saving, minimizing database disk writes and network load.
* **Zero Stat Loss on Disconnect:** Whenever a player leaves the server, any uncommitted progress is immediately committed to storage in the background.
* **Safe Server Shutdowns:** When the server stops or restarts, BlueHellStats ensures all pending stat saves are safely finalized before releasing database connections.
