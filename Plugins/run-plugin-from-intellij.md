## Launch the server with your plugin from IntelliJ

You can use the following (janky) way to launch the server from IntelliJ.

- Create a class file with the following:

```
public class RunServer {
    static void main() throws IOException {
        com.hypixel.hytale.Main.main(
                new String[]{
                        "--allow-op",
                        "--disable-sentry",
                        "--assets=C:\\Users\\<user>\\AppData\\Roaming\\Hytale\\install\\release\\package\\game\\latest\\Assets",
                        "--packs=C:\\Users\\<user>\\AppData\\Roaming\\Hytale\\UserData\\Packs"
                }
        );
    }
}
```

- Make sure `<user>` is your username.

- Then just run the file.

- You can join the "Local server" within the client.

Optionally add a Gradle run task:
```gradle
tasks.register('run', JavaExec) {
    mainClass = 'net.conczin.RunServer'
    classpath = sourceSets.main.runtimeClasspath
    workingDir = file('run') // The server will generate a bunch of files, let's not muddy the project root.
}
```

## Index Server Code
> TODO: Move to Gradle

- Fetch [Vineflower](https://github.com/Vineflower/vineflower/releases).
- Decompile the jar `java -jar .\vineflower-1.11.2.jar --decompile-inner --remove-bridge --decompile-generics --ascii-strings --remove-synthetic --include-classpath --variable-renaming=jad --ignore-invalid-bytecode --bytecode-source-mapping --dump-code-lines --indent-string="    " --log-level=TRACE --only="com/hypixel" HytaleServer.jar HytaleServer-sources.jar`.
- Link the generated sources jar in IntelliJ.
- Enjoy hacky indexed code search. Will break debugging lines.
