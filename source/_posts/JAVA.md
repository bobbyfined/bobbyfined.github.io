---
title: Java基础
date: 2022-06-01 21:14:36
tags: java
categories: java

---

# Java基础

<!-- more -->

## 面向对象三大特性

**封装，继承，多态**

- **封装**：将数据和基于数据的操作抽象化成一个对象并对其属性进行私有化，同时提供一些能被外界访问属性的方法；
- **继承**：子类扩展新的功能，并复用父类的属性和功能，单继承，多实现、
- **多态**：一个父类可以有多个子类（对同一方法进行多次重写），一个接口可以有多个实现，一个类可以实现多个接口（对接口进行不同的实现）

## java与C++区别（都是面向对象）

C++: 多继承，有指针概念可以手动管理内存

java：单继承但是有多实现，由JVM自动管理内存

## 多态实现原理

动态绑定，即在运行时才把方法调用与方法实现关联起来。

**静态绑定**：编译时就绑定，比如重载。
**动态绑定**：运行时绑定，比如重写，实现。

## static和final关键字

- **static**：修饰属性，方法（只会在堆中创建一份共享，随着类的加载而加载）
- **final**：修饰变量（修饰基础类型就不可被修改，修饰引用类型就不可被指向另一个对象），方法（锁定方法防止被子类重写，private方法隐式设置了final），类（不能被继承且所有方法被指定为final）

## 抽象类和接口

**抽象类**：abstract修饰的类，即包含抽象方法的类。（有抽象方法的类一定是抽象类，但是抽象类不是一定要有抽象方法）；只能被继承（单继承）所以不能被final修饰；不能被实例化（因为可能有没提供完整的实现的方法）

**接口**：interface修饰，属于抽象类型的类。。可以被多实现。不可以被实例化（有没提供完整的实现的方法）

### 区别

#### **相同点：**

1. 都不能被实例化
2. 都可以定义抽象方法且子类必须重写

#### **不同点：**

1. 抽象类可以有普通方法，接口只能有抽象方法（默认是public abstract修饰，在java8之后，能存在被default和static修饰的存在方法体的方法）
2. 抽象类有构造方法（可以初始化对象状态），接口没有
3. 抽象类只能被单继承，接口可以被多实现
4. 抽象类可以有不同修饰的成员变量，接口默认都是public static final 修饰

### **适合场景**

**抽象类**：

	1. 需要有基础功能并且基础功能会经常改变，那就使用抽象类，改变抽象类的基础方法就能让所有子类同时改变，达到解耦的目的。（如果是接口就需要改变每一个实现类中的方法）
	1. 拥有一些方法（但不再乎其如何实现）并且想让它们中的一些有默认实现，还想拥有实例变量，需要构造方法。

**接口**：

1. 需要多实现的场景（更多需要从业务出发，每个接口涉及不同业务，但是实现类需要涉及两个业务）
2. 需要解耦更加彻底的场景。根据不同条件获取不同实现的场景，用接口可以实现解耦（调用类只需要注入接口不要关心具体要用那个实现类）

## 泛型与泛型擦除

泛型： 参数化类型，将所需要的类型参数化，用<T>来声明一个类型参数。可以用于类、接口、方法的创建

泛型擦除：java泛型是伪泛型，虽然使用泛型的时候加上类型参数（比如ArrayList<Integer> list = new ArrayList<Integer>();）但是编译生成字节码的时候会类型擦除（实际上变成了List）。而由泛型附加的类型信息对 JVM 来说是不可见的。因此说是伪泛型。可以通过反射添加其它类型元素。

```
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(1);  //这样调用 add 方法只能存储整形，因为泛型类型的实例为 Integer

//使用反射想Integer类型中加入String的记录
list.getClass().getMethod("add", Object.class).invoke(list, "asd");

```

## 反射

**概念**：在运行状态中，对于任意一个类都能够知道这个类所有的属性和方法；并且都能够调用它的任意一个方法；

**如何得到Class的实例:**

```
	1.类名.class(就是一份字节码)
	2.Class.forName(String className);根据一个类的全限定名来构建Class对象
	3.每一个对象多有getClass()方法:obj.getClass();返回对象的真实类型
```

**适合场景：**

1. **自定义注解**：对被注解对象的操作需要用反射来执行
2. **动态代理**：AOP中拦截方法使用动态代理就需要反射
3. **开发通用框架**

## 异常体系

**Throwable**是超类，往下分为**Error**和**Exception**
一般来讲，程序无法捕获的异常就是Error，如JVM内部错误
Exception是由程序产生的，分为运行时异常（空指针，数组下标越界等）和编译时异常（语法错误等）

![image-20240510220456126](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20240510220456126.png)

## 数据结构

### ArrayList和LinkedList

​	**ArrayList：**

​	底层为数组（一段连续的内存），支持对元素的快速访问，适合随机访问，不合适插入和删除（移动元素代价高）。默认初始大小为10（初始化时容量为0，当有第一个数据进来才会初始化空间为10），扩容机制是扩大到当前的1.5倍，然后移动到新数组，移动方法：Array.copyof()。

​	**LinkedList：**

​	底层为链表（不需要连续的内存），适合数据插入和删除。可以当作堆栈，队列使用（出栈入栈对应插入删除）

**均为线程不安全**

#### 实现线程安全：

