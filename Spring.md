### Spring  AOP

**原理**：首先，通过 **point cut** 描述信息，获取到 **join point**，也就是我们想要增强的点；对这些点增添 **advice**，（before，after return，after throwing, around(执行前后)

**实现**： 用 `@Aspect` 注解一个类，类中定义方法 advice， advice 上声明 point cut `@Before("execution(* pojo.Landlord.service())")`

**好处**：减少代码冗余，降低模块间耦合度，增加扩展性和可维护性

aspect( point cut 描述信息，一组规则, advice 增强),  

join point 程序执行的时间点，Spring 中总是 **方法的执行点**。 被 point cut 形容

advised target 织入 advice 后产生的代理类，

**织入时间**：AspectJ 编译器织入，类装载期织入； Spring 动态代理织入 JDK动态代理（为接口实现代理），CGLIB动态代理（为类）

@AspectJ 是一种使用 Java 注解来实现 AOP 的编码风格





### **A,B同时开启一个事务，B的插入已经提交了，那么A能否查到B插入的数据**

看不见**，A 也完成提交后**才能看见？？

 Mysql 在 Repeatable Read 下，解决了 **幻读**，

#### **MVCC 快照读** ： 

#### **next-key 锁 当前读**： 

 把 **当前数据行** ，**与上一条数据和下一条数据之间的间隙锁定**，**保证此范围内读取的数据是一致的**。

select * from t where a=1;属于快照读 

select * from t where a=1 lock in share mode;属于当前读

当前读：在 select 的时候需要用如下语法： select * from t for update (lock in share mode) 进入当前读



### Spring ， SB，SC

**Spring**: 是一个 生态系统，它包含了 **Spring Framework**，Spring Boot，Spring Cloud 等

**Spring Framework** 是整个生态的基石，核心是 控制反转，和 **面向切面**；并且 **针对开发的 Web层，业务层，持久层** 提供了一些易用的解决方案。 缺点：配置繁琐，并且需要配合容器使用

**SpringBoot** ：解决这种问题，**消除了配置**，**简化maven（打包好的 starters）**，**内嵌Tomcat容器支持**，可以**独立构建 Spring boot应用**；**Acututor** **监控**

**Spring Cloud**：**基于Spring Boot**的**微服务解决方案**，提供了针对微服务相关的一些解决方案，配**置管理，网关，注册中心，服务发现，服务调用，监控中心，**链路追踪； but 这些组件基本给于 HTTP 通信，效果一般，不如 RPC高效。

基本是 Spring框架的 **扩展**，

1. **保留** Spring 核心，IoC， AoP
2. **消除** Spring 应用所需的 XML 配置 ？？
3. 提供 **打包好的各种 starters** 依赖，简化 maven 依赖为 Spring-boot-starter-test
4. 独立构建 Spring boot 应用，打包成 jar
5. **内嵌容器支持**，默认 Tomcat，Jetty，Undertow 三种
6. **Acututor 监控**，提供 app 的运行状况信息，



### 创建一个 SpringBoot 应用

以idea为例，项目名字，选择 java版本，打包方式 -> 获取应用框架（Spring Initializer 勾选 sb 版本，选择需要的依赖）； 写一个 Controller类 用 @Controller 标注，配合 @RequestMapping 响应http请求。

### 

### JDK动态代理，CGLib

**JDK动态代理:** 利用 **拦截器**(拦截器必须**实现InvocationHanlder**)加上**反射机制**生成一个 **实现代理接口的匿名类**，在 **调用具体方法前调用InvokeHandler来处理**。 只能针对 **实现了接口的类** 生成代理。

**CGLiB动态代理:** 利用**ASM**开源包，**将代理对象类的class文件**加载进来，通过修改其字节码 **生成子类** 来处理，因此，该类或方法 不能被声明为 **final**





### Spring IoC， AoP

**控制反转，也叫依赖注入，利用了工厂模式** ，

**将对象交给容器** 管理，你只需要在**spring配置文件**，或者利用**注解** **配置相应的bean**，以及设置相关的属性，让spring容器管理对象的生命周期，并在我们需要的时候进行注入 （构造函数注入，字段注入 ； 用 @Autowired 按类型, @Resource 安名字）。

将程序中的交叉业务逻辑（比如**安全，日志，事务**等），**封装成一个切面**，然后**注入到目标对象**（具体业务逻辑）

实现AOP的技术，主要分为两大类：一是采用**动态代理**技术，通过 **point cut** 获取到 生成动态代理对象来执行方法，以取代原有对象行为的执行；二是采用**静态织入**的方式，引入特定的语法创建“方面”，从而使得编译器可以**在编译期间**织入有关“方面”的代码.   减少冗余，模块独立，提高扩展性，维护性



### 循环依赖

**可以返回一个半成品**

- **singletonObjects：** 一级缓存，存储单例对象，Bean **已经实例化**，**初始化完成**。
- **earlySingletonObjects：** 二级缓存，存储 singletonObject，**这个 Bean 实例化了，还没有初始化**。
- **singletonFactories：** 三级缓存，存储 singletonFactory。

初始化 B 是 getBean（A）去找需要注入的 A 实例；

实例化 A 是，A 对象被需要了，要返回一个A实例，如果没有的话，要进行初始化这个 A，此时可能调用其他的

