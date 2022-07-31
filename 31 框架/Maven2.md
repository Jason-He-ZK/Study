## 多模块
[relativePath](https://blog.csdn.net/gzt19881123/article/details/105255138?spm=1005.2026.3001.5635&utm_medium=distribute.pc_relevant_ask_down.none-task-blog-2~default~OPENSEARCH~default-4.pc_feed_download_top3ask&depth_1-utm_source=distribute.pc_relevant_ask_down.none-task-blog-2~default~OPENSEARCH~default-4.pc_feed_download_top3ask)
## 概述

完成一个 java 项目，需要做哪些工作

```
1. 分析项目要做什么，知道项目有哪些组成部分。
2. 设计项目，通过哪些步骤，使用哪些技术。需要多少人， 多长的时间。
3. 组建团队，招人， 购置设备，服务器， 软件， 笔记本。
4. 开发人员写代码。 开发人员需要测试自己写代码。 重复多次的工作。
5. 测试人员，测试项目功能是否符合要求。
   测试开发人员提交代码-如果测试有问题--需要开发人员修改。。。直到-测试代码通过。
```

传统开发项目的问题（没有使用 maven 管理的项目）

```
1. 很多模块，模块之间有关系， 手工管理关系，比较繁琐。
2. 需要很多第三方功能， 需要很多jar文件，需要手工从网络中获取各个jar
3. 需要管理jar的版本， 你需要的是mysql.5.1.5.jar 拿你不能给给一个mysql.4.0.jar
4. 管理jar文件之间的依赖（a.jar 需要使用b.jar里面的类，a.class需要使用b.class， a依赖b类）
```

maven 改进项目的开发和管理

```
1. maven可以管理jar文件
2. 自动下载jar和他的文档，源代码
3. 管理jar直接的依赖， a.jar需要b.jar ， maven会自动下载b.jar
4. 管理你需要的jar版本
5. 帮你编译程序，把java编译为class
6. 帮你测试你的代码是否正确
7. 帮你打包文件，形成jar文件，或者war文件
8. 帮你部署项目
```

maven 的项目构建（构建是面向过程的，就是一些步骤，完成项目代码的编译，测试，运行，打包，部署等等）

```
1. 清理， 把之前项目编译的东西删除掉，我新的编译代码做准备。
2. 编译， 把程序源代码编译为执行代码， java-class文件（可以批量进行，与javac不一样）
3. 测试， maven可以执行测试程序代码，验证你的功能是否正确。（可以批量进行）
4. 报告， 生成测试结果的文件， 测试通过没有。
5. 打包， 把你的项目中所有的class文件，配置文件等所有资源放到一个压缩文件中。（这个压缩文件就是项目的结果文件）
         通常java程序，压缩文件是jar扩展名的。对于web应用，压缩文件扩展名是.war
6. 安装， 把5中生成的文件jar，war安装到本机仓库
7. 部署， 把程序安装好可以执行。
```

讲 maven 的使用，先难后易的
难：使用 maven 的命令，完成 maven 使用
易：在 idea 中直接使用 maven，代替命令

## maven 的安装和配置。

1. 从 maven 的官网下载 maven 的安装包 apache-maven-3.3.9-bin.zip
2. 解压安装包，解压到一个目录，非中文目录。
子目录 bin—mvn.cmd：执行程序
子目录 conf—settings.xml：maven 工具本身的配置文件
3. 配置环境变量
在系统的环境变量中，指定一个 M2_HOME：M2_HOME=D:\work\maven_work\apache-maven-3.3.9
再把 M2_HOME 加入到 path 之中，在所有路径之前加入 %M2_HOME%\bin;
4. 验证，新的命令行中，执行 mvn -v（需要配置 JAVA_HOME ，指定 jdk 路径）
出现如下内容，maven 安装，配置正确。 
```
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:4
Maven home: D:\work\maven_work\apache-maven-3.3.9
Java version: 1.8.0_40, vendor: Oracle Corporation
Java home: C:\java\JDK8-64\jre
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 7", version: "6.1", arch: "amd64", family: "dos"
```

## maven 核心概念

```
1. * POM（项目对象模型） ： 一个文件，名称是pom.xml
   maven把一个项目当做一个模型使用。控制maven构建项目的过程，管理jar依赖。
1. * 约定的目录结构 ： maven项目的目录和文件的位置都是规定的。
2. * 坐标 ： 是一个唯一的字符串，用来表示资源的。
3. * 依赖管理 ： 管理你的项目可以使用jar文件
4. 仓库管理（了解） ：你的资源存放的位置
5. 生命周期 (了解) ： maven工具构建项目的过程，就是生命周期。
6. 插件和目标（了解）：执行maven构建的时候用的工具是插件
7. 继承
8. 聚合
```

### maven 约定的目录结构

```
- Hello文件夹（每一个maven项目在磁盘中都是一个文件夹（项目-Hello））
  - src文件夹
    - main文件夹         #放你主程序java代码和配置文件
      - java文件夹                      #你的程序包和包中的java文件
      - resources文件夹             #你的java程序中要使用的配置文件
    - test文件夹           #放测试程序代码和文件的（可以没有）
      - java文件夹                      #测试程序包和包中的java文件
      - resources文件夹            #测试java程序中要使用的配置文件
  pom.xml                            #maven的核心文件（maven项目必须有）
```

### mvn compile 编译 src/main 目录下的所有 java 文件的，会下载东西

原因：maven 工具执行的操作需要很多插件（java 类--jar 文件）完成的

下载内容：jar 文件--叫做插件--插件是完成某些功能

下载内容存放位置：默认仓库（本机仓库）

执行 mvn compile， 结果是在项目的根目录下生成 target 目录（结果目录），存放最后的 class 文件

### 仓库

存放内容：1 maven 使用的插件（各种 jar），2 项目使用的 jar(第三方的工具)

使用驱动顺序：本地仓库---> 私服---> 镜像---> 中央仓库（最终都会在本地存一份）

仓库的分类

- 本地仓库：就是你的个人计算机上的文件夹，存放各种 jar 
   - C:\Users\（登录操作系统的用户名）Administrator.m2\repository
   - 修改本机仓库位置（建议先备份）
maven 安装目录/conf/settings.xml： 新目录（非中文目录）
- 远程仓库：在互联网上的，使用网络才能使用的仓库 
   - 中央仓库，最权威的， 所有的开发人员都共享使用的一个集中的仓库，
中央仓库的地址：[https://repo.maven.apache.org](https://repo.maven.apache.org)
   - 中央仓库的镜像：就是中央仓库的备份， 在各大洲，重要的城市都是镜像。
   - 私服，在公司内部，在局域网中使用的， 不是对外使用的。

### pom（项目对象模型，是一个 pom.xml 文件）

坐标：唯一的，在互联网中唯一标识一个项目

```xml
<groupId>公司域名的倒写</groupId>
<artifactId>自定义的项目名称</artifactId>
<version>自定义的版本号</version>

https://mvnrepository.com搜索坐标， 使用groupId 或者 artifactId作为搜索条件
```

packaging：打包后压缩文件的扩展名，默认是 jar ，web 应用是 war

依赖（项目中要使用的各种资源说明，相当于 java 中的 import）

```xml
<dependencies>
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.9</version>
  </dependency>
</dependencies>
```

properties：设置属性

build：maven 在进行项目的构建时， 配置信息（例如指定编译 java 代码使用的 jdk 的版本等）

### maven 生命周期， maven 的命令，maven 的插件

maven 的生命周期（构建项目的过程）：清理，编译，测试，报告，打包，安装，部署

maven 的命令：maven 独立使用，通过命令，完成 maven 的生命周期的执行。

maven 的插件： maven 命令执行时，真正完成功能的是插件，插件就是一些 jar 文件， 一些类。

单元测试（测试方法）：用的是 junit（专门测试的框架（工具））

- junit 测试的内容： 测试的是类中的方法， 每一个方法都是独立测试的。方法是测试的基本单位（单元）
- maven 借助单元测试，批量的测试类中的大量方法是否符合预期
- 使用步骤 
   1. 加入依赖，在 pom.xml 加入单元测试依赖 
```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.11</version>
  <scope>test</scope>
</dependency>
```

   2. 在 maven 项目中的 src/test/java 目录下，创建测试程序。 
```
推荐的创建类和方法的格式
	测试类的名称 是Test + 你要测试的类名，TestHelloMaven
	测试的方法名称 是：test + 方法名称，testAdd

方法要求：public，无返回值，在方法的上面加入 @Test

@Test
public void testAdd(){
    HelloMaven hello = new HelloMaven();
    int res = hello.add(10,20);
    //判断结果是否正确
    Assert.assertEquals(30,res);
}
```

mvn compile

编译 main/java/目录下的 java 为 class 文件， 同时把 class 拷贝到 target/classes 目录下面

把 main/resources 目录下的所有文件 都拷贝到 target/classes 目录下

。。。

```xml
<build>
  <!--配置拆件-->
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven compiler plugin</artifactId>
      <version> 3.8.1</version>
      <configuration>
        <source>1.8</source>
        <target>1.8</target>
      </configuration>
    </plugin>
  </plugins>
</build>
```

## idea 中设置 maven

idea 中内置了 maven ，一般不使用内置的（不方便）

使用自己安装的 maven， 需要覆盖 idea 中的默认的设置。让 idea 指定 maven 安装位置等信息

```xml
配置的入口：file--settings（other setting）--Build, Excution,Deployment--Build Tools--Maven 

Maven Home directory: maven的安装目录
User Settings File :  就是maven安装目录conf/setting.xml配置文件
Local Repository :    本机仓库的目录位置

--Maven--runner--vm potion：-DarchetypeCatalog=internal，加快项目创建
--Maven--runner--vm potion：jdk
```

使用模版创建项目（创建 module 时选中 create from archetype）

- maven-archetype-quickstart : 普通的 java 项目
- maven-archetype-webapp : web 工程

导入现成的 maven 项目

## 依赖范围（scope）

scope 的值有 compile（默认）（所有阶段），test（测试阶段）, provided（提供者，编译测试需要）

scope:表示依赖使用的范围，也就是在 maven 构建项目的那些阶段中起作用。

- maven 构建项目：编译， 测试 ，打包， 安装 ，部署
- junit 的依赖范围是 test

```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.11</version>
  <scope>test</scope>
</dependency>

<dependency>
  <groupId>a</groupId>
  <artifactId>b</artifactId>   b.jar
  <version>4.11</version>
</dependency>

<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>3.1.0</version>          servlet.jar
  <scope>provided</scope> 提供者
</dependency>
```

你在写项目的中的用到的所有依赖（jar ） ，必须在本地仓库中有。没有必须通过 maven 下载， 包括 provided 的都必须下载。

## maven 常用操作

maven 的属性设置

- 设置 maven 的常用属性 
```xml
<properties>
  <!--maven构建项目使用的是utf-8 ，避免中文的乱码-->
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <!--编译java代码使用的jdk版本-->
  <maven.compiler.source>1.8</maven.compiler.source>
  <!--你的java项目应该运行在什么样的jdk版本-->
  <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

maven 的全局变量

- 1.在 < properties> 通过自定义标签声明变量（标签名就是变量名）
- 2.在 pom.xml 文件中的其它位置，使用 ${标签名} 使用变量的值
- 一般是定义依赖的版本号，要使用多个相同的版本号，先使用全局变量定义， 在使用 ${变量名}

资源插件

- mybatis 中 SQL 映射文件会用到这个插件
- 默认没有使用 resources 的时候， maven 执行编译代码时，
会把 src/main/resource 目录中的文件拷贝到 target/classes 目录中。
对于 src/main/java 目录下的非 java 文件不处理，不拷贝
- 资源插件的作用就是让 maven 拷贝这些需要用到的文件

```xml
<build>
  <resources>
    <resource>
      <directory>src/main/java</directory><!--所在的目录-->
      <includes><!--包括目录下的.properties,.xml 文件都会扫描到-->
        <include>**/*.properties</include>
        <include>**/*.xml</include>
      </includes>
      <!-- filtering 选项 false 不启用过滤器， *.property 已经起到过滤的作用了 -->
      <filtering>false</filtering>
    </resource>
  </resources>
</build>
```