1. 使用原生的Vector，但是效率很低。底层通过synchronizedList
2. 使用**CopyOnWriteArrayList**，**写时加锁**，使用了一种叫写时复制的方法；读操作是可以不用加锁的，推荐使用

#### **fail-fast** 

快速失败机制：

- 在迭代器遍历元素的过程中，如果集合的结构（modCount）被改变的话，就会抛出异常ConcurrentModificationException，防止继续遍历。这就是所谓的快速失败机制。

  ```
  for(int i = 10; i < 100; i++){
          map.put(i, i);
      }
      List<Integer> list = new ArrayList<>();
      for(int i = 0; i < 20; i++){
          list.add(i);
      }
      Iterator<Integer> it = list.iterator();
      int temp = 0;
      while(it.hasNext()){
          if(temp == 3){
              temp++;
              list.remove(3);
          }else{
              temp++;
              System.out.println(it.next());
          }
      }
  }
  ```

  上诉代码会抛出异常，修改成以下代码就不会,不能直接使用集合的 `remove` 方法来删除元素，而应该使用 `Iterator` 的 `remove` 方法，这样可以避免 `ConcurrentModificationException`。

  ```
  // 使用 Iterator 遍历并删除元素
          Iterator<Integer> it = list.iterator();
          int temp = 0;
          while (it.hasNext()) {
              if (temp == 3) {
                  temp++;
                  it.next(); // 移动到下一个元素，因为 remove 需要在 next() 之后调用
                  it.remove(); // 使用 Iterator 的 remove 方法
              } else {
                  temp++;
                  System.out.println(it.next());
              }
          }
  ```

  

#### **fail-safe**

安全失败机制：

- 采用安全失败机制的集合容器，在遍历时不是直接在集合内容上访问的，而是先复制原有集合内容，在拷贝的集合上进行遍历。由于在遍历过程中对原集合所作的修改并不能被迭代器检测到，所以不会触发ConcurrentModificationException。
-  **缺点**：基于拷贝内容的优点是避免了ConcurrentModificationException，但同样地，迭代器并不能访问到**修改后**的内容，即：迭代器遍历的是开始遍历那一刻拿到的集合拷贝，在遍历期间原集合发生的修改迭代器是不知道的。
- 适用场景：java.util.concurrent包下的容器都是安全失败，可以在多线程下并发使用，并发修改。

### HashMap详解（JDK 1.8）

https://juejin.cn/post/6844904111817637901

#### 数据结构：

​	底层数据结构采用数组+链表+红黑树。通过散列映射来存储键值对数据。

#### 链地址法

​	HashMap 是使用哈希表来存储数据的。哈希表为了解决冲突，一般有两种方案：**开放地址法** 和 **链地址法**。HashMap 采用的便是 **链地址法**，即在数组的每个索引处都是一个链表结构，这样就可以有效解决 hash 冲突。

#### 默认参数

```java
// 默认的初始容量为 16 （PS：aka 应该是 as know as）
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16

// 最大容量（容量不够时需要扩容）
static final int MAXIMUM_CAPACITY = 1 << 30; //2的30次方

// 默认的负载因子
static final float DEFAULT_LOAD_FACTOR = 0.75f;

// 链表长度为 8 的时候会转为红黑树
static final int TREEIFY_THRESHOLD = 8;

// 长度为 6 的时候会从红黑树转为链表
static final int UNTREEIFY_THRESHOLD = 6;

// 只有桶内数据量大于 64 的时候才会允许转红黑树
static final int MIN_TREEIFY_CAPACITY = 64;

```

初始容量是 16，可以扩容，但是扩容之后的容量，也是 2 的幂次方也就是一倍。另外， **MIN_TREEIFY_CAPACITY**，虽然说当链表长度大于 8 的时候，链表会转为红黑树，但是也是需要满足桶内存储的数据量大于上述这个参数的值，否则不仅不会转红黑树，反而会进行扩容操作。

![image-20240512232213041](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20240512232213041.png)



#### 重要参数

```java
// Map 中存储的数据量，即 key-value 键值对的数量
transient int size;

// HashMap 内部结构发生变化的次数，即新增、删除数据的时候都会记录，
// 注意：修改某个 key 的值，并不会改变这个 modCount
transient int modCount;

// 重点，代表最多能容纳的数据量（初始为16 * 0.75 = 12）
// 即最多能容纳的 key-value 键值对的数量
int threshold;

// 负载因子，默认为 0.75
// 注意，这个值是可以大于 1 的
final float loadFactor;

```

**threshold** 代表最多能容纳的 Node 数量，一般 `threshold = 数组长度（初始为16） * loadFactor`，也就是说要想 HashMap 能够存储更多的数据（即获得较大的 threshold），有两种方案，一种是扩容（即增大数组长度 ），另一种便是增大负载因子。

#### 自动扩容原理

​	**初始化为空数组，第-次put 时才实例。**当数据容量size达到threshold 阈值时会触发扩容机制。调用resize()，将**数组长度**扩大到原来的2倍。



##### threshold阈值怎么计算

​	threshold = 数组长度（初始为16） * loadFactor（负载因子）



