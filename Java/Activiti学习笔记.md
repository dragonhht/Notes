#	1、项目基本实现操作

## 1、activiti.cfg.xml文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans   http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">

        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/activiti" />
        <property name="jdbcDriver" value="com.mysql.jdbc.Driver" />
        <property name="jdbcUsername" value="root" />
        <property name="jdbcPassword" value="123" />

        <property name="databaseSchemaUpdate" value="true" />

    </bean>

</beans>
```

## 2、 部署流程定义

- 使用RepositoryService部署流程定义

```
Deployment deployment =processEngine.getRepositoryService()   // 与流程定义与部署相关的service
                  	.createDeployment()             		// 创建一个部署对象
                    .name("我的流程")
                    .addClasspathResource("pool.bpmn")     // 从classpath的资源中加载, 一次只能加载一个文件`
                    .addClasspathResource("pool.png")
                    .deploy();                      		// 完成部署
        System.out.println(deployment.getId());
        System.out.println(deployment.getName());
```

## 3、启动流程实例

- 使用RuntimeService启动流程实例

```
String key = "helloword";
 ProcessInstance processInstance = processEngine.getRuntimeService()       // 与正在执行的流程实例和执行对象相关的service
          			.startProcessInstanceByKey(key);     // 使用流程定义的key启动流程,key对应bpmn文件中的id值，使用key值启动，默认是按照最新版本的流程定义启动
        System.out.println(processInstance.getId());        // 流程实例id
        System.out.println(processInstance.getProcessDefinitionId());        // 流程定义id
```

## 4、查询当前人的个人业务

- 使用TaskService查询任务

  ```
  String person = "张三";
  List<Task> list = processEngine.getTaskService()    // 与当前正在执行的任务管理相关的service
                  .createTaskQuery()          // 创建任务的查询对象
                  .taskAssignee(person)             // 指定个人业务查询,指定办理人
                  .list();
          if (list != null) {
              for (Task task : list) {
                  System.out.println(task.getId() + ":" + task.getName() + ":" + task.getCreateTime() + ":" + task.getAssignee());
              }
          }
  ```

## 5、完成任务

- 使用TaskService完成任务

```
String taskId = "5004";			// 任务ID
        processEngine.getTaskService()
                .complete(taskId);
        System.out.println("完成任务: " + taskId);
```



#	2、Activiti相关知识

##	1、bpmn文件

> ​	bpmn文件2.0结点是definitions节点，这个元素中可以定义多个流程定义（建议每个文件只包含一个流程定义,可简化维护难度). definitions元素最少也要包含xmlns和targetNamespace的声明，targetNamespace可以是任意值，它用来对流程实例进行分类

- 说明：流程定义文件有两部分组成
   	1.	bpmn文件(计算机执行用)
   	2.	展示流程图的图片(给用户查看)



##	2、部署流程定义

```
 // 引擎配置
    ProcessEngineConfiguration pec=ProcessEngineConfiguration.createProcessEngineConfigurationFromResource("activiti.cfg.xml");
    // 获取流程引擎对象
    ProcessEngine processEngine=pec.buildProcessEngine();
    
    Deployment deployment =processEngine.getRepositoryService()   // 与流程定义与部署相关的service
                  	.createDeployment()             		// 创建一个部署对象
                    .name("我的流程")
                    .addClasspathResource("pool.bpmn")     // 从classpath的资源中加载, 一次只能加载一个文件`
                    .addClasspathResource("pool.png")
                    .deploy();                      		// 完成部署
        System.out.println(deployment.getId());
        System.out.println(deployment.getName());
```

