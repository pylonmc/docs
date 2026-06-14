Adding a new fluid is very straightforward and requires simply instantiating a RebarFluid and then registering it.

```java
public static final RebarFluid PLANT_OIL = new RebarFluid(
        pylonKey("plant_oil"),
        TextColor.fromHexString("#c4b352"),
        Material.YELLOW_CONCRETE_POWDER
).addTag(FluidTemperature.NORMAL);
static {
    PLANT_OIL.register();
}
```

You should add a temperature to every fluid you register. Above, the `FluidTemperature.NORMAL` temperature is assigned to plant oil using `addTag`. The reason `addTag` is used is because fluid temperature is a fluid tag. This is explained further in the [fluid tag docs](fluid-tags.md)

### Hiding fluids from the guide

You can prevent fluids from showing up in the guide as follows:

```java
RebarGuide.hideFluid(MY_FLUID.getKey());
```

### Base ingredient fluids

You may wish to mark a fluid as a 'base ingredient', meaning that in recipe ingredient calculations, the fluid will not be broken down any further. You can do this as follows:

```java
IngredientCalculator.addBaseIngredient(PLANT_OIL);
```

