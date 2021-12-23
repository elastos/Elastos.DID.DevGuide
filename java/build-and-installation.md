# Build and installation

使用 DID Java SDK 可以有两种方式，一种是使用 Gradle 或者 Maven 的包管理器，直接从 package repository 中下载由开发者编译并发布的二进制包；一种是下载源代码，并从源代码来构建。对于普通开发者推荐使用包管理器；对于高阶的开发者，或者 DID SDK 的贡献者，可以选在从源代码构建。

There are two ways to use DID Java SDK. One is to use the package manager of Gradle or Maven to download the binary package compiled and published by the developer directly from the package repository. The other is to download the source code to build it. Package manager is recommended for ordinary developers; For advanced developers, or DID SDK contributors, building from source code can be a good choice.

## Pre-built package

### Maven

```xml
<dependency>
  <groupId>org.elastos.did</groupId>
  <artifactId>didsdk</artifactId>
  <version>2.1.3</version>
</dependency>
```

### Gradle

```groovy
dependencies {
    implementation 'org.elastos.did:didsdk:2.1.3'
}
```

## Build from source

在开始前，你需要先安装 Java 8 兼容的开发环境，以及 Git。

Before you start, you need to install Java 8 compatible development environment and Git.

### 下载代码

Download code

```bash
git clone https://github.com/elastos/Elastos.DID.Java.SDK.git
cd Elastos.DID.Java.SDK
```

### 在命令行构建

Build at the command line

```bash
./gradlew build
```

通过这个命令，可以完成代码的编译、测试和jar包的打包

With this command, you can complete the compilation and testing of the code, as well as packaging of jar package.

因为 DID Java SDK中包含大量的测试用例，所以运行测试用例的过程可能耗时比较长，如果希望跳过测试用例的运行，那么可以使用以下命令来构建。

Because DID Java SDK contains numerous test cases, the process of running test cases may take a long time. If you want to skip the running of test cases, you can use the following command to build it.

```bash
./gradlew build -x test
```
