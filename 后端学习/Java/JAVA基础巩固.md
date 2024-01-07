

### 基础概念

#### 基础

- javaSE和javaEE是基础版和企业开发版本的区别，javaEE更适合开发企业级应用或web
    
- java9开始就不需要区分JRE和JDK的关系了
    
- java从源码到运行的过程：
    
    - .java-> javac编译->.class->解释器&JIT-》机器能理解的代码
        
- java和C++的对比
    
    - java不提供指针访问内存，所以内存更安全
        
    - java类是单继承的，接口可以多继承，但是C++支持多继承
        
    - C++支持操作符重载和方法重载，java只支持方法重载
        
- 移位运算符
    
    - x>>1 // 右移运算相当于除2，数字是几就是除2的几次方
        
    - x<<1 //左移，相当于×2，同上
        
    - 由于左移位数大于等于 32 位操作时，会先求余（%）后再进行左移操作，所以代码左移 42 位相当于左移 10 位（42%32=10）
        
- 八种基本数据类型 byte short int long float double boolean char
    
- 包装类型的 **_缓存机制_**--就是valueOf方法有缓存机制
    
    - `Byte`,`Short`,`Integer`,`Long` 这 4 种包装类默认创建了数值 **[-128，127]** 的相应类型的缓存数据，`Character` 创建了数值在 **[0,127]** 范围的缓存数据，`Boolean` 直接返回 `True` or `False`。
        
    - 两个浮点类型的包装类没有实现缓存机制
        
- 应避免频繁的拆装箱
    
- 成员变量如果没有赋初始值，那么会自动以类型的默认值而赋值，但是成员变量没有（会编译时报错）
    
- 静态对象只会被赋一次内存,即使创建多个对象
    
- char在java中占两个字节
    
- 构造方法不能被重写，但是可以被重载（参数不同）
    
- 面对对象三大特征 封装 继承 多态（多态表现为父类引用指向子类实例）
    
- BigDecimal方法应该使用compareTo方法，因为bigDecimal的equals方法还会比较精度
    

#### String

- stringBuilder和StringBuffer都是继承自abstractStringBuilder方法，采用字符数组保存字符串，并提供很多修改字符串的方法
    
- String中的对象是不可变的，可以理解为常量，就是线程安全的，每次对String对象进行改变的时候都会生成一个新的String对象，然后指针指向新的对象并改变引用
    
- StringBuffer对字符的操作加了同步锁或者对方法加了同步锁，所以是线程安全的
    
- StringBuilder没有对方法加同步锁，所以是不安全的
    
- 每次对 `String` 类型进行改变的时候，都会生成一个新的 `String` 对象，然后将指针指向新的 `String` 对象。`StringBuffer` 每次都会对 `StringBuffer` 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 `StringBuilder` 相比使用 `StringBuffer` 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险
    
- String类型被final修饰，且是私有的，没有对外暴露方法，而且被final修饰不能被继承
    
- +和+=是专门为String类重载过的运算符，字符串的+和+=实际上是通过StringBuilder的append方法
    
- 不过，在循环内使用“+”进行字符串的拼接的话，存在比较明显的缺陷：**编译器不会创建单个 `StringBuilder` 以复用，会导致创建过多的 `StringBuilder` 对象**。
    
    - > java9以后这个问题得到了解决，也就是意味着可以放心使用+进行拼接了
        
- String.intern(),就是返回这个对象在常量池里的引用？新建一个对象：常量池里的字符串
    
- String是可以用== 来判断的，
    
    - String str = "string";  
        String newStr = str.intern();  
        // true   
        system.out.println(str == newStr)  
            
        
- 在编译期间可以确定的字符串，jvm会把他放到常量池中（在堆中）--==常量折叠==
    
    - 常量折叠不止能用在字符串，基本数据类型都有这种，字符串的加减乘除，以及string的+
        
    - **平时写代码的时候，避免创建多个字符串对象的拼接，因为这样会重新创建对象，可以使用StringBuilder或者StringBuffer** ==不过，字符串用了final修饰之后，可以让编译器当作常量来处理，那么编译器在编译期间就能确定值，就也会对其优化，进行常量折叠==
        

#### 异常

- 两个重要的Throwable子类：Exception 和 Error，简单来说就是可处理异常和无法处理的错误
    
- 受检异常和非受检异常（一个不catch不能通过编译，一个能编译）
    
    - runtimeException 都称之为非受检异常
        
        - 空指针 NPE
            
        - 数组越界
            
        - 非法入参
            
        - 类型转换错误
            
        - 算术错误，0/1
            
        - NumberFormat
            
        - ....
            
- finally块中不要有return，否则会忽略掉try块的return，try或者catch块中有return语句的时候，return的语句会被暂存在一个本地变量中，然后执行finally块，然后返回return ，但是当finally中有return时，那就会把本地变量变成finally中的return了
    
- ==finally块中的代码未必会执行==
    
    - 执行过程中虚拟机直接终止
        
    - 程序所在线程死亡
        
    - 关闭CPU
        
- 每次抛异常要new一个对象抛出，不然如果定义了静态变量的话会导致异常栈信息错乱
    

#### 泛型

- 泛型有三种
    
    - 泛型类
        
    - 泛型接口
        
    - 泛型方法
        
- 泛型不能用在异常处理的catch中，因为异常处理是运行时的JVM处理的，无法区别`MyException`和`MyException`的
    

#### 反射

反射是框架的灵魂，框架中都会有大量的反射机制，动态代理也依赖反射

通过类加载器获取 Class 对象不会进行初始化，意味着不进行包括初始化等一系列步骤，静态代码块和静态对象不会得到执行

#### 注解

注解的本质是一个实现了annotation的特殊接口

#### SPI

##### SPI和API的区别

广义上他们都属于接口，SPI更像给开发者的接口，API则是给调用者的接口

