# Getting started with Block-Hitboxes

- Hitboxes for blocks are stored inside Json files in:
`./Assets/Server/Item/Block/Hitboxes/`

- You can create your own hitbox by making a json file inside

 `./Packs/YourPackName/Server/Item/Block/Hitboxes/MyHitbox.json`

- This is the hitbox Block_Half, which is used for slabs
```
{
  "Boxes": [
    {
      "Min": {
        "X": 0,
        "Y": 0,
        "Z": 0
      },
      "Max": {
        "X": 1,
        "Y": 0.5,
        "Z": 1
      }
    }
  ]
}
```
- Here is a hitbox with multiple boxes, this one being Stairs
```
{
  "Boxes": [
    {
      "Min": {
        "X": 0,
        "Y": 0,
        "Z": 0
      },
      "Max": {
        "X": 1,
        "Y": 0.5,
        "Z": 1
      }
    },
    {
      "Min": {
        "X": 0,
        "Y": 0.5,
        "Z": 0
      },
      "Max": {
        "X": 1,
        "Y": 1,
        "Z": 0.5
      }
    }
  ]
}
```
- To change the blocks hitbox just the following into your Block.json, replace Stairs with the desired hitbox :
```
"HitboxType": "Stairs",
```
