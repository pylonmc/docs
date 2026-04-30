## Overview

This section describes how to add custom behaviour to an item. We'll demonstrate this by creating a baguette flamethrower which sets entities on fire when they are right clicked.

---

## The baguette flamethrower

### Creating a 'normal item'

Let's start by making a 'normal' item, like we did in the previous chapter.

```java title="MyAddonKeys.java"
public final class ExampleAddonKeys {
    public static final NamespacedKey baguetteFlamethrowerKey = new NamespacedKey(this, "baguette_flamethrower");
}
```

```java title="MyAddonitems.java"
public final class ExampleAddonItems {

    // ...

    public static final ItemStack baguetteFlamethrower = ItemStackBuilder.rebar(Material.BREAD, baguetteFlamethrowerKey)
            .build();

    // ...

    public static void initialize() {

        // ...

        RebarItem.register(RebarItem.class, baguette);
        PylonPages.FOOD.addItem(baguetteKey);
    }
}
```

<span></span>

```yaml title='resources/en.yml'
item:
  baguette:
    name: Baguette Flamethrower
    lore: |-
      <arrow> Use the power of baguettes to vanquish your enemies
      <arrow> <insn>Right click</insn> an entity to ignite it
```

### Creating a custom item class

Next, we'll add the code to set entities on fire.

In order to do this, we must create a custom `BaguetteFlamethrower` class. All Rebar item classes must extend [RebarItem].

Create a new file `BaguetteFlamethrower.java` and add the following:
```java title="BaguetteFlamethrower.java"
public class BaguetteFlamethrower extends RebarItem {
    public BaguetteFlamethrower(@NotNull ItemStack stack) {
        super(stack);
    }
}
```

We now want to run some code when the player right clicks on an entity while holding the baguette flamethrower. In order to do this, we need to implement a Rebar item interface. 

Rebar item interfaces are just interfaces that allow your interface to do more things, like running some code when the item is used.

Here, we need to implement the [RebarItemEntityInteractor] interface. This interface has one method: `onUsedToRightClickEntity`.

!!! note "You can find a full list of item interfaces [here](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/item/base/package-summary.html)."
```java title="BaguetteFlamethrower" hl_lines="6-9"
public class BaguetteFlamethrower extends RebarItem implements RebarItemEntityInteractor {
    public BaguetteFlamethrower(@NotNull ItemStack stack) {
        super(stack);
    }

    @Override
    public void onUsedToRightClickEntity(@NotNull PlayerInteractEntityEvent event, @NotNull EventPriority priority) {
        event.getRightClicked().setFireTicks(40);
    }
}
```

### Using the custom item class

In order to use this class, we now need to specify that the baguette flamethrower should use it instead of the default `RebarItem` class.

Change your `RebarItem.register(...)` line to the following:
```java
RebarItem.register(BaguetteFlamethrower.class, baguetteFlamethrower);
```

Now, try running the server. When you right click an entity with the baguette flamethrower, you should set it on fire for 40 ticks!

---

## Making items configurable

Items can have configs associated with them. If you look inside your server's Pylon settings directory (`run/plugins/Rebar/settings/pylon`) you'll see that many Pylon items and blocks have their own config file.

For example, `bandage.yml` contains:

```yaml title="bandage.yml"
consume-seconds: 1.25
heal-amount: 4.0
```

### Creating the config file

Suppose we want to add our own config file which lets you choose how long the baguette flamethrower sets entities on fire for.

Create a new file `resources/settings/baguette_flamethrower.yml` and add the following:

```yaml title="baguette_flamethrower.yml"
burn-time-ticks: 40
```

!!! success "Best practice"
    When your config option is a quantity, include the unit in the name. If we just called it `burn-time`, users might not know that it's in ticks, and assume it's in seconds or something else.

Rebar will automatically copy all of your settings files into `run/plugins/Rebar/settings/<your-addon-name>` when the settings file is first used, so that server owners can configure items.

### Reading the config file

