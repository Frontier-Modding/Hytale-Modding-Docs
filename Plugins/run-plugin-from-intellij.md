## Launch the server with your plugin from IntelliJ

You can use the following (janky) way to launch the server from IntelliJ.

- Create a class file with the following:

```
public class RunServer {
    public static void main(String[] args) throws IOException {
       com.hypixel.hytale.Main.main(
               new String[]{"--allow-op",
               "--assets=C:\\Users\\<user>\\AppData\\Roaming\\Hytale\\install\\release\\package\\game\\build-11\\Assets",
               "--packs=C:\\Users\\<user>\\AppData\\Roaming\\Hytale\\UserData\\Packs"}
       );
    }
}
```

- Make sure `<user>` is your username and the `build-11` is updated to the build you're on.

- Then just run the file.

- You can join the "Local server" within the client.

## Index Server Code
> TODO: Move to Gradle

- Fetch [a fernfower build](https://www.jetbrains.com/intellij-repository/releases/com/jetbrains/intellij/java/java-decompiler-engine/243.23654.189/java-decompiler-engine-243.23654.189.jar).
- Decompile the jar `java -cp ./jde.jar org.jetbrains.java.decompiler.main.decompiler.ConsoleDecompiler ./HytaleServer.jar ./Sources`.
- Link the generated sources jar in IntelliJ.
- Enjoy hacky indexed code search.
