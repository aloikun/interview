JAVA后台开发面试常见问题总结

注：问题之后的初、中、高 分别对应初级工程师、中级工程师、高级工程师级别
1	JAVA语言 
1.1	new String()和直接赋值有什么区别？ -----初
1.2	new出来的值可以改变吗？
1.3	StringBuilder和StringBuffer的区别？ -----初
1.4	runnable和thread你最常用哪种？为什么用这种？----- 初
1.5	collection下面有哪些子类？ -----初
1.6	ConcurrentHashMap（必问）有了解过吗？他是如何通过一个key找到对应的value的？ -----中
1.7	HashMap底层是怎么实现的？ArrayList底层怎么实现的？LinkedList底层怎么实现的？LinkedList和ArrayList的区别? ----- 初
1.8	介绍一下Java内存模型及内存管理   -----中
1.9	创建线程有几种方式？ -----初
1.10	HashMap是怎样通过一个Key找到对应的value    -----初
1.11	介绍一下虚拟机是怎么加载java的  ----- 中
1.12	双亲委派模式有了解吗？  -----中
1.13	内部类的特点，有什么好处。  -----中
1.14	多线程： -----中
​	
	创建线程的三种方式
		继承Thread
		实现Runnable接口
		excuter会创建一个线程池，可以去里面拿
	其他：
		用户访问其实就是多线程并发。
	     项目中的使用：
Hibernate中的sessionFactory是一个单例多线程对象
​		sessionFactory是线程安全的，多个线程可以并发访问sessionFactory，
​			从而获取Session实例。
​		用户可以为SessionFactory配置一个二级缓存插件。
​		用来存放工作单元被读过的数据，将来(工作单元)数据库事务
​		能共享这些数据。
​	session不是线程安全的，因此每个线程应该独自创立一个session
​		session有一个缓存，被称为一级缓存，存放当前工作单元加载的对象
​		每个session实例都有自己的缓存，且只能被当前工作单元访问。
​		session中缓存是不共享的
​	如果多个线程使用同一个session进行CRUD，就会产生数据存取的混乱。
1.15	jvm运行机制 ------中
​	java源文件-->编译器-->字节码文件-->jvm-->机器码
1.16	同步异步优缺点  -----初
​	同步：提交请求->等待服务器处理->处理完毕之前客户端无法做任何事
​	异步：请求通过事件触发->服务器处理-->浏览器可以继续做其他事情
1.17	synchronized和ReentrantLock区别 -----初
​	区别1：
​		ReentrantLock：除了普通锁功能锁投票，定时锁等候和中断锁等候
​		lock：如果获取了锁立即返回，如果别的线程持有锁，
​			当前线程则一直处于休眠状态，直到获取锁
​	区别2：
​	在资源竞争不是很激烈的情况下，Synchronized的性能要优于ReetrantLock，
​		但是在资源竞争很激烈的情况下，Synchronized的性能会下降几十倍，
​		但是ReetrantLock的性能能维持常态

1.18	HashMap底层 -----中
​	链表和数组的一体，新建一个map的时候是长度为128的。
​		长度16的数组，每个数组里面都是一个entry(长度8)
​		存：(key)
​			h = key.hashcode通过hash算法存起来(int h)
​			索引：i=h%Entry[].length，存在数组的第几位
​			entry[i] = value  value存在entry中
​		取：
​			h = key.hashcode
​			i = h%Entry[].length	，找到数组第几位
​			value = entry[i]
​			
1。hashMap可以有null的键，concurrentMap不可以有 
2。hashMap是线程不安全的
​	ConcurrentMap使用了重入锁保证线程安全。 
3。在删除元素时候，两者的算法不一样。
画出arraylist跟linkedList内存图-----初
​	
1.19	为什么arrayList查询快-----初
​	首先arraylist和linkedList的get方法实现不同。
​	arrayList的底层是数组，也就是内存中一片连续的空间。
​		可以直接通过数组下标获得元素。
​	LinkedList底层是二叉树实现，是一种双向循环链表，
​		内存中不是开辟的不是一段连续的空间。
​		每个元素都有下一个元素的地址。
1.20	OOP编程的特点。具体说一下你对多态的理解。----初


2	JAVAWEB

2.1	JSP和Servlet的区别 -----初
​	JSP(Java Server Pages)是JavaWeb服务器端的动态资源。本质上就是一个
servlet，当JSP页面首次被访问时，容器(Tomcat)会先把JSP编译成Servlet，然后再去执行servlet
2.2	Cookie与session	-----中
​	session底层依赖Cookie技术。
​	Cookie是由服务器创建，然后通过响应发送给客户端的一个键值对。
​		客户端会保存Cookie，并会标注出Cookie的来源(哪个服务器)。
​	当首次使用session时，服务器端要创建session，session是保存在服务器端，
​		而给客户端的session的id(一个cookie中保存了sessionId)。
2.3	cookie简介 -----中
​	当用户访问您的站点时，您可以利用 Cookie 保存用户首选项或其他信息，
​		这样，当用户下次再访问您的站点时，应用程序就可以检索以前保存的信息。
​	一般来说，cookie大小是4KB，一个服务器20个cookie，客户端限制300个。
​		不过有些浏览器会把cookie大小提高到8KB,提高到500个。
​	
​	
2.4	cookie登陆安全问题(cookie以纯文本形式在浏览器和服务器之间通信)  -----中
​	前端加密，后端再加密
​	cookie欺骗：前端cookie用MD5加密，然后被截获加密后的信息，然后将该信息
​		向服务器提交
​	cookie截获：(例如在某网站发布一个标签，让用户点击)
​		1.定位需要搜集cookie的网站，分析url。
​		2.对数据进行抓包
​		3.重构URL
2.5	Filter和Servlet的对比 ---初级
​	filter是servlet的一种变种,不能处理请求，也无法生成相应
​	请求-->filter预处理-->servlet处理-->filter对相应再进行处理


