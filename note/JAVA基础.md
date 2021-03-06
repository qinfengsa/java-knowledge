## 基本概念

### JVM JDK 和 JRE

JVM：Java虚拟机（JVM）是运行 Java 字节码的虚拟机

JRE：JRE 是 Java运行时环境。运行已编译的 Java 程序（class）所需的所有内容的集合，包括 Java虚拟机（JVM），Java类库，Java命令和其他的一些基础构件。但是，它不能用于创建新程序。

JDK：JDK是Java Development Kit，它是功能齐全的Java SDK。它拥有JRE所拥有的一切，还有编译器（javac）和工具（如javadoc和jdb）。它能够创建和编译程序。



## 数据类型

**基本数据类型**

布尔boolean (1个字节)   true和false

byte（1个字节），-128~127

char（2个字节），最小值是’\u0000’（即为0）；最大值是’\uffff’（即为65,535）；

short（2个字节）， -2^15 ~ 2^15 - 1

int（4个字节）, -2^31 ~ 2^31 - 1

long（8个字节），-2^63 ~ 2^63 - 1

float（4个字节）， -3.4x10^-38～3.4x10^38

double（8个字节）， -1.7x10^-308～1.7x10^308

**包装类型**

基本类型都有对应的包装类型，基本类型与对应的包装类型之间的复杂操作使用自动装箱和拆箱完成

~~~java
Integer x = 2; // 装箱 等价于 Integer.valueOf(2)
int y = x; // 拆箱 x.intValue()
~~~

**缓存池**

基本类型中常用的部分数据会被放入缓存池中，引用类型装箱时会自动取缓存池的数据； Integer 缓存池的大小默认为 -128~127

~~~java
Integer x = new Integer(123); // 新建Integer对象
Integer y = new Integer(123);
System.out.println(x == y); // false
Integer z = 123; // 装箱 等价于 Integer.valueOf(123) 会使用缓存池中的对象 
Integer k = Integer.valueOf(123);
System.out.println(z == k); // true 
~~~

**四舍五入**

Java取整函数有三个，四舍五入、向下取整、向上取整

~~~java
double num =  -1.6;
log.debug("a:{}",Math.round(num)); // 四舍五入 a:2
log.debug("b:{}",Math.floor(num)); // 向下取整 b:1.0
log.debug("c:{}",Math.ceil(num));  // 向上取整 c:2.0  
~~~

四舍五入：舍去位的数值等于5时，会向较大的数取整

~~~java
double num = 1.5;
log.debug("a:{}",Math.round(num));  // 四舍五入 a:2 
double num2 = -1.5;
log.debug("b:{}",Math.round(num2)); // 四舍五入 b:-1
~~~



## 修饰符

### **访问控制修饰符**

Java中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。Java支持4种不同的访问权限。

| 修饰符    | 访问级别                             | 同类 | 同包 | 子类 | 不同的包 |
| --------- | ------------------------------------ | :--: | :--: | :--: | :------: |
| public    | 公有，对所有类可见                   |  √   |  √   |  √   |    √     |
| protected | 受保护，对同一包内的类和所有子类可见 |  √   |  √   |  √   |    ×     |
| default   | 在同一包内可见，不使用任何修饰符     |  √   |  √   |  ×   |    ×     |
| private   | 私有，在同一类内可见                 |  √   |  ×   |  ×   |    ×     |

**访问控制和继承**

请注意以下方法继承的规则：

- 父类中声明为public的方法在子类中也必须为public。
- 父类中声明为protected的方法在子类中要么声明为protected，要么声明为public。不能声明为private。
- 父类中声明为private的方法，不能够被继承。

**非访问修饰符**

为了实现一些其他的功能，Java也提供了许多非访问修饰符。





**abstract**修饰符，用来创建抽象类和抽象方法。

**synchronized**和**volatile**修饰符，多线程相关。

