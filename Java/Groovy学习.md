# Groovy学习

-   变量

    -   申明变量:使用关键字`def`进行变量的申明,如`def num = 6`,也可不使用`def`关键字，如:`hello = "world"`(但建议使用关键字)

-   数值

    -   Groovy中使用`/`对数值进行运算时，其结果为浮点数，需要只获取整数部分时须使用`intdiv`，如：`6.intdiv(4)`

-   字符串

    -   可使用单引号:`'hello'`、双引号:`"hello"`和三引号:`"""hello"""`来分装字符串,其中使用单引号可包含多行文本
    
    -   在单引号封装的字符串的值就是字符串本身,而使用其他两种方式表示的字符串可能会被进一步解释，如`"${hello}"`中的`${hello}`将被解释为变量hello的值
    
    -   索引和索引段
    
        ```groovy
        def str = "hello world, I am dragonhht"
        println str[4]
        println str[-2]
        println str[1..4]
        println str[1..<4]
        println str[4..1]
        println str[1, 4, 6]
        ```
        
    -   基本操作

        ```groovy
        str = "hello world, I am dragonhht"
        println str * 3
        println str + ', haha'
        println str - 'world'
        println str.size()
        println str.length()
        // 统计字符的出现次数
        println str.count('l')
        println str.contains('dragonhht')
        ```
        
    -   比较字符串
    
        -   `<=>`:该方法类似于`str1.compareTo(str2)`方法，当等于时返回`0`,str1在str2之前时返回`-1`,str1在str2之后时返回`1`
        
        -   `==`: 用于比较两个字符串是否相等
        
-   列表

    -   定义及索引操作
    
        ```groovy
        def list = ["hello", 'world', 1, 4, ['dragonhht', 18], 100, true]
        println list
        println list[2]
        println list[-1]
        println list[0..2]
        println list[0..<2]
        println list[2..0]
        ```
        
    -   赋值(该处只使用操作符,不说明方法(如add, remove等))
    
        ```groovy
        // 赋值
        list[1] = 12
        list[2] = ['haha', false]
        // 追加
        list << 'I am dragonhht'
        list += 'good'
        // 连接两个列表
        list = list + [100, 300]
        // 删除元素
        list -= 'hello'
        println list
        ```

-   映射

    -   定义：`def map = ['hello': 'world', 'age': 18]`
    
    -   基本操作
    
        ```groovy
        // 获取
        println map['hello']
        println map.hello
        println map[66]
        // 赋值
        map['age'] = 20
        // 添加
        map['good'] = 'Groovy'
        ```    
-   方法

    -   定义:使用`def`进行申明
    
        ```groovy
        // 定义方法
        def sayHello() {
            println 'hello world'
        }
        // 调用方法
        sayHello()
        ```
    
    -   默认参数(给方法形参给定默认值)
    
        ```groovy
        // 方法含默认参数
        def sayHello(id, name = 'dragonhht', age = 18) {
            println "${id} and name is ${name}, age is ${age}"
        }
        sayHello(1, 'good')
        sayHello(2)
        ```
        
-   流程控制

    -   `while`
    
        ```groovy
        def index = 0
        while (index < 10) {
            index++
        }
        println index
        ```
        
    -   `for`
    
        ```groovy
        def list = [1, 3, 'hello', 9, 40]
        for (val in list) {
            println(val)
        }
        for (val in 1..9) {
            println(val)
        }
        for (val in 'hello world') {
            println(val)
        }
        ```
        
    -   `if`
    
        ```groovy
        if (index < 10) {
            println 'index < 10'
        } else {
            println 'index >= 10'
        }
        ```
    -   `switch`
    
        ```groovy
        def key = 3
        switch (key) {
            case 1:
                println 'index is 1'
                break
            case 2:
                println 'index is 2'
                break
            case 3:
                println 'index is 3'
                break
            case 4:
                println 'index is 4'
                break
            case 5:
                println 'index is 5'
                break
            case 6:
                println 'index is 6'
                break
            default:
                break
        }
        
        // 范围
        switch (key) {
            case 0..10:
                println 'key in 1..10'
                break
            case 11..20:
                println 'key in 11..20'
        }
        ```
        
-   闭包

    -   定义及调用
    
        ```groovy
        // 定义及调用闭包
        // 定义不带参数的闭包
        def say = {println 'hello'}
        say.call()
        // 定义带参数的闭包
        def fun = {name -> println "name is ${name}"}
        fun.call("dragonhht")
        // 使用隐参数it
        def fun1 = {println "name is ${it}"}
        fun1.call("hello")
        ```