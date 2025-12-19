# Getting started with Plugins

## 1. Download the IDEA Template
The easiest way to start developing plugins is by downloading the template from Darkhax & Jared. 
- [Zip File](https://github.com/Frontier-Modding/Frontier-Modding-Docs/blob/main/Plugins/_Files/Hytale-Example-Project-plugin-34d3b23.zip)
- [GitHub Repo](https://github.com/Darkhax/Hytale-Example-Project) - Currently Private

The plugin has many great features such as:
- Add the latest Hytale server files to your classpath.
- Run the game from your IDE, even with breakpoints!
- Bundle assets with your plugin, that can be modified using the official in-game asset editor.
- Supports different patchlines like releases and pre-releases.
- Includes example code and assets.

## 2. Setup the Template
Extract the contents of the ZIP file to where you want to develop your plugin. The ZIP contains a README.md file with an in-depth guide to using the plugin. 

Make sure you have done all of the following before proceeding.

1. Download Hytale using the official launcher.
2. Have IntelliJ IDEA installed. Community edition is fine.
3. Download Java 25 and add it as an SDK in IDEA.
4. Set your project name in `settings.gradle`
5. Review the properties in `gradle.properties`
6. Update `src/main/resources/manifest.json` to reflect your project.

## 3. Open the Project in IDEA
This is pretty straight forward, open the template folder in IDEA to load and initialize the project.

## 4. Tips

- To build a sharable version of your plugin run `gradle build`. The plugin JAR file will be in `builds/libs`.
- A run configuration to launch the game will be added for you. It's called `HytaleServer`.
- Plugins can be installed by placing them in `%appdata%/Hytale/UserData/Plugins`, if the folder doesn't exist you can make it.

## 5. Source Code
While IDEA will allow you to view decompiled classes, some features like indexing and find usages are severely limited without attaching sources. In the future, Hytale will provide sources for us, but for now you must generate them yourself. The easiest way to do this is with [VineFlower](https://vineflower.org/). The tool is pretty simple to use, just run `java -jar vineflower.jar ./HytaleServer.jar ./HytaleServer-src.zip` in the same folder as your HytaleServer.jar file.

**NOTE:** Decompiled source code will not line up with the compiled code at runtime. So while your ability to navigate the code improves, some features like breakpoints become less reliable. This will likely be fixed by Hytale providing the sources in the future.
