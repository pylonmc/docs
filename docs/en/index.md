Pylon is an upcoming Minecraft Java technology plugin that will hugely expand vanilla gameplay with new content: electricity, diesel machines, new fluids and fluid pipes, logistics, and much, much more!

Pylon is built using Rebar a powerful framework that allows adding custom blocks, items, researches, etc. Rebar also implements complex systems such as fluid handling logic and cargo routing. **Pylon is actually a Rebar addon**.

[:simple-discord: Join us on Discord :material-arrow-right:](https://discord.gg/4tMAnBAacW){ .md-button } [:simple-github: Find us on GitHub :material-arrow-right:](https://github.com/pylonmc){ .md-button } [:material-server: Install Pylon :material-arrow-right:](home/installing-pylon.md){ .md-button }

Keep reading to find out more.

---

## :calendar: Where are we at?
| Expected time of completion | Event |
|:----------------------------| :---- |
| <s>**December 2025**</s>    | <s>Most/all diesel machines fully implemented</s> (DONE) |
| <s>**January 2026**</s>     | <s>Cargo system fully implemented</s> (DONE) |
| <s>**February 2026**</s>    | <s>Open alpha testing (likely hosted on MetaMechanists - my own server) begins.</s> (Playtesting started) |
| **April 2026**              | Electricity fully implemented |
| **Mid 2026**                | Pylon officially released |

## :wrench: Pylon's features

| Status | Feature |
| :----- | :------ |
| :white_check_mark: | Manual machines (mixing pot, grindstone, magic altar, hammer, and much more!)
| :white_check_mark: | Hydraulic machines (early-game automation)
| :white_check_mark: | Fluid pipes & new fluids
| :white_check_mark: | Smelting & alloying system
| :white_check_mark: | Multiblocks
| :white_check_mark: | Research system (unlock new items with research points)
| :white_check_mark: | Per-player language support
| :white_check_mark: | Comprehensive and user-friendly in-game guide
| :white_check_mark: | Extensive server customisation options (including per-machine settings and customisable recipes)
| :white_check_mark: | Performance tuning options (limit the number of blocks per chunk/player, configure tick rates, etc)
| :white_check_mark: | First-class texture pack support + 'official' texture pack (including blocks)
| :white_check_mark: | Diesel machines (mid-game automation)
| :white_check_mark: | Cargo system (automatically move items from A to B)
| :o: | Electric machines (end-game automation)
| :o: | AE2-style endgame logistics system
| :o: | Builtin profiler for determining what's causing the most lag
| :o: | Bedrock support (via Geyser)

## :frame_photo: Gallery

|    |    |
| :- | :- |
| ![A Pylon fluid setup](img/fluid-setup.png) | ![Hydraulic fluid setup](img/hydraulic-fluid-setup.png) 
| ![Copper fluid tank](img/looking-at-copper-fluid-tank.png) | ![Medkit in guide](img/medkit.png) |
| ![Placing copper pipes](img/placing-pipes.png) | ![Using the hydraulic grindstone turner](img/using-grindstone-turner.png) |
| ![Searching items in the Pylon guide](img/searching-items.png) | ![Purification tower config](img/purification-tower-config.png) |
| ![Using the magic altar](img/using-magic-altar.png) | ![Using the smeltery](img/using-smeltery.png) |
| ![Pylon research in the guide](img/looking-at-research.png) | ![Hovering over research pack in Pylon guide](img/hovering-over-research-pack.png) |

## :detective: Meet the team

| Role | Member | Bio |
| ---- | ------ | --- |
| <span style="color: #ff0000;">Lead Developer</span> | :flag_us: Seggan | A veteran Slimefun addon developer (SlimefunWarfare, SFCalc, Galactifun), with a impressive contribution record for SF addons, Paper, and Slimefun. Seggan is responsible for many core Rebar and Pylon systems, including translation, WAILA, researches, the smeltery, the recipe system, and more. |
| <span style="color: #ff0000;">Lead Developer</span> | :flag_ca: Ohm | New to plugin development, but has done a fantastic job getting up to speed and adding lots of base content, like talismans and the beheading sword, as well as a *lot* of various smaller technical changes & bug fixes. |
| <span style="color: #ff0000;">Lead Developer</span> | :flag_gb: Idra | Owner of a Slimefun server for 5 years & Quaptics developer. I have developed many of the core systems including the Pylon guide, fluid system, hydraulics, cargo, diesel, automated tests, block storage, and more|
| <span style="color: #e67e22;">Developer</span> | :flag_it: Vaan | Previously headed a geopolitics server and has been doing a lot of valuable work on the 'smaller stuff' - resolving issues, fixing bugs, adding polishing, making technical changes, and lots more. |
| <span style="color: #e67e22;">Developer</span> | :flag_us: Justin | Head developer on my server for 3 years and works professionally in the Minecraft space. He has done the majority of the texture pack support side of Pylon, and other various smaller refinements.
| <span style="color: #f8ac2e;">Artist</span> | :flag_cz: Pandicka | A talented texture pack artist who worked on Slimefun texture packs, and has created the majority of the Pylon resource pack. |
| <span style="color: #bc915c;">Contributor</span> | :flag_cn: Balugaq | Former Slimefun addon developer (JustEnoughGuide, SlimefunTimeIt, MSUA, AdvancedBan, and more) who has contributed to Pylon/Rebar (primarily the recipe ingredient calculation system) and also has already written two addons. |
| <span style="color: #bc915c;">Contributor</span> | :flag_us: Blueb | Has run a Slimefun server (Orchid) for several years and has added some neat content like the elevator. |
| <span style="color: #bc915c;">Contributor</span> | :flag_cn: Ybw0014 | Former Slimefun addon developer who ran the Slimefun community. Helped with some of the Pylon/Rebar docs. |

---

## :tools: For server admins

### Performance

- :gear: Even huge multiblocks will have **almost zero** performance impact compared to a normal Rebar block
- :gear: Most of Rebar/Pylon will eventually run asynchronously
- :gear: Performance is literally built into Rebar/Pylon's design - systems like fluid pipes and cargo were designed from the ground up in the most performant way possible
- :gear: You will be able to limit the number of each type of block per player or per chunk
- :gear: Pylon/Rebar will have many more performance options, such as making fluid pipes tick less often, or reducing tick rates of individual types of machines
- :gear: We plan to add a purpose-built profiler so you can see exactly what blocks & items are using the most CPU or memory

### Stability

- :gear: Easily disable any problematic blocks or items
- :gear: Rebar or Rebar addons will refuse to start if it detects any configuration issues
- :gear: Blocks that throw errors will be safely unloaded
- :gear: Removing addons is safe, with all the data kept intact and restored if the addon is re-added
- :gear: Rebar data is stored **in the world data itself** - no need to keep extra backups

### Customisation

- :gear: The unlocks and costs of each research are configurable
- :gear: All recipes are configurable
- :gear: Most blocks and items have settings determining their tick rate, speed, diesel usage, etc

---

## :keyboard: For developers

### Addon development

- :gear: You can add content by writing a Rebar addon
- :gear: Rebar supports addons written in Kotlin
- :gear: Blocks, items, recipes, guide pages, fluids, and entities are all easy and intuitive to add
- :gear: Rebar will have extensive high-level documentation on all the features it has to offer

!!! warning "Currently, addon development is not supported due to how rapidly Pylon is still changing."

### Examples

See the following code to get a feel for how Rebar works:

| Example | Link                                                                                                                                                                                                                                            |
| ------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Portable dustbin | [https://github.com/pylonmc/pylon/blob/master/src/main/java/io/github/pylonmc/pylon/content/tools/PortableDustbin.java](https://github.com/pylonmc/pylon/blob/master/src/main/java/io/github/pylonmc/pylon/content/tools/PortableDustbin.java)  |
| Fireproof rune | [https://github.com/pylonmc/pylon/blob/master/src/main/java/io/github/pylonmc/pylon/base/content/tools/FireproofRune.java](https://github.com/pylonmc/pylon/blob/master/src/main/java/io/github/pylonmc/pylon/content/tools/FireproofRune.java) |
| Hammer recipe type | [https://github.com/pylonmc/pylon/blob/master/src/main/java/io/github/pylonmc/pylon/recipes/HammerRecipe.java](https://github.com/pylonmc/pylon/blob/master/src/main/java/io/github/pylonmc/pylon/recipes/HammerRecipe.java)                    |
| Hammer recipe files | [https://github.com/pylonmc/pylon/blob/master/src/main/resources/recipes/pylon/hammer.yml](https://github.com/pylonmc/pylon/blob/master/src/main/resources/recipes/pylonbase/hammer.yml)                                                        |
| Pylon English language file | [https://github.com/pylonmc/pylon/blob/master/src/main/resources/lang/en.yml](https://github.com/pylonmc/pylon/blob/master/src/main/resources/lang/en.yml)                                                                                      |
| Press block | [https://github.com/pylonmc/pylon/blob/master/src/main/java/io/github/pylonmc/pylon/content/machines/simple/Press.java](https://github.com/pylonmc/pylon/blob/master/src/main/java/io/github/pylonmc/pylon/content/machines/simple/Press.java)  |
| Hydraulic excavator settings | [https://github.com/pylonmc/pylon/blob/master/src/main/resources/settings/hydraulic_breaker.yml](https://github.com/pylonmc/pylon/blob/master/src/main/resources/settings/hydraulic_excavator.yml)                                              |

---

## :question: Q&A

| Question | Answer                                                                                                                                                                                                                                                                                                                                                                 |
| -------- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **How do I install Pylon?** | Read the installation guide at [https://pylonmc.github.io/home/installing-pylon/](https://pylonmc.github.io/installation/installing-pylon/). Beware that **Pylon and Rebar are still experimental and you should not run them outside of an expendable test server.**                                                                                                  |
| **Will Rebar support Slimefun addons?** | No. Migrating from Slimefun to Rebar is non-trivial, and we advise addon developers to rewrite their addons entirely to better suit Pylon's progression and style rather than attempting to migrate them 1-to-1.                                                                                                                                                       |
| **What versions will Rebar/Pylon support?** | We plan to keep each Pylon and Rebar version compatible with the latest version of Minecraft at the time. To make it easier and faster for us to update, **each version of Pylon/Rebar will only support one version of Minecraft.** This means you will need to use older versions of Pylon/Rebar for older versions of Minecraft. Critical fixes will be backported. |
| **What server software can Pylon/Rebar run on?** | Paper or paper forks only. Folia support may come later down the line.                                                                                                                                                                                                                                                                                                 |
| **Will Pylon/Rebar support bedrock (via Geyser)?** | Eventually, yes, but it is not a high priority and will be one of the last things added. Geyser is very hard for a project like this to support and there will be some jank.                                                                                                                                                                                           |
| **When will Pylon be ready?** | See the 'Provisional Timeline' section above                                                                                                                                                                                                                                                                                                                           |
| **How can I submit a translation?** | We do not currently accept translations due to how quickly the plugin is changing (any translation would go out of date quickly). There will be an announcement when we are ready for translations.                                                                                                                                                                    |

**If you have a different question, drop a message on [Discord](https://discord.gg/4tMAnBAacW) and we'll be happy to answer.**
