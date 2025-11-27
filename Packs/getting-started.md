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

3. Translation
 `./AppData/Roaming/Hytale/UserData/Packs/YourPackName/Server/Languages/es-US/items.lang`

4. You can just move the ExampleMod folder into `./AppData/Roaming/Hytale/UserData/Packs/` , in your main menu when you click worlds and then right click any world you will see PACKS you can enable/disable them there.

5. To edit the Icon of the block shown in the GUI follow these steps:
   - Open the asset editor and find your block
   
Images By Darkhax
<img width="1270" height="730" alt="image1" src="https://github.com/user-attachments/assets/23bf00f4-5c4b-42e1-9019-071cb9a28ddb" />
<img width="791" height="446" alt="image2" src="https://github.com/user-attachments/assets/2707b27f-76a3-4164-90a3-9561c5288ce5" />
