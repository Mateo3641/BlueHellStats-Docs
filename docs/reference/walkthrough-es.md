# Guía de Uso, Configuración y Despliegue (Walkthrough)

> **Plugin:** BlueHellEssence  
> **Versión:** 1.3.0  
> **Requisitos:** Paper 1.21.1+ | Java 21 | SQLite, MySQL/MariaDB o PostgreSQL  

---

## 1. Instalación Rápida

1. Compilar el proyecto ejecutando:
   ```bash
   mvn clean package
   ```
2. Copiar el archivo generado en `target/BlueHellEssence-1.3.0.jar` dentro de la carpeta `plugins/` de tu servidor Paper.
3. Iniciar el servidor para que se generen los archivos `config.yml`, `stats.yml`, `seasons.yml` y la carpeta `menus/`.

---

## 2. Configuración de Base de Datos (`config.yml`)

### Modo Servidor Individual (SQLite - Local)
Por defecto, sin necesidad de instalar ningún servidor de base de datos externo:
```yaml
database:
  type: SQLITE
  sqlite:
    file: "bluehell.db"
```

### Modo Servidor o Network con MySQL / MariaDB (phpMyAdmin / cPanel / Pterodactyl)
Recomendado para servidores de hosting tradicionales y networks BungeeCord / Velocity:
```yaml
database:
  type: MYSQL # o MARIADB
  mysql:
    host: "localhost"
    port: 3306
    database: "bluehell"
    username: "root"
    password: "tu_password_segura"
    ssl: false
```

### Modo Network Empresarial con PostgreSQL (Alto Rendimiento)
Para networks grandes con sincronización instantánea LISTEN/NOTIFY y bloqueos distribuidos:
```yaml
database:
  type: POSTGRESQL
  postgresql:
    host: "10.0.0.5"
    port: 5432
    database: "bluehell"
    username: "network_user"
    password: "tu_password_segura"
    ssl: false
```

---

## 3. Configuración Dinámica de Estadísticas (`stats.yml`)

Cada estadística es declarativa y altamente personalizable:

```yaml
stats:
  diamonds_mined:
    display-name: "<aqua>Diamantes Picados</aqua>"
    event: BLOCK_BREAK
    increment: 1
    cooldown-ms: 0
    conditions:
      materials:
        - DIAMOND_ORE
        - DEEPSLATE_DIAMOND_ORE
      game-modes:
        - SURVIVAL
      ignore-silk-touch: true # No cuenta si se pica con Toque de Seda
      world-mode: WHITELIST
      worlds:
        - world
    multipliers:
      "bluehell.multiplier.vip": 2.0 # Si el jugador tiene este permiso, suma x2
    milestones:
      - threshold: 100
        message: "<green>¡Alcanzaste 100 diamantes picados!</green>"
        broadcast: "<gold>¡<player> alcanzó el hito de 100 diamantes picados!</gold>"
        commands:
          - "give %player% diamond 5"
    season-rewards:
      1:
        - "eco give %player% 25000"
        - "give %player% diamond_block 10"
        - "broadcast <gold>[TEMPORADA]</gold> <yellow>%player%</yellow> dominó la minería con <aqua>%score%</aqua> diamantes!"
      2:
        - "eco give %player% 15000"
      3:
        - "eco give %player% 7500"

  pvp_kills:
    display-name: "<red>Asesinatos PvP</red>"
    event: ENTITY_KILL
    increment: 1
    conditions:
      entities:
        - PLAYER
      game-modes:
        - SURVIVAL
      regions: # Opcional: filtrado por regiones de WorldGuard
        - arena_pvp
    season-rewards:
      1:
        - "eco give %player% 50000"
      2:
        - "eco give %player% 25000"
      3:
        - "eco give %player% 10000"

  # Interceptar eventos de plugins externos sin dependencias en compilación
  turrets_placed:
    display-name: "<yellow>Torretas Colocadas</yellow>"
    event: CUSTOM
    increment: 1
    custom_event_data:
      event: "dt.ajneb97.api.TurretPlaceEvent"
      player_variable: "getPlayer()"
      variables_to_capture:
        - "%world%;getLocation().getWorld().getName()"
      conditions:
        - "%world% equals world"

# Requisitos Combinados (Booleanos)
requirements:
  nether_warrior:
    pvp_kills: 50
    diamonds_mined: 20
```

---

## 4. Referencia de Placeholders (PlaceholderAPI)

Todas las consultas leen directo de la memoria RAM del servidor ($O(1)$) y **nunca congelan los TPS**:

