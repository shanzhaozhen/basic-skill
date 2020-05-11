## 10. Spring

### 10.1 什么是spring?

Spring是个java企业级应用的开源开发框架。Spring主要用来开发Java应用，但是有些扩展是针对构建J2EE平台的web应用。Spring框架目标是简化Java企业级应用开发，并通过POJO为基础的编程模型促进良好的编程习惯。

### 10.2 使用Spring框架的好处是什么？

* 轻量：Spring是轻量的，基本的版本大约2MB。
* 控制反转：Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。
* 面向切面的编程(AOP)：Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。
* 容器：Spring包含并管理应用中对象的生命周期和配置。
* MVC框架：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。
* 事务管理：Spring提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）。
* 异常处理：Spring提供方便的API把具体技术相关的异常（比如由JDBC，HibernateorJDO抛出的）转化为一致的unchecked异常。

### 10.3 Spring由哪些模块组成?

以下是Spring框架的基本模块：

* Coremodule
* Beanmodule
* Contextmodule
* ExpressionLanguagemodule
* JDBCmodule
* ORMmodule
* OXMmodule
* JavaMessagingService(JMS)module
* Transactionmodule
* Webmodule
* Web-Servletmodule
* Web-Strutsmodule
* Web-Portletmodule

### 10.4 核心容器（应用上下文）模块。

这是基本的Spring模块，提供spring框架的基础功能，BeanFactory是任何以spring为基础的应用的核心。Spring框架建立在此模块之上，它使Spring成为一个容器。

### 10.5 BeanFactory–BeanFactory实现举例。

Bean工厂是工厂模式的一个实现，提供了控制反转功能，用来把应用的配置和依赖从正真的应用代码中分离。
最常用的BeanFactory实现是XmlBeanFactory类。

### 10.6.XMLBeanFactory

最常用的就是org.springframework.beans.factory.xml.XmlBeanFactory，它根据XML文件中的定义加载beans。该容器从XML文件读取配置元数据并用它去创建一个完全配置的系统或应用。

### 10.7.解释AOP模块

AOP模块用于发给我们的Spring应用做面向切面的开发，很多支持由AOP联盟提供，这样就确保了Spring和其他AOP框架的共通性。这个模块将元数据编程引入Spring。

### 10.8.解释JDBC抽象和DAO模块。

通过使用JDBC抽象和DAO模块，保证数据库代码的简洁，并能避免数据库资源错误关闭导致的问题，它在各种不同的数据库的错误信息之上，提供了一个统一的异常访问层。它还利用Spring的AOP模块给Spring应用中的对象提供事务管理服务。

### 10.10.解释对象/关系映射集成模块。

Spring通过提供ORM模块，支持我们在直接JDBC之上使用一个对象/关系映射映射(ORM)工具，Spring支持集成主流的ORM框架，如Hiberate,JDO和iBATISSQLMaps。Spring的事务管理同样支持以上所有ORM框架及JDBC。

### 10.10.解释WEB模块。

Spring的WEB模块是构建在applicationcontext模块基础之上，提供一个适合web应用的上下文。这个模块也包括支持多种面向web的任务，如透明地处理多个文件上传请求和程序级请求参数的绑定到你的业务对象。它也有对JakartaStruts的支持。

### 10.11 为什么说Spring是一个容器？

因为用来形容它用来存储单例的bean对象这个特性。

### 10.12 Spring配置文件

Spring配置文件是个XML文件，这个文件包含了类信息，描述了如何配置它们，以及如何相互调
用。
### 10.13 什么是SpringIOC容器？

SpringIOC负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

### 10.14 IOC的优点是什么？

IOC或依赖注入把应用的代码量降到最低。它使应用容易测试，单元测试不再需要单例和JNDI查找机制。最小的代价和最小的侵入性使松散耦合得以实现。IOC容器支持加载服务时的饿汉式初始化和懒加载。

### 10.15 ApplicationContext通常的实现是什么?

