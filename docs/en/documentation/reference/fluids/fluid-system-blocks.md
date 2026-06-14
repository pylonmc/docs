To allow your block to interact with the fluid system, it must implement one of:

- [FluidRebarBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidRebarBlock.html)
- [FluidBufferRebarBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidBufferRebarBlock.html)
- [FluidTankRebarBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidTankRebarBlock.html)

The most basic of these is FluidRebarBlock, which gives you maximum flexibility. Both FluidBufferRebarBlock and FluidTankRebarBlock extend FluidRebarBlock. Generally, you will only need to use FluidBufferRebarBlock and FluidTankRebarBlock as they cover the vast majority of use cases.

All three interfaces are explained further down.

---

## Creating fluid points

Once you have implemented one of the interfaces mentioned above, you need to add fluid points to your block. You can do this by calling `createFluidPoint` in your place constructor:

```java
public class ExampleBlock extends RebarBlock implements FluidRebarBlock {

    @SuppressWarnings("unused")
    public HydraulicPipeBender(@NotNull Block block, @NotNull BlockCreateContext context) {
        createFluidPoint(FluidPointType.INPUT, BlockFace.NORTH, context, false);
        createFluidPoint(FluidPointType.OUTPUT, BlockFace.SOUTH, context, false);
    }

    // ...

}
```

You need to pass in:

- The fluid point type (INPUT or OUTPUT)
- The fluid point direction (any cardinal BlockFace)
- The place context (this is used to orient your fluid points according to the direction the player placed the block)
- A boolean indicating whether the orientation can be vertical (by default, they are only oriented horizontally). 

!!! question "What does 'orientation can be vertical' mean?"
    Imagine you set the boolean indicating whether orientation can be vertical to false, and place the block while looking down. The fluid points will be oriented with respect to NORTH, SOUTH, EAST, or WEST depending on which direction is closest to where you're looking. If you set it to true, however, the orientation would be set to DOWN (because you are looking down) and the fluid points placed accordingly.

There are other overloads available. For example, you can pass in an extra float to indicate the distance at which the fluid point should be created:

```java
createFluidPoint(FluidPointType.OUTPUT, BlockFace.SOUTH, context, false, 0.6F);
```

You can find the full list of available overloads in the [FluidRebarBlock docs](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidRebarBlock.html).

!!! info "Multiple inputs/outputs"
    Your block can only have one input and one output. If you need to have more than one input/output for a machine, consider making your machine a multiblock and using [fluid hatches](fluid-hatches.md).

---

## FluidTankRebarBlock

FluidTankRebarBlock holds a single fluid at a time. It is very easy to use. Simply implement the interface, call `setCapacity` in your place constructor with your desired capacity, and implement `isAllowedFluid` to specify which fluids the tank can hold.

You can query and update the fluid tank using methods such as:

