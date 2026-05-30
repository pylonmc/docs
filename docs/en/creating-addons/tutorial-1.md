## Overview

The template addon has only one class: `ExampleAddon` (or whatever you renamed it to). This class extends [JavaPlugin] and implements [RebarAddon]. There are some comments inside the class to explain what each part does - have a read through and try to understand how it works. Let's add a new item!

Let's create a baguette, which fills 6 hunger bars, to start with. It may look like a lot, but do not fear! The process is very simple once you understand it, and doing this will expose you to a lot of Rebar's core systems.

To create a simple item, we only need two things: a **key** for the item, and an **item stack**.

---

## Adding the item

### Creating a key

[NamespacedKey]s are how Rebar identifies custom items, blocks, researches, entities, and more.

!!! question "Why use NamespacedKeys?"
    A key is just a simple piece of text, like `pylon:copper_dust`, which allows Rebar to uniquely identify your item. This is very similar to how vanilla Minecraft items have IDs. Why don't we just use `copper_dust` as the key? Well, what if two addons add an item called `copper_dust`? We won't be able to tell which one is which! To fix this, Rebar uses [NamespacedKey]s, which just means we take a string *and* your addon's name, and put them together - for example, `my_addon:copper_dust`.

By convention, keys go in the `ExampleAddonKeys` class. Create a new [NamespacedKey] called 'baguette':
```java title="ExampleAddonKeys.java"
public static final NamespacedKey baguetteKey = new NamespacedKey(ExampleAddon.getInstance(), "baguette");
```

### Creating the item stack

The second thing we need is an actual item. We'll use [ItemStackBuilder] for this.

[ItemStackBuilder] contains several different methods to help you create [ItemStack]s. For example, you can use `.set(<component>, <value>)` to set some of the item's values, like enchantments, whether the item is unbreakable, and so on.

**Whenever you're creating a Rebar item, make sure you use `ItemStackBuilder.rebar(<material>, <key>)`.** There are others ways to create [ItemStack]s, but **do not** use these to create Rebar items. 

??? question "Why use `ItemStackBuilder.rebar`, and not any of the other ways to create an [ItemStack]?"
    Under the hood, Rebar stores item keys in [PersistentDataContainer]s, or PDCs. When you call `ItemStackBuilder.rebar` and supply a key, that key is written to the item's PDC automatically. If you supply your own item stack, its PDC won't contain the item's key, and Rebar won't be able to differentiate that item with a regular Minecraft item.

    [ItemStackBuilder] also sets the name and lore of the item to the default translation keys (which will be explained later in this tutorial).

By convention, items go in the `ExampleAddonItems` class. To create a baguette, you can do as follows:
```java title="ExampleAddonItems.java"

public final class ExampleAddonItems {

    // ...

    public static final ItemStack baguette = ItemStackBuilder.rebar(Material.BREAD, baguetteKey)
            .set(DataComponentTypes.FOOD, FoodProperties.food().nutrition(6))
            .build();

    // ...
}
```

