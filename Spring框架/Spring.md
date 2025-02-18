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

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b432daf7ac26554b8b1d4e00918ca36f.png)**1. 资源加载**

- 解析`applicationContext.xml`或Java配置类(`@Configuration`)

- 读取并解析`@ComponentScan`扫描的包路径，找到符合条件的`@Component`、`@Service`、`@Repository`等Bean定义。

**2. Bean定义注册**

- 解析`@Bean` 方法、XML 配置、`@Import`、`@ComponentScan` 等，将所有 **BeanDefinition** 存入 **BeanDefinitionMap**（存放 Bean 配置信息）。

**3. BeanFactory创建&后处理**

- 实例化 `BeanFactory`（通常是 `DefaultListableBeanFactory`）。



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



## 为什么IDEA不推荐使用@Autowired注解注入Bean

**使用@Autowired的缺点**

- 不能使用final修饰字段，导致对象不可变性丧失
  - 当使用 字段注入 时，Spring 是通过反射来填充依赖的，先创建外层对象，再通过反射填充外层对象需要依赖的Bean
  - 但 Spring 通过反射注入 `@Autowired` 依赖时，是在对象创建后才填充依赖的，而 `final` 变量在对象创建完成后就不能再被修改了。
- 隐藏了类的依赖关系，使得代码可读性变差，调用者无法在类的构造方法或方法签名中看到它



## @Autowired和@Resource的区别

- `@Autowired` 是 Spring 提供的注解，按类型（byType）注入。
- `@Resource` 是 Java EE 提供的注解，按名称（byName）注入。



## 提到了byType，如果两个类型一致的发生了冲突，应该怎么处理

当容器中存在多个相同类型的 bean，编译器会提示 `Could not autowire. There is more than one bean of 'UserRepository2' type.`

```java
@Component
public class UserRepository21 implements UserRepository2 {} 

@Component
public class UserRepository22 implements UserRepository2 {} 

@Component
public class UserService2 {
    @Autowired
    private UserRepository2 userRepository; // 冲突 ❌
}

```



**解决方法1：使用`@Primary`指定默认实现**

```java
@Component
@Primary
public class UserRepository21 implements UserRepository2 {} 
```



**解决方法2：使用`@Qualifier`制定具体的实现**

```java
@Component
public class UserService2 {
    @Autowired
    @Qualifier("userRepository22") // 指定要注入 UserRepository22
    private UserRepository2 userRepository;
}
```

**注意：** `@Qualifier` 里的名称 默认是 `@Component` 类名（首字母小写），所以这里 `"userRepository22"` 对应的是 `UserRepository22` 的 Bean 名称。



**解决方法3：使用`@Resource`注解按名称注入，指定name属性**



## 什么是自动装配

自动装配是 Spring 通过 依赖注入，自动将所需的 Bean 注入到类的属性、构造方法或 setter 方法，避免手动实例化和管理对象。



## Spring有哪些自动装配的方式

Spring提供了四种自动装配类型：

- byName
  - 根据名称进行自动匹配，假设 Boss 有一个名为 car 的属性，如果容器中刚好有一个名为 car 的 bean，Spring 就会自动将其装配给 Boss 的 car 属性
- byType
  - 根据类型进行自动匹配，假设 Boss 有一个 Car 类型的属性，如果容器中刚好有一个 Car 类型的 Bean，Spring 就会自动将其装配给 Boss 这个属性
- constructor
  - 与 byType 类似， 只不过它是针对构造函数注入而言的。如果 Boss 有一个构造函数，构造函数包含一个 Car 类型的入参，如果容器中有一个 Car 类型的 Bean，则 Spring 将自动把这个 Bean 作为 Boss 构造函数的入参；如果容器中没有找到和构造函数入参匹配类型的 Bean，则 Spring 将抛出异常。
- autodetect
  - 优先使用构造方法自动装配（类似于 constructor）。
  - 如果没有匹配的构造方法，就回退到 byType进行属性自动装配。



## Bean的作用域有哪些

**1. Singleton**

在 Spring 中，Bean 默认是单例的，即在整个 Spring 容器中，每个 Bean 只有一个实例。

**2. Prototype**

可以通过在配置中指定 scope 属性，将 Bean 改为多例（Prototype）模式，这样每次获取的都是新的实例。

```java
@Bean
@Scope("prototype")  // 每次获取都是新的实例
public MyBean myBean() {
    return new MyBean();
}
```

**3. Request**

每一次 HTTP 请求都会产生一个新的 Bean，该 Bean 仅在当前 HTTP Request 内有效。

**4. Session**

同一个 Session 共享一个 Bean，不同的 Session 使用不同的 Bean。

**5.application**

