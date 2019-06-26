# 阿里Java开发手册代码规范补遗

## 一、命名风格

#### 1、【强制】代码中的命名不能以下划线或$符号开始/结束

禁止：<font color = red>\_name / $name / name\_ / name$ </font>

#### 2、【强制】抽象类命名使用Abstract/Base开头；异常类命名使用Exception结尾；测试命名以测试的类名开始，以Test结尾

例如：AbstractShape / BaseShape

​			ApplicationException / UploadTest

#### 3、【强制】类型和中括号紧挨来表示数组。

例如：int[] arrayDemo

禁止：<font color = red>main函数参数中，使用String args[]</font>

#### 4、【强制】 POJO类中布尔类型的变量都不要加is前缀，因为部分框架解析会引起序列化错误

禁止：定义为基本数据类型 Boolean isDeleted 的属性，它的方法也是 isDeleted()， <font color = red>RPC框架在反向解析的时候， “误以为” 对应的属性名称是 deleted，导致属性获取不到，进而抛出异常。 </font>

#### 5、【推荐】如果模块、接口、类、方法使用了设计模式，在命名时需体现具体模式

例如：public class OrderFactory; 

​			public class ResourceObserver; 

#### 6、【推荐】如果是形容能力的接口名称，取对应的形容词为接口名（通常是–able 的形式） 

例如：AbstractTranslator 实现 Translatable 接口 

#### 7、【参考】枚举类名建议带上Enum后缀，成员名称需要全大写，单词间用下划线隔开

说明： 枚举其实就是特殊的类， 域成员均为<font color = red>常量</font>， 且构造方法被<font color = red>默认强制是私有</font>
正例： 枚举名字为 ProcessStatusEnum 的成员名称： SUCCESS / UNKNOWN_REASON。

#### 8、【参考】各层命名规约： 

##### A) Service/DAO 层方法命名规约 

1） 获取单个对象的方法用 get 做前缀。
2） 获取多个对象的方法用 list 做前缀，复数形式结尾如：listObjects。
3） 获取统计值的方法用 count 做前缀。
4） 插入的方法用 save/insert 做前缀。
5） 删除的方法用 remove/delete 做前缀。
6） 修改的方法用 update 做前缀。 

##### B) 领域模型命名规约 

1） 数据对象： xxxDO， xxx 即为数据表名。
2） 数据传输对象： xxxDTO， xxx 为业务领域相关的名称。
3） 展示对象： xxxVO， xxx 一般为网页名称。
4） POJO 是 DO/DTO/BO/VO 的统称，禁止命名成 xxxPOJO。 



## 二、常量定义

#### 1、【强制】不允许任何魔法值（即未经预先定义的常量） 直接出现在代码中 

禁止：<font color = red>String key = <font color = blue>"Id#taobao_" </font>+ tradeId;</font>

#### 2、【强制】 在 long 或者 Long 赋值时， 数值后使用大写的 L，不能是小写的 l，小写容易跟数字 1 混淆，造成误解

例如：Long a = 2L;

禁止：<font color = red>Long a = 2l;</font>

#### 3、【推荐】不要使用一个常量类维护所有常量， 要按常量功能进行归类，分开维护

例如：缓存相关常量放在类 CacheConsts 下； 系统配置相关常量放在类 ConfigConsts 下。 

#### 4、【推荐】常量的复用层次有五层：跨应用共享常量、应用内共享常量、子工程内共享常量、包
内共享常量、类内共享常量 

1） 跨应用共享常量：放置在二方库中，通常是 client.jar 中的 constant 目录下。
2） 应用内共享常量：放置在一方库中， 通常是子模块中的 constant 目录下。
禁止：<font color = red> 易懂变量也要统一定义成应用内共享常量</font>，两位攻城师在两个类中分别定义了表示“是”的变量：

类 A 中： public static final String YES = "yes";
类 B 中： public static final String YES = "y";

A.YES.equals(B.YES)，预期是 true，但实际返回为 false，导致线上问题。

3） 子工程内部共享常量：即在当前子工程的 constant 目录下。
4） 包内共享常量：即在当前包下单独的 constant 目录下。
5） 类内共享常量：直接在类内部 private static final 定义。 

#### 5、【推荐】 如果变量值仅在一个固定范围内变化用 enum 类型来定义 

## 三、代码格式

