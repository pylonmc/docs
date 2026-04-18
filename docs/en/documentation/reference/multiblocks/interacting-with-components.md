You will often need to interact with specific components in a multiblock. For example, you might want to change a furnace in your multiblock to be lit while the multiblock is burning fuel. To do this, you can use the `getMultiblockComponent`, `getMultiblockComponentOrThrow`, and `getMultiblockBlock` methods. 

All of these methods automatically account for the rotation of the multiblock, so the position you supply to these methods should be the same as the position you supply in `getComponents`.

For example:

```java
@Override
public void tick() {
    if (!isFormedAndFullyLoaded()) {
        return;
    }

    Block furnace = getMultiblockBlock(new Vector3i(0, 2, 0));
    Furnace furnaceData = (Furnace) furnace;
    furnaceData.setLit(true);
    furnace.setBlockData(furnaceData);

    getMultiblockComponentOrThrow(FluidInputHatch.class, new Vector3i(0, 0, 1))
            .addFluid(PylonFluids.HYDRAULIC_FLUID, 100.0);
}
```

It's useful to store positions of important components in variables, to avoid repeating them and so that only one variable needs to be changed if you want to change the position of the component. For example:

```java
public static final Vector3i FURNACE_POSITION = new Vector3i(0, 2, 0);

// ...

@Override
public @NotNull Map<@NotNull Vector3i, @NotNull MultiblockComponent> getComponents() {
    return Map.of(
            FURNACE_POSITION, MultiblockComponent.of(Material.FURNACE),
            // ...
    );
}

@Override
public void tick() {
    if (!isFormedAndFullyLoaded()) {
        return;
    }

    Block furnace = getMultiblockBlock(FURNACE_POSITION);
    Furnace furnaceData = (Furnace) furnace;
    furnaceData.setLit(true);
    furnace.setBlockData(furnaceData);

    // ...
}
```


## Item and fluid hatches

!!! info "This section is only relevant if you are creating an addon for Pylon"

Pylon adds fluid and item hatches, which are very useful for multiblocks.


### Item hatches

Item hatches are simple to use. 

1. Add an item hatch/hatches to your multiblock.
2. Where needed, get the hatch using `getMultiblockComponentOrThrow`.
3. Access the inventory of the hatch and implement your ticking logic.

For example:

```java
public static final Vector3i ITEM_INPUT_HATCH_POSITION = new Vector3i(0, 2, 0);

// ...

@Override
public @NotNull Map<@NotNull Vector3i, @NotNull MultiblockComponent> getComponents() {
    return Map.of(
            ITEM_INPUT_HATCH_POSITION, MultiblockComponent.of(PylonKeys.ITEM_INPUT_HATCH)
            // ...
    );
}

@Override
public void tick() {
    if (!isFormedAndFullyLoaded()) {
        return;
    }

    VirtualInventory inventory = getMultiblockComponentOrThrow(ItemInputHatch.class, ITEM_INPUT_HATCH_POSITION).inventory;
    ItemStack stack = inventory.getItem(0);
    if (new ItemStack(Material.BLAZE_POWDER).isSimilar(stack)) {
        inventory.setItem(new MachineUpdateReason(), 0, stack.subtract());
        // ...
    }

    // ...
}

```


### Fluid hatches

Fluid hatches are similar to item hatches but slightly more complicated, as the type of fluid they contain must be set by the multiblock.

1. Add a fluid hatch/hatches to your multiblock.
2. When the multiblock is formed, set the type of fluid in each hatch.
3. Where needed, get the hatch using `getMultiblockComponentOrThrow`.
4. Implment logic to add/remove fluid from the hatch where needed.

For example:

```java
public static final Vector3i FLUID_INPUT_HATCH_POSITION = new Vector3i(0, 2, 0);

// ...

@Override
public @NotNull Map<@NotNull Vector3i, @NotNull MultiblockComponent> getComponents() {
    return Map.of(
            FLUID_INPUT_HATCH_POSITION, MultiblockComponent.of(PylonKeys.FLUID_INPUT_HATCH)
    );
}

@Override
public void onMultiblockFormed() {
    getMultiblockComponentOrThrow(FluidInputHatch.class, FLUID_INPUT_HATCH_POSITION)
            .setFluidType(PylonFluids.WATER);
}

@Override
public void tick() {
    if (!isFormedAndFullyLoaded()) {
        return;
    }

    FluidInputHatch inputHatch = getMultiblockComponentOrThrow(FluidInputHatch.class, FLUID_INPUT_HATCH_POSITION);
    if (inputHatch.fluidAmount(PylonFluids.WATER) < 1000.0) {
        return;
    }
    inputHatch.removeFluid(PylonFluids.WATER, 1000.0);

    // ...
}
```

