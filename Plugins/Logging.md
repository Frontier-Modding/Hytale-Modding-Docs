# Logging and You
Logging allows you to print messages to the log file and to the game console. Logging is useful for tracking errors, displaying progress, and communicating info.

## Getting a Logger

When logging messages in Hytale, you should always use a HytaleLogger that is specific to your project. You can create a new logger that is based on the current class name, or use a custom name unique to your project.
When logging messages in Hytale you should always use a HytaleLogger.

```java
    public static final HytaleLogger LOGGER = HytaleLogger.forEnclosingClass();
    public static final HytaleLogger LOGGER = HytaleLogger.get("Example");
```

To avoid confusion and errors, you should NOT use the following approaches.

```java
    // Uses SOUT instead of the expected logging framework.
    System.out.println("Hello World!");
    IO.println("Hello World!");

    // Uses loggers not related to your project, making it hard to trace info back to your plugin.
    HytaleLogger.getLogger().atInfo().log("Hello World!");
    LogUtil.getLogger().atInfo().log("Hello World!");
```

## Log Levels
All log messages have an associated level. The level can be used to categorize messages and configure how they are logged. Using the default configurations, only messages from `Info`, `Warn`, and `Severe` are printed.

```java
    LOGGER.atInfo().log("Provide high-level information about normal behavior.");
    LOGGER.atWarning().log("Signal a potential problem or a situation that could lead to an error.");
    LOGGER.atSevere().log("A serious error that will prevent things from working as expected.");
```

```
[2025/12/29 01:16:36   INFO]              [Example] Provide high-level information about normal behavior.
[2025/12/29 01:16:36   WARN]              [Example] Signal a potential problem or a situation that could lead to an error.
[2025/12/29 01:16:36 SEVERE]              [Example] A serious error that will prevent things from working as expected.
```

## Template Arguments
The HytaleLogger uses `printf` style arguments, and should support most of the features and specifiers used by normal string formatting.

```java
final String name = "John";
LOGGER.atInfo().log("Hello %s", name);
// prints: Hello John
```

Here is a quick overview of some of the more commonly used specifiers.

- `%s` - String
- `%d` - Integer/Long
- `%f` - Float/Double
- `%b` - Boolean
- `%c` - Char

## Exceptions and Causes
If your log message has an associated exception or you want to include a stack trace, this can be done by setting the cause. 

```java
    catch (IOException e) {
        LOGGER.atSevere().withCause(e).log("Could not read file! path='%s'", filePath);
    }
```

```
[2025/12/29 00:54:35 SEVERE]              [Example] Could not read file! path='./config.json'
java.io.FileNotFoundException: The file config.json could not be found!
	at com.example.impl.Example.<init>(Example.java:40)
	at java.base/jdk.internal.reflect.DirectConstructorHandleAccessor.newInstance(DirectConstructorHandleAccessor.java:62)
	at java.base/java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:499)
	at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:483)
	at com.hypixel.hytale.server.core.plugin.pending.PendingLoadJavaPlugin.load(PendingLoadJavaPlugin.java:50)
	at com.hypixel.hytale.server.core.plugin.pending.PendingLoadJavaPlugin.load(PendingLoadJavaPlugin.java:15)
	at com.hypixel.hytale.server.core.plugin.PluginManager.setup(PluginManager.java:218)
	at com.hypixel.hytale.server.core.HytaleServer.boot(HytaleServer.java:317)
```
