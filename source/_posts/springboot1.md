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
updated: '2024-03-05T11:04:58.238+08:00'
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

![image.png](https://s2.loli.net/2024/03/05/KpOPzZvJxaHDfNQ.png)

{% tabs 部署Jar包 %}

<!-- tab windows下运行Jar包 -->

在windows下我们这样运行这个项目：

```shell
java -jar xxx.jar
```

<!-- endtab -->

<!-- tab Linux下运行Jar包 -->

而在Linux下可以使用软连接的方式更方便地管理Jar包：

- 建立软链接

  ```shell
  ln -s <jar包绝对路径> /etc/init.d/<自定义项目名称>
  ```
- 启动服务、重启服务、查看服务状态、停止服务

  ```shell
  service <自定义项目名称> start/restart/status/stop
  ```

<!-- endtab -->

{% endtabs %}

实际应用中，我们需要将打包的项目上传到远程服务器运行，而Jar包文件过大会影响我们上传的效率，这时，我们可以通过打包时分离项目依赖来减小Jar包体积。

## 分离项目依赖

将上文`build.gradle`里添加的内容替换为如下内容：

```gradle
// 将依赖包复制到lib目录
tasks.register('copyJar', Copy) {
    // 清除现有的lib目录
    delete "$buildDir\\libs\\lib"
    from configurations.runtimeClasspath
    into "$buildDir\\libs\\lib"
}

bootJar {
    // jar包名称，默认是settings.gradle下的工程名 rootProject.name = 'esp_front'
    archiveBaseName = 'esp_front'
    // jar包版本号
    archiveVersion = '0.0.1'
    // 排除所有的jar
    excludes = ["*.jar"]
    // lib目录的清除和复制任务
    dependsOn copyJar

    // 指定依赖包的路径
    manifest {
        attributes "Manifest-Version": 1.0,
                'Class-Path': configurations.runtimeClasspath.files.collect { "lib/$it.name" }.join(' ')
    }
    launchScript()
}
```

{% folding blue::在老版本的Gradle中你可能需要将代码更换为如下内容 %}

```gradle
// 将依赖包复制到lib目录
task copyJar(type: Copy) {
    // 清除现有的lib目录
    delete "$buildDir\\libs\\lib"
    from configurations.compileClasspath
    into "$buildDir\\libs\\lib"
}

// 配置bootJar进行打包
bootJar {
    // jar包名称，默认是settings.gradle下的工程名 rootProject.name = 'esp_front'
    baseName = 'esp_front'
    // jar包版本号
    version =  '0.0.1'
    // 排除所有的jar
    excludes = ["*.jar"]
    // lib目录的清除和复制任务
    dependsOn copyJar
    // 配置目录的清除和复制任务
    dependsOn copyConfigFile

    // 指定依赖包的路径
    manifest {
        attributes "Manifest-Version": 1.0,
                'Class-Path': configurations.compileClasspath.files.collect { "lib/$it.name" }.join(' ')
    }
    launchScript()
}
```

{% endfolding %}

然后重新执行bootJar命令，可以发现打包的项目分为了一个lib文件夹和一个Jar文件，其中lib文件夹下是该项目的依赖包，将lib文件夹和Jar文件一起上传到服务器运行即可（保持lib文件夹与Jar文件同级）。

![image.png](https://s2.loli.net/2024/03/05/flHBzd9nQT71YyD.png)

后续部署时，如果仅仅项目代码有变化而项目依赖没有变化，则重新上传到服务器时不需要重复上传这个文件夹，仅仅上传Jar文件即可。
