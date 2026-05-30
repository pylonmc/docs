## Item Constructors

You might assume that the custom item constructors' are called every time the item is created - eg, every time the item is crafted. But this is **not** the case.

The important thing to know is **item constructors can be invoked arbitrarily, and not just when the item is 'created'**.

??? question "Wait... why can constructors be called arbitrarily?"
    *(sigh)* It's complicated. To make a long story short...

    Rebar can't keep track of every single item at all times. If we could actually modify the core server code, then this would be possible. Instead, Rebar only *temporarily* track Rebar items.
    
    For example, when we right click an entity with an item that implements [RebarItemEntityInteractor], Rebar listens to the [PlayerInteractEntityEvent] and sees that an entity has been right clicked with an item. In order to figure out what item it is - and if it's even a Rebar item at all - Rebar has to look at the key stored inside the item. But even if Rebar knows the key, it still don't know much about the item. Does the item implement [RebarItemEntityInteractor]? If so, Rebar needs to call its `onUsedToRightClickEntity`.
    
    This is where the constructor comes in. We can look up what class the item corresponds to and create a new instance of it. Then, we can call the `onUsedToRightClickEntity` method.

What all this means is that **the class can be created or destroyed at any time**. Any data we store in fields is temporary.

But suppose we want to store the charge level of a portable battery. If we can't store the charge level in a field, where the hell *can* we store it?

---

## Persistent data containers (PDCs)
!!! info "PDCs are added by Paper, not Pylon. "
    We are covering them here because they're used for essentially all persistent data storage (including for blocks and entities) in Pylon.

A [PersistentDataContainer] (PDC) is a simple way to store data persistently. It can be thought of as similar to JSON in how it is structured; a nested key-value store. Each value is identified by a `NamespacedKey`. PDCs can be nested in each other. Items have a PDC, so to save some data persistently, we need to write it to the item's PDC whenever it is modified.

!!! info "See [the Paper docs](https://docs.papermc.io/paper/dev/pdc/) for more info about PDCs."

Take the example of keeping track of the charge level of a portable battery. We can store the charge level in the `charge_level` key in the item's PDC. It will then be saved when the item is put in a chest, or when the server restarts.

If this is all a bit confusing - don't worry, an example should make it clearer.

---

## The Counting Baguette

### The idea

Let's create a 'Counting Baguette' that stores a number. When right clicked, the baguette will send the stored number to you in chat and increment it.

### Creating the item

We'll start by creating a new baguette that doesn't count anything yet.
```java title="MyAddonKeys.java"
public class MyAddonKeys {
    public static final NamespacedKey countingBaguetteKey = new NamespacedKey(this, "counting_baguette");
}
```

```java title="MyAddonItems.java"
public final class ExampleAddonItems {

    // ...

    public static final ItemStack countingBaguette = ItemStackBuilder.rebar(Material.BREAD, countingBaguetteKey)
            .build();

    // ...

    public static void initialize() {

        // ...

        RebarItem.register(CountingBaguette.class, countingBaguette);
        PylonPages.FOOD.addItem(countingBaguette);
    }
}
```

<span></span>

```java title="CountingBaguette.java"
public class CountingBaguette extends RebarItem {
    public CountingBaguette(@NotNull ItemStack stack) {
        super(stack);
    }
}
```

```yaml title="en.yml"
item:
  counting_baguette:
    name: "Counting Baguette"
    lore: |-
      <arrow> Mathematics is improved tenfold when combined with baguettes
```

### Adding the counting mechanic

First, we need to detect when the player right clicks:
```java title="CountingBaguette.java" hl_lines="1 6-10"
public class CountingBaguette extends RebarItem implements RebarInteractor {
    public CountingBaguette(@NotNull ItemStack stack) {
        super(stack);
    }

    @Override
    public void onUsedToClick(@NotNull PlayerInteractEvent event, @NotNull EventPriority priority) {
        // TODO send stored value to player
        // TODO increment stored value
    }
}
```

Inside the function, we need to read the stored value, send it to the player, and then store a value that is one higher.

To store a piece of data in a persistent data container, we need two other things: a [NamespacedKey] by which the piece of data is identified, and the **type** of the piece of data in the form of a [PersistentDataType].

!!! info "PylonSerializers"
    For convenience, Pylon supplies a range of types in the [PylonSerializers] file. This includes all the default types provided by Paper, but also some extra types (like UUID, Vector, BlockPosition, ItemStack, and more).

```java title="CountingBaguette.java" hl_lines="2 10-12"
public class CountingBaguette extends RebarItem implements RebarInteractor {
    public static final NamespacedKey COUNT_KEY = new NamespacedKey(MyAddon.getInstance(), "count");

    public CountingBaguette(@NotNull ItemStack stack) {
        super(stack);
    }

    @Override
    public void onUsedToClick(@NotNull PlayerInteractEvent event, @NotNull EventPriority priority) {
        int count = getStack().getPersistentDataContainer().get(COUNT_KEY, PylonSerializers.INTEGER);
        event.getPlayer().sendMessage(String.valueOf(count));
        getStack().editPersistentDataContainer(pdc -> pdc.set(COUNT_KEY, PylonSerializers.INTEGER, count + 1));
    }
}
```

### Setting the default value

You may have noticed that we have not set a starting value for the count yet. In order to do this, we need to edit the default item stack:

```java title="CountingBaguette.java" hl_lines="3"
NamespacedKey countingBaguetteKey = new NamespacedKey(this, "counting_baguette");
ItemStack countingBaguette = ItemStackBuilder.rebar(Material.BREAD, countingBaguetteKey)
        .editPdc(pdc -> pdc.set(CountingBaguette.COUNT_KEY, PylonSerializers.INTEGER, 0))
        .build();
RebarItem.register(CountingBaguette.class, countingBaguette);
PylonPages.FOOD.addItem(countingBaguette);
```

And that's it! The data we stored will persist as long as the item exists.

[NamespacedKey]: https://jd.papermc.io/paper/1.21.11/org/bukkit/NamespacedKey.html
[PersistentDataContainer]: https://docs.papermc.io/paper/dev/pdc/
[PersistentDataType]: https://docs.papermc.io/paper/dev/pdc/#data-types
[PylonSerializers]: https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/datatypes/RebarSerializers.html
[PlayerInteractEntityEvent]: https://jd.papermc.io/paper/1.21.11/org/bukkit/event/player/PlayerInteractEntityEvent.html
[RebarItemEntityInteractor]: https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/item/base/RebarItemEntityInteractor.html

