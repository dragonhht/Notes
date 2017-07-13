# 1, Geadle的命令行

- 命令行选项

参数     | 完整参数           | 作用
------ | -------------- | ----------------------------------------------------------------------------------------------------------
-？, -h | --help         | 打印出所有可用的命令行选项， 包含描述信息
-b     | --build-file   | Gradle构建脚本的默认命名约定是build.gradle。 使用这个命令行选项可以执行一个特定名字的构建脚本 （比如， `gradle -b test.gradle`)
--     | --offline      | 通常， 构建声明的依赖必须在离线仓库中存在才可用， 如果这些依赖在本地依赖缓存中没有， 那么运行在一个没有网络连接环境中的构建都会失败。 使用这个选型可以让你以离线模式运行构建， 仅仅在本地缓存中检查依赖是否存在
参数选项   |                |
-D     | --system-prop  | Gradle是以一个JVM进程运行的。 和所有的Java进程一样， 你可以提供一个系统参数， 就像-Dmyprop=muvalue这样
-P     | --project-prop | 项目参数是构建脚本中可用的变量。 你可以使用这个选项直接构建脚本中传入的参数（如， -Pmyprop=myvalue）
日志选项   |                |
-i     | --info         | 在默认配置中， Gradle构建不会提供大量的输出信息。 通过这个选项可以将Gradle的日志级别改变到INFO以获得更多的信息
-s     | --stacktrace   | 在异常抛出时会打印出简短的堆栈跟踪信息， 帮助你进行调试
帮助任务   |                |
--     | tasks          | 显示项目中所有可运行的task， 包括他们的描述信息
--     | properties     | 显示项目中所有可用的属性

- 显示每个可用的task `gradle -q tasks` , 要获取关于task的更多信息， 可使用 `--all`, 完整命令为 `gradle -q tasks --all`

- 任务执行 `gradle 任务名` ， 一次可执行多个任务， 任务名也可以以驼峰式的缩写

- 排除一个任务， 使用参数`-x`可在执行时排除你想要排除的的任务

# 2, 构建Java项目

- 使用插件

```
// 添加Java插件
apply plugin: 'java'
// 添加war插件
apply plugin: 'war'
```

- 构建项目

> gradle build<br>
> 该任务会以正确的顺序编译你的源代码， 运行测试， 组装JAR文件

- 运行项目

> 在项目的根目录使用Java命令即可

- 修改属性变量并添加JAR文件头

```
// 定义项目版本
version = 0.1
// 设置Java版本编译兼容1.8
sourceCompatibility = 1.8
// 将Main-Class头添加到JAR文件代码清单中
jar {
  manifest {
    attributes 'Main-Class': 'Main-Class的位置'
  }
}
```

- 设置仓库和添加依赖

```
// 配置对Maven Central 2仓库maven2访问的快捷方式
repositories {
  mavenCentral()
}
// 添加依赖
dependencies {
  compile group: ‘org.apache.commons’, name: 'commons-long3', version: '3.1'
}
```

## Web开发

- 使用war

  - 添加war插件

  ```
  apply plugin: 'war'
  apply plugin: 'jetty'
  ```

  - 配置war

  ```
  webAppDirName: 'webfiles'
  war {
    from: 'static'
  }
  ```

- 使用jetty

  - 添加jetty插件

  ```
  apply plugin: 'jetty'
  ```

  - 配置jetty

  ```
  jettyRun {
    // 设置端口
    httpPort = 8081
    // 设置上下文路径
    contextPath = ‘MyProject’
  }
  ```
