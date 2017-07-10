# 1，配置

> 可使用properties，也可用yml，推荐使用yml格式的配置文件

```properties下的配置
server.port = 80
server.context-path = /test
```

```yml下的配置
server:
    port: 80
    context-path: /test
```

## 1,值的注入

### 1，几个值的注入

- 配置文件的配置

```yml
userName1: dragon
age: 18
text: "name: ${userName1} , age: ${age}"
```

- Java类
- ```java类
  //将配置文件中的userName的值注入
   @Value("${userName1}")
   private String userName;

   //将配置文件中的age的值注入
   @Value("${age}")
   private Integer age;

   //将配置文件中的text的值注入
   @Value("${text}")
   private String text;
  ```

### 2，作为类注入

- 配置文件

```yml
user:
  name1: dragon
  age: 18
```

- java类

```user类
//ConfigurationProperties获取前缀为user的配置,将前缀为user的值映射过来
@Component
@ConfigurationProperties(prefix = "user")
public class User {
    private String name1;
    private Integer age;

    public String getName1() {
        return name1;
    }

    public void setName1(String name) {
        this.name1 = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name1 + '\'' +
                ", age=" + age +
                '}';
    }
}
```

```引用
@Autowired
 private User user;
```

## 2，配置文件的选择

```application.yml，选取application-prod.yml文件
spring:
  profiles:
    active: prod
```

## 3，Controller的使用

@Controller     | 处理http请求
--------------- | --------------------------------------------------
@RestController | spring4之后加入的注解，原来返回json需要@ResponseBody加@Controller
@RequestMapping | 配置URL映射

@PathVariable | 获取URL中的数据
------------- | ---------
@RequestParam | 获取请求参数的值
@GetMapping   | 组合注解

## 4，数据库操作

### 1，maven中添加相应的jar包

```pom.xml
<!-- 数据库操作 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
```

### 2,基本配置

```application.yml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/BookManager
    username: root
    password: 123
 jpa:
    hibernate:
      ddl-auto: update
```

### 3,实体类

```stylus
@Entity
public class DataTest {

    //Id主键，GeneratedValue自增长
    @Id
    @GeneratedValue
    private Integer id;
    private String name;
    private Integer age;

    public DataTest() {

    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}
```

### 4，数据库的基本操作

- 添加依赖

  ```pom.xml
  <!-- 数据库操作 -->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-data-jpa</artifactId>
       </dependency>

       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.35</version>
           <scope>runtime</scope>
       </dependency>
  ```

```
public interface DataRepository extends JpaRepository<DataTest, Integer>{
}
```

```stylus

@RestController
public class DataController {

    @Autowired
    private DataRepository dataRepository;

    //查找所有
    @RequestMapping("/get")
    public List<DataTest> getData() {
        return dataRepository.findAll();
    }

    //添加
    @RequestMapping("/add")
    public DataTest add() {
        String name = "hj";
        Integer age = 3;
        DataTest dataTest = new DataTest();
        dataTest.setAge(age);
        dataTest.setName(name);

        return dataRepository.save(dataTest);
    }

    //查询一个
    @GetMapping(value = "/getone/{id}")
    public DataTest getone(@PathVariable("id") Integer id) {
        return dataRepository.findOne(id);
    }

    //更新
    @RequestMapping("/update")
    public DataTest updateOne() {
        DataTest dataTest = new DataTest();
        dataTest.setId(3);
        dataTest.setName("张三");
        dataTest.setAge(99);

        return dataRepository.save(dataTest);
    }

    //删除
    @RequestMapping("/del")
    public void del() {
        dataRepository.delete(3);
    }
}
```

### 5，事务管理

```service类中的方法
 @Transactional
    public void addTwo() {
        DataTest dataTest = new DataTest();
        dataTest.setAge(12);
        dataTest.setName("90");
        dataRepository.save(dataTest);

        DataTest dataTest_1 = new DataTest();
        dataTest_1.setAge(34);
        dataTest_1.setName("op90909090");
        dataRepository.save(dataTest_1);

    }
```

### 6，表单验证

> 基本操作与hibernate-vaildated 校验框架相同

```实体类中所要验证的字段
//验证age最小为18
    @Min(value = 18, message = "不得小于18")
    private Integer age;
```

```controller中的使用
//表单验证,在要验证的对象前加Valid注解，表示要验证的对象,验证的结果信息放入BindingResult对象中
    @GetMapping(value = "/formCheck")
    public DataTest formCheck(@Valid DataTest data, BindingResult bindingResult) {
        if (bindingResult.hasErrors()) {
            System.out.println(bindingResult.getFieldError().getDefaultMessage());
            return null;
        }

        return dataRepository.save(data);
    }
```

### 7，使用AOP处理请求

- 添加依赖

```pom.xml
<!-- aop依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>
```

- 切面文件

```stylus
@Aspect
@Component
public class AspectTest {

    @Before("execution(public * hht.dragon.Controller.DataController.getData(..))")
    public void log() {
        System.out.println("AOP 测试 Before");
    }

    @After("execution(public * hht.dragon.Controller.DataController.getData(..))")
    public void gf() {
        System.out.println("AOP 测试 After");
    }
}
```

也可以写成

```stylus
@Aspect
@Component
public class AspectTest {

 @Pointcut("execution(public * hht.dragon.Controller.DataController.getData(..))")
    public void log() {

   }

   @After("log()")
    public void af() {
       System.out.println("AOP 测试 After");
   }

   @Before("log()")
    public void bf() {
       System.out.println("AOP 测试 Before");
   }
   }
```
