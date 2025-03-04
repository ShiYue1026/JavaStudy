# 基础

## 说说什么是MyBatis

**优点**

- Mybatis 是一个半 ORM（对象关系映射）框架，它内部封装了 JDBC，开发时只需要关注 SQL 语句本身，不需要花费精力去处理加载驱动、创建连接、创建 statement 等繁杂的过程。程序员直接编写原生态 sql，可以严格控制 sql 执行性能，灵活度高。
- MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO 映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

**缺点**

- SQL 语句的编写工作量较大，尤其当字段多、关联表多时，对开发人员编写 SQL 语句的功底有一定要求
- SQL 语句依赖于数据库，导致数据库移植性差，不能随意更换数据库



## ORM是什么

![ORM简单示意图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/mybatis-ea212850-56e0-4d12-98fb-03bb40007f44.png)

ORM（Object Relational Mapping），对象关系映射，是一种为了解决关系型数据库数据与简单Java对象（POJO）的映射关系的技术。简单来说，ORM是通过使用描述对象和数据库之间映射的元数据，将程序中的对象自动持久化到关系型数据库中。



## 为什么说MyBatis是半自动ORM映射工具，与全自动的区别在哪里

Mybatis 在查询关联对象或关联集合对象时，需要手动编写 SQL 来完成，所以，被称之为半自动 ORM 映射工具。



## JDBC编程有哪些不足之处，MyBatis是如何解决的？

![JDBC编程的不足](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/mybatis-f8b181a3-ad40-4381-98ba-351668579bfb.png)

- **数据连接创建、释放频繁造成系统资源浪费从而影响系统性能**：在 mybatis-config.xml 中配置数据链接池，使用连接池统一管理数据库连接。
- **sql 语句写在代码中造成代码不易维护**：将 sql 语句配置在 XXXXmapper.xml 文件中与 java 代码分离。
- **向 sql 语句传参数麻烦**：因为 sql 语句的 where 条件不一定，可能多也可能少，占位符需要和参数一一对应。Mybatis 自动将 java 对象映射至 sql 语句。
- **对结果集解析麻烦**：sql 变化导致解析代码变化，且解析前需要遍历，如果能将数据库记录封装成 pojo 对象解析比较方便。Mybatis 自动将 sql 执行结果映射至 java 对象。



## MyBatis的使用过程？

MyBatis 基本使用的过程大概可以分为这么几步：

![Mybatis基本使用步骤](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/mybatis-47bab2e8-5c08-4f61-9c0c-dddfe09fb2b5.png)

**1. 创建 SqlSessionFactory**

可以从配置或者直接编码来创建 SqlSessionFactory

```java
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

**2. 通过SqlSessionFactory创建SqlSession**

SqlSession（会话）可以理解为程序和数据库之间的桥梁

```java
SqlSession session = sqlSessionFactory.openSession();
```

**3. 通过 sqlsession 执行数据库操作**

通过Sqlsession获取 Mapper(映射)，然后再执行 SQL 语句：

```java
BlogMapper mapper = session.getMapper(BlogMapper.class);
Blog blog = mapper.selectBlog(101);
```

**4. 调用session.commit()提交事务**

如果是更新、删除语句，我们还需要提交一下事务。

**5. 调用session.close()关闭会话**

最后一定要记得关闭会话



## MyBatis的生命周期？

上面提到了几个 MyBatis 的组件，一般说的 MyBatis 生命周期就是这些组件的生命周期。

**SqlSessionFactoryBuilder**

- 一旦创建了 SqlSessionFactory，就不再需要它了。 因此 SqlSessionFactoryBuilder 实例的生命周期只存在于方法的内部。

**SqlSessionFactory**

- SqlSessionFactory 是用来创建 SqlSession 的，相当于一个数据库连接池，每次创建 SqlSessionFactory 都会使用数据库资源，多次创建和销毁是对资源的浪费。所以 SqlSessionFactory 是应用级的生命周期，而且应该是单例的。

**SqlSession**

- SqlSession 相当于 JDBC 中的 Connection，SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的生命周期是一次请求或一个方法。

**Mapper**

- 映射器是一些绑定映射语句的接口。映射器接口的实例是从 SqlSession 中获得的，它的生命周期在 sqlsession 事务方法之内，一般会控制在方法级。



MyBatis 通常是和 Spring 集成使用，Spring 可以帮助我们创建线程安全的、基于事务的 SqlSession 和映射器，并将它们直接注入到我们的 bean 中，我们不需要关心它们的创建过程和生命周期。



## 在Mapper中如何传递多个参数

**1. 顺序传参法**

```java
public User selectUser(String name, int deptId);