**strictfp**：即 **strict float point **(精确浮点) 可应用于类、接口或方法。使用 strictfp 关键字声明一个方法时，该方法中所有的float和double表达式都严格遵守FP-strict的限制，符合IEEE-754规范。当对一个类或接口使用 strictfp 关键字时，该类中的所有代码，包括嵌套类型中的初始设定值和代码，都将严格地进行计算。严格约束意味着所有表达式的结果都必须是 IEEE 754 算法对操作数预期的结果，以单精度和双精度格式表示。如果你想让你的浮点运算更加精确，而且不会因为不同的硬件平台所执行的结果不一致的话，可以用关键字strictfp. 

### static

**静态变量**

static修饰的变量我们称之为静态变量，没有用static修饰的变量称之为实例变量，他们两者的区别是：

静态变量是随着类加载时被完成初始化的，它在内存中仅有一个，且JVM也只会为它分配一次内存，同时类所有的实例都共享静态变量，可以直接通过类名来访问它。

但是实例变量则不同，它是伴随着实例的，每创建一个实例就会产生一个实例变量，它与该实例同生共死。

**静态方法**

static修饰的方法我们称之为静态方法，通过类名对其进行直接调用。由于静态方法在类加载的时候就存在了，不依赖于任何实例，所以static方法必须实现，不能是抽象方法abstract。

**静态代码块**

被static修饰的代码块，称之为静态代码块，静态代码块会随着类的加载一起执行

**静态内部类**

非静态内部类依赖于外部类的实例，而静态内部类不需要。

**初始化顺序**

1. 父类静态变量，静态代码块
2. 子类静态变量，静态代码块
3. 父类的实例变量，普通代码块
4. 父类的构造函数
5. 子类的实例变量，普通代码块
6. 子类的构造函数

### **final** 

1. 修饰变量：声明变量为常量，被初始化后不能被改变

   对于基本类型，final 使数值不变；

   对于引用类型，final 使引用不变，也就是不能引用其它对象，但是被引用的对象本身是可以修改的

2. 修饰方法：修饰的方法不能被继承类重写

3. 修饰类：修饰的类不能够被继承



## 运算符

**位运算**

| 操作符 | 描述                                                         |
| :----- | ------------------------------------------------------------ |
| &      | 位与操作：相同位的两个数字都为1，则为1；若有一个不为1，则为0。 |
| \|     | 位或操作：相同位只要一个为1即为1。                           |
| ^      | 异或操作：相同位不同则为1，相同则为0。                       |
| ~      | 取反：0和1全部取反                                           |
| <<     | 左移：二进制左移一位，相当于x2，存在越界的可能               |
| >>     | 右移：用符号位填充高位（负数高位：1）                        |
| >>>    | 右移补零：用0填充高位                                        |

## String

String 被声明为ﬁnal，因此它不可被继承。String内部实现是一个final修饰的char数组，JDK9修改为byte数组；

**不可变的好处**

可以缓存hash值：String 的hash值经常被使用，例如String 用做HashMap 的key。不可变的特性可以使得hash 值也不可变

字符串常量池：可以把常用的字符串缓存到String Pool 

安全性：final修饰的对象是线程安全的

**字符串常量池**

字符串常量池（String Pool）保存着所有字符串**字面量**（literal strings），这些字面量在编译时期就确定。不仅如此，还可以使用 String 的 intern() 方法在运行过程中将字符串添加到 String Pool 中。

当一个字符串调用 intern() 方法时，如果 String Pool 中已经存在一个字符串和该字符串值相等（使用 equals() 方法进行确定），那么就会返回 String Pool 中字符串的引用；否则，就会在 String Pool 中添加一个新的字符串，并返回这个新字符串的引用。



**String，StringBuffer，StringBuilder的区别。**

String，StringBuffer，StringBuilder都是一个char数组，String的char数组是final不可变得，StringBuffer，StringBuilder继承了AbstractStringBuilder，char数组是动态扩容的，StringBuffer中方法都加了synchronized修饰，所以线程安全，但是性能会有影响，StringBuilder线程不安全，但是性能更好

String：适用于少量的字符串操作的情况

