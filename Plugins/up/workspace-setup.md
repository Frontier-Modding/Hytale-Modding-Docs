# Getting started with Plugins, alternative Version

## 1. Download the Example Plugin Template
The easiest way to start developing plugins is by downloading the template:
- [Zip File](https://discord.com/channels/900128427150028811/1438151798044233792/1458786235408584786 "Download on Discord")
- [GitHub Repo](https://github.com/Hytale-Modding/example-plugin-template) - Currently Private, ask for access or wait until 1/13
- Template Mod Generator (TBD)

Plugin Features:
- custom gradle plugin so your buildscript doesnt get spammed with all the configuration code
- automatically detects Hytale server files to your classpath, regardless of OS (windows/mac/linux)
- automatic IDE and gradle run config generation with lots of customization options
    - Supports different patchlines like releases and pre-releases
    - Supports different authentication modes
    - Supports running a gradle task on project sync
    - Defaults to allowing OP players to make development easier
    - Custom run directory so it doesnt conflict with your regular installation of the game
- asset pack included by default, can be loaded and modified with the in-game asset editor
- workspace includes Buuz135's BetterModList mod by default so you can preview your mod logo
- Includes very little example code in the template so you can get started more easily
- Includes a gradle task to decompile the game for you, so you dont have to set that up manually.

## 2. Setup the Template
If using the Template repository, click the `use this template` button to create a repository on your own account and clone it;
Otherwise download and extract the zip file.

### Prerequisites:
Get an IDE, [Intellij IDEA](https://www.jetbrains.com/idea/download/ "Download IntellIj IDEA") is recommended (it is free to use).
You will also need to have Java 25 installed ([link](https://adoptium.net/temurin/releases?version=25 "Download Java 25 from Adoptium"))

Before opening the template, you should also make sure Hytale is installed in the launcher, and that you have the latest version.

### Setup
1. Open the extracted project files in IDEA.
2. Go to the `settings.gradle.kts` file, change the project name.
3. Go to the `gradle.properties` file, update the project properties.
4. Reload the gradle project via the `Gradle` tab in the top left corner of your IDE window.

> ![!IMPORTANT]
> Since Hytale currently does not include source files, if you want full code search and IDE help, you will need to generate these yourself:
In the gradle tab, select and run the `decompileServer` task. This might take a few minutes to complete. You can also ignore any errors it produces. Once the game is decompiled, you can open any of the game's class files such as `HytaleServer`, then at the top there should be a `Choose Sources...` button. Click this and select the `HytaleServer-src.zip` file right in the directory it opens.

## Test your project
The template will automatically generate a launch config named `HytaleSerer` for you. Upon running this, it will launch a game server right from your workspace, and you can launch the game and join it using the address `localhost` or `127.0.0.1`.

## Build your project
Once you are done and want to publish your project, you go back to the `Gradle` tab in the top left (Elephant icon), expand your project name -> `Tasks` -> `build` and run the `build` task.
When that is complete, your final jar file will be in the `build/libs` folder inside the project folder.
