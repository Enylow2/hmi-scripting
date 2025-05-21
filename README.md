# 📦 Hold My Items – Animation Script Documentation

**Mod Name**: Hold My Items  
**Author**: sapling.
**Description**: Customize first-person hand and item animations via resource packs using `.hmi` scripts and simple Java-based logic.

---

## 📁 Script Locations

Create or edit the following files in your resource pack:

| File Path                                              | Purpose                      |
|--------------------------------------------------------|------------------------------|
| `assets/minecraft/holdmyitems/hand_pose.hmi`           | Controls hand transformations |
| `assets/minecraft/holdmyitems/item_pose.hmi`           | Controls item transformations |

Scripts run every frame and apply transformations via a `MatrixStack`.

---

## 🧠 Java Syntax Primer

Scripts are written in a simplified Java-like syntax:

### ➕ Variables

```java
float swing = 0.3;
```

### 🧾 Function Calls

```java
M.moveX(matrices, 0.2);
```

### 🔁 Conditionals

```java
if (P.isSwimming(player)) {
    M.moveY(matrices, -0.2);
}
```

---

## ⏱ Getting deltaTime

To create smooth, frame-rate independent animations, use `HoldMyItems.getDeltaTime()`:

```java
float delta = HoldMyItems.getDeltaTime();
```

This returns the time (in seconds) since the last frame. Use it for time-based motion:

```java
float t = floatRegistry.getOrDefault("timer", 0.0);
t += delta;
floatRegistry.put("timer", t);
```

---

## 💡 Matrix Transforms (`M`)

These functions manipulate the item/hand model:

```java
M.moveX(MatrixStack matrices, float amount);
M.moveY(MatrixStack matrices, float amount);
M.moveZ(MatrixStack matrices, float amount);

M.rotateX(MatrixStack matrices, float degrees);
M.rotateY(MatrixStack matrices, float degrees);
M.rotateZ(MatrixStack matrices, float degrees);
```

### Example

```java
M.moveY(matrices, 0.1);
M.rotateZ(matrices, 30.0);
```

---

## 🧮 Math Utilities (`M`)

| Function                          | Description                            |
|----------------------------------|----------------------------------------|
| `M.sin(float radians)`           | Sine of angle                          |
| `M.cos(float radians)`           | Cosine of angle                        |
| `M.clamp(float val, min, max)`   | Clamp a value                          |
| `M.floor(float val)`             | Floor (round down)                     |
| `M.abs(float val)`               | Absolute value                         |
| `M.lerp(float t, start, end)`    | Linear interpolation                   |

```java
float wave = M.sin(t * 2.0) * 0.1;
M.moveY(matrices, wave);
```

---

## 🎚 Easing Functions

Use for smooth animations:

```java
float eased = Easings.easeInOutBack(progress);
```

| Function                        | Description                           |
|--------------------------------|---------------------------------------|
| `Easings.easeInOutBack(x)`     | Smooth ease-in/out with overshoot     |

`x` must be in the range `0.0` to `1.0`.

---

## 👤 Player State (`P`)

Check movement, posture, and status of the player.

### Movement & Actions

```java
P.isSwimming(player);          // boolean
P.isClimbing(player);
P.isCrawling(player);
P.isSubmergedInWater(player);
P.isUsingItem(player);
```

### Position & Velocity

```java
P.getX(player);                // float
P.getY(player);
P.getZ(player);

P.getXSpeed(player);          // float
P.getYSpeed(player);
P.getZSpeed(player);
P.getSpeed(player);           // overall velocity
```

### Rotation

```java
P.getYaw(player);             // head yaw (horizontal)
P.getPitch(player);           // pitch (vertical)
```

---
🧱 Item Utilities (I)
The I class provides functions to inspect the item currently being rendered. You can use it to check if the player is holding a specific item or something from a tag group.

✅ Checking Item Type
java
I.isOf(itemStack, Items.DIAMOND_SWORD);
Returns true if the item is exactly a Diamond Sword.

🏷 Checking Item Tags
java
I.isIn(itemStack, ItemTags.SWORDS);
Returns true if the item is in the #swords tag.

❌ Checking if Hand is Empty
java
I.isEmpty(itemStack);
Returns true if the player’s hand is empty.

🔍 Example Use
java
if (I.isOf(itemStack, Items.SHIELD)) {
    M.moveZ(matrices, -0.2);
    M.rotateY(matrices, 15);
}

if (I.isIn(itemStack, ItemTags.FLOWERS)) {
    M.moveY(matrices, M.sin(P.getPitch(player) * 0.1) * 0.1);
}

if (I.isEmpty(itemStack)) {
    M.rotateZ(matrices, 45);
}
📦 Item and Tag References
Items class contains every item in the game:

java
Items.APPLE, Items.CARROT, Items.CROSSBOW, ...
ItemTags contains built-in tag groups:

java
ItemTags.SWORDS, ItemTags.LOGS, ItemTags.BANNERS, ...


## 📂 Float Registry

Persist and reuse values across frames:

```java
float timer = floatRegistry.getOrDefault("myTimer", 0.0);
timer += HoldMyItems.getDeltaTime();
floatRegistry.put("myTimer", timer);
```

Use it for:
- Time tracking
- Animation state
- Interpolation progress

---

## 🧪 Example Animation

```java
float t = floatRegistry.getOrDefault("bob", 0.0);
t += HoldMyItems.getDeltaTime();
floatRegistry.put("bob", t);

float bobOffset = M.sin(t * 2.0) * 0.1;
M.moveY(matrices, bobOffset);

if (P.isUsingItem(player)) {
    M.rotateX(matrices, 20);
}
```

---

## 🔁 Looping Eased Animation

```java
float t = floatRegistry.getOrDefault("time", 0.0);
t += HoldMyItems.getDeltaTime() * 0.5;
t = t % 1.0; // Keep it looping
floatRegistry.put("time", t);

float eased = Easings.easeInOutBack(t);
M.moveZ(matrices, eased * -0.2);
```

---

## 📚 Summary Table

| Class        | Use                                  |
|--------------|--------------------------------------|
| `M`          | Move/rotate, math functions          |
| `P`          | Get player state                     |
| `Easings`    | Easing functions                     |
| `HoldMyItems.getDeltaTime()` | Frame-independent time  |
| `floatRegistry` | Persistent animation state         |

---

## 📦 Distributing Resource Packs

To publish your animations:
1. Place `.hmi` scripts in a resource pack under `assets/minecraft/holdmyitems/`
2. Include both `hand_pose.hmi` and `item_pose.hmi` for full control
3. Zip the resource pack and distribute!
