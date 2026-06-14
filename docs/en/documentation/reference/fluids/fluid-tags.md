Fluids can have [RebarFluidTag](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/fluid/RebarFluidTag.html)s associated with them which can store arbitrary data. For example:
- The temperature of the fluid (Rebar has this tag by default)
- The energy density of the fluid (e.g. diesel could have a higher energy density than plant oil)
- Whether the liquid can be used as a coolant

### Creating a new tag

To create a new tag, simply create a new class to represent your tag, and implement the RebarFluidTag class. This class has only one method; `getDisplayText`. This method is used to display your tag in the fluid's lore.

You can then add the tag to your fluid using [RebarFluid#addTag](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/fluid/RebarFluid.html#addTag(io.github.pylonmc.rebar.fluid.RebarFluidTag)).

!!! warning
    A common point of confusion is that fluid tags are associated with a single *fluid type*. You cannot use the tag system to (for example) assign a numeric temperature to a fluid which allows water to be heated up in a boiler. This is because tags are assigned to a fluid type, not to a fluid that exists in the world. This is primarily due to hugely increased complexity and reduced performance during fluid routing.