<select id="selectUser" resultMap="UserResultMap">
    select * from user
    where user_name = #{0} and dept_id = #{1}
</select>
```

- `#{}`里面的数字代表传入参数的顺序。
- 这种方法不建议使用，sql 层表达不直观，且一旦顺序调整容易出错。



**2. @Param注解传参法**

```java
public User selectUser(@Param("userName") String name, int @Param("deptId") deptId);

<select id="selectUser" resultMap="UserResultMap">
    select * from user
    where user_name = #{userName} and dept_id = #{deptId}
</select>
```

- `#{}`里面的名称对应的是注解@Param 括号里面修饰的名称。
- 这种方法在参数不多的情况还是比较直观的，（推荐使用）。



**3. Map传参法**

```java
public User selectUser(Map<String, Object> params);

<select id="selectUser" parameterType="java.util.Map" resultMap="UserResultMap">
    select * from user
    where user_name = #{userName} and dept_id = #{deptId}
</select>
```

- `#{}`里面的名称对应的是 Map 里面的 key 名称。
- 这种方法适合传递多个参数，且参数易变能灵活传递的情况。



**4. Java Bean传参法**

```java
public User selectUser(User user);

<select id="selectUser" parameterType="com.jourwon.pojo.User" resultMap="UserResultMap">
    select * from user
    where user_name = #{userName} and dept_id = #{deptId}
</select>
```

- `#{}`里面的名称对应的是 User 类里面的成员属性。
- 这种方法直观，需要建一个实体类，扩展不容易，需要加属性，但代码可读性强，业务逻辑处理方便，推荐使用。（推荐使用）。



## 实体类属性名和表中字段名不一样怎么办

第 1 种： 通过在查询的 SQL 语句中定义字段名的别名，让字段名的别名和实体类的属性名一致。

```xml
<select id="getOrder" parameterType="int" resultType="com.jourwon.pojo.Order">
       select order_id AS id, order_no AS orderno ,order_price AS price from orders where order_id=#{id};
</select>
```

第 2 种： 通过 resultMap 中的\<result>来映射字段名和实体类属性名的一一对应的关系。

```xml
<select id="getOrder" parameterType="int" resultMap="orderResultMap">
  select * from orders where order_id=#{id}
</select>

<resultMap type="com.jourwon.pojo.Order" id="orderResultMap">
    <!–用id属性来映射主键字段–>
    <id property="id" column="order_id">
    <!–用result属性来映射非主键字段，property为实体类属性名，column为数据库表中的属性–>
  <result property ="orderno" column ="order_no"/>
  <result property="price" column="order_price" />
</resultMap>
```



## #{} 和 ${} 的区别

`#{}` 是预编译处理，`${}` 是字符串替换。

- 当使用 `#{}` 时，MyBatis 会在 SQL 执行之前，将占位符替换为问号 `?`，并使用参数值来替代这些问号。

- 由于 `#{}` 使用了预处理，所以能有效防止 SQL 注入，确保参数值在到达数据库之前被正确地处理和转义。

  ```java
  <select id="selectUser" resultType="User">
    SELECT * FROM users WHERE id = #{id}
  </select>
  ```



- 当使用 `${}` 时，参数的值会直接替换到 SQL 语句中去，而不会经过预处理。
- 这就存在 SQL 注入的风险，因为参数值会直接拼接到 SQL 语句中，假如参数值是 `1 or 1=1`，那么 SQL 语句就会变成 `SELECT * FROM users WHERE id = 1 or 1=1`，这样就会导致查询出所有用户的结果。



`${}` 通常用于那些不能使用预处理的场合，比如说动态表名、列名、排序等，要提前对参数进行安全性校验



## 模糊查询like语句该怎么写

最安全的方法 是 在 Java 代码中拼接 `%`，然后使用 `#{}` 进行参数传递：

```xml
<select id="searchUserByName" resultType="User">
    SELECT * FROM users WHERE name LIKE #{name}
</select>
```

```java
public List<User> searchUserByName(String name) {
    return userMapper.searchUserByName("%" + name + "%");
}
```



