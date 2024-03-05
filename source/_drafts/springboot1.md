---
abbrlink: ''
categories:
- - java
date: '2024-03-05T09:54:22.165582+08:00'
tags:
- bootJar
- Gradle
- SpringBoot
- Jar
title: SpringBoot项目使用Gradle打包为可执行Jar包并分离项目依赖
updated: '2024-03-05T09:54:29.652+08:00'
---
## 前言

传统的Java应用程序我们会打包成war包，并配置tomcat来运行它。

在SpringBoot框架下，官方封装了一个bootJar的脚本以便于我们可以直接把项目打包成Jar包，并且不需要依赖外部tomcat运行。

## 将项目打包为Jar包并运行

在我们的项目根目录的 `build.gradle`文件后添加以下内容：

```gradle
bootJar {
    launchScript()
}
```

而后使用Gradle的 `bootJar`命令即可将项目打包为Jar包。



{% tabs 部署Jar包 %}

`<!-- tab windows下运行Jar包 -->`

在windows下我们这样运行这个项目：

```shell
java -jar xxx.jar
```

`<!-- endtab -->`

`<!-- tab Linux下运行Jar包 -->`

而在Linux下可以使用软连接的方式更方便地管理Jar包：

- 建立软链接
  
  ```shell
  ln -s <jar包绝对路径> /etc/init.d/<自定义项目名称>
  ```
- 启动服务、重启服务、查看服务状态、停止服务
  
  ```shell
  service <自定义项目名称> start/restart/status/stop
  ```

`<!-- endtab -->`

{% endtabs %}

实际应用中，我们需要将打包的项目上传到远程服务器运行，而Jar包文件过大会影响我们上传的效率，这样便有了下面一步

