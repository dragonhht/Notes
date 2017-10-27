# 1,类级别注解
## 1, @Entity
> 映射实体类

## 2, @Tale(name="", catalog="", schema="")
	1.name: 映射表的名称
	2.catalog: catalog的名称
	3.schema: schema的名称

## 3, @Embeddable
> 表示非Entity类可以嵌入到另一个Entity类中作为属性而存在

# 2,属性级别注解
## 1,@Id
> 表示主键属性,如果多个属性使用了该注解则类要实现序列化接口

## 2,@Colume(length=8)
> 用于定义该字段详细定义
	1.length: 字段长度

## 3,@GeneratedValue(strategy=GenerationType,generator="")
> 用于定义主键生成策略

	1.strategy:表示主键生成策略
		1.GenerationType.AUTO:根据底层数据库自动选择(默认)

## 4,@Transient
> 表示该属性并非一个到数据库的一个映射,ORM框架将忽略该属性

# 3,映射关系
## 1,一对一

## 2,一对多

## 3,多对多