通过CONCAT在MyBatis中拼接 `%`

```xml
<select id="searchUserByName" resultType="User">
    SELECT * FROM users WHERE name LIKE CONCAT('%', #{name}, '%')
</select>
```

```java
List<User> users = userMapper.searchUserByName("Tom");
```



## MyBatis是否支持延迟加载？原理是什么？

支持，但默认不开启，需要手动配置

只有在 真正访问关联对象时，才会执行 SQL 查询，避免一次性加载所有数据，提高性能。

MyBatis 的 `association`（多对一） 和 `collection`（一对多、多对多） 支持 延迟加载。



**例子**：

**1. 数据库表**

```sql
CREATE TABLE user (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50)
);

CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    order_name VARCHAR(50),
    FOREIGN KEY (user_id) REFERENCES user(id)
);
```

**2. Java实体类**

```java
public class User {
    private int id;
    private String name;
    private List<Order> orders; // 一对多延迟加载
}

public class Order {
    private int id;
    private int userId;
    private String orderName;
}
```

**3. resultMap配置（collection方式）**

```xml
<resultMap id="UserOrderMap" type="User">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    
    <!-- 一对多映射，使用 collection 延迟加载 -->
    <collection property="orders" ofType="Order" fetchType="lazy">
        <id property="id" column="orderId"/>
        <result property="orderName" column="orderName"/>
    </collection>
</resultMap>

<!-- 查询用户信息（不加载订单） -->
<select id="getUserById" resultMap="UserOrderMap">
    SELECT id, name FROM user WHERE id = #{id};
</select>

<!-- 查询用户的订单（MyBatis 在需要时自动执行） -->
<select id="getOrdersByUserId" resultType="Order">
    SELECT id AS orderId, order_name AS orderName FROM orders WHERE user_id = #{userId};
</select>
```

**4. 测试代码**

```java
User user = userMapper.getUserById(1); // 只查询 user 表

System.out.println(user.getName()); // ✅ 只执行了 User 查询

System.out.println(user.getOrders()); // ❗️ 访问 orders 时，MyBatis 才查询 orders 
```



## MyBatis支持动态SQL吗

MyBatis 中有一些支持动态 SQL 的标签，它们的原理是使用 OGNL 从 SQL 参数对象中计算表达式的值，根据表达式的值动态拼接 SQL，以此来完成动态 SQL 的功能。



**if**

```xml
<select id="findActiveBlogWithTitleLike"
   resultType="Blog">
SELECT * FROM BLOG
WHERE state = ‘ACTIVE’
<if test="title != null">
  AND title like #{title}
</if>
</select>
```



**choose(when, otherwise)**

`<choose>` 结构类似于 Java 的 `switch-case`， 只会执行第一个匹配的 `<when>`，如果所有 `when` 都不满足，则执行 `<otherwise>`。

```xml
<select id="findActiveBlogLike"
   resultType="Blog">
SELECT * FROM BLOG WHERE state = ‘ACTIVE’
<choose>
  <when test="title != null">
    AND title like #{title}
  </when>
  <when test="author != null and author.name != null">
    AND author_name like #{author.name}
  </when>
  <otherwise>
    AND featured = 1
  </otherwise>
</choose>
</select>
```



**\<where>**

`<where>` 的作用是 自动处理 SQL 语句的 `WHERE` 关键字，避免手写 SQL 时的 多余 `AND` 或 `OR`，确保 SQL 语法正确。

```xml
<select id="findActiveBlogLike"
   resultType="Blog">
SELECT * FROM BLOG
<where>
  <if test="state != null">
       state = #{state}
  </if>
  <if test="title != null">
      AND title like #{title}
  </if>
  <if test="author != null and author.name != null">
      AND author_name like #{author.name}
  </if>
</where>
</select>
```



**\<set>**

可以用在动态更新的时候，MyBatis 会自动去掉 `SET` 语句末尾的 `,`，保证 SQL 语法正确。

```xml
<update id="updateAuthorIfNecessary">
  update Author
    <set>
      <if test="username != null">username=#{username},</if>
      <if test="password != null">password=#{password},</if>
      <if test="email != null">email=#{email},</if>
      <if test="bio != null">bio=#{bio}</if>
    </set>
  where id=#{id}
</update>
```



**foreach**

