# Swagger-core

## 1.setup

集成swagger到项目中需要经过以下几步操作：

- 添加Swagger的依赖到项目中
- Hook Swagger into your JAX-RS application configuration
- Configure Swagger



完成这些配置，就可以在项目中使用注解。



### 1.1 Adding Swagger's dependencies to your Project

Swagger 使用maven构建和部署，所以在maven central 中存在swagger的构件，你可以在任何支持maven依赖的依赖管理系统中使用maven依赖，如:Maven,Lvy,Gradle...

在项目中使用下面的maven的依赖

```xml
<dependency>
  <groupId>io.swagger</groupId>
  <artifactId>swagger-jaxrs2</artifactId>
  <version>2.0.0-rc1</version>
</dependency>
```



### 1.2Hooking up Swagger-Core in your application(Jersey2.x)

Swagger-core 暴露一个JAX-RS资源或Servlet 用于生成API 文档和提供api文档服务。



