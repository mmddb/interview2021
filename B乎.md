Bean 是啥

注解作用：

1. **减少配置文件体积**（原来的 XML）
2. **java bean 的可读性，内聚性**



@**Autowired** Spring注解

自动装配，自动在代码上下文中找和他**类型**匹配（默认）的 Bean，自动注入到相应地方；如果有多个实现类，用 @Qualifier("classname")

@**Resource**: J2EE的注解, 默认 byName, 可以指定 name 和 type



下面三个会**被 Spring 使用类路径扫描**，**并且注入到 ApplicationContext 中**，技术相同，只是用途不同：

@**Component**：用来表示任何 Spring 管理的组件

@**Service**：用来标记 **服务层** 的类， 定义的 Bean 默认单例，@Scope("prototype") 可以使用 原型，即每次都会new一个新的出来。

@**Repository**：在 **持久层** 注释类



@ **Controller**：标记一个类是 Controller，然后配合 @RequestMapping, 

 **@RequestParam** 来定义 URL 请求和 Controller 方法间映射。

MultipartFile 用于文件



Swagger： @Api, @ApiOperation, @ApiResoponse, 

​         @ApiModel, @ApiModelProperty



接口参数校验： Post请求，根据实体类中定义的条件，

全局异常处理：@ControllerAdvice+@ExceptionHandler 注解处理异常 ？？





DELETE 语句执行删除的过程是每次从表中删除一行，并且同时将该行的删除操作作为事务记录在日志中保存以便进行进行回滚操作。

 TRUNCATE TABLE 则一次性地从表中删除所有的数据并不把单独的删除操作记录记入日志保存，删除行是**不能恢复**的。并且在删除的过程中不会激活与表有关的删除触发器。执行速度快。

**drop 语句将表所占用的空间全释放掉**。







# 计算机网络

握手与挥手

![截屏2021-09-29 18.45.11](/Users/jon/Library/Application Support/typora-user-images/截屏2021-09-29 18.45.11.png)

挥手：







short 最大值 **32767**

接口默认修饰法： 成员变量 **public static final**, 方法 ： **public abstract**

**重载**: 方法签名（名字和参数）不一样就行

**重写：** 签名一样，返回值类型小于等于，抛出异常小于等于，访问权限大于等于；（父类方法不 private） 



### 内部类：

内部类可以无条件访问外部类 **所有** **成员属性和方法**。

**匿名内部类限制**： 不能有 constructor（）

perterson算法 ： 解决临界区问题， 满足互斥，有限等待





**MVC**: Web APP 的模式； 3 个东西分别：负责数据库的存取，处理用户交互，业务逻辑；数据显示

好处：耦合性低（维护性高），面显示分离；





### **synchronized 原理**

synchronized的语义底层是通过一个 **monitor的对象** 来完成，其实 **wait/notify等方法也依赖于monitor对象**，这就是为什么**只有在同步的块或者方法中才能调用wait/notify**等方法，否则会抛出 java.lang.**IllegalMonitorStateException**的异常的原因。



ArrayList , 内部是 **object数组，因此只能存储 引用类型，不能存基础类型；动态扩容，会先创建一个两倍大小的数组，copy过去后扔掉旧数组，如果频繁扩容的话，效率会比较低。
