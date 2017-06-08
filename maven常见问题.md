#mirror和repository 区别

mirror相当于一个拦截器，它会拦截maven对远程repository的相关请求,把请求里的远程repository地址,重定向到mirror里配置的地址.
````
     <mirror>
        <id>B</id>
        <mirrorOf>A</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://10.187.144.11:8081/nexus/content/groups/public</url>
    </mirror>
````
此时，B Repository被称为A Repository的镜像。
如果仓库X可以提供仓库Y存储的所有内容，那么就可以认为X是Y的一个镜像。
#scope依赖范围控制
* compile是默认的范围,编译范围依赖在所有的classpath可用，同时它们也会被打包。
* provided只有在当JDK 或者一个容器已提供该依赖之后才使用。例如你开发了一个web应用，你可能在编译classpath中需要可用的ServletAPI来编译一个servlet，但是你不会想要在打包好的WAR 中包含这个Servlet API；这个Servlet API JAR由你的应用服务器或者servlet 容器提供。已提供范围的依赖在编译classpath（不是运行时）可用。它们不是传递性的，也不会被打包。
* runtime 依赖在运行和测试系统的时候需要，但在编译的时候不需要。比如，你可能在编译的时候只需要JDBC API JAR，而只有在运行的时候才需要JDBC驱动实现。
* test在一般的编译和运行时都不需要，它们只有在测试编译和测试运行阶段可用。
* system范围依赖与provided 类似，但是你必须显式的提供一个对于本地系统中JAR 文件的路径。这么做是为了允许基于本地对象编译，而这些对象是系统类库的一部分。这样的构件应该是一直可用的，Maven也不会在仓库中去寻找它。如果你将一个依赖范围设置成系统范围，你必须同时提供一个systemPath元素。注意该范围是不推荐使用的（你应该一直尽量去从公共或定制的 Maven 仓库中引用依赖）。
* import 在maven项目中,父模块的dependencyManagement会包含大量的依赖。如果你想把这些依赖分类以更清晰的管理，是非常困难的，import scope依赖能解决这个问题。你可以把dependencyManagement放到单独的专门用来管理依赖的pom中，然后在需要使用依赖的模块中通过import scope依赖，就可以引入dependencyManagement。例如:
````
<project>  
    <modelVersion>4.0.0</modelVersion>  
    <groupId>com.test.sample</groupId>  
    <artifactId>base-parent1</artifactId>  
    <packaging>pom</packaging>  
    <version>1.0.0-SNAPSHOT</version>  
    <dependencyManagement>  
        <dependencies>  
            <dependency>  
                <groupId>junit</groupId>  
                <artifactid>junit</artifactId>  
                <version>4.8.2</version>  
            </dependency>  
            <dependency>  
                <groupId>log4j</groupId>  
                <artifactid>log4j</artifactId>  
                <version>1.2.16</version>  
            </dependency>  
        </dependencies>  
    </dependencyManagement>  
</project>  
````

````
<dependencyManagement>  
    <dependencies>  
        <dependency>  
            <groupId>com.test.sample</groupId>  
            <artifactid>base-parent1</artifactId>  
            <version>1.0.0-SNAPSHOT</version>  
            <type>pom</type>  
            <scope>import</scope>  
        </dependency>  
    </dependencies>  
</dependencyManagement>  
  
<dependency>  
    <groupId>junit</groupId>  
    <artifactid>junit</artifactId>  
</dependency>  
<dependency>  
    <groupId>log4j</groupId>  
    <artifactid>log4j</artifactId>  
</dependency>  
````