To read the `burn-time-ticks` value from the config file, we can use `getSettings().getOrThrow(...)`. `getSettings()` will just return the config with the same name as the item key - in this case `flamethrower_baguette`:
```java title="BaguetteFlamethrower.java" hl_lines="2"
public class BaguetteFlamethrower extends RebarItem implements RebarItemEntityInteractor {
    private final int burnTimeTicks = getSettings().getOrThrow("burn-time-ticks", ConfigAdapter.INT);

    public BaguetteFlamethrower(@NotNull ItemStack stack) {
        super(stack);
    }

    @Override
    public void onUsedToRightClickEntity(@NotNull PlayerInteractEntityEvent event, @NotNull EventPriority priority) {
        event.getRightClicked().setFireTicks(40);
    }
}
```

!!! question "How does Rebar know which config file to use?"
    Config files are actually per **key**, not per item. When accessing `settings`, all this does is take the item's key and find the settings file for it! It's just a shorthand for `Settings.get(key)`. If you used `Settings.get(NamespacedKey(MyAddon.getInstance(), "buffoon"))` then the `buffoon.yml` settings file would be read instead.

Settings also have a `get` method. This method just returns null if the key was not found. If you use `getOrThrow`, a nice exception will be thrown with some information on what key is missing and where.

When calling `get` or `getOrThrow`, we have to supply a ConfigAdapter. This is a class that contains instructions on how to exactly read a value from a config file.

!!! info "You can find a list of config adapters [here](https://github.com/pylonmc/rebar/blob/master/rebar/src/main/kotlin/io/github/pylonmc/rebar/config/adapter/ConfigAdapter.kt)"

### Using the values we read from the config file

And now we can use that value when setting entities on fire!
```java title="BaguetteFlamethrower.java" hl_lines="10"
public class BaguetteFlamethrower extends RebarItem implements RebarItemEntityInteractor {
    private final int burnTimeTicks = getSettings().getOrThrow("burn-time-ticks", ConfigAdapter.INT);

    public BaguetteFlamethrower(@NotNull ItemStack stack) {
        super(stack);
    }

    @Override
    public void onUsedToRightClickEntity(@NotNull PlayerInteractEntityEvent event, @NotNull EventPriority priority) {
        event.getRightClicked().setFireTicks(burnTimeTicks);
    }
}
```

If you change the config value, the burn time should change with it.

!!! danger
    When testing configs, keep in mind that the run server task deletes everything inside the `plugins` folder whenever it's run, meaning any changes you make to your new config there will be overwritten. Instead, just try changing the default value - or run the server manually so the `plugins` folder isn't deleted when the server restarts.

---

## Full code

```java title="MyAddonKeys.java"
public final class ExampleAddonKeys {
    public static final NamespacedKey baguetteFlamethrowerKey = new NamespacedKey(this, "baguette_flamethrower");
}
```

```java title="BaguetteFlamethrower.java"
public class BaguetteFlamethrower extends RebarItem implements RebarItemEntityInteractor {
    private final int burnTimeTicks = getSettings().getOrThrow("burn-time-ticks", ConfigAdapter.INT);

    public BaguetteFlamethrower(@NotNull ItemStack stack) {
        super(stack);
    }

    @Override
    public void onUsedToRightClickEntity(@NotNull PlayerInteractEntityEvent event, @NotNull EventPriority priority) {
        event.getRightClicked().setFireTicks(burnTimeTicks);
    }
}
```

<span></span>

```java title="MyAddonitems.java"
public final class ExampleAddonItems {

    // ...

    public static final ItemStack baguetteFlamethrower = ItemStackBuilder.rebar(Material.BREAD, baguetteFlamethrowerKey)
            .build();

    // ...

    public static void initialize() {

        // ...

        RebarItem.register(RebarItem.class, baguette);
        PylonPages.FOOD.addItem(baguetteKey);
    }
}
```

<span></span>

```yaml title='resources/en.yml'
item:
  baguette:
    name: Baguette Flamethrower
    lore: |-
      <arrow> Use the power of baguettes to vanquish your enemies
      <arrow> <insn>Right click</insn> an entity to ignite it
```

<span></span>

```yaml title="baguette_flamethrower.yml"
burn-time-ticks: 40
```

[RebarItem]: https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/item/RebarItem.html
[RebarItemEntityInteractor]: https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/item/base/RebarItemEntityInteractor.html