3	设计模式/操作系统/协议/linux/
3.1	有了解过哪些协议？介绍一下TCP三次握手及原因  -----中
3.2	TCP UDP 的区别
3.3	http协议报文（随机问几个基本的）-----初
3.3.1	请求报文(请求行/请求头/请求数据/空行)
​	请求行
​	求方法字段、URL字段和HTTP协议版本
​	例如：GET /index.html HTTP/1.1
​	get方法将数据拼接在url后面，传递参数受限
​	请求方法：
​	GET、POST、HEAD、PUT、DELETE、OPTIONS、TRACE、CONNECT
​		请求头(key value形式)
​			User-Agent：产生请求的浏览器类型。
​			Accept：客户端可识别的内容类型列表。
​			Host：主机地址
​		请求数据
​			post方法中，会把数据以key value形式发送请求空行
​			发送回车符和换行符，通知服务器以下不再有请求头
3.3.2	响应报文(状态行、消息报头、响应正文)
​		状态行
​		消息报头
​		响应正文
3.4	Linux如何查看日志？如果日志很大，会不会卡死？ ----- 中
3.5	加密算法： -----初
​	对称加密：
​		DES，AES	AES>DES(资源消耗和速度都优于)
​			可逆，包含明文和密文
​	Hash算法
​		MD5：不可逆

3.6	工厂模式
-前言：暴发户坐车时总是怪怪的：上Benz车后跟司机说“开奔驰车！”，坐上Bmw后他说
​		“开宝马车！”，坐上Audi说“开奥迪车！”。你一定说：这人有病！直接说开车不就行了！
​	作用：为创建对象提供过度接口
​	1.简单工厂模式
​		组成：
​			*工厂类
​			*抽象产品
​			*具体产品
​		步骤：
​			首先：抽象产品
​				public interface Car{public void drive(); }
​			其次：实际产品
​				各个车种类实现car，并且有自己的drive();
​			再次：
​				工厂类：根据传进来的参数，生产相对应的Car
​				public class Driver{
​					 public static Car driverCar(参数){
​						if(奔驰){return benchiRun;}
​						if(宝马){return bnwRun;}
​						if(..){return ..}
​					 }
​				}
​			最后：开车
​				Car car = Driver.driverCar("benz"); 		//坐上奔驰
​			car.drive();								//下命令开车
​	2.工厂方法模式
​	3.抽象工厂模式：
​		抽象工厂
​			{创建植物();创建水果();}
​		具体工厂implement 抽象工厂
​			具体工厂A
​			{创建植物A(){return 植物A};创建水果A(){return 水果A}}
​			具体工厂B
​			{创建植物B(){return 植物B};创建水果B(){return 水果B}}
​		标志接口
​			水果接口
​				水果A实现水果接口，水果B实现水果接口
​			植物接口
​				植物A实现水果接口，植物B实现水果接口
​		总结：
​			抽象工厂模式是用来创建多个产品的等级结构的。
​			抽象工厂一般有多个方法，创建一系列产品。
​	4.举例
​		mybatis中
​			sqlSessionFactory:
​				作用，很像是一个连接池，我们需要sqlsession的时候，
​				直接从工厂中取就行
3.7	装饰者模式(类似蝙蝠侠，将人传进去，装饰之后，拥有蝙蝠侠能力)
​	结构：
​		1.接口类	(一个规范接口)
​		2.具体类
​		3.装饰角色
​		4.具体装饰角色
​	步骤	(1.人类/2.即将成为蝙蝠侠的人/3.人传进去,变成蝙蝠侠/4.蝙蝠侠能力)
​		1：interface Sourceable {void method();}
​		2：class Source implements Sourceable{void method(){普通方法}}
​		3：	实际创建Decorator的时候，内部创建了source，而source是被强化过的了
​			class Decorator implements Sourceable {
​				Sourceable source;
​				Decorator(source){super();this.source = source;} 
​				//强化方法
​				void method{将原方法装饰成需要装饰的方法}
​			}
​		4:调用：将source传进去，得到装饰后的对象
​			obj = new Decorator(source);
​			obj.method();
​	装饰者模式：主要就是用来扩展类，扩展开放，修改关闭。类的继承虽然也能实现扩展行为，但是它的粒度没有装饰者模式细，而且当有多个类需要被一个行为装饰时，你可能需要写多个继承对象。
​	装饰者模式必然有一个公共的接口或抽象类，用来作为对象的传递。你需要根据接口实现基本的被装饰类(Person)，以及装饰类的公共接口(Decorator )以后所有的装饰类都是继承自公共的装饰类接口，内部实现。			
​		
3.8	命令模式
​	简介：
​		Invoker是调用者(司令员),Receiver是被调用者(士兵)，
​		MyCommand是命令,实现了Command接口，持有接收对象.
​	步骤：
​		1.interface Command {void exe();}				//命令接口

		2.	class MyCommand implements Command {		//创造自己的命令
	​			this.receiver = receiver;
	​			void exe(){receiver.action();}			//命令士兵干活	
	​		}
		3.	public class Receiver{void action(){干活}}	//士兵干活
		4.	public class Invoker {		//发号施令
			​	this.command = command;
			​	command.exe();
			}
	原理：司令员-->发号命令-->士兵干活
	好处：实现发出者和执行者的之间的解耦
			3.9	单例模式
			3.9.1	解释
