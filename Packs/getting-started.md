# Getting started with Packs

1. Create a folder here: `./AppData/Roaming/Hytale/UserData/Packs/YourPackName`

2. Make a manifest.json file, example is in this folder

3. Now you need to create a Common and a Server folder, common handles models, textures..
   Server handles Item and Block creation, translation, particle creation etc...
   
   `./AppData/Roaming/Hytale/UserData/Packs/YourPackName/Server`
   `./AppData/Roaming/Hytale/UserData/Packs/YourPackName/Common`

# Add a Block

1. Create the following json
 `./AppData/Roaming/Hytale/UserData/Packs/YourPackName/Server/Item/Items/your_block.json`
 
 This file works like a blockstate, see the example in this folder
 
2. Create the following files
 `./AppData/Roaming/Hytale/UserData/Packs/YourPackName/Common/BlockTextures/your_block_texture.png`
 `./AppData/Roaming/Hytale/UserData/Packs/YourPackName/Common/ItemsGenerated/your_block_icon.png`

 - your_block_texture.png is your classic texture, block icons as seen in GUI are a separate png.
 - To give you a better idea see the example inside this folder.

3. Translation
 `./AppData/Roaming/Hytale/UserData/Packs/YourPackName/Server/Languages/es-US/items.lang`
