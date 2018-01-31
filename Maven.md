# Maven

## 1.Maven 介绍

### 1.1 What

1.maven 是一个用来将源码构建为一个可发布构件的工具

同时也是

1.项目管理工具，包含一个项目对象模型，一组标准集合，一个项目生命周期，一个项目依赖管理系统





### 1.2.约定优于配置

maven中设定了很多的默认值

maven 假定源码在src/main/java中

maven假定资源文件在src/main/resources中

maven假定测试源码在src/test/java中

maven假定测试资源文件在src/test/resources中





### 1.3 一个通用接口

在maven以前，不同的项目构建是不同的，不同的项目环境是什么样的，需要什么类库，类库需要放在那里...

Maven重新定义了构建软件的通用接口，当看见一个Maven管理的项目，就可以使用mvn install 。





### 1.4 基于插件的全局重用

maven 本身出来解读xml，管理生命周期外不负责任何实际的项目构建工作。

maven将实际的项目构建工作委派为一组maven插件，这些插件可以影响maven生命周期，提供对目标的访问。

而使用插件完成项目构建工作的事实允许了构建逻辑的全局重用。

例如运行单元测试，只需要在maven中使用surefire插件.

**Maven 将一般的构建任务抽象为插件，同时这些插件得到很好的维护和全局共享，我们不需要再重头开始定义自己的构建任务**



### 1.5 一个“项目” 概念模型

maven包含了一组关于软件项目和软件开发的语义规则平台，这个基于一个项目定义的模型包含如下特性

 -  依赖管理

    由于项目是一个包含组标识符、构建标识符、版本号的唯一坐标（GAV），所以可以根据该坐标声明依赖。

 -  远程仓库

    和项目依赖相关，我们可以根据GAV来创建Maven构件仓库。

 -  全局性构建逻辑重用

    插件被编写成和项目模型对象（POM） 一起工作，一切都被抽象到模型中，插件定义和自定义行为都在模型中进行。

 -  工具可移植性/集成

    IDE工具现在有了共同的地方寻找项目信息，现在Maven项目可以随意生成各个IDE自定义的项目对象模型。

 -  便于搜索和过滤构建

    类似Nexus这样的工具允许使用定义在POM中的信息对仓库中的内容进行索引和搜索







## 2. Maven

mvn help:describe -Dplugin=help -Dfull





## 3. 一个简单的Maven项目

### 1.创建

**mvn archetype:generate**

```java
C:\Users\renjie.li>mvn archetype:generate
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] >>> maven-archetype-plugin:3.0.1:generate (default-cli) > generate-sources @ standalone-pom >>>
[INFO]
[INFO] <<< maven-archetype-plugin:3.0.1:generate (default-cli) < generate-sources @ standalone-pom <<<
[INFO]
[INFO] --- maven-archetype-plugin:3.0.1:generate (default-cli) @ standalone-pom ---
[INFO] Generating project in Interactive mode
[WARNING] No archetype found in remote catalog. Defaulting to internal catalog
[INFO] No archetype defined. Using maven-archetype-quickstart (org.apache.maven.archetypes:maven-archetype-quickstart:1.0)
Choose archetype:
1: internal -> org.apache.maven.archetypes:maven-archetype-archetype (An archetype which contains a sample archetype.)
2: internal -> org.apache.maven.archetypes:maven-archetype-j2ee-simple (An archetype which contains a simplifed sample J2EE application.)
3: internal -> org.apache.maven.archetypes:maven-archetype-plugin (An archetype which contains a sample Maven plugin.)
4: internal -> org.apache.maven.archetypes:maven-archetype-plugin-site (An archetype which contains a sample Maven plugin site.
      This archetype can be layered upon an existing Maven plugin project.)
5: internal -> org.apache.maven.archetypes:maven-archetype-portlet (An archetype which contains a sample JSR-268 Portlet.)
6: internal -> org.apache.maven.archetypes:maven-archetype-profiles ()
7: internal -> org.apache.maven.archetypes:maven-archetype-quickstart (An archetype which contains a sample Maven project.)
8: internal -> org.apache.maven.archetypes:maven-archetype-site (An archetype which contains a sample Maven site which demonstrates
      some of the supported document types like APT, XDoc, and FML and demonstrates how
      to i18n your site. This archetype can be layered upon an existing Maven project.)
9: internal -> org.apache.maven.archetypes:maven-archetype-site-simple (An archetype which contains a sample Maven site.)
10: internal -> org.apache.maven.archetypes:maven-archetype-webapp (An archetype which contains a sample Maven Webapp project.)
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): 7: 7
Define value for property 'groupId': com.test.maven
Define value for property 'artifactId': app1
Define value for property 'version' 1.0-SNAPSHOT: :
Define value for property 'package' com.test.maven: :
Confirm properties configuration:
groupId: com.test.maven
artifactId: app1
version: 1.0-SNAPSHOT
package: com.test.maven
 Y: : Y
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Old (1.x) Archetype: maven-archetype-quickstart:1.1
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: basedir, Value: C:\Users\renjie.li
[INFO] Parameter: package, Value: com.test.maven
[INFO] Parameter: groupId, Value: com.test.maven
[INFO] Parameter: artifactId, Value: app1
[INFO] Parameter: packageName, Value: com.test.maven
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] project created from Old (1.x) Archetype in dir: C:\Users\renjie.li\app1
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 45.209 s
[INFO] Finished at: 2018-01-17T15:50:50+08:00
[INFO] Final Memory: 14M/133M
[INFO] ------------------------------------------------------------------------
```