##### 数组怎么扩容

 	1. 获取旧数组，旧数组长度，旧数组阈值
 	2. 如果旧数组长度 > 0
 	 	1.  旧数组长度 >=最大值，就将阈值调为Integer.MAX_VALUE（2的31次方-1），数组长度不变；
 	 	2. 数组长度变为原来的2倍，阈值 = 新数组长度 * 负载因子0.75
 	3. 如果旧数组长度 = 0，但是旧阈值 > 0，正常是带参初始化hashmap，将旧阈值作为数组长度
 	4. 如果旧数组长度 = 0 ，旧阈值 = 0，就是无参初始化hashmap，将默认初始容量 `DEFAULT_INITIAL_CAPACITY（16）`和默认负载因子 `DEFAULT_LOAD_FACTOR（0.75）`计算出新数组长度 newCap 和新阈值 newThr。
 	5. 将旧数组元素复制到新数组，一部分下标索引不变，一部分变为（原索引+旧数据长度）（索引不是指链表位置）

#### 添加元素时怎么确定存放的底层数组（桶）的索引下标？

​	通过这个与运算 `(n - 1) & hash`，其中变量 n 为数组的长度，变量 hash 就是通过 `hash()` 方法计算后的结果

#### 简单介绍一下 `hash()`原理？

​	HashMap的`hash()`就是将key对象的hashCode值进行处理（降低hash冲突的可能），得到最终的哈希值（hash）



#### JDK1.7与JDK1.8中HashMap的区别

1. 旧数组长度jdk1.8计算索引方式不同，扩容时计算新索引下标只需要（hash & 旧数组长度）即可，结果为0则新索引=原索引n，结果为旧数组长度则新索引=原索引n + 旧数组长度。jdk1.7扩容时需要一直（n-1）&hash
2. 1.8的链表引入了红黑树结构，当链表长度大于8且桶数组数据量大于64就变成红黑树，小于6则从红黑树退化为链表，。1.7则是数组+链表。
3. 1.8扩容采用尾插法，1.7用头插法。尾插法能保证节点顺序和之前保持一致。



#### 为什么1.8改用红黑树

​	当hash冲突过多时，链表过长，此时查询效率低下，大于8改为红黑树后查询方式性能得到了很好的提升，从原来的是O(n)到O(logn)。

#### Hashmap 链表转红黑树条件

	1. 链表长度大于8
	1. 数组长度大于64

####  HashMap允许空键空值么

​	HashMap最多只允许一个键为Null(多条会覆盖)，但允许多个值为Null。hash()当key为null为返回0

```
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```



#### 线程安全问题

1. 两个线程同时计算索引时，可能会得出同一个索引位置，当A获取链表头节点后时间片用完，B获取链表头节点插入，此时A再插入数据就会造成B的数组被覆盖的问题。
2. （jdk1.7时采用、头插法）两个线程同时触发resize()，同时修改链表结构会产生一个循环链表。此时get会死循环



#### ConcurrentHashMap

​	解决线程安全问题可以通过**ConcurrentHashMap** 和 **Hashtable**来实现线程安全；

​	HashTable是原始API类，通过synchronize同步修饰，效率低下；

​	ConcurrentHashMap通过分段锁实现，效率比HashTable要好；



##### 数据结构： 

​	和HashMap一样采用数组 + 链表 + 红黑树实现。扩容机制也一样



##### 与1.7区别

​	jdk1.7的concurrentHashMap使用数组 + segment（段）+分段锁实现，其内部分为一个个段（Segment）数组，Segment 通过继承 ReentrantLock（可重入锁） 来进行加锁。每次锁一个段降低锁的粒度保证线程在段内操作的安全性。但是这样每次确定索引就需要两次定位：

1. hash值 & （段数组长度 - 1），确定所属段
2. hash值 & （内部数组长度 - 1），确定所在桶

​	因此jdk1.8中优化了结构，取消分段锁，使用cas操作（compare and swap）和synchronied关键字实现优化。粒度直接到桶数组元素级别，锁住链表。



##### 不允许key和value为null

​	容易引起歧义，因为无法确认本身就是null还是被另一个线程修改的key-value



##### 如何保证线程的安全性？

​	采用大量的分而治之的思想来降低锁的粒度，提升并发性能。使用大量的cas操作保证安全性，而不是和 HashTable 一样，不论什么方法，直接简单粗暴的使用 synchronized关键字来实现。

​	cas：比较交换，通过拿一个旧值（期望值）和旧地址存的值作比较，如果相等就用新值设置并返回true，否则返回false证明已经被另一个线程修改了

##### 多并发下怎么实现扩容

​	采用分而治之的思想，分段进行扩容，即每个线程负责一段，默认最小是 16。也就是说如果 ConcurrentHashMap 中只有 16 个槽位，那么就只会有一个线程参与扩容。如果大于 16 则根据当前 CPU 数来进行分配，最大参与扩容线程数不会超过 CPU 数。
扩容后迁移数据，和hashmap类似，但是会用synchronized对当前节点加锁



### **序列化和反序列化**

​	**序列化：**将java对象转化为字节序列的过程。持久化，用于存储和传输

​	**反序列化：**将字节序列转化为java对象的过程。

##### **优点：**

 a、实现了数据的持久化，通过序列化可以把数据永久地保存到硬盘上（通常存放在文件里）Redis的RDB

 b、利用序列化实现远程通信，即在网络上传送对象的字节序列。 Google的protoBuf

##### **反序列化失败的场景：**

 序列化ID：serialVersionUID不一致的时候，导致反序列化失败（serialVersionUID是JRE根据类的内部细节自动生成，当修改对象属性或方法，serialVersionUID也会变化）

### **String**

String 使用**数组**存储内容，数组使用 **final** 修饰，因此 String 定义的字符串的值也是**不可变的**，线程安全

