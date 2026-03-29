# WebFlux 常用配置

## pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>4.0.1</version>
    <relativePath/> <!-- lookup parent from repository -->
  </parent>

  <groupId>org.qiaice</groupId>
  <artifactId>untitled</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>untitled</name>
  <description>untitled</description>

  <properties>
    <java.version>25</java.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-devtools</artifactId>
      <scope>runtime</scope>
      <optional>true</optional>
    </dependency>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <scope>provided</scope>
      <optional>true</optional>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-webflux</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-webflux-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-r2dbc</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-r2dbc-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>io.asyncer</groupId>
      <artifactId>r2dbc-mysql</artifactId>
      <scope>runtime</scope>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <configuration>
          <annotationProcessorPaths> <!-- 配置注解处理器路径 -->
            <path>
              <groupId>org.projectlombok</groupId>
              <artifactId>lombok</artifactId>
            </path>
          </annotationProcessorPaths>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <configuration>
          <excludes> <!-- 打包时排除 lombok -->
            <exclude>
              <groupId>org.projectlombok</groupId>
              <artifactId>lombok</artifactId>
            </exclude>
          </excludes>
        </configuration>
      </plugin>
      <plugin> <!-- 解决单元测试警告 -->
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <configuration>
          <argLine>
            -Xshare:off
            -javaagent:${settings.localRepository}/org/mockito/mockito-core/${mockito.version}/mockito-core-${mockito.version}.jar
          </argLine>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>

```

## application.yaml

```yaml
# 日志相关配置
logging:
  charset:
    console: utf-8
    file: utf-8
  file:
    path: ./logs # 相对于项目根路径
    name: ${logging.file.path}/application.log
  pattern:
    console: "%d{yyyy-MM-dd'T'HH:mm:ssXXX} %clr(%5p) %clr(${PID:-}){magenta} %esb(){APPLICATION_NAME}%esb{APPLICATION_GROUP}[%15.15t] %clr(%-40.40logger{40}){cyan} : %m%n%wEx"
    file: "%d{yyyy-MM-dd'T'HH:mm:ssXXX} %5p ${PID:-} %esb(){APPLICATION_NAME}%esb{APPLICATION_GROUP}[%15.15t] %-40.40logger{40} : %m%n%wEx"
  logback:
    rollingpolicy: # 配置文件滚动策略
      file-name-pattern: ${logging.file.path}/spring-%d{yyyy-MM-dd}-no-%i.log
      max-file-size: 10MB
      max-history: 7 # 单位为天
  level: # 配置日志级别
    sql: debug
    web: debug
    org.qiaice: debug

# 服务器相关配置
server:
  port: 8080

# spring 相关配置
spring:
  application:
    name: application
  webflux:
    base-path: /api
    problemdetails:
      enabled: true
  r2dbc:
    url: r2dbc:mysql://localhost:3306/application?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=utf-8&rewriteBatchedStatements=true&autoReconnect=true&maxReconnects=3
    username: root
    password: 040905
```

## .gitignore

```.gitignore
# 系统元数据
.DS_Store
Thumbs.db
Desktop.ini

# 编译输出目录
out/
target/
build/
generated/
generated_tests/

# 日志
logs/

# IntelliJ IDEA
.idea/
*.iws
*.iml
*.ipr

# Gradle
.gradle/

# Kotlin
.kotlin/

# VS Code
.vscode/

# Eclipse
.settings/
.sts4-cache/
.apt_generated
.classpath
.factorypath
.project
.springBeans

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files
*.jar
*.war
*.nar
*.ear
*.zip
*.rar
*.tar.gz

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
replay_pid*
```