用来循环的，可以对集合进行遍历

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
SELECT *
FROM POST P
<where>
  <foreach item="item" index="index" collection="list"
      open="ID in (" separator="," close=")" nullable="true">
        #{item}
  </foreach>
</where>
</select>
```



## 说说MyBatis的一级、二级缓存

**一级缓存**（默认开启）

![Mybatis一级缓存](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/mybatis-54afb458-7dfc-4d48-9a90-4ad1a8739937.png)

作用范围： `SqlSession` 级别（同一个 `SqlSession` 共享）

在 同一个 `SqlSession` 中执行相同的 SQL 查询时，MyBatis 会从缓存中读取数据，避免重复查询数据库。

当 `SqlSession` 关闭、提交事务、执行 `update/delete/insert` 操作时，缓存失效。



**二级缓存**（默认关闭）

![Mybatis二级缓存示意图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/mybatis-8dae71da-ffd4-43f5-9ee9-258ea82d216b.png)

作用范围： Mapper 级别（同一 `namespace` 共享）

生命周期： `SqlSession` 关闭后，查询结果可被其它 `SqlSession` 复用。
🔹 存储位置： 可存储在内存或第三方缓存（如 Redis）。
🔹 数据一致性问题： 更新数据时，二级缓存必须清除，否则可能读取到旧数据



# 原理

## 能说说MyBatis的工作原理吗

MyBatis 的工作流程，按工作原理，可以分为两大步：`生成会话工厂`、`会话运行`。

![MyBatis的工作流程](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/mybatis-61ac17ef-9eee-48c0-9a2d-545e1d554b13.png)



![会话运行的简单示意图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/mybatis-ebd0712a-1f62-4154-b391-2cb596634710.png)

![MyBatis整体工作原理图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/mybatis-dc142e94-8e7f-4ec6-a1f6-1d20669292ad.png)

1. 读取 MyBatis 配置文件——mybatis-config.xml 、加载映射文件——映射文件即 SQL 映射文件，文件中配置了操作数据库的 SQL 语句。最后生成一个配置对象。
2. 构造会话工厂：通过 MyBatis 的环境等配置信息构建会话工厂 SqlSessionFactory。
3. 创建会话对象：由会话工厂创建 SqlSession 对象，该对象中包含了执行 SQL 语句的所有方法。
4. Executor 执行器：MyBatis 底层定义了一个 Executor 接口来操作数据库，它将根据 SqlSession 传递的参数动态地生成需要执行的 SQL 语句，同时负责查询缓存的维护。
5. StatementHandler：数据库会话器，串联起参数映射的处理和运行结果映射的处理。
6. 参数处理：对输入参数的类型进行处理，并预编译。
7. 结果处理：对返回结果的类型进行处理，根据对象映射规则，返回相应的对象



## 为什么Mapper接口不需要实现类

MyBatis采用JDK动态代理为Mapper接口生成代理对象，拦截方法调用，自动执行SQL，因此Mapper接口不需要手写实现类



```java
public class MapperProxy implements InvocationHandler {
    private SqlSession sqlSession;
    private Class<?> mapperInterface;

    public MapperProxy(SqlSession sqlSession, Class<?> mapperInterface) {
        this.sqlSession = sqlSession;
        this.mapperInterface = mapperInterface;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        String statementId = mapperInterface.getName() + "." + method.getName(); 
        return sqlSession.selectOne(statementId, args[0]); // 执行 SQL
    }
}
```

```java
invoke(proxy, method, args)  // 拦截 getUserById(1)
→ sqlSession.selectOne("UserMapper.getUserById", 1)
→ 执行 SQL: SELECT * FROM users WHERE id = 1;
→ 解析结果，返回 User 对象
```



## MyBatis是如何进行分页的，分页插件的原理是什么

分页查询（第 `n` 页，每页 `pageSize` 条数据）

```sql
SELECT * FROM users LIMIT (n - 1) * pageSize, pageSize;
```



分页插件的原理是 使用 MyBatis 的 `Interceptor` 拦截 SQL 语句，然后 动态修改 SQL 语句，添加 `LIMIT`。

```java
@Intercepts({
    @Signature(type = StatementHandler.class, method = "prepare", args = {Connection.class, Integer.class})
})
public class PageInterceptor implements Interceptor {
    @Override
    public Object intercept(Invocation invocation) throws Throwable {
        // 1. 获取原始 SQL
        StatementHandler handler = (StatementHandler) invocation.getTarget();
        BoundSql boundSql = handler.getBoundSql();
        String originalSql = boundSql.getSql();

        // 2. 解析分页参数（pageNum, pageSize）
        PageBounds pageBounds = PageHelper.getPageBounds();

        // 3. 修改 SQL，添加 LIMIT
        String newSql = originalSql + " LIMIT " + pageBounds.getOffset() + ", " + pageBounds.getLimit();

        // 4. 重新设置 SQL
        ReflectUtil.setFieldValue(boundSql, "sql", newSql);

        // 5. 继续执行
        return invocation.proceed();
    }
}
```



## 说说JDBC的执行步骤

**1. 加载数据库驱动**

在与数据库建立连接之前，首先需要通过`Class.forName()`方法加载对应的数据库驱动。这一步确保 JDBC 驱动注册到了`DriverManager`类中。

```java
Class.forName("com.mysql.cj.jdbc.Driver");
```



**2. 建立数据库连接**

使用`DriverManager.getConnection()`方法建立到数据库的连接。这一步需要提供数据库 URL、用户名和密码作为参数。

```java
Connection conn = DriverManager.getConnection(
    "jdbc:mysql://localhost:3306/databaseName", "username", "password");
