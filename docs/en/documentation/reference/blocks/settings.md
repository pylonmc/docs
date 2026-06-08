Just like items, you can assign settings to a block.

Suppose we have a block which counts up by one every time it is right clicked:

```java title="ExampleAddonKeys.java"
public class ExampleAddonKeys {
    public static final NamespacedKey EXAMPLE_BLOCK = new NamespacedKey(ExampleAddon.getInstance(), "example_block");
}
```

```java title="ExampleBlock.java"
public class ExampleBlock extends RebarBlock implements RebarInteractBlock {

    public static final NamespacedKey COUNTER_KEY = new NamespacedKey(ExampleAddon.getInstance(), "counter");

    private int counter = 0;

    public ExampleBlock(@NotNull Block block, @NotNull BlockCreateContext context) {
        super(block, context);
    }

    public ExampleBlock(@NotNull Block block, @NotNull PersistentDataContainer pdc) {
        super(block, pdc);
        counter = pdc.get(COUNTER_KEY, RebarSerializers.INTEGER);
    }

    @Override
    public void write(@NonNull PersistentDataContainer pdc) {
        pdc.set(COUNTER_KEY, RebarSerializers.INTEGER, counter);
    }

    @Override
    public void onInteract(@NotNull PlayerInteractEvent event, @NotNull EventPriority priority) {
        if (event.getAction().isRightClick() && event.getHand() == EquipmentSlot.HAND) {
            counter++;
            event.getPlayer().sendMessage(String.valueOf(counter));
        }
    }
}
```

```java title="ExampleAddonBlocks.java"
public final class ExampleAddonBlocks {
    public static void initialize() {
        RebarBlock.register(ExampleAddonKeys.EXAMPLE_BLOCK, Material.IRON_BLOCK, ExampleBlock.class);
    }
}
```

```java title="ExampleAddonItems.java"
public final class ExampleAddonItems {

    public static final ItemStack EXAMPLE_BLOCK = ItemStackBuilder.rebar(Material.IRON_BLOCK, ExampleAddonKeys.EXAMPLE_BLOCK)
            .build();

    public static void initialize() {
        RebarItem.register(RebarItem.class, EXAMPLE_BLOCK, ExampleAddonKeys.EXAMPLE_BLOCK);
        PylonPages.MISCELLANEOUS.addItem(EXAMPLE_BLOCK);
    }
}
```

```java title="en.yml"
item:
  example_block:
    name: "Example Block"
    lore: |-
      <arrow> An example block
```

Suppose you want to change the counter by a configurable amount instead of always changing it by one. First, create a file `resources/settings/example_block.yml` (the name of the file must be the key of the block you wish to change):
```yaml title="example_block.yml"
increment-by: 4
```

You can get the config value using `getSettings()`:
```java title="ExampleBlock.java"
public class ExampleBlock extends RebarBlock implements RebarInteractBlock {


    // ...

    public final int incrementBy = getSettings().getOrThrow("increment-by", ConfigAdapter.INTEGER);
    
    // ...
}

```

Now, you can change the counter by the config value instead of by one:
```java title="ExampleBlock.java"
public class ExampleBlock extends RebarBlock implements RebarInteractBlock {


    // ...

    @Override
    public void onInteract(@NotNull PlayerInteractEvent event, @NotNull EventPriority priority) {
        if (event.getAction().isRightClick() && event.getHand() == EquipmentSlot.HAND) {
            counter += incrementBy;
            event.getPlayer().sendMessage(String.valueOf(counter));
        }
    }
}

```

The block will now count up by 4 at a time.


## One class, multiple blocks

Suppose that you want to have two different types of counters: one that counts up by 2 at a time, and one that counts up by 4 at a time. You do not need to create a new class for this. Instead, you can register two different counter blocks that both use the same class:
```java title="ExampleAddonKeys.java"
public class ExampleAddonKeys {
    public static final NamespacedKey EXAMPLE_BLOCK_1 = new NamespacedKey(ExampleAddon.getInstance(), "example_block_1");
    public static final NamespacedKey EXAMPLE_BLOCK_2 = new NamespacedKey(ExampleAddon.getInstance(), "example_block_2");
}
```

```java title="ExampleAddonBlocks.java"
public final class ExampleAddonBlocks {
    public static void initialize() {
        RebarBlock.register(ExampleAddonKeys.EXAMPLE_BLOCK_1, Material.IRON_BLOCK, ExampleBlock.class);
        RebarBlock.register(ExampleAddonKeys.EXAMPLE_BLOCK_2, Material.IRON_BLOCK, ExampleBlock.class);
    }
}
```

