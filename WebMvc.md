# WebMvc 常用配置

##  pom.xml

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
  <artifactId></artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name></name>
  <description></description>

  <properties>
    <java.version>25</java.version>
    <jjwt.version>0.13.0</jjwt.version>
    <mybatis-plus.version>3.5.15</mybatis-plus.version>
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
      <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-validation-test</artifactId>
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
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-spring-boot4-starter</artifactId>
      <version>${mybatis-plus.version}</version>
    </dependency>
    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter-test</artifactId>
      <version>${mybatis-plus.version}</version>
    </dependency>
    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-jsqlparser</artifactId>
      <version>${mybatis-plus.version}</version>
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
    <dependency>
      <groupId>io.jsonwebtoken</groupId>
      <artifactId>jjwt-api</artifactId>
      <version>${jjwt.version}</version>
    </dependency>
    <dependency>
      <groupId>io.jsonwebtoken</groupId>
      <artifactId>jjwt-impl</artifactId>
      <version>${jjwt.version}</version>
      <scope>runtime</scope>
    </dependency>
    <dependency>
      <groupId>io.jsonwebtoken</groupId>
      <artifactId>jjwt-jackson</artifactId> <!-- or jjwt-gson if Gson is preferred -->
      <version>${jjwt.version}</version>
      <scope>runtime</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-redis-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-mail</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-mail-test</artifactId>
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
  mvc:
    servlet: # 配置请求基路径
      path: /api
    problemdetails: # 启用 problemdetails
      enabled: true
  jpa:
    open-in-view: false
    properties:
      hibernate: # 显示 sql 语句
        format_sql: true
        use_sql_comments: true
  datasource: # datasource 相关配置
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/application?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=utf-8&rewriteBatchedStatements=true&autoReconnect=true&maxReconnects=3
    username: root
    password: 040905
  security:
    jwt: # jwt 相关配置
      secret: 1jTsPdo25uOzX0d/ZrRRrTrG7eG+mA3I7q4PZUFEOCk= # 密钥，由 Jwts.SIG.HS256 生成
      issuer: Qiaice # 签发者
      expiration: 1h # 有效时长，支持单位(ms, s, m, h, d, w, M)，区分大小写，默认单位 ms
      ignore: # 不需要校验 jwt 的 url
        - /api/auth/login
  data:
    redis: # redis 相关配置
      url: redis://040905@localhost:6379/0
  rabbitmq:
    addresses: 47.108.130.188
    port: 5672
    username: root
    password: 040905
    virtual-host: /rabbitmq
  mail: # 邮件发送相关配置
    host: smtp.qq.com
    port: 465 # 465 或 587
    username: boyi.zh.life@qq.com
    password: emragdnsfvogcjij # 特别注意，这里是授权码，不是邮箱密码
    send:
      from: ${spring.mail.username}
      subject: 来自新网页或移动设备的访问
      template: template/code.html
    properties:
      mail:
        smtp:
          ssl:
            enable: true

# mybatis-plus 相关配置
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl # 将 mybatis 日志输出到控制台
    map-underscore-to-camel-case: true # 开启驼峰命名转换，需要类中字段名和表中列名相对应    cId => c_id
    cache-enabled: false # 是否开启二级缓存
    # 使用 mybatis-plus 的枚举处理器
    default-enum-type-handler: com.baomidou.mybatisplus.core.handlers.MybatisEnumTypeHandler
  type-aliases-package: org.qiaice.model # 扫描指定包中的类，以避免写全类名
  mapper-locations: classpath:/mapper/**/*.xml # 扫描 mapper 文件
  global-config:
    db-config:
      id-type: auto # 设置主键增长策列
      update-strategy: not_null # 仅更新非空字段
      # 逻辑删除，对 sql 效率有影响，请斟酌使用
      # logic-delete-field: deleted # 逻辑删除字段名(实体类字段名)
      # logic-delete-value: 1 # 逻辑已删除值(默认为 1)
      # logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
    
