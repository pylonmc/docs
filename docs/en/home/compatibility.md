!!! info "Found a compatibility issue with another plugin? Want to help fix an incompatibility?"
    Please let us know on our [Discord server](https://discord.gg/4tMAnBAacW)

### Integrations

There are all the plugins Rebar integrates with to provide extra features or solve issues that are specific to that plugin.

| Plugin                            | Details                               |
|-----------------------------------|---------------------------------------|
| PlaceholderAPI                    | See the list of Rebar placeholders [here](placeholders.md) |

### Known compatiblity issues

These are all the plugins known to cause issues with Rebar.

| Plugin                            | Status | Details                               |
|-----------------------------------|--------|-------------------------------|
| WorldEdit                         | <span class="compatibility-status-minor-incompatibilities">Minor incompatibilities</span> | Works fine but do not try to WorldEdit Rebar blocks. |
| Axiom                             | <span class="compatibility-status-minor-incompatibilities">Minor incompatibilities</span> | Works fine but do not try to edit Rebar blocks using Axiom. |
| InteractiveChat                   | <span class="compatibility-status-minor-incompatibilities">Minor incompatibilities</span> | Works fine but Rebar messages shown in InteractiveChat are sometimes not translated correctly. |
| CMI                               | <span class="compatibility-status-minor-incompatibilities">Minor incompatibilities</span> | Works fine but items shown in chat with `[item]` are not translated correctly. |
| CraftEngine                       | <span class="compatibility-status-moderate-incompatibilities">Moderate incompatibilities</span> (mitigation available) | Breaks some of Rebar's recipes. If needed, you can modify the affected recipes to fix them by running `/rb exposerecipes` and editing the recipe files. |
| RoseStacker                       | <span class="compatibility-status-do-not-use">Do not use</span> | Introduces duplication bugs with Rebar items |