单例模式只生成一个实例，所以减少了系统的性能开销，当一个对象的产生需要
比较多的资源时，如读取配置、产生其他依赖对象时，则可以通过在应用启动时
直接产生一个单例对象，然后用永久驻留内存的方式来解决。
	hibernate中，sessionFactory就是单例模式。
	spring中的bean在加载的时候会自动初始化一个实例，然后每次调用bean的时候，对象是注入的，而不是重新new。因此整个系统都是这个实例
			`<bean scope=singleton>`	默认单例,需要多里设置为scope=prototype
			3.9.2	单例模式代码
	饿汉：直接new一个对象(避免多线程问题，初始化就实例化，占据内存)
	懒汉：if(实例是空)==>进入静态锁方法==>判断实例是空==>获取实例
			​	public static Singleton getInstance() {
			​		if (singleton == null) {
			​			synchronized (Singleton.class) {
			​			   if (singleton == null) {
			​				  singleton = new Singleton();
			​			   }
			​			}
			​		}
			​		return singleton;
			​	}
	静态内部类：(new的时候用静态最终，get方法也用静态最终!!!)
	本类不静态，内部类静态最终
			​	private static class LazyHolder {
			​	   private static final Singleton INSTANCE = new Singleton();
			​	}
			​	public static final Singleton getInstance() {
			​	   return LazyHolder.INSTANCE;
			​	}
	 (静态内部类也是有一定的性能消耗)

3.10	责任链模式(Chain of Responsibility)
​	例子：
​		*员工请假(3个月)-->组长(无法处理)-->项目经理(无法处理)-->项目主管(处理成功)
​		*生活中实际例子：纳税(能处理的部分处理了，不能处理的部分往后抛)
​		*java框架：
​			Java Web中的过滤器链
​			springmvc中的拦截器链
​			Struts2中的拦截器栈
​			tomcat里面的拦截器
1.解释
使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止。
1)在职责链模式里，很多对象由每一个对象对其下家的引用而连接起来形成一条链。
2)请求在这条链上传递，直到链上的某一个对象处理此请求为止。
3)发出这个请求的客户端并不知道链上的哪一个对象最终处理这个请求，这使得
系统可以在不影响客户端的情况下动态地重新组织链和分配责任。
​	2.适用：
​	在以下条件下使用Responsibility 链：
​	? 有多个的对象可以处理一个请求，哪个对象处理该请求运行时刻自动确定。
​	? 你想在不明确指定接收者的情况下，向多个对象中的一个提交一个请求。
​	? 可动态指定一组对象处理请求。
​	3.结构
​	1.抽象处理者
​	abstract class Handler{
​		protected Handler successor;	//持有后继的责任对象？？	相关部门
​		public abstract void handleRequest();//处理请求的方法请假事件
​		getSuccessor(){return successor;}		//取值方法其他相关部门
​		void setSuccessor(){this.succr=succr}	//赋值方法，设置后继对象	设置其他部门
​		}
​		2.具体处理者
​		xxHandler extends Handler{
​			if(getSuccessor()!=null)	//判断是否有后继对象有就往后扔，
​				getSuccessor.handleRequest();
​			}else{
​				处理请求!		//没有后继对象就处理请求
​			}
​		3.客户端类：
​		public class Client {
​			public static void main(String[] args) {
​				//组装责任链(2个相关部门)
​				Handler handler1 = new ConcreteHandler();
​				Handler handler2 = new ConcreteHandler();
​				handler1.setSuccessor(handler2);	//设置后继对象
​				//提交请求
​				//由于有后继对象，会继续推给handler2处理请求
​				　　handler1.handleRequest();			
​				}
​			}
​	4.责任链优缺点
​		优点：
​		1)降低偶尔度，客户端无须知道哪一个对象处理请求。只需要会被正确处理即可
​		2)职责链可简化对象的相互连接。只需指向下一个，而无须指向所有
​		3)增强对象指派职责的灵活性：
​				通过动态修改或者增加来改变处理请求的职责。
​				(可以继续往下添加处理器，也可以修改其中处理器)
​		4)增加新的请求处理类很方便
​		缺点：
​		1)不能保证请求一定被接收。
​		2)由于循环调用，系统性能会受到一定影响
​	5.纯与不纯的职责链模式
​		纯的职责链：
​			一个请求必须被一个处理者对象所接收
​		不纯的职责链：
​			一个请求最终可以不被任何收端对象接收
​	6.javaweb过滤器链：
​		request -> filter1 -> filter2 ->filter3 -> .... -> request resource.
​	7.springMVC的拦截器处理链
​			拦截器1		拦截器2		拦截器3
​		req		==>		==>			==>
​				<==		<==			<==
​	8.struts2拦截器栈：基本原理同上
​	9.Tomcat内部过滤器采用责任链模式



4	数据库 
4.1	数据库索引有了解吗？说说他是怎么实现的？ -----初
4.2	sql查询出一个班级中每一组中的最高分数 ----- 初
4.3	复杂查询 多表查询 要求现场写sql语句——初
4.4	sql优化-----中
​	索引、缓存(redis)、分表
​	索引：查询量大
​		什么时候用索引
​			1.表记录多
​			2.不需要经常插入，删除，修改的表
​			3.表中数据不重复且分布不平均的字段
sql语句优化：
​	避免select* ，where条件中in、or、like、!= <> (is not null)
​		<>修改为 <and>
​		is not null改为 >=char(0)
​		in用exist代替
​		or用union all代替(两条select语句中间+union all)
​		尽量避免子查询
​		避免在where子句中对字段进行函数操作
​	1.如果from查询双表，大表在前，小表在后(基础表)
​	2.三表查询的时候，选择交叉表作为基础表
​	3.where条件是，出现索引字段的优先放在前面
​	4.查询量较大的时候，使用表连接替代IN，EXISTS，NOT IN，NOT EXISTS等

