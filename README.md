# 📦 Hold My Items – Animation Script Documentation

**Mod Name**: Hold My Items  
**Author**: sapling
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

`x` must be in the range `0.0` to `1.0`.

list can be found here: https://easings.net/#

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
## 🧱 Item Utilities (`I`)

The `I` class provides functions to inspect the item currently being rendered. You can use it to check if the player is holding a specific item or something from a tag group.

### ✅ Checking Item Type

`I.isOf(itemStack, Items.DIAMOND_SWORD);`

Returns `true` if the item is exactly a Diamond Sword.

### 🏷 Checking Item Tags

`I.isIn(itemStack, ItemTags.SWORDS);`

Returns `true` if the item is in the `#swords` tag.

### ❌ Checking if Hand is Empty

`I.isEmpty(itemStack);`

Returns `true` if the player's hand is empty.

### 🔍 Example Use

`if (I.isOf(itemStack, Items.SHIELD)) {
    M.moveZ(matrices, -0.2);
    M.rotateY(matrices, 15);
}

if (I.isIn(itemStack, ItemTags.FLOWERS)) {
    M.moveY(matrices, M.sin(P.getPitch(player) * 0.1) * 0.1);
}

if (I.isEmpty(itemStack)) {
    M.rotateZ(matrices, 45);
}`

### 📦 Item and Tag References

-   `Items` class contains every item in the game:
    `Items.APPLE, Items.CARROT, Items.CROSSBOW, ...`

-   `ItemTags` contains built-in tag groups:
    `ItemTags.SWORDS, ItemTags.LOGS, ItemTags.BANNERS, ...`
---
## 📂 Registries

Persist and reuse values across frames:

```java
float timer = floatRegistry.getOrDefault("myTimer", 0.0); //get value from a registry, if not exists returns 0.0
timer += HoldMyItems.getDeltaTime();
floatRegistry.put("myTimer", timer); // create a variable in a registry
```

Use it for:
- Time tracking
- Animation state
- Interpolation progress

Existing registries:
 - floatRegistry (saving floats)
 - intRegistry (saving integers)
 - booleanRegistry (saving booleans)
 - itemRegistry (saving items)

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