整个 Web 应用(可能有多个Spring容器)共享一个实例



## Spring中的单例Bean会存在线程安全问题吗

Spring Bean 的默认作用域是单例（Singleton），这意味着 Spring 容器中只会存在一个 Bean 实例，并且该实例会被多个线程共享。

- 如果单例 Bean 是无状态的，也就是没有成员变量，那么这个单例 Bean 是线程安全的。比如 Spring MVC 中的 Controller、Service、Dao 等，基本上都是无状态的。

- 但如果 Bean 的内部状态是可变的，且没有进行适当的同步处理，就可能出现线程安全问题。



## 单例Bean的线程安全问题怎么解决

1. 使用局部变量。局部变量是线程安全的，因为每个线程都有自己的局部变量副本。尽量使用局部变量而不是共享的成员变量。

2. 尽量使用无状态的 Bean，即不在 Bean 中保存任何可变的状态信息。

3. 同步访问。如果 Bean 中确实需要保存可变状态，可以通过 synchronized 关键字或者 Lock 接口来保证线程安全。

   ```java
   public class MyService {
       private int sharedVar;
   
       public synchronized void increment() {
           sharedVar++;
       }
   }
   ```

4. 或者将 Bean 中的成员变量保存到 ThreadLocal 中，ThreadLocal 可以保证多线程环境下变量的隔离。

   ```java
   public class MyService {
       private ThreadLocal<Integer> localVar = ThreadLocal.withInitial(() -> 0);
   
       public void process() {
           localVar.set(localVar.get() + 1);
       }
   }
   ```

5. 再或者使用线程安全的工具类，比如说 AtomicInteger、ConcurrentHashMap、CopyOnWriteArrayList 等。

   ```java
   public class MyService {
       private ConcurrentHashMap<String, String> map = new ConcurrentHashMap<>();
   
       public void putValue(String key, String value) {
           map.put(key, value);
       }
   }
   ```

6. 将 Bean 定义为原型作用域（Prototype）。原型作用域的 Bean 每次请求都会创建一个新的实例，因此不存在线程安全问题。



## 说说循环依赖

A 依赖 B，B 依赖 A，或者 C 依赖 C，就成了循环依赖。

<img src="https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-f8fea53f-56fa-4cca-9199-ec7f648da625.png" alt="三分恶面渣逆袭：Spring循环依赖" style="zoom:50%;" />

循环依赖只发生在 Singleton 作用域的 Bean 之间，因为如果是 Prototype 作用域的 Bean，Spring 会直接抛出异常。



## Spring 可以解决哪些情况的循环依赖

- AB 均采用构造器注入，不支持
- AB 均采用 setter 注入，支持
- AB 均采用属性自动注入，支持
- A 中注入的 B 为 setter 注入，B 中注入的 A 为构造器注入，支持
- B 中注入的 A 为 setter 注入，A 中注入的 B 为构造器注入，不支持

第四种可以，第五种不可以的原因是 Spring 在创建 Bean 时默认会根据自然排序进行创建，所以 A 会先于 B 进行创建。



## Spring怎么解决循环依赖

Spring 通过三级缓存机制来解决循环依赖：

![三分恶面渣逆袭：三级缓存](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-01d92863-a2cb-4f61-8d8d-30ecf0279b28.png)

1. 一级缓存：存放完全初始化好的单例 Bean。
2. 二级缓存：存放正在创建但未完全初始化的 Bean 实例。
3. 三级缓存：存放 Bean 工厂对象，可以用过getObject方法获取代理对象



## 为什么必须要有三级缓存

为了实现AOP

- 只使用二级缓存可以解决循环依赖的问题，但会导致AOP代理失效，因为二级缓存存的是原始对象，而不是增强过的代理对象
- 为了确保获取到的是正确的代理对象，Spring引入三级缓存
  - 不是直接存Bean，而是存一个ObjectFactory对象，当需要Bean时再调用getObject方法获取代理对象



## @Autowired的实现原理

它的底层实现基于 Spring 的 `BeanPostProcessor` 机制，具体使用 `AutowiredAnnotationBeanPostProcessor` 来完成依赖注入

1. Spring 启动时，扫描所有 `BeanPostProcessor`，并注册 `AutowiredAnnotationBeanPostProcessor`。

2. Spring 依次创建 Bean：

   - 先实例化 Bean（但此时字段值为 null）

   - 放入二级缓存，避免循环依赖问题
   - 执行 `populateBean()` 填充属性

3. `AutowiredAnnotationBeanPostProcessor` 执行，解析 `@Autowired` 注解：
   - 通过 反射 查找 Bean 依赖项
   - 从 Spring 容器中获取匹配的 Bean
   - 注入到目标对象中

