---
title: mybatis-plus的配置与使用
date: 2025-11-14 21:20:34
tags:
  - [framework]
---

### 引入依赖

在 `pom.xml` 文件中，加入 `mybatis-plus-boot-starter` 的依赖。`mybatis-plus-boot-starter` 是一个“全家桶”，它会自动帮你引入 Mybatis-Plus 核心库、MyBatis 核心库以及与 Spring Boot 集成的所有必要组件。

```xml
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.4.3</version> <!-- 请根据需要选择合适的版本 -->
    </dependency>

    <!-- 你还需要数据库驱动，比如 MySQL -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
```

### 配置数据源

在 `src/main/resources/application.yaml` 中，配置数据库的连接信息。

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/my_database?serverTimezone=UTC
    username: root
    password: your_password
```

### 创建绑定

使用 @TableName 创建实体类 Entity 和数据库表的绑定；使用 @TableId、@TableField 来创建属性和字段的绑定。

```java
    @Data
    @TableName("tb_user") // 将这个类映射到数据库的 "tb_user" 表
    public class User {
        @TableId(value = "id", type = IdType.AUTO) // 声明主键，并设置为自增
        private Long id;

        private String name;
        private Integer age;
        private String email;
    }
```

让 Mapper 接口继承 `BaseMapper<T>`。通过继承 `BaseMapper<User>`，`UserMapper` 接口就拥有了一整套强大的、无需编写 SQL 的 CRUD 方法，例如 `insert`, `selectById`, `updateById`, `delete`, `selectList` 等。

```java
    // 传入你的实体类 User
    public interface UserMapper extends BaseMapper<User> {
        // 通常这里是空的，因为 BaseMapper 已经提供了所有常用方法
        // 如果有特别复杂的 SQL，可以在这里自定义方法，并配合 XML 文件实现
    }
```

`UserMapper` 还是一个接口，没有实现类，所以最后还需在 Spring Boot 主启动类上，添加 `@MapperScan` 注解。`@MapperScan` 注解会告诉 Spring Boot 在启动时去扫描指定的包，为所有继承了 `BaseMapper` 的接口动态地创建一个**代理实现类**，并注册到 Spring 容器中。这样，你才能在其他地方通过 `@Autowired` 或 `@Resource` 注入 `UserMapper`。

```java
    @SpringBootApplication
    @MapperScan("com.example.project.mapper") // 告诉 Spring 去哪里找你的 Mapper 接口
    public class ProjectApplication {
        public static void main(String[] args) {
            SpringApplication.run(ProjectApplication.class, args);
        }
    }
```

### 进阶：Service 层

让 Service 层的接口继承 `IService<T>`接口，Service 的实现类继承 `ServiceImpl<M, T>`。

```java
    // Service 接口还要继承一个接口
    public interface IUserService extends IService<User> {
    }

    // Service 实现类要实现它接口中定义的所有抽象方法，这通过继承 ServiceImpl 而实现
    @Service
    public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements IUserService {
        // 这里可以编写你的业务逻辑
        // 你可以直接使用 ServiceImpl 提供的方法，如 getById(), save(), list()，也可以使用 query(), update() 来进行链式操作
    }
```

### 进阶：插件

创建 Mybatis-Plus 的配置类，用来添加分页插件、乐观锁插件等。

```java
    @Configuration
    public class MybatisPlusConfig {

        @Bean
        public MybatisPlusInterceptor mybatisPlusInterceptor() {
            MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

            // 1. 添加分页插件 (最常用)
            interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));

            // 2. 添加乐观锁插件 (如果需要)
            // interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());

            return interceptor;
        }
    }
```
