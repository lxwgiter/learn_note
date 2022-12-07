## 1、各框架的定位

### Spring：

- 整合型框架，提供了IOC做对象管理、AOP做事务切面、事务管理整合持久层和Service层。
- Spring可整合持久层，比如对Mybatis的整合，具体做法是把SqlSessionFactoryBean交给Spring的IOC容器，并且在Srpng中对Mybatis进行配置，甚至可以省略Mybatis核心配置文件。此外还可以通过配置MapperScannerConfigurer将所有Mapper的实现类实例化并交给SpringIOC容器。
- Spring配置文件中不扫描控制层组件，控制层组件交给SpringMVC的配置文件
- Spring中配置事务管理的切面，并开启事务管理注解驱动
- 注意区分SpringIOC容器和SpringMVCIOC容器的区别SpringIOC容器是SpringMVCIOC容器的父容器，**所以在创建SpringMVCIOC容器时前者的容器必须存在**，这就要求Spring配置文件的加载一定要在SpringMVC的配置文件加载之前。所以可以使用**ContextLoaderListener**进行监听，在服务器启动时加载Spring的配置文件：ContextLoaderListener需要在web.xml中注册。
- SpringMVC和Spring可整合，也可不整合。

**综上所述，在Spring配置文件中必须要做的是：**

> 1. 扫描组件(除控制层)
> 2. 引入jdbc.properties
> 3. 配置数据源
> 4. 配置事务管理切面
> 5. 开始事务注解驱动
> 6. 配置SqlSessionFactoryBean，并设置必要属性
> 7. 配置MapperScannerConfigurer，加入所有Mapper代理实现类
> 8. 在web.xml对ContextLoaderListener监听器进行注册，并指定Spring配置文件的位置



### Mybatis：

- 持久层框架
- 配置文件可全部整合到Spring配置文件中，也可分开配置
- 日志工具为log4j



### SpringMVC:

- 表述层框架，包括View和Servlet。
- SpringMVC仅对控制层进行扫描。
- SpringMVCIOC容器在SpringIOC容器初始化之后初始化，前者可得到后者的引用，所以可在控制层对Service层进行自动装配。
- SpringMVC和Spring可整合，也可不整合。
- SpringMVC配置文件的位置在web.xml->DispatcherServlet中指定



**在Spring配置文件中必须要做的是：**

> 1. 扫描控制层组件
> 2. 配置视图解析器
> 3. 配置DefaultServlet
> 4. 开启MVC注解驱动
> 5. 配置视图控制器
> 6. 配置文件上传解析器(可选)

## 2、整合配置文件

### pom.xml:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <parent>
        <artifactId>SSM</artifactId>
        <groupId>org.lxw</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.lxw.ssm</groupId>
    <artifactId>ssm_union</artifactId>
    <packaging>war</packaging>
    <name>ssm_union Maven Webapp</name>
    <url>http://maven.apache.org</url>
    <!--自定义属性名 -->
    <properties>
        <spring.version>5.3.1</spring.version>
    </properties>
    <!--spring依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!--springmvc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!--Spring整合jdbc依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- spring切面依赖 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!--spring整合junit依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- Mybatis核心 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.7</version>
        </dependency>
        <!--mybatis和spring的整合包-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.6</version>
        </dependency>
        <!-- 连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.9</version>
        </dependency>
        <!-- junit测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- MySQL驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.16</version>
        </dependency>
        <!-- log4j日志 -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
        <!--分页插件依赖-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.2.0</version>
        </dependency>
        <!-- 日志 -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
        </dependency>
        <!-- ServletAPI -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>
        <!--json解析依赖-->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.12.1</version>
        </dependency>
        <!--文件上传解析依赖-->
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.3.1</version>
        </dependency>
        <!-- Spring5和Thymeleaf整合包 -->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
            <version>3.0.12.RELEASE</version>
        </dependency>
    </dependencies>
    <!--项目名-->
    <build>
        <finalName>ssm_union</finalName>
    </build>