数据库索引 -----初
​	唯一索引：取值不能重复，经常查询的字段
​		create unique index my_unique_idx on myTable (id);
​	重复索引：取值可以重复，经常查询的字段
​		create index my_dup_idx on myTable(name);
​	联合索引：(老师和学生)	想要查询某个老师和某个学生是否存在师生关系
​		如果建立组合索引，那么查询条件就应该如下
​		select ... from myTable where id= .... and name =....
​		如果查询单字段，效率很很低
​		(ps:单索引是根据某索引查询，然后where去除条件，会导致顺序扫描
​			此时用联合索引比较快)
​		性别，年龄
​			
4.5	严禁使用的SQL -----初
​	在大范围的数据情况下，下面的用法，坚决杜绝使用。
​	1、光标：大数据范围的循环，Oracle不能很好的支持。会造成系统性能严重下降
​	2、函数：函数同光标一样一行行的执行，效率也非常低。
​	3、"!=", "!>", "!<", "NOT", "NOT EXISTS", "NOT IN", "NOT LIKE"，
​		like ‘%aaa%’等比较运算。他们都不会走索引。坚决反对使用。
​	4、不得建没有作用的事务：事务的启动需要一定的资源
​	5、Select Into创建表：请使用显示定义Create table。
​		特别的是不要在事务里创建表和临时表。
​	6、在Where字句中，Oracle的函数和字段一定的分离。坚决不得使用如下的写法：
​		where  Convert(varchar(10),fdate,112) = ‘2003-09-06’  或者
​		where  Substring(PNO,4,3) = ‘001’ 
​		他们都是表完全扫描。(即不得在where条件中使用函数，会导致全表扫描)
4.6	需要慎重使用的SQL
​	下面这些语句容易对服务器造成额外的负担，例如Order by 消耗了大量的CPU资源。
​	所以要避免采用它，当然不是说不用他们。有时需要将结果集排序显示就一定
​	要用到Order by,但不要用多余的Order by.
​	?	1.Order By：消耗了内存和CPU，在Temp中完成操作。
​	?	2.Group By：同Order BY
​	?	3.Having：同Order BY
​	?	4.Distinct：同Order BY
​	?	5.Union (尽量用Union All) ：同Order BY
​	?	6.in ：没有Between快。
​	?	7.视图：尽量少用。(只能简化sql语句，维护方便，对实际优化没好处)
​			说明：(每次都会动态生成)
​			它相当于一个虚拟表，本身并不存储数据，当sql在操作视图时
​		所有数据都是从其他表中查出来的。因此使用视图并不能将常用
​			数据分离出来，无法优化查询速度。
​			算法：
​			合并算法
​			创表vieTab+查询vieTab
​			实际上是将上面两个语句合并成一句，到实际的表格查询
​			临时表算法
​			提前把表查出来，放在一个临时表中，查询的时候查询临时表
总结：不管哪个算法，都会给数据库带来额外的开销，并且会导致使用索引困难
补充：复杂视图可以用存储过程代替
8.SELECT COUNT(*) ：用Exists更好
复杂视图和简单视图(DML数据操作语言，增删改)
​	视图优点：
​		1、可以选择性选取需要字段
​		2、简单查询 得到复杂结果
​		3、数据维护独立
​		4、相同数据可以产生不同视图
​	简单视图
​		只从单表里面取数据，不包含函数和数据组，可以实现DML操作
​	复杂视图
​		从多表里面取数据，包含函数和数据组，不可以实现DML操作
​		
4.7	谈谈你对数据库引擎的了解-----高
​	innoDB是MySQL的数据库引擎之一
​	
​	
4.8	数据库三大范式	-----初
​	1NF	每个属性不可再拆分；
​		例如如下
​		--------------------------------
​		不合格数据库(进货下面还能拆分)			
​			进货				销售
​		数量	单价		数量	单价
​		--------------------------------
​		合格数据库
​		进货数量	进货单价	销售数量	销售单价
​		--------------------------------------------
​	2NF	一个表中只能存一种数据，不可以把多种数据存到同一张表
​		函数依赖：存在x,y字段，符合y=f(x);
​		完全依赖：好几个字段完全一样，拆表
​		(例如真正订单分为：订单+订单详情)
​			如果订单直接将订单和订单详情合并，将会产生大量冗余数据
​	3NF	每一列数据都和主键直接相关，而不能间接相关
​		

4.9	sql四种事务级别 -----中
​						脏读	不可重复读	幻读
​	Read uncommitted	√			√		√
​	Read committed		×			√		√		oracle
​	Repeatable read		×			×		√		mysql
​	Serializable		×			×		×
​	

4.10	数据库锁:悲观锁和乐观锁(有点像是同步和异步的区别)  -----中
​	悲观锁
​		当某条记录正在修改，其他记录是无法执行的。
​		(会抛出异常或者等待解锁)
​		特点：数据严谨，效率低，难以处理高并发
​	乐观锁(乐观锁在web应用中占了很大优势)
​		每次去拿数据的时候都认为别人不会修改，
​		但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，
​		当某一个用户在修改某个数据的时候，
​		其他用户是可以再修改记录的。
​		这是因为，乐观锁是通过控制版本来实现(增加一个字段作为版本号)
​		用户每修改一次，版本号+1，版本号只增不减

