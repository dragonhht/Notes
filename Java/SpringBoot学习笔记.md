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

# 2, 集成Redis做数据缓存

## 1， 添加依赖

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

## 2， 项目开启缓存

> 在springboot的启动类添加注解 `@EnableCaching`

## 3, 在需缓存的地方使用缓存 `@Cacheable(value = "key")` value为数据库中的键， 必须写

# 3, 使用WebSocket

# 1， 方法一

- 依赖

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>

<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>webjars-locator</artifactId>
</dependency>
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>sockjs-client</artifactId>
    <version>1.0.2</version>
</dependency>
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>stomp-websocket</artifactId>
    <version>2.3.3</version>
</dependency>
```

- 配置

```
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfiguration extends AbstractWebSocketMessageBrokerConfigurer {

    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        config.enableSimpleBroker("/topic");
    }

    @Override
    public void registerStompEndpoints(StompEndpointRegistry stompEndpointRegistry) {
      // 设置websocket服务器
        stompEndpointRegistry.addEndpoint("/FlowBook").withSockJS();
    }
}
```

- 接受和发送消息

```
@Controller
@EnableWebSocket // 不开启则无法访问 @MessageMapping
public class WsController {

    @MessageMapping("/chat")
    @SendTo("/topic/greetings")
    public Message chat(Message message) {
        System.out.println(message);
        return message;
    }
}
```

- 前端代码

```
// 连接服务
var stompClient = null;
function connect() {
    var socket = new SockJS('http://localhost:8080/FlowBook/FlowBook');
    stompClient = Stomp.over(socket);
    stompClient.connect({}, function (frame) {
    console.log('Connected: ' + frame);
    stompClient.subscribe('/topic/greetings', function (greeting) {
    showMessage(JSON.parse(greeting.body).message);
    });
    });
}

// 发送消息
function sendMessage() {
        var text = $('.input_div textarea').val();
        stompClient.send("/chat", {}, JSON.stringify({'message': text}));
    }
```

# 2， 方法二（点对点）

- 配置

```
@Configuration
@EnableWebSocket
public class WebSocketConfig implements WebSocketConfigurer {

    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
        registry.addHandler(myHandler(), "/webSocketHandler").addInterceptors(new HttpSessionIdHandshakeInterceptor());
    }

    @Bean
    public WebSocketHandler myHandler() {
        return new book.flow.websocket.WebSocketHandler();
    }

}
```

- websocket握手

```
@Component
public class HttpSessionIdHandshakeInterceptor extends HttpSessionHandshakeInterceptor {

    @Override
    public boolean beforeHandshake(ServerHttpRequest request, ServerHttpResponse response, WebSocketHandler wsHandler, Map<String, Object> map) throws Exception {

        if (request instanceof ServletServerHttpRequest) {
            ServletServerHttpRequest serverHttpRequest = (ServletServerHttpRequest) request;
            HttpSession session = serverHttpRequest.getServletRequest().getSession();
            if (session != null) {
                System.out.println("Session:" + ((User)session.getAttribute("user")).getUserId());
                map.put("userId", ((User)session.getAttribute("user")).getUserId());
            }
        }
        return true;
    }

    @Override
    public void afterHandshake(ServerHttpRequest request, ServerHttpResponse response, WebSocketHandler wsHandler, Exception ex) {
        super.afterHandshake(request, response, wsHandler, ex);
    }
}
```

- 处理类

```
@Service
public class WebSocketHandler extends TextWebSocketHandler{

    //在线用户列表
    private static Map<Integer, WebSocketSession> users = new HashMap<>();
    //用户标识
    private static final String CLIENT_ID = "userId";


    @Override
    public void afterConnectionEstablished(WebSocketSession session) throws Exception {
        System.out.println("成功建立连接");
        Integer userId = getClientId(session);
        if (userId != null) {
            users.put(userId, session);
            // session.sendMessage(new TextMessage("成功建立socket连接"));
            System.out.println(userId);
            System.out.println(session);
        }
    }

    /**
     * 发送信息给指定用户
     * @param clientId
     * @param message
     * @return
     */
    public boolean sendMessageToUser(Integer clientId, TextMessage message) {
        if (users.get(clientId) == null) return false;
        WebSocketSession session = users.get(clientId);
        System.out.println("sendMessage:" + session);
        if (!session.isOpen()) return false;
        try {
            session.sendMessage(message);
        } catch (IOException e) {
            e.printStackTrace();
            return false;
        }
        return true;
    }


    @Override
    protected void handleTextMessage(WebSocketSession session, TextMessage message) throws Exception {
        System.out.println(message.getPayload());

        WebSocketMessage message1 = new TextMessage("server:"+message);
        /*try {
            session.sendMessage(message1);
        } catch (IOException e) {
            e.printStackTrace();
        }*/
    }

    private Integer getClientId(WebSocketSession session) {
        try {
            Integer clientId = (Integer) session.getAttributes().get(CLIENT_ID);
            return clientId;
        } catch (Exception e) {
            return null;
        }
    }
}
```

- controller调用

```
@PostMapping("/message")
    @ResponseBody
    public String sendMessage(Message message) {
        int toUser = message.getUserId();
        boolean hasSend = webSocketHandler.sendMessageToUser(toUser, new TextMessage(message.getMessage()));
        return "message";
    }
```

- 前端使用

```
var ws = new WebSocket("ws://localhost:8080/FlowBook/webSocketHandler");
    ws.onopen = function () {
        console.log("onpen");
        ws.send("{}");
    };
    ws.onclose = function () {
        console.log("onclose");
    };
    ws.onmessage = function (msg) {
        console.log(msg.data);
        showMessage(msg.data);
    }

    function sendMessage() {
        var text = $('.input_div textarea').val();
        var to = $('#toUser').val();
        $.post('../message',
            {
                message : text,
                userId : to
            },
        function (data) {
            console.log("data:" + data);
        })
    }
```
