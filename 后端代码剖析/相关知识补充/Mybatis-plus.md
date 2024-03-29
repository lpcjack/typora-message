# 一，Mybatis-plus简介

---

​	Mybatis-Plus(简称MP)是一个Mybatis的增强工具，只是在Mybatis的基础上做了增强却不做改变，MyBatis-Plus支持所有Mybatis原生的特性，所以引入Mybatis-Plus不会对现有的Mybatis构架产生任何影响。MyBatis 增强工具包，简化 CRUD 操作。启动加载 XML 配置时注入单表 SQL 操作 ，为简化开发工作、提高生产率而生。

---

# 二，Mybatis-plus配置

---

## 1，引入依赖

```xml
		<!--引入mybatisPlus 包含了 jdbc -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>
		<!-- mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!--引入durid数据源-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.18</version>
        </dependency>
```

---

## 2、application.yml配置

```properties
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl  #日志
    map-underscore-to-camel-case: true  #开启驼峰命名
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    username: 自己的用户名
    password: 自己的密码
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis_plus?				useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=false&serverTimezone=GMT%2B8&rewriteBatchedStatements=true
```

---

## 3，主启动类

```java
@SpringBootApplication
//注意：扫描包路径必须要精确到Mapper包，否则报异常
@MapperScan(basePackages = "扫描自己的mapper路径")
public class SpringBootThree {
    public static void main(String[] args) {
        SpringApplication.run(SpringBootThree.class,args);
    }
}
```

​	注意：`@MapperScan(basePackages = "扫描自己的mapper路径")`**必须添加，且必须具体到mapper包。**

---

## 4，entity实体层

### （1），需要添加的注解

---

#### 1，**@TableName**

- **描述：表名注解，标识实体类对应的表**

- ```java
  //表明注解
  @TableName("user")
  public class User {
      @TableId(type = IdType.AUTO)
      private Long id;
  
  private String name;
  
  private String password;
  
  private String username;
  
  }
  ```

- | 属性             | 描述                                                         |
  | ---------------- | ------------------------------------------------------------ |
  | ==value==        | ==表名==                                                     |
  | keepGlobalPrefix | 是否保持使用全局的 tablePrefix 的值（当全局 tablePrefix 生效时） |
  | resultMap        | xml 中 resultMap 的 id（用于满足特定类型的实体类对象绑定）   |



#### 2，@TableId

- 描述：**主键注解**

- 使用位置：**实体类主键字段**

- ```java
  @TableName("user")
  public class User {
      //主键注解
      @TableId(type = IdType.AUTO)
      private Long id;
  
  private String name;
  
  private String password;
  
  private String username;
  
  }
  ```

  

- |   属性    | 默认值      | 描述           |
  | :-------: | ----------- | -------------- |
  | ==value== | ==""==      | ==主键字段名== |
  |   type    | IdType.NONE | 制定主键类型   |



- **IdType** 
-  **一般都是数据库自增，或者就是自己设置主键id**

- | 值       |                             描述                             |
  | -------- | :----------------------------------------------------------: |
  | ==AUTO== |                      **数据库 ID 自增**                      |
  | NONE     | 无状态，该类型为未设置主键类型（注解里等于跟随全局，全局里约等于 INPUT） |

  



#### 3，@TableField

- 描述：**字段注解（非主键）**

- ```java
  @TableName("user")
  public class User {
      @TableId(type = IdType.AUTO)
      private Long id;
  //字段注解
  @TableField("t_name")
  private String name;
  
  @TableField("t_pwd")
  private String password;
  
   @TableField("t_username")
  private String username;
  
  //表示是否为数据库字段，在进行mybatis-plus封装的CRUD时，不会携带这个字段进行数据库的查询
      @TableField(exist=false)
         //自定义类型
          private Dept dept;
  }
  ```

- | 属性  | 默认值 | 描述               |
  | :---: | ------ | ------------------ |
  | value | ""     | 数据库字段名       |
  | exist | true   | 是否为数据库表字段 |

---



## 5，Mapper层

#### （1），mapper层创建

- **BaseMapper<T>是mybatis-plus设计的一个接口**，里面包含了单表的CRUD，用起来非常方便。

- ```java
  @Repository
  //<T>表示实体类
  public interface UserMapper  extends BaseMapper<User>{
  
  }
  ```

---



#### （2），@RequestMapping

- @RequestMapping注解是一个**用来处理请求地址映射的注解**，可用于**映射一个请求或一个方法**，可以用在类或方法上。



##### （Ⅰ），标注在方法上

- 用于方法上，**表示在类的父路径下追加方法上注解中的地址将会访问到该方法。**

- ```java
  @Controller
  public class RequestMappingController {
  @RequestMapping("/testRequest")
  public String testRequest(){
      return "success";
  }
  }
  ```

  此时请求映射所映射的请求的请求路径为：`http://http://localhost:8080/springmvc_study02/testRequest`

  ![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401152220778.png)

**注意：`springmvc_study02`是项目名称**

---

##### （Ⅱ），标注在类上

- 用于方法上，**表示在类的父路径下追加方法上注解中的地址将会访问到该方法。**

- ```java
  @Controller
  public class RequestMappingController {
  @RequestMapping("/testRequest")
  public String testRequest(){
      return "success";
  }
  }
  ```

  此时请求映射所映射的请求的请求路径为：`http://http://localhost:8080/springmvc_study02/testRequest`

![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401152234880.png)

---

##### （Ⅲ），标注在类和方法上

- 用于类上，**表示类中的所有响应请求的方法都是以该地址作为父路径。**