4.11	mysql随机查询一条数据(一些常用方法) -----初
​	 order by rand() 就是随机排序 
​	 order by rand() limit 1 就是随机获取一行数据
​	 
4.12	数据库关键字执行顺序 -----中
​	from>where>group by>having>select>rownum>roder by

4.13	数据库函数 -----中
​	聚合函数max min avg count
​	分析函数todate tochar
​	
4.14	mangoDB-----高
​	是一个基于分布式文件存储的数据库
​	是一个介于关系数据库和非关系数据库之间的产品，
​		是非关系数据库当中功能最丰富，最像关系数据库的。

4.15	谈谈redis的认识-----中
​	Redis是一个速度非常快的非关系数据库(non-relational database)，
​		它可以存储键(key)与5种不同类型的值(value)之间的映射(mapping)，
​		可以将存储在内存的键值对数据持久化到硬盘.
redis,memcache
​	持久化方式：2种
​	由于redis数据都放在内存中，没配持久化，redis重启后数据就全丢失了
​	快照(默认)即rdb方案
​	默认redis是会以快照的形式将数据持久化到磁盘的(一个二进 制文件，dump.rdb)
​	数据集小，速度快，不安全，会因为断电丢失。				
​	aof
​	AOF就可以做到全程持久化，只需要在配置文件中开启（默认是no）
​	数据集大，速度慢，安全，不会因为断电丢失
4.16	HikariCP数据源-----中
​	号称性能最好，可以完美地PK掉其他连接池
4.17	Druid连接池-----中
​	阿里巴巴连接池：功能最多
​	1.Druid内置提供了一个功能强大的StatFilter插件，能够详细统计SQL的执行性能，这对于线上分析数据库访问性能
​	2.数据库密码加密。直接把数据库密码写在配置文件中，这是不好的行为，容易导致安全问题。DruidDruiver和DruidDataSource都支持PasswordCallback(自定义数据库密码加密解密)
​	3.属性跟dbcp连接池的差不多，不过加入了filters 监控
4.18	oracle存储过程 -----初
​	(CREATE OR REPLACE PROCEDURE 存储过程名)
​	把我们写的一些复杂的sql语句存储起来，类似java的封装方法。
​	正常的sql语句是一次编译，一次运行。
​	而sql存储，是可以一次编译，多次运行。
​	
4.19	oracle的游标 -----初
​	游标一些基础知识：
​	游标实际上是一种能从包括多条数据记录的结果集中每次提取一条记录的机制。
​	游标充当指针的作用。尽管游标能遍历结果中的所有行，但他一次只指向一行。
SQL的游标是一种临时的数据库对象，即可以用来存放在数据库表中的数据行副本，
也可以指向存储在数据库中的数据行的指针。游标提供了在逐行的基础上操作表中数据的方法
​	游标的结果集是由SELECT语句产生，如果处理过程需要重复使用一个记录集，
那么创建一次游标而重复使用若干次，比重复查询数据库要快的多。
​	oracle服务器-->执行sql语句-->检索行-->保存到游标-->一次处理一行
​	游标分类：
​	1，隐式游标：在 PL/SQL 程序中执行DML SQL 语句时自动创建隐式游标，名字固定叫sql。
​	2，显式游标：显式游标用于处理返回多行的查询。
​	3，REF 游标：REF 游标用于处理运行时才能确定的动态 SQL 查询的结果
​		
4.20	oracle列转行 -----中
​	聚合函数，分析函数，列转行decode,casewhen

4.21	oracle和mysql的区别-----初
​	1.oracle是大型数据库(40%)，mysql是中小型数据库(20%)
​	2.oracle支持大并发，oracle安全性也会高一些
​	3.主键自增
​		mysql有自动增长类型，主键可以指定自动增长
​		oracle没有自动增长类型，主键一般使用序列。插入记录的时候
​			把序列的下一个值赋给该字段即可
​	4.翻页：
​		mysql直接采用limit a,b;
​		oracle要用rownum表明字段位置。判断rownum<100...
​	5.空字段处理:
​		mysql非空字段里面也有空内容，
​		oracle里面非空字段不允许有空
​			oracle导入数据的时候，就要对空字段进行判断，
​			如果是null或者空字符串，需要改成一个空格的字符串
​	6.事务隔离级别+传播特性？
​		oracle:READ COMMITTED	读已提交
​			允许幻想读、不可重复读，不允许脏读
​		mysql:REPEATABLE READ	可重复读
​			允许幻想读，不允许不可重复读和脏读

5	SSH/SSM
5.1	讲下SpringMvc的核心入口类是什么,Struts1,Struts2的分别是什么
SpringMvc的是DispatchServlet,Struts1的是ActionServlet,Struts2的是StrutsPrepareAndExecuteFilter

5.2	2、SpringMvc的控制器是不是单例模式,如果是,有什么问题,怎么解决
是单例模式,所以在多线程访问的时候有线程安全问题,不要用同步,会影响性能的,解决方案是在控制器里面不能写字段

5.3	SpingMvc中的控制器的注解一般用那个,有没有别的注解可以替代
一般用@Conntroller注解,表示是表现层,不能用用别的注解代替.
5.4	@RequestMapping注解用在类上面有什么作用
用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

5.5	怎么样把某个请求映射到特定的方法上面
直接在方法上面加上注解@RequestMapping,并且在这个注解里面写上要拦截的路径
5.6	如果在拦截请求中,我想拦截get方式提交的方法,怎么配置
可以在@RequestMapping注解里面加上method=RequestMethod.GET

