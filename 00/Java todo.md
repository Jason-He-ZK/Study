### 基础

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1635685692683-52fddbb9-1793-4d0a-9ec4-3bc4c84252a7.png#clientId=u830d5a82-7067-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=416&id=jIK9U&margin=%5Bobject%20Object%5D&name=image.png&originHeight=717&originWidth=940&originalType=binary&ratio=1&rotation=0&showTitle=false&size=276659&status=done&style=none&taskId=u9976717b-4353-4c70-af31-aba9982bb5b&title=&width=545)
todo
java项目中的classpath到底是什么
https://www.cnblogs.com/law-luffy/p/11121528.html
https://blog.csdn.net/sinat_30973431/article/details/82556821

### 数据类型

**进制**

- 10 十进制
- 010 八进制
- 0x10 十六进制
- 0b10 二进制（JDK8之后）

**转换规则**
2. 如果整数型字面量没有超出 byte,short,char 的取值范围，可以直接将其赋值给byte,short,char 类型的变量；
5. byte,short,char 类型混合运算时，先各自转换成 int 类型再做运算；
6. 多种数据类型混合运算，各自先转换成容量最大的那一种再做运算；
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1635063520404-a3764cbc-a151-48bd-b3d2-78b251408edc.png#clientId=u7f00196a-ee57-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=437&id=qu56D&margin=%5Bobject%20Object%5D&name=image.png&originHeight=873&originWidth=1779&originalType=binary&ratio=1&rotation=0&showTitle=false&size=466973&status=done&style=none&taskId=u1a891956-d734-4041-93ee-5fa79d6d4b1&title=&width=889.5)

### 运算符

```java
byte a = 100;
// 以下两种方式等价，扩展运算符不改变类型
a += 199; 
a = (byte)(a + 199): 
// 与这个不同
a = a + 199:
```

位运算符

- <<  >>（不补零）  >>>（补零）

其他运算符

- new
- instanceof

### 控制语句

**选择语句（分支语句）**

- switch（注意case穿透）（可以用枚举类）

```java
switch(值){
//值允许是String（JDK8）、int（byte,short,char可以自动转换int,long可以强转）
case 值1:
  java语句;
  break;
default:
  java语句;
}
```

### 方法

#### 重载与覆盖

**重载（overload）**
和返回值类型无关，和修饰符列表无关
和形参有关
**重写（覆盖、override）**

1. 有继承关系的两个类
2. 具有相同方法名、返回值类型（或子类）、形式参数列表
3. 访问权限不能更低（public为最高）
4. 抛出异常不能更多（更宽泛）
   注意
   构造方法：无法继承，所以无法覆盖
   静态方法：覆盖没有意义（方法覆盖通常和多态联合起来）
   私有方法：无法覆盖

#### 可变长参数

替代参数个数：0~N个
参数列表中必须在最后一个位置上
只能有1个可变长度参数
可以当做一个数组来看待

```java
public static void m2(int a, String... args){
} 
```

### 包

公司域名倒序 + 项目名 + 模块名 + 功能名
package（package语句只允许出现在java源代码的第一行）
import（import语句只能出现在package语句之下，class声明语句之上）
import java.util.*;  该包中的所有类
java.lang.* 中的类，或者同包中的类，不需要import

### 访问权限控制

属性，方法：4个都能用
类，接口：public和默认能用，其它不行

| 访问控制修饰符 | 本类 | 同包 | 子类 | 任意位置 |
| -------------- | ---- | ---- | ---- | -------- |
| public         | √   | √   | √   | √       |
| protected      | √   | √   | √   | ×       |
| 默认           | √   | √   | ×   | ×       |
| private        | √   | ×   | ×   | ×       |

### 枚举

```java
方法名称	 描述
values()	以数组形式返回枚举类型的所有成员
valueOf()	将普通字符串转换为枚举实例
compareTo()	比较两个枚举成员在定义时的顺序
ordinal()	获取枚举成员的索引位置
```

```java
public enum Color {
    /**
     * 红色
     */
    RED("red", 1),
    ;
  
    private String name;
    private int value;

    Color(String name, int value) {
        this.name = name;
        this.value = value;
    }

    public String getName() { return name; }

    public void setName(String name) { this.name = name; }

    public int getValue() { return value; }

    public void setValue(int value) { this.value = value; }
}
```