* FileSystemXmlApplicationContext：此容器从一个XML文件中加载beans的定义，XMLBean配置文件的全路径名必须提供给它的构造函数。
* ClassPathXmlApplicationContext：此容器也从一个XML文件中加载beans的定义，这里，你需要正确设置classpath因为这个容器将在classpath里找bean配置。
* WebXmlApplicationContext：此容器加载一个XML文件，此文件定义了一个WEB应用的所有bean。
### 10.16 Bean工厂和Applicationcontexts有什么区别？

Applicationcontexts提供一种方法处理文本消息，一个通常的做法是加载文件资源（比如镜像），它们可以向注册为监听器的bean发布事件。另外，在容器或容器内的对象上执行的那些不得不由bean工厂以程序化方式处理的操作，可以在Applicationcontexts中以声明的方式处理。Applicationcontexts实现了MessageSource接口，该接口的实现以可插拔的方式提供获取本地化消息的方法。

### 10.17 一个Spring的应用看起来象什么？

* 一个定义了一些功能的接口。
* 这实现包括属性，它的Setter，getter方法和函数等。
* SpringAOP。
* Spring的XML配置文件。
* 使用以上功能的客户端程序。

依赖注入

### 10.18 什么是Spring的依赖注入？

依赖注入，是IOC的一个方面，是个通常的概念，它有多种解释。这概念是说你不用创建对象，而只需要描述它如何被创建。你不在代码里直接组装你的组件和服务，但是要在配置文件里描述哪些组件需要哪些服务，之后一个容器（IOC容器）负责把他们组装起来。

### 10.19 有哪些不同类型的IOC（依赖注入）方式？

* 构造器依赖注入：构造器依赖注入通过容器触发一个类的构造器来实现的，该类有一系列参数，每个参数代表一个对其他类的依赖。
* Setter方法注入：Setter方法注入是容器通过调用无参构造器或无参static工厂方法实例化bean之后，调用该bean的setter方法，即实现了基于setter的依赖注入。

### 10.20 哪种依赖注入方式你建议使用，构造器注入，还是Setter方法注入？

你两种依赖方式都可以使用，构造器注入和Setter方法注入。最好的解决方案是用构造器参数实现强制依赖，setter方法实现可选依赖。

SpringBeans

### 10.21 什么是Springbeans?

Springbeans是那些形成Spring应用的主干的java对象。它们被SpringIOC容器初始化，装配，和管理。这些beans通过容器中配置的元数据创建。比如，以XML文件中<bean/>的形式定义。
Spring框架定义的beans都是单件beans。在beantag中有个属性”singleton”，如果它被赋为TRUE，bean就是单件，否则就是一个prototypebean。默认是TRUE，所以所有在Spring框架中的beans缺省都是单件。
### 10.22 一个SpringBean定义包含什么？

一个SpringBean的定义包含容器必知的所有配置元数据，包括如何创建一个bean，它的生命周期详情及它的依赖。

### 10.23 如何给Spring容器提供配置元数据?

这里有三种重要的方法给Spring容器提供配置元数据。
XML配置文件。
基于注解的配置。
基于java的配置。

### 10.24 你怎样定义类的作用域?

当定义一个`<bean>`在Spring里，我们还能给这个bean声明一个作用域。它可以通过bean定义中的scope属性来定义。如，当Spring要在需要的时候每次生产一个新的bean实例，bean的scope属性被指定为prototype。另一方面，一个bean每次使用的时候必须返回同一个实例，这个bean的scope属性必须设为singleton。

### 10.25 解释Spring支持的几种bean的作用域。

Spring框架支持以下五种bean的作用域：
*  singleton:bean在每个Springioc容器中只有一个实例。
* prototype：一个bean的定义可以有多个实例。
* request：每次http请求都会创建一个bean，该作用域仅在基于web的SpringApplicationContext情形下有效。
* session：在一个HTTPSession中，一个bean定义对应一个实例。该作用域仅在基于web的SpringApplicationContext情形下有效。
* global-session：在一个全局的HTTPSession中，一个bean定义对应一个实例。该作用域仅在基于web的SpringApplicationContext情形下有效。
缺省的Springbean的作用域是Singleton.

### 10.26 Spring框架中的单例bean是线程安全的吗?

不，Spring框架中的单例bean不是线程安全的。

