---
title:  Java-ssm框架の拦截器
date: 2019-04-19 19:47:00
categories: Java
tags:
  - springboot
---
# Java-ssm框架の拦截器
## 为什么使用拦截器？
> 之前我一直再考虑是不是每次访问页面的时候都需要去考虑有没有存在session，再判断是否可以访问页面？其实差不多，拦截器就是干这个事情的，但是不需要你每次去做判断，只需要把你的拦截器写好，其他的就不用管了。
## 如何使用拦截器？
### 1.1配置XML
```
<!-- 拦截器 -->
    <mvc:interceptors>
        <!-- 多个拦截器，顺序执行 -->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <!--不拦截静态资源-->
            <mvc:exclude-mapping path="/*.css"/>
            <mvc:exclude-mapping path="/**/*.css"/>
            <mvc:exclude-mapping path="/**/*.js"/>
            <mvc:exclude-mapping path="/**/*.png"/>
            <mvc:exclude-mapping path="/**/*.gif"/>
            <mvc:exclude-mapping path="/**/*.jpg"/>
            <mvc:exclude-mapping path="/**/*.jpeg"/>
            <mvc:exclude-mapping path="/label_ctl/*" />
            <!--自定义拦截器-->
            <bean class="com.label.interceptor.LoginInterceptor" />
        </mvc:interceptor>
    </mvc:interceptors>
```
#### 设置拦截器有多种方法，这只是其中一种，更具自己如何构造SSM框架来决定把代码放在哪。
>  完整XML文件spring-mvc.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd">
    <context:component-scan base-package="com.label.controller" />

    <mvc:annotation-driven />

    <!--指定文件夹下所有文件不会被DispatcherServlet拦截，直接访问-->
    <mvc:resources mapping="/文件夹名称/**" location="/文件夹名称/"/>

    <!-- 拦截器 -->
    <mvc:interceptors>
        <!-- 多个拦截器，顺序执行 -->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <!--不拦截静态资源-->
            <mvc:exclude-mapping path="/*.css"/>
            <mvc:exclude-mapping path="/**/*.css"/>
            <mvc:exclude-mapping path="/**/*.js"/>
            <mvc:exclude-mapping path="/**/*.png"/>
            <mvc:exclude-mapping path="/**/*.gif"/>
            <mvc:exclude-mapping path="/**/*.jpg"/>
            <mvc:exclude-mapping path="/**/*.jpeg"/>
            <bean class="com.label.interceptor.LoginInterceptor" />
        </mvc:interceptor>
    </mvc:interceptors>

    <mvc:default-servlet-handler />
      <!--指定服务view的文件夹及后缀名-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/WEB-INF/views/" />   
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```
### 1.2 自定义拦截器
 根据上面设置的目录下创建实例LoginInterceptor
```
package com.label.interceptor;

import com.label.entity.User;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class LoginInterceptor implements HandlerInterceptor {
    /**
     * Handler执行完成之后调用这个方法
     */
    public void afterCompletion(HttpServletRequest request,
                                HttpServletResponse response, Object handler, Exception exc)
            throws Exception {

    }

    /**
     * Handler执行之后，ModelAndView返回之前调用这个方法
     */
    public void postHandle(HttpServletRequest request, HttpServletResponse response,
                           Object handler, ModelAndView modelAndView) throws Exception {
    }

    /**
     * Handler执行之前调用这个方法
     */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response,
                             Object handler) throws Exception {
        //获取请求的URL
        String url = request.getRequestURI();
        //URL:login.jsp是公开的;这个demo是除了login.jsp是可以公开访问的，其它的URL都进行拦截控制
        if(url.indexOf("/admin")>=0){
            return true;
        }
        //获取Session
        HttpSession session = request.getSession();
        User user = (User) session.getAttribute("user");

        if(user != null){
            return true;
        }
        //不符合条件的，跳转到登录界面
        request.getRequestDispatcher("/WEB-INF/views/admin/login.jsp").forward(request, response);

        return false;
    }
}

```
对于写后台的来说，上面基本是可以直接使用的。