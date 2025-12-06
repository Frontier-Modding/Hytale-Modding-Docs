# Custom Asset Stores

Asset stores are custom json directories.

```java
public class CustomAsset implements JsonAssetWithMap<String, DefaultAssetMap<String, CustomAsset>> {
    public static final AssetBuilderCodec<String, CustomAsset> CODEC = AssetBuilderCodec.builder(
                    CustomAsset.class,
                    CustomAsset::new,
                    Codec.STRING,
                    (t, id) -> t.id = id,
                    t -> t.id,
                    (t, data) -> t.data = data,
                    t -> t.data
            )
            .appendInherited(
                    // Your fields
            )
            .add()
            .afterDecode((o, extrainfo) -> {
                if (o.id != null && extrainfo instanceof AssetExtraInfo<?> assetExtraInfo) {
                    Path path = assetExtraInfo.getAssetPath();
                    // Here you can add additional post processing, including things like loading auxiliary/non-json files
                }
            })
            .build();


    private static AssetStore<String, CustomAsset, DefaultAssetMap<String, CustomAsset>> ASSET_STORE;

    public static AssetStore<String, CustomAsset, DefaultAssetMap<String, CustomAsset>> getAssetStore() {
        if (ASSET_STORE == null) {
            ASSET_STORE = AssetRegistry.getAssetStore(CustomAsset.class);
        }
        return ASSET_STORE;
    }

    private String id;
    private AssetExtraInfo.Data data;
}
```

Register it in your setup:

```java
AssetRegistry.register(HytaleAssetStore.builder(CustomAsset.class, new DefaultAssetMap<>())
        .setPath("DirectoryName/Can/Be/Nested")
        .setCodec(CustomAsset.CODEC)
        .setKeyFunction(CustomAsset::getId)
        .build()
);
```

Custom data can then be fetched by its id (file name):
```java
CustomAsset asset = MelodyAsset.getAssetStore().getAssetMap().getAsset("FileName");
```
