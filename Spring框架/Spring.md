# 基础

## Spring是什么

一句话概括：**Spring是一个轻量级、非入侵式的控制反转(IoC)和面向切面(AOP)的框架。**



## Spring有哪些特性

**1. 控制反转和依赖注入的支持**

Spring的核心是一个大的工厂容器，可以维护所有对象的创建和依赖关系，Spring工厂用于生成Bean，并且管理Bean的生命周期，实现**高内聚低耦合**的设计理念。

**2. AOP编程的支持**

Spring提供了**面向切面编程**，可以方便的实现对程序进行权限拦截、运行监控等切面功能。

**3. 声明式事务的支持**

支持通过配置来完成对事务的管理，而不需要通过硬编码的方式，以前重复的一些事务提交、回滚的JDBC代码，都可以不用自己写了

**4. 快捷测试的支持**

Spring对JUnit提供支持，可以通过注解快捷地测试Spring程序

**5. 快速集成功能**

方便集成各种优秀框架，Spring 不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如：Struts、Hibernate、MyBatis、Quartz 等）的直接支持。

**6. 复杂API模板封装**

Spring 对 JavaEE 开发中非常难用的一些 API（JDBC、JavaMail、远程调用等）都提供了模板化的封装，这些封装 API 的提供使得应用难度大大降低。



## 简单说一下什么是AOP和IoC

**AOP**：面向切面编程，是一种编程范式，主要作用是将那些<u>与核心业务无关，但是对多个对象产生影响</u>的公共行为封装起来，如日志记录、性能、事务等。

**IoC**：控制反转，是一种设计思想，主要作用是将对象的创建和对象之间的调用过程交给Spring容器来管理



## Spring有哪些模块呢

Spring 框架是分模块存在，除了最核心的`Spring Core Container`是必要模块之外，其他模块都是`可选`

![Spring模块划分](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-bb7c13ea-3174-4b32-84b8-821849ddc377.png)

1. **Spring Core**：Spring 核心，它是框架最基础的部分，提供 IoC 和依赖注入 DI 特性。
2. **Spring Context**：Spring 上下文容器，它是 BeanFactory 功能加强的一个子接口。
3. **Spring Web**：它提供 Web 应用开发的支持。
4. **Spring MVC**：它针对 Web 应用中 MVC 思想的实现。
5. **Spring DAO**：提供对 JDBC 抽象层，简化了 JDBC 编码，同时，编码更具有健壮性。
6. **Spring ORM**：它支持用于流行的 ORM 框架的整合，比如：Spring + Hibernate、Spring + iBatis、Spring + JDO 的整合等。
7. **Spring AOP**：即面向切面编程，它提供了与 AOP 联盟兼容的编程实现。



## Spring有哪些常用注解呢

Spring 提供了大量的注解来简化 Java 应用的开发和配置，主要用于 Web 开发、往容器注入 Bean、AOP、事务控制等。

<img src="https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-8d0a1518-a425-4887-9735-45321095d927.png" alt="三分恶面渣逆袭：Spring常用注解" style="zoom:50%;" />

**Web开发常用注解**

- `@Controller`：用于标注控制层组件
- `@RestController`：是`@Controller`和`@ResponseBody`的结合体，返回JSON数据时使用
- `@RequestMapping`：用于映射请求 URL 到具体的方法上，还可以细分为：
  - `@GetMapping`：只能用于处理 GET 请求
  - `@PostMapping`：只能用于处理 POST 请求
  - `@DeleteMapping`：只能用于处理 DELETE 请求

- `@ResponseBody`：直接将返回的数据放入 HTTP 响应正文中，一般用于返回 JSON 数据。
- `@RequestBody`：表示一个方法参数应该绑定到 Web 请求体。
- `@PathVariable`：用于接收路径参数，比如 `@RequestMapping(“/hello/{name}”)`，这里的 name 就是路径参数。
- `@RequestParam`：用于接收请求参数。比如 `@RequestParam(name = "key") String key`，这里的 key 就是请求参数。

**容器类有哪些注解**

- `@Component`：标识一个类为 Spring 组件，使其能够被 Spring 容器自动扫描和管理。
- `@Service`：标识一个业务逻辑组件（服务层）。比如 `@Service("userService")`，这里的 userService 就是 Bean 的名称。
- `@Repository`：标识一个数据访问组件（持久层）
- `@Autowired`：按类型自动注入依赖
- `@Configuration`：用于定义配置类，可替换 XML 配置文件
- `@Value`：用于将 Spring Boot 中 application.yml 配置的属性值赋值给变量
- `@Bean`：**适用于注册无法修改源码的第三方类**（如 `DataSource`、`ObjectMapper`）

