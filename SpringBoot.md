# SpringBoot
[51CT0学院]
![enter image description here](https://github.com/BayShirley/ReadingNotes/blob/master/imgs/SpringBoot01.png)
-------------------

[TOC]
## SpringBoot 简介
![enter image description here](https://github.com/BayShirley/ReadingNotes/blob/master/imgs/SpringBoot02.png)
![enter image description here](https://github.com/BayShirley/ReadingNotes/blob/master/imgs/SpringBoot03.png)

## Spring4快速入门
 org.apache.maven.archetypes:maven-archetype-quickstart 选择该骨架
 环境：
 Apache maven 3.3.9
Jdk1.8
SpringBoot 1.4.0.RELEASE
Spring 4.3.2.RELEASE
bean装配获取，[链接](https://github.com/fletcher/MultiMarkdown/wiki/MultiMarkdown-Syntax-Guide#footnotes) 
bean的生命周期见MyBean1类
### 装配
装配实例可以采用`@Compoent`注解，在配置类中通过`@Bean`显示声明装配推荐在引入的类实例的时候用。
其他装配注解`@Service`，用在业务层；`@Repository`,用在数据库层；`@Controller`,用在控制层。通过注解装配的类，在使用AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class,MyBean2.class);的时候，需要参数传入。
### 依赖注入
`@Autowired`,依赖注入，spring平台的。`@Resource`JSR-250注解,java平台。`@Inject`JER-330注解
`@Qualifier("")`,结合`@Autowired`使用，在有多种装配方式的时候，通过名字指定采用哪种装配方式
`@Primary`,有多种装配方式的，优先采用有该注解的装配方式
### 扫码注解
`@ComponentScan`
```java
@ComponentScan(basePackages = {"xin.shirleybay.test.spring"},excludeFilters = @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE,classes = {MyBean1.class}))
```

## Spring4扩展分析
TODO
## SpringBoot快速入门

## SpringBoot配置分析
## SpringBoot自动配置
## SpringBoot@Enable*注解的工作原理
## SpringBoot@EnableAutoConfiguration深入分析
## Springboot事件监听
## SpringBoot扩展分析
## SpringBoot运行流程分析
## SpringBoot Web
## SpringBoot定制和优化内嵌的Tomcat
## SpringBoot JDBC
## SpringBoot AOP
## SpringBoot Starter
## SpringBoot日志
## SpringBoot 监控和度量
## SpringBoot 测试
## SpringBoot 构建微服务实战
## SpringBoot 服务的注册和发现
## SpringBoot 应用的打包和部署

 


[1]: http://maxiang.info/client_zh
[2]: https://chrome.google.com/webstore/detail/kidnkfckhbdkfgbicccmdggmpgogehop
[3]: http://adrai.github.io/flowchart.js/
[4]: http://bramp.github.io/js-sequence-diagrams/
[5]: https://dev.yinxiang.com/doc/articles/enml.php