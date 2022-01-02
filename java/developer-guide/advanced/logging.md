# Logging

Java SDK 内置使用了 [SLF4J](http://www.slf4j.org/) 日志框架，可以按照 [SLF4J 官方的手册](http://www.slf4j.org/manual.html)来设定应用需要的日志后端，激活 DID SDK 的日志输出。然后通过特定的日志后端相应的配置文件来设定日志的级别和输出目标。

也可以通过编码的方式，在代码中设定日志输出选项。示例使用 [Logback](http://logback.qos.ch/) 作为日志后端时，不依赖于配置文件，仅使用代码设定日志输出的级别：

```java
import org.slf4j.LoggerFactory;
import ch.qos.logback.classic.Level;
import ch.qos.logback.classic.Logger;

// ...

Level level = Level.valueOf("INFO");
Logger root = (Logger)LoggerFactory.getLogger(Logger.ROOT_LOGGER_NAME);
root.setLevel(level);
```

