# SpringBoot


SpringBoot 与 Spring 的关系：

![img](./images/1620.jpeg)

## 依赖安装

### 配置文件加载优先级

| 配置文件               | 加载优先级 |
| ---------------------- | ---------- |
| application.yml        | 最低       |
| application.yaml       | 其次       |
| application.properties | 最高       |

- 那里能看到的这个配置文件的加载顺序呢？

```xml
<!--.m2/repository/org/springframework/boot/spring-boot-starter-parent/2.7.3/spring-boot-starter-parent-2.7.3.pom-->
<build>
    <resources>
      <resource>
        <directory>${basedir}/src/main/resources</directory>
        <filtering>true</filtering>
        <includes>
          <include>**/application*.yml</include>
          <include>**/application*.yaml</include>
          <include>**/application*.properties</include>
        </includes>
      </resource>
    </resources>
</build>
```

打开`spring-boot-starter-parent`项目的`pom.xml`文件即可看到其依赖的配置文件加载顺序。顺便提醒你`spring-boot-starter-parent`项目继承了`spring-boot-dependencies`。

## 打包

SpringBoot 打包工具是`spring-boot-maven-plugin`。

### 打的jar包过大

优点：打包了所有的依赖文件`jar`包，位于`E:\demo_project\target\demo_project-0.0.1-SNAPSHOT.jar\BOOT-INF\lib`目录中，方便部署，服务器环境只需要提供`JVM`即可，不需要解决依赖问题

缺点：稍微大点的项目，肯定依赖了很多工具包，每次分发部署的`jar`包特别大，不利于网络传输，快速部署的特点。不能做到依赖一次性解决，次次都要打包依赖包的问题

### 不打包依赖包，减小jar包

如果能做到像[composer](https://getcomposer.org)那样，只需要分发`composer.json`和`composer.lock`两个文件即可，服务器部署时，只需要检查一次依赖的包又没有新增，包的版本有没有变更，如果上述都没有，则不需要再解决依赖，因为第一次解决依赖时，下载的包已经够用了。

现在要解决的问题就是，不打包依赖，但是又要让程序运行时，能在`classpath`中找到这些依赖包。

### springboot-thin-jar

看这个工具的名字，thin 瘦的，对我们就是要打包最瘦的项目`jar`包。

#### 使用方式

找到项目的`pom.xml`文件，给`maven`的打包工具`spring-boot-maven-plugin`添加依赖包`spring-boot-thin-layout`。

添加后的效果是这样的：

```xml
<build>
    <plugins>
        <!--打包插件-->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <excludes>
                    <exclude>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok</artifactId>
                    </exclude>
                </excludes>
            </configuration>
            <!--打最小的包-->
            <dependencies>
                <dependency>
                    <groupId>org.springframework.boot.experimental</groupId>
                    <artifactId>spring-boot-thin-layout</artifactId>
                    <version>1.0.28.RELEASE</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
</build>
```

配置完了执行`mvn clean package`，然后就能在项目的`target`目录下看到一个很小的项目`jar`包。

==注意==：

当我们执行`java -jar 项目.jar`项目时，他会使用默认仓库，如果默认仓库中不存在，他就会先下载依赖包，然后再执行我们`项目.jar`中的`main()`方法。

Windows 的本地默认仓库位置：`C:\Users\用户名\.m2\repository`

当然你也可以为项目指定默认仓库的位置：

```sh
java -Dthin.root=仓库路径 -jar 项目.jar
```

- -Dthin.root=仓库路径 指定项目仓库的默认路径

预热，类似热车，冬天人还未上车，先把发动机开启，暖风打开。等人上车了，车内已经暖和了。

项目预热，就是不启动项目，即不执行`main()`方法。先把项目的依赖解决，即需要下载的提前下载好。

```sh
java -Dthin.dryrun=true -Dthin.root=仓库路径，不指定就是本地默认仓库地址 -jar 项目.jar
```

- -Dthin.dryrun=true 开启预热
- -Dthin.root=. 项目的默认仓库路径
