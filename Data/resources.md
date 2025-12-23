# Resources

Resources are Codec backed JSONs per world, stored in `universe/worlds/<id>/resources/`.

Define a class and codec:

```java
public class CustomResource implements Resource<EntityStore> {
    // E.g. via the CodecBuilder
    public static final BuilderCodec<CustomResource> CODEC;
}

// Common but optional getter pattern
public static CustomResource<EntityStore, CustomResource> getResourceType() {
    return MainClass.getInstance().getCustomResource();
}
```

Register it in `setup`, use proper namespaces to avoid file collisions:

```java
var resource = this.getEntityStoreRegistry().registerResource(CustomResource.class, "GroupModName", CustomResource.CODEC);
```

And fetch it anywhere using:

```java
CustomResource resource = ref.getStore().getResource(CustomResource.getResourceType());
```