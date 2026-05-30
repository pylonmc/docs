As you probably know, in Minecraft plugins, configs are stored in YAML. Normally in Bukkit/Paper, you would use [YamlConfiguration](https://jd.papermc.io/paper/26.1.2/org/bukkit/configuration/file/YamlConfiguration.html) and [ConfigurationSection](https://jd.papermc.io/paper/26.1.2/org/bukkit/configuration/ConfigurationSection.html) to read a config file. `YamlConfiguration` and `ConfigurationSection` have a lot of quirks, lack a lot of useful functionality, and are inflexible. Because of this (well, there are other reasons, described shortly) we did what every developer does when they encounter a problem: create a new level of abstraction. Sorry.

Enter [Config](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/config/Config.html) and [ConfigSection](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/config/ConfigSection.html). `Config` is a simple wrapper around `YamlConfiguration` and `ConfigSection` is a simple wrapper around `ConfigurationSection`.



