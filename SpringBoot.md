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
```java
pom.xml内容配置
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.3.2.RELEASE</version>
</dependency>
```
## spring 容器
``` java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
//包扫描方式
// AnnotationConfigApplicationContext context1 = new AnnotationConfigApplicationContext("xin.shirleybay.test.spring");
```
### 从容器中获取bean
``` java
context.getBean(MyBean1.class);//通过class获取
context.getBean("myBean1");//通过名字获取
```
bean的默认名字是@Bean注解上的方法名字，也可以自己指定名字，如下：
``` java
@Bean(name = "myBean1")//指定名字
@Scope("prototype")//prototype多例模式
public MyBean1 creatMyBean1(){
        return new MyBean1();
    }
```

###  bean装配获取 
``` java
package xin.shirleybay.test.spring;
import org.springframework.context.annotation.*;

@ComponentScan(basePackages = {"xin.shirleybay.test.spring"},excludeFilters = @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE,classes = {MyBean1.class}))
@Configuration
public class MyConfig {
    /*方式一============================================*/
    @Bean(name = "myBean1")
    public MyBean1 creatMyBean1(){
        return new MyBean1();
    }
    /*方式二  工厂模式一==========创建的是MyBean2对象=========*/
    @Bean
    public MyBean2FactoryBean creatMybean2FactoryBean(){
        return new MyBean2FactoryBean();
    }
    /*方式三  工厂模式二=================================*/
    @Bean
    public MyBean3Factory creatMyBean3Factory(){
        return new MyBean3Factory();
    }
    @Bean
    public MyBean3 createMyBean3(MyBean3Factory myBean3Factory){
         return myBean3Factory.creatMyBean3();
    }

}
MyBean2FactoryBean类如下：
package xin.shirleybay.test.spring;

import org.springframework.beans.factory.FactoryBean;

public class MyBean2FactoryBean implements FactoryBean<MyBean2> {//MyBean2对象可以换成你想要工厂模式创造的对象

    @Override
    public MyBean2 getObject() throws Exception {//工厂模式创造的bean
        return new MyBean2();
    }
    @Override
    public Class<?> getObjectType() { //通过类的类型获取xx.class
        return MyBean2.class;
    }//工厂模式创造bean的class类型

    @Override
    public boolean isSingleton() {
        return true;
    }//是否是单利模式
}

MyBean3Factory类如下
package xin.shirleybay.test.spring;
public class MyBean3Factory {
    public MyBean3 creatMyBean3(){
        return new MyBean3();
    }
}

```
通过工厂方法，获取工厂实例本身的方法：
``` java
context.getBean(MyBean2FactoryBean.class);//通过class获取
context.getBean("&creatMybean2FactoryBean");//通过名字获取，在名字前面加&。在org.springframework.beans.factory.BeanFactory中可以看到说明，这是一个容器的顶级接口
```
 