**AOP有哪些注解**

- `@Aspect`用于声明一个切面，可以配合其它注解一起使用，比如：
  - `@Before`：在方法执行之前执行
  - `@After`：在方法执行之后执行
  - `@Around`：方法执行前后均执行
  - `@PointCut`：定义切点，指定需要拦截的方法

**事务注解有哪些**

- `@Transactional`：用于声明一个方法需要事务支持



## Spring中应用了哪些设计模式

**1. 工厂模式用于BeanFactory和ApplicationContext，实现Bean的创建和管理**

```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
MyBean myBean = context.getBean(MyBean.class);
```

**2. 单例模式，这样可以保证Bean的唯一性，减少系统开销**

```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
MyService myService1 = context.getBean(MyService.class);
MyService myService2 = context.getBean(MyService.class);

// This will print "true" because both references point to the same instance
System.out.println(myService1 == myService2);
```

**3. AOP使用了代理模式来实现横切关注点**

```java
@Transactional
public void myTransactionalMethod() {
    // 方法实现
}
```



## Spring如何实现单例模式

Spring通过IOC容器实现单例模式，具体步骤是：

- 单例 Bean 在容器初始化时创建并使用 DefaultSingletonBeanRegistry 提供的 singletonObjects 进行缓存。

```java
// 单例缓存
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<>();

public Object getSingleton(String beanName) {
    return this.singletonObjects.get(beanName);
}

protected void addSingleton(String beanName, Object singletonObject) {
    this.singletonObjects.put(beanName, singletonObject);
}
```

- 在请求Bean时，Spring会先从缓存中获取



## Spring容器、Web容器之间的区别

Spring 容器管理业务逻辑（`@Service`、`@Repository`），Web 容器（Tomcat等）管理 HTTP 请求，SpringMVC 容器管理 Web 控制层（`@Controller`），SpringMVC 需要 Web 容器运行，并依赖 Spring 容器获取 Service 层的 Bean。



# 控制反转和依赖注入

## 什么是IoC、DI

**什么是IoC**

所谓的**IoC**，就是由容器来控制对象的生命周期和对象之间的关系。控制对象生命周期的不再是引用它的对象，而是容器，这就叫**控制反转**（Inversion of Control）。所有类的创建和销毁都通过 Spring 容器来，不再是开发者去 new，去 `= null`，这样就实现了对象的解耦。

![三分恶面渣逆袭：控制反转示意图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-440f5d0e-f4db-462c-97fb-d54407a354d5.png)

**什么是DI**

IoC 是一种思想，DI 是实现 IoC 的具体方式，比如说利用注入机制（如构造器注入、Setter 注入）将依赖传递给目标对象。



## **有自己实现过简单的 Spring 吗**

**1. beans.properties**

Bean通过一个配置文件定义，把它解析成一个类型

```properties
userDao:cn.fighter3.bean.UserDao
```

**2. BeanDefinition.java**

bean定义类，配置文件中bean定义对应的实体

```java
public class BeanDefinition {

    private String beanName;

    private Class beanClass;
     //省略getter、setter
 }
```

**3. ResourceLoader.java**

资源加载器，用来完成配置文件中配置的加载

```java
public class ResourceLoader {

    public static Map<String, BeanDefinition> getResource() {
        Map<String, BeanDefinition> beanDefinitionMap = new HashMap<>(16);
        Properties properties = new Properties();
        try {
            InputStream inputStream = ResourceLoader.class.getResourceAsStream("/beans.properties");
            properties.load(inputStream);
            Iterator<String> it = properties.stringPropertyNames().iterator();
            while (it.hasNext()) {
                String key = it.next();
                String className = properties.getProperty(key);
                BeanDefinition beanDefinition = new BeanDefinition();
                beanDefinition.setBeanName(key);
                Class clazz = Class.forName(className);
                beanDefinition.setBeanClass(clazz);
                beanDefinitionMap.put(key, beanDefinition);
            }
            inputStream.close();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
        return beanDefinitionMap;
    }

}
```

**BeanRegister.java**

对象注册器，这里用于单例 bean 的缓存，大幅简化，默认所有 bean 都是单例的。可以看到所谓单例注册，也很简单，不过是往 HashMap 里存对象。

```java
public class BeanRegister {

    //单例Bean缓存
    private Map<String, Object> singletonMap = new HashMap<>(32);

    /**
     * 获取单例Bean
     *
     * @param beanName bean名称
     * @return
     */
    public Object getSingletonBean(String beanName) {
        return singletonMap.get(beanName);
    }

    /**
     * 注册单例bean
     *
     * @param beanName
     * @param bean
     */
    public void registerSingletonBean(String beanName, Object bean) {
        if (singletonMap.containsKey(beanName)) {
            return;
        }
        singletonMap.put(beanName, bean);
    }

}
```