```



**3. 创建Statement对象**

```java
Statement stmt = conn.createStatement();
```



**4. 执行SQL语句**

使用`Statement`对象执行 SQL 语句。

- 执行查询（SELECT）语句时，使用`executeQuery()`方法，它返回`ResultSet`对象；

- 执行更新（INSERT、UPDATE、DELETE）语句时，使用`executeUpdate()`方法，它返回一个整数表示受影响的行数。

```java
ResultSet rs = stmt.executeQuery("SELECT * FROM tableName");
```

```java
int affectedRows = stmt.executeUpdate("UPDATE tableName SET column = 'value' WHERE condition");
```



**5. 处理结果集**

如果执行的是查询操作，需要处理`ResultSet`对象来获取数据。

```java
while (rs.next()) {
    String data = rs.getString("columnName");
    // 处理每一行数据
}
```



**6. 关闭资源**

最后，需要依次关闭`ResultSet`、`Statement`和`Connection`等资源，释放数据库连接等资源。

```java
if (rs != null) rs.close();
if (stmt != null) stmt.close();
if (conn != null) conn.close();
```



## 创建连接拿到的是什么对象

在 JDBC 的执行步骤中，创建连接后拿到的对象是`java.sql.Connection`对象。这个对象是 JDBC API 中用于表示数据库连接的接口，它提供了执行 SQL 语句、管理事务等一系列操作的方法。

`Connection`对象代表了应用程序和数据库的一个连接会话。

通过调用`DriverManager.getConnection()`方法并传入数据库的 URL、用户名和密码等信息来获得这个对象。

一旦获得`Connection`对象，就可以使用它来创建执行 SQL 语句的`Statement`、`PreparedStatement`和`CallableStatement`对象，以及管理事务等。



## Statement与PreparedStatement的区别

`Statement`和`PreparedStatement`都是用于执行 SQL 语句的接口，但它们之间存在几个关键的区别：

- 每次执行`Statement`对象的`executeQuery`或`executeUpdate`方法时，SQL 语句在数据库端都需要重新编译和执行。这适用于一次性执行的 SQL 语句。

- **Statement** 不支持参数化查询。如果需要在 SQL 语句中插入变量，通常需要通过字符串拼接的方式来实现，这会增加 SQL 注入攻击的风险。



- **PreparedStatement** 代表预编译的 SQL 语句的对象。这意味着 SQL 语句在`PreparedStatement`对象创建时就被发送到数据库进行预编译。之后，可以通过设置参数值来多次高效地执行这个 SQL 语句。这不仅减少了数据库编译 SQL 语句的开销，也提高了性能，尤其是对于重复执行的 SQL 操作。

- **PreparedStatement** 支持参数化查询，即可以在 SQL 语句中使用问号（`?`）作为参数占位符。通过`setXxx`方法（如`setString`、`setInt`）设置参数，可以有效防止 SQL 注入。

  

总的来说，`PreparedStatement`相比`Statement`有着更好的性能和更高的安全性，是执行 SQL 语句的首选方式，尤其是在处理含有用户输入的动态查询时。