StringBuilder：适用于单线程下在字符缓冲区进行大量操作的情况

StringBuffer：适用多线程下在字符缓冲区进行大量操作的情况



## Object常用方法

**equals()**：判断两个对象是否等价；我们可以重写equals()方法，但是有一些注意事项；**JDK中说明了实现equals()方法应该遵守的约定**：

1. 自反性：x.equals(x)必须返回true。
2. 对称性：x.equals(y)与y.equals(x)的返回值必须相等。
3. 传递性：x.equals(y)为true，y.equals(z)也为true，那么x.equals(z)必须为true。
4. 一致性：如果对象x和y在equals()中使用的信息都没有改变，那么x.equals(y)值始终不变。
5. 非null：x不是null，y为null，则x.equals(y)必须为false。



**hashCode()**：获取对象的散列值

对于hashCode，我们应该遵循如下规则：

1. 在一个应用程序执行期间，如果一个对象的equals方法做比较所用到的信息没有被修改的话，则对该对象调用hashCode方法多次，它必须始终如一地返回同一个整数。
2. 如果两个对象根据equals(Object o)方法是相等的，则调用这两个对象中任一对象的hashCode方法必须产生相同的整数结果。
3. 如果两个对象根据equals(Object o)方法是不相等的，则调用这两个对象中任一个对象的hashCode方法，不要求产生不同的整数结果。但如果能不同，则可能提高散列表的性能。

**hashCode（）与equals（）的相关规定**

1. 如果两个对象相等，则hashcode一定也是相同的
2. 两个对象相等,对两个对象分别调用equals方法都返回true
3. 两个对象有相同的hashcode值，它们也不一定是相等的
4. equals 方法被覆盖过，则 hashCode 方法也必须被覆盖
5. hashCode() 的默认行为是对堆上的对象产生独特值。如果没有重写 hashCode()，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）

## 反射

我们编译Java文件时，会生成一个class文件，class文件保存了class的信息， JVM在加载类时，会将字节流中类的静态存储结构转为方法区中的运行时数据结构；在内存中生成一个代表这个类的Class对象，作为方法区访问这个类的入口，我们就可以通过类的全名获取到Class对象，然后通过Class对象，访问内部所有的字段和方法；这种动态获取对象信息以及动态调用对象方法的功能称为Java语言的反射机制。

实现方式有三种

~~~java
// 获取对象的Class
ChildLoadTest test = new ChildLoadTest();
Class<?> clazz = test.getClass();
// 导入类加载class
Class clazz = ChildLoadTest.class;
// 通过类路径加载Class
try {
	Class clazz = Class.forName("com.qinfengsa.base.ChildLoadTest");
} catch (ClassNotFoundException e) {
    e.printStackTrace();
}
~~~

反射创建类实例

~~~java
// 第一种：通过 Class 对象的 newInstance() 方法。
Class clazz = ChildLoadTest.class;
ChildLoadTest test = (ChildLoadTest) clazz.newInstance(); 
// 第二种：通过 Constructor 对象的 newInstance() 方法
Class clazz = ChildLoadTest.class;
Constructor constructor = clazz.getConstructor();
ChildLoadTest test = (ChildLoadTest)constructor.newInstance();
// 通过 Constructor 对象创建类对象可以选择特定构造方法，而通过 Class 对象则只能使用默认的无参数构造
// 方法。下面的代码就调用了一个有参数的构造方法进行了类对象的初始化。
Class clazz = ChildLoadTest.class;
Constructor constructor = clazz.getConstructor(String.class);
ChildLoadTest test = (ChildLoadTest)constructor.newInstance("a");
~~~

**反射API**

Class 类：反射的核心类，可以获取类的属性，方法等信息。

Field 类：Java.lang.reflec 包中的类，表示类的成员变量，可以用来获取和设置类之中的属性值。

Method 类：Java.lang.reflec 包中的类，表示类的方法，它可以用来获取类中的方法信息或者执行方法。

Constructor 类：Java.lang.reflec 包中的类，表示类的构造方法。