</project>
```

### web.xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

  <!--配置Spring的编码过滤器-->
  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

  <!--配置处理请求方式的过滤器-->
  <filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

  <!--配置SpringMVC的前端控制器DispatcherServlet-->
  <servlet>
    <servlet-name>SpringMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--设置SpringMVC配置文件自定义的位置和名称-->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <!--将DispatcherServlet的初始化时间提前到服务器启动时-->
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>SpringMVC</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

  <!--配置Spring的监听器，在服务器启动时加载Spring的配置文件-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <!--设置Spring配置文件自定义的位置和名称-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring.xml</param-value>
  </context-param>

</web-app>
```



### jdbc.properties:

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC
jdbc.username=root
jdbc.password=abc123
```

### springMVC.xml:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--扫描控制层组件，只扫描控制层组件-->
    <context:component-scan base-package="com.lxw.ssm.controller"></context:component-scan>

    <!--配置视图解析器-->
    <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <property name="order" value="1"/>
        <property name="characterEncoding" value="UTF-8"/>
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
                <property name="templateResolver">
                    <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                        <!-- 视图前缀 -->
                        <property name="prefix" value="/WEB-INF/templates/"/>
                        <!-- 视图后缀 -->
                        <property name="suffix" value=".html"/>
                        <property name="templateMode" value="HTML5"/>
                        <property name="characterEncoding" value="UTF-8" />
                    </bean>
                </property>
            </bean>
        </property>
    </bean>

    <!--配置默认的servlet处理静态资源-->
    <mvc:default-servlet-handler />

    <!--开启mvc的注解驱动-->
    <mvc:annotation-driven />

    <!--配置视图控制器-->
    <mvc:view-controller path="/" view-name="index"></mvc:view-controller>
    <mvc:view-controller path="/employee/to/add" view-name="employee_add"></mvc:view-controller>

    <!--配置文件上传解析器-->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>

</beans>
```



### spring.xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--扫描组件（除控制层）-->
    <context:component-scan base-package="com.lxw.ssm">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <!--引入jdbc.properties-->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>

    <!--配置数据源，在Spring配置文件中配置过就可以在mybatis核心文件中省略-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>

    <!--配置事务管理器，id属性作为开启事务的注解驱动的transaction-manager属性值-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!--
        开启事务的注解驱动
        将使用注解@Transactional标识的方法或类中所有的方法进行事务管理
    -->
    <tx:annotation-driven transaction-manager="transactionManager" />

    <!--配置SqlSessionFactoryBean，可以直接在Spring的IOC中获取SqlSessionFactory-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--设置MyBatis的核心配置文件的路径，若是在该配置中把mybatis核心文件的需要配置的都配置了，
        那么MyBatis的核心配置文件可以省略
        -->
        <property name="configLocation" value="classpath:mybatis-config.xml"></property>
        <!--设置数据源-->
        <property name="dataSource" ref="dataSource"></property>
        <!--设置类型别名所对应的包-->
        <property name="typeAliasesPackage" value="com.lxw.ssm.pojo"></property>
        <!--设置映射文件的路径，只有映射文件的包和mapper接口的包不一致时且没有配置MapperScannerConfigurer时需要设置-->
        <!--<property name="mapperLocations" value="classpath:com/lxw/ssm/mapper/*.xml"></property>-->
    </bean>

    <!--
        配置mapper接口的扫描，可以将指定包下所有的mapper接口
        通过SqlSession创建代理实现类对象，并将这些对象交给IOC容器管理
    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.lxw.ssm.mapper"></property>
    </bean>

</beans>
```



### mybatis-config.xml:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

    <!--
        MyBatis核心配置文件中的标签必须要按照指定的顺序配置：
        properties?,settings?,typeAliases?,typeHandlers?,
        objectFactory?,objectWrapperFactory?,reflectorFactory?,
        plugins?,environments?,databaseIdProvider?,mappers?
    -->

    <settings>
        <!--将下划线映射为驼峰-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

    <plugins>
        <!--配置分页插件-->
        <plugin interceptor="com.github.pagehelper.PageInterceptor">
            <!--设置分页展示的合理性，默认值为false-->
            <property name="reasonable" value="true" />
        </plugin>
    </plugins>

</configuration>
```

