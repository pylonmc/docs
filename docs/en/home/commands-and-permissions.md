# Commands & permissions

## Default commands
| Command                            | Permission                               | Description |
|------------------------------------|------------------------------------------|-------------|
| `/rb`                              | `rebar.command.guide`                    | Opens the Rebar guide
| `/rb guide`                        | `rebar.command.guide`                    | Gives you the Rebar guide

## Admin commands
| Command                                          | Permission                               | Description |
|--------------------------------------------------|------------------------------------------|-------------|
| `/rb confetti <amount> [speed] [lifetime]`       | `rebar.command.confetti`                 | Spawns confetti particles
| `/rb debug`                                      | `rebar.command.debug`                    | Gives you the debug item, which can be used to view Rebar block/entity data and forcibly remove Rebar blocks/entities
| `/rb exposerecipeconfig <addon> <recipe_type>`   | `rebar.command.exposerecipeconfig`       | Allows recipes to be edited for a specific recipe stype. Doing this will prevent any recipes of the given type from being automatically updated, so use with caution!
| `/rb fill <x1> <y1> <z1> <x2> <y2> <z2> <block>` | `rebar.command.setblock`                 | Sets the given Rebar block at the coordinates provided
| `/rb fillmultiblock`                             | `rebar.command.fillmultiblock`           | Attempts to complete the multiblock you are looking at
| `/rb give <player> <item> [amount]`              | `rebar.command.give`                     | Gives a player a Rebar item
| `/rb key`                                        | `rebar.command.key`                      | Shows you the key of the Rebar item you're holding in your hand
| `/rb setblock <x> <y> <z> <block>`               | `rebar.command.setblock`                 | Sets the given Rebar block at the coordinates provided
| `/rb research add <player> <research>`           | `rebar.command.research.add`             | Adds the given research to a player
| `/rb research list <player> <research>`          | `rebar.command.research.list`            | Lists the researches a player has
| `/rb research addall <player>`                   | `rebar.command.research.addall`          | Adds all researches to a player
| `/rb research remove <player> <research>`        | `rebar.command.research.remove`          | Removes the given research from a player
| `/rb research points add <player> <amount>`      | `rebar.command.research.points.add`      | Adds research points to a player
| `/rb research points get <player>`               | `rebar.command.research.points.get`      | Shows you the number of research points a player has
| `/rb research points set <player> <amount>`      | `rebar.command.research.points.set`      | Sets the number of research points a player has
| `/rb research points subtract <player> <amount>` | `rebar.command.research.points.subtract` | Removes research points from a player
| `/rb setphantom <x> <y> <z>`                     | `rebar.command.setphantom`               | Converts a block to a phantom block
| `/rb versions`                                   | `rebar.command.versions`                 | Shows version information about server software, Rebar, and any addons

## Other permissions
| Permission                     | Description |
|--------------------------------|-------------|
| `rebar.guide.cheat`            | Allows you to use drop, ctrl+drop, or middle click to cheat in items from the Rebar guide |
| `rebar.guide.view_admin_pages` | Allows you to see admin-only guide pages                                                  |