**反射的优点** : 

- **可扩展性** ：应用程序可以利用全限定名创建可扩展对象的实例，来使用来自外部的用户自定义类。
- **类浏览器和可视化开发环境** ：一个类浏览器需要可以枚举类的成员。可视化开发环境（如 IDE）可以从利用反射中可用的类型信息中受益，以帮助程序员编写正确的代码。
- **调试器和测试工具** ： 调试器需要能够检查一个类里的私有成员。测试工具可以利用反射来自动地调用类里定义的可被发现的 API 定义，以确保一组测试中有较高的代码覆盖率。

**反射的缺点** : 

尽管反射非常强大，但也不能滥用。如果一个功能可以不用反射完成，那么最好就不用。 

- **性能开销** ：反射涉及了动态类型的解析，所以 JVM 无法对这些代码进行优化。因此，反射操作的效率要比那些非反射操作低得多。我们应该避免在经常被执行的代码或对性能要求很高的程序中使用反射。
- **安全限制** ：使用反射技术要求程序必须在一个没有安全限制的环境中运行。
- **内部暴露** ：由于反射允许代码执行一些在正常情况下不被允许的操作（比如访问私有的属性和方法），所以使用反射可能会导致意料之外的副作用，这可能导致代码功能失调并破坏可移植性。反射代码破坏了抽象性，因此当平台发生改变的时候，代码的行为就有可能也随着变化。

## 异常

**Throwable**

Throwable是Java异常的顶级类，所有的异常都继承于这个类。

**Error**

Error是非程序异常，即程序不能捕获的异常，一般是编译或者系统性的错误，如OutOfMemorry内存溢出异常等。

**Exception**

Exception是程序异常类，由程序内部产生。Exception又分为运行时异常、非运行时异常。

**运行时异常**

运行时异常的特点是Java编译器不会检查它

**非运行时异常**

非运行时异常是程序必须处理的异常，编译器会提示必须处理这类异常（捕获或者抛出），否则无法编译通过；如常见的IOException、ClassNotFoundException等。

## 泛型

泛型是Java SE 1.5的新特性，泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。这种参数类型可以用在类、接口和方法的创建中，分别称为泛型类、泛型接口、泛型方法。

在Java SE 1.5之前，没有泛型的情况下，通过对类型Object的引用来实现参数的“任意化”，“任意化”带来的缺点是要做显式的强制类型转换，而这种转换是要求开发者对实际参数类型可以预知的情况下进行的。对于强制类型转换错误的情况，编译器可能不提示错误，在运行的时候才出现异常，这是一个安全隐患。

泛型的好处是在编译的时候检查类型安全，并且所有的强制转换]都是自动和隐式的，以提高代码的重用率。

**类型擦除**：Java 中的泛型基本上都是在编译器这个层次来实现的。在生成的Java 字节代码中是不包含泛型中的类型信息的。使用泛型的时候加上的类型参数，会被编译器在编译的时候去掉。这个过程就称为类型擦除。如在代码中定义的List<Object>和List<String>等类型，在编译之后都会变成List。JVM 看到的只是List，而由泛型附加的类型信息对JVM 来说是不可见的。

## 注解

### 元注解

**@Target** ：指定注解使用的目标范围（类、方法、字段等），其参考值见类的定义：java.lang.annotation.ElementType

- ElementType.CONSTRUCTOR: 用于描述构造器
- ElementType.FIELD: 成员变量、对象、属性（包括enum实例）
- ElementType.LOCAL_VARIABLE: 用于描述局部变量
- ElementType.METHOD: 用于描述方法
- ElementType.PACKAGE: 用于描述包
- ElementType.PARAMETER: 用于描述参数
- ElementType.TYPE: 用于描述类、接口(包括注解类型) 或enum声明
- ElementType.TYPE_PARAMETER：对参数类型的注解
- ElementType.TYPE_USE： 对类型的注解

**@Documented**：指定被标注的注解会包含在javadoc中。

