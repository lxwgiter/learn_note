## 1、SpringBoot简介

### SpringBoot的时代背景

**微服务：**

- 微服务是一种架构风格
- 一个应用拆分为一组小型服务
- 每个服务运行在自己的进程内，也就是可独立部署和升级
- 服务之间使用轻量级HTTP交互
- 服务围绕业务功能拆分
- 可以由全自动部署机制独立部署
- 去中心化，服务自治。服务可以使用不同的语言、不同的存储技术



**分布式：**

- 有了微服务，更好理解分布式的概念，不同微服务可以部署到不同机器上，甚至同一个微服务也可以部署到不同机器上以增加体量。



**分布式的困难：**

- 远程调用
- 服务发现
- 负载均衡
- 服务容错
- 配置管理
- 服务监控
- 链路追踪
- 日志管理
- 任务调度
- ......



**分布式的解决：**

​	SpringBoot快速开发应用，SpringCloud搭建分布式框架，SpringCloudDataFlow用于信息传输。



**云原生**

​	原生应用如何上云。 Cloud Native



**上云的困难**

- 服务自愈
- 弹性伸缩
- 服务隔离
- 自动化部署
- 灰度发布
- 流量治理
- ......





### 什么是SpringBoot？

- SpringBoot是更高维的框架，SSM的底层是JavaWeb，而SpringBoot的底层是SSM
- Spring Boot 是所有基于 Spring 开发的项目的起点。Spring Boot 的设计是为了让你尽可能快的跑起来 Spring 应用程序并且尽可能减少你的配置文件。
- SpringBoot核心：快速整合第三方框架
- SpringBoot并不是Spring的替代品，是为了让程序员更好的使用Spring，简化Spring的开发





### SpringBoot的优点

- 创建独立Spring应用
	- 打包时一步到位，通过打包插件生成jar包，无需任何环境即可部署运行。
- 内嵌web服务器，意味着可以脱离Tomcat容器
<img src="E:\BaiduNetdiskDownload\学习笔记\SpringBoot\img\1.png" style="zoom:50%;" />
- 自动starter依赖，简化构建配置
	- spring-boot-starter-* 标识为启动器，该启动器包含了所有启动项目所需的依赖
- 自动配置Spring以及第三方功能
- 提供生产级别的监控、健康检查及外部化配置
- 无代码生成、无需编写XML
	- 使用注解+配置类完成配置，甚至无需配置，所有需要配置的组件都有默认值


- SpringBoot是整合Spring技术栈的一站式框架
- SpringBoot是简化Spring技术栈的快速开发脚手架





### SpringBoot缺点

- 人称版本帝，迭代快，需要时刻关注变化
- 封装太深，内部原理复杂，不容易精通



## 2、SpringBoot之HelloWorld

### 环境支持

- Maven3.3+
- Java8

### 项目代码

**pom.xml**

```xml
<!--需要声明Maven工程的父工程为spring-boot-starter-parent-->
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.3.4.RELEASE</version>
</parent>
<!--只做简单的表述层演示，spring-boot-starter-web用于启动一个web项目
	注意看这里没有版本号，版本号的声明在父工程里-->
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</dependency>
</dependencies>
```

**MainApplication.java**

```java
//主程序，SpringBoot项目运行的入口，不需要再配置Tomcat，运行在内置的Tomcat里
@SpringBootApplication//该注解用于标识主程序
public class MainApplication 
    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
    }
}
```



**HelloController.java**

```java
@RestController//该注解等价于@Comtroller+@ResponseBody
public class HelloController {
    @RequestMapping("/hello")
    public String handle01(){
        return "Hello, Spring Boot 2!";
    }
}
```

**效果展示：**

![](E:\BaiduNetdiskDownload\学习笔记\SpringBoot\img\2.png)



### 项目复盘

**从这个项目可以看出SpringBoot的诸多特性**

- 脱离了SSM的配置地狱，不用再写大堆的配置文件

- 不用配置Tomcat，甚至无需下载Tomcat，直接运行

- 无需设置字符过滤器，自动使用UTF-8

