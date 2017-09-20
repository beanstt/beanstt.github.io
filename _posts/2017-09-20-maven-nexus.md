---
layout: post
title:  "[答疑] - Nexus私服停运后，编译项目时如何禁止jar远程私服下载调用"
date:   2017-09-20 10:36:00 +0800
categories: maven
comments: true
---

```
sequenceDiagram
    loop every day
        A->>B: How are you?
        B->>A: Great!
    end
```

本问题答疑以名为third.jar实例。
### 问题
    在使用私服时，上传了很多自己的jar或者公网仓库没有的第三方jar，Nexus私服停运后，编译项目时如何禁止jar远程私服下载调用？

### 前提
    本地.m2文件夹中保存有使用过Nexus私服下载的third.jar文件；

### 答疑
    使用私服或者公网仓库下载jar时候，由于本地.m2对应的third的jar文件夹里保存了一份_remote.repositories记录文件，所以当我们私服撤销后，我们需要做的事，就是要把这个文件删除掉即可。

### _remote.repositories内容实例
    ```
    #NOTE: This is an Aether internal implementation file, its format can be changed without prior notice.
    #Wed May 04 13:26:01 CST 2016
    ojdbc6-11.2.0.1.0.pom>nexus=
    ojdbc6-11.2.0.1.0.jar>nexus=
    ```
    其中>nexus=就是仓库的名称，因本地setting.xml配置不同而不同。