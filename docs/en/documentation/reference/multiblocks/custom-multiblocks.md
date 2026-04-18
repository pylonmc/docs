`RebarSimpleMultiblock` is very useful and suitable for most multiblocks. However, on occasion you may want more flexibility. A simple example would be the fluid tank; a multiblock which can have 0-8 multiblock tank casings on top of it. Obviously, this is not suitable for regular `RebarSimpleMultiblock`.

This can be accomplished using `RebarMultiblock`, which RebarSimpleMultiblock is built on top of. At its core, the multiblock system is just a change detection system which listens for blocks being placed or broken, and notifies your multiblock if it has had a component placed or broken. In order to do this, the change detection system requires you to provide:

* A list of chunks which the multiblock spans
* A function which returns whether a given block is part of the multiblock

To provide these, your multiblock must implement `getChunksOccupied` and `isPartOfMultiblock`. For example:

```java
@Override
public @NotNull Set<@NotNull ChunkPosition> getChunksOccupied() {
    return Set.of(new ChunkPosition(getBlock().getChunk()));
}

@Override
public boolean isPartOfMultiblock(@NotNull Block otherBlock) {
    return otherBlock.equals(getBlock().getRelative(BlockFace.DOWN));
}
```

Note that `isPartOfMultiblock` must return true if the block could *potentially* be part of the multiblock, so you should not be doing any checks for the *type* of block - just the position! For example, if your multiblock needs a fire in the block below it, then `isPartOfMultiblock` should ALWAYS return true if the block below your multiblock is supplied to it.

Additionally, you must implement `checkFormed`, for example:

```java
@Override
public boolean checkFormed() {
    Block belowBlock = getBlock().getRelative(BlockFace.DOWN);
    return !BlockStorage.isRebarBlock(belowBlock) && belowBlock.getType() == Material.FIRE;
}
```

This will be called whenever a block that is part of your multiblock (i.e. is within the chunks supplied by `getChunksOccupied` and satisfies `isPartOfMultiblock`) is modified. It can be thought of as a 'refresh' function which makes sure that the multiblock is still constructed correctly.

`RebarMultiblock` still comes with the `isFormedAndFullyLoaded`, `onMultiblockFormed`, and `onMultiblockUnformed` methods, which are useful for implementing your own multiblock management logic.

Obviously, this is extremely barebones. You will need to implement simple concepts like rotation yourself - you may wish to look at source code of `RebarSimpleMultiblock` to make this easier.

Finally, if you want to give your custom multiblock ghost blocks like `RebarSimpleMultiblock`, you can use [RebarGhostBlockHolder](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarGhostBlockHolder.html). This automatically manages ghost blocks. You can create a new ghost block with `addGhostBlock` and remove a ghost block with `removeGhostBlock`.