### bean的生命周期
bean生命周期实现一：
使用@Bean装配的bean
bean实现接口方式。实现`InitializingBean`接口重写public void afterPropertiesSet()方法，在属性初始化之后执行；实现`DisposableBean`接口重写public void destroy()方法，在bean销毁之后执行。
bean生命周期实现二：
使用@Bean装配的bean
不用实现接口，在bean装配的时候，通过@Bean(name = "myBean1",initMethod = "init",destroyMethod = "des")指定初始化和销毁的方法，然后在bean中实现这些方法。但是这些方法都是无参数的。
bean生命周期三：
在bean中通过注解方式：
``` java
	@PostConstruct
    public void init3(){
        System.out.println("mybean1 init3");
    }

    @PreDestroy
    public void destroy3(){
        System.out.println("mybean1 destroy3");
    }
```
通过`@Component`，`@Service`，`@Repository`，`@Controller`装配的bean无法使用上述的生命周期
###  装配
装配实例可以采用`@Compoent`注解，在配置类中通过`@Bean`显示声明装配，推荐在引入的类实例的时候用。
其他装配注解`@Service`，用在业务层；`@Repository`,用在数据库层；`@Controller`,用在控制层。通过注解装配的类，在使用AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class,MyBean2.class);的时候，需要参数传入。因为没有指明扫描包，在类上的注解会不生效。
### 依赖注入
`@Autowired`,依赖注入，spring平台的。`@Resource`JSR-250注解,java平台。`@Inject`JER-330注解，使用该注解需要在pom文件中加入以下依赖：
``` java
<dependency>
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
</dependency>
```
`@Qualifier("")`,结合`@Autowired`使用，在有多种装配方式的时候，通过名字指定采用哪种装配方式
`@Primary`和`@Bean`一起使用,有多种装配方式的，优先采用有该注解的装配方式
###  扫码注解
`@ComponentScan`
```java
@ComponentScan(basePackages = {"xin.shirleybay.test.spring"},excludeFilters = @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE,classes = {MyBean1.class}))
```
excludeFilters排除
## Spring4扩展分析
### 依赖注入`ApplicationContext`
方法一：使用`@Autowired`注解：
``` java
package xin.shirleybay.test.spring;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.stereotype.Component;

@Component
public class MyBean3  {
    @Autowired
    private ApplicationContext applicationContext;
    public void show(){
        System.out.println(applicationContext.getClass());
    }
}

App类
package xin.shirleybay.test.spring;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;
public class App 
{
    public static void main( String[] args )
    {       
        //包扫描方式
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext("xin.shirleybay.test.spring");
        context.getBean(MyBean3.class).show(); 
        context.close();
    }
}

```
方式二：实现`ApplicationContextAware`接口方式
``` java
package xin.shirleybay.test.spring;

import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
import org.springframework.stereotype.Component;

@Component
public class MyBean3 implements ApplicationContextAware {
    private ApplicationContext applicationContext;
    @Override
    /*spring会自动调用该方法并把ApplicationContext的实例传入进去*/
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }
    public void show(){
        System.out.println(applicationContext.getClass());
    }
}

```
方式三：构造函数传入。使用spring4.3新特性
由于spring容器创建实例调用的是空参构造，采用该方法，构造函数只能有一个，且传入的参数只能是`ApplicationContext `
``` java
@Component
public class MyBean3   {
    private ApplicationContext applicationContext;
    public MyBean3(ApplicationContext applicationContext) {
        this.applicationContext = applicationContext;
    }
    public void show(){
        System.out.println(applicationContext.getClass());
    }
}
```
### spring管理的bean 的所有的生命周期：`BeanPostProcessor`
对于实现`BeanPostProcessor`接口的子类，且被spring管理后，spring会对所有的bean调用postProcessBeforeInitialization和postProcessAfterInitialization，且这两个方法的返回值就是bean装配的值，如果返回null，spring容器就获取不到该bean。（可以利用这一点返回的是bean的代理对象而不是bean对象本身）
``` java
package xin.shirleybay.test.spring;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.stereotype.Component;

@Component
public class EchoBeanPostProcessor implements BeanPostProcessor {
    @Override
    //在构造，bean依赖注入完成之后调用，在前面所讲的bean自己生命周期之后调用
    public Object postProcessBeforeInitialization(Object bean, String s) throws BeansException {
        System.out.println("before "+bean.getClass()+"==s:"+s);
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String s) throws BeansException {
        System.out.println(bean.getClass()+"==s::"+s);
        return bean;
    }
}

```
`ApplicationContextAware`的实现就是通过`BeanPostProcessor`
### 接口`BeanFactoryPostProcessor`对spring容器初始化后的调用
只会触发一次
该接口的方法会在bean前面将的生命周期之前调用，等等，是最先执行的
### 接口BeanDefinitionRegistryPostProcessor
该接口是`BeanFactoryPostProcessor`的子接口
该接口的方法`void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry var1) `会传入BeanDefinitionRegistry，其作用是注册一个bean到spring容器中，等同于`@Bean`,`@Component`等注解。能够实习动态注册
如下：
``` java
package xin.shirleybay.test.spring;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.ConfigurableListableBeanFactory;
import org.springframework.beans.factory.support.BeanDefinitionBuilder;
import org.springframework.beans.factory.support.BeanDefinitionRegistry;
import org.springframework.beans.factory.support.BeanDefinitionRegistryPostProcessor;
import org.springframework.stereotype.Component;

@Component
public class MyBeanDefinitionRegistryPostProcessor implements BeanDefinitionRegistryPostProcessor {
    @Override
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry beanDefinitionRegistry) throws BeansException {
       //注入5个对象，相当于@Bean,@Component等静态注入5次
        for (int i=0;i<5;i++){
            //类的定义是基于MyBean2这个对象的
            BeanDefinitionBuilder builder = BeanDefinitionBuilder.rootBeanDefinition(MyBean2.class);
            //对象属性注入，调用的是对象的setXXX方法
            builder.addPropertyValue("name","admin"+i);
            beanDefinitionRegistry.registerBeanDefinition("mybean2_"+i,builder.getBeanDefinition());
        }
    }

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory configurableListableBeanFactory) throws BeansException {

    }
}

App类中调用：
package xin.shirleybay.test.spring;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
 
public class App 
{
    public static void main( String[] args )
    {
        //包扫描方式
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext("xin.shirleybay.test.spring");
        context.getBean("mybean2_1");
        context.close();
    }
}

```
## SpringBoot快速入门
pom文件配置参照官网的快速开发，或者如下：
``` java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--<parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.0.0.RELEASE</version>
   </parent>-->
    <groupId>xin.shirleybay.test.sboot</groupId>
    <artifactId>sboot</artifactId>
    <version>1.0.0</version>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>UTF-8</maven.compiler.source>
        <maven.compiler.target>UTF-8</maven.compiler.target>
    </properties>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>1.5.2.RELEASE</version>
                <scope>import</scope>
                <type>pom</type>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
    </dependencies>

</project>
```
SpringBoot应用程序运行：
``` java
package xin.shirleybay.test.sboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.Bean;

import java.util.HashSet;
import java.util.Set;

/*不使用注解也可以运行，SpringApplication.run(App.class,args)会把第一个参数App.class装配成配置类*/
@SpringBootApplication
public class App {
    @Bean
    public Runnable getRunnable(){
        return () -> {
            System.out.println("springBoot run");
        };
    }
    public static void main(String[] args) {
        /*ConfigurableApplicationContext context = SpringApplication.run(App.class,args);
        context.getBean(Runnable.class).run();*/
        //下面这种方法可以通过set集合设置多个配置类
        SpringApplication app = new SpringApplication();
        Set<Object> set = new HashSet<>();
        set.add(App.class);
        app.setSources(set);
        ConfigurableApplicationContext context = app.run(args);
        context.getBean(Runnable.class).run();
    }
}
```

