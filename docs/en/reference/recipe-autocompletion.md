# Recipe Autocompletion

`AUTHOR: Vaan1310`
`HAS CONTEXT: Vaan1310`

In order to not have pylon items autocomplete into vanilla recipes, we have to use NMS and override 2 behaviours:
- Item movement to crafting table
- The accounting logic for recipes

## Starting point

We listen to `PlayerRecipeBookClickEvent` and obtain the NMS data that was getting processed.

## Fix the movement logic

`HandlerRecipeBookClick` is a class made to contain most of the basic logic.

As many other functionalities depend on the recipe book, we only consider the cases regarding the crafting table.

`HandlerRecipeBookClick#handlePylonItemPlacement` method should handle how placement takes place, we have a method, but normally it is handled by a method in `AbstractCraftingMenu`, so we need to use reflection to call some stuff around to simulate normal behaviour and just change what we need.

> [!NOTE]
> To optimize most of the calls here, everything uses method handles, these methods might be called a lot of times, so they have to be performant

`ServerPlaceRecipe` handles the core logic for moving items and accounting them, however most of it is private, that is why we create our own version, that delegates most behaviour to the NMS class.

After following a chain of private method calls which, of course, we cannot override ( `tryPlaceRecipe` → `placeRecipe` → `moveToGrid` → `findSlotMatchingCraftingIngredient`), we arrive at one of the targets.

The objective is to exclude from `ItemOrExact.Item` matches our pylon items. 

> [!NOTE]
> Minecraft matches items in 2 ways: 
> - ItemOrExact.Item (which is Material based matching), 
> - ItemOrExact.Exact (pdc and component, perfect matching).

## Fix the accounting logic

Now items doesn't get moved to grid as expected, but it doesn't fail either, so we need to send a ghost recipe in said cases, this is handled by `StackedItemContents#canCraft`

`StackedItemContents` basically accounts for every item in the inventory in his own way, so I made a bunch of reflection in order to add a makeshift method: `accountPylonItems` which makes pylon item stacks be process as ItemOrExact.Exact ALWAYS