## 4. Spring

### 4.1 讲讲 Spring 加载流程。

通过listener入口，核心是在AbstractApplicationContext的refresh方法，在此处进行装载bean工厂，bean，创建bean实例，拦截器，后置处理器等。

[Spring 框架的设计理念与设计模式分析](https://www.ibm.com/developerworks/cn/java/j-lo-spring-principle/)

### 4.2 讲讲 Spring 事务的传播属性

七种传播属性。
事务传播行为
所谓事务的传播行为是指，如果在开始当前事务之前，一个事务上下文已经存在，此时有若干选项可以指定一个事务性方法的执行行为。在TransactionDefinition定义中包括了如下几个表示传播行为的常量：

1. TransactionDefinition.PROPAGATION_REQUIRED：如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
2. TransactionDefinition.PROPAGATION_REQUIRES_NEW：创建一个新的事务，如果当前存在事务，则把当前事务挂起。
3. TransactionDefinition.PROPAGATION_SUPPORTS：如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
4. TransactionDefinition.PROPAGATION_NOT_SUPPORTED：以非事务方式运行，如果当前存在事务，则把当前事务挂起。
5. TransactionDefinition.PROPAGATION_NEVER：以非事务方式运行，如果当前存在事务，则抛出异常。
6. TransactionDefinition.PROPAGATION_MANDATORY：如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。
7. TransactionDefinition.PROPAGATION_NESTED：如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于TransactionDefinition.PROPAGATION_REQUIRED。

[全面分析 Spring 的编程式事务管理及声明式事务管理](https://www.ibm.com/developerworks/cn/education/opensource/os-cn-spring-trans/)

### 4.3 Spring 如何管理事务的

编程式和声明式

（同上）

### 4.4 Spring 怎么配置事务（具体说出一些关键的 xml 元素）

配置事务的方法有两种：

* 基于XML的事务配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- from the file 'context.xml' -- 
<beans xmlns="http://www.springframework.org/schema/beans"  
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
xmlns:aop="http://www.springframework.org/schema/aop"  
xmlns:tx="http://www.springframework.org/schema/tx"  
xsi:schemaLocation="  
http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd  
http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd  
http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd" 

<!-- 数据元信息 -- 
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close" 
<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"/ 
<property name="url" value="jdbc:oracle:thin:@rj-t42:1521:elvis"/ 
<property name="username" value="root"/ 
<property name="password" value="root"/ 
</bean 

<!-- 管理事务的类,指定我们用谁来管理我们的事务-- 
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager" 
<property name="dataSource" ref="dataSource"/ 
</bean  

<!-- 首先我们要把服务对象声明成一个bean  例如HelloService -- 
<bean id="helloService" class="com.yintong.service.HelloService"/ 

<!-- 然后是声明一个事物建议tx:advice,spring为我们提供了事物的封装，这个就是封装在了<tx:advice/>中 -->
<!-- <tx:advice/>有一个transaction-manager属性，我们可以用它来指定我们的事物由谁来管理。
默认：事务传播设置是 REQUIRED，隔离级别是DEFAULT -->
<tx:advice id="txAdvice" transaction-manager="txManager" 
<!-- 配置这个事务建议的属性 -- 
<tx:attributes 
<!-- 指定所有get开头的方法执行在只读事务上下文中 -- 
<tx:method name="get*" read-only="true"/ 
<!-- 其余方法执行在默认的读写上下文中 -- 
<tx:method name="*"/ 
</tx:attributes 
</tx:advice 

<!-- 我们定义一个切面，它匹配FooService接口定义的所有操作 -- 
<aop:config 
<!-- <aop:pointcut/>元素定义AspectJ的切面表示法，这里是表示com.yintong.service.helloService包下的任意方法。 -->
<aop:pointcut id="helloServiceOperation" expression="execution(* com.yintong.service.helloService.*(..))"/ 
<!-- 然后我们用一个通知器：<aop:advisor/>把这个切面和tx:advice绑定在一起，表示当这个切面：fooServiceOperation执行时tx:advice定义的通知逻辑将被执行 -->
<aop:advisor advice-ref="txAdvice" pointcut-ref="helloServiceOperation"/ 
</aop:config 

</beans>
```

* 基于注解方式的事务配置。

@Transactional：直接在Java源代码中声明事务的做法让事务声明和将受其影响的代码距离更近了，而且一般来说不会有不恰当的耦合的风险，因为，使用事务性的代码几乎总是被部署在事务环境中。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"  
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
xmlns:aop="http://www.springframework.org/schema/aop"  
xmlns:tx="http://www.springframework.org/schema/tx"  
xsi:schemaLocation="  
http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd  
http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd  
http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd" 

<bean id="helloService" class="com.yintong.service.HelloService"/ 
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager" 
<property name="dataSource" ref="dataSource"/ 
</bean>
<!-- 配置注解事务 -- 
<tx:annotation-driven transaction-manager="txManager"/ 
</beans>
```

[Spring系列面试题](https://www.jianshu.com/p/805d3cd24d51)

### 4.5 说说你对 Spring 的理解，非单例注入的原理？它的生命周期？循环注入的原理， aop 的实现原理，说说 aop 中的几个术语，它们是怎么相互工作的。

AOP与IOC的概念（即spring的核心）

* **IOC**：Spring是开源框架，使用框架可以使我们减少工作量，提高工作效率并且它是分层结构，即相对应的层处理对应的业务逻辑，减少代码的耦合度。而spring的核心是IOC控制反转和AOP面向切面编程。IOC控制反转主要强调的是程序之间的关系是由容器控制的，容器控制对象，控制了对外部资源的获取。而反转即为，在传统的编程中都是由我们创建对象获取依赖对象，而在IOC中是容器帮我们创建对象并注入依赖对象，正是容器帮我们查找和注入对象，对象是被获取，所以叫反转。

*  **AOP**：面向切面编程，主要是管理系统层的业务，比如日志，权限，事物等。AOP是将封装好的对象剖开，找出其中对多个对象产生影响的公共行为，并将其封装为一个可重用的模块，这个模块被命名为切面（aspect），切面将那些与业务逻辑无关，却被业务模块共同调用的逻辑提取并封装起来，减少了系统中的重复代码，降低了模块间的耦合度，同时提高了系统的可维护性。

核心组件：bean，context，core，单例注入是通过单例beanFactory进行创建，生命周期是在创建的时候通过接口实现开启，循环注入是通过后置处理器，aop其实就是通过反射进行动态代理，pointcut，advice等。

Aop相关：

[Java程序员从笨鸟到菜鸟之（七十四）细谈Spring（六）spring之AOP基本概念和配置详解](https://blog.csdn.net/csh624366188/article/details/7651702/)

### 4.6 Springmvc 中 DispatcherServlet 初始化过程。

入口是web.xml中配置的ds，ds继承了HttpServletBean，FrameworkServlet，通过其中的init方法进行初始化装载bean和实例，initServletBean是实际完成上下文工作和bean初始化的方法。

[Spring源码分析: SpringMVC启动流程与DispatcherServlet请求处理流程](http://www.mamicode.com/info-detail-512105.html)

### 4.7 springMVC的执行流程

springMVC是由dispatchservlet为核心的分层控制框架。首先客户端发出一个请求web服务器解析请求url并去匹配dispatchservlet的映射url，如果匹配上就将这个请求放入到dispatchservlet，dispatchservlet根据mapping映射配置去寻找相对应的handel，然后把处理权交给找到的handel，handel封装了处理业务逻辑的代码，当handel处理完后会返回一个逻辑视图modelandview给dispatchservlet，此时的modelandview是一个逻辑视图不是一个正式视图，所以dispatchservlet会通过viewresource视图资源去解析modelandview，然后将解析后的参数放到view中返回到客户端并展现。

### 4.10 事务的理解

**事物**具有**原子性**、**一致性**、**持久性**、**隔离性**。

* **原子性**：是指在一个事物中，要么全部执行成功，要么全部失败回滚。
* **一致性**：事物执行之前和执行之后都处于一致性状态。
* **持久性**：事物多数据的操作是永久性。
* **隔离性**：当一个事物正在对数据进行操作时，另一个事物不可以对数据进行操作，也就是多个并发事物之间相互隔离。