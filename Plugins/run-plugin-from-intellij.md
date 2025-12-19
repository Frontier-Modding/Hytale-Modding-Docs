# Running the Server within IntelliJ IDEA

## Gradle Approach
If you are using the [template](https://github.com/Frontier-Modding/Frontier-Modding-Docs/blob/main/Plugins/getting-started.md), a `HytaleServer` run configuration should be generated for you automatically. If you do not want to use the template, you can replicate the templates approach by copying the following and modifying it to suit your needs.

Add the idea-ext plugin to your buildscript.

```groovy
plugins {
    id 'java'
    id 'org.jetbrains.gradle.plugin.idea-ext' version '1.3'
}
```

Locate the Hytale install directory.
```groovy
ext {
    hytaleHome = "${System.getProperty("user.home")}/AppData/Roaming/Hytale/install"
}
```

Generate the working directory.

```groovy
def serverRunDir = file("$projectDir/run")
if (!serverRunDir.exists()) {
    serverRunDir.mkdirs()
}
```

Add a new run configuration using the plugin.

```groovy
idea.project.settings.runConfigurations {
    'HytaleServer'(org.jetbrains.gradle.ext.Application) {
        mainClass = 'com.hypixel.hytale.Main'
        moduleName = project.idea.module.name + '.main'
        programParameters = "--allow-op --disable-sentry --assets=$hytaleHome/$patchline/package/game/latest/Assets.zip"
        workingDirectory = serverRunDir.absolutePath
    }
}
```

## Index Server Code
> TODO: Move to Gradle

- Fetch [Vineflower](https://github.com/Vineflower/vineflower/releases).
- Decompile the jar `java -jar .\vineflower-1.11.2.jar --decompile-inner --remove-bridge --decompile-generics --ascii-strings --remove-synthetic --include-classpath --variable-renaming=jad --ignore-invalid-bytecode --bytecode-source-mapping --dump-code-lines --indent-string="    " --log-level=TRACE --only="com/hypixel" HytaleServer.jar HytaleServer-sources.jar`.
- Link the generated sources jar in IntelliJ.
- Enjoy hacky indexed code search. Will break debugging lines.
