
# Hold My Items

**Hold My Items** is a **first-person animation mod for Minecraft 1.21.5**, allowing resource packs to define fully custom animations using JavaScript. Whether you're making swords feel more impactful, tools look more realistic, or just creating stylish motion effects — this mod gives you full control over how items and hands animate in first person.

## ✨ Features

- 🎬 JavaScript-based animation scripting
- 🧰 Built-in math, player, and item helpers
- 🧠 Persistent variables (float, boolean, int, item)
- 📦 Fully resourcepack-driven (no modding required for new animations)
- 👐 Supports separate hand and item animations
- 🔁 Supports easing functions for smooth transitions

---

## 📁 Resource Pack Structure

To define your custom animations, include the following files in a resource pack:

```
assets/minecraft/holdmyitems/hand_pose.js
assets/minecraft/holdmyitems/item_pose.js
```

These scripts will be automatically loaded and run during first-person item rendering.

---

## 📜 Scripting API

The mod uses JavaScript (via GraalVM) to define animations. Scripts are executed each frame and are provided with the following **bindings** and **utility classes**:

### ✅ Provided Bindings

| Name              | Type                     | Description |
|-------------------|--------------------------|-------------|
| `matrices`        | `MatrixStack`            | Used to apply translations, rotations, and scaling |
| `deltaTime`       | `float`                  | delta time |
| `equipProgress`   | `float`                  | 0.0–1.0 progress of item equip animation |
| `swingProgress`   | `float`                  | 0.0–1.0 progress of item swing animation |
| `item`            | `ItemStack`              | Currently rendered item |
| `hand`            | `Hand`                   | `main_hand` or `off_hand` |
| `mainHand`        | `boolean`                | `true` if the item is in the main hand |
| `bl`        | `boolean`                | `true` if current hand is right |
| `player`          | `AbstractClientPlayerEntity` | Reference to the local player |
| `registry`        | `Map<String, Float>`     | For defining persistent float variables via `global.varName = value;` |

### 🧠 Script Helpers

#### `M` – Math & Matrix Helpers

```java
M.scale(matrices, x, y, z);
M.moveX(matrices, amount);
M.rotateY(matrices, degrees);
M.sin(angleRadians); // etc.
```

Includes:

- Basic trig: `sin`, `cos`, `abs`, `floor`, `ceil`, `round`, `pow`, `clamp`
- Transforms: `scale`, `moveX/Y/Z`, `rotateX/Y/Z` (with and without pivot)

---

## 📘 `P` — Player Wrapper

The `P` class is a utility that exposes **player-related properties and states** in a script-friendly way. These methods help you query player movement, position, state (like sneaking or swimming), view rotation, and equipped items.

### Methods

#### `boolean isSneaking(player)`
Returns `true` if the player is sneaking (crouching).

#### `boolean isOnGround(player)`
Returns `true` if the player is currently on the ground.

#### `boolean isSwimming(player)`
Returns `true` if the player is swimming (in water and using swimming mechanics).

#### `boolean isClimbing(player)`
Returns `true` if the player is currently climbing (e.g., on a ladder or vine).

#### `boolean isCrawling(player)`
Returns `true` if the player is in crawling pose (triggered by 1-block height gaps or trapdoors).

#### `boolean isSubmergedInWater(player)`
Returns `true` if the player is fully submerged in water (e.g., for underwater swimming).

### Position Methods

These return the player’s world position:

- `double getX(player)` – Returns the X coordinate
- `double getY(player)` – Returns the Y coordinate
- `double getZ(player)` – Returns the Z coordinate

### Velocity Methods

Useful for movement-based animations:

- `double getXSpeed(player)` – Horizontal X velocity
- `double getYSpeed(player)` – Vertical Y velocity
- `double getZSpeed(player)` – Horizontal Z velocity
- `double getSpeed(player)` – Combined movement speed (vector length)

### Action and State

- `boolean isUsingItem(player)` – Returns `true` if the player is holding and using an item (e.g., eating, charging a bow)
- `Hand getActiveHand(player)` – Returns the hand (`main_hand` or `off_hand`) used in the active item use

### View Rotation

- `double getYaw(player)` – Player’s head yaw (horizontal rotation)
- `double getPitch(player)` – Player’s head pitch (vertical rotation)

### Held Items

- `ItemStack getMainItem(player)` – Item in main hand
- `ItemStack getOffhandItem(player)` – Item in off-hand

---

## 📘 `I` — Item Stack Wrapper

The `I` class is a helper utility for querying **item stack properties** such as type, tags, action, and name.

### Methods

#### `boolean isOf(itemStack, item)`
Returns `true` if the `ItemStack` is of the given `Item`.

```js
I.isOf(item, Items.get("minecraft:diamond_sword"));
```

#### `boolean isIn(itemStack, tag)`
Returns `true` if the item is part of a given tag group.

```js
I.isIn(item, ItemTags.SWORDS);
```

Tags can also include custom or conventional tags loaded from datapacks or mods.

#### `boolean isEmpty(itemStack)`
Returns `true` if the item stack is empty.

#### `String getUseAction(itemStack)`
Returns the item's use animation as a string (e.g., `"eat"`, `"drink"`, `"bow"`).

```js
if (I.getUseAction(item) === "bow") {
  // Animate bow drawing
}
```

#### `String getName(itemStack)`
Returns the localized display name of the item as a string.

#### `boolean isChargedCrossbow(itemStack)`
Returns `true` if the item is a **crossbow** and is currently **charged** (i.e., ready to fire).


Useful for checking tags, use state, or item identity.

---

## `Items` – Item Registry

```java
Items.get("minecraft:diamond_sword")
```

Retrieves an `Item` instance by namespaced ID.

---
## `ItemTags`, `ConventionalItemTags`

For checking standard item tags or custom tag groups.

---
## `Easings`

Contains various easing functions (e.g., `easeInOutQuad`, `easeOutSine`) for smooth transitions.

---

## 💾 Persistent Variables

Define persistent values inside scripts:

```js
global.swingOffset = 0;
global.isCharging = false;
```

Example usage
```js
global.idle = 0;
idle += 0.1 * deltaTime;
M.rotateX(matrices,  3 * M.sin(idle));
```

These values persist across frames and reloads, allowing stateful animations.

You can reference and mutate them freely.


## 📦 Installation

1. Install [Fabric](https://fabricmc.net/) for Minecraft `1.21.5`.
2. Download and add your received beta build of `Hold My Items` to your `mods/` folder.
3. Add or create a resource pack with your animation scripts.

---

## 🧪 License

MIT License — free to use and modify.

---

## 📣 Credits

Created by ```sapling```.  
Thanks to the FabricMC and Minecraft modding communities!