```java title="ExampleAddonItems.java"
public final class ExampleAddonItems {

    public static final ItemStack EXAMPLE_BLOCK_1 = ItemStackBuilder.rebar(Material.IRON_BLOCK, ExampleAddonKeys.EXAMPLE_BLOCK_1)
            .build();

    public static final ItemStack EXAMPLE_BLOCK_2 = ItemStackBuilder.rebar(Material.IRON_BLOCK, ExampleAddonKeys.EXAMPLE_BLOCK_2)
            .build();

    public static void initialize() {
        RebarItem.register(RebarItem.class, EXAMPLE_BLOCK_1, ExampleAddonKeys.EXAMPLE_BLOCK_1);
        PylonPages.MISCELLANEOUS.addItem(EXAMPLE_BLOCK_1);

        RebarItem.register(RebarItem.class, EXAMPLE_BLOCK_2, ExampleAddonKeys.EXAMPLE_BLOCK_2);
        PylonPages.MISCELLANEOUS.addItem(EXAMPLE_BLOCK_2);
    }
}
```

```java title="en.yml"
item:
  example_block_1:
    name: "Example Block I"
    lore: |-
      <arrow> An example block

  example_block_2:
    name: "Example Block II"
    lore: |-
      <arrow> An example block
```

Each block can have its own settings:
```yaml title="example_block_1.yml"
increment-by: 2
```

```yaml title="example_block_2.yml"
increment-by: 4
```

Now, you have two blocks; one that increments by 2, and one that increments by 4.


## Blocks and items share settings

Settings are referenced by `NamespacedKey`s. Under the hood, `getSettings()` is just a shorthand for `Settings.get(getKey())`. Since the items in this case have the same keys as the blocks, calling `getSettings()` within the item class returns the same settings. 

To demonstrate this, suppose we want to add a value to the lore of the Example Block item which shows how much the counter is incremented by. We could just hardcode the value into the language file:
```java title="en.yml"
item:
  example_block_1:
    name: "Example Block I"
    lore: |-
      <arrow> <attr>Increments by:</attr> 2

  example_block_2:
    name: "Example Block II"
    lore: |-
      <arrow> <attr>Increments by:</attr> 4
```

However, if the config value is changed, this change will not be reflected in the item's lore. Therefore, we will supply a placeholder:
```java title="en.yml"
item:
  example_block_1:
    name: "Example Block I"
    lore: |-
      <arrow> <attr>Increments by:</attr> %increment-by%

  example_block_2:
    name: "Example Block II"
    lore: |-
      <arrow> <attr>Increments by:</attr> %increment-by%
```

In order to supply the placeholder, we need to create a new class which extends `RebarItem` and override `getPlaceholders()`. We will then supply the configurable `incrementBy` value as `%increment-by%`:
```java title="ExampleBlockItem.java"
public class ExampleBlockItem extends RebarItem {

    public final int incrementBy = getSettings().getOrThrow("increment-by", ConfigAdapter.INTEGER);

    public ExampleBlockItem(@NonNull ItemStack stack) {
        super(stack);
    }

    @Override
    public @NonNull List<RebarArgument> getPlaceholders() {
        return List.of(
            RebarArgument.of("increment-by", incrementBy)
        );
    }
}
```

!!! question "Do I really need to get the value of incrementBy twice - once in the item and once in the block?"
    Unfortunately, yes. Ways around this were hotly debated in the past, but this is the least-bad compromise. Other options involve more complexity and less flexibility.

We can now register the items using this class:
```java title="ExampleAddonItems.java"
public final class ExampleAddonItems {

    public static final ItemStack EXAMPLE_BLOCK_1 = ItemStackBuilder.rebar(Material.IRON_BLOCK, ExampleAddonKeys.EXAMPLE_BLOCK_1)
            .build();

    public static final ItemStack EXAMPLE_BLOCK_2 = ItemStackBuilder.rebar(Material.IRON_BLOCK, ExampleAddonKeys.EXAMPLE_BLOCK_2)
            .build();

    public static void initialize() {
        RebarItem.register(ExampleBlockItem.class, EXAMPLE_BLOCK_1, ExampleAddonKeys.EXAMPLE_BLOCK_1);
        PylonPages.MISCELLANEOUS.addItem(EXAMPLE_BLOCK_1);

        RebarItem.register(ExampleBlockItem.class, EXAMPLE_BLOCK_2, ExampleAddonKeys.EXAMPLE_BLOCK_2);
        PylonPages.MISCELLANEOUS.addItem(EXAMPLE_BLOCK_2);
    }
}
```

This will show the amount by which the block will increment its value in the item's lore:

![Counter I](img/counter-1.png)
![Counter II](img/counter-2.png)
