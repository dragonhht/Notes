# 1，Mybatius的基本查询
## 1,查询

- 使用占位符

###### 说明
> #{}为占位符  
    parameterType:指定输入参数的类型  
    #{id}:其中id表示接收的参数，如果输入的参数是简单类型，则#{}中的参数名可以任意  
    resultType:指定sql输出结果所映射的java对象的类型,该处引用了别名

###### 代码

>  <select id="findDomById" parameterType="int" resultType="dom">  
    	select * from dom where id=#{id}  
    </select>

- 使用拼接(不建议使用)
 
###### 说明

> ${}:表示sql拼接，将接受的参数不加任何修饰的拼接在sql中，不建议使用${}  
    	${value}:value表示输入的参数内容，如果传入的参数是简单类型，则${}中只能使用value

###### 代码

> <select id="findDomById" parameterType="java.lang.String" resultType="dom">  
    	select * from dom where no like '%${value}%'  
    </select>  


## 2,添加
### 1,基本的添加语句

###### 说明

> parameterType:指定传入的参数为pojo类  
    #{}中指定pojo的属性名

###### 代码

> <insert id="insertDom" parameterType="dom">  
    	insert into dom(no) value(#{no})  
    </insert>

### 2,返回新添加的数据的id的插入（仅支持主键自增长）   
###### 说明

> last_insert_id():得到刚insert进去的记录的主键，值适合于自增长的主键  
    	keyProperty : 将查询到的主键放到parameterType指定的对象的指定属性中  
    	order：select last_insert_id()相对于insert语句的执行顺序  
    	resultType:指定select last_insert_id()的结果类型

###### 代码

> <insert id="insertDom" parameterType="dom">  
    	<selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">  
    		select last_insert_id()  
    	</selectKey>  
    	insert into dom(no) value(#{no})  
    </insert>

## 3，删除

###### 代码

> <delete id="delDom" parameterType="int">  
    	delete from dom where id=#{id}  
    </delete>

## 4，修改

###### 代码

> <update id="updateDom" parameterType="dom" >  
    	update dom set no=#{no} where id=#{id}  
    </update>
    
# 2,开发dao的两种方法

## 1,原始的dao层开发方法

###### 需编写dao的接口和相应的实现类

## 2,mapper代理开发方法(推荐使用)

#### 只需编写mapper接口类，和编写相应的xml文件

###### mapper接口类中方法的书写规则

1. 接口中的方法名与mapper.xml中的statement的id一致
2. 接口中方法的参数类型与mapper.xml的statement的parameterType值一致
3. 接口中方法的返回值的类型和mapper.xml的statement的resultType值一致


```
    public Dom findDomById(int id)throws Exception;
	public List<Dom> findDomByName(String name)throws Exception;
	public void insertDom(Dom dom)throws Exception;
	public void delDom(int id)throws Exception;
	public void updateDom(Dom dom)throws Exception;
```


###### mapper.xml文件的书写


```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- namespace命名空间，对Sql进行分类化管理 ,namespace等于mapper接口的地址-->
<mapper namespace="mapper.DomMapper">

<!-- 定义sql片段 
	一般基于单表来定义sql片段,在sql片段中不要包含where 
-->
<sql id="query_dom">
	<if test="value!=null and value!=''">
    	no like '%${value}%'
    </if>
</sql>

<!-- 定义resuleMap 
	将select id _id,no _no from dom where id=#{id}查询出来的列与
	Dom做相应的映射
	type:resultMap最终所映射的java对象
-->
<resultMap type="Dom" id="domResultMap">
	<id column="_id" property="id"/>
	<!-- result对普通列的映射对应 -->
	<result column="_no" property="no"/>
</resultMap>

    <!-- <select id="findDomById" parameterType="int" resultType="pojo.Dom">
    	select * from dom where id=#{id}
    </select> -->
    
    <select id="findDomById" parameterType="int" resultMap="domResultMap">
    	select id _id,no _no from dom where id=#{id}   	
    </select>
    
    
    <select id="findDomByName" parameterType="string" resultType="pojo.Dom">
    		select * from dom where no like '%${value}%'
    </select>
    <insert id="insertDom" parameterType="pojo.Dom">
    	<selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">
    		select last_insert_id()
    	</selectKey>
    	insert into dom(no) value(#{no})
    </insert>
    
    <!-- 删除dom -->
    <delete id="delDom" parameterType="int">
    	delete from dom where id=#{id}
    </delete>
    
    <!-- 更新 -->
    <update id="updateDom" parameterType="pojo.Dom" >
    	update dom set no=#{no} where id=#{id}
    </update>
    
</mapper>
```
## java调用代码


```
        //获取Mybatis的配置文件的流
		InputStream input=Resources.getResourceAsStream("MybatisConfig.xml");
		
		//创建会话工厂,传入mybatis的配置文件信息
		sqlSessionFactory=new SqlSessionFactoryBuilder().build(input);
		//通过工厂得到SQLSession
		SqlSession sqlSession=sqlSessionFactory.openSession();
		
		//创建DomMapper对象
		DomMapper domMapper=sqlSession.getMapper(DomMapper.class);
		
		//调用dommapper的方法
		Dom dom=domMapper.findDomById(5);
		
		sqlSession.close();
```

# 3,Mybatis的全局配置文件

```
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
"http://mybatis.org/dtd/mybatis-3-config.dtd">  
<!-- mybatis全局文件 -->
<configuration> 

<!-- 加载属性文件 ,建议不要再properties元素体内添加任何属性值，只将属性值定义在properties文件中，且名字具有特殊性，
            不然可能会被后面同名的属性调用 -->
<properties resource="jdbc.properties" ></properties>



<!-- 自定义别名 -->
<typeAliases>
	<!-- 针对单个别名定义 
		type:类型的路径
		alias:类型的别名	-->
	<!-- <typeAlias type="pojo.Dom" alias="dom"></typeAlias>  -->
	
	<!-- 批量别名定义 
		指定包名，mybatis会自动扫描包中的pojo类，自动定义别名，别名就是包名（首字母大小写都可以）
	-->
	<package name="pojo" />
	
	
</typeAliases>
       
    <environments default="development">  
        <environment id="development">
        	
        	<!-- 使用jdbc事务管理，事务控制由mybatis进行 -->  
            <transactionManager type="JDBC"/>
            
            <!-- 数据库连接由mybatis管理 -->  
            <dataSource type="POOLED">  
                <property name="driver" value="${jdbc.driver}"/>  
                <property name="url" value="${jdbc.url}"/>  
                <property name="username" value="${jdbc.user}"/>  
                <property name="password" value="${jdbc.password}"/>  
            </dataSource>  
        </environment>  
    </environments>  
     
     <!-- 加载映射文件 -->
     <mappers>
     	<!-- 通过resource加载单个的映射文件 -->
     	<!-- <mapper resource="Sqlmap/Dom.xml" />
     	<mapper resource="mapper/DomMapper.xml" /> -->
     	
     	<!-- 通过mapper接口加载映射文件 
     		需要将mapper接口类名和mapper.xml映射文件名称保持一致，且在同一个目录中
     		必须是使用mapper代理的方法才能用
     	-->
     	<!-- <mapper class="mapper.DomMapper" /> -->
     	
     	<!-- 批量加载(推荐使用)
     		指定mapper接口的包名
     		需要将mapper接口类名和mapper.xml映射文件名称保持一致，且在同一个目录中
     		必须是使用mapper代理的方法才能用
     	 -->
     	 <package name="mapper" />
     	
     </mappers>
      
</configuration> 
```




# 4,输入映射
# 5,输出映射
## 1,使用resultType
## 2，使用resultMap

```
<!-- 定义resuleMap 
	将select id _id,no _no from dom where id=#{id}查询出来的列与
	Dom做相应的映射
	type:resultMap最终所映射的java对象
-->
<resultMap type="Dom" id="domResultMap">
	<!-- id表示查询结果中的唯一的标识 
		column:查询出来的列名
		property:type中指定的pojo类的属性名
	-->
	<id column="_id" property="id"/>
	<!-- result对普通列的映射对应 -->
	<result column="_no" property="no"/>
</resultMap>

<!-- 使用resuleMap来进行输出的映射 
    	resultMap:指定定义的resultMap的id，如果这个resultMap在其他的mapper文件中，前面要加namespace
    -->
    <select id="findDomById" parameterType="int" resultMap="domResultMap">
    	select id _id,no _no from dom where id=#{id}   	
    </select>
```
# 6,动态sql

```
<!-- 定义sql片段 
	一般基于单表来定义sql片段,在sql片段中不要包含where 
-->
<sql id="query_dom">
	<if test="value!=null and value!=''">
    	no like '%${value}%'
    </if>
</sql>

 <select id="findDomByName" parameterType="string" resultType="pojo.Dom">
    		select * from dom 
    		
    		<!-- 动态sql 
    		sql拼接
    	-->
    	<!-- 不使用sql片段 -->
    	<!-- <where>
    		<if test="value!=null and value!=''">
    			no like '%${value}%'
    		</if>
    	</where> -->
    	
    	<!-- 引用sql片段,若 使用的sql片段不在本mapper.xml中，则需在引用的sql片段的id前加namespace-->
    	<where>
    		<include refid="query_dom"></include>
    		<!-- 还可引用其他的sql片段 -->
    	</where>
    	
    </select>
```

# 7,关系映射

## 1，一对一

### 1,使用resultMap

###### 定义resuleMap


## 2，一对多

## 3，多对多

# 8，延迟加载

# 9，查询缓存
## 1,一级缓存

## 2，二级缓存

# 10,与spring的整合



