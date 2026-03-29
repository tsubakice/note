# WebMvc 函数式端点

## .gitignore

```.gitignore
# 无用元数据
.DS_Store

# 编译输出
target/
build/

# 日志
logs/

# IntelliJ IDEA
.run/
.idea/
*.iws
*.iml
*.ipr

# Gradle & Kotlin
.gradle/
.kotlin/

# VS Code
.vscode/

```

## pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>4.0.3</version>
    <relativePath/> <!-- lookup parent from repository -->
  </parent>

  <groupId>org.qiaice</groupId>
  <artifactId>application</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>application</name>
  <description>application</description>

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
      <artifactId>spring-boot-starter-webmvc</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-webmvc-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>com.mysql</groupId>
      <artifactId>mysql-connector-j</artifactId>
      <scope>runtime</scope>
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
  file:
    path: ./logs # 相对于项目根路径
    name: ${logging.file.path}/application.log
  pattern:
    console: "%d{yyyy-MM-dd'T'HH:mm:ssXXX} %clr(%5p) %clr(${PID:-}){magenta} %esb(){APPLICATION_NAME}%esb{APPLICATION_GROUP}[%15.15t] %clr(%-40.40logger{40}){cyan} : %m%n%wEx"
    file: "%d{yyyy-MM-dd'T'HH:mm:ssXXX} %5p ${PID:-} %esb(){APPLICATION_NAME}%esb{APPLICATION_GROUP}[%15.15t] %-40.40logger{40} : %m%n%wEx"
  logback:
    rollingpolicy:
      file-name-pattern: ${logging.file.path}/spring-%d{yyyy-MM-dd}-no-%i.log
      max-file-size: 10MB
      max-history: 7 # 单位为天
  level:
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
  mvc:
    servlet:
      path: /api
    problemdetails:
      enabled: true
    log-request-details: true
  jpa:
    properties:
      hibernate: # 格式化并高亮 sql 语句
        format_sql: true
        highlight_sql: true
        use_sql_comments: true
    open-in-view: false
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/application?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=utf-8&rewriteBatchedStatements=true&autoReconnect=true&maxReconnects=3
    username: root
    password: 040905
  security:
    jjwt:
      secret: LjB6uPiYtZPETTkhEVkyp/uF9EhjfIs4956TXt8bias=
      issuer: tsubaki
      expiration: 1h # 支持的时间单位: m, h, d, 不加单位默认为秒
      ignore:
        - /api/auth/login/email+passwd

```

## Result.java

```java
package org.qiaice.model.api;

import org.springframework.http.HttpStatus;
import org.springframework.util.StringUtils;

import java.util.Objects;

public record Result<T>(Integer code, String message, T data, Long timestamp) {
    private static final Object NULL = new Object();

    public static <T> Result<T> build(HttpStatus status) {
        return build(status, status.getReasonPhrase());
    }

    public static <T> Result<T> build(HttpStatus status, String message) {
        return build(status, message, null);
    }

    @SuppressWarnings(value = "unchecked")
    public static <T> Result<T> build(HttpStatus status, String message, T data) {
        return new Result<>(status.value(), StringUtils.hasText(message) ? message : status.getReasonPhrase(),
                Objects.nonNull(data) ? data : (T) NULL, System.currentTimeMillis());
    }
}

```

## BaseHandler.java

```java
package org.qiaice.handler;

import org.qiaice.model.api.Result;
import org.springframework.http.HttpStatus;
import org.springframework.web.servlet.function.ServerResponse;

import java.util.Objects;

public class BaseHandler {
    private static final ServerResponse.BodyBuilder BUILDER = ServerResponse.ok();

    protected <T> ServerResponse ok(T data) {
        HttpStatus status = Objects.nonNull(data) ? HttpStatus.OK : HttpStatus.NO_CONTENT;
        return BUILDER.body(Result.build(status, status.getReasonPhrase(), data));
    }
}

```