![img](file://D:/study/img/1ebd1df862c34880bc26b9d494535b3dtplv-k3u1fbpfcp-watermark.png?lastModify=1703848504)

#### 序列化和反序列化

什么场景需要序列化和反序列化？

如果需要持久化Java对象，比如将对象保存在文件中或者网络传输java对象，都需要序列化

#### 代理模式

简单理解就是给你的对象穿装备，目的是拓展对象的功能

- 静态代理
    
- 动态代理 ==重点（尤其在框架中用的多）==
    
    - JDK动态代理机制：
        
        > 核心是InvocationHandler接口和Proxy类
        
    
    创建一个代理之后要实现一个handler类，用proxy传建的类（Object强转），然后这个类就相当于穿了一层装备，每次调用方法实际是走的你实现的那个handler类里面的invoke方法，这样你可以在handler实现里面加上自己的处理
    

==？？？**序列化协议对应于 TCP/IP 4 层模型的哪一层？这个后续再看，不太懂**==

#### Java魔法类unsaffe ==???不太懂容后再看==

介绍

> `Unsafe` 是位于 `sun.misc` 包下的一个类，主要提供一些用于执行低级别、不安全操作的方法，如直接访问系统内存资源、自主管理内存资源等，这些方法在提升 Java 运行效率、增强 Java 语言底层资源操作能力方面起到了很大的作用。但由于 `Unsafe` 类使 Java 语言拥有了类似 C 语言指针一样操作内存空间的能力，这无疑也增加了程序发生相关指针问题的风险。在程序中过度、不正确使用 `Unsafe` 类会使得程序出错的概率变大，使得 Java 这种安全的语言变得不再“安全”，因此对 `Unsafe` 的使用一定要慎重。
> 
> ---
> 
> 著作权归JavaGuide(javaguide.cn)所有 基于MIT协议 原文链接：[https://javaguide.cn/java/basis/unsafe.html](https://javaguide.cn/java/basis/unsafe.html)

#### java语法糖

- 条件编译，有局限，在通过判断常量之后会把确定的不会走的语句去掉再去编译
    
- assert 底层逻辑就是if判断
    
- 数值字面量：不管是浮点数还是整数，java允许我们在数值中加入任意多个下划线，这些下划线不会对数字产生影响，目的就是为了方便阅读
    
    int i = 100_000_12;
    

### 集合

java集合，也叫做容器，主要时两大类派生出来的，

- 一个是collection 用于存储单一的值，
    
    - collection下面又分为三个大类
        
        - List
            
            - arrayList
                
            - vector
                
            - linkedList
                
        - set
            
            - hashSet
                
            - linkedHashSet
                
            - TreeSet:(红黑树，自平衡的排序二叉树)
                
        - queue
            
            - arryQueue
                
            - DelayQueue
                
            - PriorityQueue
                
- 一个是Map用来存贮键值对
    
    - hashMap
        
    - linkedhashMap
        
    - Hashtable
        
    - TreeMap
        

#### List

- arrayList里面不能储存基本类型的数据，，只能储存包装类，但是数组可以，（记：数组创建必须要确定大小）
    
- **arrayList的扩容机制**
    
    - 无参构造Arraylist的时候，先是初始化一个空数组
        
    - 然后真正赋值的时候，添加第一个数组的时候，数组容量扩大为10
        
    
- ArrayList和Vector的区别，Vector很古老，线程安全（不是很推荐使用，线程安全可以用CopyOnWriteArrayList或者自己手动实现），arrayList属于新写法
    
- ==ArrayList插入和删除元素的时间复杂度==
    
    - 头部插入，由于需要将所有的元素都往后移动一个位置，所以时间复杂度是O（n）
        
    - 尾部插入
        
        - 尾部插入分两种情况，一个是arrayList没有到达极限大小，那么复杂度就是O(1)
            
        - 如果数组到达了极限大小，那么就要先做一个O（n)的操作把数组全体复制到一个大的数组中，再做一个O（1）的添加操作
            
        - 指定位置插入，同头部插入O(n)
            
    - 删除操作同理
        
- ==LinkedList插入删除的时间复杂度==
    
    - 头部插入/删除：只需要修改头结点的指针即可完成插入/删除操作，因此时间复杂度为 O(1)。
        
        尾部插入/删除：只需要修改尾结点的指针即可完成插入/删除操作，因此时间复杂度为 O(1)。
        
        指定位置插入/删除：需要先移动到指定位置，再修改指定节点的指针完成插入/删除，因此需要移动平均 n/2 个元素，时间复杂度为 O(n)。
        
        ---
        
        著作权归JavaGuide(javaguide.cn)所有 基于MIT协议 原文链接：[https://javaguide.cn/java/collection/java-collection-questions-01.html](https://javaguide.cn/java/collection/java-collection-questions-01.html)
        
- LinkedList为什么不能实现randomAccess接口
    
    randomAccess接口是一个标记接口，他的作用是表面，实现他的类可以随机地址访问（就是通过索引快速访问元素），但是linkedList底层是链表，地址不连续，只能通过指针访问，不支持快速随机访问
    
    > **所以其实，linkedList并没有在增删性能上优于arraylist，指定位置插入的删除复杂度也是O(n)**
    

==随机访问时通过索引快速访问，数组天然支持快速随机访问，时间复杂度是O(1),而链表需要遍历到指定位置才能访问特定位置的元素，arrayList的底层就是数组，所以大多数场景，arrayList要比linkedList更好用==

#### Set

- comparable 和Comparator的区别：**实现方法的参数不同**

- 无序性和不可重复性的含义是什么
    
    - 无序性不是说就是随机排列，而是不是按照数组的索引顺序添加，是按照元素的hash值决定的
        
        - 不可重复性是指元素的判断相同时，元素的hashCode和equals方法不能不一致，要同时重写这两个方法
            

#### queue和Deque的区别

- queue是单端队列，先进先出  
- deque是双端队列

#### ArrayDeque和linkedList的区别

其实这俩都实现了Deque的接口

- arrayDeque是基于可变长的数组和双指针来实现的，但是linkedList是基于链表来实现的
    
- ArrayDeque不能存储null，但是linkedList可以
    
- arrayDeque在插入时可能会有扩容，不过均摊后复杂度还是O（1）,linkedList不需要扩容，但是每次插入的时候都要申请新的堆空间，性能更慢
    
    > 性能角度上，arrayDeque会更好
    

#### priorityQueue

作用是优先级更高的元素先出队，优先级用comparator来定义

#### blockingQueue

阻塞队列，支持没有元素时一直阻塞到有元素，还支持当队列已满的时候会一直等到可以插入的时候再插入新元素

- arrayBlockingQueue等  
- ....了解即可

#### Map==重要==

#### hashMap和Hashtable的区别

- hashMap是非线程安全的，但是hashTable是线程安全的，（但是hashTable快要被淘汰了，不要在代码里面使用他，想要线程安全可以用concurrentHashMap,**concurrenthashmap里面的key不能为null**）
    
- hashMap是支持null 的key和value的，但是key只能有一个，hashtable会报空指针
    
- hashmap 的默认大小是16，以后每次扩容都变成原来的2倍，hashtable默认大小是11,以后每次扩容都会变成原来的2n+1
    
- 如果创建时给定了大小，那么hashTable会直接使用给定大小，但是hashMap会把他扩充成2的幂次方（**为了尽量把数据分配均匀，减少碰撞，碰撞会产生链表**）
    
- hashMap解决hash冲突时候（hash冲突会产生链表结构），当链表大于阈值（默认为8）就会先判断，if 当前数组长度小于64，那么就优先进行数组扩容，如果大于64则转链表为红黑树，而hashTable没有这个机制
    

#### hashMap和hashSet的区别

> 没啥大区别，Hash Set的底层就是基于HashMap实现的

#### treeMap

treeMap可以实现Comparator接口实现key排序

#### hashSet如何去重？

通过hashCode和equals方法

#### hashMap多线程操作导致死循环（了解就行）

#### hashMap常见的遍历方式

- 迭代器
    
    - EntrySet
        
    - EntryKey
        
- ForEach
    
- Lambada
    
- Stream
    

#### 集合的一些注意事项

- **在使用 `java.util.stream.Collectors` 类的 `toMap()` 方法转为 `Map` 集合时，一定要注意当 value 为 null 时会抛 NPE 异常。**
    
- 不要在foreach里面进行元素的remove/add操作，remove元素请用Iterator的方式，如果是并发操作，请加锁
    
- ==使用Arrays.asList()的时候的注意事项==
    
    - 第一这个数组不能是基本数据类型的，比如 int[] arr = {1,2,3}，Arrays.aslist(arr)会报错，要使用包装类型
        
    - 使用集合的add,remove等方法会报错，因为`Arrays.asList()` 方法返回的并不是 `java.util.ArrayList` ，而是 `java.util.Arrays` 的一个内部类,这个内部类并没有实现集合的修改方法或者说并没有重写这些方法。
        
        ---
        
        著作权归JavaGuide(javaguide.cn)所有 基于MIT协议 原文链接：[https://javaguide.cn/java/collection/java-collection-precautions-for-use.html](https://javaguide.cn/java/collection/java-collection-precautions-for-use.html)
        
    

#### ==源码分析暂时没怎么看，略扫了一眼，后面再研究，不要太钻牛角尖==

### 并发编程

#### 线程与进程

- 线程比进程更小，但是同类的多个线程共享资源，而且每个线程都有自己的程序计数器和虚拟栈等
    
- JAVA天生多线程，一个java程序的运行是main线程和多个其他线程同时运行
    
- 一个进程可以有多个线程，线程共享进程的堆和方法区，但是每个线程有自己的**_虚拟机栈_**，**_程序计数器_**和**_本地方法栈_**（==私有的==）
    
    - 虚拟机栈
        
        - 虚拟机栈相当于一个执行java的小空间
            
    - 程序计数器
        
        - 程序计数器是记录当前线程的运行位置，方便切回来的时候，线程知道自己运行到哪了，方便实现代码的流程控制
            
    - 本地方法栈
        
        - 和虚拟机栈类似，这个是用来执行native方法的
            

#### 并发和并行

- 并发是指两个及以上的作业在同一时间段运行  
- 并行是指两个及以上的作业同时运行

#### 线程的生命周期和状态（六种）

- 初始
    
- 运行
    
    - 调用strat（）方法开始运行
        
        > 初始和运行状态在JVM中都视作runnable状态
        
- 阻塞
    
    - 当线程进入了synchronized 方法块，但是这个时候锁被其他线程占用，这个时候，线程就会进入到RUNNABLE状态
        
- 等待
    
    - 执行wait（）方法之后，就会进入等待状态，这个时候只能通过其他线程的notify才能回到运行时状态
        
- 超时等待
    
    - 等于在等待状态的基础上加了超时限制，比如sleep。等过了超时时间，会自动回到runnable状态
        
- 终止状态
    

#### 上下文切换（context）

==什么是上下文==：线程在执行过程中会有自己的运行过程和状态，就称之为上下文，上下文就是说是保存线程的执行状态，虚拟机栈，本地方法栈，以及程序计数器状态的一个统称

==上下文切换发送的条件？==：就是说线程sleep，或者报错，终止，时间片用完，都会切换，换到别的线程的上下文

==频繁的切换会导致整体效率低下==

#### 线程死锁

- 什么是线程死锁
    
    - 举个例子，线程 A 持有资源 2，线程 B 持有资源 1，他们同时都想申请对方的资源，所以这两个线程就会互相等待而进入死锁状态
        
    - 代码为例
        
        - public class DeadLockDemo {
                private static Object resource1 = new Object();//资源 1
                private static Object resource2 = new Object();//资源 2
            
                public static void main(String[] args) {
                    new Thread(() -> {
                        synchronized (resource1) {
                            System.out.println(Thread.currentThread() + "get resource1");
                            try {
                                Thread.sleep(1000);
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                            System.out.println(Thread.currentThread() + "waiting get resource2");
                            synchronized (resource2) {
                                System.out.println(Thread.currentThread() + "get resource2");
                            }
                        }
                    }, "线程 1").start();
            
                    new Thread(() -> {
                        synchronized (resource2) {
                            System.out.println(Thread.currentThread() + "get resource2");
                            try {
                                Thread.sleep(1000);
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                            System.out.println(Thread.currentThread() + "waiting get resource1");
                            synchronized (resource1) {
                                System.out.println(Thread.currentThread() + "get resource1");
                            }
                        }
                    }, "线程 2").start();
                }
            }
            
        - Thread[线程 1,5,main]get resource1
            Thread[线程 2,5,main]get resource2
            Thread[线程 1,5,main]waiting get resource2
            Thread[线程 2,5,main]waiting get resource1
            
- 如何预防和避免线程死锁？
    
    - 预防（三点）
        
        - 第一是破坏请求与保持条件，比如一次性就申请所有的需要的资源，而不是一点一点申请
            
    - 破坏不剥夺条件：占用部分资源的线程在进一步申请其他资源的时候，如果没有申请到，那就释放掉自己占用的资源
        
    - 给申请资源的时候，_排序_，固定好释放和请求的顺序，按照某一顺序申请资源，释放也是按照顺序反向释放
        
    - 避免
        
        - 借助算法（比如等线程一跑完，线程二才能申请到资源）
            

#### sleep和wait方法

- sleep()没有释放锁，但是wait方法释放了锁
    
- wait()方法调用后，线程不会自动苏醒，要靠notify来唤醒，或者可以设置wait（long timeout），超时等待也会超时后苏醒，sleep会自动苏醒
    
- sleep是Thread类的静态本地方法，而wait（）则是Object的本地方法
    

#### 可以直接调用Thread类的run方法吗

这个问题要从线程启动的流程来看

- 首先创建一个线程，new一个Thread以后，调用他的start方法，就会启动该线程，做一些准备工作，等到时间片分配到了以后，会自动执行他的run方法，这是真正的_多线程工作_，但是如果直接执行他的run方法的话，那这个线程就会被视为mian线程下的普通方法执行，而不是多线程工作了
    

#### volatile关键字和synchronized关键字（互补）

- volatile关键字修饰的变量保证了变量的可视性，但是没有保证其原子性，就是volatile关键字修饰的的变量是共享的，每次使用都要去主内存去读取
- synchrinized关键字是既能保证可视性，又能保证原子性

- 如何保证原子性（改进）？
    
    - 对于volitile变量，操作他的方法加synchronized关键字
        
        - 数字可以用automaticInteger来代替
            
    - 用ReentrantLock改进
        
- ==Synchronized==
    
    - 修饰实例方法
        
    - 修饰静态方法
        
    - 修饰代码块
        
        > 静态synchronized方法和实例synchronized方法调用互斥吗？不互斥，因为一个是类的锁，一个是实例的锁
        
    - 尽量不要使用Synchronized(String)，因为有常量池这个东西，你一锁，就锁一片，影响其他线程
        
    - 构造方法本身就属于线程安全，不能用Syncronized 修饰
        
        > Synchronized的底层原理，主要原理在于两个指令
        > 
        > 一个是monitorenter，一个是monitorexit，
        
        ![执行 monitorenter 获取锁](https://oss.javaguide.cn/github/javaguide/java/concurrent/synchronized-get-lock-code-block.png)
        
        ![执行 monitorexit 释放锁](file://D:/study/img/synchronized-release-lock-block.png?lastModify=1703848504)
        

##### 禁止指令重排序

volatile除了保证变量的可变性，还有一个重要作用就是==防止JVM指令重排序==

>  什么是指令重排序
>
>  ​	为了提升执行速度/性能，计算机在执行程序代码的时候，会执行指令重排序，就是说，计算机执行程序的时候，不一定按照你代码写的顺序来

- 双重检验锁方式实现单例模式
    
    - public class SingleTon{
        	private static volite SingleTon singleTon;
            
            private SingleTon(){
            }
            
            public static SingleTon getInstance(){
            	if(singleTon == null){
                	synchronized(SingleTon.class){
                    	if(singleTon == null){
                            // 这一段实际上分三步执行    
                        	singleTon = new singleTon()
                        }
                    }
                }
                return singleTon;
            }
            
        }
        
    - `singleTon = new singleTon()`这一段实际分三步执行，
        
        1. 为singleTon分配内存空间
            
        2. 初始化singleTon
            
        3. 将singleTon指向分配的地址
            
        
        但是由于JVM有指令重排的特性，这一步可能变成1-3-2，单线程环境下没有问题，多线程环境下就有问题出现了
        
        `比如，线程A执行了1 3 然后时间片耗尽，这时候，线程2执行了这一段，发现singleTon不为null，就直接拿到了未初始化的singleTon对象，所以这个时候，volitile关键字就很有必要了`
        

#### 乐观锁和悲观锁

- 悲观锁：认为共享的资源每次都会被修改，所以每次申请资源的时候，都会上锁，其他想要访问的线程都会阻塞一直到上一个拿到资源的线程释放线程
    
    - 像java中的synchronized 和reentrantLock就是悲观锁的思想实现
        
        - _缺点_，在高并发的场景下，频繁的阻塞会很浪费资源，增加性能开销，而且还可能有死锁的风险
            
- 乐观锁：认为线程是可以无限制的执行下去的，每次资源都不会被其他线程修改，但是在提交的时候会做一个验证，判断资源是否已经被其他线程修改过了，修改过了我就不给你修改了或者我就再重试一遍，重新拿资源修改一遍（版本号机制或者CAS算法）
    
    - 版本号机制：
        
        就是取资源的时候，资源上加一个version版本号，然后我修改完成以后，再看一下数据库里面这个版本号是不是跟我取的一样，一样就说明没别人动过，就改掉，顺便version加一，不一样就说明别人动过，那就驳回请求
        
    - CAS
        
        > CAS 是一个原子操作，底层依赖于一条 CPU 的原子指令。
        > 
        > > **原子操作** 即最小不可拆分的操作，也就是说操作一旦开始，就不能被打断，直到操作完成。
        > 
        > CAS 涉及到三个操作数：
        > 
        > - **V**：要更新的变量值(Var)
        >     
        > - **E**：预期值(Expected)
        >     
        > - **N**：拟写入的新值(New)
        >     
        > 
        > 当且仅当 V 的值等于 E 时，CAS 通过原子方式用新值 N 来更新 V 的值。如果不等，说明已经有其它线程更新了 V，则当前线程放弃更新。
        > 
        > **举一个简单的例子**：线程 A 要修改变量 i 的值为 6，i 原值为 1（V = 1，E=1，N=6，假设不存在 ABA 问题）。
        > 
        > 1. i 与 1 进行比较，如果相等， 则说明没被其他线程修改，可以被设置为 6 。
        >     
        > 2. i 与 1 进行比较，如果不相等，则说明被其他线程修改，当前线程放弃更新，CAS 操作失败。
        >     
        > 
        > 当多个线程同时使用 CAS 操作一个变量时，只有一个会胜出，并成功更新，其余均会失败，但失败的线程并不会被挂起，仅是被告知失败，并且允许再次尝试，当然也允许失败的线程放弃操作。
        > 
        > Java 语言并没有直接实现 CAS，CAS 相关的实现是通过 C++ 内联汇编的形式实现的（JNI 调用）。因此， CAS 的具体实现和操作系统以及 CPU 都有关系。
        > 
        > ---
        > 
        > 著作权归JavaGuide(javaguide.cn)所有 基于MIT协议 原文链接：[https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html](https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html)
        
- 乐观锁存在哪些问题？
    
    - ABA问题：解决思路是CAS基础上加版本号或者时间戳
        
    - 多写入场景频繁的重试或者失败会导致性能问题
        

#### ReentrantLock

ReentrantLock是一个实现了Lock接口的类，作用和Synchronized类似，是一个`可重入且独占式的锁`，但是更强大 (`增加了，轮询，超时，中断，公平锁和非公平锁等高级功能`)

- 公平锁
    
    - 锁被释放以后，先申请的线程先获得锁，但是会导致上下文频繁切换
        
- 非公平锁
    
- 锁释放后，可能随机线程获得锁，但是可能导致某些线程永远拿不到锁
    
- 可重入锁
    
    - 可重入锁也叫递归锁，这个可以解决部分死锁问题，简单例子就是一个线程拿到了一个对象的锁还没释放的时候，还能再获得该对象的锁，递归释放，目前JDK提供的LOCK实现类都是可重入式锁
        
        - public class SynchronizedDemo {
                public synchronized void method1() {
                    System.out.println("方法1");
                    method2();
                }
            
                public synchronized void method2() {
                    System.out.println("方法2");
                }
            }
            
        - 由于 `synchronized`锁是可重入的，同一个线程在调用`method1()` 时可以直接获得当前对象的锁，执行 `method2()` 的时候可以再次获取这个对象的锁，不会产生死锁问题。假如`synchronized`是不可重入锁的话，由于该对象的锁已被当前线程所持有且无法释放，这就导致线程在执行 `method2()`时获取锁失败，会出现死锁问题。
            
- 中断（获取锁的过程可中断）
    
    - `ReentrantLock`提供了一种能够中断等待锁的线程的机制，通过 `lock.lockInterruptibly()` 来实现这个机制。也就是说正在等待的线程可以选择放弃等待，改为处理其他事情
        

#### ThreadLocal

他是什么？为什么出现

- 首先，我们创建的变量在通常情况下都是能被所有线程访问修改的，但是这时候我们希望，能让线程拥有自己的专属本地变量，应该怎么办
    
- ThreadLocal就出现了，用get(),set()或者独属于线程自己的本地变量，主要是避免线程竞争？？ `这个是不是就相当于变量锁一直不放`?
    
- 原理解析：
    

> Thread 类里面有两个Map，一个是自身的map（ThreadLocalMap），一个是继承下来的map(inherientThreadLocalMap)，一开始，这两个都是null（ThreadLocalMap可以视作为ThreadLocal类定制实现的HashMap），我们在外面创建一个Thread的子类的时候，给他创建了一个成员变量ThreadLocl
> 
>  private static final ThreadLocal<SimpleDateFormat> formatter = ThreadLocal.withInitial(() -> new SimpleDateFormat("yyyyMMdd HHmm"));
> 
> 类似上面这样，那么线程重写（实现）run方法的时候，要在方法里调用这个formatter.get()或者set()的时候，就会初始化这个线程的这个定制的hashMap，然后拿值或者取值，所以实际上这个变量会存在线程中的ThreadLocalmaps里面，哪怕你给子类写了两个ThreadLocal，存还是存在一个ThreadLocalmap里面,key就是ThreadLocal对象，value就是set放的值

![ThreadLocal 数据结构](file://D:/study/img/threadlocal-data-structure.png?lastModify=1703848504)

#### ThreadLocal会导致内存泄漏问题？

`ThreadLocalMap` 中使用的 key 为 `ThreadLocal` 的弱引用，而 value 是强引用。所以，如果 `ThreadLocal` 没有被外部强引用的情况下，在垃圾回收的时候，key 会被清理掉，而 value 不会被清理掉。

这样一来，`ThreadLocalMap` 中就会出现 key 为 null 的 Entry。假如我们不做任何措施的话，value 永远无法被 GC 回收，这个时候就可能会产生内存泄露。`ThreadLocalMap` 实现中已经考虑了这种情况，在调用 `set()`、`get()`、`remove()` 方法的时候，会清理掉 key 为 null 的记录。使用完 `ThreadLocal`方法后最好手动调用`remove()`方法

---

著作权归JavaGuide(javaguide.cn)所有 基于MIT协议 原文链接：[https://javaguide.cn/java/concurrent/java-concurrent-questions-03.html](https://javaguide.cn/java/concurrent/java-concurrent-questions-03.html)

#### 线程池

池化思想，线程池中的线程在完成任务后并不会被销毁，而是等待下一个任务

- 节约了资源的损耗
    
- 提升了线程的响应速度
    
- 提高线程可管理性
    

##### 如何创建线程池

1. 构造函数创建
    
    ThreadPoolExecutor来创建
    
2. Executor的框架工具类Executors来创建（不推荐，要开发的人更明确自己创建线程的意义）
    

##### 注意事项

-  不推荐Excutors 的方式创建线程，推荐构造方法创建，因为这样开发同学能更明确自己要什么样的线程池
-  阿里巴巴开发规范明确要求线程必须通过线程池来提供，不允许手动创建线程

- 线程池构造方法参数（7个）
    
    - 线程池大小 maxPoreSize
        
        - `当任务队列达到了最大值的时候，线程池允许的最大同时运行的线程数`
            
    - 核心线程数
        
        - `当任务队列没达到最大时，线程池允许的最大线程数`
            
    - 当线程池大于核心线程数的大小时，其他的非核心的线程存活的时间
        
        - `垃圾回收会对核心线程和非核心线程一视同仁，直到线程数量等于核心线程数为止`
            
            - 时间单位
                
            - queue，队列存放任务的
                
    - 拒绝策略
        
        - 就是当任务队列已经满了，而且线程池也已经满负荷了，这时候还加任务进来的拒绝策略
            
        - 工厂（创建线程的工厂，一般默认就行）
            

##### 线程池常用的一些阻塞队列有哪些

1. LinkedBlockIngQueue :无界的，所以你创建线程池的时候，永远不会满（实际容量为Integer.maxValue），那就创建一个核心线程数和最大一致的线程池

2. SynchronousQueue：同步的queue，同步的意思就是，我里面没有容量，一来任务我这就是满的，所以会一直需要最大线程数的线程池，可能会创建大量线程，导致OOM
    
3. DelayedWorkQueue： 这个特殊在于排序，按任务进来的延迟排序，延迟越低越先出栈
    

![图解线程池实现原理](https://oss.javaguide.cn/javaguide/%E5%9B%BE%E8%A7%A3%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86.png)

##### 给线程池命名

这个还是蛮重要的，因为普通的默认线程池名字不容易分辨，不带业务名称，看日志很成问题

1. 自己实现工厂方法创建线程
    
2. GUAVA的ThreadFactoryBuilder
    

##### 如何设定线程池大小？

- CPU密集型任务--》N+1（N是CPU核心数）
    
    - 就是密集的计算啊啥的
        
- I/O密集型--》2N
    

##### 实现一个优先级执行线程池

PriorityBlockingQueue

##### 如何动态修改线程池的参数

详见美团技术团队的一些设置

##### 创建一个优先级排序的线程池

1. 首先是考虑PriorityBlockingQueue 作为线程池的任务队列，因为这个是可以排序的，可以视为线程安全的PriorityQueue

2. 然后由于这个任务队列可能是无界的，所以我们可以创建一个类继承PriorityBlockingQueue,重写他的offer方法，判断队列到达指定大小后就返回false，无法继续加进去
    
3. 会可能存在某些任务进去以后，由于优先级的原因一直得不到线程，那么就可以继续修改一下这个子类，当一个任务超时多久就提升一个优先级之类的
    

#### CountDownLatch

允许多个线程阻塞在一个地方

#### JVM(内存模型)

##### CPU缓存模型

类比我们开发时候用到redis是为了解决程序处理速度和关系型数据库速度不匹配的问题

缓存就是为了解决CPU处理速度和内存存储速度不匹配的问题（硬盘的访问速度比较慢）

##### 并发编程的三个重要特性

1. 原子性

2. 可见性
    
3. 有序性（volatile关键字可以禁止指令重排）
    

### javaIO基础知识

笔记栏

1. 什么是IO ?
    
    1. 四个基类
        
        1. InputStream/Reader
            
        2. OutputStream/Writer
            
2. 能做什么？
    
    1. > 读取文件，写文件
        
3. 关于字节流，字符流之类的方法比较多，不详细记录，主要重点感觉是flush方法，由于流是慢慢传输的，有时候，流在写入（`只有写入流有这个方法`）文件的时候，你不能说你觉得读完了就把流关闭了，你要在流关闭之前，flush一下，把流里面的东西清空，然后再关闭
    
    public class test02 {
        public static void main(String[] args) {
            File file = new File("./xx.txt");
            try (FileOutputStream fileOutputStream = new FileOutputStream(file)) {
                fileOutputStream.write(new byte[1024]);
                fileOutputStream.flush();
    
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        }
    }
    
    上面的方法就可以得到1024个null字符的txt文件
    
4. 字节缓冲流
    
    1. 是什么
        
        1. 是一个包装类，创建方法就是new BufferedInputStream(inputStream)
            
    2. 做什么
        
        1. 包装输入流，增强输入流能力的
            
        2. IO操作是非常耗费性能的操作，缓冲流能将数据一下子加载到缓冲区再慢慢操作，从而就能避免频繁的IO操作
            
    3. 什么特征？
        
        1. 文件越大性能更好，缓存区的存在可以节约更多的性能
            

#### IO设计模式总结

##### 装饰器模式

> 不改变对象的前提下，拓展其功能

就是跟跟上面说的‘包装类’有类似的效果，需要的是装饰器需要跟原始类继承相同的抽象类或者实现相同的接口

例如：我们常见的`BufferedInputStream`(字节缓冲输入流)、`DataInputStream` 等等都是`FilterInputStream` 的子类，我们可以通过 `BufferedInputStream`（字节缓冲输入流）来增强 `FileInputStream` 的功能

用子类去增强原始类就既可以调原始类的方法，也可以调字节写的新方法了

##### 适配器模式（Adapter）

> 主要用于适配接口不兼容的情形

举个例子，如何将字符流转换成字节流？

依靠 InputStream这个大类来转换,下面的代码把文件输入流（字符流）转换成了InputStreamReader（字节流），这里是因为InPutstream里面有一个StreamDecoder，专门用来转换的

// InputStreamReader 是适配器，FileInputStream 是被适配的类
InputStreamReader isr = new InputStreamReader(new FileInputStream(fileName), "UTF-8");
// BufferedReader 增强 InputStreamReader 的功能（装饰器模式）
BufferedReader bufferedReader = new BufferedReader(isr);

##### 工厂模式

> 用于创建对象，就比如Files 类的 Files.newInputStream()方法

##### 观察者模式

> 我理解就是，监听，在代码中，你定义了一个类，然后实现Watchable接口，然后去重写他的监听方法，比如监听文件的path类的监听方法里面采用的就是**[守护线程](https://cloud.tencent.com/developer/article/2318134)**？采用轮询的方式去监听操作的文件的变化

#### IO模型详解

常见IO模型

1. BIO
    
    > 同步阻塞IO
    
    在同步阻塞IO模型中，应用发起read之后，会一直阻塞，一直到内核把数据拷贝到用户空间里面，然后直到read返回，才会接触阻塞，这个同步的方式在大量用户大量读取的场景下，比较吃力
    
    ![图源：《深入拆解Tomcat & Jetty》](file://D:/study/img/6a9e704af49b4380bb686f0c96d33b81~tplv-k3u1fbpfcp-watermark.png?lastModify=1703848504)
    
2. NIO
    
    > NonBlocking IO 不阻塞IO
    
    NIO属于多路复用模型，而不是同步非阻塞模型
    
    ![图源：《深入拆解Tomcat & Jetty》](file://D:/study/img/bb174e22dbe04bb79fe3fc126aed0c61~tplv-k3u1fbpfcp-watermark.png?lastModify=1703848504)
    
    ![img](file://D:/study/img/88ff862764024c3b8567367df11df6ab~tplv-k3u1fbpfcp-watermark.png?lastModify=1703848504)
    
    两者的差别就在于在准备数据阶段，`同步不阻塞模型`一直轮询采用read调用去判断数据有没有准备好，好了以后再阻塞的去拷贝数据
    
    而`多路复用模型`则是直接select去查询系统是否准备好了，等准备好了，再阻塞的调用read
    
    > **IO 多路复用模型，通过减少无效的系统调用，减少了对 CPU 资源的消耗。**
    > 
    > Java 中的 NIO ，有一个非常重要的**选择器 ( Selector )** 的概念，也可以被称为 **多路复用器**。通过它，只需要一个线程便可以管理多个客户端连接。当客户端数据到了之后，才会为其服务。
    > 
    > ---
    > 
    > 著作权归JavaGuide(javaguide.cn)所有 基于MIT协议 原文链接：[https://javaguide.cn/java/io/io-model.html](https://javaguide.cn/java/io/io-model.html)
    
    NIO,主要包括下面三个核心组件
    
    - 缓冲区
        
        - NIO的读写数据都是通过缓冲区实现的，读操作的时候，把Channel中的数据写到buffer里面，写操作的时候，把Buffer中的数据写到Channel里面
            
        - NIO在读写数据的时候，都是通过缓冲区进行操作
            
    - Channel（通道）
        
    - Selector（选择器）
        
3. AIO(Asynchronous I/O)
    

AIO就是NIO2 ，属于异步IO模型，也就是完全不阻塞，基于`回调机制实现`，应用读取完会直接返回，等后台数据准备完成，操作系统会通知相对应的程序去进行操作

### JVM

#### java内存区域

![Java 运行时数据区域（JDK1.8 ）](file://D:/study/img/java-runtime-data-areas-jdk1.8.png?lastModify=1703848504)

java 天生是多线程的语言，那么我们按照线程拥有的来划分可以得到

_线程私有的：_

- 程序计数器
    
    - 这个是因为线程总是在CPU分配的时间片上才是活的，有自己的上下文，计数器用来记录线程跑到哪里了，方便切回来的时候继续运行，控制代码的流程
        
- 虚拟机栈
    
    - 这个相当于一块小的虚拟机用来运行
        
        - `栈`是JVM运行时核心，除了本地方法，其他所有的java方法的调用都是通过栈来完成的（和程序计数器配合）
            
    - 每一次方法的调用，都会有一个对应的栈帧被压入栈中，调用结束后会被弹出
        
- 本地方法栈
    
    - 这个和虚拟机栈类似，但是是用来执行本地方法的
        

_线程共享的：_

- 堆
    
    - 堆是jvm管理内存中最大的一块内存，这一块的目的就是为了`存放对象实例`，几乎所有的对象实例和数组都在这里分配内存
        
    - 为什么说几乎，因为JDK1.7开始，就已经开始默认开启逃逸分析，一些方法的的对象的引用没有被返回，或者没有被外面引用，那么这个对象的创建会在栈上
        
    - 字符串常量池在堆上，为什么放堆上？：因为堆是GC管理的最大的区域，放堆中可以及时高效的回收，常量池里存放的直接是字符串对象，而不是引用
        
    - `方法区`
        
        - JDK1.7的时候，堆内存被分为新生代，老生代，永久代，方法区就属于永久代
            
        - 方法区是线程共享的一块逻辑区域，如何实现要看不同的虚拟机的实现
            
        - JDK1.8的时候，方法区（永久代）就被移除了，取而代之的是元空间
            
- 直接内存
    

##### 新生代区、老生代区、永久区（略看一眼）---详细看JVM垃圾回收那块

#### hotSopt虚拟对象

##### 对象创建的步骤 ==5步==

1. 第一步 类加载检查
    
    > 当虚拟机遇到一条新的new指令的时候，首先会去检查能不能从常量池找到对应对象的引用，然后再判断 new的这个类，是否在之前就被加载，解析和初始化过了，如果没有，那要执行相应的类加载过程
    
2. 第二步 分配内存
    
    > 在类加载完成之后，就会进入分配内存的过程中，这个大小在类加载完成后就可以确定，分配方式分为指针碰撞和空闲列表两种，具体选哪种要看堆内存是否规整，而堆内存是否规整要看垃圾回收装置是否带压缩整理功能（规整的收集器：parNew,serial，不规整的：CMS）
    
    - 指针碰撞
        
        - 如果内存是规整的前提下，用过的内存在一边，没用过的内存在另一边，中间会有个指针，只要把指针往没有用过的内存那边移动一个该类内存大小就可以了
            
    - 空闲列表
        
        - 如果内存是不规整的，那么虚拟机会自己维护一个列表，里面记录了哪些内存块是可以用的，分配内存的时候，分配一个足够大的内存块给这个类就可以了
            
    
    _内存分配的并发问题_：
    
    创建对象来说，虚拟机必须要保证创建过程是线程安全的，一般来说，虚拟机会通过两种方式来保证线程安全（因为堆是共享的，所以保证线程安全就很重要）
    
    - TLAB：首先是虚拟机会为每个线程在Eden区域分配一块内存，创建对象的时候，优先在eden区域中创建，只有这一块内存不够的时候，再去堆中，用CAS+失败重试的方式去创建（乐观锁）
        
        - eden区域，堆内存的一块地方，属于新生代
            
        - CAS +失败重试
            
3. 第三步 初始化零值
    
    > 当内存分配完毕以后，虚拟机就会把对象里面的一些内存空间都初始化零值，比如，int 零值就是0 对象类零值就是null，这一部保证了 _java的实例字段（成员变量）不需要初始化就能用_
    > 
    > 但是这不包含对象头
    
4. 第四步 设置对象头
    
    > 对象头就是一些信息，例如这个是哪个类的实例，或者对象的GC分代年龄等信息
    
    **Hotspot 虚拟机的对象头包括两部分信息**，**第一部分用于存储对象自身的运行时数据**（哈希码、GC 分代年龄、锁状态标志等等），**另一部分是类型指针**，即对象指向它的类元数据的指针，虚拟机通过这个指针来确定这个对象是哪个类的实例。
    
    ---
    
    著作权归JavaGuide(javaguide.cn)所有 基于MIT协议 原文链接：[https://javaguide.cn/java/jvm/memory-area.html](https://javaguide.cn/java/jvm/memory-area.html)
    
5. 第五步 init方法
    
    > 实际上，在上一步的时候，虚拟机已经认为一个新的对象被创建好了，但是从程序的视角来看，这才刚刚开始，init就是构造方法之类的一些防范，给对象赋上真正的程序需要的值，变成一个程序可用的对象
    

#### JVM垃圾回收

需要排查各种内存溢出问题，当系统过大，垃圾回收成为系统实现更高并发的瓶颈的时候，我们就要对这些进行必要的监控和调节

##### 堆空间的基本结构

Java 自动内存管理最核心的功能是 **堆** 内存中对象的分配与回收

在jdk7及以前的版本里，堆内存通常分为如下的三块部分

1. 新生代（eden和两个相等大小的survivor区）
    
2. 老生代
    
3. 永久代（JDK8以后被元空间取代，metaspace，元空间用的是直接内存）
    

![堆内存结构](file://D:/study/img/hotspot-heap-structure.png?lastModify=1703848504)

上面的eden区和S0 S1属于新生代

Tenured属于老生代

下面就是永久层/元空间

##### 创建对象时候的minor GC

1. 创建对象有5步（看上面笔记）在分配内存的那一步的时候，一般是会在eden层进行，如果eden层内存空间不足了这时候再创建一个对象，那么垃圾回收就会执行一次minor GC
    

`minorGC`: 就是会把这个eden里面的已经存在对象，找到里面活跃的对象，然后放到S1里面去，同时清理掉eden区和s1区的数据，然后对象年龄加一，一般15岁就会存放到老生代区

> 大部分情况，对象都会首先在 Eden 区域分配，在一次新生代垃圾回收后，如果对象还存活，则会进入 S0 或者 S1，并且对象的年龄还会加 1(Eden 区->Survivor区后对象的初始年龄变为 1)，当它的年龄增加到一定程度（默认为 15 岁，cms是6），就会被晋升到老年代中。对象晋升到老年代的年龄阈值，可以通过参数
> 
> -XX:MaxTenuringThreshold
> 
> 来设置。
> 
> 就是说对象每熬过一次minorGC就会长一岁（这个计算是动态的，根据对象年龄大小和占用动态涨年龄，说的浅显一点就是大概长了一岁）

2. 当进行minorGC以后，发现这个老的对象还是无法存入Survior区域，那么就会通过_分配担保机制_把这个老对象直接放到老年区
    
3. 后面创建的新的对象，如果能放eden区域的话，那么还是会在eden区域进行分配内存的
    

##### 大对象直接进入老年代

1. 进入老年代有两种方式，一种是年龄到了，自动进入老年代

2. 另一种是大对象（需要连续区域的大对象，一般有大数组，字符串之类的），会直接进入老年代

##### HostSpot VM的垃圾回收机制

- 分类：
    
    - 部分收集
        
        - youngGC/minor GC 是用来回收新生代的
            
        - old GC /major GC 用来回收老年代的
            
        - mixed GC 用来回收新生代和老年代的
            
    - 整堆收集
        
        - full GC 收集整个java堆和方法区
            
- 什么时候进行这些回收？
    
    - 首先是yong GC 这个是创建对象的时候，eden区放不下新的对象的内存了，就会触发yong GC，这时候，eden区域存活的对象，会进入survior区，年龄也会增长，如果发现放不进去survior区，那就晋升到老年区去
        
    - 那如果老年区也不够用咋办呢？那新生代的对象就放不进去老年区了，这时候，就不会触发yongGC了，会直接触发fullGC，回收掉整个java堆和方法区（判断对象死亡：就是不再被任何地方引用）
        
- 引用类型介绍（强，软弱无力）
    
    - 强引用
        
        - 大部分我们使用的都是强引用，java回收装置宁愿抛出outofmenory错误也不会回收强引用
            
    - 软引用
        
        - 可有可无的引用，当内存不足 的时候，垃圾回收器就会回收他们
            
    - 弱引用
        
        - 这个比软引用具有更短的生命周期，一旦被垃圾回收器扫到就一定会回收
            
    - 虚引用
        
        - 这个不会影响对象的生命周期，几乎就等于没有任何引用，任何时候都可能会被回收
            
- 垃圾清理算法
    
    1. 标记-清理算法
        
    2. 复制算法
        
    3. 标记-整理算法
        
    4. 分代收集算法
        

#### 类文件结构详解

##### 字节码

字节码就是 .class文件，是jvm可以理解的代码，是不同语言在java虚拟机上的重要桥梁，很多语言，包括 GRoovy，Scala，Kotlin 都是运行在Java虚拟机上的

Class文件通过ClassFile定义

ClassFile {
    u4             magic; //Class 文件的标志
    u2             minor_version;//Class 的小版本号
    u2             major_version;//Class 的大版本号
    u2             constant_pool_count;//常量池的数量
    cp_info        constant_pool[constant_pool_count-1];//常量池
    u2             access_flags;//Class 的访问标记
    u2             this_class;//当前类
    u2             super_class;//父类
    u2             interfaces_count;//接口数量
    u2             interfaces[interfaces_count];//一个类可以实现多个接口
    u2             fields_count;//字段数量
    field_info     fields[fields_count];//一个类可以有多个字段
    u2             methods_count;//方法数量
    method_info    methods[methods_count];//一个类可以有个多个方法
    u2             attributes_count;//此类的属性表中的属性数
    attribute_info attributes[attributes_count];//属性表集合
}

在 Java 中，JVM 可以理解的代码就叫做`字节码`（即扩展名为 `.class` 的文件），它不面向任何特定的处理器，只面向虚拟机。Java 语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。所以 Java 程序运行时比较高效，而且，由于字节码并不针对一种特定的机器，因此，Java 程序无须重新编译便可在多种不同操作系统的计算机上运行。

---

著作权归JavaGuide(javaguide.cn)所有 基于MIT协议 原文链接：[https://javaguide.cn/java/jvm/class-file-structure.html](https://javaguide.cn/java/jvm/class-file-structure.html)

#### 类加载过程详解

##### 类的生命周期

类从被加载一直到被卸载，可以简单概括成7个阶段

1. 加载
    
2. 验证
    
3. 准备
    
4. 解析
    
5. 初始化
    
6. 使用
    
7. 卸载
    

其中，验证-准备-解析可以统称为连接（Link）

![一个类的完整生命周期](file://D:/study/img/lifecycle-of-a-class.png?lastModify=1703848504)

##### 类加载过程

类加载过程，就是如上图所示的，加载-连接-初始化

连接就是框里的三步（验证-准备-解析）

- 加载
    
    加载这一步主要是通过类加载器（ClassLoader）去确定的，用哪个类加载器是由 `双亲委派模型确定的`（不过我们也能打破双亲委派模型）
    
    每个java类都有指向他的ClassLoader，不过数组类除外，他们是JVM需要的时候自动创建的，数组类通过 getClassLoader获取他的ClassLoader 得到的和他内部元素的ClassLoader是一样的
    
    类加载的过程主要做三件事
    
    1. 通过全类名获取定义此类的二进制字节流。（通过名字获取二进制字节流）
        
    2. 将字节流所代表的静态存储结构转换为方法区的运行时数据结构。（然后把字节流的静态格式，转换成方法去动态的数据结构）
        
    3. 在内存中生成一个代表该类的 `Class` 对象，作为方法区这些数据的访问入口。（然后生成一个Class入口）
        
    
- 验证
    
    保证Class文件的字节流信息不会损害虚拟机
    
- 准备
    
    分配内存和准备类变量初始值
    
- 解析
    
    **解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程**
    
- 初始化
    
    **初始化阶段是执行初始化方法的过程，是类加载的最后一步，这一步 JVM 才开始真正执行类中定义的 Java 程序代码(字节码)。**
    

#### 类加载器详解

##### 类加载器介绍

首先

1. 每个java类（除了数组）都有一个引用指向加载他的Classloader

2. 类加载器负责实现类的加载过程
    
3. 数组类不是通过ClassLoader创建的，是JVM直接生成的
    

简而言之，就是类加载器作用就是 把class文件转换到虚拟机里面生成一个class对象

##### 类加载器加载规则

- JVM中类是动态加载的，JVM启动的时候不会加载所有的类，而是根据需要动态的加载类，对内存更加友好
- 对JVM来说，相同的名称的类只会加载一次，加载过的类会存在ClassLoader里面，需要的时候直接返回

##### 类加载器总结

jvm中内置了三个重要的Classloader

1. BootstrapClassLoader
    
    顶层加载器，通过getParent方法找这个加载器是null，因为这是C写的，java中没有对应的类
    
2. ExtensionClassLoader
    
3. AppClassLoader
    

##### 双亲委派模型

java类是由类加载器加载的，但是具体是哪个类加载器，则需要通过双亲委派模型了

- ClassLoader使用委派机制
    
- 双亲委派模型要求除了最顶层的ClassLoader之外，每个ClassLoader都要有自己的父ClassLoader
    
- 在ClassLoader亲自去加载资源的之前，会委派父类Classloader去加载（比如加载的时候要判断这个类之前有没有加载过，就要一直找到最顶上的父类去查，然后查出来没有，我就依次向下，开始加载你这个类）
    

![类加载器层次关系图](file://D:/study/img/class-loader-parents-delegation-model.png?lastModify=1703848504)

> 这里要注意的是，类加载器之间的父子关系不是以继承来实现的，而是_组合_ 如下
> 
> public abstract class ClassLoader {
> ...
> // 组合
> private final ClassLoader parent;
> protected ClassLoader(ClassLoader parent) {
>     this(checkCreateClassLoader(), parent);
> }
> ...
> }
> 
> 在面向对象编程中，有一条非常经典的设计原则：**组合优于继承，多用组合少用继承**

🌈 拓展一下：

**JVM 判定两个 Java 类是否相同的具体规则**：JVM 不仅要看类的全名是否相同，还要看加载此类的类加载器是否一样。只有两者都相同的情况，才认为两个类是相同的。即使两个类来源于同一个 `Class` 文件，被同一个虚拟机加载，只要加载它们的类加载器不同，那这两个类就必定不相同。

---

著作权归JavaGuide(javaguide.cn)所有 基于MIT协议 原文链接：[https://javaguide.cn/java/jvm/classloader.html](https://javaguide.cn/java/jvm/classloader.html)

###### 双亲委派模型的好处

1. 可以保证java的稳定运行，避免类的重复加载，

2. 二来，（java判断类相同的标准加上了加载器）也保证了java核心API不会被篡改
    

###### 打破双亲委派

首先是如何自定义一个类加载器

1. 写一个类，继承ClassLoader类，重写里面的findClass（String name），（比如，我们想加载一个D盘某个目录下的所有的类，那我们就findClass写成从D盘某个路径下的 name.class把他转换成class类这种）

2. 但是如果想要打破双亲委派模型，那么就要重写，loadClass()方法了，就可以自定义怎么loadClass了，不需要一层层往上找了（比如tomcat服务器，为了优先加载web目录下的类，再加载其他目录下的类，就自己定义实现了webAppClassLoader这个类）
    

## 面试题错误部分整理

### JMM内存模型（重看）和JVM不同的

主内存（Main Memory）：主内存是所有线程共享的内存区域，用于存储共享变量。当一个线程需要读取或修改共享变量时，它首先会从主内存中获取该变量的值，然后在自己的工作内存中进行操作，最后将结果写回主内存。

工作内存（Working Memory）：工作内存是每个线程私有的内存区域，用于存储主内存中共享变量的副本。线程对共享变量的所有操作都发生在工作内存中，然后将结果同步回主内存。这样可以提高程序的执行效率，避免每次操作共享变量都需要直接访问主内存。

内存间交互操作：JMM定义了八种操作，用于描述生成的class字节码指令对内存的读写要求。这些操作包括：lock（锁定）、unlock（解锁）、read（读取）、load（载入）、use（使用）、assign（赋值）、store（存储）和write（写入）。

原子性：原子性是指一个操作要么全部完成，要么全部不完成。JMM保证了基本类型的读取和写入操作的原子性，但对于复合操作（例如自增运算），JMM并没有保证其原子性。为了解决这个问题，可以使用synchronized关键字或显式锁（如ReentrantLock）来确保复合操作的原子性。

可见性：可见性是指当一个线程修改了共享变量的值，其他线程能够立即看到修改后的值。JMM通过volatile关键字和synchronized关键字来保证可见性。当一个共享变量被volatile修饰时，其他线程能够立即看到该变量的最新值；当一个共享变量在synchronized块中被修改时，其他线程在退出synchronized块后能够立即看到修改后的值。

有序性：有序性是指程序执行的顺序应该按照代码的先后顺序进行。然而，由于处理器和编译器的优化，代码的执行顺序可能会被重新排序。JMM通过happens-before规则来保证程序的有序性。如果一个操作happens-before另一个操作，那么第一个操作的结果对于第二个操作是可见的，而且第一个操作不会因为线程的执行而重排序。

总之，JMM内存模型定义了一组规则，用于确保多线程程序在各个虚拟机上运行时能够得到一致的结果。这些规则涉及原子性、可见性和有序性等方面，可以帮助开发者编写正确、可靠的多线程程序。

### **Java**  **线程有几种状态，分别是啥**

新建（New）：线程刚被创建，但是尚未启动。还没调用start()方法。

可运行（Runnable）：线程可以在Java虚拟机中运行的状态，可能正在运行自己代码，也可能没有，这取决于操作系统处理器。包括运行（running）和就绪（ready）两种状态。

阻塞（Blocked）：线程进入等待状态，线程因为某种原因，放弃了CPU的使用权。阻塞的情况分三种： A. 等待阻塞：运行的线程执行了wait()，JVM会把当前线程放入等待队列。 B. 同步阻塞：运行的线程在获取对象的同步锁时，如果该同步锁被其他线程占用了，JVM会把当前线程放入锁池中。 C. 其他阻塞：运行的线程执行sleep()、join()或者发出IO请求时，JVM会把当前线程设置为阻塞状态，当sleep()执行完，join()线程终止，IO处理完毕线程再次恢复。

等待（Waiting）：线程拥有了某个锁之后，调用了wait()方法，等待其他线程/锁拥有者调用notify/notifyAll以便该线程可以继续下一步操作。线程调用join()方法时，也会进入等待状态，等待被join的线程执行结束。

超时等待（Timed Waiting）：同等待状态，有几个方法有超时参数，调用他们将进入Timed Waiting状态。这一状态将一直保持到超时期满或者接收到唤醒通知。带有超时参数的常用方法有Thread.sleep、Object.wait。

终止（Terminated）：线程执行完毕，因为run()方法正常退出或者因为没有捕获的异常终止了run()方法而死亡。终止的线程不可再次恢复。

### AQS的补充

AQS（AbstractQueuedSynchronizer）是Java并发包java.util.concurrent中的一个抽象类，它提供了构建锁和其他同步组件的一个通用框架。AQS实现了基于队列的阻塞和唤醒机制，用于构建各种同步组件，如ReentrantLock、Semaphore、CountDownLatch等。

AQS内部维护了一个整数状态（state）和一个双向队列（CLH队列），用于实现同步组件的阻塞和唤醒操作。它定义了三个核心方法：tryAcquire、tryRelease和isHeldExclusively。这些方法需要子类根据具体同步组件的实现来提供。

