---
title: SpringBootの代码生成器
date: 2019-05-16 09:38:00
categories: Java
tags:
  - springboot
---
# SpringBootの代码生成器
## 前言
>之前我们在使用SpringMVC的时候也讲到代码生成器。可以看[代码自动生成器のgeneratorconfig.xml](https://kalifun.top/archives/214)进行了解。代码生成器的好处就不用多说了，反正让我们可以快速搭建一个项目就对了。
## 配置
配置pox.xml
```
<!-- mybatisplus与springboot整合 -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.0.5</version>
        </dependency>
        <!-- velocity 模板引擎, 默认 -->
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity-engine-core</artifactId>
            <version>2.0</version>
        </dependency>
        <!-- freemarker 模板引擎 -->
        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>2.3.28</version>
        </dependency>
        <!-- mysql数据库连接驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.31</version>
        </dependency>
        <!--阿里druid数据库链接依赖-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.12</version>
        </dependency>
```
我们这里发现一个和我们之前提到不相同的东西那就是Mybatis-puls。
>  MyBatis-Plus（简称 MP）是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。
### 编写生成器程序
 到这步肯定都很懵了，就直接写生成器程序啦？对的，这里并不是采用的xml来配置生成器。而且是采用的springboot并没有之前繁琐的配置了。
```
public class CodeGenerator {
    public static String scanner(String tip){
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        help.append("请输入"+tip+":");
        System.out.println(help.toString());
        if (scanner.hasNext()){
            String ipt = scanner.next();
            if (StringUtils.isNotEmpty(ipt)){
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的"+tip);
    }

    public static void main(String[] args){
        AutoGenerator mpg = new AutoGenerator();

        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath+"/src/main/java");
        gc.setOpen(false);

        gc.setFileOverride(true);
        gc.setActiveRecord(false);
        gc.setEnableCache(false);
        gc.setBaseResultMap(true);
        gc.setBaseColumnList(false);
        gc.setAuthor("fun");
        mpg.setGlobalConfig(gc);

        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/ssm?useUnicode=true&useSSL=false&characterEncoding=utf8");     //连接数据库地址
        dsc.setDriverName("com.mysql.jdbc.Driver");       //连接数据库的驱动
        dsc.setUsername("root");                     //数据库用户名
        dsc.setPassword("root");                      //数据库密码
        mpg.setDataSource(dsc);

        PackageConfig pc = new PackageConfig();
        pc.setModuleName(scanner("模块名"));          //生成的代码将在此处输入的文件夹目录下
        pc.setParent("top.kalifun.imgurl");                   //创建项目时的src/java下的目录
        mpg.setPackageInfo(pc); 

        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {

            }
        };
        List<FileOutConfig> focList = new ArrayList<>();
        focList.add(new FileOutConfig("/templates/mapper.xml.ftl") {
            @Override
            public String outputFile(TableInfo tableInfo) {
                return projectPath + "/src/main/resources/mapper/" + pc.getModuleName() + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
            }
        });
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);
        mpg.setTemplate(new TemplateConfig().setXml(null));

        StrategyConfig strategy = new StrategyConfig();
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        strategy.setRestControllerStyle(true);
        strategy.setInclude(scanner("表名"));             //输入你需要生成此表相关的代码
        strategy.setSuperEntityColumns("id");
        strategy.setControllerMappingHyphenStyle(true);
        strategy.setTablePrefix(pc.getModuleName()+"_");
        mpg.setStrategy(strategy);
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());
        mpg.execute();

    }
}
```
这个可以定义为一个脚手架，当你需要自动生成代码时，直接对此代码进行修改就可以直接使用了，对效率提升是不是很大。
![![](https://image.kalifun.top/temp/1905/5879ce03f08a51af.png)](https://image.kalifun.top/temp/1905/5879ce03f08a51af.png)
此程序可以直接运行，执行后将会获得执行log
![![](https://image.kalifun.top/temp/1905/2bdf13cd16a6c3e5.png)](https://image.kalifun.top/temp/1905/2bdf13cd16a6c3e5.png)
我们将会获得controller,entity,mapper,service文件夹。在resource目录下的mapper也会根据数据库创建相应的mapper.xml文件。