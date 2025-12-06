# Entities

> [!NOTE]
> Still under research!

Entities are `Holder<EntityStore>` with several components attached to them.
For example, the following code adds a Minecart.

```java
protected void spawn(Store<EntityStore> store) {
    // Create a blank entity
    Holder<EntityStore> holder = EntityStore.REGISTRY.newHolder();
    
    // And add all components required. Systems will then add behavior based on the components present.
    Vector3d position = new Vector3f(); // E.g. at the executing players position
    Vector3f rotation = new Vector3f();
    holder.addComponent(TransformComponent.getComponentType(), new TransformComponent(position, rotation));
    holder.ensureComponent(UUIDComponent.getComponentType());

    ModelAsset modelAsset = ModelAsset.getAssetMap().getAsset("Minecart");
    if (modelAsset == null) {
        modelAsset = ModelAsset.DEBUG;
    }

    Model model = Model.createRandomScaleModel(modelAsset);
    holder.addComponent(PersistentModel.getComponentType(), new PersistentModel(model.toReference()));
    holder.addComponent(ModelComponent.getComponentType(), new ModelComponent(model));
    holder.addComponent(BoundingBox.getComponentType(), new BoundingBox(model.getBoundingBox()));

    holder.addComponent(NetworkId.getComponentType(), new NetworkId(store.getExternalData().takeNextNetworkId()));
    holder.ensureComponent(Interactable.getComponentType());
    holder.addComponent(Interactions.getComponentType(), new Interactions());
    holder.ensureComponent(CosineComponent.getComponentType());  // Here wem add custom data!

    store.addEntity(holder, AddReason.SPAWN);
}
```

Which uses a custom component to store data:
```java
public class CosineComponent implements Component<EntityStore> {
    public static final BuilderCodec<CosineComponent> CODEC = BuilderCodec.builder(CosineComponent.class, () -> new CosineComponent(0.0))
            .append(
                    new KeyedCodec<>("Strength", Codec.DOUBLE),
                    (component, strength) -> component.strength = strength,
                    (component) -> component.strength).add()
            .build();
...
```

To actually add new behavior to the entity, an entity system must be added.
A query tests against the existence of required components.
Here, an entity ticker is used to move the Entity around.
```java
public class CosinusSystem extends EntityTickingSystem<EntityStore> {
    private static final Query<EntityStore> QUERY = PluginExample.getInstance().getCosineComponentType();

    @Override
    public void tick(float dt, int index, @Nonnull ArchetypeChunk<EntityStore> archetypeChunk, @Nonnull Store<EntityStore> store, @Nonnull CommandBuffer<EntityStore> commandBuffer) {
        CosineComponent cosineComponent = archetypeChunk.getComponent(index, PluginExample.getInstance().getCosineComponentType());
        TransformComponent transform = archetypeChunk.getComponent(index, TransformComponent.getComponentType());

        double v = cosineComponent.getStrength() + dt;
        cosineComponent.setStrength(v);

        transform.setPosition(transform.getPosition().add(0, Math.cos(v) * 0.1, 0));
    }

    @Nullable
    @Override
    public Query<EntityStore> getQuery() {
        return QUERY;
    }
}
```

Components and systems must be registered as usual:
```java
this.cosineComponentType = this.getEntityStoreRegistry().registerComponent(CosineComponent.class, "Cosine", CosineComponent.CODEC);
this.getEntityStoreRegistry().registerSystem(new CosinusSystem());
```