**BeanFactory**

对象工厂，我们最**核心**的一个类，在它初始化的时候，创建了 bean 注册器，完成了资源的加载。

获取 bean 的时候，先从单例缓存中取，如果没有取到，就创建并注册一个 bean

```java
public class BeanFactory {

    private Map<String, BeanDefinition> beanDefinitionMap = new HashMap<>();

    private BeanRegister beanRegister;

    public BeanFactory() {
        //创建bean注册器
        beanRegister = new BeanRegister();
        //加载资源
        this.beanDefinitionMap = new ResourceLoader().getResource();
    }

    /**
     * 获取bean
     *
     * @param beanName bean名称
     * @return
     */
    public Object getBean(String beanName) {
        //从bean缓存中取
        Object bean = beanRegister.getSingletonBean(beanName);
        if (bean != null) {
            return bean;
        }
        //根据bean定义，创建bean
        return createBean(beanDefinitionMap.get(beanName));
    }

    /**
     * 创建Bean
     *
     * @param beanDefinition bean定义
     * @return
     */
    private Object createBean(BeanDefinition beanDefinition) {
        try {
            Object bean = beanDefinition.getBeanClass().newInstance();
            //缓存bean
            beanRegister.registerSingletonBean(beanDefinition.getBeanName(), bean);
            return bean;
        } catch (InstantiationException | IllegalAccessException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```



## 说说BeanFactory和ApplicationContext

可以这么比喻，BeanFactory 是 Spring 的“心脏”，而 ApplicantContext 是 Spring 的完整“身躯”。

- BeanFactory 主要负责配置、创建和管理 bean，为 Spring 提供了基本的依赖注入（DI）支持。
- ApplicationContext 是 BeanFactory 的子接口，在 BeanFactory 的基础上添加了企业级的功能支持。



**详细说说BeanFactory**

BeanFactory 位于整个 Spring IoC 容器的顶端，ApplicationContext 算是 BeanFactory 的子接口。

![三分恶面渣逆袭：Spring5 BeanFactory继承体系](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-6e6d4b69-f36c-41e6-b8ba-9277be147c9b.png)

它最主要的方法就是 `getBean()`，这个方法负责从容器中返回特定名称或者类型的 Bean 实例。



**详细说说ApplicaitonContext**

ApplicationContext 继承了 HierachicalBeanFactory 和 ListableBeanFactory 接口，算是 BeanFactory 的自动挡版本，是 Spring 应用的默认方式。

ApplicationContext 会在启动时预先创建和配置所有的单例 bean，并支持如 JDBC、ORM 框架的集成，内置面向切面编程（AOP）的支持，可以配置声明式事务管理等。



**二者区别**

- 都是容器，因为都提供了`getBean`方法
- 父子关系，ApplicationContext继承了BeanFactory
- ApplicationContext直接面向用户提供了很多功能，如扫描配置信息、事件监听、注册Bean的后置处理器等等。BeanFactory只有创建Bean一个功能



## Spring的容器启动阶段会做什么

Spring 的 IoC 容器工作的过程，其实可以划分为两个阶段：**容器启动阶段**和**Bean 实例化阶段**。

容器启动开始，首先会通过某种途径加载 Configuration MetaData，在大部分情况下，容器需要依赖某些工具类（BeanDefinitionReader）对加载的 Configuration MetaData 进行解析和分析，并将分析后的信息组为相应的 BeanDefinition。

![xml配置信息映射注册过程](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-dfb3d8c4-ba8d-4a2c-aef2-4ad425f7180c.png)

最后把这些保存了 Bean 定义必要信息的 BeanDefinition，注册到相应的 BeanDefinitionRegistry，这样容器启动就完成了。



## Spring的Bean实例化阶段会做什么

![容器启动和Bean实例化阶段](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-8f8103f7-2a51-4858-856e-96a4ac400d76.png)



## 四种Bean实例化的方式

**1. 基于XML配置（applicationContext.xml）**

- applicationContext.xml

  ```xml
  <bean id="userService" class="com.example.service.UserService"/>
  ```

- 获取Bean

  ```java
  ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
  UserService userService = (UserService) context.getBean("userService");
  ```

**2. 基于配置类**

- AppConfig.java

  ```java
  @Configuration
  public class AppConfig {
      @Bean
      public UserService userService() {
          return new UserService();
      }
  }
  ```

- 获取Bean

  ```java
  ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
  UserService userService = context.getBean(UserService.class);
  ```

