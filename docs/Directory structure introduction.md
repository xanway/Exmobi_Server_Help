### 2.1 目录结构介绍

新建工程的目录结构

```
src
  |---com		代码目录
  |---config.xml	服务参数配置文件，ExMobi管理端会自动读取该路径文件展示参数并支持动态配置
  |---log4j.properties  (可选)日志配置
  |---mvc-config.xml  SpringMVC扫描配置
  |---spring-db.xml  (可选)数据库SpringMVC配置
WebContent      服务根目录
    | --- WEB-INF  整个WEB中最安全的目录，无法直接访问，若访问，需要在web.xml中配置
    |      |-- lib           存放第三方的jar文件
    |      |-- views         存放jsp文件
    |      └-- web.xml  WEB的部署描述文件
    |---   css  (可选)存放所有的*.css文件
    |---   js   (可选)存放所有的*.js文件
    |---   jsp  (可选)存放所有的jsp文件
    |---   index.jsp  (可选)欢迎页面
```

编译后WAR包的目录结构

```
ExMobiService      服务根目录
    | --- WEB-INF  整个WEB中最安全的目录，无法直接访问，若访问，需要在web.xml中配置
    |      |-- classes  保存所有的*.class文件    所有的class都要放在包中
    |      |   |-- config.xml服务参数配置文件，ExMobi管理端会自动读取该路径文件展示参数并支持动态配置
    |      |   |-- api(可选)用于存放API定义文件的目录，使用SpringMVC开发的服务，无需定义该目录及ac文件
    |      |   └-- api.ac(可选)API定义文件，服务端会解析这些文件并展示在API管理中
    |      |-- lib           存放第三方的jar文件
    |      |-- views         存放jsp文件
    |      └-- web.xml  WEB的部署描述文件
    |---   css  (可选)存放所有的*.css文件
    |---   js   (可选)存放所有的*.js文件
    |---   jsp  (可选)存放所有的jsp文件
    |---   index.jsp  (可选)欢迎页面
```