### log4j.xml:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">

<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">

    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
        <param name="Encoding" value="UTF-8" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m  (%F:%L) \n" />
        </layout>
    </appender>
    <logger name="java.sql">
        <level value="debug" />
    </logger>
    <logger name="org.apache.ibatis">
        <level value="info" />
    </logger>
    <root>
        <level value="debug" />
        <appender-ref ref="STDOUT" />
    </root>
</log4j:configuration>
```



## 3、SSM-CRUD

### com.lxw.ssm

#### controller

##### EmployeeController.java

```java
/**
 * 查询所有的员工信息-->/employee-->get
 * 查询员工的分页信息-->/employee/page/1-->get
 * 根据id查询员工信息-->/employee/1-->get
 * 跳转到添加页面-->/to/add-->get
 * 添加员工信息-->/employee-->post
 * 修改员工信息-->/employee-->put
 * 删除员工信息-->/employee/1-->delete
 * 注意点：请求头中referer属性，必须时从某个页面发起请求才可获取，手动在地址栏中输入会报：
 * Missing request header 'referer' for method parameter of type String
 * 调试时注意
 */
@Controller
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;

    @RequestMapping(value = "/employee/page/{pageNum}", method = RequestMethod.GET)
    public String getEmployeePage(@PathVariable("pageNum") Integer pageNum, Model model){
        //获取员工的分页信息
        PageInfo<Employee> page = employeeService.getEmployeePage(pageNum);
        //将分页数据共享到请求域中
        model.addAttribute("page", page);
        //跳转到employee_list.html
        return "employee_list";
    }
    @RequestMapping(value = "/employee/lastPage",method = RequestMethod.GET)
    public String ToLastPage(Model model){
        //获取员工的分页信息
        PageInfo<Employee> page = employeeService.toLastPage();
        //将分页数据共享到请求域中
        model.addAttribute("page", page);
        //跳转到employee_list.html
        return "employee_list";
    }

    @RequestMapping(value = "/employee", method = RequestMethod.GET)
    public String getAllEmployee(Model model){
        //查询所有的员工信息
        List<Employee> list = employeeService.getAllEmployee();
        //将员工信息在请求域中共享
        model.addAttribute("list", list);
        //跳转到employee_list.html
        return "all_employee";
    }

    @RequestMapping(value = "/employee/{id}",method = RequestMethod.GET)
    public String getEmployeeById(@PathVariable("id") Integer id,Model model){
        Employee employee=employeeService.getEmployeeById(id);
        model.addAttribute("employee",employee);
        return "employee";
    }

    @RequestMapping(value="/employee",method = RequestMethod.POST)
    public String addEmployee(Employee employee){
        employeeService.addEmployee(employee);
        //添加员工后跳转到最后一页
        return "redirect:/employee/lastPage";
    }

    @RequestMapping(value="/employee",method=RequestMethod.PUT)
    public String updateEmployee(Employee employee, HttpServletRequest request){
        String pageNumStr = request.getParameter("pageNumStr");
        employeeService.upateEmployee(employee);
        return "redirect:/employee/page/"+pageNumStr;
    }

    @RequestMapping(value="employee/to/update/{id}",method = RequestMethod.GET)
    public String toUpdate(@PathVariable("id") Integer id,Model model,@RequestHeader("referer") String referer){
        //获取发起请求的页码pageNumStr,共享在请求域中
        String pageNumStr = referer.substring(referer.lastIndexOf("/")+1);
        Employee employee = employeeService.getEmployeeById(id);
        model.addAttribute("employee",employee);
        model.addAttribute("pageNumStr",pageNumStr);
        return "employee_update";
    }

    @RequestMapping(value="/employee/{id}",method = RequestMethod.DELETE)
    public String deleteEmployeeById(@PathVariable Integer id,@RequestHeader("referer") String referer){
        //获取发起请求的页码pageNumStr,共享在请求域中
        String pageNumStr = referer.substring(referer.lastIndexOf("/")+1);
        employeeService.deleteEmployeeById(id);
        return "redirect:/employee/page/"+pageNumStr;
    }


}
```



#### mapper

##### EmployeeMapper.java

```java
public interface EmployeeMapper {

