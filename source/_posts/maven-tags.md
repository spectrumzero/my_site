---
title: maven常见标签
date: 2025-10-09 17:46:59
tags:
  - [maven]
---

## 基础坐标

- `<groupId>`: 项目组 ID。通常是公司或组织的反向域名。groupId 一般分为多段，通常情况下，第一段为域，第二段为公司名称，第三段为项目名称。
- `<artifactId>`: 项目 ID。它是在 groupId 内唯一的项目名称。
- `<version>`: 项目版本号。例如 0.0.1-SNAPSHOT。
  - SNAPSHOT 后缀表示这是一个不稳定的开发版本。
- `<packaging>`: 项目的打包方式。
  - jar (默认): 打包成 Java Archive 文件。
  - war: 打包成 Web Application Archive 文件，用于部署到 Web 服务器。
  - pom: 用于**父模块**或聚合模块，表示**它只管理其他模块，本身不产生构建产物**。

## 依赖管理

- `<dependencies>`: 根标签，包含所有具体的依赖项。
- `<dependency>`: 定义一个具体的依赖项。
  - `<groupId>`, `<artifactId>`, `<version>`: 依赖项自身的 GAV 坐标。
  - `<type>`: 用来指定依赖项的类型。默认为 jar，也有可能为 pom——在这种情况下**常常与`<scope>import</scope>`一起使用**，以实现 BOM ( Bill of Materials ) 机制，导入依赖版本配置。
  - `<scope>`: 依赖的范围，非常重要，决定了**依赖在何时生效**。
    - compile (默认): 编译、测试、运行时都需要。会被打包到最终产物中。
    - provided: 编译和测试时需要，但运行时由环境（如 JDK 或 Web 容器）提供。例如 servlet-api。
    - runtime: 编译时不需要，但运行时需要。例如 JDBC 驱动。
    - test: 只在测试编译和运行时需要。例如 JUnit。
    - import: 特殊，只在 `<dependencyManagement>` 中使用，用于从其他 POM 文件（“物料清单”）中导入依赖版本配置。
- `<dependencyManagement>`: 依赖管理声明。**它不直接引入依赖，而是用于统一管理所有子模块中依赖的版本号**。在**父 POM**中使用此标签声明版本后，子模块在引入该依赖时就无需再写 `<version>`，从而保证了所有模块版本一致，避免了版本冲突。
- **注意**： 定义在父 POM 的 `<dependencies>` 里的依赖，会被所有子模块自动继承**并引入**；而定义在 `<dependencyManagement>` 里的依赖，**仅仅是声明版本**，如果子模块不通过自己的 `<dependencies>` 标签去引入它，那么这个依赖是不会被下载的。

## 项目关系

- `<parent>`: 在子模块中声明其父项目。子模块会继承父项目 pom.xml 中的配置（如依赖版本、插件配置等）。
- `<modules>`: 在父模块（<packaging>pom</packaging>）中声明其包含的所有子模块。

## 构建配置

- `<build>`: 构建配置的根标签。
- `<plugins>`: 包含所有构建过程中使用的插件。
- `<plugin>`: 定义一个具体的插件，例如 maven-compiler-plugin (用于编译代码) 或 spotless-maven-plugin (用于格式化代码)。
- `<resources>`: 定义需要被包含到最终产物中的资源文件（非 .java 文件，如 .xml, .properties）。默认目录是 src/main/resources。

## 属性

- `<properties>`: 属性的根标签。用于定义可复用的变量，使配置更灵活、易于维护。比如可以在这里定义版本号、配置参数等，然后在**该 POM 的其他地方**通过 ${...} 的形式引用。

## 其他信息

- `<name>`: 项目的名称，更具可读性。
- `<description>`: 项目的详细描述。
- `<developers>`: 项目开发者信息。

## 参考

- [Java Guide](https://javaguide.cn/).
