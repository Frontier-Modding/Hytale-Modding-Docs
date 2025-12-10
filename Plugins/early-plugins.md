# Early Plugins
Early plugins are a special category of plugin that load before the Hytale server. They do not have access to the normal plugin APIs, event busses, registries, or lifecycle callbacks. Their primary purpose is to perform class transformations, modifying and injecting bytecode to classes as they are loaded. Because these plugins run in a sensitive part of the loading pipeline, they can significantly influence the behavior and stability of the game. You should not create an early plugin without a good reason.

## Loading Early Plugins
By default, the game will load early plugins from the `earlyplugins` folder. You must create this folder manually, the server does not do it for you. Additional paths can be defined using the `--early-plugins` launch argument.

The user will receive a warning when launching the game with early plugins installed. This warning can be skipped by pressing the enter key, or by adding the `--accept-early-plugins` launch argument. This warning is **NOT** displayed in singleplayer.

```
===============================================================================================
                              Loaded 2 class transformer(s)!!
===============================================================================================
                       This is unsupported and may cause stability issues.
                                     Use at your own risk!!
===============================================================================================
Press ENTER to accept and continue...
```

## Creating Early Plugins
Any `.jar` file can be loaded as an early plugin. These plugins do not have a `manifest.json` file, or any other required entrypoints.

## Class Transformers
Class transformers allow you to modify the bytecode of classes as they are loaded.

### Registration
Class transformers are loaded using the `com.hypixel.hytale.plugin.early.ClassTransformer` service loader.

1. Create a Java class that implements `com.hypixel.hytale.plugin.early.ClassTransformer`.
2. Create a file in `src/main/resources/META-INF/services/com.hypixel.hytale.plugin.early.ClassTransformer`
3. Add the full name of your class to the file. For example `com.example.early.ExampleTransformer`

### Writing Class Transformers

#### Priority
ClassTransformers are ran in the order of their priority. Higher priority transformers run before lower priority ones. By default all transformers have a priority of `0` but this can be overriden by your transformer class.

```java
    @Override
    public int priority() {
        return -100;
    }
```

#### Restricted Classes
For safety reasons, transformers will never be allowed to modify classes from certain libraries. The following packages are all on the restricted package list.

- java
- javax
- jdk
- sun
- com.sun
- org.bouncycastle
- io.netty
- org.objectweb.asm
- com.google.gson
- org.slf4j
- org.apache.logging
- ch.qos.logback
- com.google.flogger
- io.sentry
- com.hypixel.protoplus
- com.hypixel.fastutil
- com.hypixel.hytale.plugin.early

#### Transforming
After early plugins have been loaded and the server is launched, your transformer will be given the chance to modify the bytecode of classes as they are loaded. You can even modify the HytaleServer class! When a class is loaded its bytes will be passed through a chain of the available transformers and the resulting bytes are what will be loaded by the JVM. 

> ⚠️ **Here be dragons 🐉**    
> Moddifying class bytes is not intuitive and there are many pitfalls that even experienced developers
> will run into. This is why using libraries like ASM or abstraction layers like Mixin is recommended.

#### Example Transformer

The following example contains a ClassTransformer that will replace the startup message with a custom one. This is done by finding the bytes of the original String constant and replacing them with new ones. This is just an example, and is not intended to be used in production environments.
```java
import com.hypixel.hytale.plugin.early.ClassTransformer;
import java.nio.ByteBuffer;
import java.nio.charset.StandardCharsets;

public class ExampleTransformer implements ClassTransformer {

    @Override
    public byte[] transform(String name, String path, byte[] bytes) {
        if (name.equals("com.hypixel.hytale.server.core.HytaleServer")) {
            return patchString(bytes, "Starting HytaleServer", "Starting Patched HytaleServer >:)");
        }
        return bytes;
    }

    /**
     * Replaces the first occurrence of a string within an array of Java class bytes.
     *
     * @param bytes       The Java class bytes.
     * @param target      The target string to be replaced.
     * @param replacement The string to replace it with.
     * @return The patched class bytes.
     */
    public static byte[] patchString(byte[] bytes, String target, String replacement) {
        final byte[] targetBytes = encodeStringWithLength(target);
        final byte[] replacementBytes = encodeStringWithLength(replacement);
        final int index = indexOf(bytes, targetBytes);
        if (index == -1) {
            return bytes;
        }
        final byte[] result = new byte[bytes.length - targetBytes.length + replacementBytes.length];
        System.arraycopy(bytes, 0, result, 0, index); // Before the target
        System.arraycopy(replacementBytes, 0, result, index, replacementBytes.length); // Replacement
        System.arraycopy(bytes, index + targetBytes.length, result, index + replacementBytes.length, bytes.length - index - targetBytes.length); // After the target
        return result;
    }

    /**
     * Encodes a string as length (short) + UTF_8, this is the format used in Java bytecode for string constants.
     *
     * @param str The string to encode.
     * @return The encoded string.
     */
    public static byte[] encodeStringWithLength(String str) {
        final byte[] utf8Bytes = str.getBytes(StandardCharsets.UTF_8);
        ByteBuffer buffer = ByteBuffer.allocate(2 + utf8Bytes.length);
        return buffer.putShort((short) utf8Bytes.length).put(utf8Bytes).array();
    }

    /**
     * Finds the first index of a byte array within a larger sequence of bytes.
     *
     * @param array  The sequence of bytes to search through.
     * @param target The target to find.
     * @return The index of the target, or -1 if the target was not found.
     */
    private static int indexOf(byte[] array, byte[] target) {
        outer:
        for (int i = 0; i <= array.length - target.length; i++) {
            for (int j = 0; j < target.length; j++) {
                if (array[i + j] != target[j]) {
                    continue outer;
                }
            }
            return i;
        }
        return -1;
    }
}
```
