# 🗿 In-World 3D Podiums Guide

BlueHellStats allows you to spawn physical 3D podium statues directly in the Minecraft world to honor top contenders in real time.

---

## 🎨 Supported Podium Providers

Configure the podium type per entry in `podiums.yml`:

```yaml
podiums:
  diamonds_mined_1:
    stat: "diamonds_mined"
    rank: 1
    type: FANCY_NPCS # Options: ARMOR_STAND, CITIZENS, FANCY_NPCS
    world: "world"
    x: 100.5
    y: 65.0
    z: -200.5
    yaw: 180.0
    pitch: 0.0
```

### 1. `ARMOR_STAND` (Native Paper 1.21+)
* **Dependencies:** None! Works out of the box on vanilla Paper servers.
* **Features:** Spawns an ArmorStand wearing the real top player's skin skull (retrieved automatically from the Mojang API), tier-themed armor (Gold for 1st, Iron for 2nd, Chainmail for 3rd), and a hand-held trophy item.
* **Protection:** Equipment and armor stands are marked invulnerable and tamper-proof.

### 2. `CITIZENS` (Citizens 2)
* **Dependencies:** [Citizens 2](https://www.spigotmc.org/resources/citizens.13812/).
* **Features:** Spawns a full NPC player model dynamically applying the top player's skin. Supports Citizens traits, equipment, and poses.

### 3. `FANCY_NPCS` (FancyNpcs 2.10+ & 2.12+)
* **Dependencies:** [FancyNpcs](https://modrinth.com/plugin/fancynpcs).
* **Features:** Lightweight packet-based player NPCs with zero server entity overhead.
* **Compatibility:** Fully compatible out of the box with Paper 1.21 and 1.21.1+.

---

## 🧘 Managing Poses with FancyNpcs

FancyNpcs allows you to customize the pose of any podium NPC in game:

```bash
/npc attribute bhe_podium_<stat>_<rank> set pose <pose>
```

Supported poses for Player NPCs:
* `standing` (Default upright posture)
* `crouching` (Sneaking posture)
* `sleeping` (Horizontal sleeping posture)
* `swimming` (Horizontal swimming posture)
* `sitting` (Mounted sitting posture)

**Example:**
```bash
/npc attribute bhe_podium_diamonds_mined_1 set pose crouching
```

---

## 🪧 Paper 1.21 TextDisplay Billboards

All podium providers spawn an overhead floating `TextDisplay` entity at $Y + 2.15$:
* Billboard mode is set to `CENTER`, ensuring the text smoothly rotates to face approaching players from any angle.
* Text displays the placement medal, player name, and formatted score:
  ```
  [#1] Steve
  1,450 Diamonds Mined
  ```

---

## 🖱️ Interactive Click Routing

When players punch or right-click any podium statue:
* Players are automatically routed directly to `/top <stat>` for that podium's category.
* Includes fast-click spam protection to ensure smooth client performance.
