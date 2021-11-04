---
description: >-
  The compilation of sources writed by typescript and is used for javascript
  project.
---

# Build and installation

## Build from source

```
$ git clone https://github.com/elastos/Elastos.DID.JS.SDK.git
$ cd Elastos.DID.JS.SDK
$ npm install -D
$ cd tests
$ npm install -D
$ sudo npm link ..
$ cd ..
$ npm run build
```

## Use maven package

nodejs项目使用dist/did.js即可，浏览器项目使用dist/es/下的js文件即可。
