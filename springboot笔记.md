## springboot

## 1 简介

parent

1. 开发SpringBoot程序要继承spring-boot-starter-parent
2. spring-boot-starter-parent中定义了若干个**依赖管理**
3. 继承parent模块可以避免多个依赖使用相同技术时出现依赖版本冲突
4. 继承parent的形式也可以采用引入依赖的形式实现效果

starter

1. 开发SpringBoot程序需要导入坐标时通常导入对应的starter
2. 每个不同的starter根据功能不同，通常包含多个**依赖坐标**
3. 使用starter可以实现快速配置的效果，达到简化配置的目的

> GAV只用写GA
> ```xml
> <groupId>：表示哪一个组织/企业提供的
> <artifactId>：表示这个组织/企业提供的哪一个模块
> <version>
> ```

引导类

```java
@SpringBootApplication
public class Springboot0101QuickstartApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot0101QuickstartApplication.class, args);
    }
}
```

1. SpringBoot工程提供引导类用来启动程序
2. SpringBoot工程启动后创建并初始化Spring容器

内嵌tomcat

1. 内嵌Tomcat服务器是SpringBoot辅助功能之一
2. 内嵌Tomcat工作原理是将Tomcat服务器作为对象运行，并将该对象交给Spring容器管理
3. 变更内嵌服务器思想是去除现有服务器，添加全新的服务器

## 2 REST

### 2.1 REST简介

- **REST** (Representational State Transfer) ，表现形式状态转换
  - 传统风格资源描述形式
    - `http://localhost/user/getById?id=1`
    - `http://localhost/user/saveUser`
  - REST风格描述形式
    - `http://localhost/user/1`
    - `http://localhost/user`
- 优点:
  - 隐藏资源的访问行为，无法通过地址得知对资源是何种操作
  - 书写简化

### 2.2 REST风格简介

- 按照REST风格访问资源时使用**行为动作**区分对资源进行了何种操作
  - `http://localhost/users`      查询全部用户信息   GET (查询)
  - `http://localhost/users/1`    查询指定用户信息  GET (查询)
  - `http://localhost/users`      添加用户信息      POST (新增/保存)
  - `http://localhost/users`      修改用户信息      PUT (修改/更新)
  - `http://localhost/users/1`    删除用户信息      DELETE (删除)
- 根据REST风格对资源进行访问称为RESTful

> **注意事项**
>
> 上述行为是约定方式，约定不是规范，可以打破，所以称REST风格，而不是REST规范
>
> 描述模块的名称通常使用复数，也就是加s的格式描述，表示此类资源，而非单个资源，
> 例如：users、books、accounts......

### @RequestParam @RequestBody @PathVariable

- **区别**
  - `@RequestParam`用于接收url地址传参或表单传参
  - `@RequestBody`用于接收json数据
  - `@PathVariable`用于接收路径参数，使用{参数名称}描述路径参数
- **应用**
  - 后期开发中，发送请求参数超过1个时，以json格式为主，`@RequestBody`应用较广
  - 如果发送非json格式数据，选用`@RequestParam`接收请求参数
  - 采用RESTful进行开发，当参数数量较少时，例如1个，可以采用`@PathVariable`接收请求路径变量，通常用于传递id值

> @RequestMapping（"/books"）
>
> 放在类前，使mapping方法自带相同前缀