**@Retention**：指定注解的生命周期（源码、class文件、运行时），其参考值见类的定义：java.lang.annotation.RetentionPolicy

- RetentionPolicy.SOURCE : 在编译阶段丢弃。这些注解在编译结束之后就不再有任何意义，所以它们不会写入字节码。@Override, @SuppressWarnings都属于这类注解。
- RetentionPolicy.CLASS : 在类加载的时候丢弃。在字节码文件的处理中有用。注解默认使用这种方式
- RetentionPolicy.RUNTIME : 始终不会丢弃，运行期也保留该注解，因此可以使用反射机制读取该注解的信息。我们自定义的注解通常使用这种方式。

**@Inherited**：指定子类可以继承父类的注解，只能是类上的注解，方法和字段的注解不能继承。即如果父类上的注解是@Inherited修饰的就能被子类继承。

**@Native**：指定字段是一个常量，其值引用native code。

**@Repeatable**：注解上可以使用重复注解，即可以在一个地方可以重复使用同一个注解，像spring中的包扫描注解就使用了这个。

## 序列化

序列化 (Serialization)是将对象的状态信息转换为可以存储或传输的形式的过程。在序列化期间，对象将其当前状态写入到临时或持久性存储区。之后可以通过从存储区中读取或反序列化对象的状态，重新创建该对象。 

可以借助commons-lang3工具包里面的类实现对象的序列化及反序列化

~~~java
@Slf4j
public class SerialTest {
    @Test // 序列化方法 
    public void test1() { 
        VipUser user = new VipUser();
        user.setVipNo("123456");
        user.setAddr("testAddr");
        user.setAge(18);
        user.setName("qin");
        byte [] bytes = SerializationUtils.serialize(user);
        VipUser user1 = SerializationUtils.deserialize(bytes);
        log.debug("user1:{}",user1);
        log.debug("age:{}",user1.getAge()); 
    }
}
~~~

- 序列化对象必须实现序列化接口。
- 序列化对象里面的属性是对象的话也要实现序列化接口。
- 类的对象序列化后，类的序列化ID不能轻易修改，不然反序列化会失败。
- 类的对象序列化后，类的属性有增加或者删除不会影响序列化，只是值会丢失。
- 如果父类序列化了，子类会继承父类的序列化，子类无需添加序列化接口。
- 如果父类没有序列化，子类序列化了，子类中的属性能正常序列化，但父类的属性会丢失，不能序列化。
- 用Java序列化的二进制字节数据只能由Java反序列化，不能被其他语言反序列化。如果要进行前后端或者不同语言之间的交互一般需要将对象转变成Json/Xml通用格式的数据，再恢复原来的对象。
- 如果某个字段不想序列化，在该字段前加上transient关键字即可。

### Java序列化的缺点

1. **无法跨语言**：Java序列化技术是Java内部的私有协议，其他语言不支持

2. **序列化后的码流太大**：序列化之后的码流大小对网络传输有很大的影响，会影响系统的吞吐量

3. **序列化性能太低**：

   * Java序列化会携带类本身的信息；
   * 如果父类也实现了序列化，则会携带父类相关的信息；
   * 如果引用了某些对象，则引用对象也会被序列化。

   这些原因导致Java序列化相比其他序列化工具，性能差很多

## 面向对象

面向对象编程三大特性: 封装 继承 多态

**封装**：把一个对象的属性私有化，同时提供一些可以被外界访问的属性的方法；封装就是隐藏一切可以隐藏的属性，只向外界提供最简单的接口；

封装的优点：

1. 良好的封装能够减少耦合；
2. 类内部的结构可以自由修改；
3. 可以对成员进行更精确的控制；
4. 隐藏信息，实现细节

**继承**：使用已有的类通过继承关系创建新的类，实现代码的复用，新的类可以增加新的属性和方法；

使用继承时需要记住三句话：

- 子类拥有父类非private的属性和方法。
- 子类可以拥有自己属性和方法，即子类可以对父类进行扩展。
- 子类可以用自己的方式实现父类的方法。重写

继承存在如下缺陷：