- 打包方式为jar包，只有部署在外部容器时才需要打成war包

- 只需设置父工程和极少数的依赖，就把所需要的依赖全部传递

- **@SpringBootApplication注解：**

  - 用于标识主程序类，自动进行包扫描，扫描组件

  - 默认扫描的包为：主程序所在包及其下面的所有子包

  - scanBasePackages属性用于指定扫描的包，如：@SpringBootApplication(scanBasePackages="com.lxw")

  - 该注解是复合注解，等价于

    ​	@SpringBootConfiguration

    ​	@EnableAutoConfiguration

    ​	@ComponentScan("com.lxw")



### 拓展

**application.properties文件**

maven工程的resources文件夹中创建application.properties文件，配置信息均可在这里修改，如：



```properties
# 设置Tomcat的端口号
server.port=8888
```

**打包部署**

在pom.xml添加打包插件

```xml
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
	</plugins>
</build>
```

在IDEA的Maven插件上点击运行 clean 、package，把helloworld工程项目的打包成jar包，

打包好的jar包被生成在helloworld工程项目的target文件夹内。

用cmd运行`java -jar boot-01-helloworld-1.0-SNAPSHOT.jar`，既可以运行helloworld工程项目。



## 3、SpringBoot之依赖管理

所有的SpringBoot项目都有同一个父项目：

```xml
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>2.3.4.RELEASE</version>
</parent>
```

这个父项目专门用来做依赖管理，如父项目其中一个依赖spring-boot-starter-web，只需这一个依赖即可引入所有环境

**IDEA中快捷键：ctrl + shift + alt + U**



![](E:\BaiduNetdiskDownload\学习笔记\SpringBoot\img\3.png)



而spring-boot-starter-parent的的父工程又为：

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-dependencies</artifactId>
  <version>2.3.4.RELEASE</version>
</parent>
```



### 自动版本仲裁机制

- spring-boot-dependencies里声明了所有可能用到的第三方依赖版本，这样做可以避免不同版本的第三方jar不兼容deng问题，比如声明了MySQL的驱动<mysql.version>8.0.21</mysql.version>，但是并不意味着整合了MySQL，只是提供了兼容的版本号，我们自己导入依赖时就不必关注版本号的问题了。

- 重写依赖版本：
  1. 查看spring-boot-dependencies里面规定当前依赖的版本 用的 key。
  1. 在当前项目里面重写配置，如下面的代码。


  ```xml
  <properties>
  	<mysql.version>5.1.43</mysql.version>
  </properties>
  ```



### 开发导入starter场景启动器

1. 见到很多 spring-boot-starter-* ： *就某种场景
2. 只要引入starter，这个场景的所有常规需要的依赖我们都自动引入
3. [更多SpringBoot所有支持的场景](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter)
4. 见到的  *-spring-boot-starter： 第三方为我们提供的简化开发的场景启动器。



## 4、@Configuration详解

**作用**：将一个类标识为配置类，用以代替配置文件，该配置类同样会被注册到IOC中

> 拓展：注册到IOC中的其实是配置类的代理类，该代理类一定通过AOP的增强实现了方法返回值的单例，所以
>
> 通过IOC获取配置类，再调用配置类中被@Bean标记的方法，拿到的对象一定是单例的。
>
> 证明：*com.lxw.boot.config.MyConfig$$EnhancerBySpringCGLIB$$51f1e1ca@1654a892*
>
> 



### 基本使用

- **Full模式**：全模式、设置为true、单例模式、性能差
- **Lite模式**：轻量级模式、设置为false、多例模式、性能好
- 示例

```java
/**
 * 1、@Bean注解：将方法的返回值注册到IOC中，id默认为方法名，可通过value指定id
 * 2、配置类本身也是组件
 * 3、proxyBeanMethods：代理bean的方法
 *      Full(proxyBeanMethods = true)（保证每个@Bean方法被调用多少次返回的组件都是单实例的）（默认）
 *      Lite(proxyBeanMethods = false)（每个@Bean方法被调用多少次返回的组件都是新创建的）
 */
@Configuration(proxyBeanMethods = false)
public class MyConfig {