    /**
     * 查询所有的员工信息
     * @return
     */
    List<Employee> getAllEmployee();

    /**
     * 根据Id查询员工
     * @param id 员工id
     * @return 员工对象
     */
    Employee getEmployeeById(@Param("id") Integer id);

    /**
     * 添加员工
     * @param employee
     */
    void addEmployee(Employee employee);

    /**
     * 更新员工数据
     * @param employee
     */
    void upateEmployee(Employee employee);
    /**
     * 根据id删除员工
     */
    void deleteEmployeeById( @Param("id")Integer id);
}
```

#### pojo

##### Employee.java

```java
public class Employee {

    private Integer empId;

    private String empName;

    private Integer age;

    private String gender;

    private String email;

    public Employee() {
    }

    public Employee(Integer empId, String empName, Integer age, String gender, String email) {
        this.empId = empId;
        this.empName = empName;
        this.age = age;
        this.gender = gender;
        this.email = email;
    }

    public Integer getEmpId() {
        return empId;
    }

    public void setEmpId(Integer empId) {
        this.empId = empId;
    }

    public String getEmpName() {
        return empName;
    }

    public void setEmpName(String empName) {
        this.empName = empName;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "empId=" + empId +
                ", empName='" + empName + '\'' +
                ", age=" + age +
                ", gender='" + gender + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}
```

#### service

##### impl

###### EmployeeService.java

```java
@Service
@Transactional
public class EmployeeServiceImpl implements EmployeeService {

    @Autowired
    private EmployeeMapper employeeMapper;

    @Override
    public List<Employee> getAllEmployee() {
        return employeeMapper.getAllEmployee();
    }

    @Override
    public Employee getEmployeeById(Integer id) {
        Employee employee = employeeMapper.getEmployeeById(id);
        return employee;
    }

    @Override
    public void addEmployee(Employee employee) {
        employeeMapper.addEmployee(employee);
    }

    @Override
    public void deleteEmployeeById(Integer id) {
        employeeMapper.deleteEmployeeById(id);
    }

    @Override
    public void upateEmployee(Employee employee) {
        employeeMapper.upateEmployee(employee);
    }

    @Override
    public PageInfo<Employee> toLastPage() {
        //开启分页功能
        PageHelper.startPage(Integer.MAX_VALUE, 4);
        //查询所有的员工信息
        List<Employee> list = employeeMapper.getAllEmployee();
        //获取分页相关数据
        PageInfo<Employee> page = new PageInfo<>(list, 5);
        return page;
    }

    @Override
    public PageInfo<Employee> getEmployeePage(Integer pageNum) {
        //开启分页功能
        PageHelper.startPage(pageNum, 4);
        //查询所有的员工信息
        List<Employee> list = employeeMapper.getAllEmployee();
        //获取分页相关数据
        PageInfo<Employee> page = new PageInfo<>(list, 5);
        return page;
    }
}
```
##### EmployeeService.java

```java
public interface EmployeeService {

    /**
     * 添加员工
     */
    void addEmployee(Employee employee);

    /**
     * 查询所有的员工信息
     * @return
     */
    List<Employee> getAllEmployee();

    /**
     * 获取员工的分页信息
     * @param pageNum
     * @return
     */
    PageInfo<Employee> getEmployeePage(Integer pageNum);

    /**
     * 根据Id查询员工
     * @param id 员工id
     * @return 员工对象
     */
    Employee getEmployeeById(Integer id);
    /**
     * 跳转到最后一页
     */
    PageInfo<Employee> toLastPage();

