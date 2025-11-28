# Custom Guis

The .ui format is explained in the UI Docs, this guide will cover what to do to create a new GUI.

UI files go inside your pack in this route: `Common\UI\Custom`. All the files in this path will be sent to the client. Example of full path: `UserData\Packs\TestGuiPack\Common\UI\Custom\Pages\TestGui.ui`

## Plugin Configuration

GUIs implement `InteractiveCustomUIPage` with a class type that will be used to communicate between the client and the server.

```JAVA
public class AdvancedItemInfo extends InteractiveCustomUIPage<AdvancedItemInfo.SearchGuiData> {

    private String searchQuery = "";
    private final Map<String, Item> visibleItems = new HashMap<>();

    public AdvancedItemInfo(@Nonnull PlayerRef playerRef, @Nonnull CustomPageLifetime lifetime) {
        super(playerRef, lifetime, SearchGuiData.CODEC);
    }

    @Override
    public void build(@Nonnull Ref<EntityStore> ref, @Nonnull UICommandBuilder uiCommandBuilder, @Nonnull UIEventBuilder uiEventBuilder, @Nonnull Store<EntityStore> store) {
        /**
         * The append method loads the .ui file from the path specified and loads all the elements inside it.
         */
        uiCommandBuilder.append("Pages/TestGUI.ui");
        /**
         * This page has a search bar, and you use `addEventBinding` to configure what to do when interacted with an element.
         * 
         * In this case, when the user types in the search bar `#SearchInput`, the value of the search bar is sent to the server.
         * 
         * What its sent it's defined in the EventData object, the first parameter is the key that will be used to send the value to the server (in the codec), the second parameter is the selector of the ui element.
         */
        uiEventBuilder.addEventBinding(CustomUIEventBindingType.ValueChanged, "#SearchInput", EventData.of("@SearchQuery", "#SearchInput.Value"), false);
        
        this.buildList(ref, uiCommandBuilder, uiEventBuilder, store);
    }

    /**
     * You can add ui elements dynamically
     */
    private void buildButtons(Map<String, Item> items, @Nonnull Player playerComponent, @Nonnull UICommandBuilder commandBuilder, @Nonnull UIEventBuilder eventBuilder) {
        /**
         * Clears the elements inside the group `#SubcommandCards` to remove old ones.
         */
        commandBuilder.clear("#SubcommandCards");
        int rowIndex = 0;
        int cardsInCurrentRow = 0;

        for (Map.Entry<String, Item> entry : items.entrySet()) {
            Item subcommand = entry.getValue();

            if (cardsInCurrentRow == 0) {
                commandBuilder.appendInline("#SubcommandCards", "Group { LayoutMode: Left; Anchor: (Bottom: 0); }");
            }
            
            /**
             * The append method loads the .ui file from the path specified and loads all the elements inside it and adds it to the SubcommandCards element.
             */
            commandBuilder.append("#SubcommandCards[" + rowIndex + "]", "Pages/SearchItemIcon.ui");
            var tooltip = "ID: " + entry.getKey();

            /**
             * Then you can access the elements and set their values like if they were an array
             * 
             * The set method allows you to modify properties of the element (the properties are explained in the UI Docs)
             * 
             * This selector is used to modify the text of the tooltip when hovering the main element `#SubcommandCards[x][y].TooltipText`
             */
            commandBuilder.set("#SubcommandCards[" + rowIndex + "][" + cardsInCurrentRow + "].TooltipText", tooltip);
            /**
             * This selector modifies a child element of #SubcommandCards[x][y]. #ItemIcon is a child element of #SubcommandCards[x][y] and its modifying the value of ItemId
             */
            commandBuilder.set("#SubcommandCards[" + rowIndex + "][" + cardsInCurrentRow + "] #ItemIcon.ItemId", entry.getKey());
            commandBuilder.set("#SubcommandCards[" + rowIndex + "][" + cardsInCurrentRow + "] #ItemName.TextSpans", Message.translation(subcommand.getTranslationKey()));
            //commandBuilder.set("#SubcommandCards[" + rowIndex + "][" + cardsInCurrentRow + "] #SubcommandUsage.TextSpans", this.getSimplifiedUsage(subcommand, playerComponent));
            /**
             * The Activating event is used to send the item id to the server when the user interacts with the element.
             */
            eventBuilder.addEventBinding(CustomUIEventBindingType.Activating, "#SubcommandCards[" + rowIndex + "][" + cardsInCurrentRow + "]", EventData.of("Item", entry.getKey()));
            ++cardsInCurrentRow;
            if (cardsInCurrentRow >= 7) {
                cardsInCurrentRow = 0;
                ++rowIndex;
            }
        }
    }
}
```

### CODEC

The codec is used to communicate between the client and the server. The keys of the fields are used in the event binding to define what to do with the value sent by the client.

```JAVA
    public static class SearchGuiData {
        static final String KEY_ITEM = "Item";
        static final String KEY_SEARCH_QUERY = "@SearchQuery";
        public static final BuilderCodec<SearchGuiData> CODEC = BuilderCodec.<SearchGuiData>builder(SearchGuiData.class, SearchGuiData::new)
                .addField(new KeyedCodec<>(KEY_SEARCH_QUERY, Codec.STRING), (searchGuiData, s) -> searchGuiData.searchQuery = s, searchGuiData -> searchGuiData.searchQuery)
                .addField(new KeyedCodec<>(KEY_ITEM, Codec.STRING), (searchGuiData, s) -> searchGuiData.item = s, searchGuiData -> searchGuiData.item).build();

        private String item;
        private String searchQuery;

    }
