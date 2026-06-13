You can create your own [ConfigAdapter](https://pylonmc.github.io/rebar/docs/javadoc/io/github/pylonmc/rebar/config/adapter/ConfigAdapter.html) for any type you want, as long as you can write the logic to deserialize it.

A ConfigAdapter needs to implement the `getType` method, which returns the class of the type that the config adapter is converting to, and the `convert` method. The convert method takes in an object and tries to convert it to the config adapter's type.

Since we use Bukkit's deserialization logic, the provided object can be anything that [ConfigurationSection#get](https://jd.papermc.io/paper/26.1.2/org/bukkit/configuration/ConfigurationSection.html#get(java.lang.String)) can return.

It's recommended to [look at some of Rebar's builtin config adapters](https://github.com/pylonmc/rebar/tree/master/rebar/src/main/kotlin/io/github/pylonmc/rebar/config/adapter) to learn how they work if you are confused on how to write a new one.

!!! example "Example: UUID config adapter"
  
    ```java
    class UUIDConfigAdapter implements ConfigAdapter<UUID> {
    
        @Override
        public @NotNull Type getType() {
            return UUID.class;
        }
    
        @Override
        public UUID convert(@NotNull Object value) {
            if (value instanceof String string) {
                return java.util.UUID.fromString(string);
            }
    
            List<Long> longs = ConfigAdapter.LIST.from(ConfigAdapter.LONG).convert(value);
            Preconditions.checkState(longs.size() == 2, "Expected a list of 2 longs for UUID, got " + longs.size());
            return new UUID(longs.get(0), longs.get(1));
        }
    }
    ```

!!! tip
    Test your config adapter with invalid inputs to check that the error messages it gives are useful.

!!! tip
    Often it can be simpler to just use `ConfigSection`'s methods than to write a new ConfigAdapter.

