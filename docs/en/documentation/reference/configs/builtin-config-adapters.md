This page contains examples (and extra documentation where needed) for every ConfigAdapter bundled with Rebar.

## Primitive adapters

??? abstract "ConfigAdapter.BYTE"

    Range: -128 to 127
    
    ```yaml
    example: 5
    ```

??? abstract "ConfigAdapter.SHORT"

    Range: -32768 to 32767
    
    ```yaml
    example: 5
    ```

??? abstract "ConfigAdapter.INTEGER"

    Range: -2147483648 and 2147483647
    
    ```yaml
    example: 5
    ```

??? abstract "ConfigAdapter.LONG"

    Range: -9223372036854775808 and 9223372036854775807
    
    ```yaml
    example: 5
    ```

??? abstract "ConfigAdapter.FLOAT"

    ```yaml
    example-1: 5
    example-2: 5.2
    ```

??? abstract "ConfigAdapter.DOUBLE"

    ```yaml
    example-1: 5
    example-2: 5.2
    ```

??? abstract "ConfigAdapter.INT_RANGE"

    ```yaml
    example-1: [3, 8]
    example-2:
        min: 3
        max: 8
    example-3: 5 # 5 <= x <= 5
    ```

??? abstract "ConfigAdapter.BOOLEAN"

    ```yaml
    example: true
    ```

??? abstract "ConfigAdapter.CHAR"

    ```yaml
    example-1: 'h'
    example-2: "h"
    ```

??? abstract "ConfigAdapter.STRING"

    ```yaml
    example: "watermelon"
    ```