1. 父类变，子类就必须变
2. 继承破坏了封装，对于父类而言，它的实现细节对与子类来说都是透明的
3. 继承是一种强耦合关系。

**多态**：所谓多态就是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定，即一个引用变量到底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。

在Java中有两种形式可以实现多态：继承（多个子类对同一方法的重写）和接口（实现接口并覆盖接口中同一方法）。

## 接口和抽象类

抽象类和接口的比较

- 默认实现：1.8之前接口方法是没有默认实现的，1.8之后通default关键字可以添加默认实现
- 构造器：抽象类可以有构造方法，接口没有构造方法
- 与正常Java类的区别：抽象类除了不能实例化之外，和普通的Java类没有什么区别；
- 访问修饰符：抽象方法可以有public、protected和default这些修饰符；接口方法默认修饰符是public，不能使用其它修饰符。
- 多继承：抽象方法可以继承一个类和实现多个接口；接口只可以继承一个或多个其它接口

一个类不能继承多个类，如果类A同时继承类B和类C，B和C中有相同的方法a();那类A调用super.a()；就无法判断调用哪个；接口可以继承多个接口，如果有相同的方法，用一个实现类实现相同的方法即可



##  重载和重写

- **重载**:  发生在同一个类中，方法名必须相同，参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同，发生在编译时。 　　
- **重写**:  发生在父子类中，方法名、参数列表必须相同，返回值范围小于等于父类，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类；如果父类方法访问修饰符为 private 则子类就不能重写该方法。

## Java中类与类之间的关系

类之间的关系大体上存在五种—继承(实现)、依赖、关联、聚合、组合。

### 继承(实现)

继承：子类通过extends继承父类，然后可以在父类的基础上扩展新的功能，满足is-a的关系

实现：类通过implements实现interface接口



### 依赖

一个类A中的方法使用到了另一个类B，满足use-a的关系

使用关系是具有偶然性的、临时性的、非常弱的，但是B类的变化会影响到A

~~~java
public class Pen {
    public void write(){
        System.out.println("use pen to write");
    }
}
public class Me {
    public void write(Pen pen){//这里，pen作为Me类方法的参数
        pen.write();// pen类的改变，有可能会影响到Me类的结果
    }
} 
// 一般而言，依赖关系在Java中体现为局域变量、方法的形参，或者对静态方法的调用。
~~~

### 关联

关联体现的是两个类、或者类与接口之间语义级别的一种强依赖关系。

这种关系比依赖更强、不存在依赖关系的偶然性、关系也不是临时性的，一般是长期性的，而且双方的关系一般是平等的、关联可以是单向、双向的。

~~~java
public class You {
    private Pen pen; // 让pen成为you的类属性 
    public You(Pen p){
        this.pen = p;
    }
    public void write(){
        pen.write();
    }
} 
~~~

在Java中，关联关系一般使用成员变量来实现。

### 聚合

聚合是关联关系的一种特例，他体现的是整体与部分、拥有的关系，即has-a的关系

~~~java
public class Family {
    private List<Child> children; //一个家庭里有许多孩子
}
~~~

不同于关联关系的平等地位，聚合关系中两个类的地位是不平等。

### 组合

组合也是关联关系的一种特例，他体现的是一种contains-a的关系，这种关系比聚合更强，也称为强聚合；

~~~java
public class Man {
    private Eye eye = new Eye();  //一个人有鼻子有眼睛
    private Nose nose = new Nose(); 
}
~~~

组合关系中，两个类的地位也是不平等的

**聚合和组合的区别**

**聚合：雁群和大雁；组合：大雁和翅膀**

聚合：整体和部分是可以分割的，可以拥有不同的生命周期

组合：整体和部分不可分割，拥有相同的生命周期

## 参考

* [ Java学习+面试指南 ](https://github.com/Snailclimb/JavaGuide)
* [Java 提高篇](http://cmsblogs.com/?p=3740)
* [CS-Notes  技术面试必备 ](https://github.com/CyC2018/CS-Notes)

