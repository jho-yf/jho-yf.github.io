# Swagger2



## Swagger2概述

### Open API

Open API规范（Open API Specification），Swagger规范，是REST API的API描述格式。Open API规范（OAS）为REST API定义了一个与语言无关的标准接口，允许人和计算机发现和理解服务的功能，而无需访问源代码，文档或通过网络流量检查。

Open API文件允许描述整个API，包括：

- 访问地址
- 访问地址的类型：GET、POST、DELETE、PUT、PATCH
- 操作参数：请求参数、响应参数
- 认证方法
- 连接信息，声明，使用团队和其他信息

Open API规范可以使用`YAML`或`JSON`格式编写，以便人和机器阅读。

### Swagger简介

Swagger是一套围绕Open API规范构建的开源工具，可以帮助设计，构建，记录和使用REST API。

Swagger工具包括的组件：

- Swagger Editor：基于浏览器编辑器，可以在里面编写Open API规范。
- Swagger UI：将Open API规范呈现为交互式API文档。用可视化UI展示描述文件。
- Swagger Codegen：将OpenAPI规范生成为服务器存根和客户端库。通过Swagger Codegen可以将描述文件生成HTML格式和CWIKI形式的接口文档，同事也可以生成多种语言的客户端和服务端代码。
- Swagger Inspector：和Swagger UI有点类似，但是可以返回更多信息，也会保存请求的实际参数数据。
- Swagger Hub：集成了上面所有项目的各个中年，可以以项目和版本为单位，将描述文件上次到Swagger Hub中

## SpringFox

SpringFox是根据代码生成接口文档，正常的进行更新项目版本，修改代码即可，而不需要跟随修改描述文件。SpringFox利用自身AOP特性，把Swagger集成进来。

> 官网：http://springfox.github.io/springfox/



## Swagger2极致用法

导入maven依赖

```xml
<!-- springfox -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>
```

编写Controller

```java
@RestController
@RequestMapping("/person")
public class PersonController {

    @PostMapping
    public Person add(Person person) {
        return person;
    }

    @GetMapping
    public Person get(@RequestParam Integer id) {
        return new Person(id);
    }

    @RequestMapping("/req")
    public String req(String m) {
        return "【req】" + m;
    }

}
```

开启Swagger2

`@EnableSwagger2`：是SpringFox提供的一个注解，代表Swagger2相关技术开启，会扫描当前类所在包以及子包所有的类型中的注解。

```java
@SpringBootApplication
@EnableSwagger2
public class SwaggerUsingApp {
    
    public static void main(String[] args) {
        SpringApplication.run(SwaggerUsingApp.class, args);
    }

}
```

访问swagger-ui:http://localhost:8080/swagger-ui/#/



## Swagger2配置

```java
@Configuration
public class SwaggerConfiguration {

    /**
     * Docket是swagger的全局配置对象
     * @return {@link Docket}
     */
    @Bean
    public Docket docket() {
        Docket docket = new Docket(DocumentationType.SWAGGER_2);
        // name     文档发布者名称
        // url      文档发布者网站地址。企业网站
        // email    文档发布者的电子邮箱
        Contact contact = new Contact("乙方小弟",
                "https://github.com/jho-yf",
                "xu-jihong@qq.com");
        ApiInfo apiInfo = new ApiInfoBuilder()
                .contact(contact)
                .title("Swagger学习文档")
                .description("Swagger框架帮助文档详细描述")
                .version("1.1")
                .build();

        // 添加api描述信息
        docket.apiInfo(apiInfo);

        // 获取Docket中的选择器
        docket = docket.select()
                // 当方法上有注解时返回false
                .apis(RequestHandlerSelectors.withMethodAnnotation(MyAnnotation4Swagger.class).negate())
                // 设定扫描哪个包中的注解
                .apis(RequestHandlerSelectors.basePackage("cn.jho.swagger.controller"))
                // 使用正则表达式，约束生成API文档的路径地址
                .paths(PathSelectors.regex("/person/*").or(PathSelectors.regex("/*")))
                .build();

        return docket;
    }

}
```
