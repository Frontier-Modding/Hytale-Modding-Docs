# Getting Started

This document details how to create and register a custom configuration file using the server's `Codec` system.

The configuration class acts as a data model that defines the variables to be saved in the JSON file and how they are serialized/deserialized.

## Code Structure

The following snippet shows how to define the `Config` class and its associated `CODEC`:

```java
public class ExampleConfig {

    // 1. Codec definition for serialization/deserialization
    public static final BuilderCodec<ExampleConfig> CODEC = BuilderCodec.builder(ExampleConfig.class, ExampleConfig::new)
        .append(new KeyedCodec<Double>("LuckIncreaseChance", Codec.DOUBLE),
                (exConfig, aDouble, extraInfo) -> exConfig.LuckIncreaseChance = aDouble, // Setter
                (exConfig, extraInfo) -> exConfig.LuckIncreaseChance)                    // Getter
        .add()
        .build();

    // 2. Configuration variable with default value
    private double LuckIncreaseChance = 0.40;

    public ExampleConfig() {
    }
}
```


## Registration in the Main Plugin

For the server to recognize and load the configuration file, it must be registered during the main plugin's initialization.

```java
public class ExamplePlugin extends JavaPlugin {

    private final Config<ExampleConfig> config;

    public ExamplePlugin(@Nonnull JavaPluginInit init) {
        super(init);
        // Registers the configuration with the filename "ExamplePlugin"
        this.config = this.withConfig("ExamplePlugin", ExampleConfig.CODEC);
    }
}
```


*   **`this.withConfig(...)`**: This method links the file (which will be generated as `com.buuz135_Example Plugin/ExamplePlugin.example.json` (`{Group}_{Name}/{ConfigName.example.json}`) inside the `plugins` folder in the root directory) with the `CODEC` defined earlier. If the file does not exist, it will be created with the default values defined in `ExampleConfig`.