    @Bean
    public User user01(){
        User zhangsan = new User("zhangsan", 18);
        //user组件依赖了Pet组件，这就要求这两个组件必须单例
        zhangsan.setPet(tomcatPet());
        return zhangsan;
    }

    @Bean("tom")
    public Pet tomcatPet(){
        return new Pet("tomcat");
    }
}
```

**@Configuration测试代码如下:**

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.lxw.boot")
public class MainApplication {

    public static void main(String[] args) {
    //1、返回我们IOC容器
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);

    //2、查看容器里面的组件
        String[] names = run.getBeanDefinitionNames();
        for (String name : names) {
            System.out.println(name);
        }

    //3、从容器中获取组件
        Pet tom01 = run.getBean("tom", Pet.class);
        Pet tom02 = run.getBean("tom", Pet.class);
        System.out.println("组件："+(tom01 == tom02));

    //4、com.lxw.boot.config.MyConfig$$EnhancerBySpringCGLIB$$51f1e1ca@1654a892
        MyConfig bean = run.getBean(MyConfig.class);
        System.out.println(bean);

    //如果@Configuration(proxyBeanMethods = true)代理对象调用方法。SpringBoot总会检查这个组件是否在容器中是否存在，确保保持组件单实例，所以性能会比较差
        User user = bean.user01();
        User user1 = bean.user01();
        System.out.println(user == user1);

        User user01 = run.getBean("user01", User.class);
        Pet tom = run.getBean("tom", Pet.class);

        System.out.println("用户的宠物："+(user01.getPet() == tom));
    }
}
```

### 总结

- 配置 类组件之间**无依赖关系**用Lite模式加速容器启动过程，减少判断
- 配置 类组件之间**有依赖关系**，方法会被调用得到之前单实例组件，用Full模式（默认）

> lite 英 [laɪt]   美 [laɪt]  
> adj. 低热量的，清淡的(light的一种拼写方法);类似…的劣质品



## 5、@Import注解

​	@Import注解引入普通的类可以帮助我们把普通的类定义为Bean，在SpringBoot底层大量使用



**使用语法：**

​	标注在类的上方：**@Import(Class类对象数组)**，例如：

​	@Import({User.class, DBHelper.class})



**标注位置：**

​	@Import可以添加在@SpringBootApplication(启动类)、@Configuration(配置类)、@Component(组件类)对应的类上。

```java
@Import({User.class, DBHelper.class})
@Configuration(proxyBeanMethods = false) //告诉SpringBoot这是一个配置类 == 配置文件
public class MyConfig {
}
```



**作用：**

​	给容器中**自动创建出这两个类型的组件**、默认组件的名字就是全类名



```java
@Import({DBHelper.class})
@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
        DBHelper bean = run.getBean(DBHelper.class);
        System.out.println(bean);//ch.qos.logback.core.db.DBHelper@5ba745bc
    }
}
```



## 6、@Conditional条件装配

**作用：**

​	***条件装配：满足Conditional指定的条件，则进行组件注入，是SpringBoot实现按需加载的关键！***



**标注位置：**

- 标在配置类的方法上：只有满足了条件装配，才有可能执行当前方法。
- 标在配置类上：只有满足了条件装配，才有可能执行当前类中的方法、类中的属性才可能生效。



@Conditional只是父注解，他的派生注解才是真正起作用的，如：



| @Conditional扩展注解            | 作用（判断是否满足当前指定条件）                 |
| ------------------------------- | ------------------------------------------------ |
| @ConditionalOnJava              | 系统的java版本是否符合要求                       |
| @ConditionalOnBean              | 容器中存在指定Bean；                             |
| @ConditionalOnMissingBean       | 容器中不存在指定Bean；                           |
| @ConditionalOnExpression        | 满足SpEL表达式指定                               |
| @ConditionalOnClass             | 系统中有指定的类                                 |
| @ConditionalOnMissingClass      | 系统中没有指定的类                               |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
| @ConditionalOnResource          | 类路径下是否存在指定资源文件                     |
| @ConditionalOnWebApplication    | 当前是web环境                                    |
| @ConditionalOnNotWebApplication | 当前不是web环境                                  |
| @ConditionalOnJndi              | JNDI存在指定项                                   |

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnMissingBean(name = "tom")//当容器中没有name为tom的Bean时，MyConfig类的Bean才能生效。
public class MyConfig {