4. 所有 Bean 创建完成后，Spring 移除二级缓存，放入一级缓存（单例池）。



# AOP

## 什么是AOP

AOP，也就是面向切面编程，简单点说，AOP 就是把一些业务逻辑中的相同代码抽取到一个独立的模块中，让业务逻辑更加清爽。



AOP的核心概念：

- Aspect（切面）：
  - 本质上是一个类，包含了切点（Pointcut）和增强逻辑（Advice）
- Advice（通知）
  - 通知 是增强逻辑，用于在方法执行的 不同阶段 执行额外代码。

- Pointcut（切点）
  - 用于指定哪些方法需要被AOP增强
- JoinPoint（连接点）
  - 连接点指的是可以被AOP代理的方法，Spring仅支持方法级别代理
- Weaving（织入）
  - 织入是指将切面逻辑应用到目标对象的过程



## AOP有哪些环绕方式

AOP 一般有 **5 种**环绕方式：

- 前置通知 (@Before)
- 返回通知 (@AfterReturning)
- 异常通知 (@AfterThrowing)
- 后置通知 (@After)
- 环绕通知 (@Around)

![image-20250217124232935](C:/Users/shiyu/AppData/Roaming/Typora/typora-user-images/image-20250217124232935.png)

## Spring AOP发生在什么时候

Spring AOP 基于运行时代理机制，这意味着 Spring AOP 是**在运行时通过动态代理生成的**，而不是在编译时或类加载时生成的。

在 Spring 容器初始化 Bean 的过程中，Spring AOP 会检查 Bean 是否需要应用切面。如果需要，Spring 会为该 Bean 创建一个代理对象，并在代理对象中织入切面逻辑。这一过程发生在 **Spring 容器的后处理器（BeanPostProcessor）阶段。**



## AOP的使用场景有哪些

AOP 的使用场景有很多，比如说日志记录、事务管理、权限控制、性能监控等。



## 说说JDK动态代理和CGLIB代理

**JDK动态代理**

JDK 动态代理是基于接口的代理，只能代理实现了接口的类。

```java
public interface Service {
    void perform();
}

public class ServiceImpl implements Service {
    public void perform() {
        System.out.println("Performing service...");
    }
}

public class ServiceInvocationHandler implements InvocationHandler {
    private Object target;

    public ServiceInvocationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before method");
        Object result = method.invoke(target, args);
        System.out.println("After method");
        return result;
    }
}

public class Main {
    public static void main(String[] args) {
        Service service = new ServiceImpl();
        Service proxy = (Service) Proxy.newProxyInstance(
            service.getClass().getClassLoader(),
            service.getClass().getInterfaces(),
            new ServiceInvocationHandler(service)
        );
        proxy.perform();
    }
}
```



**CGLIB动态代理**

CGLIB 动态代理是基于继承的代理，可以代理没有实现接口的类。

```java
public class Service {
    public void perform() {
        System.out.println("Performing service...");
    }
}

public class ServiceInterceptor implements MethodInterceptor {
    @Override
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
        System.out.println("Before method");
        Object result = proxy.invokeSuper(obj, args);
        System.out.println("After method");
        return result;
    }
}

public class Main {
    public static void main(String[] args) {
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(Service.class);
        enhancer.setCallback(new ServiceInterceptor());

        Service proxy = (Service) enhancer.create();
        proxy.perform();
    }
}
```



**选择CGLIB还是JDK动态代理**

- 如果目标对象没有实现任何接口，则只能使用 CGLIB 代理。如果目标对象实现了接口，通常首选 JDK 动态代理。
- 虽然 CGLIB 在代理类的生成过程中可能消耗更多资源，但在运行时具有较高的性能。对于性能敏感且代理对象创建频率不高的场景，可以考虑使用 CGLIB。
- JDK 动态代理是 Java 原生支持的，不需要额外引入库。而 CGLIB 需要将 CGLIB 库作为依赖加入项目中。



## AOP和反射的区别

- 反射：通过类的字节码（Class）对象在运行时动态获取类的信息。用于检查和操作类的方法和字段，动态调用方法或访问字段。反射是 Java 提供的内置机制，直接操作类对象。

- 动态代理：通过生成代理类来拦截方法调用，通常用于 AOP 实现。动态代理使用反射来调用被代理的方法。



## Spring AOP和AspectJ AOP的区别

Spring AOP 属于**运行时增强**，主要具有如下特点：

1. 基于动态代理来实现，默认如果使用接口的，用 JDK 提供的动态代理实现，如果是方法则使用 CGLIB 实现
2. Spring AOP 需要依赖 IoC 容器来管理，并且只能作用于 Spring 容器，使用纯 Java 代码实现
3. 在性能上，由于 Spring AOP 是基于**动态代理**来实现的，**在容器启动时需要生成代理实例**，在方法调用上也会增加栈的深度，使得 Spring AOP 的性能不如 AspectJ 的那么好。
4. Spring AOP 致力于解决企业级开发中最普遍的 AOP(方法织入)。