| 类型          | 操作效率 | 线程安全             |
| :------------ | :------- | :------------------- |
| String        | 低       | 安全（final）        |
| StringBuffer  | 中       | 安全（synchronized） |
| StringBuilder | 高       | 非安全               |



# 设计模式

## 单例模式

1. 饿汉式（立即加载）

   饿汉式单例模式在类加载时就创建实例，线程安全，但如果实例初始化过程复杂且不一定会用到，可能会浪费资源。

   ```java
   public class SingletonEager {
       // 私有构造函数，防止外部实例化
       private SingletonEager() {}
   
       // 饿汉式在类加载时创建实例
       private static final SingletonEager instance = new SingletonEager();
   
       // 提供公共静态方法获取实例
       public static SingletonEager getInstance() {
           return instance;
       }
   }
   
   ```

   

2. 懒汉式 （延迟加载）

   线程不安全懒汉式

   ```java
   public class SingletonLazy {
       // 私有构造函数，防止外部实例化
       private SingletonLazy() {}
   
       // 懒汉式在需要时创建实例
       private static SingletonLazy instance;
   
       // 提供公共静态方法获取实例
       public static SingletonLazy getInstance() {
           if (instance == null) {
               instance = new SingletonLazy();
           }
           return instance;
       }
   }
   
   ```

   线程安全的懒汉式

   ```java
   class Singleton{
       private volatile static Singleton instance = null;   //禁止指令重排
       private Singleton() {
            
       }
       public static Singleton getInstance() {
           if(instance==null) { //减少加锁的损耗
               synchronized (Singleton.class) {
                   if(instance==null) //确认是否初始化完成
                       instance = new Singleton();
               }
           }
           return instance;
       }
   }
   ```

   使用双重**双重检查锁定**:

   - **第一次检查**：在同步块外检查 `instance` 是否为 `null`，目的是减少不必要的同步，提升性能。
   - **同步块**：如果第一次检查发现 `instance` 为 `null`，进入同步块，确保只有一个线程能够执行此块代码。
   - **第二次检查**：在同步块内再次检查 `instance` 是否为 `null`，因为可能有多个线程在第一次检查时都发现 `instance` 为 `null`，如果没有第二次检查，那么多个线程可能会创建多个实例。
   - **实例化**：只有在确认 `instance` 为 `null` 的情况下，才会创建新的实例。

   

   **优化，使用静态内部类实现懒汉式**

   ```java
   public class SingletonLazy {
   
       // 私有构造函数，防止外部实例化
       private SingletonLazy() {}
   
       // 懒汉式在需要时创建实例
       private static class SingletonLazyHelper {
           private static final SingletonLazy INSTANCE = new SingletonLazy();
       }
   
       // 提供公共静态方法获取实例
       public static SingletonLazy getInstance() {
           return SingletonLazyHelper.INSTANCE;
       }
   
   }
   ```

   ### 解释

   1. **私有构造函数**：`private Singleton()` 确保外部无法实例化该类。
   2. **静态内部类**：`SingletonHolder` 是一个私有的静态内部类，它只在 `Singleton.getInstance()` 被调用时才会被加载和初始化。
   3. **静态初始化器**：`private static final Singleton INSTANCE = new Singleton();` 由 JVM 保证在类加载时线程安全。
   4. **公共静态方法**：`public static Singleton getInstance()` 通过调用 `SingletonHolder.INSTANCE` 返回单例实例。

   这种方式利用了 JVM 类加载机制的线程安全特性，不需要显式的同步机制，同时实现了懒加载，确保了单例实例只有在第一次使用时才会被创建。

   ### 优点

   - **延迟加载**：单例实例在第一次使用时才被创建。
   - **线程安全**：静态内部类的加载和初始化是由 JVM 保证的，天然是线程安全的。
   - **实现简单**：不需要显式的同步代码，代码简洁明了。





# Spring篇



### 设计思想&Beans



#### **1、IOC 控制反转**



 IoC（Inverse of Control:控制反转）是⼀种设计思想，就是将原本在程序中⼿动创建对象的控制权，交由Spring框架来管理。 IoC 在其他语⾔中也有应⽤，并⾮ Spring 特有。

 IoC 容器是 Spring⽤来实现 IoC 的载体， IoC 容器实际上就是个Map（key，value）,Map 中存放的是各种对象。将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注⼊。这样可以很⼤程度上简化应⽤的开发，把应⽤从复杂的依赖关系中解放出来。 IoC 容器就像是⼀个⼯⼚⼀样，当我们需要创建⼀个对象的时候，只需要配置好配置⽂件/注解即可，完全不⽤考虑对象是如何被创建出来的。

**DI 依赖注入**

 DI:（Dependancy Injection：依赖注入)站在容器的角度，将对象创建依赖的其他对象注入到对象中。

#### **2、AOP 动态代理**



 AOP(Aspect-Oriented Programming:⾯向切⾯编程)能够将那些与业务⽆关，却为业务模块所共同调⽤的逻辑或责任（例如事务处理、⽇志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

 Spring AOP就是基于动态代理的，如果要代理的对象，实现了某个接⼝，那么Spring AOP会使⽤JDKProxy，去创建代理对象，**⽽对于没有实现接⼝的对象，就⽆法使⽤ JDK Proxy 去进⾏代理了**，这时候Spring AOP会使⽤基于asm框架字节流的Cglib动态代理 ，这时候Spring AOP会使⽤ Cglib ⽣成⼀个被代理对象的⼦类来作为代理。

