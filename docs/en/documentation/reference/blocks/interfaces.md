
!!! info "See the [interface quick reference](interface-quick-reference.md) for a list of all available block interfaces."

Rebar has a number of builtin interfaces which add custom behaviour to blocks. For example, there are interfaces which allow you to:

- add fluid buffers ([FluidBufferRebarBlock](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/FluidBufferRebarBlock.html))
- run code when the block is clicked ([InteractRebarBlockHandler](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/InteractRebarBlockHandler.html))
- run code when a player jumps on top of a block ([JumpRebarBlockHandler](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/JumpRebarBlockHandler.html))
- ...and many many more

!!! note "Handlers"
    You might notice some interfaces have a `handler` suffix. This just means that the interface is a proxy for some kind of event (like the [PlayerInteractEvent](https://jd.papermc.io/paper/26.1.2/org/bukkit/event/player/PlayerInteractEvent.html)) and implements no or minimal additional logic. The distinction is not too important to understand from an addon developer perspective.

!!! question "How do I use interface X?"
    1. Check if it has any documentation on this site (scroll down).
    2. Read the JavaDocs of the interface and of the relevant methods.
    3. Check for examples in Pylon or other addons where the interface is used.
    4. Ask for help on the Pylon Discord.

For example, suppose you want to spawn particles when the block is right clicked.. You can do this using [InteractRebarBlockHandler](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/block/interfaces/InteractRebarBlockHandler.html):
```java title="ExampleBlock.java"
public class ExampleBlock extends RebarBlock implements InteractRebarBlockHandler {

    // ...


    @Override
    public void onInteractedWith(@NotNull PlayerInteractEvent event, @NotNull EventPriority priority) {
        if (!event.getAction().isRightClick()
                || event.getPlayer().isSneaking()
                || event.getHand() != EquipmentSlot.HAND
        ) {
            return;
        }

        new ParticleBuilder(Particle.CAMPFIRE_COSY_SMOKE)
                .location(getBlock().getLocation().toCenterLocation().add(0, 0.7, 0))
                .count(20)
                .extra(0.1) // speed
                .spawn();
    }
}

```

## The `postLoad` method

Many block interfaces store persistent data. Instead of forcing you to load and save the data yourself in the `write` method and your load constructor, all of Rebar's block interfaces have a separate mechanism for saving their data to the block's PDC. A side effect of this is that the block's load constructor is called, none of Rebar's block interfaces will have loaded their data yet. Therefore, as a rule of thumb, you should not call any methods related to block interfaces in your load constructor. Instead, override the `postLoad` method and do whatever you need to do there instead. The `postLoad` method will be called after all block interfaces associated with the block have loaded their data.

!!! info "postInitialise"
    It is also worth noting the `postInitialise` method, which is the same as `postLoad` except in that it is also called after the place constructor.