### 随机

```java
// 方法一：种子数
Random random = new Random();
Random random1 = new Random(1000);
// [0,100)
int r1 = random.nextInt(100);

// 方法二：[0,1) 的double值
double r2 = Math.random();

// 方法三
long r3 = currentTimeMillis();
```

```java
RandomStringUtils.randomAlphanumeric(3);

```

### 时间

```java
// 生成
LocalDateTime now = LocalDateTime.now();
LocalDateTime someDate = LocalDateTime.of(1998, 9, 12, 9, 21, 32);
// 获取
int year = now.getYear();
// 修改
now = now.plusYears(2);
now = now.withYear(1998);
// 格式化时间
DateTimeFormatter dateTimeFormatter1 = DateTimeFormatter.ISO_DATE_TIME;
DateTimeFormatter dateTimeFormatter2 = DateTimeFormatter.ofPattern("yyyy/MM/dd HH:mm");
String dateStr = now.format(dateTimeFormatter2);
// 反解析
LocalDateTime parse = LocalDateTime.parse("2002/01/02 11:21", dateTimeFormatter2);
```

### 字符串

格式化

```java
// 0代表前面补充0，4代表长度为4，d代表参数为正数型
String str = String.format("%04d", youNumber);
```

### IO

Java7自动关闭流

```java
try (
    FileReader fileReader = new FileReader("/a.txt");
) {
    int read = fileReader.read();
} catch (Exception e) {
    e.printStackTrace();
}
```

### 异常

catch多个异常

```java
try {
  
} catch (ArrayIndexOutOfBoundsException | NumberFormatException e) {
    e.printStackTrace();
}
```

### 序列化

transient int age; 忽略序列化
private static final long serialVersionUID = -111111111111111111L;

### 反射

```java
final Class<?> aClass = Class.forName("learn.Person");
final Class<Person> aClass1 = Person.class;
final Class<? extends Person> aClass2 = new Person().getClass();
```

### 注解

注解

@NotNull *(* groups= *{* AddGroup.class, UpdateGroup.class*})*

@NotNull:不能为null，但可以为empty,没有Size的约束

@NotEmpty 用在集合类上面 加了@NotEmpty的String类、Collection、Map、数组，是不能为null或者长度为0的(String Collection Map的isEmpty()方法)

@NotBlank只用于String,不能为null且trim()之后size>0



### 线程

```java
// 方法一：继承Thread接口
class My01 extends Thread {
    @Override
    public void run() {
        System.out.println("My01");
    }
}
Thread thread01 = new My01();


// 方法二：实现Runnable接口
class My02 implements Runnable {
    @Override
    public void run() {
        System.out.println("My02");
    }
}
Thread thread02 = new Thread(new My02());


// 方法三：实现Callable接口
class My03 implements Callable<Integer> {
    @Override
    public Integer call() throws Exception {
        System.out.println("My03");
        return 1;
    }
}
FutureTask<Integer> futureTask = new FutureTask<>(new My03());
```

设置为守护线程（必须在 thread.start()之前设置）
thread01.setDaemon(true)

### 泛型

### 接口 抽象类

### 集合

#### 比较（Comparable，Comparator）

```java
// 方法一：实现Comparable接口（Comparable是默认的比较接口，需要和比较的对象紧密结合到一起）
public int compareTo(Object o) {
    return this.age - ((Person) o).age;
}


// 方法二：新建实现Comparator接口的比较类，构建集合时传入（可以分离比较规则，更具灵活）
class MyComparator implements Comparator<Integer> {
    @Override
    public int compare(Integer o1, Integer o2) {
        return o1 - o2;
    }
}
Comparator myComparator = new MyComparator();
Set set = new TreeSet(MyComparator);
```

---

## Json

### FastJson

#### 序列化和反序列化