### 10.27 解释Spring框架中bean的生命周期。

* Spring容器从XML文件中读取bean的定义，并实例化bean。
* Spring根据bean的定义填充所有的属性。
* 如果bean实现了BeanNameAware接口，Spring传递bean的ID到setBeanName方法。
* 如果Bean实现了BeanFactoryAware接口，Spring传递beanfactory给setBeanFactory方法。
* 如果有任何与bean相关联的BeanPostProcessors，Spring会在postProcesserBeforeInitialization()方法内调用它们。
* 如果bean实现IntializingBean了，调用它的afterPropertySet方法，如果bean声明了初始化方法，调用此初始化方法。
* 如果有BeanPostProcessors和bean关联，这些bean的postProcessAfterInitialization()方法将被调用。
* 如果bean实现了DisposableBean，它将调用destroy()方法。

### 10.28 哪些是重要的bean生命周期方法？你能重载它们吗？

有两个重要的bean生命周期方法，第一个是setup，它是在容器加载bean的时候被调用。第二个方法是teardown它是在容器卸载类的时候被调用。
Thebean标签有两个重要的属性（init-method和destroy-method）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（@PostConstruct和@PreDestroy）。

### 10.29 什么是Spring的内部bean？

当一个bean仅被用作另一个bean的属性时，它能被声明为一个内部bean，为了定义innerbean，在Spring的基于XML的配置元数据中，可以在<property/>或<constructor-arg/>元素内使用<bean/>元素，内部bean通常是匿名的，它们的Scope一般是prototype。

### 10.30 在Spring中如何注入一个java集合？

Spring提供以下几种集合的配置元素：
* `<list>`类型用于注入一列值，允许有相同的值。
* `<set>`类型用于注入一组值，不允许有相同的值。
* `<map>`类型用于注入一组键值对，键和值都可以为任意类型。
* `<props>`类型用于注入一组键值对，键和值都只能为String类型。
### 10.31 什么是bean装配?
装配，或bean装配是指在Spring容器中把bean组装到一起，前提是容器需要知道bean的依赖关系，如何通过依赖注入来把它们装配到一起。
### 10.32 什么是bean的自动装配？
Spring容器能够自动装配相互合作的bean，这意味着容器不需要`<constructor-arg>`和`<property>`配置，能通过Bean工厂自动处理bean之间的协作。

### 10.33 解释不同方式的自动装配。

有五种自动装配的方式，可以用来指导Spring容器用自动装配方式来进行依赖注入。
*  no：默认的方式是不进行自动装配，通过显式设置ref属性来进行装配。
* byName：通过参数名自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byname，之后容器试图匹配、装配和该bean的属性具有相同名字的bean。
*  byType:：通过参数类型自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byType，之后容器试图匹配、装配和该bean的属性具有相同类型的bean。如果有多个bean符合条件，则抛出错误。
*  constructor：这个方式类似于byType，但是要提供给构造器参数，如果没有确定的带参数的构造器参数类型，将会抛出异常。
*  autodetect：首先尝试使用constructor来自动装配，如果无法工作，则使用byType方式。

### 10.34 自动装配有哪些局限性?

自动装配的局限性是：
* **重写**：你仍需用`<constructor-arg>`和`<property>`配置来定义依赖，意味着总要重写自动装配。
* **基本数据类型**：你不能自动装配简单的属性，如基本数据类型，String字符串，和类。
* **模糊特性**：自动装配不如显式装配精确，如果有可能，建议使用显式装配。

### 10.35 你可以在Spring中注入一个null和一个空字符串吗？

可以。Spring注解

### 10.36 什么是基于Java的Spring注解配置?给一些注解的例子

基于Java的配置，允许你在少量的Java注解的帮助下，进行你的大部分Spring配置而非通过XML文件。
以@Configuration注解为例，它用来标记类可以当做一个bean的定义，被SpringIOC容器使用。另一个例子是@Bean注解，它表示此方法将要返回一个对象，作为一个bean注册进Spring应用上下文。

### 10.37 什么是基于注解的容器配置?