    @Bean
    public User user01(){
        User zhangsan = new User("zhangsan", 18);
        zhangsan.setPet(tomcatPet());
        return zhangsan;
    }

    @Bean("tom22")
    public Pet tomcatPet(){
        return new Pet("tomcat");
    }
}
```



## 7、@ImportResource注解

**作用：**

​	导入Spring的xml配置文件，使其生效。

**语法：**

​	@ImportResource("classpath:文件名")，要求配置文件的位置复合规范，在resources目录下

**标注位置：**

​	配置类上



**使用示例：**

```java
@ImportResource("classpath:beans.xml")
public class MyConfig {
...
}
```



## 8、@ConfigurationProperties配置绑定

**作用：**

​	从配置文件(application.properties)中读取配置信息，配装到专门的配置Bean中

**语法：**

​	@ConfigurationProperties(prefix = ""),prefix指前缀，及在application.propeoties中声明的前缀

**标注位置：**

​	配置Bean的类上方



**设有配置文件application.propeoties**

```properties
mycar.brand=BYD
mycar.price=100000
```

### 配置绑定实现方式之一

​	使用**@ConfigurationProperties + @Component**注解



*以下是自己创建的配置Bean（一般都是SpringBoot提供的配置Bean）*，这里做演示用途：

```java
@Component//标识为组件
@ConfigurationProperties(prefix = "mycar")
public class Car {
	String brand;
    double price;
}
```

> 当实例化Car对象时，其两个属性已经被注入，且是从application.propeoties读取的属性值

### 配置绑定实现方式之二

​	使用**@EnableConfigurationProperties + @ConfigurationProperties**注解，这种方式就不用使用@Component注解将配置Bean配置到IOC容器中了，@EnableConfigurationProperties会帮我们完成Bean组件的配置。



**@EnableConfigurationProperties注解**

- 用于启用配置绑定
- 参数为需要启动配置绑定的配置Bean的Class对象，如：
- @EnableConfigurationProperties(Car.class)
- 标注在配置类的上方，而不是配置Bean的上方



**使用示范：**

```java
@EnableConfigurationProperties(Car.class)
public class MyConfig {
}
```

```java
@ConfigurationProperties(prefix = "mycar")
public class Car {
	String brand;
    double price;
}
```

> 当实例化Car对象时，其两个属性已经被注入，且是从application.propeoties读取的属性值



## 9、源码分析入口-@SpringBootApplication

**作用：**

​	用于标识SpringBoot程序的入口



**下面开始拆解@SpringBootApplication注解**



@SpringBootApplication("包名")是复合注解，它等价于：

- @SpringBootConfiguration

- @EnableAutoConfiguration
- @ComponentScan("包名")



### @SpringBootConfiguration注解

- 注解源码：

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
    @AliasFor(
        annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}
```

> @SpringBootConfiguration注解底层就是@Configuration注解，用于说明该类是SpringBoot配置类

### @EnableAutoConfiguration注解

注解源码：

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

    Class<?>[] exclude() default {};

    String[] excludeName() default {};
}
```

> 该注解复合方式：
>
> - @AutoConfigurationPackage、
> - @Import({AutoConfigurationImportSelector.class})



#### @AutoConfigurationPackage注解

​	注解名直译为：自动配置包，指定了默认的包规则。



注解源码：

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@Import({AutoConfigurationPackages.Registrar.class})
public @interface AutoConfigurationPackage {
    String[] basePackages() default {};

    Class<?>[] basePackageClasses() default {};
}
```

> 该注解本质其实是**@Import({AutoConfigurationPackages.Registrar.class})**，起名@AutoConfigurationPackage是为了见名知意

##### @Import({AutoConfigurationPackages.Registrar.class})

**作用：**

- **导入了Registrar类。**

