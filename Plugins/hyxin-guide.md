# Hyxin Guide

Hyxin is a [Mixin](https://github.com/SpongePowered/Mixin) runtime designed specifically for the game 
[Hytale](https://hytale.com/). It enables developers to modify, extend, and inject behavior into Hytale's codebase 
without modifying the game directly.

## ⚠️ Early Access ⚠️
The game Hytale is in early access. Things are unstable, and prone to frequent changes. Things may not work, or may work
in unexpected or suboptimal ways.

**Caveats**
- Mixin configs are only loaded from early plugins.
- Mixin configs only checks the ./earlyplugins folder. Additional paths can be added using run arguments, but we do not scan them.
- When developing Hyxin itself, files are loaded from the AppClassLoader instead of the expected early plugins URLClassLoader.
- Accessors, invokers, interface injection, and similar features are not production ready.
- The `DisabledByDefault` option, along with the disabled plugins feature is not supported.

## Features
Both [FabricMC's Mixin fork](https://github.com/FabricMC/Mixin) and [LlamaLad7's MixinExtras](https://github.com/LlamaLad7/MixinExtras)
are available by default. You should be able to use any features added by either project, however not everything is
production ready yet.

## Loading & Installing
The Hyxin plugin can go in any valid `earlyplugins` folder. Plugins that rely on Hyxin must be in the servers 
`earlyplugins` folder. On a server, you will make this folder right next to your `plugins` and `logs` folder. In 
singleplayer your plugin needs to go in the worlds `earlyplugins` folder.

## Getting Started
The Hyxin project is not on Maven yet. You can download it from [here](https://github.com/Frontier-Modding/Frontier-Modding-Docs/blob/main/Plugins/_Files/Hyxin-0.0.11-early_access.zip).

Add a Hyxin config to your standard plugin `manifest.json` file. The `Configs` array defines a list of Mixin 
configuration files that Hyxin should load from your plugin jar.

```json
{
  "Group": "...",
  "Name": "...",
  "Version": "...",
  "Description": "...",
  "Hyxin": {
    "Configs": [
      "your_plugin.mixins.json"
    ]
  }
}
```

Inside the `your_plugin.mixins.json` you should set the root package name that Mixin classes are loaded from, and then
fill the mixins array with the name of each class you want to load. 

```json
{
  "required": true,
  "minVersion": "0.8",
  "package": "com.example.mixins",
  "mixins": [
    "ExampleMixin"
  ]
}
```

Then define your mixin class at `src/main/com/example/mixins/ExampleMixin.java`. In the following example, we are 
injecting our onMain method immediately before `EarlyPluginLoader#hasTransformers` is invoked in the constructor of 
`HytaleServer`.

```java
@Mixin(HytaleServer.class)
public class ExampleMixin {

    @Inject(method = "<init>", at = @At(value = "INVOKE", target = "Lcom/hypixel/hytale/plugin/early/EarlyPluginLoader;hasTransformers()Z"))
    private static void onMain(CallbackInfo ci) {
        HytaleLogger.get("Hyxin-Example").at(Level.INFO).log("Hello from Hyxin! The server has been patched!");
    }
}
```

## Credits & Acknowledgements

The Hyxin project is developed and maintained by [Darkhax](https://www.curseforge.com/members/darkhaxdev/projects) and [Jaredlll08](https://www.curseforge.com/members/jaredlll08/projects).
The Hyxin project is built upon [Mixin](https://github.com/SpongePowered/Mixin). Special thanks to [Mumfrey](https://github.com/Mumfrey) in particular, who has poured countless hours of time into maintaining the project.

- [Mixin - MIT License](https://github.com/SpongePowered/Mixin/blob/master/LICENSE.txt)
- [FabricMC Mixin - MIT License](https://github.com/FabricMC/Mixin/blob/main/LICENSE.txt)
- [LlamaLad7's MixinExtras - MIT License](https://github.com/LlamaLad7/MixinExtras/blob/master/LICENSE)
- [Google's Guava - Apache 2.0 License](https://github.com/google/guava/blob/master/LICENSE)
- [OW2 ASM - 3-Clause BSD License](https://asm.ow2.io/license.html)

We are very appreciative of all the prior works that have made this project possible. All trademarks, copyrights, and 
ownership remain with their original authors. The inclusion of these libraries does not imply endorsement of this
project by their creators or affiliated organizations.