AspectJ 是一个易用的功能强大的 AOP 框架，属于**编译时增强**， 可以单独使用，也可以整合到其它框架中，是 AOP 编程的完全解决方案。AspectJ 需要用到单独的编译器 ajc。

AspectJ 属于**静态织入**，通过修改代码来实现，在实际运行之前就完成了织入，所以说它**生成的类是没有额外运行时开销的**，一般有如下几个织入的时机：

1. 编译期织入（Compile-time weaving）：如类 A 使用 AspectJ 添加了一个属性，类 B 引用了它，这个场景就需要编译期的时候就进行织入，否则没法编译类 B。
2. 编译后织入（Post-compile weaving）：也就是已经生成了 .class 文件，或已经打成 jar 包了，这种情况我们需要增强处理的话，就要用到编译后织入。
3. 类加载后织入（Load-time weaving）：指的是在加载类的时候进行织入，要实现这个时期的织入，有几种常见的方法



# 事务

Spring事务的本质其实就是数据库对事务的支持，没有数据库的事务支持，Spring是无法提供事务功能的。Spring只提供统一事务管理接口，具体实现都是由各数据库自己实现，数据库事务的提交和回滚是通过数据库自己的事务机制实现。



## Spring事务的种类

在Spring中，事务管理可以分为两大类：

- 编程式事务管理
- 声明式事务管理



**编程式事务管理**

编程式事务可以使用 TransactionTemplate 和 PlatformTransactionManager 来实现，需要显式执行事务。允许我们在代码中直接控制事务的边界，通过编程方式明确指定事务的开始、提交和回滚。

```java
public class AccountService {
    private TransactionTemplate transactionTemplate;

    public void setTransactionTemplate(TransactionTemplate transactionTemplate) {
        this.transactionTemplate = transactionTemplate;
    }

    public void transfer(final String out, final String in, final Double money) {
        transactionTemplate.execute(new TransactionCallbackWithoutResult() {
            @Override
            protected void doInTransactionWithoutResult(TransactionStatus status) {
                // 转出
                accountDao.outMoney(out, money);
                // 转入
                accountDao.inMoney(in, money);
            }
        });
    }
}
```



**声明式事务管理**

声明式事务是建立在 AOP 之上的。其本质是通过 AOP 功能，对方法前后进行拦截，将事务处理的功能编织到拦截的方法中，也就是在目标方法开始之前启动一个事务，在目标方法执行完之后根据执行情况提交或者回滚事务。

相比较编程式事务，优点是**不需要在业务逻辑代码中掺杂事务管理的代码**，Spring 推荐通过 @Transactional 注解的方式来实现声明式事务管理，也是日常开发中最常用的。

不足的地方是，声明式事务管理最细粒度只能作用到方法级别，无法像编程式事务那样可以作用到代码块级别。



## 说说Spring的事务隔离级别

事务的隔离级别定义了一个事务可能受其他并发事务影响的程度。SQL 标准定义了四个隔离级别，Spring 都支持，并且提供了对应的机制来配置它们，定义在 TransactionDefinition 接口中。



- ISOLATION_DEFAULT：使用数据库默认的隔离级别，MySQL 默认的是可重复读，Oracle 默认的读已提交。
- ISOLATION_READ_UNCOMMITTED：读未提交，允许事务读取未被其他事务提交的更改。这是隔离级别最低的设置，可能会导致“脏读”问题。
- ISOLATION_READ_COMMITTED：读已提交，确保事务只能读取已经被其他事务提交的更改。这可以防止“脏读”，但仍然可能发生“不可重复读”和“幻读”问题。
- ISOLATION_REPEATABLE_READ：可重复读，确保事务可以多次从一个字段中读取相同的值，即在这个事务内，其他事务无法更改这个字段，从而避免了“不可重复读”，但仍可能发生“幻读”问题。
- ISOLATION_SERIALIZABLE：串行化，这是最高的隔离级别，它完全隔离了事务，确保事务序列化执行，以此来避免“脏读”、“不可重复读”和“幻读”问题，但性能影响也最大。



## Spring的事务传播机制

事务传播机制是指当一个事务方法调用另一个事务方法时，Spring 如何管理它们的事务关系。Spring 通过 `@Transactional(propagation = Propagation.XXX)` 来控制事务的传播方式。

![image-20250218111009342](C:/Users/shiyu/AppData/Roaming/Typora/typora-user-images/image-20250218111009342.png)