!!! info "Data components"
    Data components are just a way to specify some information about an item - like 'this is a food and it restores 6 hunger', or 'this is a pickaxe and it breaks blocks at XYZ speed'. You can see a full list [here](https://jd.papermc.io/paper/1.21.8/io/papermc/paper/datacomponent/DataComponentTypes.html)

### Registering the item

Next, we need to register our item with Rebar. This means we need to pass two things: the item stack, and the class that should be used to represent the item. We'll cover how to make your own item classes in the next chapter, but for now, you can use the default [RebarItem] class:
```java
public final class ExampleAddonItems {

    // ...

    public static final ItemStack baguette = ItemStackBuilder.rebar(Material.BREAD, baguetteKey)
            .set(DataComponentTypes.FOOD, FoodProperties.food().nutrition(6))
            .build();

    // ...

    public static void initialize() {

        // ...

        RebarItem.register(RebarItem.class, baguette);
    }
}
```

### Adding the item to the guide

Finally, we want to add our item to the Rebar guide. Let's add it to the 'food' section.
```java
public final class ExampleAddonItems {

    // ...

    public static final ItemStack baguette = ItemStackBuilder.rebar(Material.BREAD, baguetteKey)
            .set(DataComponentTypes.FOOD, FoodProperties.food().nutrition(6))
            .build();

    // ...

    public static void initialize() {

        // ...

        RebarItem.register(RebarItem.class, baguette);
        PylonPages.FOOD.addItem(baguetteKey);
    }
}
```

---

## Testing the baguette

Let's test our baguette. Start up the test server. Now, you can give yourself the Baguette from the previous section. You can do this using `/py give`. For example, `/py give Idra my-addon:baguette`. If you've done everything right, you should receive your new baguette.

But wait...

What's going on here?

![Baguette with missing translation keys](img/baguette-missing-translation-key.png)

To fix this, we first need to understand the **language system**.

---

## Using the language system

### What is a language system?

!!! info "Feel free to skip this if you know what a language system is"

'Language system' might sound intimidating if you've never used one before, but it's very straightforward. A language system is just a way to make things translateable.

For example, let's suppose I want to create a wonderful new item designed to thrill server admins the world over: the 'Nuclear Bomb'. I write the item code, and then, in my code, I set the item's name to be 'Nuclear Bomb'.
```java
...
// (some code to create the item)
...
item.setName("Nuclear Bomb")
...
```
<small>(not real code - just for demonstration purposes)</small>

Now, suppose we want Spanish speakers to be able to play our addon. Well, in Spanish, that's called 'Bomba Nuclear' according to google translate. But I've hardcoded in 'Nuclear Bomb'... so how can we make sure that Spanish people see 'Bomba Nuclear' instead?

The solution to this is to just use a generic 'translation key'.
```java
...
// (some code to create the item)
...
item.setName("item.nuclear-bomb.name")
...
```
<small>(not real code - just for demonstration purposes)</small>

Now, we can create a different file for each language, containing all the translation keys for that language!
```yaml title="en.yml"
item.nuclear-bomb.name: "Nuclear Bomb"
```
<small>(not real code - just for demonstration purposes)</small>

```yaml title="es.yml"
item.nuclear-bomb.name: "Bomba Nuclear"
```
<small>(not real code - just for demonstration purposes)</small>

Obviously, we'll need some system to substitute in the right translation for the right people, but Rebar will handle that for you automatically. Now, let's see how to do the same thing with Rebar.


### Using Rebar's language system

Remember how we did `item.setName("item.nuclear-bomb.name")` above? In Rebar, you don't need to do that because Rebar **automatically generates the translation key** based on your item's key. All we need to do is create translation files and make sure they contain the correct keys.

Let's do this for the Baguette (fom the previous section). Open the 'en.yml' file ('en' is a code for 'English') in the `src/main/resources/lang` folder.

!!! Note "Adding translations for other languages"
    If we wanted to create a Spanish language file, we would call it 'es.yml' - or 'cs.yml' for Czech, and so on. See [this Wikipedia page](https://minecraft.wiki/w/Language) for a full list of these 2-letter codes.

Next, add this inside the file:
```yaml title="en.yml" hl_lines="3-7"
addon: "<your addon name here>"

item:
  baguette:
    name: "Baguette"
    lore: |-
      <arrow> The best food
```

Note that we have an `addon` key. This is just the name of your addon.

We've also added `name` and `lore` for the baguette. Notice that we're using `baguette` here because that's the key that identifies the baguette, as we specified earlier.

Start up the server again. Your baguette should now have name and lore!
![Baguette with translations](img/baguette.png)

---

## Adding a recipe

### How do recipes work?

Recipes are stored as YAML files in the `resources/recipes` folder of your addon. For example, in Pylon, the recipe for tin nuggets is stored in `resources/recipes/minecraft/crafting_shapeless.yml` and looks like this:

```yaml title="crafting_shapeless.yml"
pylon:tin_nuggets_from_tin_ingot:
  ingredients:
    - pylon:tin_ingot
  result:
    pylon:tin_nugget: 9
```

Each recipe type has a specific YAML structure you must follow when writing recipes for it.

!!! info "Writing recipe configs"
    If you are not sure how to write a config for a specific type of recipe, look for YAML files corresponding to that recipe type. For example, you could look at the [`resources/recipes` folder of Pylon](https://github.com/pylonmc/pylon/tree/master/src/main/resources/recipes/pylon) to see how to create a shimmer altar recipe.

### Creating a recipe for the baguette

Dough can be smelted into bread in a normal furnace or smoker. However, for the baguette, we need something more heavy duty: the blast furnace.

Let's create a recipe that smelts dough into baguettes in a blast furnace.

Pylon base already has [an example of how to write blasting recipes](https://github.com/pylonmc/pylon/blob/master/src/main/resources/recipes/minecraft/blasting.yml), so let's look there.

Based off of that, let's create our recipe. First, create a file `resources/recipes/minecraft/blasting.yml`. Then, insert the following (and change `myaddon` to the key of your addon):
```yaml title="blasting.yml"
pylon:baguette_blasting:
  ingredient: pylon:dough
  result: myaddon:baguette
  experience: 0.5
  category: misc
```

The first line is the **key** of the recipe. All recipes of a given type must have a unique key. The rest of the recipe YAML is fairly self explanatory, except perhaps `category`, which is the category that the recipe should appear in in the crafting book.

!!! info "Finding item keys"
    If you're not sure what an item's key is, hold it in your hand and run `/py key`.

---

## Adding a research

### How do researches work?

Adding a research is even easier than adding a recipe. All of an addon's researches are stored in `resources/researches.yml`. For example, here is the 'improved drilling techniques' research:

```yaml title="researches.yml"
...
improved_drilling_techniques:
  material: iron_pickaxe
  cost: 15
  unlocks:
  - pylon:improved_manual_core_drill
  - pylon:subsurface_core_chunk
...
```

Each research needs a corresponding language entry:
```yaml title="en.yml"
...
research:
  ...
  improved_drilling_techniques: "<#4f4641>Improved drilling techniques"
  ...
```

### Adding a research for the baguette

Let's create a research called 'Baguette Supremacy' that unlocks the baguette.

First, create a file `resources/researches.yml`. Inside the file, paste the following (and change `myaddon` to your addon's key):
```yaml title="researches.yml"
baguette_supremacy:
  material: bread
  cost: 3
  unlocks:
  - myaddon:baguette
```

Next, create a `researches` section in `en.yml` and add an entry for the research as follows:

```yaml title="en.yml"
researches:
  baguette_supremacy: "<#ff0000>BAGUETTE SUPREMACY"
```

!!! question "What's this `<#ff0000>` business?"
    This is [hex color code](https://www.howtogeek.com/761277/what-is-a-hex-code-for-colors/) inside a minimessage tag. You can find out more in [tutorial 3](tutorial-3.md) section.

---

## Full code

```java title="MyAddonKeys.java"
public final class ExampleAddonKeys {
    public static final NamespacedKey baguetteKey = new NamespacedKey(this, "baguette");
}
```

```java title="MyAddonitems.java"
public final class ExampleAddonItems {

    // ...

    public static final ItemStack baguette = ItemStackBuilder.rebar(Material.BREAD, baguetteKey)
            .set(DataComponentTypes.FOOD, FoodProperties.food().nutrition(6))
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
addon: "<your addon name here>"

item:
  baguette:
    name: Baguette
    lore: <arrow> <blue>The <white>best <dark_red>food

researches:
  baguette_supremacy: "<#ff0000>BAGUETTE SUPREMACY"
```

<span></span>

```yaml title="resources/recipes/blasting.yml"
pylon:baguette_blasting:
  ingredient: pylon:dough
  result: myaddon:baguette
  experience: 0.5
  category: misc
```

```yaml title="resources/researches.yml"
baguette_supremacy:
  material: bread
  cost: 3
  unlocks:
  - myaddon:baguette
```

[DataComponentTypes.MAX_STACK_SIZE]: https://jd.papermc.io/paper/1.21.8/io/papermc/paper/datacomponent/DataComponentTypes.html#MAX_STACK_SIZE
[DataComonentTypes.MAX_DAMAGE]: https://jd.papermc.io/paper/1.21.8/io/papermc/paper/datacomponent/DataComponentTypes.html#MAX_DAMAGE
[DataComponentTypes.DAMAGE]: https://jd.papermc.io/paper/1.21.8/io/papermc/paper/datacomponent/DataComponentTypes.html#DAMAGE
[JavaPlugin]: https://jd.papermc.io/paper/1.21.11/org/bukkit/plugin/java/JavaPlugin.html
[RebarAddon]: https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/addon/RebarAddon.html
[NamespacedKey]: https://jd.papermc.io/paper/1.21.11/org/bukkit/NamespacedKey.html
[ItemStackBuilder]: https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/item/builder/ItemStackBuilder.html
[ItemStack]: https://jd.papermc.io/paper/1.21.11/org/bukkit/inventory/ItemStack.html
[RebarItem]: https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/item/RebarItem.html
[PersistentDataContainer]: https://jd.papermc.io/paper/1.21.11/org/bukkit/persistence/PersistentDataContainer.html
