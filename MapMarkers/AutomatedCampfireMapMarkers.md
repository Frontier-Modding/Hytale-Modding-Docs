# Automatic ProcessingBenchMapMarkers

Processing benches like Campfires automatically create a Player Worldspeciffic PlayerMarker/WorldMapMarker that can be seen on the Worldmap/Compass.

Not yet sure if there is another way without a Java plugin to generate these.

For custom blocks to have this property they need to:
- be a ProcessingBench
```json
    "Bench": {
      "Type": "Processing",
```
- ``Icon`` needs to be != null
- ``IconItem`` needs to be the same as the block key (id?)
- the ``Icon`` .png needs to exist, otherwise the client just ignores it apparently

<img width="638" height="556" alt="image" src="https://github.com/user-attachments/assets/f98be435-623b-4e92-8722-498137ff6bfb" />
<img width="138" height="124" alt="image" src="https://github.com/user-attachments/assets/4b3ed8ea-9165-498d-a4fa-adf2892913ed" />

Fun fact: the position is the player pos on place, not the actual block position itself?
