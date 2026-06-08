Rebar has a number of builtin interfaces which add custom behaviour to blocks. For example, there are interfaces which allow you to:

- add fluid buffers ([RebarFluidBufferBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarFluidBufferBlock.html))
- run code when the block is clicked ([RebarInteractBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarInteractBlock.html))
- run code when a player jumps on top of a block ([RebarJumpBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarJumpBlock.html))
- ...and many many more

!!! question "How do I use interface X?"
    1. Check if it has any documentation on this site (scroll down).
    2. Read the JavaDocs of the interface and of the relevant methods.
    3. Check for examples in Pylon or other addons where the interface is used.
    4. Ask for help on the Pylon Discord.

For example, suppose you want to spawn particles when the block is right clicked.. You can do this using [RebarInteractBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarInteractBlock.html):
```java title="ExampleBlock.java"
public class ExampleBlock extends RebarBlock implements RebarInteractBlock {
    
    // ...

    @Override
    public void onInteract(@NotNull PlayerInteractEvent event, @NotNull EventPriority priority) {
        if (!event.getAction().isRightClick()
                || event.getPlayer().isSneaking()
                || event.getHand() != EquipmentSlot.HAND
        ) {
            return;
        }

        new ParticleBuilder(Particle.CAMPFIRE_COSY_SMOKE)
                .location(getBlock().getLocation().toCenterLocation().add(0, 0.7, 0))
                .count(smokeCount)
                .extra(0.1) // speed
                .spawn();
    }
}
```

## The `postLoad` method

Many block interfaces store persistent data. Instead of forcing you to load and save the data yourself in the `write` method and your load constructor, all of Rebar's block interfaces have a separate mechanism for saving their data to the block's PDC. A side effect of this is that the block's load constructor is called, none of Rebar's block interfaces will have loaded their data yet. Therefore, as a rule of thumb, you should not call any methods related to block interfaces in your load constructor. Instead, override the `postLoad` method and do whatever you need to do there instead. The `postLoad` method will be called after all block interfaces associated with the block have loaded their data.

!!! info "postInitialise"
    It is also worth noting the `postInitialise` method, which is the same as `postLoad` except in that it is also called after the place constructor.


## Interface quick reference

<div class="grid cards" markdown>
- !!! abstract "Interaction interfaces"

        Interfaces which do something when a player interacts with them
    
        | Interface | Javadocs |
        | :-------: | :------- |
        | RebarGuiBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarGuiBlock.html)|
        | RebarInteractBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/Rebar(InteractBlock).html)|
        | RebarJumpBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/Rebar(RebarJumpBlock).html)|
        | RebarSneakBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/Rebar(RebarSneakBlock).html)|


- !!! abstract "Miscellaneous interfaces"

        Interfaces which do not fall into any particular category.
    
        | Interface | Javadocs |
        | :-------: | :------- |
        | RebarBreakHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarBreakHandler.html)|
        | RebarDirectionalBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarDirectionalBlock.html)|
        | RebarFacadeBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarFacadeBlock.html)|
        | RebarFallingBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarFallingBlock.html)|
        | RebarTickingBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarSneakBlock.html)|
        | RebarUnloadBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarSneakBlock.html)|
        | RebarVirtualInventoryBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarVirtualInventoryBlock.html)|


- !!! abstract "Logistics interfaces"

        Interfaces which allow your block to interact with logistic systems, including cargo.
    
        If you want your machine to be compatible with cargo inserters/extractors, you need to implement [RebarLogisticBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarLogisticBlock.html).
    
        | Interface | Javadocs |
        | :-------: | :------- |
        | RebarCargoBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarCargoBlock.html)|
        | RebarLogisticBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarLogisticBlock.html)|


- !!! abstract "Fluid interfaces"

        Interfaces which allow your block to interact with the fluid system.
        
        You should use `RebarFluidBufferBlock` and `RebarFluidTank` unless necessary. The `RebarFluidBlock` interface is quite abstract and designed to be as flexible as possible, requiring your machine to implement its own fluid storage logic.
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | RebarFluidBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarFluidBlock.html)|
        | RebarFluidBufferBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarFluidBufferBlock.html)|
        | RebarFluidTank | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarFluidTank.html)|


- !!! abstract "Processor interfaces"

        Interfaces which keep track of a process, such as the amount of fuel or the progress of a recipe.
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | RebarProcessor | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarProcessor.html)|
        | RebarRecipeProcessor | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarRecipeProcessor.html)|

  
- !!! abstract "Multiblock interfaces"

        Interfaces which turn your block into a multiblock.
  
        Most multiblocks you create can use [RebarSimpleMultiblock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarSimpleMultiblock.html). For more complex multiblocks, you can [RebarMultiblock], but this class is designed to be as flexible as possible and so is very barebones - it does not come with ghost blocks, does not account for rotation, etc.
  
        Due to the way multiblock work under the hood, there is almost zero performance cost associated with them (contrary to what you may expect). Even extremely large multiblocks are very performance-friendly.
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | RebarMultiblock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarMultiblock.html)|
        | RebarSimpleMultiblock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarSimpleMultiblock.html)|

  
- !!! abstract "Entity interfaces"

        Interfaces which interact with entities in some way.
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | RebarEntityChangedBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarEntityChangedBlock.html)|
        | RebarEntityHolderBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarEntityHolderBlock.html)|


- !!! abstract "Culling interfaces"

        Interfaces which allow your block to interact with Rebar's culling system.
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | RebarCulledBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarCulledBlock.html)|
        | RebarGroupCulledBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarGroupCulledBlock.html)|
        | RebarEntityCulledBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarEntityCulledBlock.html)|
        | RebarEntityGroupCulledBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarEntityGroupCulledBlock.html)|


- !!! abstract "Vanilla interfaces"

        Interfaces which expose functionality of vanilla blocks. For example, if you add a block which is a beacon and want to run some code when the beacon is deactivated, you could use RebarBeacon.
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | RebarBeacon | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarBeacon.html)|
        | RebarBell | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarBell.html)|
        | RebarBrewingStand | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarBrewingStand.html)|
        | RebarCampfire | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarCampfire.html)|
        | RebarCauldron | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarCauldron.html)|
        | RebarComposter | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarComposter.html)|
        | RebarCopperBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarCopperBlock.html)|
        | RebarCrafter | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarCrafter.html)|
        | RebarDispenser | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarDispenser.html)|
        | RebarEnchantingTable | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarEnchantingTable.html)|
        | RebarFlowerPot | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarFlowerPot.html)|
        | RebarFurnace | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarFurnace.html)|
        | RebarGrowable | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarGrowable.html)|
        | RebarHopper | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarHopper.html)|
        | RebarJobBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarJobBlock.html)|
        | RebarLeaf | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarLeaf.html)|
        | RebarLectern | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarLectern.html)|
        | RebarNoJobBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarNoJobBlock.html)|
        | RebarNoteBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarNoteBlock.html)|
        | RebarNoVanillaContainerBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarNoVanillaContainerBlock.html)|
        | RebarPiston | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarPiston.html)|
        | RebarRedstoneBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarRedstoneBlock.html)|
        | RebarShearable | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarShearable.html)|
        | RebarSign | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarSign.html)|
        | RebarSponge | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarSponge.html)|
        | RebarTargetBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarTargetBlock.html)|
        | RebarTNT | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarTNT.html)|
        | RebarVault | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarVault.html)|
        | RebarVanillaContainerBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/base/RebarVanillaContainerBlock.html)|

</div>
