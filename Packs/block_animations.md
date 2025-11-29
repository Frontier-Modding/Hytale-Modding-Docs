# Getting started with Block Animations

First you need to edit your blockstate "State", add the desired animation.

```
"State": {
      "Definitions": {
        "On": {
          "CustomModelAnimation": "Blocks/Animations/Empty_Water.blockyanim"
        },
        "Off": {
          "CustomModelAnimation": "Blocks/Animations/Rising_Water.blockyanim"
        }
      }
    }
```

1. First you need to create a group in your model via blockbench on in json and give it a name, I named it "Water"
   That will be the element which you will animate, you can put in that folder multiple elements..
   
2. This is Rising_Water.blockyanim
   You can see that in "nodeAnimations" there is "Water", that is the name of the group in the model which the animation will apply to
   The only thing changing here is the position of the Y of the model.
   This will make the elements inside the "Water" folder in your model change position, so they will go up by 8.5

 Position : Will change the position of elements
 
 Orientation : Will change the rotation of elements
 
 ShapeStretch : Will stretch the elements
 
 ShapeVisible : If the shape is hidden or not
 
```
{
  "formatVersion": 1,
  "duration": 50,
  "holdLastKeyframe": true,
  "nodeAnimations": {
    "Water": {
      "position": [ {
          "time": 0,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0
          },
          "interpolationType": "smooth"
        }, {
          "time": 50,
          "delta": {
            "x": 0,
            "y": 8.5,
            "z": 0
          },
          "interpolationType": "smooth"
        } ],
      "orientation": [],
      "shapeStretch": [],
      "shapeVisible": [],
      "shapeUvOffset": []
    }
  }
}
```

To give you another idea we can take a look at how Wardrobes are handled.
- Wardrobes are using Wardrobe_Close.blockyanim and Wardrobe_Open.blockyanim



This is Wardrobe_Open.blockyanim
- There are multiple groups in nodeAnimations that will be animated, Door-L, Door-R, Door-Knocker, Door-Knocker-L, Door-Knocker-R, Drawer
- You can open any wardrobe model and look how the groups are made to give you a better idea
- The easiest animation here is the drawer where only Z potion of the element is changed from 0 to 12 to 10
- Because it starts inside the wardrobe, then it gets pulled out, and then it moves back a little bit. 


```
{
  "formatVersion": 1,
  "duration": 50,
  "holdLastKeyframe": true,
  "nodeAnimations": {
    "Door-L": {
      "position": [],
      "orientation": [ {
          "time": 0,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        }, {
          "time": 15,
          "delta": {
            "x": 0,
            "y": 0.866025,
            "z": 0,
            "w": 0.5
          },
          "interpolationType": "smooth"
        }, {
          "time": 25,
          "delta": {
            "x": 0,
            "y": 0.793353,
            "z": 0,
            "w": 0.608761
          },
          "interpolationType": "smooth"
        } ],
      "shapeStretch": [],
      "shapeVisible": [],
      "shapeUvOffset": []
    },
    "Door-R": {
      "position": [],
      "orientation": [ {
          "time": 0,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        }, {
          "time": 20,
          "delta": {
            "x": 0,
            "y": -0.866025,
            "z": 0,
            "w": 0.5
          },
          "interpolationType": "smooth"
        }, {
          "time": 30,
          "delta": {
            "x": 0,
            "y": -0.793353,
            "z": 0,
            "w": 0.608761
          },
          "interpolationType": "smooth"
        } ],
      "shapeStretch": [],
      "shapeVisible": [],
      "shapeUvOffset": []
    },
    "Door-Knocker-R": {
      "position": [],
      "orientation": [ {
          "time": 0,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        }, {
          "time": 20,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        }, {
          "time": 27,
          "delta": {
            "x": -0.382683,
            "y": 0,
            "z": 0,
            "w": 0.92388
          },
          "interpolationType": "smooth"
        }, {
          "time": 34,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        }, {
          "time": 42,
          "delta": {
            "x": -0.130526,
            "y": 0,
            "z": 0,
            "w": 0.991445
          },
          "interpolationType": "smooth"
        }, {
          "time": 50,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        } ],
      "shapeStretch": [],
      "shapeVisible": [],
      "shapeUvOffset": []
    },
    "Door-Knocker-L": {
      "position": [],
      "orientation": [ {
          "time": 0,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        }, {
          "time": 15,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        }, {
          "time": 22,
          "delta": {
            "x": -0.382683,
            "y": 0,
            "z": 0,
            "w": 0.92388
          },
          "interpolationType": "smooth"
        }, {
          "time": 29,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        }, {
          "time": 37,
          "delta": {
            "x": -0.130526,
            "y": 0,
            "z": 0,
            "w": 0.991445
          },
          "interpolationType": "smooth"
        }, {
          "time": 45,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        } ],
      "shapeStretch": [],
      "shapeVisible": [],
      "shapeUvOffset": []
    },
    "Drawer": {
      "position": [ {
          "time": 0,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0
          },
          "interpolationType": "smooth"
        }, {
          "time": 10,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 12
          },
          "interpolationType": "smooth"
        }, {
          "time": 20,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 10
          },
          "interpolationType": "smooth"
        } ],
      "orientation": [],
      "shapeStretch": [],
      "shapeVisible": [],
      "shapeUvOffset": []
    },
    "Door-Knocker": {
      "position": [],
      "orientation": [ {
          "time": 0,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        }, {
          "time": 10,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        }, {
          "time": 17,
          "delta": {
            "x": -0.5,
            "y": 0,
            "z": 0,
            "w": 0.866025
          },
          "interpolationType": "smooth"
        }, {
          "time": 24,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        }, {
          "time": 32,
          "delta": {
            "x": -0.130526,
            "y": 0,
            "z": 0,
            "w": 0.991445
          },
          "interpolationType": "smooth"
        }, {
          "time": 40,
          "delta": {
            "x": 0,
            "y": 0,
            "z": 0,
            "w": 1
          },
          "interpolationType": "smooth"
        } ],
      "shapeStretch": [],
      "shapeVisible": [],
      "shapeUvOffset": []
    }
  }
}
```