5.7	如果在拦截请求中,我想拦截提交参数中包含”type=test”字符串,怎么配置
可以在@RequestMapping注解里面加上params=”type=test”
5.8	我想在拦截的方法里面得到从前台传入的参数,怎么得到
直接在形参里面声明这个参数就可以,但必须名字和传过来的参数一样
5.9	如果前台有很多个参数传入,并且这些参数都是一个对象的,那么怎么样快速得到这个对象
直接在方法中声明这个对象,SpringMvc就自动会把属性赋值到这个对象里面
5.10	怎么样在方法里面得到Request,或者Session
直接在方法的形参中声明request,SpringMvc就自动把request对象传入
5.11	SpringMvc中函数的返回值是什么.
返回值可以有很多类型,有String, ModelAndView,当一般用String比较好
5.12	SpringMvc怎么处理返回值的
SpringMvc根据配置文件中InternalResourceViewResolver的前缀和后缀,用前缀+返回值+后缀组成完整的返回值
5.13	SpringMVC怎么样设定重定向和转发的
在返回值前面加”forward:”就可以让结果转发,譬如”forward:user.do?name=method4” 在返回值前面加”redirect:”就可以让返回值重定向,譬如”redirect:http://www.baidu.com”

5.14	 SpringMvc用什么对象从后台向前台传递数据的
通过ModelMap对象,可以在这个对象里面用put方法,把对象加到里面,前台就可以通过el表达式拿到
5.15	SpringMvc中有个类把视图和数据都合并的一起的,叫什么
ModelAndView
5.16	怎么样把ModelMap里面的数据放入Session里面
可以在类上面加上@SessionAttributes注解,里面包含的字符串就是要放入session里面的key
5.17	SpringMvc怎么和AJAX相互调用的
通过Jackson框架就可以把Java里面的对象直接转化成Js可以识别的Json对象 
具体步骤如下 
1.加入Jackson.jar 
2.在配置文件中配置json的映射 
3.在接受Ajax方法里面可以直接返回Object,List等,但方法前面要加上@ResponseBody注解
5.18	当一个方法向AJAX返回特殊对象,譬如Object,List等,需要做什么处理
要加上@ResponseBody注解

5.19	SpringMvc里面拦截器是怎么写的
有两种写法,一种是实现接口,另外一种是继承适配器类,然后在SpringMvc的配置文件中配置拦截器即可: 
​	<!-- 只针对部分请求拦截 -->
<mvc:interceptor>
   <mvc:mapping path="/modelMap.do" />
   <bean class="com.et.action.MyHandlerInterceptorAdapter" />
</mvc:interceptor>
5.20	讲下SpringMvc的执行流程
系统启动的时候根据配置文件创建spring的容器, 首先是发送http请求到核心控制器disPatherServlet，spring容器通过映射器去寻找业务控制器， 
使用适配器找到相应的业务类，在进业务类时进行数据封装，在封装前可能会涉及到类型转换，执行完业务类后使用ModelAndView进行视图转发，数据放在model中，用map传递数据进行页面显示。
5.21	、Spring MVC的简介：
1、可以插入的MVC架构。这中架构可以通过一：内置的spring web框架 二是：Struts Web框架 来实现。 
2、spring。xml中还可以通过策略接口来实现其框架高度的配置，即:可配置多种视图技术，如：jsp velocity tiles iTest POI 
3、Spring MVC 分离了控制器、模型对象、分派器以及处理程序对象的角色
5.22	22、Spring MVC的优点：
1、易于通view框架无缝集成，采用IOC便于测试 
2、典型的纯MVC构架，Struts是不完全基于MVC框架的 
3、与tapestry是纯正的Servlet系统，（这也是相对于Struts的优势）
5.23	struts2 基本知识-----初
5.23.1	包括3部分
​		1.核心控制器(FilterDispachter):控制流程和处理机制
​		2.业务控制器Action
​		3.业务逻辑组件
​		其中两个2、3是需要用户自己实现，
5.23.2	工作流程
​		同时编写相关配置文件来供核心控制器使用
​		请求-->servlet-->被一系列filter拦截
​		-->调用核心控制器(FD)询问ACtionMapper，如果调用某个Action
​		-->核心控制器把请求交给ActionProxy
​		-->通过配置文件找到相应的Action类
​		-->创建一个ActionInvocation的实例
​		-->之后再去调用Action
​		Action调用时，会涉及到相关拦截器的调用
5.24	说一下struts2工作原理：-----初
5.25	Struts2拦截器和过滤器  -----初
过滤器和拦截器的区别：
　　1.拦截器是基于Java的反射机制的，而过滤器是基于函数回调。
　　2.拦截器不依赖与servlet容器，过滤器依赖与servlet容器。
　　3.拦截器只能对action请求起作用，
​		而过滤器则可以对几乎所有的请求起作用。
　　4.拦截器可以访问action上下文、值栈里的对象，而过滤器不能访问。
　　5.在action的生命周期中，拦截器可以多次被调用，
​		而过滤器只能在容器初始化时被调用一次。
　　6.拦截器可以获取IOC容器中的各个bean，而过滤器就不行，
​		我们可以在拦截器里注入一个service，可以调用业务逻辑。

5.26	谈谈Struts2优点
​	1.不依赖于servlet API和struts API
​	2.拦截器：参数拦截注入
​	3.类型转换器，特殊请求参数类型转换成需要的类型
​	4.多种表现层技术：jsp.freeMarker,velocity
​	5.输入校验可以指定某个方法进行校验
​	6.提供全局范围，包范围和Action范围的国际化资源管理 

