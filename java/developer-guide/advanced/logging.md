# Logging

Java SDK has a built-in SLF4J log framework, which can set the log backend required by the application according to the SLF4J official manual and activate the log output of the DID SDK. Then, set the log level and output target through the corresponding configuration file of the specific log backend.

You can also set log output options in the code by encoding them. When using the Logback as the log back end according to the example, you only use code to set the level of log output, independent of the configuration file:

```java
import org.slf4j.LoggerFactory;
import ch.qos.logback.classic.Level;
import ch.qos.logback.classic.Logger;

// ...

Level level = Level.valueOf("INFO");
Logger root = (Logger)LoggerFactory.getLogger(Logger.ROOT_LOGGER_NAME);
root.setLevel(level);
```