- [FluidTankRebarBlock#getFluidType](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidTankRebarBlock.html#getFluidType())
- [FluidTankRebarBlock#getFluidAmount](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidTankRebarBlock.html#getFluidAmount())
- [FluidTankRebarBlock#getFluidCapacity](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidTankRebarBlock.html#getFluidCapacity())
- [FluidTankRebarBlock#getFluidSpaceRemaining](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidTankRebarBlock.html#getFluidSpaceRemaining())
- [FluidTankRebarBlock#addFluid](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidTankRebarBlock.html#addFluid(java.lang.Double))
- ...and more (see [FluidBufferRebarBlock's JavaDocs](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidTankRebarBlock.html))

!!! example
    ```java
    public class ExampleBlock extends RebarBlock implements FluidTankRebarBlock {

        @SuppressWarnings("unused")
        public ExampleBlock(@NotNull Block block, @NotNull BlockCreateContext context) {
            super(block, context);
            createFluidPoint(FluidPointType.INPUT, BlockFace.NORTH, context, false);
            createFluidPoint(FluidPointType.OUTPUT, BlockFace.SOUTH, context, false);
            setCapacity(2000.0);
        }
    
        // ...
    
        @Override
        public boolean isAllowedFluid(@NotNull RebarFluid fluid) {
            return fluid.equals(PylonFluids.PLANT_OIL);
        }

        @Override
        public @Nullable WailaDisplay getWaila(@NotNull Player player) {
            return WailaDisplay.of(this, player)
                    .add(ProgressBar.fluidContents(getFluidType(), getFluidCapacity(), getFluidAmount()));
        }
    }
    ```
    
---

## FluidBufferRebarBlock

Often, you will want to store several different fluids in your block. A common pattern is a block which takes one fluid as an input and outputs a different one. In this case, FluidTankRebarBlock cannot be used.

FluidBufferRebarBlock allows you to create an arbitrary number of fluid buffers by calling [FluidBufferRebarBlock#createFluidBuffer](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidBufferRebarBlock.html#createFluidBuffer(io.github.pylonmc.rebar.fluid.RebarFluid,java.lang.Double,java.lang.Boolean,java.lang.Boolean)) in your create constructor. Each fluid can only have one corresponding buffer. Buffers can be marked as input (input fluid points can add to this buffer), output (output fluid points can take from this buffer), both, or neither.

You can query and update the fluid buffers using methods such as:

- [FluidBufferRebarBlock#hasFluid](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidBufferRebarBlock.html#hasFluid(io.github.pylonmc.rebar.fluid.RebarFluid))
- [FluidBufferRebarBlock#fluidAmount](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidBufferRebarBlock.html#fluidAmount(io.github.pylonmc.rebar.fluid.RebarFluid))
- [FluidBufferRebarBlock#fluidCapacity](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidBufferRebarBlock.html#fluidCapacity(io.github.pylonmc.rebar.fluid.RebarFluid))
- [FluidBufferRebarBlock#fluidSpaceRemaining](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidBufferRebarBlock.html#fluidSpaceRemaining(io.github.pylonmc.rebar.fluid.RebarFluid))
- [FluidBufferRebarBlock#addFluid](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidBufferRebarBlock.html#addFluid(io.github.pylonmc.rebar.fluid.RebarFluid,java.lang.Double))
- ...and more (see [FluidBufferRebarBlock's JavaDocs](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidBufferRebarBlock.html))

### Example

!!! example
    ```java
    public class ExampleBlock extends RebarBlock implements FluidBufferRebarBlock {
    
        @SuppressWarnings("unused")
        public ExampleBlock(@NotNull Block block, @NotNull BlockCreateContext context) {
            super(block, context);
            createFluidPoint(FluidPointType.INPUT, BlockFace.NORTH, context, false);
            createFluidPoint(FluidPointType.OUTPUT, BlockFace.SOUTH, context, false);
    
            // fluid, capacity, input, output
            createFluidBuffer(PylonFluids.HYDRAULIC_FLUID, 1000.0, true, false);
            createFluidBuffer(PylonFluids.DIRTY_HYDRAULIC_FLUID, 500.0, false, true);
        }
    
        // ...
    
        @Override
        public @Nullable WailaDisplay getWaila(@NotNull Player player) {
            return WailaDisplay.of(this, player)
                    .add(ProgressBar.fluidContents(
                            PylonFluids.HYDRAULIC_FLUID,
                            fluidCapacity(PylonFluids.HYDRAULIC_FLUID),
                            fluidAmount(PylonFluids.HYDRAULIC_FLUID)
                    ))
                    .add(ProgressBar.fluidContents(
                            PylonFluids.DIRTY_HYDRAULIC_FLUID,
                            fluidCapacity(PylonFluids.DIRTY_HYDRAULIC_FLUID),
                            fluidAmount(PylonFluids.DIRTY_HYDRAULIC_FLUID)
                    ));
        }
    }
    ```

---

## FluidRebarBlock

This is the most flexible of all the fluid interfaces but requires you to implement all of your own logic for handling fluids. You should prefer to use FluidBufferRebarBlock or FluidTankRebarBlock unless necessary.

### Inputs

If your block has an input connection point, you need to override [FluidRebarBlock#fluidAmountRequested](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidRebarBlock.html#fluidAmountRequested(io.github.pylonmc.rebar.fluid.RebarFluid)) and [FluidRebarBlock#onFluidAdded](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidRebarBlock.html#onFluidAdded(io.github.pylonmc.rebar.fluid.RebarFluid,java.lang.Double)).

`fluidAmountRequested` dictates how much of any type of fluid you want to receive on the next fluid tick. For example, if you have a machine that consumes 5 water per second, it should request `5.0 * RebarConfig.fluidTickInterval / 20.0` when the fluid supplied is water, and return 0 for every other fluid.

`onFluidAdded` dictates what should happen when a fluid is added. For example, if your machine has an internal buffer of plant oil represented by a variable, you should add the supplied `amount` argument to the variable when the fluid removed is plant oil. The `fluid` argument will always be one which your `fluidAmountRequested` function returns more than zero, and the amount will never be greater than `fluidAmountRequested(fluid)`.

### Outputs

If your block has an output connection point, you need to override [FluidRebarBlock#getSuppliedFluids](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidRebarBlock.html#getSuppliedFluids()) and [FluidRebarBlock#onFluidRemoved](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidRebarBlock.html#onFluidRemoved(io.github.pylonmc.rebar.fluid.RebarFluid,java.lang.Double)).

`getSuppliedFluids` dictates the the fluids (and their corresponding amounts) that can be supplied by the block on the next fluid tick. For example, if you have a machine that produces up to 5 water per second, it should have one entry in the map returned, which is water to `5.0 * RebarConfig.fluidTickInterval / 20.0`.

`onFluidRemoved` dictates what should happen when a fluid is removed. For example, if your machine has an internal buffer of plant oil represented by a variable, you should subtract the supplied `amount` argument from the variable. The `fluid` argument will always be one supplied by `getSuppliedFluids` function, and the amount will never be greater than the amount specified by `getSuppliedFluids`.

### Example

!!! example
    Here is the (slightly abbreviated) source code for the water pump, to demonstrate how to use FluidRebarBlock.
    
    ```java
    public class WaterPump extends RebarBlock implements FluidRebarBlock {
    
        // ...
    
        public WaterPump(@NotNull Block block, @NotNull BlockCreateContext context) {
            super(block, context);
            createFluidPoint(FluidPointType.OUTPUT, BlockFace.UP);
        }
    
        // ...
    
    
        @Override
        public @NotNull List<Pair<RebarFluid, Double>> getSuppliedFluids() {
            Block below = getBlock().getRelative(BlockFace.DOWN);
            FluidData data = below.getWorld().getFluidData(below.getLocation());
            if (data.getFluidType() == Fluid.WATER && data.isSource()) {
                return List.of(new Pair<>(
                    PylonFluids.WATER, 
                    waterPerSecond * RebarConfig.FLUID_TICK_INTERVAL / 20.0
                ));
            }
    
            return List.of();
        }
    
        @Override
        public void onFluidRemoved(@NotNull RebarFluid fluid, double amount) {
            // `fluid` will always be `PylonFluids.WATER`
            // `amount` will always be less than or equal to `waterPerSecond * RebarConfig.FLUID_TICK_INTERVAL / 20.0`
            // Nothing needs to be done in this function for the water pump
        }
    }
    ```