Spring 的默认传播行为是 PROPAGATION_REQUIRED，即如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。



事务传播机制是使用 ThreadLocal 实现的，所以，如果调用的方法是在新线程中，事务传播会失效。

```java
@Transactional
public void parentMethod() {
    new Thread(() -> childMethod()).start();
}

public void childMethod() {
    // 这里的操作将不会在 parentMethod 的事务范围内执行
}
```



## protected和private方法加事务注解会生效吗

在 Spring 中，**只有通过 Spring 容器的 AOP 代理调用的公开方法（public method）上的`@Transactional`注解才会生效**。

如果在 protected、private 方法上使用`@Transactional`，这些事务注解将不会生效，原因：Spring 默认使用基于 JDK 的动态代理（当接口存在时）或基于 CGLIB 的代理（当只有类时）来实现事务。这两种代理机制都只能代理公开的方法。



## 声明式事务实现原理了解吗

Spring 的声明式事务管理是通过 AOP（面向切面编程）和代理机制实现的。



Spring 事务管理主要由 `TransactionInterceptor` + `TransactionManager` 组成，AOP 代理拦截 `@Transactional` 方法，最终调用 `TransactionManager` 进行事务管理。



**1. 在Bean初始化阶段创建代理对象**

- Spring 容器在初始化单例 Bean 的时候，会遍历所有的 `BeanPostProcessor` 实现类，并执行其 `postProcessAfterInitialization` 方法。

- 在执行 `postProcessAfterInitialization` 方法时会遍历容器中所有的切面，查找与当前 Bean 匹配的切面，这里会获取事务的属性切面，也就是 `@Transactional` 注解及其属性值。



**2. 在执行目标方法时进行事务增强操作**

当通过代理对象调用 Bean 方法的时候，会触发对应的 AOP 增强拦截器，声明式事务是一种环绕增强，对应接口为`MethodInterceptor`，事务增强对该接口的实现为`TransactionInterceptor`

事务拦截器`TransactionInterceptor`在`invoke`方法中，通过调用父类`TransactionAspectSupport`的`invokeWithinTransaction`方法进行事务处理，包括开启事务、事务提交、异常回滚等。



## 声明式事务在哪些情况下会失效

**1. @Transactional注解应用在非public修饰的方法上**

**2. @Transactional注解属性propagation设置错误**

- 适应PROPAGATION_NOT_SUPPORTED或者PROPAGATION_NEVER

**3. @Transactional注解属性rollbackFor设置错误**

- Spring 默认抛出未检查 unchecked 异常（继承自 RuntimeException 的异常）或者 Error 才回滚事务，其他异常不会触发回滚事务。

**4. 使用this.method() 调用自身 @Transactional方法**

```java
@Service
public class OrderService {
    @Transactional
    public void createOrder() {
        this.insertLog(); // ❌ 事务失效（直接调用，不经过代理）
    }

    @Transactional
    public void insertLog() { 
        logRepository.save(new Log("订单日志"));
        throw new RuntimeException("插入日志失败");
    }
}
```

**5. Spring 事务基于 AOP 代理，非 Spring 管理的 Bean 不会被代理**

**6.如果在 @Transactional 方法内自己 catch了异常，Spring不会检测到异常的传播，事务不会触发回滚**

```java
@Service
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;

    @Transactional
    public void createOrder() {
        orderRepository.save(new Order());

        try {
            int result = 1 / 0; // 除零异常
        } catch (Exception e) {
            System.out.println("异常被捕获：" + e.getMessage());
            // ❌ 事务不会回滚，因为 Spring 没有检测到异常
        }
    }
}
```



# MVC

## Spring MVC的核心组件

1. **DispatcherServlet**：前置控制器，是整个流程控制的**核心**，控制其他组件的执行，进行统一调度，降低组件之间的耦合性，相当于总指挥。
2. **Handler**：处理器，完成具体的业务逻辑，相当于 Servlet 或 Action。
3. **HandlerMapping**：DispatcherServlet 接收到请求之后，通过 HandlerMapping 将不同的请求映射到不同的 Handler。
4. **HandlerInterceptor**：处理器拦截器，是一个接口，如果需要完成一些拦截处理，可以实现该接口。
5. **HandlerExecutionChain**：处理器执行链，包括两部分内容：Handler 和 HandlerInterceptor（系统会有一个默认的 HandlerInterceptor，如果需要额外设置拦截，可以添加拦截器）。
6. **HandlerAdapter**：处理器适配器，Handler 执行业务方法之前，需要进行一系列的操作，包括表单数据的验证、数据类型的转换、将表单数据封装到 JavaBean 等，这些操作都是由 HandlerApater 来完成，开发者只需将注意力集中业务逻辑的处理上，DispatcherServlet 通过 HandlerAdapter 执行不同的 Handler。
7. **ModelAndView**：装载了模型数据和视图信息，作为 Handler 的处理结果，返回给 DispatcherServlet。
8. **ViewResolver**：视图解析器，DispatcheServlet 通过它将逻辑视图解析为物理视图，最终将渲染结果响应给客户端。