相对于XML文件，注解型的配置依赖于通过字节码元数据装配组件，而非尖括号的声明。
开发者通过在相应的类，方法或属性上使用注解的方式，直接组件类中进行配置，而不是使用xml表述bean的装配关系。

### 10.38 怎样开启注解装配？

注解装配在默认情况下是不开启的，为了使用注解装配，我们必须在Spring配置文件中配置`<context:annotation-config/>`元素。

### 10.39 @Required注解

这个注解表明bean的属性必须在配置的时候设置，通过一个bean定义的显式的属性值或通过自动装配，若@Required注解的bean属性未被设置，容器将抛出BeanInitializationException。

### 10.40 @Autowired注解

@Autowired注解提供了更细粒度的控制，包括在何处以及如何完成自动装配。它的用法和@Required一样，修饰setter方法、构造器、属性或者具有任意名称和/或多个参数的PN方法。

### 10.41 @Qualifier注解

当有多个相同类型的bean却只有一个需要自动装配时，将@Qualifier注解和@Autowire注解结合使用以消除这种混淆，指定需要装配的确切的bean。

Spring数据访问

### 10.42 在Spring框架中如何更有效地使用JDBC?

使用SpringJDBC框架，资源管理和错误处理的代价都会被减轻。所以开发者只需写statements和queries从数据存取数据，JDBC也可以在Spring框架提供的模板类的帮助下更有效地被使用，这个模板叫JdbcTemplate（例子见这里here）

### 10.43 JdbcTemplate

JdbcTemplate类提供了很多便利的方法解决诸如把数据库数据转变成基本数据类型或对象，执行写好的或可调用的数据库操作语句，提供自定义的数据错误处理。

### 10.44 Spring对DAO的支持

Spring对数据访问对象（DAO）的支持旨在简化它和数据访问技术如JDBC，HibernateorJDO结合使用。这使我们可以方便切换持久层。编码时也不用担心会捕获每种技术特有的异常。
45.使用Spring通过什么方式访问Hibernate?
在Spring中有两种方式访问Hibernate：
控制反转HibernateTemplate和Callback。
继承HibernateDAOSupport提供一个AOP拦截器。

### 10.46 Spring支持的ORM

Spring支持以下ORM：
* Hibernate
* iBatis
* JPA(JavaPersistenceAPI)
* TopLink
* JDO(JavaDataObjects)
* OJB


### 10.47 如何通过HibernateDaoSupport将Spring和Hibernate结合起来？

用Spring的SessionFactory调用LocalSessionFactory。集成过程分三步：
* 配置theHibernateSessionFactory。
* 继承HibernateDaoSupport实现一个DAO。
* 在AOP支持的事务中装配。

### 10.48 Spring支持的事务管理类型

Spring支持两种类型的事务管理：
* 编程式事务管理：这意味你通过编程的方式管理事务，给你带来极大的灵活性，但是难维护。
* 声明式事务管理：这意味着你可以将业务代码和事务管理分离，你只需用注解和XML配置来管理事务。

### 10.49 Spring框架的事务管理有哪些优点？

它为不同的事务API如JTA，JDBC，Hibernate，JPA和JDO，提供一个不变的编程模式。
它为编程式事务管理提供了一套简单的API而不是一些复杂的事务API如
它支持声明式事务管理。
它和Spring各种数据访问抽象层很好得集成。

### 10.50 你更倾向用那种事务管理类型？

大多数Spring框架的用户选择声明式事务管理，因为它对应用代码的影响最小，因此更符合一个无侵入的轻量级容器的思想。声明式事务管理要优于编程式事务管理，虽然比编程式事务管理（这种方式允许你通过代码控制事务）少了一点灵活性。

Spring面向切面编程（AOP）

### 10.51 解释AOP

面向切面的编程，或AOP，是一种编程技术，允许程序模块化横向切割关注点，或横切典型的责任划分，如日志和事务管理。

### 10.52 Aspect切面

AOP核心就是切面，它将多个类的通用行为封装成可重用的模块，该模块含有一组API提供横切功能。比如，一个日志模块可以被称作日志的AOP切面。根据需求的不同，一个应用程序可以有若干切面。在SpringAOP中，切面通过带有@Aspect注解的类实现。

