# Guía de Gobernanza Arquitectónica y Estándares de Ingeniería

> **Proyecto:** BlueHellEssence  
> **Plataforma:** Paper 1.21.1+ (Java 21)  
> **Estándar:** Clean Architecture Pragmática & Concurrencia Asíncrona  

---

## 1. Principios Inmutables del Proyecto

1. **El Hilo Principal (Tick Loop de 20 TPS) es Sagrado:**
   - Cero I/O (JDBC, archivos, HTTP, cómputo masivo) en el hilo principal de Bukkit.
   - Cualquier interacción con bases de datos debe ejecutarse en el pool secundario (`BlueHell-DB-Worker`) mediante `CompletableFuture<T>`.

2. **Conceptos antes que Código (KISS & YAGNI):**
   - No implementar complejidad accidental (parsers innecesarios, abstracciones dogmáticas 1-a-1 sin polimorfismo real).
   - El código debe ser comprensible, modular y directo.

3. **Inmutabilidad y Java 21:**
   - Todo modelo de datos que represente un estado congelado debe ser un **Java Record** (`StatDefinition`, `LeaderboardEntry`, `Milestone`).
   - Prohibido retener referencias a entidades vivas de Bukkit (`Player`, `Entity`) dentro de colecciones persistentes. Usar siempre `UUID`.

---

## 2. Mapa de Capas y Responsabilidades

```
com.dev.bluehell
├── core/
│   ├── condition/           # Validadores desacoplados de condiciones (StatConditionValidator)
│   ├── config/              # Lectura y tipado estricto de YAMLs (ConfigManager, DatabaseConfig)
│   ├── database/            # Conexión HikariCP, schema migrations y lifecycle (DatabaseManager)
│   ├── event/               # Despachador dinámico y MethodHandles precompilados (DynamicEventRegistry)
│   └── message/             # Internacionalización MiniMessage desacoplada (MessageService)
├── gui/                     # Capa de presentación: Holders, Menús YAML, Paginador y Anti-Dupe
├── model/                   # Records inmutables y Enums de dominio puro (PlayerProfile, StatDefinition)
├── podium/                  # Proveedores de estatuas desacoplados (ArmorStand, Citizens, FancyNpcs)
├── season/                  # Sub-dominio de torneos estacionales, rollover y bloqueo distribuido
├── storage/                 # Puerto (StatRepository) y Adaptador ANSI SQL (SqlStatRepository)
├── service/                 # Servicios de negocio, cache en RAM y write-behind (StatService, PlayerProfileService)
├── listener/                # Thin Listeners: validación rápida y delegación a servicios
├── command/                 # Manejo modular de comandos y autocompletado (TopCommand, AdminStatCommand)
└── hook/                    # Integraciones opcionales (WorldGuard, DecentHolograms, PlaceholderAPI)
```

### Reglas de Dependencia entre Capas:
- **`model`** no depende de nadie.
- **`storage`** solo interactúa con `DatabaseManager` y expone métodos que retornan `CompletableFuture`.
- **`service`** inyecta dependencias por constructor. Nunca usa `getInstance()` estáticos.
- **`listener`** es delgado: cancela eventos inválidos, extrae parámetros y delega al servicio.

---

## 3. Protocolo de Persistencia y Concurrencia (Write-Behind Cache)

Para garantizar 0 micro-lags en servidores de alto tráfico:

1. **Mutación en Caliente:**
   - Cuando un jugador incrementa una estadística, la mutación ocurre en memoria RAM (`playerStatsCache`) en menos de 10 nanosegundos.
   - El delta acumulado se registra en `dirtyDeltas`.

2. **Drenado Periódico Asíncrono:**
   - Un scheduler asíncrono toma un snapshot de `dirtyDeltas` cada $N$ segundos y ejecuta un `executeBatch()` transaccional en PostgreSQL/SQLite.
   - Si un jugador se desconecta (`PlayerQuitEvent`), sus deltas pendientes se descargan de inmediato y su sesión en RAM se limpia para evitar memory leaks.

3. **Cierre Seguro (`onDisable`):**
   - En el apagado del servidor, `StatService.shutdown()` realiza el volcado de los deltas restantes, el `ExecutorService` espera a que las tareas pendientes terminen (`awaitTermination`) y finalmente se cierra el pool de `HikariDataSource`.

---

## 4. Gobernanza para Añadir Nuevas Estadísticas y Eventos

Cuando se desee incorporar un nuevo tipo de evento (ej. daño infligido, pociones lanzadas, tiempo jugado):

1. **Enum:** Agregar la constante en `model/StatEventType.java`.
2. **Listener:**
   - Abrir `listener/DynamicStatListener.java`.
   - Crear el método `@EventHandler(priority = EventPriority.MONITOR, ignoreCancelled = true)`.
   - Validar las condiciones específicas (ej. tipo de poción, daño mínimo).
   - Invocar `statService.recordIncrement(player, def, amount)`.
3. **Documentación:** Actualizar `stats.yml` con el ejemplo comentado.

---

## 5. Reglas de Git y Commits

- Utilizar **Conventional Commits**:
  - `feat: add support for player damage stat event`
  - `fix: resolve stale delta flush on unexpected server stop`
  - `refactor: extract requirement evaluation logic`
- **Cero atribuciones de IA o co-authored-by en los commits.**