### 2.构建

在pom.xml文件所在目录下运行**mvn install

执行结果：

~~~
C:\Users\renjie.li\app1>mvn install
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building app1 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ app1 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory C:\Users\renjie.li\app1\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ app1 ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to C:\Users\renjie.li\app1\target\classes
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ app1 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory C:\Users\renjie.li\app1\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ app1 ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to C:\Users\renjie.li\app1\target\test-classes
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ app1 ---
[INFO] Surefire report directory: C:\Users\renjie.li\app1\target\surefire-reports
Downloading: http://maven.aliyun.com/nexus/content/groups/public/org/apache/maven/surefire/surefire-junit3/2.12.4/surefire-junit3-2.12.4.pom
Downloaded: http://maven.aliyun.com/nexus/content/groups/public/org/apache/maven/surefire/surefire-junit3/2.12.4/surefire-junit3-2.12.4.pom (0 B at 0.0 KB/sec)
Downloading: http://maven.aliyun.com/nexus/content/groups/public/org/apache/maven/surefire/surefire-providers/2.12.4/surefire-providers-2.12.4.pom
Downloaded: http://maven.aliyun.com/nexus/content/groups/public/org/apache/maven/surefire/surefire-providers/2.12.4/surefire-providers-2.12.4.pom (0 B at 0.0 KB/sec)
Downloading: http://maven.aliyun.com/nexus/content/groups/public/org/apache/maven/surefire/surefire-junit3/2.12.4/surefire-junit3-2.12.4.jar
Downloaded: http://maven.aliyun.com/nexus/content/groups/public/org/apache/maven/surefire/surefire-junit3/2.12.4/surefire-junit3-2.12.4.jar (0 B at 0.0 KB/sec)

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running com.test.maven.AppTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.015 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ app1 ---
[INFO] Building jar: C:\Users\renjie.li\app1\target\app1-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ app1 ---
Downloading: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-utils/3.0.5/plexus-utils-3.0.5.pom
Downloaded: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-utils/3.0.5/plexus-utils-3.0.5.pom (0 B at 0.0 KB/sec)
Downloading: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-digest/1.0/plexus-digest-1.0.pom
Downloaded: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-digest/1.0/plexus-digest-1.0.pom (0 B at 0.0 KB/sec)
Downloading: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-components/1.1.7/plexus-components-1.1.7.pom
Downloaded: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-components/1.1.7/plexus-components-1.1.7.pom (0 B at 0.0 KB/sec)
Downloading: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-container-default/1.0-alpha-8/plexus-container-default-1.0-alpha-8.pom
Downloaded: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-container-default/1.0-alpha-8/plexus-container-default-1.0-alpha-8.pom (0 B at 0.0 KB/sec)
Downloading: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-utils/3.0.5/plexus-utils-3.0.5.jar
Downloading: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-digest/1.0/plexus-digest-1.0.jar
Downloaded: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-utils/3.0.5/plexus-utils-3.0.5.jar (0 B at 0.0 KB/sec)
Downloaded: http://maven.aliyun.com/nexus/content/groups/public/org/codehaus/plexus/plexus-digest/1.0/plexus-digest-1.0.jar (0 B at 0.0 KB/sec)
[INFO] Installing C:\Users\renjie.li\app1\target\app1-1.0-SNAPSHOT.jar to C:\02java\work\apache-magen-repository\com\test\maven\app1\1.0-SNAPSHOT\app1-1.0-SNAPSHOT.jar
[INFO] Installing C:\Users\renjie.li\app1\pom.xml to C:\02java\work\apache-magen-repository\com\test\maven\app1\1.0-SNAPSHOT\app1-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 9.534 s
[INFO] Finished at: 2018-01-17T16:11:27+08:00
[INFO] Final Memory: 16M/140M
[INFO] ------------------------------------------------------------------------
~~~