### 10.52 在SpringAOP中，关注点和横切关注的区别是什么？

关注点是应用中一个模块的行为，一个关注点可能会被定义成一个我们想实现的一个功能。
横切关注点是一个关注点，此关注点是整个应用都会使用的功能，并影响整个应用，比如日志，安全和数据传输，几乎应用的每个模块都需要的功能。因此这些都属于横切关注点。

### 10.54 连接点

连接点代表一个应用程序的某个位置，在这个位置我们可以插入一个AOP切面，它实际上是个应用程序执行SpringAOP的位置。

### 10.55 通知

通知是个在方法执行前或执行后要做的动作，实际上是程序执行时要通过SpringAOP框架触发的代码段。
Spring切面可以应用五种类型的通知：
* before：前置通知，在一个方法执行前被调用。
* after:在方法执行之后调用的通知，无论方法执行是否成功。
* after-returning:仅当方法成功完成后执行的通知。
* after-throwing:在方法抛出异常退出时执行的通知。
* around:在方法执行之前和之后调用的通知。

### 10.56 切点

切入点是一个或一组连接点，通知将在这些位置执行。可以通过表达式或匹配的方式指明切入点。

### 10.57 什么是引入?

引入允许我们在已存在的类中增加新的方法和属性。

### 10.58 什么是目标对象?

被一个或者多个切面所通知的对象。它通常是一个代理对象。也指被通知（advised）对象。

### 10.59 什么是代理?

代理是通知目标对象后创建的对象。从客户端的角度看，代理对象和目标对象是一样的。

### 10.60 有几种不同类型的自动代理？

BeanNameAutoProxyCreator
DefaultAdvisorAutoProxyCreator
Metadataautoproxying

### 10.61 什么是织入。什么是织入应用的不同点？

织入是将切面和到其他应用类型或对象连接或创建一个被通知对象的过程。
织入可以在编译时，加载时，或运行时完成。

### 10.62 解释基于XMLSchema方式的切面实现。

在这种情况下，切面由常规类以及基于XML的配置实现。

### 10.63 解释基于注解的切面实现

在这种情况下(基于@AspectJ的实现)，涉及到的切面声明的风格与带有java5标注的普通java类一致。

Spring的MVC

### 10.64 什么是Spring的MVC框架？

Spring配备构建Web应用的全功能MVC框架。Spring可以很便捷地和其他MVC框架集成，如Struts，Spring的MVC框架用控制反转把业务对象和控制逻辑清晰地隔离。它也允许以声明的方式把请求参数和业务对象绑定。

### 10.65 DispatcherServlet

Spring的MVC框架是围绕DispatcherServlet来设计的，它用来处理所有的HTTP请求和响应。

### 10.66 WebApplicationContext

WebApplicationContext继承了ApplicationContext并增加了一些WEB应用必备的特有功能，它不同于一般的ApplicationContext，因为它能处理主题，并找到被关联的servlet。

### 10.67 什么是SpringMVC框架的控制器？

控制器提供一个访问应用程序的行为，此行为通常通过服务接口实现。控制器解析用户输入并将其转换为一个由视图呈现给用户的模型。Spring用一个非常抽象的方式实现了一个控制层，允许用户创建多种用途的控制器。

### 10.68 @Controller注解

该注解表明该类扮演控制器的角色，Spring不需要你继承任何其他控制器基类或引用ServletAPI。

### 10.69 @RequestMapping注解

该注解是用来映射一个URL到一个类或一个特定的方处理法上。

### 10.70 返回Json用什么注解？

@ResponseBody

### 10.71 Spring事务支持的隔离级别

Spring 事务上提供以下的隔离级别: 
* ISOLATION_DEFAULT: 使用后端数据库默认的隔离级别
* ISOLATION_READ_UNCOMMITTED　: 允许读取未提交的数据变更，可能会导致脏读，幻读或不可重复读
*  ISOLATION_READ_COMMITTD : 允许读取为提交数据,可以阻止脏读，当时幻读或不可重复读仍可能发生
*  ISOLATION_REPEATABLE_READ: 对统一字段多次读取结果是一致的，除非数据是被本事务自己修改．可以阻止脏读，不可重复读，但幻读可能发生 
* ISOLATION_SERIALIZABLE :　完全服从ACID

