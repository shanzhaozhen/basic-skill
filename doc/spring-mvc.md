## 11. SpringMVC

### 11.1 什么是SpringMvc？

SpringMvc是spring的一个模块，基于MVC的一个框架，无需中间整合层来整合。

### 11.2 Spring MVC的优点：

1. 它是基于组件技术的.全部的应用对象,无论控制器和视图,还是业务对象之类的都是 java组件.并且和Spring提供的其他基础结构紧密集成. 
2. 不依赖于Servlet API(目标虽是如此,但是在实现的时候确实是依赖于Servlet的) 
3. 可以任意使用各种视图技术,而不仅仅局限于JSP 
4. 支持各种请求资源的映射策略 
5. 它应是易于扩展的

### 11.3 SpringMVC工作原理？

1. 客户端发送请求到DispatcherServlet 
2. DispatcherServlet查询handlerMapping找到处理请求的Controller 
3. Controller调用业务逻辑后，返回ModelAndView 
4. DispatcherServlet查询ModelAndView，找到指定视图 
5. 视图将结果返回到客户端

### 11.4 SpringMVC流程？

1. 用户发送请求至前端控制器DispatcherServlet。 
2. DispatcherServlet收到请求调用HandlerMapping处理器映射器。
3. 处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。 
4. DispatcherServlet调用HandlerAdapter处理器适配器。 
5. HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。 
6. Controller执行完成返回ModelAndView。 
7. HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。 
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器。 
9. ViewReslover解析后返回具体View。 
10. DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。 
11. DispatcherServlet响应用户。

### 11.6、SpringMvc的控制器是不是单例模式,如果是,有什么问题,怎么解决？

是单例模式,所以在多线程访问的时候有线程安全问题,不要用同步,会影响性能的,解决方案是在控制器里面不能写字段。

### 11.7、如果你也用过struts2.简单介绍下springMVC和struts2的区别有哪些?

1. springmvc的入口是一个servlet即前端控制器，而struts2入口是一个filter过虑器。
2. springmvc是基于方法开发(一个url对应一个方法)，请求参数传递到方法的形参，可以设计为单例或多例(建议单例)，struts2是基于类开发，传递参数是通过类的属性，只能设计为多例。
3. Struts采用值栈存储请求和响应的数据，通过OGNL存取数据，springmvc通过参数解析器是将request请求内容解析，并给方法形参赋值，将数据和视图封装成ModelAndView对象，最后又将ModelAndView中的模型数据通过reques域传输到页面。Jsp视图解析器默认使用jstl。

### 11.8、SpingMvc中的控制器的注解一般用那个,有没有别的注解可以替代？

一般用@Conntroller注解,表示是表现层,不能用用别的注解代替。

### 11.9、 @RequestMapping注解用在类上面有什么作用？

是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

### 11.10、怎么样把某个请求映射到特定的方法上面？

直接在方法上面加上注解@RequestMapping,并且在这个注解里面写上要拦截的路径

### 11.11、如果在拦截请求中,我想拦截get方式提交的方法,怎么配置？

可以在@RequestMapping注解里面加上method=RequestMethod.GET

### 11.12、怎么样在方法里面得到Request,或者Session？

直接在方法的形参中声明request,SpringMvc就自动把request对象传入

### 11.13 我想在拦截的方法里面得到从前台传入的参数,怎么得到？

直接在形参里面声明这个参数就可以,但必须名字和传过来的参数一样

### 11.14 如果前台有很多个参数传入,并且这些参数都是一个对象的,那么怎么样快速得到这个对象？

直接在方法中声明这个对象,SpringMvc就自动会把属性赋值到这个对象里面。

### 11.15 SpringMvc中函数的返回值是什么？

返回值可以有很多类型,有String, ModelAndView,当一般用String比较好。

### 11.16 SpringMVC怎么样设定重定向和转发的？

在返回值前面加"forward:"就可以让结果转发,譬如"forward:user.do?name=method4" 在返回值前面加"redirect:"就可以让返回值重定向,譬如"redirect:http://www.baidu.com"

### 11.17 SpringMvc用什么对象从后台向前台传递数据的？

通过ModelMap对象,可以在这个对象里面用put方法,把对象加到里面,前台就可以通过el表达式拿到。

### 11.18 SpringMvc中有个类把视图和数据都合并的一起的,叫什么？

叫ModelAndView。

### 11.19 怎么样把ModelMap里面的数据放入Session里面？

可以在类上面加上@SessionAttributes注解,里面包含的字符串就是要放入session里面的key

### 11.20 SpringMvc怎么和AJAX相互调用的？

通过Jackson框架就可以把Java里面的对象直接转化成Js可以识别的Json对象。 

具体步骤如下 ：

1）加入Jackson.jar 

2）在配置文件中配置json的映射 

3）在接受Ajax方法里面可以直接返回Object,List等,但方法前面要加上@ResponseBody注解

### 11.21 当一个方法向AJAX返回特殊对象,譬如Object,List等,需要做什么处理？

要加上@ResponseBody注解

### 11.22 SpringMvc里面拦截器是怎么写的

有两种写法,一种是实现接口,另外一种是继承适配器类,然后在SpringMvc的配置文件中配置拦截器即可： 

``` xml
<!-- 配置SpringMvc的拦截器 -->

<mvc:interceptors>  

<!-- 配置一个拦截器的Bean就可以了 默认是对所有请求都拦截 -->  

<bean id="myInterceptor" class="com.et.action.MyHandlerInterceptor"></bean>  

<!-- 只针对部分请求拦截 -->  

<mvc:interceptor>    

    <mvc:mapping path="/modelMap.do" />    

    <bean class="com.et.action.MyHandlerInterceptorAdapter" /> 

</mvc:interceptor>

</mvc:interceptors>
```

### 11.23 讲下SpringMvc的执行流程

系统启动的时候根据配置文件创建spring的容器, 首先是发送http请求到核心控制器disPatherServlet，spring容器通过映射器去寻找业务控制器，使用适配器找到相应的业务类，在进业务类时进行数据封装，在封装前可能会涉及到类型转换，执行完业务类后使用ModelAndView进行视图转发，数据放在model中，用map传递数据进行页面显示。