```

### Handling Event Bindings

```JAVA
    @Override
    public void handleDataEvent(@Nonnull Ref<EntityStore> ref, @Nonnull Store<EntityStore> store, @Nonnull SearchGuiData data) {
        super.handleDataEvent(ref, store, data);
        /**
        * Always check if the data is null, to know if the user interacted with an element.
        */
        if (data.item != null) {
            /**
             * When interacting with an element that has a binding, you need to send the data to the client using `sendUpdate`. This method answers with nothing.
             */
            this.sendUpdate();
        }
        if (data.searchQuery != null) {
            /**
             * We update the search query and rebuild the list.
             */
            this.searchQuery = data.searchQuery.trim().toLowerCase();
            /**
             * We resend the whole GUI to the client with the new elements
             */
            UICommandBuilder commandBuilder = new UICommandBuilder();
            UIEventBuilder eventBuilder = new UIEventBuilder();
            this.buildList(ref, commandBuilder, eventBuilder, store);
            this.sendUpdate(commandBuilder, eventBuilder, false);
        }
    }

```

### How to open the GUI

Example by using a command:

```JAVA
    @Nullable
    @Override
    protected CompletableFuture<Void> execute(@Nonnull CommandContext context) {
        CommandSender sender = context.sender();
        if (sender instanceof Player player) {
            Ref<EntityStore> ref = player.getReference();
            if (ref != null && ref.isValid()) {
                Store<EntityStore> store = ref.getStore();
                World world = store.getExternalData().getWorld();
                return CompletableFuture.runAsync(() -> {
                    PlayerRef playerRefComponent = store.getComponent(ref, PlayerRef.getComponentType());
                    if (playerRefComponent != null) {
                        player.getPageManager().openCustomPage(ref, store, new AdvancedItemInfo(playerRefComponent, CustomPageLifetime.CanDismiss));
                    }
                }, world);
            } else {
                context.sendMessage(MESSAGE_COMMANDS_ERRORS_PLAYER_NOT_IN_WORLD);
                return CompletableFuture.completedFuture(null);
            }
        } else {
            return CompletableFuture.completedFuture(null);
        }
    }
```

