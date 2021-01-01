---
layout: post
title: First Spring Boot Application 
category: Spring Boot
tags: [Spring Boot]
---

Quick note about Spring Boot.



## First Spring Boot Application

### 1.Creating the POM

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.0.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.3.0.RELEASE</version>
            </plugin>
        </plugins>
```



### 2.Writing the Code

- FirstSpringApplication.java

```java
@SpringBootApplication
public class FirstSpringApplication {

    public static void main(String[] args) {
        SpringApplication.run(FirstSpringApplication.class, args);
    }

}
```

- TestControl.java

```java
@RestController
public class TestControl {

    @RequestMapping("hello")
    public String hello(){
        return "hello Springboot";
    }
}
```



### 3.Creating an Executable Jar

- mvn -f module-name clean package

```
[INFO] Scanning for projects...
[INFO]
[INFO] ---------------------< com.stone:springboot-first >---------------------
[INFO] Building springboot-first 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:3.1.0:clean (default-clean) @ springboot-first ---
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:resources (default-resources) @ springboot-first ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.8.1:compile (default-compile) @ springboot-first ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 2 source files to D:\JAR\workspace\SpringBoot\st_sboot\springboot-first\target\classes
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:testResources (default-testResources) @ springboot-first ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\JAR\workspace\SpringBoot\st_sboot\springboot-first\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.8.1:testCompile (default-testCompile) @ springboot-first ---
[INFO] Changes detected - recompiling the module!
[INFO]
[INFO] --- maven-surefire-plugin:2.22.2:test (default-test) @ springboot-first ---
[INFO]
[INFO] --- maven-jar-plugin:3.2.0:jar (default-jar) @ springboot-first ---
[INFO] Building jar: D:\JAR\workspace\SpringBoot\st_sboot\springboot-first\target\springboot-first-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:2.3.0.RELEASE:repackage (repackage) @ springboot-first ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  6.743 s
[INFO] Finished at: 2021-01-01T17:07:55+08:00
[INFO] ------------------------------------------------------------------------
```



### 4.Running

java -jar module-name\target\springboot-first-1.0-SNAPSHOT.jar

```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.3.0.RELEASE)

2021-01-01 17:16:05.246  INFO 6580 --- [           main] com.stone.sboot.FirstSpringApplication   : Starting FirstSpringApplication v1.0-SNAPSHOT on DESKTOP-FQI55S0 with PID 6580 (D:\J
AR\workspace\SpringBoot\st_sboot\springboot-first\target\springboot-first-1.0-SNAPSHOT.jar started by Administrator in D:\JAR\workspace\SpringBoot\st_sboot)
2021-01-01 17:16:05.251  INFO 6580 --- [           main] com.stone.sboot.FirstSpringApplication   : No active profile set, falling back to default profiles: default
2021-01-01 17:16:07.964  INFO 6580 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2021-01-01 17:16:07.985  INFO 6580 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2021-01-01 17:16:07.986  INFO 6580 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.35]
2021-01-01 17:16:08.127  INFO 6580 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2021-01-01 17:16:08.127  INFO 6580 --- [           main] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 2685 ms
2021-01-01 17:16:08.463  INFO 6580 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2021-01-01 17:16:08.764  INFO 6580 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2021-01-01 17:16:08.781  INFO 6580 --- [           main] com.stone.sboot.FirstSpringApplication   : Started FirstSpringApplication in 4.382 seconds (JVM running for 5.137)
2021-01-01 17:21:39.748  INFO 6580 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2021-01-01 17:21:39.749  INFO 6580 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2021-01-01 17:21:39.763  INFO 6580 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 13 ms
```
