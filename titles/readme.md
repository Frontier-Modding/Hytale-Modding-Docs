## Example command to show a Minecraft title equivalent

```java
public class ModdedCommand extends CommandBase {

    public ModdedCommand() {
        super("moddedcommand", "A test command", false);
    }

    @Override
    protected void executeSync(@Nonnull CommandContext commandContext) {
        commandContext.senderAsPlayer().getWorld().execute(() -> {
            EventTitleUtil.showEventTitleToPlayer(commandContext.senderAsPlayer().getReference(), Message.raw("It's modded!"), Message.raw("Yeppers"), true, commandContext.senderAsPlayer().getWorld().getEntityStore().getStore());
        });
    }
}
```

<img width="468" height="188" alt="image" src="https://github.com/user-attachments/assets/0f6a120c-c890-47b3-8bdb-af00499920b3" />
