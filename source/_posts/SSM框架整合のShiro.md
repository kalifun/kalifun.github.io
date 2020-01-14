---
title: SSM框架整合のShiro
date: 2019-04-29  15:47:00
categories: Java
tags:
  - springboot
  - shiro
---
# SSM框架整合のShiro
>  前面我们学习了如何使用IDEA+Maven创建SSM框架，配置拦截器等。当你的项目越来越大，对不同的用户就有不同的权限，这样我们就不只是局限于系统去判定他是否登录了。
## 前提
>  已经利用Maven创建好SSM框架[IDEA 简单创建SSM框架](https://kalifun.top/archives/184)。数据相关内容请查看[Shiroの表结构设计](https://kalifun.top/archives/200)
## 1.导入jar包
```
<dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-spring</artifactId>
      <version>1.4.0</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-ehcache -->
    <dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-ehcache</artifactId>
      <version>1.4.0</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-quartz -->
    <dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-quartz</artifactId>
      <version>1.4.0</version>
    </dependency>


    <!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-web -->
    <dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-web</artifactId>
      <version>1.4.0</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-core -->
    <dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-core</artifactId>
      <version>1.4.0</version>
    </dependency>
```
将上面的贴到pom.xml中。当你使用其他版本时需要注意关联。尽可能版本相同。
## 2.整合Shiro
### 2.1 配置过滤器
> #### 修改web.xml文件
```
<filter>
    <filter-name>shiroFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
      <param-name>targetFilterLifecycle</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>shiroFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```
 你会发现和我们配置SpringMVC的EncodingFilter很像。DelegatingFilterProxy会自动到Spring容器中name为shiroFilter的bean，并且将所有Filter的操作都委托给他管理。
### 2.2配置Bean
>  创建spring-shiro.xml文件
```
<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean"></bean>
```
 需要注意这里的id需要和我们在web.xml中创建的过滤器名称相同否则无效。看过我[IDEA 简单创建SSM框架](https://kalifun.top/archives/184)这个文章的就知道，因为在web.xml设置名称为spring-*.xml。会自己去扫描文件前缀后缀匹配的。
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:util="http://www.springframework.org/schema/util"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">

    <!-- Shiro的Web过滤器 -->
    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <!-- Shiro的安全管理器，所有关于安全的操作都会经过SecurityManager -->
        <property name="securityManager" ref="securityManager"/>
        <!-- 系统认证提交地址，如果用户退出即session丢失就会访问这个页面 -->
        <property name="loginUrl" value="/login.jsp"/>
        <!-- 权限验证失败跳转的页面，需要配合Spring的ExceptionHandler异常处理机制使用 -->
        <property name="unauthorizedUrl" value="/unauthorized.jsp"/>
        <property name="filters">
            <util:map>
                <entry key="authc" value-ref="formAuthenticationFilter"/>
            </util:map>
        </property>
        <!-- 自定义的过滤器链，从上向下执行，一般将`/**`放到最下面 -->
        <property name="filterChainDefinitions">
            <value>
                <!-- 静态资源不拦截 -->
                /static/** = anon
                /lib/** = anon
                /js/** = anon

                <!-- 登录页面不拦截 -->
                /login.jsp = anon
                /login.do = anon

                <!-- Shiro提供了退出登录的配置`logout`，会生成路径为`/logout`的请求地址，访问这个地址即会退出当前账户并清空缓存 -->
                /logout = logout

                <!-- user表示身份通过或通过记住我通过的用户都能访问系统 -->
                /index.jsp = user

                <!-- `/**`表示所有请求，表示访问该地址的用户是身份验证通过或RememberMe登录的都可以 -->
                /** = user
            </value>
        </property>
    </bean>

    <!-- 基于Form表单的身份验证过滤器 -->
    <bean id="formAuthenticationFilter" class="org.apache.shiro.web.filter.authc.FormAuthenticationFilter">
        <property name="usernameParam" value="username"/>
        <property name="passwordParam" value="password"/>
        <property name="loginUrl" value="/login.jsp"/>
    </bean>

    <!-- 安全管理器 -->
    <bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
        <property name="realm" ref="userRealm"/>
    </bean>

    <!-- Realm实现 -->
    <bean id="userRealm" class="xx.xxxxx.xxxxx.UserRealm"></bean>
</bean> 
```
>  Realm：域，Shiro 从从Realm获取安全数据（如用户、角色、权限），就是说SecurityManager要验证用户身份，那么它需要从Realm获取相应的用户进行比较以确定用户身份是否合法；也需要从Realm得到用户相应的角色/权限进行验证用户是否能进行操作；可以把Realm看成DataSource ， 即安全数据源。
 上面配置类似于我们使用的mvc:interceptors一样。对需要过滤的进行设置，对不过滤的放入安全管理器进行管理。我们利用创建UserRealm实例来实现自定义的数据管理。
## 3.Shiro实现身份认证
### 3.1身份认证流程
>  一般后台登录我们需要提交的表单是username,password。不排除于利用邮箱登录的。根据后台登录页面提交的字段来针对form表单的身份验证过滤器进行修改（formAuthenticationFilter）。
-  用户输入这两个用户名和密码后提交表单，通过绑定了SecurityManager的SecurityUtils得到Subject实例，然后获取身份验证的UsernamePasswordToken传入用户名和密码。
-   调用subject.login(token)进行登录，SecurityManager会委托Authenticator把相应的token传给Realm，从Realm中获取身份认证信息。
-  Realm可以是自己实现的Realm，Realm会根据传入的用户名和密码去数据库进行校验（提供Service层登录接口）。
-  Shiro从Realm中获取安全数据（如用户、身份、权限等），如果校验失败，就会抛出异常，登录失败；否则就登录成功。
```
@Controller
public class LoginController {
    @RequestMapping("/login")
    public String login(
            @RequestParam(value = "username", required = false) String username,
            @RequestParam(value = "password", required = false) String password,
            Model model) {
        String error = null;
        if (username != null && password != null) {
            //初始化
            Subject subject = SecurityUtils.getSubject();
            UsernamePasswordToken token = new UsernamePasswordToken(username, password);
            try {
                //登录，即身份校验，由通过Spring注入的UserRealm会自动校验输入的用户名和密码在数据库中是否有对应的值
                subject.login(token);
                return "redirect:index.do";
            }catch (Exception e){
                e.printStackTrace();
                error = "未知错误，错误信息：" + e.getMessage();
            }
        } else {
            error = "请输入用户名和密码";
        }
        //登录失败，跳转到login页面，这里不做登录成功的处理，由
        model.addAttribute("error", error);
        return "login";
    }
}
```
 上面是一个login的controller。从前端获得获得字段为username,password。然后进行判断。
 当前端没有输入账号或者密码直接error，然后跳转到login。
 若两个字段都输入了的话。subject.login(token)进行登录，随后就是通过Realm进行登录校验，如果登录失败就可能抛出一系列异常，比如UnknownAccountException用户账户不存在异常、IncorrectCredentialsException用户名或密码错误异常、LockedAccountException账户锁定异常… 。
