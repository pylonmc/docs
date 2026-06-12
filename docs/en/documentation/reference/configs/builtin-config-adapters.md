This page contains examples (and extra documentation where needed) for every ConfigAdapter bundled with Rebar.

## Simple adapters

### ConfigAdapter.BYTE

Range: -128 to 127

```yaml
example: 5
```

### ConfigAdapter.SHORT

Range: -32768 to 32767

```yaml
example: 5
```

### ConfigAdapter.INTEGER

Range: -2147483648 and 2147483647

```yaml
example: 5
```

### ConfigAdapter.LONG

Range: -9223372036854775808 and 9223372036854775807

```yaml
example: 5
```

### ConfigAdapter.FLOAT

```yaml
example-1: 5
example-2: 5.2
```

### ConfigAdapter.DOUBLE

```yaml
example-1: 5
example-2: 5.2
```

### ConfigAdapter.INT_RANGE

```yaml
example-1: [3, 8]
example-2:
    min: 3
    max: 8
example-3: 5 # 5 <= x <= 5
```

### ConfigAdapter.CHAR

```yaml
example-1: 'h'
example-2: "h"
```

### ConfigAdapter.BOOLEAN

```yaml
example: true
```

### ConfigAdapter.ANY

A special config adapter which returns the raw type returned by [ConfigurationSection#get](https://jd.papermc.io/paper/26.1.2/org/bukkit/configuration/ConfigurationSection.html#get(java.lang.String)).

You should basically never need to use this.

### ConfigAdapter.STRING

```yaml
example: "some text"
```

---

## Collection adapters

### ConfigAdapter.LIST

```yaml
# All examples read using ConfigAdapter.LIST.from(ConfigAdapter.INTEGER)

example-1:
    - 5
    - 7

example-2: [5, 7]

example-3: []
```

### ConfigAdapter.MAP

```yaml
# Read using ConfigAdapter.MAP.from(ConfigAdapter.INTEGER, ConfigAdapter.STRING)
example-1:
  5: "wat"
  9: "erm"
  66: "elon"

# Read using:
# ConfigAdapter.MAP.from(
#         ConfigAdapter.LIST.from(ConfigAdapter.INTEGER), 
#         ConfigAdapter.STRING)
# );
example-2:
  - key: [2, 6]
    value: "wat"
  - key: [7, 9, 3]
    value: "erm"
  - key: []
    value: "elon"
```


