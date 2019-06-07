# Spring源码阅读

## Gradle

1. 安装配置

   查看版本：gradle -v

2. 与IDEA集成

3. 仓库配置

4. 



## 一、搭建源码阅读环境

1. 从官方仓库 <https://github.com/spring-projects/spring-framework> `Fork` 出属于自己的仓库。

2. 使用 `IntelliJ IDEA` 从 `Fork` 出来的仓库拉取代码。因为 Spring 项目比较大，从仓库中拉取代码的时间会比较长。

3. `gradlew cleanIdea :spring-oxm:compileTestJava`

4. ``` cmd
   > Task :spring-oxm:genJaxb
   [ant:javac] : warning: 'includeantruntime' was not set, defaulting to build.sysclasspath=last; set to false for repeatable builds
   
   BUILD SUCCESSFUL in 33m 42s
   108 actionable tasks: 59 executed, 49 up-to-date
   
   ```

5. 

idea搜索类快捷键:Ctrl+Shift+Alt+N





