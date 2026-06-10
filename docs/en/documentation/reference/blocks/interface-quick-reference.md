<div class="grid cards" markdown>
- !!! abstract "Interaction interfaces"

        Interfaces which do something when a player interacts with them
    
        | Interface | Javadocs |
        | :-------: | :------- |
        | GuiRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/GuiRebarBlock.html)|
        | InteractRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/InteractRebarBlockHandler.html)|
        | JumpRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/JumpRebarBlockHandler.html)|
        | SneakRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/SneakRebarBlockHandler.html)|


- !!! abstract "Miscellaneous interfaces"

        Interfaces which do not fall into any particular category.
    
        | Interface | Javadocs |
        | :-------: | :------- |
        | BlockBreakRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/BlockBreakRebarBlockHandler.html)|
        | DirectionalRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/DirectionalRebarBlock.html)|
        | FacadeRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FacadeRebarBlock.html)|
        | FallingRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FallingRebarBlockHandler.html)|
        | TickingRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/TickingRebarBlock.html)|
        | UnloadRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/UnloadRebarBlockHandler.html)|
        | VirtualInventoryRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/VirtualInventoryRebarBlock.html)|
        | NoVanillaInventoryRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/NoVanillaInventoryRebarBlock.html)|
        | VanillaInventoryRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/VanillaInventoryRebarBlockHandler.html)|


- !!! abstract "Logistics interfaces"

        Interfaces which allow your block to interact with logistic systems, including cargo.
    
        If you want your machine to be compatible with cargo inserters/extractors, you need to implement [LogisticRebarBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/LogisticRebarBlock.html).
    
        | Interface | Javadocs |
        | :-------: | :------- |
        | CargoRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/CargoRebarBlock.html)|
        | LogisticRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/LogisticRebarBlock.html)|


- !!! abstract "Fluid interfaces"

        Interfaces which allow your block to interact with the fluid system.
        
        You should use [FluidBufferRebarBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidBufferRebarBlock.html) and [FluidTankRebarBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidTankRebarBlock.html) over [FluidRebarBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidRebarBlock.html) unless necessary. The [FluidRebarBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidRebarBlock.html) interface is quite abstract and designed to be as flexible as possible, requiring your machine to implement its own fluid storage logic.
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | FluidRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidRebarBlock.html)|
        | FluidBufferRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidBufferRebarBlock.html)|
        | FluidTankRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidTankRebarBlock.html)|


- !!! abstract "Processor interfaces"

        Interfaces which keep track of a process, such as the amount of fuel or the progress of a recipe.
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | ProcessorRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/ProcessorRebarBlock.html)|
        | RecipeProcessorRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/RecipeProcessorRebarBlock.html)|

  
- !!! abstract "Multiblock interfaces"

        Interfaces which turn your block into a multiblock.
  
        Most multiblocks you create can use [SimpleRebarMultiblock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/SimpleRebarMultiblock.html). For more complex multiblocks, you can [RebarMultiblock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/RebarMultiblock.html), but this class is designed to be as flexible as possible and so is very barebones - it does not come with ghost blocks, does not account for rotation, etc.
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | RebarMultiblock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/RebarMultiblock.html)|
        | SimpleRebarMultiblock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/SimpleRebarMultiblock.html)|

  
- !!! abstract "Entity interfaces"

        Interfaces which interact with entities in some way.
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | EntityChangeRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/EntityChangeRebarBlockHandler.html)|
        | EntityHolderRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/EntityHolderRebarBlock.html)|


- !!! abstract "Culling interfaces"

        Interfaces which allow your block to interact with Rebar's culling system.
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | CulledRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/CulledRebarBlock.html)|
        | GroupCulledRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/GroupCulledRebarBlock.html)|
        | EntityCulledRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/EntityCulledRebarBlock.html)|
        | EntityGroupCulledRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/EntityGroupCulledRebarBlock.html)|

- !!! abstract "Vanilla interfaces"

        Interfaces which expose functionality of vanilla blocks. For example, if you add a block which is a beacon and want to run some code when the beacon is deactivated, you could use [BeaconRebarBlockHandler](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/BeaconRebarBlockHandler.html).
  
        | Interface | Javadocs |
        | :-------: | :------- |
        | BeaconRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/BeaconRebarBlockHandler.html)|
        | BellRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/BellRebarBlockHandler.html)|
        | BrewingStandRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/BrewingStandRebarBlockHandler.html)|
        | CampfireRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/CampfireRebarBlockHandler.html)|
        | CauldronRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/CauldronRebarBlockHandler.html)|
        | ComposterRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/ComposterRebarBlockHandler.html)|
        | CopperRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/CopperRebarBlockHandler.html)|
        | CrafterRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/CrafterRebarBlockHandler.html)|
        | DispenserRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/DispenserRebarBlockHandler.html)|
        | EnchantingTableRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/EnchantingTableRebarBlockHandler.html)|
        | FlowerPotRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FlowerPotRebarBlockHandler.html)|
        | FurnaceRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FurnaceRebarBlockHandler.html)|
        | GrowRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/GrowRebarBlockHandler.html)|
        | HopperRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/HopperRebarBlockHandler.html)|
        | JobRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/JobRebarBlockHandler.html)|
        | LeafRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/LeafRebarBlockHandler.html)|
        | LecternRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/LecternRebarBlockHandler.html)|
        | NoJobRebarBlock | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/NoJobRebarBlock.html)|
        | NoteRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/NoteRebarBlockHandler.html)|
        | PistonRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/PistonRebarBlockHandler.html)|
        | RedstoneRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/RedstoneRebarBlockHandler.html)|
        | ShearRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/ShearRebarBlockHandler.html)|
        | SignRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/SignRebarBlockHandler.html)|
        | SpongeRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/SpongeRebarBlockHandler.html)|
        | TargetRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/TargetRebarBlockHandler.html)|
        | TNTRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/TNTRebarBlockHandler.html)|
        | VaultRebarBlockHandler | [Javadocs :material-arrow-right:](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/VaultRebarBlockHandler.html)|

</div>