# springdoc-openapi 项目配置
springdoc:
  swagger-ui:
    path: /swagger-ui # swagger 前端请求路径
    tags-sorter: alpha # 按字母排序
    operations-sorter: alpha # or method 按方法定义顺序排序
  api-docs:
    enabled: true # 开启文档功能
    path: /v3/api-docs # 获取 api 文档的 json 格式描述(末尾加上 .yaml 后缀可获得文档的 yaml 格式)
  group-configs: # 分组配置，可配置多个分组
    - group: default # 分组名称
      packages-to-scan: org.qiaice # 配置要扫描的包的路径，一般配置到启动类所在的包名
      paths-to-match: /** # 配置需要匹配的路径

# knife4j 的增强配置
knife4j:
  enable: true # 开启 knife4j 增强配置
  production: false # 标识是否为生产环境(是否关闭接口文档)
  setting:
    language: zh_cn
    enable-swagger-models: true
    swagger-model-name: 实体列表
    enable-dynamic-parameter: true # 开启动态请求参数
  basic: # 配置 Swagger 的 Basic 认证功能
    enable: false
    username: tsubaki # 用户名自定义
    password: 040905 # 密码自定义
    
# 阿里云 OSS 相关配置
oss:
  domain: https://sky-take-out-zby1.oss-cn-chengdu.aliyuncs.com # OSS 资源服务器
  endpoint: https://oss-cn-chengdu.aliyuncs.com # 访问 OSS 的入口端点
  bucket: sky-take-out-zby1 # bucket 名称
  region: cn-chengdu # 数据存储中心的地域 ID
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



### Result 统一结果集

```java
package org.qiaice.model.api;

import org.springframework.http.HttpStatus;
import org.springframework.util.StringUtils;

import java.util.Objects;

public record Result<T>(Integer code, String message, T data, Long timestamp) {
    private static final Object NULL = new Object();

    public static <T> Result<T> created(T data) {
        return build(HttpStatus.CREATED, null, data);
    }

    public static <T> Result<T> ok(T data) {
        return ok(null, data);
    }

    public static <T> Result<T> ok(String message, T data) {
        return build(Objects.nonNull(data) ? HttpStatus.OK : HttpStatus.NO_CONTENT, message, data);
    }

    public static <T> Result<T> bad(String message) {
        return build(HttpStatus.BAD_REQUEST, message, null);
    }

    public static <T> Result<T> build(HttpStatus status) {
        return build(status, null, null);
    }

    @SuppressWarnings(value = "unchecked")
    private static <T> Result<T> build(HttpStatus status, String message, T data) {
        message = StringUtils.hasText(message) ? message : status.getReasonPhrase();
        data = Objects.nonNull(data) ? data : (T) NULL;
        return new Result<>(status.value(), message, data, System.currentTimeMillis());
    }
}

```

### PageResult 分页结果集

```java
package org.qiaice.result;

import java.util.List;

public record PageResult<T>(Long total, List<T> records) {
}

```

### User 简单用户实体

```java
package org.qiaice.model;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import lombok.Data;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import java.util.Collection;
import java.util.List;

@Data
public class User implements UserDetails {

    @TableId(value = "uid", type = IdType.AUTO)
    private Integer userid;

    @TableField(value = "uname")
    private String username;

    @TableField(value = "passwd")
    private String password;

    private String email;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return List.of(() -> "USER");
    }
}

```

### UserService 用户服务

```java
package org.qiaice.service;

import com.baomidou.mybatisplus.extension.service.IService;
import org.qiaice.model.User;
import org.springframework.security.core.userdetails.UserDetailsService;

public interface UserService extends IService<User>, UserDetailsService {
}

```

### UserServiceImpl 用户服务实现

```java
package org.qiaice.service.impl;

import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.qiaice.mapper.UserMapper;
import org.qiaice.model.User;
import org.qiaice.service.UserService;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import java.util.Objects;

@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = lambdaQuery().eq(User::getUsername, username).one();
        if (Objects.isNull(user)) {
            throw new UsernameNotFoundException("用户名或密码有误");
        }
        return user;
    }
}

```

### UserMapper 用户仓储

```java
package org.qiaice.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Mapper;
import org.qiaice.model.User;

@Mapper
public interface UserMapper extends BaseMapper<User> {
}

```

### UnameLoginDTO 用户名登录 DTO

```java
package org.qiaice.model.dto;

import lombok.Data;

@Data
public class UnameLoginDTO {
    private String uname;
    private String passwd;
}

```

### EmailLoginDTO 邮箱登录 DTO

```java
package org.qiaice.model.dto;

import lombok.Data;

@Data
public class EmailLoginDTO {
    private String email;
    private String code;
}

```

### UserLoginVO 用户登录 VO

```java
package org.qiaice.model.vo;

import lombok.Builder;
import lombok.Getter;

@Getter
@Builder
public class UserLoginVO {
    private Integer uid;
    private String uname;
    private String jwt;
}

```

### UnamePasswdAuthenticationFilter 用户名密码认证过滤器

```java
package org.qiaice.config.login.uname;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.qiaice.model.dto.UnameLoginDTO;
import org.qiaice.util.JsonUtil;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.AuthenticationServiceException;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.util.StringUtils;

import java.io.IOException;
import java.util.stream.Collectors;

public class UnamePasswdAuthenticationFilter extends AbstractAuthenticationProcessingFilter {
    private static final String DEFAULT_FILTER_PROCESSES_URL = "/api/auth/login/uname";

    private UnamePasswdAuthenticationFilter(String defaultFilterProcessesUrl, AuthenticationManager authenticationManager) {
        super(defaultFilterProcessesUrl, authenticationManager);
    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response)
            throws AuthenticationException, IOException {
        if (!"POST".equals(request.getMethod())) {
            throw new AuthenticationServiceException("暂不支持该种认证请求方式: " + request.getMethod());
        }
        return getAuthenticationManager().authenticate(authenticationConverter(request));
    }

    private Authentication authenticationConverter(HttpServletRequest request) throws IOException {
        String requestBody = request.getReader().lines().collect(Collectors.joining(System.lineSeparator()));
        // 这里需要特别注意一下, 字段名对不上的结果是该字段为 null, 不会抛异常
        UnameLoginDTO unameLoginDTO = JsonUtil.toObj(requestBody, UnameLoginDTO.class);
        if (!checkUnameLoginDTO(unameLoginDTO)) {
            throw new BadCredentialsException("用户名和密码均为必填项, 请不要忽略任意一项");
        }
        UsernamePasswordAuthenticationToken authenticationToken = UsernamePasswordAuthenticationToken
                .unauthenticated(unameLoginDTO.getUname(), unameLoginDTO.getPasswd());
        authenticationToken.setDetails(authenticationDetailsSource.buildDetails(request));
        return authenticationToken;
    }

    private boolean checkUnameLoginDTO(UnameLoginDTO unameLoginDTO) {
        return StringUtils.hasText(unameLoginDTO.getUname()) && StringUtils.hasText(unameLoginDTO.getPasswd());
    }

    public static UnamePasswdAuthenticationFilter getInstance(
            AuthenticationManager manager, AuthenticationSuccessHandler successHandler,
            AuthenticationFailureHandler failureHandler) {
        UnamePasswdAuthenticationFilter authenticationFilter =
                new UnamePasswdAuthenticationFilter(DEFAULT_FILTER_PROCESSES_URL, manager);
        authenticationFilter.setAuthenticationSuccessHandler(successHandler);
        authenticationFilter.setAuthenticationFailureHandler(failureHandler);
        return authenticationFilter;
    }
}

```

### JwtAuthenticationFilter jwt 认证过滤器

```java
package org.qiaice.config.login.jwt;

import io.jsonwebtoken.Claims;
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.qiaice.model.User;
import org.qiaice.util.JwtUtil;
import org.springframework.security.authentication.AuthenticationDetailsSource;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContext;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.web.authentication.WebAuthenticationDetails;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.util.StringUtils;
import org.springframework.web.filter.OncePerRequestFilter;

import java.io.IOException;

public class JwtAuthenticationFilter extends OncePerRequestFilter {
    private static final String HEADER = "Authorization";
    private static final String BEARER = "Bearer ";

    private static final AuthenticationDetailsSource<HttpServletRequest, WebAuthenticationDetails> authenticationDetailsSource =
            new WebAuthenticationDetailsSource();

    private final UserDetailsService userDetailsService;

    private JwtAuthenticationFilter(UserDetailsService userDetailsService) {
        this.userDetailsService = userDetailsService;
    }

    @Override
    protected boolean shouldNotFilter(HttpServletRequest request) {
        return JwtUtil.getIgnore().contains(request.getRequestURI());
    }

    @Override
    protected void doFilterInternal(
            HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        String header = request.getHeader(HEADER);
        if (!StringUtils.hasText(header)) {
            filterChain.doFilter(request, response);
            return;
        }
        if (!header.startsWith(BEARER)) {
            throw new BadCredentialsException("无效的密钥, 请确保密钥以 'Bearer ' 作为开始");
        }
        authenticate(header.substring(BEARER.length()), request);
        filterChain.doFilter(request, response);
    }

    private void authenticate(String jwt, HttpServletRequest request) {
        try {
            Claims payload = JwtUtil.parseJwt(jwt).getPayload();
            String uname = payload.get("uname", String.class);
            User user = (User) userDetailsService.loadUserByUsername(uname);
            UsernamePasswordAuthenticationToken authentication = UsernamePasswordAuthenticationToken
                    .authenticated(user, null, user.getAuthorities());
            authentication.setDetails(authenticationDetailsSource.buildDetails(request));
            setContext(SecurityContextHolder.createEmptyContext(), authentication);
        } catch (RuntimeException e) {
            throw new BadCredentialsException(e.getLocalizedMessage());
        }
    }

    private void setContext(SecurityContext context, Authentication authentication) {
        context.setAuthentication(authentication);
        SecurityContextHolder.setContext(context);
    }

    public static JwtAuthenticationFilter getInstance(UserDetailsService userDetailsService) {
        return new JwtAuthenticationFilter(userDetailsService);
    }
}

```

### CodeProviderFilter 验证码提供过滤器

```java
package org.qiaice.config.login.email;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import lombok.extern.slf4j.Slf4j;
import org.qiaice.model.User;
import org.qiaice.model.dto.EmailLoginDTO;
import org.qiaice.util.JsonUtil;
import org.qiaice.util.MailUtil;
import org.springframework.data.redis.RedisConnectionFailureException;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.security.authentication.AuthenticationServiceException;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.util.StringUtils;

import java.io.IOException;
import java.time.Duration;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
import java.util.stream.Collectors;

@Slf4j
public class CodeProviderFilter extends AbstractAuthenticationProcessingFilter {
    private static final String DEFAULT_FILTER_PROCESSES_URL = "/api/auth/login/email/code";
    private static final List<String> NUMBERS = Arrays.asList("0", "1", "2", "3", "4", "5", "6", "7", "8", "9");
    private static final String PREFIX = "login:email:";
    private static final User user = new User();

    static {
        user.setUsername("code_provider");
    }

    private final StringRedisTemplate stringRedisTemplate;

    private CodeProviderFilter(String defaultFilterProcessesUrl, StringRedisTemplate stringRedisTemplate) {
        super(defaultFilterProcessesUrl);
        this.stringRedisTemplate = stringRedisTemplate;
    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response)
            throws AuthenticationException, IOException {
        if (!"POST".equals(request.getMethod())) {
            throw new AuthenticationServiceException("暂不支持该种认证请求方式: " + request.getMethod());
        }
        String email = emailConverter(request);
        String code = saveEmailToRedis(email);
        MailUtil.sendCode(email, code);
        return UsernamePasswordAuthenticationToken.authenticated(user, null, user.getAuthorities());
    }

    private String emailConverter(HttpServletRequest request) throws IOException {
        String requestBody = request.getReader().lines().collect(Collectors.joining(System.lineSeparator()));
        String email = JsonUtil.toObj(requestBody, EmailLoginDTO.class).getEmail();
        if (!StringUtils.hasText(email)) {
            throw new BadCredentialsException("邮箱不能为空, 请填写邮箱");
        }
        return email;
    }

    private String saveEmailToRedis(String email) {
        String code = generateCode();
        try {
            stringRedisTemplate.opsForValue().set(PREFIX + email, code, Duration.ofMinutes(15));
        } catch (RedisConnectionFailureException e) {
            log.error(e.getLocalizedMessage(), e);
            throw new AuthenticationServiceException("当前注册人数过多, 请稍后再试");
        }
        return code;
    }

    private String generateCode() {
        Collections.shuffle(NUMBERS);
        return String.join("", NUMBERS).substring(0, 6);
    }

    public static CodeProviderFilter getInstance(
            StringRedisTemplate stringRedisTemplate, AuthenticationSuccessHandler successHandler,
            AuthenticationFailureHandler failureHandler) {
        CodeProviderFilter codeProviderFilter = new CodeProviderFilter(DEFAULT_FILTER_PROCESSES_URL, stringRedisTemplate);
        codeProviderFilter.setAuthenticationSuccessHandler(successHandler);
        codeProviderFilter.setAuthenticationFailureHandler(failureHandler);
        return codeProviderFilter;
    }
}

```

### EmailAuthenticationFilter email 邮箱验证码认证过滤器

```java
package org.qiaice.config.login.email;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.qiaice.model.dto.EmailLoginDTO;
import org.qiaice.util.JsonUtil;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.AuthenticationServiceException;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.util.StringUtils;

import java.io.IOException;
import java.util.stream.Collectors;

public class EmailAuthenticationFilter extends AbstractAuthenticationProcessingFilter {
    private static final String DEFAULT_FILTER_PROCESSES_URL = "/api/auth/login/email";

    private EmailAuthenticationFilter(String defaultFilterProcessesUrl, AuthenticationManager authenticationManager) {
        super(defaultFilterProcessesUrl, authenticationManager);
    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response)
            throws AuthenticationException, IOException {
        if (!"POST".equals(request.getMethod())) {
            throw new AuthenticationServiceException("暂不支持该种认证请求方式: " + request.getMethod());
        }
        return getAuthenticationManager().authenticate(authenticationConverter(request));
    }

    private Authentication authenticationConverter(HttpServletRequest request) throws IOException {
        String requestBody = request.getReader().lines().collect(Collectors.joining(System.lineSeparator()));
        // 这里需要特别注意一下, 字段名对不上的结果是该字段为 null, 不会抛异常
        EmailLoginDTO emailLoginDTO = JsonUtil.toObj(requestBody, EmailLoginDTO.class);
        if (!checkEmailLoginDTO(emailLoginDTO)) {
            throw new BadCredentialsException("邮箱和验证码均为必填项, 请不要忽略任意一项");
        }
        EmailAuthenticationToken authenticationToken = EmailAuthenticationToken
                .unauthenticated(emailLoginDTO.getEmail(), emailLoginDTO.getCode());
        authenticationToken.setDetails(authenticationDetailsSource.buildDetails(request));
        return authenticationToken;
    }

    private boolean checkEmailLoginDTO(EmailLoginDTO emailLoginDTO) {
        return StringUtils.hasText(emailLoginDTO.getEmail()) && StringUtils.hasText(emailLoginDTO.getCode());
    }

    public static EmailAuthenticationFilter getInstance(
            AuthenticationManager manager, AuthenticationSuccessHandler successHandler,
            AuthenticationFailureHandler failureHandler) {
        EmailAuthenticationFilter authenticationFilter =
                new EmailAuthenticationFilter(DEFAULT_FILTER_PROCESSES_URL, manager);
        authenticationFilter.setAuthenticationSuccessHandler(successHandler);
        authenticationFilter.setAuthenticationFailureHandler(failureHandler);
        return authenticationFilter;
    }
}

```

### EmailAuthenticationToken email 认证密钥

```java
package org.qiaice.config.login.email;

import org.springframework.security.authentication.AbstractAuthenticationToken;
import org.springframework.security.core.GrantedAuthority;

import java.util.Collection;

public class EmailAuthenticationToken extends AbstractAuthenticationToken {
    private final Object principal;
    private Object credentials;

    private EmailAuthenticationToken(Object principal, Object credentials) {
        super(null);
        this.principal = principal;
        this.credentials = credentials;
        super.setAuthenticated(false);
    }

    private EmailAuthenticationToken(
            Object principal, Object credentials, Collection<? extends GrantedAuthority> authorities) {
        super(authorities);
        this.principal = principal;
        this.credentials = credentials;
        super.setAuthenticated(true);
    }

    public static EmailAuthenticationToken unauthenticated(Object principal, Object credentials) {
        return new EmailAuthenticationToken(principal, credentials);
    }

    public static EmailAuthenticationToken authenticated(
            Object principal, Object credentials, Collection<? extends GrantedAuthority> authorities) {
        return new EmailAuthenticationToken(principal, credentials, authorities);
    }

    @Override
    public Object getPrincipal() {
        return principal;
    }

    @Override
    public Object getCredentials() {
        return credentials;
    }

    @Override
    public void eraseCredentials() {
        super.eraseCredentials();
        this.credentials = null;
    }
}

```

### EmailAuthenticationProvider email 认证提供者

```java
package org.qiaice.config.login.email;

import org.qiaice.model.User;
import org.qiaice.service.UserService;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;

import java.util.Objects;

public class EmailAuthenticationProvider implements AuthenticationProvider {
    private static final String PREFIX = "login:email:";

    private final UserDetailsService userDetailsService;
    private final StringRedisTemplate stringRedisTemplate;

    public EmailAuthenticationProvider(UserDetailsService userDetailsService, StringRedisTemplate stringRedisTemplate) {
        this.userDetailsService = userDetailsService;
        this.stringRedisTemplate = stringRedisTemplate;
    }

    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        UserService userService = (UserService) userDetailsService;
        User user = userService.lambdaQuery().eq(User::getEmail, authentication.getPrincipal()).one();
        if (Objects.isNull(user)) {
            throw new UsernameNotFoundException("邮箱或验证码有误");
        }
        Object credentials = authentication.getCredentials();
        String key = PREFIX + user.getEmail();
        if (!credentials.equals(stringRedisTemplate.opsForValue().get(key))) {
            throw new BadCredentialsException("邮箱或验证码有误");
        }
        return EmailAuthenticationToken.authenticated(user, user.getPassword(), user.getAuthorities());
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return EmailAuthenticationToken.class.isAssignableFrom(authentication);
    }
}

```

### SpringSecurity 配置

```java
package org.qiaice.config;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.qiaice.config.login.email.CodeProviderFilter;
import org.qiaice.config.login.email.EmailAuthenticationFilter;
import org.qiaice.config.login.email.EmailAuthenticationProvider;
import org.qiaice.config.login.jwt.JwtAuthenticationFilter;
import org.qiaice.config.login.uname.UnamePasswdAuthenticationFilter;
import org.qiaice.model.User;
import org.qiaice.model.vo.UserLoginVO;
import org.qiaice.result.Result;
import org.qiaice.util.JsonUtil;
import org.qiaice.util.JwtUtil;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.security.access.AccessDeniedException;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.ProviderManager;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.access.ExceptionTranslationFilter;

import java.util.List;

@Configuration
@EnableMethodSecurity
public class SecurityConfig {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(BCryptPasswordEncoder.BCryptVersion.$2B);
    }

    @Bean
    public AuthenticationManager authenticationManager(
            UserDetailsService userDetailsService, PasswordEncoder passwordEncoder,
            StringRedisTemplate stringRedisTemplate) {
        DaoAuthenticationProvider daoAuthenticationProvider = new DaoAuthenticationProvider(userDetailsService);
        daoAuthenticationProvider.setPasswordEncoder(passwordEncoder);
        EmailAuthenticationProvider emailAuthenticationProvider =
                new EmailAuthenticationProvider(userDetailsService, stringRedisTemplate);
        return new ProviderManager(List.of(daoAuthenticationProvider, emailAuthenticationProvider));
    }

    @Bean
    public SecurityFilterChain securityFilterChain(
            HttpSecurity httpSecurity, AuthenticationManager authenticationManager,
            UserDetailsService userDetailsService, StringRedisTemplate stringRedisTemplate) throws Exception {
        return httpSecurity

                // 定义授权规则
                .authorizeHttpRequests(registry ->
                        registry.anyRequest().authenticated())

                // 关闭 csrf 防护, 纯 api 场景不需要
                .csrf(AbstractHttpConfigurer::disable)

                // 关闭所有页面/状态相关的认证方式
                .httpBasic(AbstractHttpConfigurer::disable)
                .formLogin(AbstractHttpConfigurer::disable)
                .rememberMe(AbstractHttpConfigurer::disable)
                .logout(AbstractHttpConfigurer::disable)

                // 关闭请求重定向缓存
                .requestCache(AbstractHttpConfigurer::disable)

                // 无状态 session, 但保留会话安全策略
                .sessionManagement(config ->
                        config.sessionCreationPolicy(SessionCreationPolicy.STATELESS))

                // 关闭用户匿名身份发放
                .anonymous(AbstractHttpConfigurer::disable)

                // 定义接口异常访问应对方案
                .exceptionHandling(conf -> conf
                        .authenticationEntryPoint(this::authenticationEntryPoint) // 请求未认证
                        .accessDeniedHandler(this::accessDeniedHandler)) // 请求权限不足

                // 添加自定义 JwtAuthenticationFilter
                .addFilterAfter(JwtAuthenticationFilter.getInstance(userDetailsService),
                        ExceptionTranslationFilter.class)

                // 添加自定义 UnamePasswdAuthenticationFilter
                .addFilterAfter(UnamePasswdAuthenticationFilter.getInstance(authenticationManager,
                                this::authenticationSuccessHandler, this::authenticationFailureHandler),
                        ExceptionTranslationFilter.class)

                // 添加自定义 CodeProviderFilter
                .addFilterAfter(CodeProviderFilter.getInstance(stringRedisTemplate,
                        this::sendCodeSuccessHandler, this::sendCodeFailureHandler),
                        ExceptionTranslationFilter.class)

                // 添加自定义 EmailAuthenticationFilter
                .addFilterAfter(EmailAuthenticationFilter.getInstance(authenticationManager,
                        this::authenticationSuccessHandler, this::authenticationFailureHandler),
                        ExceptionTranslationFilter.class)

                // 构建过滤器链
                .build();
    }

    // 发送验证码成功处理器
    private void sendCodeSuccessHandler(
            HttpServletRequest request, HttpServletResponse response, Authentication authentication) {
        JsonUtil.write(response, Result.ok("验证码已发送, 请注意查收"));
    }

    // 发送验证码失败处理器
    private void sendCodeFailureHandler(
            HttpServletRequest request, HttpServletResponse response, AuthenticationException exception) {
        authenticationFailureHandler(request, response, exception);
    }

    // 认证成功处理器
    private void authenticationSuccessHandler(
            HttpServletRequest request, HttpServletResponse response, Authentication authentication) {
        User user = (User) authentication.getPrincipal();
        JsonUtil.write(response, Result.ok(UserLoginVO.builder()
                .uid(user.getUserid())
                .uname(user.getUsername())
                .jwt(JwtUtil.createJwt(user))
                .build()));
    }

    // 认证失败处理器
    private void authenticationFailureHandler(
            HttpServletRequest request, HttpServletResponse response, AuthenticationException exception) {
        JsonUtil.write(response, Result.no(exception.getLocalizedMessage()));
    }

    // 请求未认证处理器
    private void authenticationEntryPoint(
            HttpServletRequest request, HttpServletResponse response, AuthenticationException exception) {
        JsonUtil.write(response, Result.unauthenticated(exception.getLocalizedMessage()));
    }

    // 请求权限不足处理器
    private void accessDeniedHandler(
            HttpServletRequest request, HttpServletResponse response, AccessDeniedException exception) {
        JsonUtil.write(response, Result.forbidden(exception.getLocalizedMessage()));
    }
}

```

### MybatisPlus 配置

```java
package org.qiaice.config;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {

        // 初始化核心插件
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 初始化分页插件
        // 如果有多数据源可以不配置具体类型，否则都建议配上具体的 DbType
        PaginationInnerInterceptor paginationInnerInterceptor = new PaginationInnerInterceptor(DbType.MYSQL);

        // 设置分页上限
        paginationInnerInterceptor.setMaxLimit(1000L);

        // 添加分页插件
        // 如果配置多个插件，切记分页插件最后添加
        interceptor.addInnerInterceptor(paginationInnerInterceptor);
        
        return interceptor;
    }
}

```

### Redis 配置

```java
package org.qiaice.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.RedisSerializer;

@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<Object, Object> jsonRedisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<Object, Object> jsonRedisTemplate = new RedisTemplate<>();
        jsonRedisTemplate.setConnectionFactory(redisConnectionFactory);
        // string 序列化 key
        jsonRedisTemplate.setKeySerializer(RedisSerializer.string());
        jsonRedisTemplate.setHashKeySerializer(RedisSerializer.string());
        // json 序列化 value
        jsonRedisTemplate.setValueSerializer(RedisSerializer.json());
        jsonRedisTemplate.setHashValueSerializer(RedisSerializer.json());
        return jsonRedisTemplate;
    }
}

```

### JsonUtil json 工具类

```java
package org.qiaice.util;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import tools.jackson.databind.ObjectMapper;

@Component
public class JsonUtil {
    private static ObjectMapper objectMapper;

    private JsonUtil() {
    }

    @Autowired
    public void setObjectMapper(ObjectMapper objectMapper) {
        JsonUtil.objectMapper = objectMapper;
    }

    public static <T> String toJson(T data) {
        return objectMapper.writeValueAsString(data);
    }

    public static <T> T toObj(String json, Class<T> clazz) {
        return objectMapper.readValue(json, clazz);
    }
}

```

### JwtProperty jwt 属性类

```java
package org.qiaice.property;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;

import java.util.Set;

@Data
@ConfigurationProperties(value = "spring.security.jwt")
public class JwtProperty {
    private String secret;
    private String issuer;
    private String expiration;
    private Set<String> ignore;
}

```

### JwtUtil jwt 工具类

```java
package org.qiaice.util;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jws;
import io.jsonwebtoken.JwtException;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.security.Keys;
import org.qiaice.model.User;
import org.qiaice.properties.JwtProperties;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import javax.crypto.SecretKey;
import java.util.*;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

@Component
public class JwtUtil {
    private static JwtProperties jwtProperties;
    private static SecretKey secretKey;

    private static final Map<String, String> DEFAULT_HEADERS = Map.of("typ", "JWT");
    private static final Pattern EXPIRATION_PATTERN = Pattern.compile("(\\d+)(ms|s|m|h|d|w|M)?");

    @Autowired
    public void setJwtProperties(JwtProperties jwtProperties) {
        JwtUtil.jwtProperties = jwtProperties;
        JwtUtil.secretKey = Keys.hmacShaKeyFor(Base64.getDecoder().decode(jwtProperties.getSecret()));
    }

    private JwtUtil() {
    }

    /**
     * 使用用户信息创建 jwt
     *
     * @param user 用户信息
     * @return jwt
     */
    public static String createJwt(User user) {
        return createJwt(Map.of("uid", user.getUserid(), "uname", user.getUsername()));
    }

    /**
     * 使用默认头部和自定义负载创建 jwt
     *
     * @param payload 自定义负载
     * @return jwt
     */
    public static String createJwt(Map<String, ?> payload) {
        return createJwt(DEFAULT_HEADERS, payload);
    }

    /**
     * 使用自定义头部和自定义负载创建 jwt
     *
     * @param header  自定义头部
     * @param payload 自定义负载
     * @return jwt
     */
    public static String createJwt(Map<String, ?> header, Map<String, ?> payload) {
        return Jwts.builder()
                .header()
                .add(header) // 添加头部
                .and()
                .claims(payload) // 添加负载
                .id(UUID.randomUUID().toString()) // 这里的键是 jti 不是 id
                .issuer(jwtProperties.getIssuer())
                .issuedAt(new Date(System.currentTimeMillis()))
                .expiration(convert2Date(jwtProperties.getExpiration()))
                .signWith(secretKey)
                .compact();
    }

    /**
     * 解析 String 格式的时间，并以 Date 格式返回
     *
     * @param exp String 格式的过期时间
     * @return Date 格式的过期时间
     */
    private static Date convert2Date(String exp) {
        Matcher matcher = EXPIRATION_PATTERN.matcher(exp);
        if (!matcher.matches())
            throw new RuntimeException("暂不支持该种时间单位: " + exp);
        Calendar calendar = Calendar.getInstance();
        calendar.add(switch (matcher.group(2)) {
            case "s" -> Calendar.SECOND;
            case "m" -> Calendar.MINUTE;
            case "h" -> Calendar.HOUR;
            case "d" -> Calendar.DAY_OF_YEAR;
            case "w" -> Calendar.WEEK_OF_YEAR;
            case "M" -> Calendar.MONTH;
            case null, default -> Calendar.MILLISECOND;
        }, Integer.parseInt(matcher.group(1)));
        return calendar.getTime();
    }

    /**
     * 解析 jwt 并返回获得的数据
     *
     * @param jwt jwt
     * @return 解析 jwt 获得的数据
     */
    public static Jws<Claims> parseJwt(String jwt) {
        try {
            return Jwts.parser()
                    .verifyWith(secretKey)
                    .build()
                    .parseSignedClaims(jwt);
        } catch (JwtException e) {
            throw new RuntimeException(e.getLocalizedMessage());
        }
    }

    /**
     * 提供需要忽略 jwt 校验的 url
     *
     * @return 需要忽略 jwt 校验的 url
     */
    public static Set<String> getIgnore() {
        return jwtProperties.getIgnore();
    }
}

