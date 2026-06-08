Since each physical block has a corresponding instance, you can store block data in your block's class.

For example, consider a block which increments a counter by one every time it is right clicked:

```java title="ExampleBlock.java"
public class ExampleBlock extends RebarBlock implements RebarInteractBlock {

    private int counter = 0;

    public ExampleBlock(@NotNull Block block, @NotNull BlockCreateContext context) {
        super(block, context);
    }

    public ExampleBlock(@NotNull Block block, @NotNull PersistentDataContainer pdc) {
        super(block, pdc);
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

This appears to work fine at first glance. However, there is a problem. If the chunk containing the block is unloaded, the counter resets to zero. This is because the instance of the class is destroyed when the block's chunk is unloaded, and the value of `counter` is not saved anywhere.

To resolve this, the value of `counter` must be saved to the block's `PersistentDataContainer`.


## PersistentDataContainers

A `PersistentDataContainer` (PDC) is a simple way to store data persistently. It can be thought of as similar to JSON in how it is structured; a nested key-value store. Each value is identified by a `NamespacedKey`. PDCs can be nested in each other.

!!! info "See [the Paper docs](https://docs.papermc.io/paper/dev/pdc/) for more info about PDCs."

In 'normal' Minecraft, blocks don't have a PDC. Entities, chunks, item stacks, and a few other things do. However, through the power of dark magic, Rebar assigns each block a PDC which you can save data to when the block is unloaded, and load data from when the block is loaded.

!!! note "Wait, how does that work?"
    Under the hood, block PDCs are saved inside chunk PDCs. This has the great advantage that it means **Rebar block data is always synchronized with the chunk's other data.** The PDC that is passed to the `write` method and the load constructor is transient - it only exists while the block is being loaded or saved.

To save a piece of data in the block's PDC, we first need a key to identify it:
```java title="ExampleBlock.java"
public class ExampleBlock extends RebarBlock implements RebarInteractBlock {

    public static final NamespacedKey COUNTER_KEY = new NamespacedKey(ExampleAddon.getInstance(), "counter");

    // ...
}
```

You can save data in the write function:
```java title="ExampleBlock.java"
public class ExampleBlock extends RebarBlock implements RebarInteractBlock {

    // ...

    @Override
    public void write(@NonNull PersistentDataContainer pdc) {
        pdc.set(COUNTER_KEY, RebarSerializers.INTEGER, counter);
    }

    // ...
}
```

!!! danger "Do not assume that `write` is only called when the block is being unloaded"
    As discussed further down, `write` can be called for reasons other than a block being unloaded. If you need to run some code when the block is unloaded, see [RebarUnloadBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarUnloadBlock.html).

You can load data in the load constructor:
```java title="ExampleBlock.java"
public class ExampleBlock extends RebarBlock implements RebarInteractBlock {

    // ...

    public ExampleBlock(@NotNull Block block, @NotNull PersistentDataContainer pdc) {
        super(block, pdc);
        counter = pdc.get(COUNTER_KEY, RebarSerializers.INTEGER);
    }

    // ...
}
```

Putting it all together:
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

The example block now saves its counter and persists it across restarts.


## PersistentDataTypes

Above, we provided `PylonSerializers.INTEGER` to the `set` and `get` functions. This is a Persistent Data Type (PDT). We can only save certain types of data to disk (how would you save a `World` for example?). A PDT is a class which explains how to serialize a datatype into one of these primitive data types, and vice versa. If that sounds confusing, here is an example.

Rebar's `BlockPosition` contains X, Y, and Z coordinates along with the world in the form of a UUID.

```kotlin title="BlockPositionPersistentDataType.kt"
class BlockPosition(val worldId: UUID?, val x: Int, val y: Int, val z: Int) {
    // ...
}
```

Suppose we want write a PDT that lets us save `BlockPosition`. We can do this by writing the X, Y, Z, and world ID into another PDC, and returning that new PDC:
```kotlin title="BlockPositionPersistentDataType.kt"
object BlockPositionPersistentDataType : PersistentDataType<PersistentDataContainer, BlockPosition> {
    
    val worldKey = rebarKey("world")
    val xKey = rebarKey("x")
    val yKey = rebarKey("y")
    val zKey = rebarKey("z")

    // ...

    // Converts a `BlockPosition` into a `PersistentDataContainer`
    override fun toPrimitive(complex: BlockPosition, context: PersistentDataAdapterContext): PersistentDataContainer {
        val pdc = context.newPersistentDataContainer() // create new pdc
        pdc.set(xKey, PersistentDataType.INTEGER, complex.x) // write X coordinate
        pdc.set(yKey, PersistentDataType.INTEGER, complex.y) // write Y coordinate
        pdc.set(zKey, PersistentDataType.INTEGER, complex.z) // write Z coordinate
        complex.worldId?.let { pdc.set(worldKey, RebarSerializers.UUID, it) } // write the world UUID only if the world exists
        return pdc
    }
    
    // Converts a `PersistentDataContainer` into a `BlockPosition`
    override fun fromPrimitive(
        primitive: PersistentDataContainer,
        context: PersistentDataAdapterContext
    ): BlockPosition {
        val x = primitive.get(xKey, PersistentDataType.INTEGER)!! // read X coordinate
        val y = primitive.get(yKey, PersistentDataType.INTEGER)!! // read Y coordinate
        val z = primitive.get(zKey, PersistentDataType.INTEGER)!! // read Z coordinate
        val worldId = primitive.get(worldKey, RebarSerializers.UUID) // read the world UUID
        return BlockPosition(worldId, x, y, z)
    }
}
```

Paper already provides several PDTs, including bytes, longs, arrays of bytes, lists, and so on. However, Rebar comes with many new useful serializers including UUIDs, sets, maps, enums, item stacks, materials, fluids, locations, and many more. All of Rebar's PDTs are accessible using the `RebarSerializers` class, along with all of the default Paper serializers, for convenience.

You can find a full list of PDTs [here](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/datatypes/RebarSerializers.html). You can of course also create your own PDTs if needed.

!!! info "See [the Paper docs](https://docs.papermc.io/paper/dev/pdc/#data-types) for more info about PDTs."


## Debugging

You can use the `Debug Waxed Weathered Cut Copper Stairs` to view a block's PDC. Simply run `/pdc debug` to obtain the item, then right click a block. This will call the block's `write` function, serialize the resulting PDC into a more human-readable format, and show the result in chat.