## Spring MVC的工作流程

![三分恶面渣逆袭：Spring MVC的工作流程](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-e29a122b-db07-48b8-8289-7251032e87a1.png)

首先，客户端发送请求，DispatcherServlet 拦截并通过 HandlerMapping 找到对应的控制器。

DispatcherServlet 使用 HandlerAdapter 调用控制器方法，执行具体的业务逻辑，返回一个 ModelAndView 对象。

然后 DispatcherServlet 通过 ViewResolver 解析视图。

最后，DispatcherServlet 渲染视图并将响应返回给客户端。



## Handler是什么？为什么需要HandlerAdapter

Handler 一般就是指 Controller，Controller 是 Spring MVC 的核心组件，负责处理请求，返回响应。

Spring MVC 允许使用多种类型的处理器。不仅仅是标准的`@Controller`注解的类，还可以是实现了特定接口的其他类（如 HttpRequestHandler 或 SimpleControllerHandlerAdapter 等）。这些处理器可能有不同的方法签名和交互方式。

HandlerAdapter 的主要职责就是调用 Handler 的方法来处理请求，并且适配不同类型的处理器。HandlerAdapter 确保 DispatcherServlet 可以以统一的方式调用不同类型的处理器，无需关心具体的执行细节。



## Spring MVC的Restful风格的接口的流程是什么样的

我们都知道 Restful 接口，响应格式是 json，这就用到了一个常用注解：**@ResponseBody**

```java
    @GetMapping("/user")
    @ResponseBody
    public User user(){
        return new User(1,"张三");
    }
```

![Spring MVC Restful请求响应示意图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-2da963a0-5da9-4b3a-aafd-fd8dbc7e1807.png)

1. 客户端向服务端发送一次请求，这个请求会先到前端控制器 DispatcherServlet

2. DispatcherServlet 接收到请求后会调用 HandlerMapping 处理器映射器。由此得知，该请求该由哪个 Controller 来处理

3. DispatcherServlet 调用 HandlerAdapter 处理器适配器，告诉处理器适配器应该要去执行哪个 Controller

4. Controller 被封装成了 ServletInvocableHandlerMethod，HandlerAdapter 处理器适配器去执行 invokeAndHandle 方法，完成对 Controller 的请求处理

5. HandlerAdapter 执行完对 Controller 的请求，会调用 HandlerMethodReturnValueHandler 去处理返回值，主要的过程：

   5.1. 调用 RequestResponseBodyMethodProcessor，创建 ServletServerHttpResponse（Spring 对原生 ServerHttpResponse 的封装）实例

   5.2.使用 HttpMessageConverter 的 write 方法，将返回值写入 ServletServerHttpResponse 的 OutputStream 输出流中

   5.3.在写入的过程中，会使用 JsonGenerator（默认使用 Jackson 框架）对返回值进行 Json 序列化

6. 执行完请求后，返回的 ModealAndView 为 null，ServletServerHttpResponse 里也已经写入了响应，所以不用关心 View 的处理



# Spring Boot

## 介绍一下SpringBoot，有哪些优点

Spring Boot 的优点非常多，比如说：

1. Spring Boot 内嵌了 Tomcat、Jetty、Undertow 等容器，直接运行 jar 包就可以启动项目。
2. Spring Boot 内置了 Starter 和自动装配，避免繁琐的手动配置。例如，如果项目中添加了 spring-boot-starter-web，Spring Boot 会自动配置 Tomcat 和 Spring MVC。
3. Spring Boot 内置了 Actuator 和 DevTools，便于调试和监控。



## Spring Boot核心注解

![image-20250218115451450](C:/Users/shiyu/AppData/Roaming/Typora/typora-user-images/image-20250218115451450.png)



## Spring Boot自动配置原理了解吗

![三分恶面渣逆袭：SpringBoot自动配置原理](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/spring-df77ee15-2ff0-4ec7-8e65-e4ebb8ba88f1.png)

**自动配置文件**

所有`spring-boot-starter-x`的组件配置都是放在`spring-boot-autoconfigura`的组件中的