#### 1、【强制】大括号的使用约定。如果是大括号内为空，则简洁地写成{}即可，不需要换行； 如果
是非空代码块则： 

1） 左大括号前加空格不换行。
2） 左大括号后换行。
3） 右大括号前换行。
4） 右大括号后还有 else 等代码则不换行； 表示终止的右大括号后必须换行。 

​	if (xxx) {

​		xxx;

​	} else {

​		xxx;

​	}

​	xxxx

#### 2、【强制】 if/for/while/switch/do 等保留字与括号之间都必须加空格

#### 3、【强制】 采用 4 个空格缩进，禁止使用 tab 字符

说明： 如果使用 tab 缩进，必须设置 1 个 tab 为 4 个空格。 IDEA 设置 tab 为 4 个空格时，请勿勾选 <font color = red>Use tab character</font>；而在 eclipse 中，必须勾选 <font color = red> insert spaces for tabs</font>。 

标准如下：

![SpaceStandard](https://github.com/Mccreeeee/For-Dreams/blob/master/阿里Java开发规范学习/SpaceStandard.png)

#### 4、【强制】注释的双斜线与注释内容之间有且仅有一个空格 

例如：<font color = green>// 这是示例注释，请注意在双斜线之后有一个空格</font>

#### 5、【强制】单行字符数限制不超过 120 个，超出需要换行，换行时遵循如下原则： 

1） 第二行相对第一行缩进 4 个空格，从第三行开始，不再继续缩进，参考示例。
2） 运算符与下文一起换行。
3） 方法调用的点符号与下文一起换行。
4） 方法调用中的多个参数需要换行时， 在逗号后进行。
5） 在括号前不要换行，见反例。 

![NewlineStandard](https://github.com/Mccreeeee/For-Dreams/blob/master/阿里Java开发规范学习/NewlineStandard.png)

#### 6、【强制】IDE 的 text file encoding 设置为 UTF-8; IDE 中，文件的换行符使用 Unix 格式，不要使用 Windows 格式

#### 7、【推荐】单个方法的总行数不超过 80 行

说明： 包括方法签名、结束右大括号、方法内代码、注释、空行、回车及任何不可见字符的总行数不超过 80 行。

正例： 代码逻辑分清红花和绿叶， 个性和共性，绿叶逻辑单独出来成为额外方法，使主干代码更加清晰； 共性逻辑抽取成为共性方法，便于复用和维护。 

#### 8、【推荐】 不同逻辑、不同语义、不同业务的代码之间插入一个空行分隔开来以提升可读性

##  四、OOP规约

#### 1、【强制】避免通过一个类的对象引用访问此类的静态变量或静态方法，无谓增加编译器解析成本，直接用<font color = blue>类名</font>来访问即可 

#### 2、【强制】所有的覆写方法，必须加@Override 注解，所有过时的方法必须加@Deprecated注解同时清晰地说明采用的新接口/新服务是什么

#### 3、【强制】相同参数类型，相同业务含义，才可以使用 Java 的可变参数，避免使用 Object。

说明： 可变参数必须放置在参数列表的最后。（提倡同学们尽量不用可变参数编程）
正例： public List\<User\> listUsers(String type, Long... ids) {...} 

#### 4、【强制】 Object 的 equals 方法容易抛空指针异常，应使用常量或确定有值的对象来调用equals 

正例： <font color = green>"test".equals(object);</font>
反例： <font color = red>object.equals("test");</font>
说明： 推荐使用 java.util.Objects#equals（JDK7 引入的工具类） 

#### 5、【强制】所有的相同类型的包装类对象之间值的比较，全部使用 equals 方法比较

对于 Integer var = ? 在<font color = red>-128 至 127 </font>范围内的赋值， Integer 对象是在<font color = red>IntegerCache.cache </font>产生，会复用已有对象，这个区间内的Integer 值可以直接使用==进行判断，但是超过这个区间就在堆上产生对象，此时就不能用==判断了，为了保证一致性，使用equals方法进行比较

#### 6、关于基本数据类型与包装数据类型的使用标准如下：
1） 【强制】 所有的 POJO 类属性必须使用包装数据类型
2） 【强制】 RPC 方法的返回值和参数必须使用包装数据类型
3） 【推荐】 所有的局部变量使用基本数据类型

这些要求均是为了避免NPE问题，即NullPointerException问题

#### 7、【强制】定义 DO/DTO/VO 等 POJO 类时，不要设定任何属性<font color = blue>默认值</font>

此要求是为了避免以下情况：POJO 类的 gmtCreate 默认值为 new Date()， 但是这个属性在数据提取时并没有置入具体值，更新其它字段时会附带更新了此字段，导致创建时间被修改

#### 8、【强制】序列化类新增属性时，请不要修改serialVersionUID 字段，避免反序列失败； 如果完全不兼容升级，避免反序列化混乱，那么请修改 serialVersionUID 值 

#### 9、【强制】POJO类必须写toString方法，使用 IDE 中的工具： source> generate toString时，如果继承了另一个 POJO 类，注意在前面加一下super.toString

#### 10、【推荐】 类内方法定义的顺序依次是：公有方法或保护方法 > 私有方法 > getter/setter方法

<font color = brown>说明</font>： 公有方法是类的调用者和维护者最关心的方法，首屏展示最好； 保护方法虽然只是子类关心，也可能是“模板设计模式”下的核心方法； 而私有方法外部一般不需要特别关心，是一个黑盒实现； 因为承载的信息价值较低，所有 Service 和 DAO 的 getter/setter 方法放在类体最后

#### 11、【推荐】循环体内，字符串的连接方式，使用 StringBuilder 的 append 方法进行扩展

#### 12、【推荐】慎用 Object 的 clone 方法来拷贝对象 

原因：对象的clone方法默认是浅拷贝（基本类型是复制值，对象类型是复制引用）

#### 13、【推荐】类成员与方法访问控制从严：

1） 如果不允许外部直接通过 new 来创建对象，那么构造方法必须是 private。
2） 工具类不允许有 public 或 default 构造方法。
3） 类非 static 成员变量并且与子类共享，必须是 protected。
4） 类非 static 成员变量并且仅在本类使用，必须是 private。
5） 类 static 成员变量如果仅在本类使用，必须是 private。
6） 若是 static 成员变量， 考虑是否为 final。
7） 类成员方法只供类内部调用，必须是 private。
8） 类成员方法只对继承类公开，那么限制为 protected。 

## 五、集合处理

#### 1、【强制】 关于 hashCode 和 equals 的处理，遵循如下规则： 

1） 只要重写 equals，就必须重写 hashCode。
2） 因为 Set 存储的是不重复的对象，依据 hashCode 和 equals 进行判断，所以 <font color = red>Set 存储的对象</font>必须重写这两个方法。
3） 如果<font color = red>自定义对象作为 Map 的键</font>，那么必须重写 hashCode 和 equals。 

#### 2、【强制】ArrayList的subList结果不可强转成ArrayList，否则会抛出 ClassCastException异常， 即 java.util.RandomAccessSubList cannot be cast to java.util.ArrayList。

<font color = brown>注意</font>：subList 返回的是 ArrayList 的内部类 SubList，并不是 ArrayList 而是 ArrayList的一个视图，对于 SubList 子列表的所有操作最终会反映到原列表上 ，简单来说，<font color = red>SubList不是ArrayList的子类，无法强转</font>

#### 3、【强制】在 subList 场景中， 高度注意对原集合元素的增加或删除， 均会导致子列表的遍历、增加、删除产生ConcurrentModificationException 异常 

原因：这个异常是两方modCount不一致所导致的，在subList的时候会将原List的modCount复制一份过来，<font color = red>只有在对SubList进行更改时才会修改两方的modCount，若只修改原List，是不会影响到SubList的modCount的</font>，这时就会导致不一致的问题

#### 4、【强制】使用集合转数组的方法，必须使用集合的 toArray(T[] array)，传入的是类型完全一样的数组，大小就是 list.size() 

原因：使用带参toArray方法时，入参分配的数组空间不够大时， toArray 方法内部将重新分配内存空间，并返回新数组地址； 如果数组元素个数大于实际所需，下标为[ list.size() ]的数组元素将被置为 null（用作标记，表明这之后的数都不用管它，不需要复制过去了），其它数组元素保持原值，因此<font color = red>最好将方法入参数组大小定义与集合元素个数一致 </font>，而使用无参函数的话返回值为Object[]，只能一个一个元素的强转，否则会有ClassCastException错误

#### 5、【强制】使用工具类 Arrays.asList()把数组转换成集合时，不能使用其修改集合相关的方法，它的 add/remove/clear 方法会抛出UnsupportedOperationException 异常 

<font color = brown>说明</font>： asList 的返回对象是一个 Arrays 内部类，并没有实现集合的修改方法。Arrays.asList体现的是适配器模式，只是转换接口，后台的数据仍是数组，传入的数组的引用被赋值给<font color = red>Arrays内部的静态内部类ArrayList</font>的属性E[] a，get、set操作都用的这个属性，所以外部传入的数组发生改变，这个a也会发生改变，get的值也会改变，这个静态内部类<font color = red>未重写add和remove</font>，用的是父类的，父类的是直接抛异常

#### 6、【强制】泛型通配符<? extends T>来接收返回的数据，此写法的泛型集合不能使用 add 方法， 而<? super T>不能使用 get 方法，作为接口调用赋值时易出错 

<font color = brown>说明</font>： 扩展说一下 PECS(Producer Extends Consumer Super)原则： 第一、 频繁往外读取内容的，适合用<? extends T>。 第二、 经常往里插入的，适合用<? super T> 

#### 7、不要在 foreach 循环里进行元素的 remove/add 操作。 remove 元素请使用 Iterator方式，如果并发操作，需要对 Iterator 对象<u>加锁</u> 

![IteratorDefinition](https://github.com/Mccreeeee/For-Dreams/blob/master/阿里Java开发规范学习/IteratorDefinition.png)![Iterator](https://github.com/Mccreeeee/For-Dreams/blob/master/阿里Java开发规范学习/Iterator.png)

原因：第一次不报错是因为”1”被移除后，it的corsor = 1，list的size = 1，it的hasNext 方法{return corsor != size}返回false，直接结束了循环，没有进入到next方法内部执行checkForComodification方法，而第二次 移除 “2”以后 it的corsor = 2，list的size = 1，it的hasNext 方法 {return corsor != size}返回true。 继续执行了 it的next方法进入到next方法内部执行checkForComodification方法发现modCount和expCount不同，抛异常**java.util.ConcurrentModificationException**

#### 8、【推荐】集合初始化时， 尽量指定集合初始值大小 ，避免其自动扩增影响性能

<font color = brown>说明</font>： HashMap 使用 HashMap(int initialCapacity) 初始化。

<font color = green>正例</font>：initialCapacity = (需要存储的元素个数 / 负载因子) + 1（避免HashMap从数组链表转红黑树这个步骤而影响性能）。注意负载因子（即 loader factor） 默认为 0.75， 如果暂时无法确定初始值大小，请设置为 16（即默认值）。

<font color = red>反例</font>： HashMap 需要放置 1024 个元素， 由于没有设置容量初始大小，随着元素不断增加，容量 7 次被迫扩大， resize 需要重建 hash 表，严重影响性能。 

#### 9、【推荐】使用 entrySet 遍历 Map 类集合 KV，而不是 keySet 方式进行遍历

#### 10、【推荐】高度注意 Map 类集合 K/V 能不能存储 null 值的情况，如下表格： 

![Map&null](https://github.com/Mccreeeee/For-Dreams/blob/master/阿里Java开发规范学习/Map&null.png)

#### 11、【参考】利用 Set 元素唯一的特性，可以快速对一个集合进行<u>去重</u>操作，避免使用 List 的contains 方法进行遍历、对比、 去重操作

## 六、并发处理

#### 1、【强制】 获取单例对象需要保证线程安全，其中的方法也要保证线程安全

#### 2、【强制】线程资源必须通过线程池提供，不允许在应用中自行显式创建线程

<font color = brown>说明</font>： 使用线程池的好处是<font color = red>减少在创建和销毁线程上所消耗的时间以及系统资源的开销，解决资源不足</font>的问题。如果不使用线程池，有可能造成系统创建大量同类线程而导致<font color = red>消耗完内存或者“过度切换”</font>的问题

#### 3、【强制】线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险 

<font color = brown>说明</font>： Executors 返回的线程池对象的弊端如下：
1） <font color = red>FixedThreadPool 和 SingleThreadPool</font>:
允许的请求队列长度为 Integer.MAX_VALUE，可能会堆积大量的请求，从而导致 OOM。
2） <font color = red>CachedThreadPool 和 ScheduledThreadPool</font>:
允许的创建线程数量为 Integer.MAX_VALUE， 可能会创建大量的线程，从而导致 OOM。 

```java
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue,
                          ThreadFactory threadFactory,
                          RejectedExecutionHandler handler)
```

#### 4、【强制】 SimpleDateFormat 是线程不安全的类，一般不要定义为 static 变量，如果定义为static，必须加锁，或者使用 DateUtils 工具类

![SimpleDateFormat](https://github.com/Mccreeeee/For-Dreams/blob/master/阿里Java开发规范学习/SimpleDateFormat.png)

<font color = brown>说明</font>： 如果是 JDK8 的应用，可以使用 Instant 代替 Date， LocalDateTime 代替 Calendar，DateTimeFormatter 代替 SimpleDateFormat，官方给出的解释： simple beautiful strong immutable thread-safe 

#### 5、【强制】并发修改同一记录时，避免更新丢失， 需要加锁。 要么在应用层加锁，要么在缓存加锁，要么在数据库层使用乐观锁，使用 version 作为更新依据 

<font color = brown>说明</font>： 如果每次访问冲突概率小于 20%，推荐使用乐观锁，否则使用悲观锁。乐观锁的重试次数不得小于 3 次 

#### 6、【强制】多线程并行处理定时任务时， Timer 运行多个 TimeTask 时，只要其中之一没有捕获抛出的异常，其它任务便会自动终止运行，使用<u>ScheduledExecutorService</u> 则没有这个问题 

#### 7、【推荐】使用 CountDownLatch 进行异步转同步操作，每个线程退出前必须调用 countDown方法，线程执行代码注意 catch 异常，确保 countDown 方法被执行到，避免主线程无法执行至 await 方法，直到超时才返回结果 

#### 8、【推荐】避免 Random 实例被多线程使用，虽然共享该实例是线程安全的，但会因竞争同一seed 导致的性能下降 

<font color = brown>说明</font>： Random 实例包括 java.util.Random 的实例或者Math.random()的方式
<font color = green>正例</font>： 在 JDK7 之后，可以直接使用 API <u>ThreadLocalRandom</u>， 而在 JDK7 之前， 需要编码保证每个线程持有一个实例 

#### 9、【推荐】 在并发场景下， 通过双重检查锁（double-checked locking） 实现延迟初始化的优化问题隐患(可参考 The "Double-Checked Locking is Broken" Declaration)， 推荐解决方案中较为简单一种（适用于 JDK5 及以上版本） ，将目标属性声明为 volatile 型 

双重检查锁的正确示范：

```java
public class Singleton {
    private volatile static Singleton uniqueSingleton;

    private Singleton() {
    }

    public Singleton getInstance() {
        if (null == uniqueSingleton) {
            synchronized (Singleton.class) {
                if (null == uniqueSingleton) {
                    uniqueSingleton = new Singleton();
                }
            }
        }
        return uniqueSingleton;
    }
}
```

#### 10、【参考】 volatile 解决多线程内存不可见问题。对于一写多读，是可以解决变量同步问题，但是如果多写，同样无法解决线程安全问题。如果是 <u>count++</u>操作，使用如下类实现：AtomicInteger count = new AtomicInteger(); count.addAndGet(1); 如果是 JDK8，推荐使用 LongAdder 对象，比 AtomicLong 性能更好（减少乐观锁的重试次数） 

## 七、控制语句

#### 1、【强制】在一个 switch 块内，都必须包含一个 default 语句并且放在最后，即使空代码 ；在if/else/for/while/do 语句中即使只有一行代码，也必须使用大括号。  

#### 2、【强制】在高并发场景中，避免使用”等于”判断作为中断或退出的条件

<font color = brown>说明</font>： 如果并发控制没有处理好，容易产生等值判断被<font color = red>“击穿”</font>的情况，使用<font color = red>大于或小于的区间判断条件</font>来代替
<font color = red>反例</font>： 判断剩余奖品数量等于 0 时，终止发放奖品，但因为并发处理错误导致奖品数量瞬间变成了负数， 这样的话，活动无法终止

## 八、其它

#### 1、【强制】在使用正则表达式时，利用好其预编译功能，可以有效加快正则匹配速度

<font color = brown>说明</font>：Pattern要定义为static final静态变量，以避免执行多次预编译

```java
private static final Pattern pattern = Pattern.compile(regexRule);
 
private void func(...) {
    Matcher m = pattern.matcher(content);
    if (m.matches()) {
        ...
    }
}
```

#### 2、【强制】后台输送给页面的变量必须加$!{var}——中间的感叹号

<font color = brown>说明</font>： 如果 var 等于 null 或者不存在，那么${var}会直接显示在页面上 

#### 3、【强制】注意 Math.random() 这个方法返回是 double 类型，注意取值的范围 0≤x<1（能够取到零值，注意除零异常）

#### 4、【强制】获取当前毫秒数System.currentTimeMillis(); 而不是new Date().getTime()

<font color = brown>说明</font>： 如果想获取更加精确的纳秒级时间值， 使用System.nanoTime()的方式。在 JDK8 中，针对统计时间等场景，推荐使用 Instant 类 

#### 5、【推荐】防止 NPE，是程序员的基本修养

<font color = brown>说明</font>：可以使用 JDK8 的 Optional 类来防止 NPE 问题 

## 九、日志规约

#### 1、【强制】应用中不可直接使用日志系统（Log4j、 Logback） 中的 API，而应依赖使用日志框架SLF4J 中的 API，使用门面模式的日志框架，有利于维护和各个类的日志处理方式统一 

#### 2、【强制】应用中的扩展日志（如打点、临时监控、访问日志等） 命名方式：appName_logType_logName.log 

<font color = brown>说明</font>：logType:日志类型， 如 stats/monitor/access 等； logName:日志描述。这种命名的好处：通过文件名就可知道日志文件属于什么应用，什么类型，什么目的，也有利于归类查找 

推荐对日志进行分类， 如将错误日志和业务日志分开存放，便于开发人员查看，也便于通过日志对系统进行及时监控 

## 十、MySQL数据库

#### 1、【强制】表达是与否概念的字段，必须使用 is_xxx 的方式命名，数据类型是 unsigned tinyint（1 表示是， 0 表示否）

#### 2、【强制】表名、字段名必须使用小写字母或数字， 禁止出现数字开头，禁止两个下划线中间只出现数字。数据库字段名的修改代价很大，因为无法进行预发布，所以字段名称需要慎重考虑

<font color = brown>说明</font>： MySQL 在 Windows 下不区分大小写，但在 Linux 下默认是区分大小写。因此，数据库名、表名、字段名，都不允许出现任何大写字母，避免节外生枝 

#### 3、【强制】 主键索引名为 pk\_字段名； 唯一索引名为 uk\_字段名； 普通索引名则为 idx\_字段名

#### 4、【强制】小数类型为 decimal，禁止使用 float 和 double

<font color = brown>说明</font>： float 和 double 在存储的时候，存在<font color = red>精度损失</font>的问题，很可能在值的比较时，得到不正确的结果。如果存储的数据范围超过decimal 的范围，建议将数据拆成整数和小数分开存储 

#### 5、【强制】 varchar 是可变长字符串，不预先分配存储空间，长度不要超过 5000，如果存储长度大于此值，定义字段类型为 text，独立出来一张表，用主键来对应，避免影响其它字段索引效率 

#### 6、【强制】表必备三字段： id, gmt_create, gmt_modified

<font color = brown>说明</font>： 其中 id 必为主键，类型为 bigint unsigned、单表时自增、步长为 1。 gmt_create, gmt_modified 的类型均为 datetime 类型，前者现在时表示主动创建，后者过去分词表示被动更新 

#### 7、【推荐】单表行数超过 500 万行或者单表容量超过 2GB，才推荐进行分库分表

## 十一、索引规约

#### 1、【强制】业务上具有唯一特性的字段，即使是多个字段的组合，也必须建成唯一索引

#### 2、【强制】超过三个表禁止 join。需要 join 的字段，数据类型必须绝对一致； 多表关联查询时，保证被关联的字段需要有索引 

#### 3、【强制】页面搜索严禁左模糊或者全模糊，如果需要请走搜索引擎来解决

<font color = brown>说明</font>： 索引文件具有 B-Tree 的最左前缀匹配特性，如果左边的值未确定，那么无法使用此索引

#### 4、【推荐】如果有 order by 的场景，请注意利用索引的<u>有序性</u>。 order by 最后的字段是组合索引的一部分，并且放在索引组合顺序的最后，避免出现file_sort 的情况，影响查询性能 

<font color = green>正例</font>： where a=? and b=? order by c; 索引： a_b_c 

<font color = red>反例</font>： 索引中有范围查找，那么索引有序性无法利用，如： WHERE a>10 ORDER BY b; 索引a_b 无法排序 

#### 5、【推荐】利用延迟关联或者子查询优化超多分页场景

<font color = brown>说明</font>： MySQL 并不是跳过 offset 行，而是取 offset+N 行，然后返回放弃前 offset 行，返回N 行，那当 offset 特别大的时候，效率就非常的低下，要么控制返回的总页数，要么对超过特定阈值的页数进行 SQL 改写 

<font color = green>正例</font>： 先快速定位需要获取的 id 段，然后再关联：

```mysql
SELECT a.* FROM 表 1 a, (select id from 表 1 where 条件 LIMIT 100000,20 ) b where a.id=b.id
```

定位到第100000行的id，从这里继续

#### 6、【推荐】建组合索引的时候，区分度最高的在最左边

<font color = green>正例</font>： 如果 where a=? and b=? ， 如果 a 列的几乎接近于唯一值，那么只需要单建 idx_a索引即可

<font color = brown>说明</font>： 存在非等号和等号混合时，在建索引时，请把等号条件的列前置。如： where c>? and d=? 那么即使 c 的区分度更高，也必须把 d 放在索引的最前列， 即索引 idx_d_c 

## 十二、SQL语句

#### 1、【强制】 count(distinct col) 计算该列除 NULL 之外的不重复行数， 注意 count(distinct col1, col2) 如果其中一列全为 NULL，那么即使另一列有不同的值，也返回为 0

#### 2、【强制】当某一列的值全是 NULL 时， count(col)的返回结果为 0，但 sum(col)的返回结果为NULL，因此使用 sum()时需注意 NPE 问题

<font color = green>正例</font>： 可以使用如下方式来避免 sum 的 NPE 问题： 

```mysql
SELECT IF(ISNULL(SUM(g)),0,SUM(g)) FROM table
```

####  3、【强制】使用 ISNULL()来判断是否为 NULL 值 

<font color = brown>说明</font>： NULL 与任何值的直接比较都为 NULL
1） NULL<>NULL 的返回结果是 NULL， 而不是 false
2） NULL=NULL 的返回结果是 NULL， 而不是 true
3） NULL<>1 的返回结果是 NULL，而不是 true

#### 4、【强制】不得使用外键与级联，一切外键概念必须在应用层解决

<font color = brown>说明</font>：外键与级联更新适用于单机低并发，不适合分布式、高并发集群； 级联更新是强阻塞，存在数据库更新风暴的风险； 外键影响数据库的插入速度 

#### 5、【强制】禁止使用存储过程，存储过程难以调试和扩展，更没有移植性

#### 6、【参考】 TRUNCATE TABLE 比 DELETE 速度快，且使用的系统和事务日志资源少，但 TRUNCATE无事务且不触发 trigger，有可能造成事故，故不建议在开发代码中使用此语句 

## 十三、ORM框架

#### 1、【强制】更新数据表记录时，必须同时更新记录对应的 gmt_modified 字段值为当前时间

## 十四、应用分层

#### 1、【参考】分层领域模型规约：

**DO（Data Object）** ： 此对象与数据库表结构一一对应，通过 DAO 层向上传输数据源对象。
**DTO（Data Transfer Object）** ：数据传输对象， Service 或Manager 向外传输的对象。
**BO（Business Object）** ：业务对象， 由 Service 层输出的封装业务逻辑的对象。
**AO（Application Object）**： 应用对象， 在 Web 层与 Service 层之间抽象的复用对象模型，极为贴近展示层，复用度不高。
**VO（View Object）** ： 显示层对象，通常是 Web 向模板渲染引擎层传输的对象。
**Query**：数据查询对象，各层接收上层的查询请求。 注意超过 2 个参数的查询封装，禁止使用 Map 类来传输。 

# **TODO** LIST：

## （以下均只是粗略看过，待日后补遗）

# <font color = red>1、工程结构规范补遗</font>

# <font color = red>2、设计规约补遗</font>
