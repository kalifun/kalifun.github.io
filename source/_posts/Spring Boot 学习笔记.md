---
title: Spring Boot 学习笔记
date: 2019-05-24 09:15:00
categories: Java
tags:
  - springboot
---

# Spring Boot 学习笔记

## 注释器

### @Aspect

> ###    把当前的注释类标识为一个切面供容器读取

### @ResponseBody

> #### 此方法返回的是文本而不是视图

###　@RestController

> #### RestController = @Controller + @ResponseBody

### @Component

> #### 声明此类是一个Spring管理的类

###  @Configuration

> #### 声明此类是一个配置类，通常与注解@Bean配合使用

```java
@Configuration
public class DataSourceConfig {
    @Bean(name = "datasoucre")
    public DataSource dataSource(Environment env){
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setUrl(env.getProperty("spring.datasource.url"));
        druidDataSource.setUsername(env.getProperty("spring.datasource.username"));
        druidDataSource.setPassword(env.getProperty("spring.datasource.password"));
        druidDataSource.setDriverClassName(env.getProperty("spring.datasource.driver-class-name"));
        return druidDataSource;
    }
}
```

### ＠RequestMapping

>  #### Spring提供了简化后的@RequestMapping，提供了新的注解来表示http方法：

- **@GetMapping** 

- **@PostMapping**

- **@DeleteMapping**

- **@PutMapping**

- **@PatchMapping**

  ------

  

  #### 属性consumes意味着请求的http头的Content-Type媒体类型与consumes匹配才能调用此方法

  ```java
  @GetMapping(value=("/test.json"),consumes="appliation/json")
  ```

  ##### 这里映射的请求媒体类型是**application/json**，所以无法接收到Ajax的请求方法。

  ------

  

  #### produces 属性对应于 HTTP 请求 Accept 字段 只有匹配得上的方法才能被调用。

  ```java
  @GetMapping(path="/user/{userID}",produces=MediaType.APPLICATION_JSON_UTF8_VALUE)
  ```

------



## 方法参数

- **@PathVariable，可以将URL的值映射到方法参数中。**

  ```java
  @RequestMapping("/user/{id}")
  public String user(@PathVariable Long id)
  ```

  

- **Model，Spring中通用的MVC模型，也可以使用Map和ModelMap作为渲染视图的模型**

  ```java
  User userinfo = userService.getUserByName(userid);
  model.addAttribute("user",userinfo);
  return "user.jsp";
  ```

  ##### ~~将获得的userinfo的信息绑定到在user中。我们在user.jsp中即可调用。使用方法为：~~

  ```java
  ${{user}}
  ```

  

- **ModelAndView，包含了模型和视图路径**

  ```java
  public String hello(ModelAndView model){
      model.addObject("user",userinfo);
      model.setViewName("hello.jsp");
  }
  ```

- **MultipartFile,用于处理文件上传**

  ```java
  @PostMapping("/form")
  @ResponseBody
  public String handleFormUpload(String name,MultipartFile file) throws IOException{
      if (!file.isEmpty()) {
          String filename = file.getOriginalFilename();
          InputStream ins = file.getInputStream();
          return "success";
      }
      return "failure"
  }
  ```

  **MultipartFile提供了以下方法来获取上传的文件信息：**

  - **getOriginalFilename，获取上传的文件名字；**
  - **getBytes，获取上传文件内容，转为字节数组。**
  - **getInputStream，获取一个InputStream**
  - **getSize，获取文件大小。**
  - **isEmpty，文件上传内容为空，或者就没有文件上传。**
  - **transferTo(File dest),保存上传文件到目标文件系统。**

## 验证框架

### JSR-303

> #### JSR-303是Java标准的验证框架，已有的实现有Hibernate validator。

- **空检查**

  - **@Null，验证对象为空**

  - **@NotNull，验证对象不为空。**

  - **@NotBlank，验证字符串不为空或者不是空字符串。**

  - **NotEmpty，验证字符不为空，或者字符集不为空。**

    

- **长度检查**
  - **@Size(min=,max=),验证字符长度，支持字符串，字符集。**
  - **@Lenth，字符长度。**

- **数值检测**
  - **@Min，验证数字是否大于等于指定值。**
  - **@Max，验证数字是否小于等于指定值。**
  - **@Digits，验证数字是否符合指定格式，如@Digits(integer=2，fraction=1)**
  - **@Range，验证数字是否在指定的范围内。@Range(min=1，max=5)**

- 其他
  - **@Email，验证是否为邮箱格式，为null则不做校验。**
  - **@Pattern，验证String对象是否符合正则表达式的规则。**