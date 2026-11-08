# ⚙️ Database Configuration

BlueHellStats provides an asynchronous database layer powered by **HikariCP** connection pooling and automatic schema migrations.

---

## 🗄️ Supported Database Engines

Configure your storage engine in `plugins/BlueHellEssence/config.yml`:

### 1. SQLite (Default — Single Server / Local)
Ideal for single-server setups, survival servers, or local testing. Zero installation required.

```yaml
locale: "en"

database:
  type: SQLITE
  sqlite:
    file: "bluehell.db"
```

### 2. MySQL / MariaDB (Multi-Server Network / Bungee / Velocity)
Ideal for networks hosted across multiple nodes or managed via phpMyAdmin/Pterodactyl.

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
Recommended for massive network scale with ultra-low query latency and advanced connection pooling.

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

## ⚡ Write-Behind Caching Architecture

To guarantee that the Bukkit tick thread is never stalled:
1. **In-Memory Mutations:** When players break blocks, kill mobs, or trigger stats, their score delta is accumulated into a local concurrent RAM cache (`dirtyDeltas`) in nanoseconds.
2. **Periodic Batch Flushing:** Every `flush-interval-seconds` (default: 20s), an asynchronous background worker drains dirty deltas and sends batch SQL statements (`executeBatch()`) within a single database transaction.
3. **Session Flush on Disconnect:** When a player disconnects (`PlayerQuitEvent`), any pending uncommitted delta is immediately flushed asynchronously to ensure zero stat loss.
4. **Clean Server Shutdown:** On `onDisable()`, the plugin blocks gracefully until all pending database writes finish before closing the HikariCP connection pool.

