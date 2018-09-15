# 容器

接口`org.springframework.context.ApplicationContext`表示Spring IoC容器，负责实例化，配置和组装bean。容器通过读取配置元数据获取有关要实例化、配置和组装的对象的指令。

# Bean概述

在容器内bean的定义包含以下信息:

-   `包限定的类名`：通常定义ben的实现类
-   `bean的行为元素`:包含bean的范围、生命周期等
-   `依赖项`：该bean所引用的依赖项
-   `设置其他属性配置`：如配置连接池bean中使用的连接数等

# Bean的实例化

-   使用静态方法实例化bean

    -   指定本类中的静态方法实例化对象(単例模式),官方实例配置如下

    ```
    <bean id="clientService"
        class="examples.ClientService"
        factory-method="createInstance"/>
    ```

    ```
    public class ClientService {
        private static ClientService clientService = new ClientService();
        private ClientService() {}

        public static ClientService createInstance() {
            return clientService;
        }
    }
    ```

    -   调用容器中其他类的方法实例化bean,官方实例如下

    ```
    <bean id="serviceLocator" class="examples.DefaultServiceLocator">
    </bean>

    <!-- 该bean的定义中class属性为空 -->
    <bean id="clientService"
        factory-bean="serviceLocator"
        factory-method="createClientServiceInstance"/>
    ```

    ```
    public class DefaultServiceLocator {

        private static ClientService clientService = new ClientServiceImpl();

        public ClientService createClientServiceInstance() {
            return clientService;
        }
    }
    ```



# Bean的作用域

在最新的Spring Framework中支持六种作用域，且其中有四种只适合于web的Spring ApplicationContext

-   `singleton`:単例（默认），定义该作用域后，在Spring IOC中只会存在一个共享的bean实例。所有对该bean的请求，只要匹配，都只会返回bean的同一实例
-   `prototype`：原型，定义该作用域后，每次对bean的请求（注入，或通过getBean获取）都会创建一个新的bean实例。
-   `request`：在HTTP请求时，每次请求都会有个对应的bean实例。该实例仅在本次请求内有效，当请求处理完毕后，request域的bean实例也将销毁
-   `session`：一个HTTPSession，对应一个bean实例
-   `application`：一个ServletContext，对应一个bean实例
-   `websocket`：一个WebSocket，对应一个bean实例
   