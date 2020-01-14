---
title: Java分页のPageHelper
date: 2019-04-19 20:22:00
categories: Java
tags:
  - springboot
---
# Java分页のPageHelper
## 为什么选择PageHelper
>  一听到分页很多人第一反应利用jQuery不就可以啦，我一开始也是这么想的。但是对于没有学过前端的人来说，真的懒得看前端怎么实现分页功能的。唉，除了懒感觉自己没有别的优点了。 
- #### 利用超少代码实现分页功能。
- #### 快速实现，不需要繁琐的前端知识。
## PageHelper实现步骤
### 1.1 配置pom.xml实现导包
```
<dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>4.1.6</version>
  </dependency>
```
 建议高版本，因为我在低版本时功能无法实现。
### 1.2配置mybatis
 因为我们是针对数据库查询后，进行数据分页。这也算是为什么选择它的原因。
```
<!-- 配置SqlSessionFactory对象 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 注入数据库连接池 -->
        <property name="dataSource" ref="dataSource"/>
        <!-- 扫描entity包 使用别名 -->
        <property name="typeAliasesPackage" value="com.label.entity"/>
        <!-- 扫描sql配置文件:mapper需要的xml文件 -->
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
        <!--设置PageHelper-->
        <property name="plugins">
            <array>
                <bean class="com.github.pagehelper.PageHelper">
                    <property name="properties">
                        <value>
                          <!-- 还有其他设置，可以自己去看文档根据自己需求进行设置-->
                            dialect=mysql
                        </value>
                    </property>
                </bean>
            </array>
        </property>
    </bean>
```
### 1.3SSM代码
 controller与前端交互
>  controller片段代码
```
public ModelAndView fonTable(Page page){
        ModelAndView mav = new ModelAndView("admin/test");
        int total = pictureService.getTotal();
        System.out.println("Total>>>>"+total);
        page.calculateEnd(total);
        if (page.getStart() < 0) {
            page.setStart(0);
        }else if (page.getStart() > total) {
            page.setEnd(page.getEnd());
        }
        PageHelper.offsetPage(page.getStart(),16);
        List<PictureDate> allPic = pictureService.listPic();
        for (int i = 0;i < allPic.size();i++) {
            System.out.println(allPic.get(i));
        }
        System.out.println(allPic);
        mav.addObject("allpic",allPic);
        return mav;
    }
```
>  前端片段代码
```
<li class="am-disabled"><a href="?start=0">«</a></li>
<li class="am-active"><a href="?start=${page.getStart()-page.getCount()}">上一页</a></li>
<li><a href="?start=${page.getStart()+page.getCount()}">下一页</a></li>
<li><a href="?start=${page.getEnd()}">»</a></li>
```
 其实很多代码都省略了，比如entity，dao，service，mapper相关的代码我都省略了。
 立一个flag，写一个平台出一个详细的文章进行学习。