[https://www.cnblogs.com/jajian/p/10051901.html](https://www.cnblogs.com/jajian/p/10051901.html)

```java
<dependency>
     <groupId>com.alibaba</groupId>
     <artifactId>fastjson</artifactId>
     <version>x.x.x</version>
</dependency>
```

```java
String jsonString = JSON.toJSONString(obj);

VO vo = JSON.parseObject("...", VO.class);

import com.alibaba.fastjson.TypeReference;
List<VO> list = JSON.parseObject("...", new TypeReference<List<VO>>() {});


```

输出对象obj的null值，SerializerFeature的几个枚举值，可多选
WriteNullListAsEmpty		将Collection类型字段的字段空值输出为[]
WriteNullStringAsEmpty		将字符串类型字段的空值输出为空字符串 ""
WriteNullNumberAsZero	将数值类型字段的空值输出为0
WriteNullBooleanAsFalse	将Boolean类型字段的空值输出为false
日期

```java
JSON.toJSONStringWithDateFormat(date, "yyyy-MM-dd HH:mm:ss.SSS")
```

#### 定制序列化

##### @JSONField（可加在属性，方法上）

```java
package com.alibaba.fastjson.annotation;

public @interface JSONField {
    // 配置序列化和反序列化的顺序，1.1.42版本之后才支持
    int ordinal() default 0;
    // 指定字段的名称
    String name() default "";
    // 指定字段的格式，对日期格式有用
    String format() default "";
    // 是否序列化
    boolean serialize() default true;
    // 是否反序列化
    boolean deserialize() default true;
}
```

指定序列化格式

```java
public static class Model {
    @JSONField(serializeUsing = ModelValueSerializer.class)
    public int value;
}

public static class ModelValueSerializer implements ObjectSerializer {
    @Override
    public void write(JSONSerializer serializer, Object object, Object fieldName, Type fieldType,
                      int features) throws IOException {
        Integer value = (Integer) object;
        String text = value + "元";
        serializer.write(text);
    }
}
```

```java
Model model = new Model();
model.value = 100;
String json = JSON.toJSONString(model);
Assert.assertEquals("{\"value\":\"100元\"}", json);
```

##### @JSONType（配置在类上）

##### SerializeFilter

##### ParseProcess（反序列化）

#### 在 Spring MVC 中集成 Fastjson

## 并发/多线程

并发编程基础

### 线程池

#### 构造参数

```java
public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
	BlockingQueue<Runnable> workQueue,ThreadFactory threadFactory,RejectedExecutionHandler handler);
```

| corePoolSize
 | 必需 | 核心线程数 | 线程池中一直保持存活的线程数，即使这些线程处于空闲。

| 但是将allowCoreThreadTimeOut参数设置为true后，核心线程处于空闲一段时间以上，也会被回收  |        |                             |                                                                                                                                                                                           |
| --------------------------------------------------------------------------------------- | ------ | --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| maximumPoolSize                                                                         | 必需   | 池中允许的最大线程数        | 当核心线程全部繁忙且任务队列打满之后，线程池会临时追加线程，直到总线程数达到maximumPoolSize这个上限                                                                                       |
| keepAliveTime                                                                           | 必需   | 线程空闲超时时间            | 当非核心线程处于空闲状态的时间超过这个时间后，该线程将被回收。                                                                                                                            |
| unit                                                                                    | 必需   | keepAliveTime参数的时间单位 | TimeUnit.DAYS（天）、TimeUnit.HOURS（小时）、TimeUnit.MINUTES（分钟）、TimeUnit.SECONDS（秒）、TimeUnit.MILLISECONDS（毫秒）、TimeUnit.MICROSECONDS（微秒）、TimeUnit.NANOSECONDS（纳秒） |
| workQueue                                                                               | 必须   | 任务队列                    | 采用阻塞队列实现。                                                                                                                                                                        |
| 当核心线程全部繁忙时，后续由execute方法提交的Runnable将存放在任务队列中，等待被线程处理 |        |                             |                                                                                                                                                                                           |
| threadFactory                                                                           | 非必须 | 线程工厂                    | 指定线程池创建线程的方式                                                                                                                                                                  |
| handler                                                                                 | 非必须 | 拒绝策略                    | 当线程池中线程数达到maximumPoolSize且workQueue打满时，后续提交的任务将被拒绝，handler可以指定用什么方式拒绝任务                                                                           |

任务队列

- SynchronousQueue：同步队列。这是一个内部没有任何容量的阻塞队列，任何一次插入操作的元素都要等待相对的删除/读取操作，否则进行插入操作的线程就要一直等待，反之亦然。
- LinkedBlockingQueue：无界队列（严格来说并非无界，上限是Integer.MAX_VALUE），基于链表结构。使用无界队列后，当核心线程都繁忙时，后续任务可以无限加入队列，因此线程池中线程数不会超过核心线程数。这种队列可以提高线程池吞吐量，但代价是牺牲内存空间，甚至会导致内存溢出。另外，使用它时可以指定容量，这样它也就是一种有界队列了。
- ArrayBlockingQueue（使用较少）：有界队列，基于数组实现。在线程池初始化时，指定队列的容量，后续无法再调整。这种有界队列有利于防止资源耗尽，但可能更难调整和控制。

线程工厂

- 默认提供的线程工厂

```java
DefaultThreadFactory() {
    SecurityManager s = System.getSecurityManager();
    group = (s != null) ? s.getThreadGroup() :
    Thread.currentThread().getThreadGroup();
    namePrefix = "pool-" +
        poolNumber.getAndIncrement() +
        "-thread-";
}
```

拒绝策略

- AbortPolicy（默认）：丢弃任务并抛出RejectedExecutionException异常。
- CallerRunsPolicy：直接运行这个任务的run方法，但并非是由线程池的线程处理，而是交由任务的调用线程处理。
- DiscardPolicy：直接丢弃任务，不抛出任何异常。
- DiscardOldestPolicy：将当前处于等待队列列头的等待任务强行取出，然后再试图将当前被拒绝的任务提交到线程池执行。

#### 状态

- RUNNING：当创建线程池后，初始时，线程池处于RUNNING状态；
- SHUTDOWN：如果调用了shutdown()方法，则线程池处于SHUTDOWN状态
  （此时线程池不能够接受新的任务，它会等待所有任务执行完毕）
- STOP：如果调用了shutdownNow()方法，则线程池处于STOP状态
  （此时线程池不能接受新的任务，并且会去尝试终止正在执行的任务）
- TERMINATED：当线程池处于SHUTDOWN或STOP状态，并且所有工作线程已经销毁，任务缓存队列已经清空或执行结束后，线程池被设置为TERMINATED状态。

#### 执行方法

execute()
submit()：还是调用的execute()方法，能够返回任务执行的结果

#### 使用

```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

public class MyTest {
	public static void main(String[] args) {
		// 创建线程池
		ThreadPoolExecutor threadPool = new ThreadPoolExecutor(3, 5, 5, TimeUnit.SECONDS,
				new ArrayBlockingQueue<Runnable>(5));
		// 向线程池提交任务
		for (int i = 0; i < threadPool.getCorePoolSize(); i++) {
			threadPool.execute(new Runnable() {
				@Override
				public void run() {
					for (int x = 0; x < 2; x++) {
						System.out.println(Thread.currentThread().getName() + ":" + x);
						try {
							Thread.sleep(2000);
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
					}
				}
			});
		}

		// 关闭线程池
		threadPool.shutdown(); // 设置线程池的状态为SHUTDOWN，然后中断所有没有正在执行任务的线程
		// threadPool.shutdownNow(); // 设置线程池的状态为STOP，然后尝试停止所有的正在执行或暂停任务的线程，并返回等待执行任务的列表，该方法要慎用，容易造成不可控的后果
	}
}

```

#### 四种预先定义好的线程池

- CachedThreadPool:可缓存的线程池
- SecudleThreadPool:周期性执行任务的线程池
- SingleThreadPool:只有一条线程来执行任务
- FixedThreadPool:定长的线程池

锁
并发容器
原子类
juc并发工具类

并发编程
	线程和进程
	线程状态
	并行和并发
	同步和异步
	Synchronized
	Volatile 关键字
	Lock 锁
	死锁
	可重入锁
	线程安全
	线程池
	JUC 的使用
	AQS
	Fork Join
	CAS

## JVM

类加载机制
字节码执行机制
jvm内存模型
GC垃圾回收
jvm性能监控与故障定位
jvm调优

JVM
JVM 内存结构
JVM 生命周期
主流虚拟机
Java 代码执行流程
类加载
	类加载器
	类加载过程
	双亲委派机制
垃圾回收
	垃圾回收器
	垃圾回收策略
	垃圾回收算法
	StopTheWorld
字节码
内存分配和回收
JVM 性能调优
	性能分析方法
	常用工具
	参数设置
