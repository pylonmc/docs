# Multiblock state

You will often need to get or listen for information about a multiblock's state. There are two main pieces of information tracked by Rebar that are important to know about:

* Is the multiblock formed?
* Are all parts of the multiblock loaded?

The first is only true if all the components of the multiblock have been placed. The second is only true if every component in the multiblock is in a loaded chunk. This is necessary because the multiblock might span multiple chunks: unlike a regular block, in a multiblock you may need to ensure that the entire multiblock structure is loaded when running certain code.

To be safe, you should check that the multiblock is complete and fully loaded when doing anything that interacts with other components of the multiblock. You can do this by calling `isFormedAndFullyLoaded()`. For example:
```java
@Override
public void tick() {
    if (!isFormedAndFullyLoaded()) {
        return;
    }
    
    // Ticking logic here
}
```

You can also execute code when the multiblock is formed or unformed using `onMultiblockFormed` and `onMultiblockUnformed`. For example:
```java
@Override
public void onMultiblockFormed() {
    RebarSimpleMultiblock.super.onMultiblockFormed();
    Bukkit.getServer().broadcast(Component.text("Stonks"));
}

@Override
public void onMultiblockUnformed(boolean partUnloaded) {
    RebarSimpleMultiblock.super.onMultiblockUnformed(partUnloaded);
    Bukkit.getServer().broadcast(Component.text("Not stonks"));
}
```

In `onMultiblockUnformed`, `partUnloaded` is true if the multiblock has become unformed because it was unloaded. If false, this indicates that part of the multiblock has been broken.

!!! danger "Always ensure you call `RebarMultiblock.super.onMultiblockFormed()` and `RebarMultiblock.super.onMultiblockUnformed(partUnloaded)` when overriding the methods."
    This is because RebarSimpleMultiblock uses the methods for managing WAILA overrides, ghost blocks, etc.