| Placeholder | Descripción | Ejemplo de Salida |
| :--- | :--- | :--- |
| `%bluehell_stat_<stat_id>%` | Valor numérico exacto del jugador | `1450` |
| `%bluehell_stat_formatted_<stat_id>%` | Valor formateado con separadores de miles | `1,450` |
| `%bluehell_rank_<stat_id>%` | Puesto del jugador en el ranking | `#3` (o `N/A`) |
| `%bluehell_top_<stat_id>_<pos>_name%` | Nombre del jugador en la posición `<pos>` | `Steve` |
| `%bluehell_top_<stat_id>_<pos>_value%` | Puntaje del jugador en la posición `<pos>` | `500` |
| `%bluehell_top_<stat_id>_<pos>_value_formatted%` | Puntaje formateado en la posición `<pos>` | `500` |
| `%bluehell_has_<stat_id>_<minimo>%` | Comprobación booleana inmediata | `true` o `false` |
| `%bluehell_req_<requirement_id>%` | Comprobación de requisito combinado | `true` o `false` |
| `%bluehell_season_current_id%` | ID numérico de la temporada activa | `1` |
| `%bluehell_season_current_name%` | Nombre de la temporada activa | `Temporada 1` |
| `%bluehell_season_time_remaining%` | Tiempo restante para el cierre de temporada | `14d 5h` o `Terminada` |
| `%bluehell_history_<season>_<stat>_<puesto>_name%` | Ganador del podio en una temporada pasada | `Steve` |
| `%bluehell_history_<season>_<stat>_<puesto>_score%` | Puntaje final del podio en temporada pasada | `2500` |
| `%bluehell_history_<season>_<stat>_<puesto>_score_formatted%` | Puntaje formateado del podio en temporada pasada | `2,500` |

---

## 5. Comandos de Administrador

Permiso requerido: `bluehell.admin` (por defecto asignado a operadores).

- **`"add"`**: Suma una cantidad a la estadística del jugador.  
  `/bhe add <jugador> <stat> <cantidad>`
- **`"set"`**: Asigna un valor exacto a la estadística del jugador.  
  `/bhe set <jugador> <stat> <valor>`
- **`"reset"`**: Restablece la estadística del jugador a 0.  
  `/bhe reset <jugador> <stat>`
- **`"profile"`**: Abre el menú GUI de inspección de perfil multi-estadística de cualquier jugador.  
  `/bhe profile [jugador]`
- **`"podium create"`**: Genera un podio 3D en las coordenadas y orientación del admin.  
  `/bhe podium create <stat> <1|2|3> [id]`
- **`"podium remove"`**: Elimina un podio específico por su ID o todos los podios de una stat.  
  `/bhe podium remove <id|stat>`
- **`"podium list"`**: Lista todos los podios físicos registrados y sus coordenadas.  
  `/bhe podium list`
- **`"podium refresh"`**: Fuerza el refresco visual inmediato de los podios en el mundo.  
  `/bhe podium refresh [id]`
- **`"holo create"`**: Genera un holograma flotante de tabla de clasificación con DecentHolograms.  
  `/bhe holo create <stat> [filas] [nombre]`
- **`"holo remove"`**: Elimina un holograma por nombre o estadística.  
  `/bhe holo remove <nombre|stat>`
- **`"reload"`**: Recarga los archivos de configuración (`config.yml`, `stats.yml`, `seasons.yml`, `menus/*.yml`) y recalcula las tablas de clasificación en caliente.  
  `/bhe reload`

### Comandos de Temporadas (`/season`)
Permiso administrativo: `bluehell.season.admin`

- **`/season status`**: Muestra el estado de la temporada actual y cuenta regresiva.
- **`/season start <nombre> [duración]`**: Inicia una nueva temporada (ej. `/season start "Verano 2026" 30d`).
- **`/season end`**: Finaliza la temporada activa.
- **`/season rollover [nombre] [duración]`**: Ejecuta el cierre de temporada en 2 fases con cálculo de podios, entrega de premios Top-3 y apertura de la siguiente temporada.
- **`/season claim`**: Reclama manualmente las recompensas estacionales pendientes (también se reclaman solas al entrar al servidor).

---

## 6. Comandos de Jugador

- **`/top`**: Abre el Salón de la Fama (Hub general de categorías configuradas en `menus/hub.yml`).
- **`/top <stat_id>`**: Salta directamente a la visualización de la estadística especificada (ej. `/top diamonds_mined`).
- **`/top profile [jugador]`**: Abre la GUI de perfil personal o de otro jugador mostrando todas sus estadísticas y rangos actuales (`bluehell.profile`).
- **`/halloffame` (alias `/hof`)**: Abre el navegador interactivo de temporadas pasadas y podios históricos.

---

## 7. Configuración de GUIs con Mapeo Libre de Slots

Las interfaces se gestionan en la carpeta `plugins/BlueHellEssence/menus/`:

### `menus/hub.yml` (Selector de Categorías)
Cada ítem tiene su `slot:` exacto y una acción (`OPEN_STAT:<id>`, `CLOSE`, etc.):
```yaml
title: "<dark_gray>Salón de la Fama - Categorías</dark_gray>"
rows: 4
items:
  diamonds_category:
    slot: 11
    material: DIAMOND
    action: OPEN_STAT:diamonds_mined
    name: "<aqua><bold>Diamantes Picados</bold></aqua>"
```

