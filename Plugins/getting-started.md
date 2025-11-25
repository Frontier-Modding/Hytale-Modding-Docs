# Getting started with Plugins

1. Download the IntelliJ template (by Darkhax & Jared):\
[ExamplePlugin-Template-0.0.1.zip](https://github.com/Frontier-Modding/Frontier-Modding-Docs/raw/refs/heads/main/Plugins/_Files/ExamplePlugin-Template-0.0.1.zip)

- It will compile against the actual JAR the launcher installed. This means windows only, and only default install locations for now.
- If Hytale updates, you need to update the game_build in `gradle.properties`. The example is for build-11.

2. Extract the zip.

3. Import into IntelliJ as a module.

### To build the JAR:
- Just run `gradle build`. It'll be located in `./build/libs/.`

### To launch the game with your plugin either:

- Put the JAR in the plugins folder: `./AppData/Roaming/Hytale/UserData/Saves/<world-name>/plugins`

**Or**

- Jared's kind of janky way to launch the game with the plugin from IDEA using run args:
  - See: [run-plugin-from-intellij.md](https://github.com/Frontier-Modding/Frontier-Modding-Docs/blob/main/Plugins/run-plugin-from-intellij.md)
