Rebar's language files make use of the [MiniMessage](https://docs.advntr.dev/minimessage/index.html) format. MiniMessage has many tags already built in, but Rebar adds new tags on top of that.

!!! info "See [tutorial 3](../../../creating-addons/tutorial-3.md) for a much more in-depth explanation of tags and how to use them."

## List of tags that Rebar adds

| Tag | Description | Example | Example result |
| :-- | :---------- | :------ | :----- |
| <span style="white-space: nowrap;">`<arrow>`<br>`<arrow:[COLOR]>`</span> | Inserts a right arrow (→) with the specified color (default: 0x666666) | <span style="white-space: nowrap;">`<arrow> Watermelon`</span> | ![arrow](img/arrow.png) |
| <span style="white-space: nowrap;">`<guidearrow>`</span> | Shorthand for `<arrow:0x653d6d>`; used in the Rebar guide | <span style="white-space: nowrap;">`<guidearrow> Watermelon`</span> | ![guidearrow](img/guidearrow.png) |
| <span style="white-space: nowrap;">`<diamond>`<br>`<diamond:[COLOR]>`</span> | Inserts a diamond (◆) with the specified color (default: 0x666666) | <span style="white-space: nowrap;">`<diamond> Watermelon`</span> | ![diamond](img/diamond.png) |
| <span style="white-space: nowrap;">`<star>`<br>`<star:[COLOR]>`</span> | Inserts a star (★) with the specified color (default: NamedTextColor.BLUE) | <span style="white-space: nowrap;">`<star> Watermelon`</span> | ![star](img/star.png) |
| <span style="white-space: nowrap;">`<insn></insn>`</span> | Applies a yellow styling (0xf9d104), used for instructions | <span style="white-space: nowrap;">`<insn>Right click</insn>`</span> | ![insn](img/insn.png) |
| <span style="white-space: nowrap;">`<guidehint></guidehint>`</span> | Applies a light purple styling (0xeac5f4), used for guide hints | <span style="white-space: nowrap;">`<guidehint>Right click</guidehint>`</span> | ![guidehint](img/guidehint.png) |
| <span style="white-space: nowrap;">`<guideinsn></guideinsn>`</span> | Applies a purple styling (0xc907f4), used for guide instructions | <span style="white-space: nowrap;">`<guideinsn>Right click</guideinsn>`</span> | ![guideinsn](img/guideinsn.png) |
| <span style="white-space: nowrap;">`<story></story>`</span> |  Applies a light purple italic styling (0xde76e0), used for story text | <span style="white-space: nowrap;">`<story>Right click</story>`</span> | ![story](img/story.png) |
| <span style="white-space: nowrap;">`<attr></attr>`</span> | Applies a cyan styling (0xa9d9e8), used for attributes | <span style="white-space: nowrap;">`<attr>Watermelons:</attr> 5`</span> | ![attr](img/attr.png) |
| <span style="white-space: nowrap;">`<unit:[unit]></unit>`<br>`<unit:[prefix]:[unit]></unit>`</span> | Formats a constant number as a unit, with an optional metric prefix | <span style="white-space: nowrap;">`<unit:seconds>1000</unit>`</span> | ![unit](img/unit.png) |
| <span style="white-space: nowrap;">`<nbsp></nbsp>`</span> | Replaces spaces with non-breaking spaces, preventing line breaks | <span style="white-space: nowrap;">`<nbsp>Watermelon</nbsp>`</span> | ![nbsp](img/nbsp.png) |
| <span style="white-space: nowrap;">`<item:[item_name]>`</span> | Renders the translated name of a vanilla/rebar item | <span style="white-space: nowrap;">`<item:pylon:bandage>`</span> | ![item](img/item.png) |
| <span style="white-space: nowrap;">`<entity:[entity_type]>`</span> | Renders the translated name of an entity type | <span style="white-space: nowrap;">`<entity:creeper>`</span> | ![enitity](img/entity.png) |
| <span style="white-space: nowrap;">`<effect:[effect_type]>`</span> | Renders the translated name of a potion effect | <span style="white-space: nowrap;">`<effect:speed>`</span> | ![effect](img/effect.png) |
| <span style="white-space: nowrap;">`<enchant:[enchant_name]>`</span> | Renders the translated name of an enchantment | <span style="white-space: nowrap;">`<enchant:sharpness>`</span> | ![enchant](img/enchant.png) |
| <span style="white-space: nowrap;">`<biome:[biome_name]>`</span> | Renders the translated name of a biome | <span style="white-space: nowrap;">`<biome:plains>`</span> | ![biome](img/biome.png) |

