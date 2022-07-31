## ⭐ 动态代理是什么

基于反射机制

使用 jdk 的反射机制，创建对象的能力， 创建的是代理类的对象。 而不用你创建类文件。不用写 java 文件。

动态：在程序执行时，调用 jdk 提供的方法才能创建代理类的对象。

jdk 动态代理，必须有接口，目标类必须实现接口， 没有接口时，需要使用 cglib 动态代理

## ⭐ 动态代理能做什么

可以在不改变原来目标方法功能的前提下， 可以在代理中增强自己的功能代码。

减少代码的重复

专注业务逻辑代码

解耦合，让你的业务功能和日志，事务非业务功能分离。

## 使用代理模式的作用

1.功能增强： 在你原有的功能上，增加了额外的功能。

2.控制访问： 代理类不让你访问目标，例如商家不让用户访问厂家。

## 代理实现方式

### 静态代理

1）代理类是自己手工实现的，自己创建一个 java 类，表示代理类。

2）同时你所要代理的目标类是确定的。

特点： 1）实现简单  2）容易理解。

当项目中目标类和代理类很多时候，有以下的缺点：

1）当目标类增加了， 代理类可能也需要成倍的增加。 代理类数量过多。

2）当你的接口中功能增加了， 或者修改了，会影响众多的实现类，厂家类，代理都需要修改。影响比较多。

### 动态代理

在静态代理中目标类很多时候，可以使用动态代理，避免静态代理的缺点。

动态代理中目标类即使很多， 1）代理类数量可以很少， 2）当你修改了接口中的方法时，不会影响代理类。

动态代理： 在程序执行过程中，使用 jdk 的反射机制，创建代理类对象， 并动态的指定要代理目标类。

（动态代理是一种创建 java 对象的能力，让你不用创建 TaoBao 类，就能创建代理类对象）

动态代理的两种实现

- jdk 动态代理（理解）： 使用 java 反射包中的类和接口实现动态代理的功能（要求目标类有接口） 
   - 反射包 java.lang.reflect , 里面有三个类 ： InvocationHandler , Method, Proxy.
- cglib 动态代理（了解）：cglib 是第三方的工具库， 创建代理对象。 
   - cglib 通过继承目标类，创建它的子类，在子类中重写父类中同名的方法， 实现功能的修改
   - 因为 cglib 是继承，重写方法，所以要求目标类不能是 final 的， 方法也不能是 final 的。
   - cglib 的要求目标类比较宽松， 只要能继承就可以了
   - cglib 在很多的框架（如 mybatis ，spring）中使用，

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038763667-13f0e21f-5834-4851-ad0c-bc7cdf2e6c60.png#clientId=uf2be4cab-6726-4&from=paste&height=337&id=ud062d36b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=674&originWidth=777&originalType=binary&ratio=1&size=70114&status=done&style=none&taskId=udaea79d2-c9fb-42c2-81d7-2ca26637c3c&width=388.5)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038767030-5b2e3a48-01d3-4cd0-94c7-7c56e047d79e.png#clientId=uf2be4cab-6726-4&from=paste&height=433&id=ubf2d02a7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=866&originWidth=738&originalType=binary&ratio=1&size=92084&status=done&style=none&taskId=u83a68946-da20-43c4-a614-bb2d6bd8582&width=369)

jdk 动态代理： （未整理）

反射包 java.lang.reflect , 里面有三个类 ： InvocationHandler , Method, Proxy

invoke（）:表示代理对象要执行的功能代码。你的代理类要完成的功能就写在

invoke()方法中。

代理类完成的功能：

1. 调用目标方法，执行目标方法的功能
2. 功能增强，在目标方法调用时，增加功能。

方法原型：

参数： Object proxy:jdk 创建的代理对象，无需赋值。

```
Method method:目标类中的方法，jdk 提供 method 对象的

  Object[] args：目标类中方法的参数， jdk 提供的。

public Object invoke(Object proxy, Method method, Object[] args)

  InvocationHandler 接口：表示你的代理要干什么。
```

怎么用： 1.创建类实现接口 InvocationHandler

```
2.重写 invoke（）方法， 把原来静态代理中代理类要完成的功能，写在这。
```

1. Method 类：表示方法的， 确切的说就是目标类中的方法。

作用：通过 Method 可以执行某个目标类的方法，Method.invoke();

method.invoke(目标对象，方法的参数)

Object ret = method.invoke(service2, "李四");

说明： method.invoke（）就是用来执行目标方法的，等同于静态代理中的

//向厂家发送订单，告诉厂家，我买了 u 盘，厂家发货

float price = factory.sell(amount); //厂家的价格。

1. Proxy 类：核心的对象，创建代理对象。之前创建对象都是 new 类的构造方法()

现在我们是使用 Proxy 类的方法，代替 new 的使用。

方法： 静态方法 newProxyInstance()

作用是： 创建代理对象， 等同于静态代理中的 TaoBao taoBao = new TaoBao();

参数：

1. ClassLoader loader 类加载器，负责向内存中加载对象的。 使用反射获取对象的 ClassLoader

类 a , a.getCalss().getClassLoader(),  目标对象的类加载器

1. Class<?>[] interfaces： 接口， 目标对象实现的接口，也是反射获取的。
2. InvocationHandler h : 我们自己写的，代理类要完成的功能。

返回值：就是代理对象

```java
public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)
```

实现动态代理的步骤：

1. 创建接口，定义目标类要完成的功能
2. 创建目标类实现接口
3. 创建 InvocationHandler 接口的实现类，在 invoke 方法中完成代理类的功能
1.调用目标方法
2.增强功能
4. 使用 Proxy 类的静态方法，创建代理对象。 并把返回值转为接口类型。

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038773301-907a4967-a4ba-412a-af64-f150472cceeb.png#clientId=uf2be4cab-6726-4&from=paste&height=275&id=uc164f697&margin=%5Bobject%20Object%5D&name=image.png&originHeight=549&originWidth=1359&originalType=binary&ratio=1&size=59354&status=done&style=none&taskId=u08f6c4be-b4f9-48e2-a30b-7567b6fab5b&width=679.5)