- ```java
  @Controller
  @RequestMapping(“/hello”)
  public class RequestMappingController {
  @RequestMapping("/testRequest")
  public String testRequest(){
      return "success";
  }
  }
  ```

  - 此时请求映射所映射的请求的请求路径为：`http://localhost:8080/springmvc_study02/hello/testRequest`

  ![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401152237406.png)

​	 注意：**当你在类上添加RequestMapping注解后，如果要请求映射，就意味着请求要先映射到标注类的位置，然后再映射到该类的方法上。**如果不加就会出现如下错误。

![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401152239361.png)

---



##### （Ⅳ），value属性

-  @RequestMapping的value属性

- @RequestMapping 的 value 属性必须设值；

- @RequestMapping 的 value 属性是通过当前请求的请求地址来匹配请求；

- 从上面的源码中可以看到value属性是一个字符串类型的数组，因此说明可以将多个请求映射到一个方法上，只需要给 value 来指定一个包含多个路径的数组。

- ```java
  @Controller
  public class RequestMappingController {
  @RequestMapping(value = {"/testRequest","/test"})
  public String testRequest(){
      return "success";
  }
  
  }
  ```

  在浏览器中输入下面路径进行测试：

  ![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401161027679.png)

​	![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401161027153.png)

**注意：从上面两张图我们能够看到，这时的请求映射所映射的请求的请求路径为选择value数组中的任意一个都可以。**

---

##### （Ⅴ），method属性

- @RequestMapping的method属性是通过**当前请求的请求方式来匹配请求**；
- 浏览器向服务器发送请求，请求方式有很多**GET、HEAD、POST、PUT、PATCH、DELETE、OPTIONS、TRACE**。可以使用 method 属性来约束请求方式。
- 设置请求方式为get：

```java
@Controller
public class RequestMappingController {

    @RequestMapping(value = "/testRequest",method = RequestMethod.GET)
    public String testRequest(){
        return "success";
    }

}
```

- 以post方式请求：

```css
<form th:action="@{/testRequest}" method="post">
    <input type="submit">
</form>
```

- 请求结果：

  ![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401161046873.png)

​	注意：我们在@RequestMapping注解里加一个方法限定：**method = RequestMethod.GET，则请求必须是get的**，否则就会发生以上的错误。

​	映射方法中明确要求请求方式为get，所以post方式不被允许，只有修改为get，才能够请求成功，如果要想两种方式都支持，只需在@RequestMapping	注解的method属性中添加另一种方式即可，中间用英文逗号隔开。

- **扩展：**

  ```
  @GetMapping：处理get方式请求的映射
  
  @PostMapping：处理post方式请求的映射
  
  @PutMapping：处理put方式请求的映射
  
  @DeleteMapping：处理delete方式请求的映射
  
  @GetMapping就相当于@RequestMapping(method=RequestMethod.GET),它会将get映射到特定的方法上。
  ```

  使用方式：`@GetMapping(value = "/testRequest")`



---



##### （Ⅵ），params属性

- @RequestMapping的params属性是通过当前请求的请求参数来匹配请求；

- @RequestMapping的params属性是一个字符串类型的数组，可以通过下面四种表达是来设置匹配关系

```
“param”：要求请求映射的请求必须为包含 param的请求参数
“!param”：要求请求映射的请求是不能包含param的请求参数
“param=value”：要求请求映射的请求必须包含 param 的请求参数，且 param 参数的值必须为 value
“param!=value”： 要求请求映射的请求是必须包含 param 的请求参数，其值不能为 value
```

- 设置params的属性值为username：

  ```java
  @RequestMapping(value = "/test",params = "username")
      public String test(){
          return "success";
      }
  ```

- 请求结果：

  ![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401161106952.png)

- 注意：我们设置了params属性，就意味着**该请求映射的请求必须包含username才能够请求成功。**

  ![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401161108674.png)

- **当给params属性设置多个属性值时，必须同时满足才能够请求成功，否则会出现下面的错误：**

  ![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401161109143.png)

---

##### （Ⅶ），headers属性

- @RequestMapping的headers属性是通过当前请求的请求头信息来匹配请求；

- @RequestMapping的headers属性是一个字符串类型的数组，可以通过下面四种表达是来设置匹配关系

```
“header”：要求请求映射的请求必须为包含 header的请求头信息
“!header”：要求请求映射的请求必须为不包含 header的请求头信息
“header=value”：要求请求映射的请求必须为包含 header的请求头信息，并且header的值必须为value
“header!=value”：要求请求映射的请求必须为包含 header的请求头信息，并且header的值必须不是value
```

- 设置请求头信息：

  ```java
   @RequestMapping(value = "/test",headers = "Host = localhost:8081")
      public String test(){
          return "success";
      }
  ```

- 请求结果：

  ![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401161113574.png)

- **注意：如果当前请求不满足headers属性，此时页面就会显示404错误，即资源未找到。**



---



---



## 6，Service层

- IService<T> 是对BaseMapper的进一步封装，一般很少用。但是很方便。

- ```java
  public interface UserService extends IService<User>{
  
  }
  ```



---

## 7，ServiceImpl层

- ServiceIMpl<M extends BaseMapper<T>,T> 是对BaseMapper的进一步封装，一般很少用。但是很方便。

- ```java
  //impl层
  //声明其为服务层
  @Service
  public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
      @Autowired
      UserMapper userMapper;
  
  }
  ```



---

## 8，Controller层

- ```java
  //声明这是一个控制层
  @Controller
  public class UserController {
   @Autowired
      UserService userService;
  @RequestMapping("/test")
      public String TestUser(@RequestParam("id") Long id){
      User user = userService.getById(id);
         System.out.print(user)
  }
  }
  ```

  

