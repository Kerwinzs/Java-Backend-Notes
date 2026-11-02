## JAVA后端学习笔记

**目录**

[toc]



## MAVEN

**Maven 就是帮助我们管理这些依赖，并自动完成项目构建的工具。**

Maven 解决的问题

> 1. 依赖管理（Dependency Management）
>
> 声明项目需要哪些第三方库，Maven 可以下载这些库，并处理它们的传递依赖。
>
> 2. 项目构建（Build）
>
> 自动执行编译、测试、打包等步骤，不必每次手动操作。
>
> 3. 统一项目结构
>
> 让团队成员遵循相对统一的目录结构和构建方式，方便协作与阅读开源项目。

### pom.xml

`pom.xml` 是 Maven 的项目说明书，告诉 Maven 这个项目叫什么、使用什么配置、需要哪些第三方库。

```xml
<groupId>com.example</groupId>
<artifactId>mall</artifactId>
<version>1.0.0</version>
```

| 字段         | 含义             | 示例          |
| ------------ | ---------------- | ------------- |
| `groupId`    | 组织或开发方标识 | `com.example` |
| `artifactId` | 项目或模块名称   | `mall`        |
| `version`    | 版本号           | `1.0.0`       |

#### properties

统一管理配置 指定 Java 编译目标为 Java 17

```xml
<properties>
    <maven.compiler.release>17</maven.compiler.release>
</properties>
```

#### dependencies

**声明依赖，最重要的部分**

```xml
<dependencies>
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <version>8.4.0</version>
    </dependency>
</dependencies>
```

这里声明了一个 MySQL JDBC 驱动依赖。示例版本不代表所有项目都应该使用这个版本。

注意：`dependencies` 里面可以包含多个 `dependency`，每一个都代表一个依赖声明。

#### Scope

Maven 提供了 `scope` 来控制依赖在哪些阶段可用。

| scope      | 典型用途                                   |
| ---------- | ------------------------------------------ |
| `compile`  | 默认值，编译主代码、测试代码时都可用       |
| `provided` | 编译时需要，但运行时通常由外部环境提供     |
| `runtime`  | 运行时需要，但通常不需要直接用于主代码编译 |
| `test`     | 仅测试编译和测试运行时使用                 |

```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.11.0</version>
    <scope>test</scope>
</dependency>
```



---

### 依赖冲突

Maven 有一个核心规则，叫作 **Nearest Definition**（最近定义原则）。

简单来说：依赖路径更短的版本优先；如果路径长度相同，通常优先选择在依赖声明中先出现的版本。



### Maven 生命周期

Java 项目需要编译、测试和打包。Maven 把这些工作组织成一套有顺序的流程，叫作 Build Lifecycle（构建生命周期）

> 1. mvn validate（验证）
>
> 检查项目结构和必要配置。
>
> 2. mvn compile（编译）
>
> 编译主代码，生成字节码。
>
> 3. mvn test（测试）
>
> 编译并运行单元测试。
>
> 4. mvn package（打包）
>
> 将编译结果打包成 JAR 或 WAR 等构件。
>
> 5. mvn install（安装到本地仓库）
>
> 将构建好的构件放入本机 Maven 仓库，供其他本地项目使用。

### **JAR**（Java Archive） 

打包格式

Java 归档文件。可以包含编译后的 `.class` 文件、资源文件以及元数据。

常用于 Java 类库和应用程序。配置合适时，可以通过 `java -jar app.jar` 启动应用。

### Maven Wrapper

Maven Wrapper（Maven 包装器） 是一个让项目能够使用指定版本 Maven 的工具。

```
gs-rest-service/
├── mvnw
├── mvnw.cmd
├── .mvn/
│   └── wrapper/
│       └── maven-wrapper.properties
└── pom.xml
```

其中：

- `mvnw`：macOS / Linux 使用的脚本。
- `mvnw.cmd`：Windows 使用的脚本。
- `maven-wrapper.properties`：配置 Maven Wrapper 使用的 Maven 发行版等信息。
- `pom.xml`：项目的 Maven 构建配置。

`````cmd
./mvnw clean package
`````

使用命令，构建软件

## REST

**REST**（Representational State Transfer），中文通常翻译为表述性状态转移

REST是一种基于Web的架构设计风格

**REST 的核心思想之一是：把商品、用户、订单等业务对象当成资源（Resource），通过 URL 定位资源，通过 HTTP 方法表达操作。**

```java
@RestController
public class ProductController {

    @GetMapping("/products/1")
    public Product getProduct() {
        return new Product(1, "苹果", 5);
    }
}

RESTful
GET /products/1
URL
GET /getProductById?id=1
```

