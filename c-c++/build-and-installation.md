---
description: >-
  The compilation of sources works on macOS, Linux (Ubuntu, Debian, etc.), and
  Windows and provides the option to cross-compile for target systems of iOS,
  Android, and RaspberryPi.
---

# Build and installation

**CMake** is used to build, test, and package the Elastos DID project in an operating system and compiler independent manner.

Confident knowledge of CMake is required.

## Build on Ubuntu / Debian / Linux Host

### 1. Brief introduction

On Ubuntu / Debian / Linux, besides the compilation for the host itself, cross-compilation is possible for the following targets:

* Android with architectures of **armv7a**, **arm64**, and simulators of **x86/x86\_64** are supported.
* RaspberryPi with architecture **armv7l** only will be supported later.

### 2. Install Pre-Requirements

To generate Makefiles by using **configure** or **cmake** and manage dependencies of the DID project certain packages must be installed on the host before compilation.

Run the following commands to install the prerequisite utilities:

```
sudo apt-get update
sudo apt-get install -f build-essential autoconf automake autopoint libtool flex bison libncurses5-dev cmake
```

Download this repository using Git:

```
git clone https://github.com/elastos/Elastos.DID.Native.SDK
```

### 3. Build to run on host (Ubuntu / Debian / Linux)

To compile the project from source code for the target to run on Ubuntu / Debian / Linux, carry out the following steps:

Open a new terminal window.

Navigate to the previously downloaded folder that contains the source code of the DID project.

```
cd YOUR-PATH/Elastos.DID.Native.SDK
```

Enter the 'build' folder.

```
cd build
```

Create a new folder with the target platform name, then change the directory.

```
mkdir linux
cd linux
```

Generate the Makefile in the current directory:

_Note: Please see custom options below._

```
cmake ../..
```

{% hint style="info" %}
Optional (Generate the Makefile): To be able to build a distribution with a specific build type **Debug/Release**, as well as with customized install location of distributions, run the following commands:

**cmake -DCMAKE\_BUILD\_TYPE=Release -DCMAKE\_INSTALL\_PREFIX=YOUR-INSTALL-PATH ../..**

**Tips**: Must update cmake version larger than 3.13, if enable python (-DENABLE\_PYTHON=TURE) on Linux platform!
{% endhint %}

Build the program:

_Note: If "make" fails due to missing permissions, use "sudo make" instead._

```
make
```

Install the program:

_Note: If "make install" fails due to missing permissions, use "sudo make install" instead._

```
make install
```

Create distribution package:

_Note: If "make dist" fails due to missing permissions, use "sudo make dist" instead._

```
make dist
```

### 4.Use maven package

```
cd linux/outputs/lib
```

静态连接就打包该文件夹下的a文件，动态连接就打包文件夹下的dylib文件即可。

Package the file _a_ under the folder for static connection and package the file _dylib_ under the folder for dynamic connection.

## Build on macOS Host

### 1. Brief introduction

On macOS, besides the compilation for the host itself, cross-compilation is possible for the following targets:

* Android with architectures of **armv7a**, **arm64**, and simulators of **x86/x86\_64** are supported.
* iOS platforms to run on **iPhone-arm64** and **iPhoneSimulator-x86\_64**.

### 2. Install Pre-Requirements

packages must be installed on the host before compilation.

The following packages related to **configure** and **cmake** must be installed on the host before compilation either by installation through the package manager **homebrew** or by building from source:

Note: Homebrew can be downloaded from the [Homebrew web site](https://brew.sh/).

Install packages with Homebrew:

```
brew install autoconf automake libtool shtool pkg-config gettext cmake
```

Please note that **homebrew** has an issue with linking **gettext**. If you have an issue with the execution of **autopoint**, fix it by running:

```
brew link --force gettext
```

Download this repository using Git:

```
git clone https://github.com/elastos/Elastos.DID.Native.SDK
```

### 3. Build to run on host

To compile the project from source code for the target to run on MacOS, carry out the following steps:

Open a new terminal window.

Navigate to the previously downloaded folder that contains the source code of the DID project.

```
cd YOUR-PATH/Elastos.DID.Native.SDK
```

Enter the 'build' folder.

```
cd build
```

Create a new folder with the target platform name, then change directory.

```
mkdir macos
cd macos
```

Generate the Makefile in the current directory:

_Note: Please see custom options below_

```
cmake ../..
```

{% hint style="info" %}
Optional (Generate the Makefile): To be able to build a distribution with a specific build type **Debug/Release**, as well as with customized install location of distributions, run the following commands:\
&#x20;**cmake -DCMAKE\_BUILD\_TYPE=Release -DCMAKE\_INSTALL\_PREFIX=YOUR-INSTALL-PATH ../..**
{% endhint %}

Build the program:

_Note: If "make" fails due to missing permissions, use "sudo make" instead._

```
make
```

Install the program:

_Note: If "make install" fails due to missing permissions, use "sudo make install" instead._

```
make install
```

Create distribution package:

_Note: If "make dist" fails due to missing permissions, use "sudo make dist" instead._

```
make dist
```

### 4. Cross-compilation for iOS Platform

With CMake, Elastos DID can be cross-compiled to run on iOS as a target platform, while compilation is carried out on a MacOS host with XCode.

**Prerequisite**: MacOS version must be **9.0** or higher.

Open a new terminal window.

Navigate to the previously downloaded folder that contains the source code of the DID project.

```
cd YOUR-PATH/Elastos.DID.Native.SDK
```

Enter the 'build' folder.

```
cd build
```

Create a new folder with the target platform name, then change directory.

```
mkdir ios
cd ios
```

To generate the required Makefile in the current directory, please make sure to first replace 'YOUR-IOS-PLATFORM' with the correct option.

\-DIOS\_PLATFORM accepts the following target architecture options:

* iphoneos
* iphonesimulator

Replace 'YOUR-IOS-PLATFORM' with the path to the extracted NDK folder.

Run the command with the correct options described above:

```
cmake -DIOS_PLATFORM=YOUR-IOS-PLATFORM -DCMAKE_TOOLCHAIN_FILE=../../cmake/iOSToolchain.cmake ../..
```

Build the program:

_Note: If "make" fails due to missing permissions, use "sudo make" instead._

```
make
```

Install the program:

_Note: If "make install" fails due to missing permissions, use "sudo make install" instead._

```
make install
```

Create distribution package:

_Note: If "make dist" fails due to missing permissions, use "sudo make dist" instead._

```
make dist
```

### 5.Use maven package

```
cd macos/outputs/lib
```

静态连接就打包该文件夹下的a文件，动态连接就打包文件夹下的dylib文件即可。

Package the file _a_ under the folder for static connection and package the file _dylib_ under the folder for dynamic connection.

## Build on Windows Host

### 1. Brief introduction

With CMake, Elastos DID can be cross-compiled to run only on Windows as target platform, while compilation is carried out on a Windows host. Now only support 64-bit (32-bit later) target versions are supported.

### 2. Set up Environment

**Prerequisites**:

* Visual Studio IDE is required. The Community version can be downloaded at [Visual Studio downloads](https://visualstudio.microsoft.com/downloads/) for free.
* Download and install "Visual Studio Command Prompt (devCmd)" from [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ShemeerNS.VisualStudioCommandPromptdevCmd).
* Install 'Desktop development with C++' Workload

Start the program 'Visual Studio Installer'.

> Alternative: Start Visual Studio IDE. In the menu, go to "Tools >> Get Tools and Features", it will open the Visual Studio Installer.

Make sure 'Desktop development with C++' Workload is installed.

On the right side, make sure in the 'Installation details' all of the following are installed:

* "Windows 8.1 SDK and UCRT SDK" <- might have to be selected additionally
* "Windows 10 SDK (10.0.17134.0)" <- might have to be selected additionally
* "VC++ 2017 version 15.9 ... tools"
* "C++ Profiling tools"
* "Visual C++ tools for CMake"
* "Visual C++ ATL for x86 and x64"

Additional tools are optional, some additional ones are installed by default with the Workload.

After modifications, restarting of Visual Studio might be required.

### 3. Build to run on a host

To compile the project from source code for the target to run on Windows, carry out the following steps:

In Visual Studio, open Visual Studio Command Prompt from the menu "Tools >> Visual Studio Command Prompt". It will open a new terminal window.

> Note: To build for a 32-bit target , select `x86 Native Tools Command Console` to run building commands, otherwise, select `x64 Native Tools Command Console` for a 64-bit target.

Navigate to the previously downloaded folder that contains the source code of the DID project.

```
cd YOUR-PATH/Elastos.DID.Native.SDK
```

Enter the 'build' folder.

```
cd build
```

Create a new folder with the target platform name, then change directory.

```
mkdir win
cd win
```

Generate the Makefile in the current directory:

```
cmake -G "NMake Makefiles" -DCMAKE_INSTALL_PREFIX=outputs ..\..
```

Build the program:

```
nmake
```

Install the program:

```
nmake install
```

Create distribution package:

```
nmake dist
```

### 4.Use maven package

```
cd win/outputs/lib
```

静态连接就打包该文件夹下的a文件，动态连接就打包文件夹下的dylib文件即可。

Package the file _a_ under the folder for static connection and package the file _dylib_ under the folder for dynamic connection.