执行顺序

1.处理src/main/resources 

2.编译src/main/java

3.处理src/test/resources

4.编译src/test/java

5.执行测试，报告测试

6.执行打包，执行安装



### 3.基本项目对象模型（POM）

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.test.maven</groupId>
  <artifactId>app1</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>app1</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>

```

这是POM是最基本的，在实际执行的时候POM会复杂得多：定义多个依赖，自定义插件行为...

### 4.有效的POM

在POM文件所在目录下执行：**mvn help:effective-pom**

~~~xml
<!-- C:\Users\renjie.li\app1>mvn help:effective-pom                                    -->
<!-- [INFO] Scanning for projects...                                                   -->
<!-- [INFO]                                                                            -->
<!-- [INFO] ------------------------------------------------------------------------   -->
<!-- [INFO] Building app1 1.0-SNAPSHOT                                                 -->
<!-- [INFO] ------------------------------------------------------------------------   -->
<!-- [INFO]                                                                            -->
<!-- [INFO] --- maven-help-plugin:2.2:effective-pom (default-cli) @ app1 ---           -->
<!-- [INFO]                                                                            -->
<!-- Effective POMs, after inheritance, interpolation, and profiles are applied:       -->
<!--                                                                                   -->
<!-- <!-- ====================================================================== -- >  -->
<!-- <!--                                                                        -- >  -->
<!-- <!-- Generated by Maven Help Plugin on 2018-01-17T04:33:10                  -- >  -->
<!-- <!-- See: http://maven.apache.org/plugins/maven-help-plugin/                -- >  -->
<!-- <!--                                                                        -- >  -->
<!-- <!-- ====================================================================== -- >  -->
<!--                                                                                   -->
<!-- <!-- ====================================================================== -- >  -->
<!-- <!--                                                                        -- >  -->
<!-- <!-- Effective POM for project 'com.test.maven:app1:jar:1.0-SNAPSHOT'       -- >  -->
<!-- <!--                                                                        -- >  -->
<!-- <!-- ====================================================================== -- >  -->

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.test.maven</groupId>
  <artifactId>app1</artifactId>
  <version>1.0-SNAPSHOT</version>
  <name>app1</name>
  <url>http://maven.apache.org</url>
  <properties>
    <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <repositories>
    <repository>
      <releases>
        <enabled>true</enabled>
      </releases>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>nexus</id>
      <name>local private nexus</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    </repository>
    <repository>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
    </repository>
  </repositories>
  <pluginRepositories>
    <pluginRepository>
      <releases>
        <enabled>true</enabled>
      </releases>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>nexus</id>
      <name>local private nexus</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    </pluginRepository>
    <pluginRepository>
      <releases>
        <updatePolicy>never</updatePolicy>
      </releases>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
    </pluginRepository>
  </pluginRepositories>
  <build>
    <sourceDirectory>C:\Users\renjie.li\app1\src\main\java</sourceDirectory>
    <scriptSourceDirectory>C:\Users\renjie.li\app1\src\main\scripts</scriptSourceDirectory>
    <testSourceDirectory>C:\Users\renjie.li\app1\src\test\java</testSourceDirectory>
    <outputDirectory>C:\Users\renjie.li\app1\target\classes</outputDirectory>
    <testOutputDirectory>C:\Users\renjie.li\app1\target\test-classes</testOutputDirectory>
    <resources>
      <resource>
        <directory>C:\Users\renjie.li\app1\src\main\resources</directory>
      </resource>
    </resources>
    <testResources>
      <testResource>
        <directory>C:\Users\renjie.li\app1\src\test\resources</directory>
      </testResource>
    </testResources>
    <directory>C:\Users\renjie.li\app1\target</directory>
    <finalName>app1-1.0-SNAPSHOT</finalName>
    <pluginManagement>
      <plugins>
        <plugin>
          <artifactId>maven-antrun-plugin</artifactId>
          <version>1.3</version>
        </plugin>
        <plugin>
          <artifactId>maven-assembly-plugin</artifactId>
          <version>2.2-beta-5</version>
        </plugin>
        <plugin>
          <artifactId>maven-dependency-plugin</artifactId>
          <version>2.8</version>
        </plugin>
        <plugin>
          <artifactId>maven-release-plugin</artifactId>
          <version>2.3.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
    <plugins>
      <plugin>
        <artifactId>maven-clean-plugin</artifactId>
        <version>2.5</version>
        <executions>
          <execution>
            <id>default-clean</id>
            <phase>clean</phase>
            <goals>
              <goal>clean</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-resources-plugin</artifactId>
        <version>2.6</version>
        <executions>
          <execution>
            <id>default-testResources</id>
            <phase>process-test-resources</phase>
            <goals>
              <goal>testResources</goal>
            </goals>
          </execution>
          <execution>
            <id>default-resources</id>
            <phase>process-resources</phase>
            <goals>
              <goal>resources</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-jar-plugin</artifactId>
        <version>2.4</version>
        <executions>
          <execution>
            <id>default-jar</id>
            <phase>package</phase>
            <goals>
              <goal>jar</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
        <executions>
          <execution>
            <id>default-compile</id>
            <phase>compile</phase>
            <goals>
              <goal>compile</goal>
            </goals>
          </execution>
          <execution>
            <id>default-testCompile</id>
            <phase>test-compile</phase>
            <goals>
              <goal>testCompile</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>2.12.4</version>
        <executions>
          <execution>
            <id>default-test</id>
            <phase>test</phase>
            <goals>
              <goal>test</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-install-plugin</artifactId>
        <version>2.4</version>
        <executions>
          <execution>
            <id>default-install</id>
            <phase>install</phase>
            <goals>
              <goal>install</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-deploy-plugin</artifactId>
        <version>2.7</version>
        <executions>
          <execution>
            <id>default-deploy</id>
            <phase>deploy</phase>
            <goals>
              <goal>deploy</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-site-plugin</artifactId>
        <version>3.3</version>
        <executions>
          <execution>
            <id>default-site</id>
            <phase>site</phase>
            <goals>
              <goal>site</goal>
            </goals>
            <configuration>
              <outputDirectory>C:\Users\renjie.li\app1\target\site</outputDirectory>
              <reportPlugins>
                <reportPlugin>
                  <groupId>org.apache.maven.plugins</groupId>
                  <artifactId>maven-project-info-reports-plugin</artifactId>
                </reportPlugin>
              </reportPlugins>
            </configuration>
          </execution>
          <execution>
            <id>default-deploy</id>
            <phase>site-deploy</phase>
            <goals>
              <goal>deploy</goal>
            </goals>
            <configuration>
              <outputDirectory>C:\Users\renjie.li\app1\target\site</outputDirectory>
              <reportPlugins>
                <reportPlugin>
                  <groupId>org.apache.maven.plugins</groupId>
                  <artifactId>maven-project-info-reports-plugin</artifactId>
                </reportPlugin>
              </reportPlugins>
            </configuration>
          </execution>
        </executions>
        <configuration>
          <outputDirectory>C:\Users\renjie.li\app1\target\site</outputDirectory>
          <reportPlugins>
            <reportPlugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-project-info-reports-plugin</artifactId>
            </reportPlugin>
          </reportPlugins>
        </configuration>
      </plugin>
    </plugins>
  </build>
  <reporting>
    <outputDirectory>C:\Users\renjie.li\app1\target\site</outputDirectory>
  </reporting>
</project>
~~~





### 5.核心概念

#### 1.插件和目标

| 命令行类型  | 命令                     |
| ------ | ---------------------- |
| 单个插件目标 | mvn archetype:generate |
| 生命周期阶段 | mvn install            |



一个maven插件是一个或多个目标的集合。

一个目标是一个明确的任务，他可以作为一个单独的目标运行，或者作为一个大的构建的一部分和其他目标一起运行，一个目标就是maven中的一个工作单元。



目标通过配置属性进行配置，例如compiler:compile目标定义了一组配置参数，允许设置目标jdk版本或是否使用编译优化。

目标定义了一些配置参数，这些参数一般都提供了默认值（**约定优于配置**）。

#### 2.生命周期

mvn install 命令并没有指定一个插件目标，而是指定了一个maven**生命周期阶段**，一个**阶段**是在maven中的**构建生命周期**中的一个步骤。

生命周期是：包含在一个项目构建过程中一系列有序的阶段。

maven可以支持许多不同的生命周期，最常用的是maven默认生命周期。

这个生命周期中一开始的一个阶段是验证项目的基本完整性，最后一个阶段是把项目发布成产品。

生命周期被特地留的含糊，单独的定义为验证（validation），测试(test)，发布(deployment),而他们对于不同的项目意味着不同的事情。

例如打包这个阶段在一个项目里生成一个jar，而另一个项目中打包这个阶段可能生成一个war。

![一个生命周期是一些阶段的序列](image\maven/maven-default-lifecycle.png)



插件目标可以附着在生命周期的阶段上，随着生命周期阶段的移动，他会执行特定阶段上的特定插件目标。每个阶段可能绑定了零个或多个插件目标。

当执行package阶段，maven会执行的阶段以及阶段绑定的插件目标

![](image\maven\phases-to-goals.png)



#### 3.Maven坐标

Maven POM 为项目定义了一组唯一的标识符，他们可以用来唯一标识一个项目，一个以来或一个maven插件。

```xml
<groupId>com.test.maven</groupId>
  <artifactId>app1</artifactId>
  <version>1.0-SNAPSHOT</version>