- 利用Registrar给容器中导入一系列组件。
- 将指定的一个包下的所有组件导入进MainApplication所在包下。

>在Registrar类中会读取@SpringBootApplication("包名")注解设置的包名，对该包及其子包的组件进行注册，
>
>若是没有指定包名，则默认注册MainApplication所在包及其子包的组件。

#### @Import({AutoConfigurationImportSelector.class})

作用：**配合@Conditional条件装配，实现SpringBoot的按需加载配置文件的特性。**



AutoConfigurationImportSelector的作用：**读取某配置文件，加载127个配置类。**



```properties
# 文件里面写死了spring-boot一启动就要给容器中加载的所有配置类
# spring-boot-autoconfigure-2.3.4.RELEASE.jar/META-INF/spring.factories
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
...
```

> - xxxAutoConfiguration其实就是自动配置类，我们之所以可以直接启动SpringBoot程序，而不需要进行配置是因为，这些自动配置类已经为我们进行了自动配置。
> - 虽然我们127个场景的所有自动配置启动的时候默认全部加载，但是`xxxxAutoConfiguration`按照条件装配规则（`@Conditional`），最终会按需配置。

如`AopAutoConfiguration`类：

```java
@Configuration(
    proxyBeanMethods = false
)
@ConditionalOnProperty(
    prefix = "spring.aop",
    name = "auto",
    havingValue = "true",
    matchIfMissing = true
)//满足条件时才会进行配置，并加入到IOC容器中
public class AopAutoConfiguration {
    public AopAutoConfiguration() {
    }
	...
}
```



### @ComponentScan("包名")

​	作用：指定扫描的包



## 10、源码分析-自动配置流程

以`DispatcherServletAutoConfiguration`的内部类`DispatcherServletConfiguration`为例子:

```java
@Bean
@ConditionalOnBean(MultipartResolver.class)  //容器中有这个类型组件
 //容器中没有这个名字（multipartResolver）的组件
@ConditionalOnMissingBean(name = DispatcherServlet.MULTIPART_RESOLVER_BEAN_NAME)
public MultipartResolver multipartResolver(MultipartResolver resolver) {
	//给@Bean标注的方法传入了对象参数，这个参数的值就会从容器中找。
	//SpringMVC multipartResolver。防止有些用户配置的文件上传解析器不符合规范
	return resolver;//给容器中加入了文件上传解析器；
}
```

配置流程以HttpEncodingAutoConfiguration配置类为例(已删去不必要阅读的代码)：

```java
@Configuration(
    proxyBeanMethods = false
)
@EnableConfigurationProperties({ServerProperties.class})//开启配置Bean
@ConditionalOnClass({CharacterEncodingFilter.class})//条件装配
@ConditionalOnProperty(
    prefix = "server.servlet.encoding",
    value = {"enabled"},
    matchIfMissing = true
)//条件装配
public class HttpEncodingAutoConfiguration {
    private final Encoding properties;

    public HttpEncodingAutoConfiguration(ServerProperties properties) {
        this.properties = properties.getServlet().getEncoding();
    }

    @Bean
    @ConditionalOnMissingBean//意思是，若是用户没有配置，那么这里就会进行自动配置，体现了用户的优先性
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.web.servlet.server.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.web.servlet.server.Encoding.Type.RESPONSE));
        return filter;
    }
}
```

总结：

1. 配置Bean部分：
   - 自动配置类中会由@EnableConfigurationProperties注解，值即为配置Bean
   - 自动配置类的属性从配置Bean的属性中读取，配置Bean都有默认值。
   - 我们可以在application.properties中修改配置Bean的值，前缀可以点进源码看prefix的值
   - 查看prefix的值的步骤为：找到xxxAutoConfiguration->查看@EnableConfigurationProperties注解->进入配置Bean

2. 配置

   - SpringBoot先加载所有的自动配置类  xxxxxAutoConfiguration

   - 生效的配置类就会给容器中装配很多组件

   - 只要容器中有这些组件，相当于这些功能就有了

   - 定制化配置
     - 用户直接自己@Bean替换底层的组件
     - 用户去看这个组件是获取的配置文件什么值就去修改。







