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