---
## 🏷️ ConventionalItemTags
STONES  
COBBLESTONES  
DEEPSLATE_COBBLESTONES  
INFESTED_COBBLESTONES  
MOSSY_COBBLESTONES  
NORMAL_COBBLESTONES  
NETHERRACKS  
END_STONES  
GRAVELS  
OBSIDIANS  
NORMAL_OBSIDIANS  
CRYING_OBSIDIANS  
TOOLS  
SHEAR_TOOLS  
SPEAR_TOOLS  
BOW_TOOLS  
CROSSBOW_TOOLS  
SHIELD_TOOLS  
FISHING_ROD_TOOLS  
BRUSH_TOOLS  
IGNITER_TOOLS  
MACE_TOOLS  
WRENCH_TOOLS  
MELEE_WEAPON_TOOLS  
RANGED_WEAPON_TOOLS  
MINING_TOOL_TOOLS  
ARMORS  
ENCHANTABLES  
BRICKS  
DUSTS  
CLUMPS  
GEMS  
INGOTS  
NUGGETS  
ORES  
RAW_MATERIALS  
IRON_RAW_MATERIALS  
GOLD_RAW_MATERIALS  
COPPER_RAW_MATERIALS  
NORMAL_BRICKS  
NETHER_BRICKS  
RESIN_BRICKS  
IRON_INGOTS  
GOLD_INGOTS  
COPPER_INGOTS  
NETHERITE_INGOTS  
COAL_ORES  
COPPER_ORES  
DIAMOND_ORES  
EMERALD_ORES  
GOLD_ORES  
IRON_ORES  
LAPIS_ORES  
NETHERITE_SCRAP_ORES  
QUARTZ_ORES  
REDSTONE_ORES  
QUARTZ_GEMS  
LAPIS_GEMS  
DIAMOND_GEMS  
AMETHYST_GEMS  
EMERALD_GEMS  
PRISMARINE_GEMS  
IRON_NUGGETS  
GOLD_NUGGETS  
REDSTONE_DUSTS  
GLOWSTONE_DUSTS  
RESIN_CLUMPS  
POTIONS  
BOTTLE_POTIONS  
FOODS  
ANIMAL_FOODS  
FRUIT_FOODS  
VEGETABLE_FOODS  
BERRY_FOODS  
BREAD_FOODS  
COOKIE_FOODS  
RAW_MEAT_FOODS  
COOKED_MEAT_FOODS  
RAW_FISH_FOODS  
COOKED_FISH_FOODS  
SOUP_FOODS  
CANDY_FOODS  
PIE_FOODS  
GOLDEN_FOODS  
EDIBLE_WHEN_PLACED_FOODS  
FOOD_POISONING_FOODS  
DRINKS  
WATER_DRINKS  
WATERY_DRINKS  
MILK_DRINKS  
HONEY_DRINKS  
MAGIC_DRINKS  
OMINOUS_DRINKS  
JUICE_DRINKS  
DRINK_CONTAINING_BUCKET  
DRINK_CONTAINING_BOTTLE  
BUCKETS  
EMPTY_BUCKETS  
WATER_BUCKETS  
LAVA_BUCKETS  
MILK_BUCKETS  
POWDER_SNOW_BUCKETS  
ENTITY_WATER_BUCKETS  
BARRELS  
WOODEN_BARRELS  
BOOKSHELVES  
CHESTS  
WOODEN_CHESTS  
TRAPPED_CHESTS  
ENDER_CHESTS  
GLASS_BLOCKS  
GLASS_BLOCKS_COLORLESS  
GLASS_BLOCKS_CHEAP  
GLASS_BLOCKS_TINTED  
GLASS_PANES  
GLASS_PANES_COLORLESS  
SHULKER_BOXES  
GLAZED_TERRACOTTAS  
CONCRETES  
CONCRETE_POWDERS  
BUDDING_BLOCKS  
BUDS  
CLUSTERS  
VILLAGER_JOB_SITES  
SANDS  
RED_SANDS  
COLORLESS_SANDS  
SANDSTONE_BLOCKS  
SANDSTONE_SLABS  
SANDSTONE_STAIRS  
RED_SANDSTONE_BLOCKS  
RED_SANDSTONE_SLABS  
RED_SANDSTONE_STAIRS  
UNCOLORED_SANDSTONE_BLOCKS  
UNCOLORED_SANDSTONE_SLABS  
UNCOLORED_SANDSTONE_STAIRS  
SMALL_FLOWERS  
TALL_FLOWERS  
FLOWERS  
FENCES  
WOODEN_FENCES  
NETHER_BRICK_FENCES  
FENCE_GATES  
WOODEN_FENCE_GATES  
PUMPKINS  
NORMAL_PUMPKINS  
CARVED_PUMPKINS  
JACK_O_LANTERNS_PUMPKINS  
DYES  
BLACK_DYES  
BLUE_DYES  
BROWN_DYES  
CYAN_DYES  
GRAY_DYES  
GREEN_DYES  
LIGHT_BLUE_DYES  
LIGHT_GRAY_DYES  
LIME_DYES  
MAGENTA_DYES  
ORANGE_DYES  
PINK_DYES  
PURPLE_DYES  
RED_DYES  
WHITE_DYES  
YELLOW_DYES  
DYED  
BLACK_DYED  
BLUE_DYED  
BROWN_DYED  
CYAN_DYED  
GRAY_DYED  
GREEN_DYED  
LIGHT_BLUE_DYED  
LIGHT_GRAY_DYED  
LIME_DYED  
MAGENTA_DYED  
ORANGE_DYED  
PINK_DYED  
PURPLE_DYED  
RED_DYED  
WHITE_DYED  
YELLOW_DYED  
STORAGE_BLOCKS  
STORAGE_BLOCKS_BONE_MEAL  
STORAGE_BLOCKS_COAL  
STORAGE_BLOCKS_COPPER  
STORAGE_BLOCKS_DIAMOND  
STORAGE_BLOCKS_DRIED_KELP  
STORAGE_BLOCKS_EMERALD  
STORAGE_BLOCKS_GOLD  
STORAGE_BLOCKS_IRON  
STORAGE_BLOCKS_LAPIS  
STORAGE_BLOCKS_NETHERITE  
STORAGE_BLOCKS_RAW_COPPER  
STORAGE_BLOCKS_RAW_GOLD  
STORAGE_BLOCKS_RAW_IRON  
STORAGE_BLOCKS_REDSTONE  
STORAGE_BLOCKS_RESIN  
STORAGE_BLOCKS_SLIME  
STORAGE_BLOCKS_WHEAT  
STRIPPED_LOGS  
STRIPPED_WOODS  
CROPS  
BEETROOT_CROPS  
CACTUS_CROPS  
CARROT_CROPS  
COCOA_BEAN_CROPS  
MELON_CROPS  
NETHER_WART_CROPS  
POTATO_CROPS  
PUMPKIN_CROPS  
SUGAR_CANE_CROPS  
WHEAT_CROPS  
SEEDS  
BEETROOT_SEEDS  
MELON_SEEDS  
PUMPKIN_SEEDS  
TORCHFLOWER_SEEDS  
PITCHER_PLANT_SEEDS  
WHEAT_SEEDS  
PLAYER_WORKSTATIONS_CRAFTING_TABLES  
PLAYER_WORKSTATIONS_FURNACES  
STRINGS  
LEATHERS  
BONES  
EGGS  
FEATHERS  
GUNPOWDERS  
MUSHROOMS  
NETHER_STARS  
MUSIC_DISCS  
RODS  
WOODEN_RODS  
BLAZE_RODS  
BREEZE_RODS  
ROPES  
CHAINS  
ENDER_PEARLS  
SLIME_BALLS  
FERTILIZERS  
HIDDEN_FROM_RECIPE_VIEWERS  
ORE_BEARING_GROUND_DEEPSLATE  
ORE_BEARING_GROUND_NETHERRACK  
ORE_BEARING_GROUND_STONE  
ORE_RATES_DENSE  
ORE_RATES_SINGULAR  
ORE_RATES_SPARSE  
ORES_IN_GROUND_DEEPSLATE  
ORES_IN_GROUND_NETHERRACK  
ORES_IN_GROUND_STONE


