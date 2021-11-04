---
description: >-
  此文档可以帮助开发人员快速了解DID的使用，主要面向Swift开发。以下步骤说明是为xcode10及以上版本编写的，使用IOS11
  SDK，如果您使用是旧版本的xcode，可能在使用之前需要更新xcode。
---

# Build and installation

## Build from source

```
$ git clone https://github.com/elastos/Elastos.DID.Swift.SDK.git
$ cd Elastos.DID.Swift.SDK
$ pod install
$ open ElastosDIDSDK.xcworkspace
```

然后可以打开ElastosDIDSDKTests运行单元测试

## Use cocapods&#x20;

使用CocoaPods将ElastosDIDSDK集成到项目中，在podfile中指定：

```
pod 'ElastosDIDSDK'
```

#### **步骤1： 下载CocoaPods**

#### CocoaPods是swift的依赖管理器，它简化了在项目中使用DID等第三方库的过程。CocoaPods通过在终端app中运行以下命令安装：

```
$ sudo gem install cocoapods
$ pod setup
```

#### 步骤2：创建podfile

cocoapods需要管理的项目依赖在podfile中指定，在与xcode项目.xcodeproj相同的目录中创建此文件，cd到项目的根目录，运行以下命令：

```
$ touch Podfile
$ open -a Xcode Podfile
```

将以下代码片段贴入podfile中：

```
target :"YOUR-APP-PROJECT-NAME" do
    source 'https://github.com/CocoaPods/Specs.git'
    platform :ios, '9.0'
    pod 'ElastosDIDSDK', '~> 2.2.0'
end
```

#### 步骤3： 下载依赖库

```
$ pod install
```

请使用xocde生成的.xcworkspace来打开项目：

```
$ open <YourProjectName>.xcworkspace
```

#### 步骤4： 使用

此时，已经准备好使用ElastosDIDSDK了，只需要 import ElastosDIDSDK 即可。