**3. 基于注解的方式**

 使用 `@Component`（或 `@Service`、`@Repository`）注解自动扫描 Bean。

**4. 使用FactoryBean方式**

- 使用 `FactoryBean` 可以控制 Bean 的实例化逻辑，而不是直接返回类的实例。

  ```java
  import org.springframework.beans.factory.FactoryBean;
  import org.springframework.stereotype.Component;
  
  @Component
  public class UserServiceFactoryBean implements FactoryBean<UserService> {
      @Override
      public UserService getObject() throws Exception {
          System.out.println("Creating UserService instance...");
          return new UserService();
      }
  
      @Override
      public Class<?> getObjectType() {
          return UserService.class;
      }
  
      @Override
      public boolean isSingleton() {
          return true;  // 是否单例
      }
  }
  
  ```



## 你是怎么理解Bean的

Bean 是指由 Spring 容器管理的对象，它的生命周期由容器控制，包括创建、初始化、使用和销毁。以通过三种方式声明：**注解方式**、**XML 配置**、**Java 配置**。

①、使用 `@Component`、`@Service`、`@Repository`、`@Controller` 等注解定义，主流。

②、基于 XML 配置，Spring Boot 项目已经不怎么用了。

③、使用 Java 配置类创建 Bean：



## @Component和@Bean的区别

`@Component` 是 Spring 提供的一个**类级别注解**，由 Spring 自动扫描并注册到 Spring 容器中。

`@Bean` 是一个**方法级别注解**，用于显式地声明一个 Bean，当我们需要第三方库或者无法使用 `@Component` 注解类时，可以使用 `@Bean` 来将其实例注册到容器中。

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import javax.sql.DataSource;
import com.zaxxer.hikari.HikariDataSource;

@Configuration
public class DatabaseConfig {
    @Bean
    public DataSource dataSource() {
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/mydb");
        dataSource.setUsername("root");
        dataSource.setPassword("password");
        return dataSource;
    }
}
```



## 能说一下Bean的生命周期吗

![三分恶面渣逆袭：Bean生命周期五个阶段](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-595fce5b-36cb-4dcb-b08c-8205a1e98d8a.png)

**1. 实例化**

- Spring 首先使用构造方法或者工厂方法创建一个 Bean 的实例。
- 在这个阶段，Bean 只是一个空的 Java 对象，还未设置任何属性。

**2. 属性赋值**

- Spring 将配置文件中的属性值或依赖的 Bean 注入到该 Bean 中。
- 这个过程称为依赖注入，确保 Bean 所需的所有依赖都被注入。

**3. 初始化**

- Spring 调用 afterPropertiesSet 方法，或通过配置文件指定的 init-method 方法，完成初始化。

**4. 使用中**

- Bean 准备好可以使用了。

**5. 销毁**

- 在容器关闭时，Spring 会调用 destroy 方法，完成 Bean 的清理工作。



**源码角度**

![三分恶面渣逆袭：Spring Bean生命周期](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-942a927a-86e4-4a01-8f52-9addd89642ff.png)

可以在源码 `AbstractAutowireCapableBeanFactory` 中的 `doCreateBean` 方法中，看到 Bean 的前三个生命周期：

```java
protected Object doCreateBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args) throws BeanCreationException {
    BeanWrapper instanceWrapper = null;
    if (mbd.isSingleton()) {
        instanceWrapper = (BeanWrapper)this.factoryBeanInstanceCache.remove(beanName);
    }

    if (instanceWrapper == null) {
        // 实例化阶段
        instanceWrapper = this.createBeanInstance(beanName, mbd, args);
    }

    ...

    Object exposedObject = bean;

    try {
        // 属性赋值阶段
        this.populateBean(beanName, mbd, instanceWrapper);
        // 初始化阶段
        exposedObject = this.initializeBean(beanName, exposedObject, mbd);
    } catch (Throwable var18) {
        ...
    }

    ...
}
```

![三分恶面渣逆袭：Bean生命周期源码追踪](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-d2da20a3-08d0-4648-b9a3-2fff8512b159.png)

## Aware类型的接口有什么用

Spring 提供了一系列 `Aware` 接口，用于让 Bean 感知 Spring 容器的某些特性。实现 `Aware` 接口的 Bean 可以获取 Spring 容器中的相关对象，例如 `ApplicationContext`、`BeanFactory`、`Environment` 等

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
import org.springframework.stereotype.Component;

@Component
public class SpringContextUtil implements ApplicationContextAware {
    private static ApplicationContext context;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) {
        context = applicationContext;
    }

    public static <T> T getBean(Class<T> beanClass) {
        return context.getBean(beanClass);
    }
}
```

