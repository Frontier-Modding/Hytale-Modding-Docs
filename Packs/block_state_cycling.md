# Block state changing/cycling

- This is how to handle block state changes

1. Select your block in:
 `.Packs/YourPackName/Server/Item/Items/your_block.json`
 
2. Here we have an example, all is handled in the BlockType
- The base model is in "CustomModel" with a texture "CustomModelTexture"
- You need to create an interaction which will change states
- Then you need to add the states in "States"
- You can also add other stuff to the states like animations, sounds, particles...


```
"BlockType": {
    "DrawType": "Model",
    "CustomModel": "Blocks/model_one.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/your_texture.png"
      }
    ],
	
    "Interactions": {
      "Use": {
        "Interactions": [
          {
            "Type": "ChangeState",
            "Changes": {
              "default": "Off",
              "On": "Off",
              "Off": "On"
            }
          }
        ]
      }
    },
	
    "State": {
      "Definitions": {
        "On": {
		  "InteractionHint": "interactionHints.turnoff",
		  "CustomModel": "Blocks/model_one.blockymodel",
			"CustomModelTexture": [
            {
              "Texture": "Blocks/Texture/textureone.png"
            }
          ]
        },
        "Off": {
          "InteractionHint": "interactionHints.turnon",
		  "CustomModel": "Blocks/model_two.blockymodel",
			"CustomModelTexture": [
            {
              "Texture": "Blocks/Texture/textureone.png"
            }
          ]
        }
      }
    }
  },
```