<packaging>jar</packaging>
```

一个maven坐标就是“空间”中的一个点，当一个项目通过依赖，插件或者父项目引用和另外一个项目相关联的时候，Maven通过坐标来精确定位一个项目。

maven通过冒号作为分隔符书写坐标：groupId:artifactId:packaging:version.

groupId: 团队，小组，公司等标识符，通常是使用创建这个项目的组织的逆向域名，org.apache

artifactId:在groupId下表示一个单独的项目的标识符

version:一个项目的特定的版本，正在开发中的版本使用特殊标识符，版本号-SANPSHOT

packaging: **maven唯一坐标中不包含**。打包方式，默认是jar



#### 4.仓库

1.构件和插件在被需要的时候才会被下载

2.maven自带一个提供核心插件和依赖的远程仓库地址：http://repo1.maven.org/maven2

3.默认的远程仓库可以被替代或增加一个自己的仓库







## 1.Setting.xml

setting.xml中的settings元素包含多个用于配置maven执行的元素，使maven以不同的方式执行，例如pom.xml文件，但是setting.xml不应该绑定到特定的项目。这些值包括本地仓库地址，远程仓库替代服务器，权限信息。



setting.xml可以存在于两个地方

- 1.Maven install: **MAVEN_HOME/conf/setting.xml**
- 2.User install: **USER_HOME/.m2/setting.xml**

前面的setting.xml叫做全局设置，后面的setting.xml叫做用户设置。如果两个文件都存在，那么他们的内容会合并，以用户设置优先。

### 1.1setting.xml 文件顶级元素预览

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
      <localRepository/>
      <interactiveMode/>
      <usePluginRegistry/>
      <offline/>
      <pluginGroups/>
      <servers/>
      <mirrors/>
      <proxies/>
      <profiles/>
      <activeProfiles/>
    </settings>
```

The contents of the setting.xml can be interpolated(插值) using the following expressions:

- 1.${user.home} and all other system properties(since Maven 3)
- 2.${env.HOME} etc. for environment variables

Note that properties defined in profiles within setting.xml cannot be used for interpolation(插值);

#### 1.1.1 简单值

setting.xml中有一半的顶级元素都是简单值元素，表示一个范围的值用于在描述构建系统.

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      https://maven.apache.org/xsd/settings-1.0.0.xsd">
  <localRepository>${user.home}/.m2/repository</localRepository>
  <interactiveMode>true</interactiveMode>
  <usePluginRegistry>false</usePluginRegistry>
  <offline>false</offline>
  ...
</settings>
```

| Keywords           | Means |
| ------------------ | ----- |
| localRepository    |       |
| interactiveMode    |       |
| userPloginRegistry |       |
|                    |       |