两个池子：**一个成品池子，一个半成品池子**。能解决循环依赖的前提是：**spring开启了allowCircularReferences**，那么**一个正在被创建的 bean才会被放在半成品池子**里。在注入bean，向容器获取bean的时候，**优先向成品池子要，要不到，再去向半成品池子要**。

出现循环依赖一定是你的**业务设计有问题**。**高层业务和底层业务的划分不够清晰**，一般，业务的依赖方向一定是无环的，有环的业务，在**后续的维护和拓展**一定非常鸡肋

可以**放在 Setter 中进行注入**，





### Spring Boot的配置文件叫啥名？ 

**application.properties** ; 

**application.yml**： 多环境配置

@**ConfigurationProperties  来读取多个属性**， **@Value （"spring.datasource.url"）读取单个**

加载优先级：

1. **工程根目录**的**config**目录：`file:./config/`
2. **工程根目录**：`file:./`
3. 类路径的**config**目录：`classpath:/config/`
4. **类路径**：`classpath:/` （推荐使用）

**命令行参数** 优先级非常之高





### 用过哪些注解？

MVC: @Controller, @RestController, @CrossOrigin

方法映射的：@RequestMapping, @GetMapping, PostMapping, 

方法参数的：@PathVariable, @RequestBody

@Component, @Resource,  @Service, @Repository,  @Bean		 ？？

IoC: @Autowired, @Required；

API: @Api, @ApiOperation, @ApiModel，@ApiImplicitParam, @ApiResponse

mybatis： @MapperScan("com.jon.user.mapper")， @Select，@Result

网关：implements GlobalFilter

注册中心：@EnableEurekaServer

服务调用：@FeignClient



### mybatis的mapper有哪些规则？（名字，返回类型）

id, 参数类型，返回类型，排序，使用自动生成的 key， 主键属性



###  mybatis的分页插件用过没？



### #{}与${}的区别

#{} **表示一个占位符号**
通过#{}可以实现 preparedStatement 向占位符中设置值，自动进行 java 类型和 jdbc 类型转换，#{}可以有效防止 sql 注入。 #{}可以接收简单类型值或 pojo 属性值。 如果 parameterType 传输单个简单类型值，#{}括号中可以是 value 或其它名称。

$ {} **表示拼接** **sql 串**
通过$ {}可以将 parameterType 传入的内容拼接在 sql中且不进行 jdbc 类型转换， $ {}可以接收简单类型值或 pojo 属性值，如果 parameterType 传输单个简单类型值，$ {}括号中只能是 value。

大多数情况下，我们去参数的值都应该去使用#{}；





### MVC 模式是什么以及优缺点 

**一种应用设计模式**

**模型(model)** 它是 应用程序的**主体部分**，主要包括业务**逻辑模块**和**数据模块**。模型与数据格式无关，这样一个模型能为多个视图提供数据。由于应用于模型的代码只需写一次就可以被多个视图重用，所以减少了代码的重复性
**视图**(view) **用户与之交互的界面**、在 web 中视图一般由jsp,html组成
**控制器**(controller) **接收来自界面的请求并交给模型进行处理** 在这个过程中控制器不做任何处理只是起到了一个连接的作用

1、**降低耦合**。在MVC模式中，三个层各施其职，所以如果一旦哪一层的需求发生了变化，就只需要更改相应的层中的代码而不会影响到其他层中的代码。
2、**分工合作**。在MVC模式中，由于按层把系统分开，那么就能更好的实现开发中的分工。网页设计人员可进行开发视图层中的 JSP，而对业务熟悉的人员可开发业务层，而其他开发人员可开发控制层。
3、**组件重用** 。如控制层可独立成一个能用的组件，表示层也可做成通用的操作界面。可以为一个模型在运行时同时建立和使用多个视图。



 1、**增加了系统结构和实现的  复杂性**。对于简单的界面，严格遵循MVC，使模型、视图与控制器分离，会增加结构的复杂性，并可能产生过多的更新操作，降低运行效率。
2、**视图与控制器间的过于紧密的连接**。视图与控制器是相互分离，但确实联系紧密的部件，视图没有控制器的存在，其应用是很有限的，反之亦然，这样就妨碍了他们的独立重用。



##  Spring MVC怎么分层 

- **表示层**：**页面展示，请求分发**。 **DispatcherServle**t 分配请求給 ， **Controller** 调用 业务层
- **业务层**：业务处理接口和实现。 调用 
- **持久层**：数据访问和持久化。 被







## maven

| **验证 validate** | **验证项目** | **验证项目是否正确且所有必须信息是可用的**                   |
| ----------------- | ------------ | ------------------------------------------------------------ |
| **编译 compile**  | **执行编译** | **源代码编译在此阶段完成**                                   |
| **测试 Test**     | **测试**     | **使用适当的单元测试框架（例如 JUnit ）运行测试。**          |
| **包装 package**  | **打包**     | **创建JAR/WAR包如在 pom.xml 中定义提及的包**                 |
| **检查 verify**   | **检查**     | **对集成测试的结果进行检查，以保证质量达标**                 |
| **安装 install**  | **安装**     | **安装打包的项目到本地仓库，以供其他项目使用**               |
| **部署 deploy**   | **部署**     | **拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程** |

