# Bean的作用域

在Spring Framework中支持五中作用域，且其中有三种只适合于web的Spring ApplicationContext

-   `singleton`:単例（默认），定义该作用域后，在Spring IOC中只会存在一个共享的bean实例。所有对该bean的请求，只要匹配，都只会返回bean的同一实例
-   `prototype`：原型，定义该作用域后，每次对bean的请求（注入，或通过getBean获取）都会创建一个新的bean实例。
-   `request`：在HTTP请求时，每次请求都会有个对应的bean实例。该实例仅在本次请求内有效，当请求处理完毕后，request域的bean实例也将销毁
-   `session`：一个HTTPSession，对应一个bean实例
-   `global session`：一个全局的HTTP Session，对应一个bean实例


# Bean的生命周期

> Spring能精确地追踪`singleton`作用域下的bean，知道何时被创建、何时初始化完成、何时被销毁。但在`prototype`作用域下的bean，Spring只负责创建，在将实例交给客户端后，Spring将不在跟踪其生命周期

