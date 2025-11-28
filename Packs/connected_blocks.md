# Getting started with Connected Blocks

1. We are going to create a double-wide block when two of the same block are placed next to each other.
   Create two blockstates, for this guide its gonna be Furniture_Small, Furniture_Large

- Furniture_Small is the single block without any connected blocks
- Furniture_Large happens when you place two of Furniture_Small next to each other.

2. this is inside Furniture_Small.json

    "ConnectedBlockShapeMaterial": {
      "TemplateShapeAssetId": "ChestConnectedBlockTemplate",
      "TemplateShapeBlockPatterns": {
        "Default": "Furniture_Small",
        "Double": "Furniture_Large"
      }
    }

3. Connected blocks are using templates from 
`\package\game\latest\Assets\Server\Item\ConnectedBlockTemplates`

I haven't tried adding a custom one yet, so I will show you the ChestConnectedBlockTemplate
There are set rules for templates that need to match so the block changes, this is used for chests when you place two of the same, next of each other
they will connect to form the large chest.

```
{
    "MaterialName":"Chest",
    "ConnectsToOtherMaterials":true,
    "DefaultShape": "Default",
    "Shapes":{
        "Default":{
            "PatternsToMatchAnyOf": []
        },
        "Double":{
            "PatternsToMatchAnyOf": [
                {
                    "TransformRulesToPlacedOrientation": true,
                    "Type": "Custom",
                    "RulesToMatch":[
                        {
                            "Position":{ "X":-1, "Y":0, "Z":0 },
                            "IncludeOrExclude":"Include",
                            "Shapes":[
                                "Chest_Default",
                                "Chest_Double|Filler=-1,0,0"
                            ]
                        },
                        {
                            "Position":{ "X":0, "Y":0, "Z":0 },
                            "IncludeOrExclude":"Exclude",
                            "Shapes":[
                                "Chest_Double|Filler=-1,0,0"
                            ]
                        }
                    ]
                }
            ]
        }
    }
}

```
