# Build and Installation

There are two ways to use the DID Java SDK. One is to use the package manager of Gradle or Maven to download the binary package compiled and published by the developer directly from the package repository - the other is to download the source code to build it. The package manager is recommended for ordinary developers.&#x20;

For advanced developers (or DID SDK contributors), building from the source code is an effective choice and solution.

## Pre-built Package

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

Before you start, you need to install the Java 8 compatible development environment and Git.

Download the code:

```bash
git clone https://github.com/elastos/Elastos.DID.Java.SDK.git
cd Elastos.DID.Java.SDK
```

Build at the command line:

```bash
./gradlew build
```

With this command, you can complete the compilation and testing of the code, as well as the packaging of the jar package.

Because the DID Java SDK contains numerous test cases, the process of running test cases may take a long time. If you want to skip this part, you can use the following command to build it:

```bash
./gradlew build -x test
```