---
## 🏷️ ItemTags
WOOL  
PLANKS  
STONE_BRICKS  
WOODEN_BUTTONS  
STONE_BUTTONS  
BUTTONS  
WOOL_CARPETS  
WOODEN_DOORS  
WOODEN_STAIRS  
WOODEN_SLABS  
WOODEN_FENCES  
FENCE_GATES  
WOODEN_PRESSURE_PLATES  
WOODEN_TRAPDOORS  
DOORS  
SAPLINGS  
LOGS_THAT_BURN  
LOGS  
DARK_OAK_LOGS  
PALE_OAK_LOGS  
OAK_LOGS  
BIRCH_LOGS  
ACACIA_LOGS  
CHERRY_LOGS  
JUNGLE_LOGS  
SPRUCE_LOGS  
MANGROVE_LOGS  
CRIMSON_STEMS  
WARPED_STEMS  
BAMBOO_BLOCKS  
WART_BLOCKS  
BANNERS  
SAND  
SMELTS_TO_GLASS  
STAIRS  
SLABS  
WALLS  
ANVIL  
RAILS  
LEAVES  
TRAPDOORS  
SMALL_FLOWERS  
FLOWERS  
BEDS  
FENCES  
PIGLIN_REPELLENTS  
PIGLIN_LOVED  
IGNORED_BY_PIGLIN_BABIES  
PIGLIN_SAFE_ARMOR  
DUPLICATES_ALLAYS  
BREWING_FUEL  
SHULKER_BOXES  
EGGS  
MEAT  
SNIFFER_FOOD  
PIGLIN_FOOD  
FOX_FOOD  
COW_FOOD  
GOAT_FOOD  
SHEEP_FOOD  
WOLF_FOOD  
CAT_FOOD  
HORSE_FOOD  
HORSE_TEMPT_ITEMS  
CAMEL_FOOD  
ARMADILLO_FOOD  
BEE_FOOD  
CHICKEN_FOOD  
FROG_FOOD  
HOGLIN_FOOD  
LLAMA_FOOD  
LLAMA_TEMPT_ITEMS  
OCELOT_FOOD  
PANDA_FOOD  
PANDA_EATS_FROM_GROUND  
PIG_FOOD  
RABBIT_FOOD  
STRIDER_FOOD  
STRIDER_TEMPT_ITEMS  
TURTLE_FOOD  
PARROT_FOOD  
PARROT_POISONOUS_FOOD  
AXOLOTL_FOOD  
GOLD_ORES  
IRON_ORES  
DIAMOND_ORES  
REDSTONE_ORES  
LAPIS_ORES  
COAL_ORES  
EMERALD_ORES  
COPPER_ORES  
NON_FLAMMABLE_WOOD  
SOUL_FIRE_BASE_BLOCKS  
CANDLES  
DIRT  
TERRACOTTA  
COMPLETES_FIND_TREE_TUTORIAL  
BOATS  
CHEST_BOATS  
FISHES  
SIGNS  
CREEPER_DROP_MUSIC_DISCS  
COALS  
ARROWS  
LECTERN_BOOKS  
BOOKSHELF_BOOKS  
BEACON_PAYMENT_ITEMS  
WOODEN_TOOL_MATERIALS  
STONE_TOOL_MATERIALS  
IRON_TOOL_MATERIALS  
GOLD_TOOL_MATERIALS  
DIAMOND_TOOL_MATERIALS  
NETHERITE_TOOL_MATERIALS  
REPAIRS_LEATHER_ARMOR  
REPAIRS_CHAIN_ARMOR  
REPAIRS_IRON_ARMOR  
REPAIRS_GOLD_ARMOR  
REPAIRS_DIAMOND_ARMOR  
REPAIRS_NETHERITE_ARMOR  
REPAIRS_TURTLE_HELMET  
REPAIRS_WOLF_ARMOR  
STONE_CRAFTING_MATERIALS  
FREEZE_IMMUNE_WEARABLES  
DAMPENS_VIBRATIONS  
CLUSTER_MAX_HARVESTABLES  
COMPASSES  
HANGING_SIGNS  
CREEPER_IGNITERS  
NOTEBLOCK_TOP_INSTRUMENTS  
FOOT_ARMOR  
LEG_ARMOR  
CHEST_ARMOR  
HEAD_ARMOR  
SKULLS  
TRIMMABLE_ARMOR  
TRIM_MATERIALS  
DECORATED_POT_SHERDS  
DECORATED_POT_INGREDIENTS  
SWORDS  
AXES  
HOES  
PICKAXES  
SHOVELS  
BREAKS_DECORATED_POTS  
VILLAGER_PLANTABLE_SEEDS  
VILLAGER_PICKS_UP  
DYEABLE  
FURNACE_MINECART_FUEL  
BUNDLES  
BOOK_CLONING_TARGET  
SKELETON_PREFERRED_WEAPONS  
DROWNED_PREFERRED_WEAPONS  
PIGLIN_PREFERRED_WEAPONS  
PILLAGER_PREFERRED_WEAPONS  
WITHER_SKELETON_DISLIKED_WEAPONS  
FOOT_ARMOR_ENCHANTABLE  
LEG_ARMOR_ENCHANTABLE  
CHEST_ARMOR_ENCHANTABLE  
HEAD_ARMOR_ENCHANTABLE  
ARMOR_ENCHANTABLE  
SWORD_ENCHANTABLE  
FIRE_ASPECT_ENCHANTABLE  
SHARP_WEAPON_ENCHANTABLE  
WEAPON_ENCHANTABLE  
MINING_ENCHANTABLE  
MINING_LOOT_ENCHANTABLE  
FISHING_ENCHANTABLE  
TRIDENT_ENCHANTABLE  
DURABILITY_ENCHANTABLE  
BOW_ENCHANTABLE  
EQUIPPABLE_ENCHANTABLE  
CROSSBOW_ENCHANTABLE  
VANISHING_ENCHANTABLE  
MACE_ENCHANTABLE  
MAP_INVISIBILITY_EQUIPMENT  
GAZE_DISGUISE_EQUIPMENT  