### 10.1 讲讲 Spring 加载流程。

通过listener入口，核心是在AbstractApplicationContext的refresh方法，在此处进行装载bean工厂，bean，创建bean实例，拦截器，后置处理器等。

[Spring 框架的设计理念与设计模式分析](https://www.ibm.com/developerworks/cn/java/j-lo-spring-principle/)

### 10.2 讲讲 Spring 事务的传播属性

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

### 10.3 Spring 如何管理事务的

编程式和声明式

（同上）

### 10.4 Spring 怎么配置事务（具体说出一些关键的 xml 元素）

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

### 10.5 说说你对 Spring 的理解，非单例注入的原理？它的生命周期？循环注入的原理， aop 的实现原理，说说 aop 中的几个术语，它们是怎么相互工作的。

AOP与IOC的概念（即spring的核心）

* **IOC**：Spring是开源框架，使用框架可以使我们减少工作量，提高工作效率并且它是分层结构，即相对应的层处理对应的业务逻辑，减少代码的耦合度。而spring的核心是IOC控制反转和AOP面向切面编程。IOC控制反转主要强调的是程序之间的关系是由容器控制的，容器控制对象，控制了对外部资源的获取。而反转即为，在传统的编程中都是由我们创建对象获取依赖对象，而在IOC中是容器帮我们创建对象并注入依赖对象，正是容器帮我们查找和注入对象，对象是被获取，所以叫反转。

*  **AOP**：面向切面编程，主要是管理系统层的业务，比如日志，权限，事物等。AOP是将封装好的对象剖开，找出其中对多个对象产生影响的公共行为，并将其封装为一个可重用的模块，这个模块被命名为切面（aspect），切面将那些与业务逻辑无关，却被业务模块共同调用的逻辑提取并封装起来，减少了系统中的重复代码，降低了模块间的耦合度，同时提高了系统的可维护性。

核心组件：bean，context，core，单例注入是通过单例beanFactory进行创建，生命周期是在创建的时候通过接口实现开启，循环注入是通过后置处理器，aop其实就是通过反射进行动态代理，pointcut，advice等。

Aop相关：

[Java程序员从笨鸟到菜鸟之（七十四）细谈Spring（六）spring之AOP基本概念和配置详解](https://blog.csdn.net/csh624366188/article/details/7651702/)

### 10.6 Springmvc 中 DispatcherServlet 初始化过程。

入口是web.xml中配置的ds，ds继承了HttpServletBean，FrameworkServlet，通过其中的init方法进行初始化装载bean和实例，initServletBean是实际完成上下文工作和bean初始化的方法。

[Spring源码分析: SpringMVC启动流程与DispatcherServlet请求处理流程](http://www.mamicode.com/info-detail-512105.html)

### 10.7 springMVC的执行流程

springMVC是由dispatchservlet为核心的分层控制框架。首先客户端发出一个请求web服务器解析请求url并去匹配dispatchservlet的映射url，如果匹配上就将这个请求放入到dispatchservlet，dispatchservlet根据mapping映射配置去寻找相对应的handel，然后把处理权交给找到的handel，handel封装了处理业务逻辑的代码，当handel处理完后会返回一个逻辑视图modelandview给dispatchservlet，此时的modelandview是一个逻辑视图不是一个正式视图，所以dispatchservlet会通过viewresource视图资源去解析modelandview，然后将解析后的参数放到view中返回到客户端并展现。

### 10.10 事务的理解

**事物**具有**原子性**、**一致性**、**持久性**、**隔离性**。

* **原子性**：是指在一个事物中，要么全部执行成功，要么全部失败回滚。
* **一致性**：事物执行之前和执行之后都处于一致性状态。
* **持久性**：事物多数据的操作是永久性。
* **隔离性**：当一个事物正在对数据进行操作时，另一个事物不可以对数据进行操作，也就是多个并发事物之间相互隔离。