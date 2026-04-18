# Simple multiblocks

In Rebar, there are two classes that handle multiblock: [RebarMultiblock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarMultiblock.html) and [RebarSimpleMultiblock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarSimpleMultiblock.html). 

This page only covers `RebarSimpleMultiblock` as it is the simpler of the two and provides nice abstractions for working with multiblocks, while `RebarMultiblock` is far more flexible but also barebones.


## Multiblock basics

In order to use `RebarSimpleMultiblock`, implement the interface on a block. Then, specify a list of components. For example:

```java
public class ExampleMultiblock extends RebarBlock implements RebarSimpleMultiblock {

    public ExampleMultiblock(@NotNull Block block, @NotNull BlockCreateContext context) {
        super(block, context);
    }

    public ExampleMultiblock(@NotNull Block block, @NotNull PersistentDataContainer pdc) {
        super(block, pdc);
    }

    @Override
    public @NotNull Map<@NotNull Vector3i, @NotNull MultiblockComponent> getComponents() {
        return Map.of(
                new Vector3i(0, 0, 1), MultiblockComponent.of(Material.SEA_LANTERN),
                new Vector3i(0, 2, 0), MultiblockComponent.of(PylonKeys.BRONZE_BLOCK)
        );
    }
}
```

This is the bare minimum needed to implement a simple multiblock. `RebarSimpleMultiblock` will automatically create and manage ghost blocks which show the player how to build the multiblock. Once a player builds your multiblock, all components of the multiblock will automatically be assigned the WAILA of the block that implements `RebarSimpleMultiblock`, which will revert if any part of the multiblock is broken.


## Multiblock components

The [MultiblockComponent](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarSimpleMultiblock.MultiblockComponent.html) class is flexible and can be used for detecting a range of different blocks.

#### Vanilla block
```java
MultiblockComponent.of(Material.SEA_LANTERN)
```

#### Rebar block
```java
MultiblockComponent.of(PylonKeys.BRONZE_BLOCK)
```

#### Vanilla block with specific block data
```java
MultiblockComponent.of(Material.REDSTONE_LAMP.createBlockData("[lit=false]"))
```

#### Vanilla block or rebar block
```java
MultiblockComponent.of(
        List.of(Material.CAMPFIRE.createBlockData()),
        List.of(PylonKeys.BRONZE_BLOCK)
)
```

#### Vanilla block with specific block data or Rebar block
```java
MultiblockComponent.of(
        List.of(
                Material.CAMPFIRE.createBlockData(),
                Material.REDSTONE_LAMP.createBlockData("[lit=false]")
        ),
        List.of(
                PylonKeys.BRONZE_BLOCK,
                PylonKeys.CHARCOAL_BLOCK
        )
)
```


## Multiblock direction

Multiblocks are inherently directional (unless all the components are on the Y axis). In order to set the direction of the multiblock, call `setMultiblockDirection` in your place constructor. For example:

```java
public ExampleMultiblock(@NotNull Block block, @NotNull BlockCreateContext context) {
    super(block, context);
    setMultiblockDirection(context.getFacing());
}
```

By convention, you should also implement [RebarDirectionalBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarDirectionalBlock.html) and call `setFacing` alongside `setMultiblockDirection`. This will (among other things) allow your block to be textured according to the direction it is facing.

## Performance

This warrants a mention as it is not immediately obvious: in Rebar, **multiblocks do not have any significant performance overhead and the performance overhead scales very slow with size.** In other words: multiblocks are not going to be measurably laggier than a regular block, and large multiblocks have performance comparable to small multiblocks.