![img](https://pic4.zhimg.com/v2-0f26d2001be785a479cfefbd5a78b023_1440w.jpg)

这里有个spring.factories的文件，翻译一下就是spring的工厂

![img](https://pic1.zhimg.com/v2-9c08877c31063353953370cefd67fc20_1440w.jpg)

以很明显的看到最下面有个自动配置的注释，key还是个`EnableAutoConfiguration`，开启自动配置！



**SpringBootApplication注解**

![img](https://picx.zhimg.com/v2-99f1362346bbd80407ec08aaf4f452a5_1440w.jpg)

进入@SpringBootApplication注解会发现@EnableAutoConfiguration注解

![img](https://pica.zhimg.com/v2-934dedae2c6426c2215b78c7a3d5ebe8_1440w.jpg)

进入@EnableAutoConfiguration注解，内存存在@Import注解，并且Import了一个Selector

**selectImports**

```java
@Override
public String[] selectImports(AnnotationMetadata annotationMetadata) {
  if (!isEnabled(annotationMetadata)) {
    return NO_IMPORTS;
  }
  AutoConfigurationEntry autoConfigurationEntry = getAutoConfigurationEntry(annotationMetadata);
  return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
}
```

**getAutoConfigurationEntry**

```java
protected AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
  if (!isEnabled(annotationMetadata)) {
    return EMPTY_ENTRY;
  }
  AnnotationAttributes attributes = getAttributes(annotationMetadata);
  // 获取候选的配置类
  List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
  // 移除重复的配置
  configurations = removeDuplicates(configurations);
  // 获取到要排除的配置
  Set<String> exclusions = getExclusions(annotationMetadata, attributes);
  checkExcludedClasses(configurations, exclusions);
  // 移除所有要排除的配置
  configurations.removeAll(exclusions);
  // 过滤掉不具备注入条件的配置类，通过Conditional注解
  configurations = getConfigurationClassFilter().filter(configurations);
  // 通知自动配置相关的监听器
  fireAutoConfigurationImportEvents(configurations, exclusions);
  // 返回所有自动配置类
  return new AutoConfigurationEntry(configurations, exclusions);
}
```

主要看看是如何从配置文件读取的

**getCandidateConfigurations**

```java
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
  // 这里就是关键，使用SpringFactoriesLoader加载所有配置类，是不是像我们SPI的ServicesLoader
  List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
                                                                       getBeanClassLoader());
  Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you "
                  + "are using a custom packaging, make sure that file is correct.");
  return configurations;
}
```

**getSpringFactoriesLoaderFactoryClass**

```java
protected Class<?> getSpringFactoriesLoaderFactoryClass() {
  return EnableAutoConfiguration.class;
}
```

结合上一步，就是加载配置文件，并且读取key为EnableAutoConfiguration的配置

**loadFactoryNames**

```java
public static List<String> loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader) {
  String factoryTypeName = factoryType.getName();
  return loadSpringFactories(classLoader).getOrDefault(factoryTypeName, Collections.emptyList());
}

private static Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader) {

  try {
    // FACTORIES_RESOURCE_LOCATION的值为：META-INF/spring.factories
    // 这步就是意味中读取classpath下的META-INF/spring.factories文件
    Enumeration<URL> urls = (classLoader != null ?
                             classLoader.getResources(FACTORIES_RESOURCE_LOCATION) :
                             ClassLoader.getSystemResources(FACTORIES_RESOURCE_LOCATION));
    // 接下来就是读取出文件内容，封装成map的操作了
    result = new LinkedMultiValueMap<>();
    while (urls.hasMoreElements()) {
      URL url = urls.nextElement();
      UrlResource resource = new UrlResource(url);
      Properties properties = PropertiesLoaderUtils.loadProperties(resource);
      for (Map.Entry<?, ?> entry : properties.entrySet()) {
        String factoryTypeName = ((String) entry.getKey()).trim();
        for (String factoryImplementationName : StringUtils.commaDelimitedListToStringArray((String) entry.getValue())) {
          result.add(factoryTypeName, factoryImplementationName.trim());
        }
      }
    }
    cache.put(classLoader, result);
    return result;
  }
  catch (IOException ex) {
    throw new IllegalArgumentException("Unable to load factories from location [" +
                                       FACTORIES_RESOURCE_LOCATION + "]", ex);
  }
}
```



## SpringBoot Starter的原理了解吗

Spring Boot Starter 主要通过**起步依赖**和**自动配置**机制来简化项目的构建和配置过程。

- 起步依赖是 Spring Boot 提供的一组预定义依赖项，它们将一组相关的库和模块打包在一起。比如 `spring-boot-starter-web` 就包含了 Spring MVC、Tomcat 和 Jackson 等依赖。
- 自动配置机制是 Spring Boot 的核心特性，通过自动扫描类路径下的类、资源文件和配置文件，自动创建和配置应用程序所需的 Bean 和组件。



比如有了 `spring-boot-starter-web`，我们开发者就不需要再手动配置 Tomcat、Spring MVC 等，Spring Boot 会自动帮我们完成这些工作。



## Spring Boot启动原理了解吗

Spring Boot 的启动由 SpringApplication 类负责：

- 第一步，创建 SpringApplication 实例，负责应用的启动和初始化；
- 第二步，从 application.yml 中加载配置文件和环境变量；
- 第三步，创建上下文环境 ApplicationContext，并加载 Bean，完成依赖注入；
- 第四步，启动内嵌的 Web 容器。
- 第五步，发布启动完成事件 ApplicationReadyEvent，并调用 ApplicationRunner 的 run 方法完成启动后的逻辑。



![image-20250218122736537](C:/Users/shiyu/AppData/Roaming/Typora/typora-user-images/image-20250218122736537.png)



## 了解@SpringBootApplication注解吗

`@SpringBootApplication`是 Spring Boot 的核心注解，经常用于主类上，作为项目启动入口的标识。它是一个组合注解：

- `@SpringBootConfiguration`：继承自 `@Configuration`，标注该类是一个配置类，相当于一个 Spring 配置文件。
- `@EnableAutoConfiguration`：告诉 Spring Boot 根据 pom.xml 中添加的依赖自动配置项目。例如，如果 spring-boot-starter-web 依赖被添加到项目中，Spring Boot 会自动配置 Tomcat 和 Spring MVC。
- `@ComponentScan`：扫描当前包及其子包下被`@Component`、`@Service`、`@Controller`、`@Repository` 注解标记的类，并注册为 Spring Bean。



## 为什么Spring Boot在启动的时候能够找到main方法上的@SpringBootApplication注解

Spring Boot 在启动时能够找到主类上的`@SpringBootApplication`注解，是因为它利用了 Java 的反射机制和类加载机制，结合 Spring 框架内部的一系列处理流程。

当运行一个 Spring Boot 程序时，通常会调用主类中的`main`方法，这个方法会执行`SpringApplication.run()`，比如：



```java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

`SpringApplication.run(Class<?> primarySource, String... args)`方法接收两个参数：第一个是主应用类（即包含`main`方法的类），第二个是命令行参数。`primarySource`参数提供了一个起点，Spring Boot 通过它来加载应用上下文。

Spring Boot 利用 Java 反射机制来读取传递给`run`方法的类（`MyApplication.class`）。它会检查这个类上的注解，包括`@SpringBootApplication`。



## Spring Boot默认的包扫描路径是什么

Spring Boot 的默认包扫描路径是以启动类 `@SpringBootApplication` 注解所在的包为根目录的，即默认情况下，Spring Boot 会扫描启动类所在包及其子包下的所有组件。



如果需要自定义包扫描路径，可以在`@SpringBootApplication`注解上添加`@ComponentScan`注解，指定要扫描的包路径。



```java
@SpringBootApplication
@ComponentScan(basePackages = {"com.github.paicoding.forum"})
public class QuickForumApplication {
    public static void main(String[] args) {
        SpringApplication.run(QuickForumApplication.class, args);
    }
}
```

这种方式会覆盖默认的包扫描路径，只扫描`com.github.paicoding.forum`包及其子包下的所有组件。



## Spring Boot和Spring MVC的区别

Spring MVC 是基于 Spring 框架中的Web框架，提供了一种 Model-View-Controller（模型-视图-控制器）的开发模式。

Spring Boot 旨在简化 Spring 应用的配置和部署过程，提供了大量的自动配置选项，以及运行时环境的内嵌 Web 服务器，这样就可以更快速地开发一个 SpringMVC 的 Web 项目。



**Spring Boot 默认使用 Spring MVC 作为 Web 框架，提供自动配置，无需手动配置 DispatcherServlet。**



## Spring Boot和Spring有什么区别

Spring 是一个 Java 应用框架，提供 IOC（依赖注入）、AOP（面向切面编程）、事务管理、Spring MVC（Web 框架）等功能，需要大量的手动配置。



Spring Boot 是 Spring 的扩展框架，提供 自动配置、内嵌 Web 服务器、简化依赖管理，让开发者能快速搭建 Spring 应用，无需繁琐的 XML 配置。



# Spring Cloud

## 对Spring Cloud了解多少

Spring Cloud 是一个基于 Spring Boot，提供构建分布式系统和微服务架构的工具集。用于解决分布式系统中的一些常见问题，如配置管理、服务发现、负载均衡等等。





## 微服务架构主要要解决哪些问题？

- 服务很多，客户端怎么访问，如何提供对外网关?
- 这么多服务，服务之间如何通信? HTTP 还是 RPC?
- 这么多服务，如何治理? 服务的注册和发现。
- 服务挂了怎么办？熔断机制。