#### **3、Bean生命周期**



**单例对象：** singleton

总结：单例对象的生命周期和容器相同

**多例对象：** prototype

出生：使用对象时spring框架为我们创建

活着：对象只要是在使用过程中就一直活着

死亡：当对象长时间不用且没有其它对象引用时，由java的垃圾回收机制回收

[![img](https://camo.githubusercontent.com/ff500845fb259b45f67c7d6fe787850907bedb69b49cb8cf236880c59ce77d83/68747470733a2f2f73302e6c677374617469632e636f6d2f692f696d616765332f4d30312f38392f30432f43677132786c365776487141646d743441414247416e32655369493633312e706e67)](https://camo.githubusercontent.com/ff500845fb259b45f67c7d6fe787850907bedb69b49cb8cf236880c59ce77d83/68747470733a2f2f73302e6c677374617469632e636f6d2f692f696d616765332f4d30312f38392f30432f43677132786c365776487141646d743441414247416e32655369493633312e706e67)

IOC容器初始化加载Bean流程：

```
@Override
public void refresh() throws BeansException, IllegalStateException { synchronized (this.startupShutdownMonitor) {
  // 第一步:刷新前的预处理 
  prepareRefresh();
  //第二步: 获取BeanFactory并注册到 BeanDefitionRegistry
  ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();
  // 第三步:加载BeanFactory的预准备工作(BeanFactory进行一些设置，比如context的类加载器等)
  prepareBeanFactory(beanFactory);
  try {
    // 第四步:完成BeanFactory准备工作后的前置处理工作 
    postProcessBeanFactory(beanFactory);
    // 第五步:实例化BeanFactoryPostProcessor接口的Bean 
    invokeBeanFactoryPostProcessors(beanFactory);
    // 第六步:注册BeanPostProcessor后置处理器，在创建bean的后执行 
    registerBeanPostProcessors(beanFactory);
    // 第七步:初始化MessageSource组件(做国际化功能;消息绑定，消息解析); 
    initMessageSource();
    // 第八步:注册初始化事件派发器 
    initApplicationEventMulticaster();
    // 第九步:子类重写这个方法，在容器刷新的时候可以自定义逻辑 
    onRefresh();
    // 第十步:注册应用的监听器。就是注册实现了ApplicationListener接口的监听器
    registerListeners();
    //第十一步:初始化所有剩下的非懒加载的单例bean 初始化创建非懒加载方式的单例Bean实例(未设置属性)
    finishBeanFactoryInitialization(beanFactory);
    //第十二步: 完成context的刷新。主要是调用LifecycleProcessor的onRefresh()方法，完成创建
    finishRefresh();
	}
  ……
} 
```



总结：

**四个阶段**

- 实例化 Instantiation
- 属性赋值 Populate
- 初始化 Initialization
- 销毁 Destruction

**多个扩展点**

- 影响多个Bean
  - BeanPostProcessor
  - InstantiationAwareBeanPostProcessor
- 影响单个Bean
  - Aware

**完整流程**

1. 实例化一个Bean－－也就是我们常说的**new**；
2. 按照Spring上下文对实例化的Bean进行配置－－**也就是IOC注入**；
3. 如果这个Bean已经实现了BeanNameAware接口，会调用它实现的setBeanName(String)方法，也就是根据就是Spring配置文件中**Bean的id和name进行传递**
4. 如果这个Bean已经实现了BeanFactoryAware接口，会调用它实现setBeanFactory(BeanFactory)也就是Spring配置文件配置的**Spring工厂自身进行传递**；
5. 如果这个Bean已经实现了ApplicationContextAware接口，会调用setApplicationContext(ApplicationContext)方法，和4传递的信息一样但是因为ApplicationContext是BeanFactory的子接口，所以**更加灵活**
6. 如果这个Bean关联了BeanPostProcessor接口，将会调用postProcessBeforeInitialization()方法，BeanPostProcessor经常被用作是Bean内容的更改，由于这个是在Bean初始化结束时调用那个的方法，也可以被应用于**内存或缓存技**术
7. 如果Bean在Spring配置文件中配置了init-method属性会自动调用其配置的初始化方法。
8. 如果这个Bean关联了BeanPostProcessor接口，将会调用postProcessAfterInitialization()，**打印日志或者三级缓存技术里面的bean升级**
9. 以上工作完成以后就可以应用这个Bean了，那这个Bean是一个Singleton的，所以一般情况下我们调用同一个id的Bean会是在内容地址相同的实例，当然在Spring配置文件中也可以配置非Singleton，这里我们不做赘述。
10. 当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean这个接口，或者根据spring配置的destroy-method属性，调用实现的destroy()方法

#### **4**、Bean作用域



| 名称           | 作用域                                                       |
| -------------- | ------------------------------------------------------------ |
| **singleton**  | **单例对象，默认值的作用域**                                 |
| **prototype**  | **每次获取都会创建⼀个新的 bean 实例**                       |
| request        | 每⼀次HTTP请求都会产⽣⼀个新的bean，该bean仅在当前HTTP request内有效。 |
| session        | 在一次 HTTP session 中，容器将返回同一个实例                 |
| global-session | 将对象存入到web项目集群的session域中,若不存在集群,则global session相当于session |

默认作用域是singleton，多个线程访问同一个bean时会存在线程不安全问题

**保障线程安全方法：**

1. 在Bean对象中尽量避免定义可变的成员变量（不太现实）。
2. 在类中定义⼀个ThreadLocal成员变量，将需要的可变成员变量保存在 ThreadLocal 中

**ThreadLocal**：

 每个线程中都有一个自己的ThreadLocalMap类对象，可以将线程自己的对象保持到其中，各管各的，线程可以正确的访问到自己的对象。

 将一个共用的ThreadLocal静态实例作为key，将不同对象的引用保存到不同线程的ThreadLocalMap中，然后**在线程执行的各处通过这个静态ThreadLocal实例的get()方法取得自己线程保存的那个对象**，避免了将这个对象作为参数传递的麻烦。

#### 5、循环依赖



 循环依赖其实就是循环引用，也就是两个或者两个以上的 Bean 互相持有对方，最终形成闭环。比如A 依赖于B，B又依赖于A

Spring中循环依赖场景有:

- prototype 原型 bean循环依赖

- 构造器的循环依赖（构造器注入）

- Field 属性的循环依赖（set注入）

  其中，构造器的循环依赖问题无法解决，在解决属性循环依赖时，可以使用懒加载，spring采用的是提前暴露对象的方法。

**懒加载@Lazy解决循环依赖问题**

 Spring 启动的时候会把所有bean信息(包括XML和注解)解析转化成Spring能够识别的BeanDefinition并存到Hashmap里供下面的初始化时用，然后对每个 BeanDefinition 进行处理。普通 Bean 的初始化是在容器启动初始化阶段执行的，而被lazy-init=true修饰的 bean 则是在从容器里第一次进行**context.getBean() 时进行触发**。



**三级缓存解决循环依赖问题**

[![循环依赖问题](https://camo.githubusercontent.com/de2991ecc66213f6d747a23957a07acf037eb3f0f4f0398e1cd1445a6e479fcf/68747470733a2f2f747661312e73696e61696d672e636e2f6c617267652f303038314b636b776c7931676c763769767275326c6a333139383071636e31332e6a7067)](https://camo.githubusercontent.com/de2991ecc66213f6d747a23957a07acf037eb3f0f4f0398e1cd1445a6e479fcf/68747470733a2f2f747661312e73696e61696d672e636e2f6c617267652f303038314b636b776c7931676c763769767275326c6a333139383071636e31332e6a7067)

1. Spring容器初始化ClassA通过构造器初始化对象后提前暴露到Spring容器中的singletonFactorys（三级缓存中）。
2. ClassA调用setClassB方法，Spring首先尝试从容器中获取ClassB，此时ClassB不存在Spring 容器中。
3. Spring容器初始化ClassB，ClasssB首先将自己暴露在三级缓存中，然后从Spring容器一级、二级、三级缓存中一次中获取ClassA 。
4. 获取到ClassA后将自己实例化放入单例池中，实例 ClassA通过Spring容器获取到ClassB，完成了自己对象初始化操作。
5. 这样ClassA和ClassB都完成了对象初始化操作，从而解决了循环依赖问题。

##### Spring 框架通过 "三级缓存" 机制来解决单例 Bean 的循环依赖问题。三级缓存主要涉及以下三个缓存：

1. **一级缓存**（singletonObjects）：存储已经完全初始化的单例 Bean。
2. **二级缓存**（earlySingletonObjects）：存储提前暴露的早期单例 Bean，未完成依赖注入但已经实例化。
3. **三级缓存**（singletonFactories）：存储能够创建 Bean 的工厂对象，用于创建 Bean 的代理对象或提前曝光 Bean 的实例。

三级缓存解决循环依赖问题的工作机制

1. **Bean 实例化**：
   - Spring 在创建 Bean 的过程中，首先会实例化该 Bean（即调用构造函数创建 Bean 对象，但未进行依赖注入）。
2. **将 Bean 的工厂对象加入三级缓存**：
   - Spring 会将创建 Bean 的工厂对象放入三级缓存（singletonFactories）。
3. **依赖注入**：
   - 当需要注入依赖时，Spring 会从缓存中获取依赖的 Bean。如果依赖的 Bean 已经在一级缓存中，则直接使用；如果在二级缓存中，也直接使用；如果在三级缓存中，Spring 会通过工厂对象获取 Bean 的早期引用，并将其移动到二级缓存中以供其他 Bean 使用。
4. **完成依赖注入和初始化**：
   - 一旦依赖注入完成，Spring 会将完全初始化的 Bean 移动到一级缓存中，同时从二级缓存和三级缓存中移除。

代码示例

为了更好地理解三级缓存的工作机制，以下是一个简化的代码示例：

```
java复制代码import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component
@Scope("singleton")
public class A {
    private final B b;

    @Autowired
    public A(B b) {
        this.b = b;
    }
}

@Component
@Scope("singleton")
public class B {
    private final A a;

    @Autowired
    public B(A a) {
        this.a = a;
    }
}
```

在这个例子中，类 `A` 依赖于类 `B`，而类 `B` 也依赖于类 `A`，形成了循环依赖。

三级缓存的详细工作过程

1. **实例化 Bean A**：
   - Spring 创建 Bean A 的实例，并将 Bean A 的工厂对象放入三级缓存。
2. **实例化 Bean B**：
   - 在创建 Bean B 的过程中，发现需要注入 Bean A。
   - Spring 从三级缓存中获取 Bean A 的工厂对象，通过工厂对象获取 Bean A 的早期引用（即尚未完成依赖注入的实例），并将其放入二级缓存。
3. **完成 Bean B 的实例化和依赖注入**：
   - Spring 完成 Bean B 的实例化和依赖注入，将完全初始化的 Bean B 放入一级缓存。
4. **完成 Bean A 的依赖注入**：
   - Spring 使用从二级缓存中获取的 Bean B 完成 Bean A 的依赖注入，并将完全初始化的 Bean A 移动到一级缓存。

### 总结

通过三级缓存机制，Spring 能够在 Bean 的创建过程中提前暴露一个创建中的 Bean，从而解决单例 Bean 的循环依赖问题。这种机制确保了依赖注入的顺利进行，同时避免了死循环或堆栈溢出错误。然而，这种机制仅适用于单例作用域的 Bean，对于原型作用域的 Bean，Spring 无法使用这种机制来解决循环依赖问题。

### Spring注解



#### 1、@SpringBoot



 **声明bean的注解**

 **@Component** 通⽤的注解，可标注任意类为 Spring 组件

 **@Service** 在业务逻辑层使用（service层）

 **@Repository** 在数据访问层使用（dao层）

 **@Controller** 在展现层使用，控制器的声明（controller层）

 **注入bean的注解**

 **@Autowired**：默认按照类型来装配注入，**@Qualifier**：可以改成名称

 **@Resource**：默认按照名称来装配注入，JDK的注解，新版本已经弃用

**@Autowired注解原理**

 @Autowired的使用简化了我们的开发，

 实现 AutowiredAnnotationBeanPostProcessor 类，该类实现了 Spring 框架的一些扩展接口。 实现 BeanFactoryAware 接口使其内部持有了 BeanFactory（可轻松的获取需要依赖的的 Bean）。 实现 MergedBeanDefinitionPostProcessor 接口，实例化Bean 前获取到 里面的 @Autowired 信息并缓存下来； 实现 postProcessPropertyValues 接口， 实例化Bean 后从缓存取出注解信息，通过反射将依赖对象设置到 Bean 属性里面。

**@SpringBootApplication**

```
@SpringBootApplication
public class JpaApplication {
    public static void main(String[] args) {
        SpringApplication.run(JpaApplication.class, args);
    }
}
```



**@SpringBootApplication**注解等同于下面三个注解：

- **@SpringBootConfiguration：** 底层是**Configuration**注解，说白了就是支持**JavaConfig**的方式来进行配置
- **@EnableAutoConfiguration：开启自动配置**功能
- **@ComponentScan：就是扫描**注解，默认是扫描**当前类下**的package

**其中`@EnableAutoConfiguration`是关键(启用自动配置)，内部实际上就去加载`META-INF/spring.factories`文件的信息，然后筛选出以`EnableAutoConfiguration`为key的数据，加载到IOC容器中，实现自动配置功能！**

它主要加载了@SpringBootApplication注解主配置类，这个@SpringBootApplication注解主配置类里边最主要的功能就是SpringBoot开启了一个@EnableAutoConfiguration注解的自动配置功能。

**@EnableAutoConfiguration作用：**

它主要利用了一个

EnableAutoConfigurationImportSelector选择器给Spring容器中来导入一些组件。

```
@Import(EnableAutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration 
```



#### **2、@SpringMVC**



```
@Controller 声明该类为SpringMVC中的Controller
@RequestMapping 用于映射Web请求
@ResponseBody 支持将返回值放在response内，而不是一个页面，通常用户返回json数据
@RequestBody 允许request的参数在request体中，而不是在直接连接在地址后面。
@PathVariable 用于接收路径参数
@RequestMapping("/hello/{name}")申明的路径，将注解放在参数中前，即可获取该值，通常作为Restful的接口实现方法。
```



**SpringMVC原理**

[![img](https://camo.githubusercontent.com/d9b573af7586c35692c03cf192231b9518ddd4a92bc805da05032edab42d4916/68747470733a2f2f696d672d626c6f672e6373646e2e6e65742f32303138313032323232343035383631373f77617465726d61726b2f322f746578742f6148523063484d364c7939696247396e4c6d4e7a5a473475626d56304c3246335957746c5832787861413d3d2f666f6e742f3561364c354c32542f666f6e7473697a652f3430302f66696c6c2f49304a42516b46434d413d3d2f646973736f6c76652f3730)](https://camo.githubusercontent.com/d9b573af7586c35692c03cf192231b9518ddd4a92bc805da05032edab42d4916/68747470733a2f2f696d672d626c6f672e6373646e2e6e65742f32303138313032323232343035383631373f77617465726d61726b2f322f746578742f6148523063484d364c7939696247396e4c6d4e7a5a473475626d56304c3246335957746c5832787861413d3d2f666f6e742f3561364c354c32542f666f6e7473697a652f3430302f66696c6c2f49304a42516b46434d413d3d2f646973736f6c76652f3730)

1. 客户端（浏览器）发送请求，直接请求到 DispatcherServlet 。
2. DispatcherServlet 根据请求信息调⽤ HandlerMapping ，解析请求对应的 Handler 。
3. 解析到对应的 Handler （也就是 Controller 控制器）后，开始由HandlerAdapter 适配器处理。
4. HandlerAdapter 会根据 Handler 来调⽤真正的处理器开处理请求，并处理相应的业务逻辑。
5. 处理器处理完业务后，会返回⼀个 ModelAndView 对象， Model 是返回的数据对象
6. ViewResolver 会根据逻辑 View 查找实际的 View 。
7. DispaterServlet 把返回的 Model 传给 View （视图渲染）。
8. 把 View 返回给请求者（浏览器）

#### 3、@SpringMybatis



```
@Insert ： 插入sql ,和xml insert sql语法完全一样
@Select ： 查询sql, 和xml select sql语法完全一样
@Update ： 更新sql, 和xml update sql语法完全一样
@Delete ： 删除sql, 和xml delete sql语法完全一样
@Param ： 入参
@Results ： 设置结果集合@Result ： 结果
@ResultMap ： 引用结果集合
@SelectKey ： 获取最新插入id 
```



**mybatis如何防止sql注入？**

 简单的说就是#{}是经过预编译的，是安全的，**$**{}是未经过预编译的，仅仅是取变量的值，是非安全的，存在SQL注入。在编写mybatis的映射语句时，尽量采用**“#{xxx}”**这样的格式。如果需要实现动态传入表名、列名，还需要做如下修改：添加属性**statementType="STATEMENT"**，同时sql里的属有变量取值都改成**${xxxx}**

**Mybatis和Hibernate的区别**

**Hibernate 框架：**

 **Hibernate**是一个开放源代码的对象关系映射框架,它对JDBC进行了非常轻量级的对象封装,建立对象与数据库表的映射。是一个全自动的、完全面向对象的持久层框架。

**Mybatis框架：**

 **Mybatis**是一个开源对象关系映射框架，原名：ibatis,2010年由谷歌接管以后更名。是一个半自动化的持久层框架。

**区别：**

**开发方面**

 在项目开发过程当中，就速度而言：

 hibernate开发中，sql语句已经被封装，直接可以使用，加快系统开发；

 Mybatis 属于半自动化，sql需要手工完成，稍微繁琐；

 但是，凡事都不是绝对的，如果对于庞大复杂的系统项目来说，复杂语句较多，hibernate 就不是好方案。

**sql优化方面**

 Hibernate 自动生成sql,有些语句较为繁琐，会多消耗一些性能；

 Mybatis 手动编写sql，可以避免不需要的查询，提高系统性能；

**对象管理比对**

 Hibernate 是完整的对象-关系映射的框架，开发工程中，无需过多关注底层实现，只要去管理对象即可；

 Mybatis 需要自行管理映射关系；

#### 4、@Transactional



```
@EnableTransactionManagement 
@Transactional
```



注意事项：

 ①事务函数中不要处理耗时任务，会导致长期占有数据库连接。

 ②事务函数中不要处理无关业务，防止产生异常导致事务回滚。

**事务传播属性**

**1) REQUIRED（默认属性）** 如果存在一个事务，则支持当前事务。如果没有事务则开启一个新的事务。

1. MANDATORY 支持当前事务，如果当前没有事务，就抛出异常。
2. NEVER 以非事务方式执行，如果当前存在事务，则抛出异常。
3. NOT_SUPPORTED 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
4. REQUIRES_NEW 新建事务，如果当前存在事务，把当前事务挂起。
5. SUPPORTS 支持当前事务，如果当前没有事务，就以非事务方式执行。

**7) NESTED** （**局部回滚**） 支持当前事务，新增Savepoint点，与当前事务同步提交或回滚。 **嵌套事务一个非常重要的概念就是内层事务依赖于外层事务。外层事务失败时，会回滚内层事务所做的动作。而内层事务操作失败并不会引起外层事务的回滚。**

### Spring源码阅读



#### **1、Spring中的设计模式**



参考：[spring中的设计模式](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247485303&idx=1&sn=9e4626a1e3f001f9b0d84a6fa0cff04a&chksm=cea248bcf9d5c1aaf48b67cc52bac74eb29d6037848d6cf213b0e5466f2d1fda970db700ba41&token=255050878&lang=zh_CN%23rd)

**单例设计模式 :** Spring 中的 Bean 默认都是单例的。

**⼯⼚设计模式 :** Spring使⽤⼯⼚模式通过 BeanFactory 、 ApplicationContext 创建bean 对象。

**代理设计模式 :** Spring AOP 功能的实现。

**观察者模式：** Spring 事件驱动模型就是观察者模式很经典的⼀个应⽤。

**适配器模式：**Spring AOP 的增强或通知(Advice)使⽤到了适配器模式、spring MVC 中也是⽤到了适配器模式适配 Controller 。

# Springboot

自动装配原理：

​	基于spring框架的IOC（控制反转）和DI（依赖注入）机制，通过自动配置机制来简化Spring应用的配置过程。主要是通过核心注解@SpringbootApplication，可以看作`@Configuration`、`@EnableAutoConfiguration`、`@ComponentScan` 注解的集合。其中最主要的是@EnableAutoConfiguration。@EnableAutoConfiguration实际是通过 `AutoConfigurationImportSelector`类（加载自动装配类），将符合条件的bean进行装配。

## 自动配置的执行过程

**启动阶段**：应用启动时，Spring Boot会扫描`META-INF/spring.factories`文件，找到所有自动配置类。

**加载阶段**：使用`SpringFactoriesLoader`加载这些配置类。

**条件判断**：对于每个自动配置类，Spring Boot会根据`@Conditional`注解的条件进行判断，如果满足条件，则装配相应的Bean。

**注入阶段**：根据DI机制，将满足条件的Bean注入到Spring上下文中。

