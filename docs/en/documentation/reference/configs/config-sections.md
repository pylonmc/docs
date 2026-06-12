As you probably know, in Minecraft plugins, configs are stored in YAML. Normally in Bukkit/Paper, you would use [YamlConfiguration](https://jd.papermc.io/paper/26.1.2/org/bukkit/configuration/file/YamlConfiguration.html) and [ConfigurationSection](https://jd.papermc.io/paper/26.1.2/org/bukkit/configuration/ConfigurationSection.html) to read a config file. `YamlConfiguration` and `ConfigurationSection` have a lot of quirks, lack a lot of useful functionality, and are inflexible. Because of this, we did what every developer does when they encounter a problem: create a new level of abstraction. Sorry.

Specifically, we created [ConfigSection](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/config/ConfigSection.html). This is a modern wrapper around ConfigurationSection which makes reading from configs easier, provides better error messages, adds a caching layer to improve performance, and makes plenty of other improvements.

## Creating a ConfigSection

You can read a config into a ConfigSection using ConfigSection's static constructor methods:

| Action | Methods |
|--------|---------|
| Directly turning a YamlConfiguration into a ConfigSection | `from(String name, ConfigurationSection configSection)`
| Reading a config from a file | `from(File file)`<br> `from(Path path)`<br> `fromOrThrow(File file)`<br> `fromOrThrow(Path path)`
| Reading a config from your plugin's folder in `plugins` | `fromDataFolder(Plugin plugin, String path)`<br> `from(Plugin plugin, String path)`
| Reading a resource embedded in your plugin (under your `resources` folder) | `fromResource(Plugin plugin, String path)`<br>`fromResourceOrThrow(Plugin plugin, String path)`
| Reading a key's settings file | `fromSettings(NamespacedKey key)` |

## Reading from configs

To read from a ConfigSection, you can use the `get` or `getOrThrow` methods. These methods require you to supply a [ConfigAdapter](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/config/adapter/ConfigAdapter.html). A ConfigAdapter is just a class which explains how to turn a part of config into a Java/Kotlin value. For example, the `ConfigAdapter.INT` config adapter allows an integer to be read from a config.

`getOrThrow` throws an exception if the key is not found. You should use this where a config key is required, as it provides a useful error message if the key is not found. You can also use `get` which just returns null if the key is not found.

!!! example

    ```yaml title="pylon/some_config.yml"
    some-value: 5
    some-other-value: "hello"
    ```

    ```java
    int x = section.getOrThrow("some-value", ConfigAdapter.INTEGER);
    // x = 5
    ```

??? question "How can I create my own config adapter?"
    See the [config adapter docs page](config-adapter.md).

## Copying & merging resources

Plugins have a `resources` folder containing files relevant to your plugin. When your plugin is compiled into a .jar, the .jar file contains the resources folder. You can therefore access any of the resources at runtime. 

You will often want to copy a config into your plugin's data folder (the plugin's folder in `plugins`). For example, if you add a default config.yml to your resource folder, you will want to copy it into your plugin's data folder if it does not already contain a config.yml. You can do this with the [copyResource](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/config/ConfigSection.html#copyResource(org.bukkit.plugin.Plugin,java.lang.String)) method. 

If the file already exists in your plugin's data folder when you call this method, the file in your resources folder is merged on top of it. This means that any keys that do not exist in the data folder's file but do exist in the resource folder's file are copied from the resource folder's file into the data folder's file. **See the example below if confused.**

This allows for new config values to be added to existing config files while preserving any changes that might have been made to existing config values.

!!! example
    If `plugins/ExampleAddon/some_config.yml` contains the following:

    ```yaml title="plugins/ExampleAddon/some_config.yml"
    some-key: 5
    some-section:
        some-other-key: 8
    ```

    ...and `resources/some_config.yml` contains the following:

    ```yaml title="plugins/ExampleAddon/some_config.yml"
    some-key: 5
    some-section:
        some-other-key: 3
        some-other-other-key: true
    some-other-section: 
        oh-no: true
    ```

    ...and `ConfigSection.copyResource(ExampleAddon.getInstance(), "some_config")` is called, then `plugins/ExampleAddon/some_config.yml` will end up as:

    ```yaml title="plugins/ExampleAddon/some_config.yml"
    some-key: 5
    some-section:
        some-other-key: 8
        some-other-other-key: true
    some-other-section: 
        oh-no: true
    ```


## Caching

In order to reduce performance overhead if you are doing lots of repeated reads (common for settings), any values read from a ConfigSection are cached. Any calls to `set` invalidate the corresponding cache entry. You do not generally need to worry about the cache; it works automatically.