5.27	谈谈struts2核心控制器的作用
​	StrutsPrepareAndFilter		
​	作用：
​		负责拦截用户请求。然后我们可以对用户请求做一些处理
​			<url-pattern>/*	-->
​			<url-pattern>*.action
​		如果拦截多种请求，则用逗号隔开	
5.28	谈谈你对struts2值栈的认识-----初
​	原理??  
ActionContext、ServletContext、PageContext区别
​	1.ActionContext:action的上下文环境，可以获取request,session,servletContext
​	2.servletContext是全文域对象，一个web一个servlet，生命周期伴随整个web
​	3.pageContext是当前jsp内置对象，作用只针对当前页，页面结束就销毁

5.29	newFixedThreadPool和newCachedThreadPool有什么区别？ -----中
5.30	介绍一下IOC和AOP   -----初
5.31	讲一下springMVC一次请求的流程  -----初
5.32	springboot、springcloud有了解吗？ ----- 中
5.33	hibernate中对象的三个状态，以及session的生命周期  -----初
​	首先有三个状态，托管态，持久态，游离态。
​		托管态：当一个对象被创建
​		持久态：当对象被session管理起来，就处于持久态。
​			怎么被session管理起来呢。
​			调用get、load、update、saveOrUpdate
​		游离态：session关闭的时候，或者调用delete
​			那么游离态怎么变成持久态呢
​		重新调用update()、saveOrUpdate()
​		在没有任何变量重新引用游离态的时候，它将被gc在适当的时机回收
5.34	hibernate核心配置文件
​	hibernate.properties：配置常用的属性
​	hibernate.cfg.xml：配置加载hbm映射文件，配置缓存策略

5.35	hibernate中get和load区别
​	get-->(二级)缓存取-->没有数据库取	(不支持延迟加载)
​		如果取不到，返回null
​	load-->缓存取-->(支持延迟加载)
​		如果没有缓存-->判断是否是lazy方式
​		1.不是，数据库取
​		2.是，建立代理对象，对象的属性initialized = false，target = null，
​		在获得对象属性时，访问数据库，找到记录后，把记录复制到target上
​		即：target = 对象，然后initialized = true
​		只有在对象getId()之外的方法调用才会去访问数据库
​		能减轻数据库压力，提高性能
​		如果取不到，会抛出异常(objNotFoundExp)


5.36	spring  AOP	IOC     ID  -----初
​	IOC：spring框架将service对象统一管理起来
​		IOC是通过反射实现的。
​		IoC容器来管理对象的生命周期、依赖关系。可以解决一些代码耦合。
​		把对象生成放在xml定义里面
​	ID:依赖注入，spring将依赖注入的对象动态管理起来
​		setter注入：
​			我们可以增加一个setter函数来传入创建好的对象，避免了初始化对象。
​		构造器注入：创建一个构造器
​		接口注入：
​			首先创建接口：
​				public interface InjectFinder {
​					void injectFinder(MovieFinder finder);
​				}
​			其次实现接口：
​				class MovieLister implements InjectFinder{
​					public void injectFinder(MovieFinder finder) {
​					  this.finder = finder;
​					}
​				}
​			3.根据不同框架创建被依赖的MovieFinder的实现
​	AOP:
​	1.Before、after、afterReturning(发生异常不执行)、after Throwing、around(环绕)
​	2.如果要用AOP注解，需要开启<aop:aspectj-autoproxy proxy-target-class="true"/>

5.37	ioc在实际开发中是干嘛的就行  -----初
实际开发中aop怎么用，可以绕过aop一些杂的问题,具体点
​	ioc和aop有解耦合的作用
5.38	谈谈spring事务管理-----初
​	编程式事务: 通过编程来管理事务
​	声明式事务：将业务代码和事务分离，只要配置注解和xml
​	即不需要手动写事务。

5.39	mybatis动态sql语句怎么设定  -----初
​	一般通过if节点实现，通过ognl语法来实现，判断语句有where和and
​	
5.40	mybatis缓存(不好用)   -----中
​	一级缓存
​		放在session里面，默认就有
​	二级缓存
​		二级缓存放在命名空间里面
​		属性类需要实现Serializable序列化接口(用来保存对象的状态)

5.41	mybatis有哪些执行器  -----初

5.42	hibernate和mybatis区别  -----初
​	1.开发速度(hibernate的dao层开发速度快)
​	2.对象管理(抓取策略？？)
​		hibernate是完全对象管理，用面向对象的思想处理sql语言
​		Mybatis需要自己对对象进行详细管理
​	3.入门门槛:
​		mybatis容易掌握，hibernate
​	4.缓存机制：mybatis本身提供的缓存机制不佳
​		hibernate有更好的二级缓存，用户还可以使用第三方缓存
​	5.sql优化
​	6.数据库移植，
​		比如业务问题，用mysql移动到oracle，hibernate很容易就移植过去
​		hibernate则是把sql语句管理起来，用户可以不用管是哪一种数据库
​		但是mybatis数据库移植性不要，不同数据库需要写不同sql
6	情商问题
6.1	压力很大的时候你是怎么解决的  -----中
6.2	你周末一般干什么？
6.3	你最大的缺点是什么？你对加班怎么看？


7	扩展（负载均衡/高并发/集群/搜索）
以下为扩展内容。如果简历上出现使用过相关框架/技术可以针对性的提问：
7.1	介绍一下dubbo -----中  
7.2	dubbo(通讯，负载均衡，不用写死服务地址)  -----中
dubbo实现service层和web层的远程通信功能。
能够实现负载均衡
服务自动注册与发现，可以不用写死服务提供方地址
7.3	有做过ActiveMQ集群吗？ -----高
7.4	数据库集群-----高
​	可以采用不同数据库(不推荐)
​	在数据库源配置多个数据库，管理起来
7.5	分布式事物你怎么看？ ----- 中

7.6	谈谈solr全文检索框架  -----中
当我们访问购物网站的时候，我们就可以根据随意所想的内容查询出相关内容。但是这并不一定数据库的相关字段。这时候就跟全文检索工具有关了。solr里面带有IKAnalyzer分词器。它内部根据一定算法，把关键字智能切分。
solr是一个企业级搜索应用服务器。我在之前的项目中是这样的，把solr放到tomcat中运行。然后再solr应用下的web.xml配置中配置solrHome目录，这个solrHome目录是我们手动创建出来的。
solrHome：是solr存放索引的文件夹(里面的data文件夹就是存放索引文件的目录),是solr运行的主目录。solrHome下有多个SolrCore实例，每个SolrCore都提供单独的索引和索引服务。SolrCore下的collection是一个solrHome的目录。
如果我们需要查看某项服务的详情，可以进入solr的后台管理，然后选择相应的collection就可以看到。对了，这个后台管理我们可以用来测试一些查询的条件。
solrHome实例中中有2个配置文件是比较重要的，一个是scheme.xml，一个是solrconfig.xml,其中配置了读取data-config.xml,data-config.xml中配置了连接数据库的一些信息。
scheme.xml是配置相关于所在的配置文件。即：配置字段是否分词，是否索引，是否存储。
solrconfig.xml是配置相关索引库位置及第三方jar依赖位置的配置文件。
data文件夹就是存储索引文件的目录。	
上面是从配置上来说的，从代码方面上来说的话：首先，我们搜索，就不从数据库直接搜索，我们从索引库查询。然后，我们可以在这边设置一些过滤查询的条件，排序，分页，高亮。然后我们增删改的话，也是直接从索引库中修改的。
从页面设计上来讲的话，我们模拟了淘宝首页，中上方一个搜索框，往下一条栏目类别，中央一个轮播图，左侧放类别导航，右侧放一些友情链接、商城广告之类的。

7.7	ngnix    -----中
​	反向代理是什么
​		正向代理：客户端-->代理-->服务端
​			服务端不知道哪个客户端访问
​			代理对象：客户端
​		反向代理：
​			客户端不知道访问的是服务端的哪一个
​			代理对象：服务端
​	怎么分发请求：负载均衡，权重	
​			软负载均衡算法，5种，提到项目用到哪一种
7.8	ngnix和tomcat区别 -----中
​	ngnix
​		ngnix是一个高性能的HTTP和反向代理服务器,
​			也是一个IMAP/POP3/SMTP 代理服务器,
​		处理静态文件，索引文件以及自动索引；打开文件描述符缓冲．
​			无缓存的反向代理加速，简单的负载均衡和容错．
​			FastCGI，简单的负载均衡和容错．

7.9	zookeeper -----中
​	zookeeper是一种分布式协调技术
​	主要用来解决分布式环境当中多个进程之间的同步控制
​	zookeeper主要是将dubbo的服务信息给管理起来。
​		Provider: 暴露服务的服务提供方。
​		Consumer: 调用远程服务的服务消费方。
​		Registry: 服务注册与发现的注册中心。
​		Monitor: 统计服务的调用次调和调用时间的监控中心。
​		Container: 服务运行容器。
​				monitor(监控consumer和provider)
​				在内存中累计调用次数和调用时间
​		consumer(订阅服务)-->registry-->provider(提供服务)(Container)
​		
	zookeeper(负载均衡)
		可以对集群进行管理
		根据权重选举master。
		zookeeper在每个机器启动的时候，都会创建一个节点。
			一旦master挂掉，节点消失，zookeeper就会收到通知。
			zookeeper就会从slave中选出一个master顶上来干活

7.10	消息队列(ActiveMQ) -----中
7.10.1		消息队列可以削峰，
将拦截大量并发请求，这也是一个异步处理过程，
​		把请求放到队列里面，一个一个排好
​		后台业务根据自己的处理能力，从消息队列中主动的拉取请求消息进行业务处理。
7.10.2		利用消息队列实现服务器与客户端的通信。
​	消息队列设计一个简易的双人聊天程序(一个服务器，两个客户端)。
​	消息队列重点在于消息类型的匹配，客户端和服务端的“通信协议”的设计。
​		
	服务器端：接受客户端发来的任何消息，并根据其消息类型，
		转发给对应的客户端。同时，检测是否有退出标记，
		有则给所有的客户端发送退出标记，等待1s后，
		确定客户端都退出，删除消息队列，释放空间，并退出。
	客户端：A和B。A给B发送信息，先发给服务器，由服务器根据自定义协议
		转发该消息给B。同时B也可以通过服务器给A发消息

7.11	怎么处理redis高并发延迟更新问题-----高
​	为什么出现并发：(单线程)
​	redis是一种单线程机制的nosql数据库。由于单线程本身并没有锁的概念，
​	多个客户端并发访问的时候会出现问题。发生连接超时、数据转换错误、
​		阻塞、客户端关闭连接等。
​	解决办法：
​		*客户端将连接进行池化，同时对客户端读写redis操作采用内部锁synchronized
​		*服务端，利用setnx变相实现分布式锁		
7.12	集群  -----中
​	负载均衡(将负载尽可能的平均分摊处理)
​		(负载：应用程序负载，网络流量负载)
​	高可用(有机器挂掉，集群软件会将该系统任务分配到其他机器)

7.13	有没有做过秒杀功能？如果让你做你会怎么做？应该考虑哪些问题？