## SpringBoot配置分析
 SpringBoot默认的配置文件为application.properties。放在src/main/resources或者src/main/resources/config下
 application.properties内容示例如下：
``` java
 local.ip=192.168.3.65
```
 获取application.properties里面的配置：
``` java
 context.getEnvironment().getProperty("local.ip")；
 //也可以将Environment对象依赖注入，进行使用
 /*或者采用下面的方式进*/
 @Value("${local.ip}")
 private  String ip;
```
application.properties文件中也可以对文件中本身的键值对进行引用，如下：
``` java
name=wangwu
tt.name=this is ${name}
```
 在properties文件中，有键没有值的时候，可以设置默认值，如下
``` java
 application.properties文件内容如下：
 tomcat.port
 配置类中如下
 @Value("${tomcat.port:8080}")//设置默认值为8080
 Private String port
```
在配置类上使用`@PropertySource("classPath:xx.properties")`注解,或者`@PropertySources({@PropertySource("classpath:/a/a.properties"),@PropertySource("classpath:/a/b.properties")})`可以让spring纳入多个propertie文件
### 将properties文件中的键值对封装在java对象中
xx.properties文件中内容如下：
``` java
ds.url=http://localhost:xxxx
ds.driveClassName=ccc
ds.userName=12
```
通过`@ConfigurationProperties(prefix = "ds")`进行自动封装.但是属性需要实现set方法，通过调用set方式实现封装
``` java
package xin.shirleybay.test.sboot;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.PropertySource;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "ds")
@PropertySource("classpath:/a/c.properties")
public class DateSource {
    public String url;
    public String driveClassName;
    public String userName;

    public void setUrl(String url) {
        this.url = url;
    }

    public void setDriveClassName(String driveClassName) {
        this.driveClassName = driveClassName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }
}

```
TODO
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