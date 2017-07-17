# 1， 获取类

- 通过Object类中的getClass()方法

```
String str = "你好";
Class<?> cls = str.getClass();
```

- 通过具体的类名的位置

```
Class<?> cls = java.lang.String.class;
```

- 通过forName()

```
Class<?> cls = Class.forName("java.lang.String");
```

# 2， 通过类获取对象实例

```
// 该类须有无参构造器
Book book = (Book) cls.newInstance();
```

# 3, 属性文件内容操作

- 通过ResourceBundle获取classPath下的属性文件

```
ResourceBundle bundle = ResourceBundle.getBundle("test");
String city = bundle.getString("name");
```

- 通过Properties对象获取配置文件

```
Properties pro = new Properties();
pro.load(new FileInputStream(new File("./test.properties")));
String name = (String) pro.get("name");
```

- 使用Properties保存配置文件

```
Properties pro = new Properties();
pro.setProperty("name", "java");
pro.setProperty("study", "sdf");
pro.store(new FileOutputStream(new File("test.properties")), "one file");
```

# 4, 注解

- 获取类的注解

```
Class<?> cls = Class.forName("first.Book");
Annotation[] as = cls.getAnnotations();
for (Annotation a : as) {
      System.out.println(a);
}
```

- Annotation的三个作用范围

  - CLASS

  > 程序编译时起作用

  - RUNTIME

  > 程序运行时起作用

  - SOURCE

  > 在源代码中起作用

- 自定义Annotation

```
@Retention(value = RetentionPolicy.RUNTIME)
public @interface GetItem {
    // 设置属性内容
    String name() default "hello";
    String value();
}
```

- 获取指定的Annotation

```
Class<?> cls = Class.forName("first.Book");
GetItem annotation = cls.getAnnotation(GetItem.class);
System.out.println(annotation.name());
System.out.println(annotation.value());
```