```

### MailSendProperty mail 属性类

```java
package org.qiaice.property;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;

@Data
@ConfigurationProperties(value = "spring.mail.send")
public class MailSenderProperty {
    private String from;
    private String subject;
    private String template;
}

```

### MailUtil mail 工具类

```java
package org.qiaice.util;

import jakarta.mail.MessagingException;
import jakarta.mail.internet.MimeMessage;
import org.qiaice.property.MailSenderProperty;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;
import org.springframework.stereotype.Component;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
import java.util.Optional;
import java.util.stream.Collectors;

@Component
public class MailUtil {
    private static final List<String> collection = Arrays.asList("0", "1", "2", "3", "4", "5", "6", "7", "8", "9");
    private static final StringBuilder builder = new StringBuilder();

    private static JavaMailSender javaMailSender;
    private static MailSenderProperty mailProperty;

    @Autowired
    private void setJavaMailSender(JavaMailSender javaMailSender) {
        MailUtil.javaMailSender = javaMailSender;
    }

    @Autowired
    private void setMailSenderProperty(MailSenderProperty mailProperty) {
        MailUtil.mailProperty = mailProperty;
        Optional.ofNullable(MailUtil.class.getClassLoader().getResourceAsStream(mailProperty.getTemplate()))
                .ifPresentOrElse(inputStream -> {
                    try (BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream))) {
                        builder.append(reader.lines().collect(Collectors.joining(System.lineSeparator())));
                    } catch (IOException e) {
                        throw new RuntimeException(e.getLocalizedMessage());
                    }
                }, () -> builder.append("您的验证码为 {{code}}"));
    }

    public static void sendCode(String email) {
        Collections.shuffle(collection);
        String text = builder.toString()
                .replaceAll("\\{\\{code}}", String.join("", collection).substring(0, 6))
                .replaceAll("\\{\\{from}}", mailProperty.getFrom())
                .replaceAll("\\{\\{email}}", email);
        try {
            MimeMessage message = javaMailSender.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(message, true);
            helper.setFrom(mailProperty.getFrom());
            helper.setSubject(mailProperty.getSubject());
            helper.setText(text, true);
            helper.setTo(email);
            javaMailSender.send(message);
        } catch (MessagingException e) {
            throw new RuntimeException(e.getLocalizedMessage());
        }
    }
}

```

































application-dev.yaml

```yaml
# spring 相关配置
spring:
  config:
    import:
      - optional:nacos:${spring.application.name}-dev.yml?refreshEnabled=true
  
    nacos: # nacos 相关配置
      server-addr: localhost:8848
      username: nacos
      password: nacos
      discovery:
        ephemeral: true # 临时实例, 关掉服务 -> 实例释放
        cluster-name: DEFAULT # 默认集群
        weight: 1 # 默认权重
    loadbalancer:
      nacos: # 启用负载均衡的 nacos 支持, 同一集群的服务优先
        enabled: true
  
```