??? abstract "ConfigAdapter.ANY"

    A special config adapter which returns the raw type returned by [ConfigurationSection#get](https://jd.papermc.io/paper/26.1.2/org/bukkit/configuration/ConfigurationSection.html#get(java.lang.String)).
    
    You should basically never need to use this.

---

## Miscellaneous adapters

??? abstract "ConfigAdapter.CONFIG_SECTION"

    ```yaml
    example:
      some-key: 5
      some-other-key: 10
    ```

??? abstract "ConfigAdapter.UUID"

    ```yaml
    example-1: "b48e7ff7-4c0f-4234-869b-69b6db1d4c57"
    exmaple-2: [-8747281617155961769, -5436267000079891916]
    ```

??? abstract "ConfigAdapter.NAMESPACED_KEY"

    ```yaml
    example: minecraft:melon
    ```

??? abstract "ConfigAdapter.MATERIAL"

    ```yaml
    example-1: red_wool
    example-2: Red_wOoL
    ```

??? abstract "ConfigAdapter.BLOCK_DATA"

    ```yaml
    example: minecraft:chest[waterlogged=true]
    ```

??? abstract "ConfigAdapter.TEXT_COLOR"

    Can be any of the following:
    - A hex color code
    - A color name (see [NamedTextColor](https://jd.advntr.dev/api/4.26.1/net/kyori/adventure/text/format/NamedTextColor.html) for a list of available colors)
    - An RGB color (r, g, and b are from 0-255)
    - A HSV color (h, s, and v are from 0.0-1.0)
    
    ```yaml
    example-1: "#5a45f3"
    example-2: gold
    example-3: liGHt_pURPLe
    example-4:
      r: 200
      g: 155
      b: 30
    example-5:
      h: 0.83
      s: 0.76
      v: 0.28
    ```

??? abstract "ConfigAdapter.ITEM_TAG"

    See [the Minecraft wiki](https://minecraft.wiki/w/Item_tag_(Java_Edition)) for a list of available item tags.
    
    ```yaml
    example: "#glazed_terracotta"
    ```

??? abstract "ConfigAdapter.SOUND"

    ```yaml
    example:
      sound: minecraft:block.anvil.use
      source: player
      volume: 0.5
      pitch: 1.0
    ```

??? abstract "ConfigAdapter.RANDOMIZED_SOUND"

    See [RandomizedSound](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/util/RandomizedSound.html).
    
    ```yaml
    example:
      hammer-sound:
        sounds:
        - minecraft:block.anvil.use
        - minecraft:block.anvil.land
        source: player
        volume:
          min: 0.3
          max: 0.7
        pitch:
          - 0.8
          - 1.2
    ```

??? abstract "ConfigAdapter.WAILA_DISPLAY"

    ```yaml
    example:
      text: ""
      color: white
      overlay: progress
      progress: 1.0
    ```

??? abstract "ConfigAdapter.CULLING_PRESET"

    ```yaml
    example:
      index: 1
      id: "low"
      material: "minecraft:green_concrete"
      update-interval: 1
      hidden-interval: 1
      visible-interval: 10
      always-show-radius: 48
      cull-radius: 128
      max-occluding-count: 5
    ```

??? abstract "ConfigAdapter.CONTRIBUTOR"

    ```yaml
    example:
      display-name: "Watermelon"
      description: "<white><!i>Hey girl, are you a kettle? Because I'm 90% water."
      minecraft-uuid: "bb685dd0-8a71-4506-bc20-24d374fb28b4"
      github: "WatermelonMC"
    ```

---

## Collection adapters

??? abstract "ConfigAdapter.LIST"

    ```yaml
    # All examples read using ConfigAdapter.LIST.from(ConfigAdapter.INTEGER)
    
    example-1:
        - 5
        - 7
    
    example-2: [5, 7]
    
    example-3: []
    ```

??? abstract "ConfigAdapter.SET"

    Same format as `ConfigAdapter.LIST`. Duplicate values do not cause errors, and only appear once in the resulting map.

    ```yaml
    # All examples read using ConfigAdapter.SET.from(ConfigAdapter.INTEGER)
    
    example-1:
        - 5
        - 7
    
    example-2: [5, 7]
    
    example-3: []
    ```

??? abstract "ConfigAdapter.MAP"

    If there are any duplicated keys, an error will be thrown.
    
    ```yaml
    # Read using ConfigAdapter.MAP.from(ConfigAdapter.INTEGER, ConfigAdapter.STRING)
    example-1:
      5: "wat"
      9: "erm"
      66: "elon"
    
    # Read using:
    # ConfigAdapter.MAP.from(
    #         ConfigAdapter.LIST.from(ConfigAdapter.INTEGER), 
    #         ConfigAdapter.STRING)
    # );
    example-2:
      - key: [2, 6]
        value: "wat"
      - key: [7, 9, 3]
        value: "erm"
      - key: []
        value: "elon"
    ```

??? abstract "ConfigAdapter.WEIGHTED_SET"

    See [WeightedSet](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/util/WeightedSet.html) for more info.
    
    ```yaml
    example:
      - value: "watermelon"
        weight: 5
      - value: "cantaloupe"
        weight: 3
      - value: "honeydew melon"
        weight: 8
    ```

---

## Vector adapters

??? abstract "ConfigAdapter.VECTOR_2I"

    ```yaml
    example: [0, 6]
    ```

??? abstract "ConfigAdapter.VECTOR_2F"
    
    ```yaml
    example-1: [2.6, 1.3]
    example-2: [1.0, 6]
    ```

??? abstract "ConfigAdapter.VECTOR_2D"

    ```yaml
    example-1: [2.6, 1.3]
    example-2: [1.0, 6]
    ```

??? abstract "ConfigAdapter.VECTOR_3I"

    ```yaml
    example: [0, 6, 3]
    ```

??? abstract "ConfigAdapter.VECTOR_3F"

    ```yaml
    example-1: [2.6, 1.3, 6.4]
    example-2: [1.0, 6, 6.4]
    ```

??? abstract "ConfigAdapter.VECTOR_3D"

    ```yaml
    example-1: [2.6, 1.3, 6.4]
    example-2: [1.0, 6, 6.4]
    ```

---

## Generic adapters

??? abstract "ConfigAdapter.ENUM"

    A ConfigAdapter which represents an enum. You can specify which enum to use by calling `ConfigAdapter.ENUM.from(<enum class>)`. Case insensitive.
    
    ```yaml
    # Read with ConfigAdapter.ENUM.from(Material.class)
    example-1: diamond_pickaxe
    
    # Read with ConfigAdapter.ENUM.from(MiningLevel.class)
    example-2: gold
    ```

??? abstract "ConfigAdapter.KEYED"

    A ConfigAdapter which reads from any set of things associated with a key, such as materials, Rebar items, etc.
    
    Usually created from a Rebar registry via `ConfigAdapter.KEYED.fromRegistry`, but you can also create one by passing in your own function to turn keys into values with `ConfigAdapter.KEYED.fromGetter`.
    
    ```yaml
    # Read with ConfigAdapter.KEYED.fromRegistry(RebarItemSchema.class, RebarRegistry.ITEMS)
    example: pylon:fluid_pipe_copper
    ```
    
---

## Fluid/Item adapters

??? abstract "ConfigAdapter.ITEM_STACK"

    ```yaml
    example-1: stick
    example-2: minecraft:stick
    example-3:
      stick: 5
    example-4: potion[potion_contents={potion:"healing"}]
    example-5: 
      'potion[potion_contents={potion:"healing"}]': 7
    ```

??? abstract "ConfigAdapter.ITEM_TYPE_WRAPPER"

    ```yaml
    example-1: pylon:fluid_pipe_copper
    example-2: minecraft:apple
    ```

??? abstract "ConfigAdapter.ITEM_CHOICE"

    Amount defaults to one if unspecified.
    
    If `exact` is set, the choice will require all the components to be their default value. For example, if exact is set and the item is renamed, it will no longer match the recipe choice. You can specify components to omit in this check using `ignore`.
    
    ```yaml
    example-1: "#minecraft:acacia_logs"
    example-2: 'minecraft:potion[potion_contents={potion:"healing"}]'
    example-3: minecraft:apple
    example-4: pylon:copper_dust
    example-5:
      '#minecraft:acacia_logs': 5
    
    example-6:
      choices:
      - pylon:bronze_pickaxe
      - "#minecraft:pickaxes"
    
    example-7:
      amount: 5
      choices:
      - item: pylon:bronze_pickaxe
        exact: true
        ignore:
          - damage
      - item: pylon:bronze_ingot
      - item: minecraft:stone_pickaxe
    ```

??? abstract "ConfigAdapter.REBAR_FLUID"

    ```yaml
    example: pylon:plant_oil
    ```

??? abstract "ConfigAdapter.FLUID_TEMPERATURE"

    ```yaml
    example: hot
    ```

??? abstract "ConfigAdapter.FLUID_CHOICE"

    ```yaml
    example-1:
      pylon:water: 100
    example-2:
      pylon:water: 333.333333333
    example-3:
      fluids: [pylon:water, pylon:lava]
      amount: 300
    ```

??? abstract "ConfigAdapter.FLUID_OR_ITEM"

    Accepts anything that `ConfigAdapter.ITEM_STACK` or `ConfigAdapter.FLUID_WITH_AMOUNT` accept.
    
    ```yaml
    example-1: potion[potion_contents={potion:"healing"}]
    example-2:
      pylon:copper_dust: 5
    example-3: minecraft:apple
    example-4:
      pylon:water: 100
    example-5:
      pylon:water: 333.333333333
    ```

??? abstract "ConfigAdapter.FLUID_WITH_AMOUNT"

    ```yaml
    example-1:
      pylon:water: 100
    example-2:
      pylon:water: 333.333333333
    ```

---

