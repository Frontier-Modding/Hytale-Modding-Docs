# Interactions

> [!NOTE]
> Still under research!

Interactions are called in various places, such as tool/weapon behavior.

## Create an Interaction
Interactions extend from Simple(Instant)Interaction.
Notice that custom interactions can only be server-sided; packets like ProjectileInteraction rely on the hardcoded client/protocol.

## Create Interaction Assets

Using the Asset Editor you will need to create a new Interactions for the type `Utterance` (the same as the codec) and RootInteraction referencing the new Interaction.

```java
public class UtteranceInteraction extends SimpleInstantInteraction {
    // Here we add a single config to store additional data, where UtteranceConfig is a Codec extending from NetworkSerializable.
    protected UtteranceConfig config;

    protected void firstRun(@Nonnull InteractionType type, @Nonnull InteractionContext context, @Nonnull CooldownHandler cooldownHandler) {
        //Server-sided magic, e.g. playing a sound.
        InteractionSyncData clientState = context.getClientState();
        CommandBuffer<EntityStore> commandBuffer = context.getCommandBuffer();
        if (clientState == null) return;
        if (commandBuffer == null) return;
        Ref<EntityStore> entity = context.getEntity();
        TransformComponent transform = commandBuffer.getComponent(entity, TransformComponent.getComponentType());
        if (transform == null) return;
        SoundUtils.playSoundEvent3d(config.getLaunchWorldSoundEventIndex(), SoundCategory.SFX, transform.getPosition(), commandBuffer);
    }

    public static final BuilderCodec<UtteranceInteraction> CODEC = BuilderCodec.builder(UtteranceInteraction.class, UtteranceInteraction::new, SimpleInstantInteraction.CODEC)
            .documentation("Yaps.")
            .appendInherited(
                    new KeyedCodec<>("UtteranceConfig", UtteranceConfig.CODEC),
                    (o, i) -> o.config = i, (o) -> o.config,
                    (o, p) -> o.config = p.config)
            .addValidator(Validators.nonNull())
            .add()
            .build();
}
```

And then register it to the Interaction registry.
```java
public class YapperModule extends JavaPlugin {
    @Override
    protected void setup() {
        // Here, register the interaction codec. It will now appear in the Asset editor
        this.getCodecRegistry(Interaction.CODEC).register("Utterance", UtteranceInteraction.class, UtteranceInteraction.CODEC);
    }
}
```