    /**
     * 更新员工数据
     */
    void upateEmployee(Employee employee);
    /**
     * 根据Id删除员工
     */
    void deleteEmployeeById(Integer id);
}
```

### resources

#### com.lxw.ssm.mapper

##### EmployeeMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.lxw.ssm.mapper.EmployeeMapper">

    <!--List<Employee> getAllEmployee();-->
    <select id="getAllEmployee" resultType="Employee">
        select * from t_emp
    </select>

    <!--Employee getEmployeeById(Integer id);-->
    <select id="getEmployeeById" resultType="Employee">
        select * from t_emp where emp_id=#{id}
    </select>

    <!--void addEmployee(Employee employee);-->
    <insert id="addEmployee" >
        insert into t_emp values (NULL,#{empName},#{age},#{gender},#{email})
    </insert>

    <!--void upateEmployee(Employee employee);-->
    <update id="upateEmployee" >
        update t_emp set emp_name=#{empName},age=#{age},gender=#{gender},email=#{email} where emp_id=#{empId}
    </update>

    <!--void deleteEmployeeById(Integer id);-->
    <delete id="deleteEmployeeById">
        delete from t_emp where emp_id=#{id}
    </delete>
</mapper>
```

### webapp

#### WEB-INF.templates

##### all_employee.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>员工列表</title>
  <link rel="stylesheet" th:href="@{/static/css/index_work.css}">
</head>
<body>
<table>
  <tr>
    <th colspan="5">员工列表</th>
  </tr>
  <tr>
    <th>流水号</th>
    <th>员工姓名</th>
    <th>年龄</th>
    <th>性别</th>
    <th>邮箱</th>
  </tr>
  <tr th:each="employee,status : ${list}">
    <td th:text="${status.count}"></td>
    <td th:text="${employee.empName}"></td>
    <td th:text="${employee.age}"></td>
    <td th:text="${employee.gender}"></td>
    <td th:text="${employee.email}"></td>
  </tr>
</table>
</body>
</html>
```

##### employee.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>查询员工</title>
    <link rel="stylesheet" th:href="@{/static/css/index_work.css}">
</head>
<body>
<table>
    <tr>
        <th colspan="4">员工列表</th>
    </tr>
    <tr>
        <th>员工姓名</th>
        <th>年龄</th>
        <th>性别</th>
        <th>邮箱</th>
    </tr>
    <tr >
        <td th:text="${employee.empName}"></td>
        <td th:text="${employee.age}"></td>
        <td th:text="${employee.gender}"></td>
        <td th:text="${employee.email}"></td>
    </tr>
</table>
</body>
</html>
```

##### employee_add.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>add employee</title>
    <link rel="stylesheet" th:href="@{/static/css/index_work.css}">
</head>
<body>
<form th:action="@{/employee}" method="post">
    <table>
        <tr>
            <th colspan="2">add employee</th>
        </tr>
        <tr>
            <td>empName</td>
            <td>
                <input type="text" name="empName">
            </td>
        </tr>
        <tr>
            <td>age</td>
            <td>
                <input type="text" name="age">
            </td>
        </tr>
        <tr>
            <td>gender</td>
            <td>
                <input type="radio" name="gender" value="男">male
                <input type="radio" name="gender" value="女">female
            </td>
        </tr>
        <tr>
            <td>email</td>
            <td>
                <input type="text" name="email">
            </td>
        </tr>
        <tr>
            <td colspan="2">
                <input type="submit" value="add">
            </td>
        </tr>
    </table>
</form>
</body>
</html>
```

##### employee_list.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>员工列表</title>
    <link rel="stylesheet" th:href="@{/static/css/index_work.css}">
</head>
<body>
<table>
    <tr>
        <th colspan="6">员工列表</th>
    </tr>
    <tr>
        <th>流水号</th>
        <th>员工姓名</th>
        <th>年龄</th>
        <th>性别</th>
        <th>邮箱</th>
        <th>操作</th>
    </tr>
    <tr th:each="employee,status : ${page.list}">
        <td th:text="${status.count}"></td>
        <td th:text="${employee.empName}"></td>
        <td th:text="${employee.age}"></td>
        <td th:text="${employee.gender}"></td>
        <td th:text="${employee.email}"></td>
        <td>
            <a th:href="@{'/employee/'+${employee.empId}}" id="delete">删除</a>
            <a th:href="@{'/employee/to/update/'+${employee.empId}}" id="update">修改</a>
        </td>
    </tr>
</table>

<form method="post">
    <input type="hidden" name="_method" value="delete">
</form>
<div style="text-align: center;">
    <a th:if="${page.hasPreviousPage}" th:href="@{/employee/page/1}">首页</a>
    <a th:if="${page.hasPreviousPage}" th:href="@{'/employee/page/'+${page.prePage}}">上一页</a>
    <span th:each="num : ${page.navigatepageNums}">
        <a th:if="${page.pageNum == num}" style="color: red;" th:href="@{'/employee/page/'+${num}}" th:text="'['+${num}+']'"></a>
        <a th:if="${page.pageNum != num}" th:href="@{'/employee/page/'+${num}}" th:text="${num}"></a>
    </span>
    <a th:if="${page.hasNextPage}" th:href="@{'/employee/page/'+${page.nextPage}}">下一页</a>
    <a th:if="${page.hasNextPage}" th:href="@{'/employee/page/'+${page.pages}}">末页</a>
</div>
</body>
<script>
    window.onload=function(){
        var table=document.getElementsByTagName("table")[0];
        var form=document.getElementsByTagName("form")[0];
        table.onclick=function (){
            if(event.target.id=="delete"){
                form.action=event.target.href;
                form.submit();
            }
            //这里必须对修改进行判断，否则返回false会取消超链接的默认行为
            if(event.target.id=="update"){
                return true;
            }
            return false;
        }
    }
</script>
</html>
```

##### employee_update.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>add employee</title>
  <link rel="stylesheet" th:href="@{/static/css/index_work.css}">
</head>
<body>
<form th:action="@{'/employee?pageNumStr='+${pageNumStr}}" method="post">
  <input type="hidden" name="_method" value="put">
  <input type="hidden" name="empId" th:value="${employee.empId}">
  <table>
    <tr>
      <th colspan="2">add employee</th>
    </tr>
    <tr>
      <td>empName</td>
      <td>
        <input type="text" name="empName" th:value="${employee.empName}">
      </td>
    </tr>
    <tr>
      <td>age</td>
      <td>
        <input type="text" name="age" th:value="${employee.age}">
      </td>
    </tr>
    <tr>
      <td>gender</td>
      <td>
        <input type="radio" name="gender" value="男" th:field="${employee.gender}">male
        <input type="radio" name="gender" value="女" th:field="${employee.gender}">female
      </td>
    </tr>
    <tr>
      <td>email</td>
      <td>
        <input type="text" name="email" th:value="${employee.email}">
      </td>
    </tr>
    <tr>
      <td colspan="2">
        <input type="submit" value="update" >
      </td>
    </tr>
  </table>
</form>
</body>
</html>
```

##### index.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
    <script>
        window.onload=function(){
            var button =document.getElementById("button");
            button.onclick=function(){
                var input=document.getElementById("selectById");
                var form=document.getElementById("form");
                form.action="/ssm/employee/"+input.value;
                form.submit();
            }
        }
    </script>
</head>
<body>
<h1>index.html</h1>

<a th:href="@{/employee/}">查询所有员工的信息</a><br/>
<a th:href="@{/employee/page/1}">查询员工的分页信息</a>

<form method="get" id="form">
    <input type="text" id="selectById" >
    <button id="button">根据Id查询员工</button>
</form>

<a th:href="@{/employee/to/add}">增加新员工</a>
</body>
</html>
```