### `menus/top.yml` (Ranking con Podio Libre)
El administrador define la coordenada de cada puesto del ranking en `top_slots`:
```yaml
title: "<dark_gray>Top - <stat_display></dark_gray>"
rows: 5

# Podio y posiciones en cualquier slot
top_slots:
  1: 13  # Puesto 1 (centro arriba)
  2: 21  # Puesto 2 (izquierda)
  3: 23  # Puesto 3 (derecha)
  4: 29
  5: 30
  6: 31
  7: 32
  8: 33
  9: 39
  10: 41

items:
  info_book:
    slot: 4
    material: BOOK
    name: "<aqua><bold>Información</bold></aqua>"
  my_stats:
    slot: 40
    material: PLAYER_HEAD
    action: PLAYER_STATS
    name: "<gold>Tu Posición: <rank></gold>"
  back_button:
    slot: 36
    material: ARROW
    action: BACK
    name: "<red>« Volver al Hub</red>"
  close_button:
    slot: 44
    material: BARRIER
    action: CLOSE
    name: "<red>Cerrar Menú</red>"
```

### Protección Anti-Dupe Integrada:
1. **Cancelación Inmediata:** Todo click o arrastre (`InventoryClickEvent`, `InventoryDragEvent`) dentro de las GUIs se cancela en la máxima prioridad (`HIGHEST`).
2. **Bloqueo de Inyección:** Shift-click y hotbar keys (1-9) desde el inventario del jugador quedan terminantemente bloqueados.
3. **Debouncer Anti-Macro:** 150ms de enfriamiento por jugador para evitar abuso de autoclickers en transiciones de menús.

### `menus/profile.yml` (Perfil Detallado Multi-Estadística)
Permite inspeccionar todas las estadísticas de un jugador y sus posiciones relativas en el ranking con paginación genérica automática:
```yaml
title: "<dark_gray>Perfil: <player></dark_gray>"
rows: 6
stat_slots: [10, 11, 12, 13, 14, 15, 16, 19, 20, 21, 22, 23, 24, 25, 28, 29, 30, 31, 32, 33, 34]
previous_page_slot: 45
next_page_slot: 53
stat_item:
  material: BOOK
  name: "<yellow><stat_display></yellow>"
  lore:
    - "<gray>Puntaje:</gray> <white><score></white>"
    - "<gray>Posición en Ranking:</gray> <gold><rank></gold>"
```

---

## 8. Podios Físicos 3D en el Mundo (ArmorStand, Citizens & FancyNpcs)

El sistema de podios (`podiums.yml`) renderiza en tiempo real a los jugadores líderes del ranking sobre bloques físicos en el mundo:

* **Proveedores Soportados:**
  * `ARMOR_STAND`: Nativo de Paper 1.21 sin dependencias externas. Usa cabezas con texturas del jugador y armaduras con tintes de rango.
  * `CITIZENS`: Soporte completo para Citizens 2 con traits y `/npc pose`.
  * `FANCY_NPCS`: Soporte para FancyNpcs 2.10+ / 2.12+.
* **Cartel TextDisplay Billboard:**
  Todos los proveedores spawnan un `TextDisplay` flotante Paper 1.21 que mira automáticamente a los jugadores y muestra el puesto, nombre y puntaje.
* **Manejo de Poses en FancyNpcs:**
  En FancyNpcs las poses son atributos:
  ```bash
  /npc attribute bhe_podium_<stat>_<rank> set pose <standing|crouching|sleeping|swimming|sitting>
  ```
* **Compatibilidad Upstream:**
  BlueHellEssence incluye un hotfix en `FancyNpcsStatueProvider` que inyecta automáticamente el atributo faltante `custom_model` para evitar el `NullPointerException` interno de versiones 2.10.x.

---

## 9. Motor de Eventos Custom (Zero-Overhead Dynamic Events)

Permite interceptar eventos de cualquier plugin de terceros (ej. torretas, jobs, dungeons) sin necesidad de agregar dependencias Maven en compilación:

1. **Resolución en Arranque:** `MethodHandleChain` compila las rutas de métodos getter a `MethodHandle` de la JVM en el inicio del plugin.
2. **Cero Reflexión en Runtime:** Durante el juego, la invocación de métodos se ejecuta a velocidad directa de bytecode sin llamar a `Method.invoke()`.
3. **Registro Bukkit Nativo:** `DynamicEventRegistry` conecta dinámicamente con Bukkit usando un `EventExecutor` de alta prioridad.

---

## 10. Pruebas y Verificación

El proyecto cuenta con **301 pruebas unitarias e integrales** que garantizan:
- Persistencia asíncrona segura con HikariCP y transacciones batch.
- Ausencia de bloqueos o race conditions en el tick loop principal.
- Integridad anti-dupe en GUIs e inmutabilidad de modelos de dominio.

Ejecutar la suite completa:
```bash
mvn clean test
```

