# Java基础常见面试题总结--6h

## 基础概念与常识

#### 1.JVM JDK JRE

JVM：java虚拟机。运行java字节码（.class）的虚拟机，针对不同的系统有不同的实现；

JDK：java开发工具包。是功能齐全的java sdk，除了有jre，还有编译器javac，其他工具javadoc和jdb；

JRE：java运行时环境。是运行已编译java程序所需的所有内容集合，只需要运行java程序的话，安装jre即可，如果需要进行java编程，则需要安装jdk。

#### 2.字节码

扩展名为.class的文件叫做字节码，不面向任何特定的处理器，只面向虚拟机。

#### 3.java程序从源码到运行的过程

![Java程序转变为机器代码的过程](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/java-code-to-machine-code.png)

#### 4.为什么说java是编译与解释共存的语言

首先理解什么是编译语言和解释语言？

编译语言：一次性将源代码翻译成机器码执行，编译语言的执行速度比较快，开发速度比较慢；C+

+/Go

解释语言：将源代码通过解释器一句一句解释成机器码再执行，解释语言开发效率高，但执行速度慢。Python/JavaScript

![编译型语言和解释型语言](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/compiled-and-interpreted-languages.png)

Java语言既具有编译型语言的特征，也具有解释型语言的特征。java源码通过javac编译成字节码.class文件，再通过解释器解释字节码文件形成机器码。速度相对较慢。

#### 5.openjdk与oraclejdk

openjdk：开源

oraclejdk：部分开源

#### 6.java与c++的区别

java不提供指针直接访问内存；

java类单继承；

java有自动内存管理和垃圾回收机制；

java只有方法重载，没有操作符重载；



## 基本语法

#### 1.注释的方式

单行注释、多行注释、文档注释

#### 2.标识符和关键字

标识符：类名、方法名、变量名等；

关键字：赋予特殊含义的标识符，如private、public、int、new，不能当做变量使用；

#### 3.continue、break、return

continue：跳出当前循环，继续下一次循环；

break：结束当前循环；

return：结束当前方法；

#### 4.成员变量与局部变量

成员变量属于类，局部变量属于方法或代码块。

#### 5.字符常量与字符串常量

字符常量：‘a’; 相当于整型值，占用2个字节；

字符串常量：“abc”；代表一个地址值，占用若干个字节；

#### 6.静态方法为什么不能调用非静态成员

静态方法属于类，类加载时就分配内存，可以通过类名访问；

非静态成员属于方法，对象实例化之后才存在，通过类的实例对象访问。

在类的非静态成员不存在的时候，静态成员就已经存在，所以无法调用。

#### 7.重载与重写

方法重载，表现为方法名相同，参数类型个数顺序不同；

重写/覆写，子类对父类方法实现过程进行重写；

## 基本数据类型

#### 1.java中有多少基本数据类型

java中有8种基本数据类型，分为：

6种数字类型：byte short int（4字节） long float double 

1种字符类型：char

1种布尔类型：boolean

#### 2.基本数据类型与包装类型

基本数据类型存放在java虚拟机栈中的局部变量表中，包装类型存放在java虚拟机的堆中；

#### 3.包装类型的缓存机制

包装类对象之间值的比较，全部使用equals方法。

Integer var = 120；在[-128,127]之间，会复用已有对象，Integer对象是同一个对象，可以直接使用==判断，但是这个区间之外的所有数据，都会在堆上产生，并不会复用已有对象，需要使用重写后的equals判断是否相等。

#### 4.自动装箱与自动拆箱

装箱：将基本数据类型用引用类型包装起来；Integer i = 10，其实就是Integer i = Integer.valueOf(10);

拆箱：将包装类型转换为基本数据类型；int n = i，其实就是int n = i.intValue();

#### 5.浮点数运算精度丢失

这个问题还是比较复杂的，主要有两个原因：

- 当浮点数达到一定大数时，自动采用科学计数法，而科学计数法是有误差的；
- `double d1 = 0.3；`中十进制小数准换为二进制，本身就有误差；

如何解决？使用BigDecimal实现对浮点数的运算。

- 商业计算使用BigDecimal；
- 科学计算和工程计算使用float、double；

参考链接：

[double为什么会丢失精度](https://cloud.tencent.com/developer/article/1468551)

[十进制数如何转化为二进制数](https://www.runoob.com/w3cnote/decimal-decimals-are-converted-to-binary-fractions.html)

#### 6.表示超过long整型的数据

使用BigInteger，其内部使用int[]数组来存储任意大小的整型数据。

## 面向对象基础

#### 1.面向对象面向过程区别

主要区别在于解决问题的方式不同：

面向过程是将解决问题的过程拆分成一个一个方法，通过一个一个方法的执行解决问题；

面向对象会先抽象出类和对象，通过类间协作，对象通信的方式来解决问题。

#### 2.使用什么运算符创建对象，对象实体与对象引用有何不同？

使用new运算符创建对象。

对象实例保存在堆内存中，对象引用指向对象实例，保存在栈内存中。

#### 3.对象相等与引用相等

对象相等一般指内存中存放的内容是否相等；

引用相等一般指指向的内存地址是否相等。

#### 4.类的构造方法的作用

构造方法是一种特殊的方法，主要作用是完成对象的初始化工作。

#### 5.没有显式声明构造方法，程序可以正常执行吗

可以。没有显式声明构造方法，类也会有默认无参构造方法，用来参与对象的初始化工作。

#### 6.构造方法哟哪些特点，可以被重写吗

构造方法：

- 名字与类名相同
- 没有返回值
- 生成类对象时自动执行，无需调用

构造方法不能被override，但是可以被overload，所以一个类中往往有多个重载的构造函数。

#### 7.面向对象三大特征

封装：将对象的状态信息隐藏在对象内部，不允许外部对象直接访问。

继承：使用已经存在的类提高代码重用性；

多态：父类的引用指向子类的实例，引用类型变量发出的方法调用只有在运行期间才能准确确定。

#### 8.接口与抽象类的异同

同：

- 都不能被实例化
- 都可以包含抽象方法
- 都可以有默认实现的方法

异：

- 接口主要用于对行为进行约束，抽象类主要用于代码复用；
- 类只能继承一个父类，但是可以实现多个接口；
- 接口中的成员变量只能是public static final类型的，不能被修改且必须由初始值，而抽象类的成员变量默认default，可在子类中被重新定义，也可被重新赋值。

#### 9.深拷贝与浅拷贝，什么是引用拷贝

![浅拷贝、深拷贝、引用拷贝示意图](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/shallow&deep-copy.png)

**浅拷贝**在堆内存上创建一个新的对象，如果对象内部属性是引用类型的话，浅拷贝会直接复制对象内部的引用地址，即拷贝对象与原对象共用一个内部对象。

**深拷贝**完全复制整个对象，包括对象包含的内部对象。

**引用拷贝**是两个不同的引用指向同一个对象。

## Java常见类

### Object类

Object是一个特殊的类，它是所有java类的父类，主要提供11个方法：

```java
/**
 * native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
 */
public final native Class<?> getClass()
/**
 * native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
 */
public native int hashCode()
/**
 * 用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
 */
public boolean equals(Object obj)
/**
 * naitive 方法，用于创建并返回当前对象的一份拷贝。
 */
protected native Object clone() throws CloneNotSupportedException
/**
 * 返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
 */
public String toString()
/**
 * native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
 */
public final native void notify()
/**
 * native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
 */
public final native void notifyAll()
/**
 * native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
 */
public final native void wait(long timeout) throws InterruptedException
/**
 * 多了 nanos 参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上 nanos 毫秒。。
 */
public final void wait(long timeout, int nanos) throws InterruptedException
/**
 * 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
 */
public final void wait() throws InterruptedException
/**
 * 实例被垃圾回收器回收的时候触发的操作
 */
protected void finalize() throws Throwable { }
```

#### 1.==与equals的区别

对于基本数据类型与引用数据类型，

- 基本数据类型，==比较的是值；
- 引用数据类型，==比较的是对象内存地址；

本质上，无论基本数据类型还是引用数据类型，比较的都是变量的值，引用数据类型的值是对象的地址。

equals用于判断两个对象是否相等，在Object类中，所有的类都有这个方法。

- 类没有重写equals方法，通过equals比较该类的两个对象时，等价于通过==比较两个对象的地址；
- 类重写equals方法，可以用来比较两个对象属性是否相等，如果属性相等，则认为两个对象相等。

#### 2.hashcode()有什么用

hashcode()作用是获取哈希码，也称为散列码。这个哈希码的作用是确定该对象在哈希表中的索引位置。

hashcode()定义在Object类中，即所有类中都含有hashcode()函数，Object类中的hashcode()是本地方法。

hashcode()主要是用来判断元素是否在容器中，比如HashSet集合，会先计算对象的hashcode值来判断对象加入的位置，同时也会与其他加入对象的hashcode比较，如果没有相同的hashcode，HashSet会假设对象没有重复出现。如果发现有相同的hashcode，还会继续调用equals方法判断是否对象真的相同。大大减少了equals的次数，提高哈希表的性能。

#### 3.两个对象的hashcode相同，他们一定是相等的吗

不一定。哈希算法可能碰撞产生相同的哈希值，实际对象相等需要equals方法判断。

### String类

#### 1.String、StringBuilder、StringBuffer

String是不可变的，线程安全，每次改变都会生成一个新的String对象，然后将指针指向新的String对象。

StringBuffer每次都是对StringBuffer对象本身进行操作，对append方法加了同步锁，是线程安全的；

StringBuilder与StringBuffer有公共父类AbstratStringBuilder，StringBuilder没有对方法加同步锁，所以是非线程安全的，但是性能略有提升。

> 操作少量数据：String
>
> 单线程下操作大量数据：StringBuilder
>
> 多线程下操作大量数据：StringBuffer

#### 2.字符串拼接使用“+”还是StringBuilder

字符串对象使用“+”拼接，实际上是创建StringBuilder对象，通过StringBuilder对象调用append方法实现字符串拼接。

在循环内使用“+”进行拼接，编译器每次都会创建一个新的StringBuilder，性能消耗很大，可以在循环外手动创建Stringbuilder对象进行拼接。

#### 3.String的queals()与Object的equals()区别

String的equals方法是重写过的，比较的是两个字符串的字面值是否相等，Object类中的equals()方法比较的是对象的地址，等同于“==”效果。

#### 4.字符串常量池

字符串常量池是JVM为了减少内存消耗针对字符串（String类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

#### 5.String str = new String("abc");创建了几个字符串对象

如果字符串常量池中没有字符串对象“abc”的引用，会在堆中创建2个字符串对象“abc”；

如果字符串常量池中已经存在字符串对象“abc”的引用，则只会在堆内存中创建1个字符串对象“abc”；

#### 6.intern方法

`String.intern()` 是一个 native（本地）方法，其作用是将指定的字符串对象的引用保存在字符串常量池中，可以简单分为两种情况：

- 如果字符串常量池中保存了对应的字符串对象的引用，就直接返回该引用。
- 如果字符串常量池中没有保存了对应的字符串对象的引用，那就在常量池中创建一个指向该字符串对象的引用并返回。

```java
// 在堆中创建字符串对象”Java“
// 将字符串对象”Java“的引用保存在字符串常量池中
String s1 = "Java";
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s2 = s1.intern();
// 会在堆中在单独创建一个字符串对象
String s3 = new String("Java");
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s4 = s3.intern();
// s1 和 s2 指向的是堆中的同一个对象
System.out.println(s1 == s2); // true
// s3 和 s4 指向的是堆中不同的对象
System.out.println(s3 == s4); // false
// s1 和 s4 指向的是堆中的同一个对象
System.out.println(s1 == s4); //true
```

#### 7.String类型的变量和常量“+”运算时发生了什么

**对于编译期可以确定值的字符串，也就是常量字符串 ，jvm 会将其存入字符串常量池。并且，字符串常量拼接得到的字符串常量在编译阶段就已经被存放字符串常量池，这个得益于编译器的优化。**

在编译过程中，Javac 编译器（下文中统称为编译器）会进行一个叫做 **常量折叠(Constant Folding)** 的代码优化。

常量折叠会把常量表达式的值求出来作为常量嵌在最终生成的代码中，这是 Javac 编译器会对源代码做的极少量优化措施之一(代码优化几乎都在即时编译器中进行)。

对于 `String str3 = "str" + "ing";` 编译器会给你优化成 `String str3 = "string";` 。

## 异常

异常层次类结构图：

![Java 异常类层次结构图](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/types-of-exceptions-in-java.png)

#### 1.Exception与Error的区别

在 Java 中，所有的异常都有一个共同的祖先 `java.lang` 包中的 `Throwable` 类。`Throwable` 类有两个重要的子类:

- **`Exception`** :程序本身可以处理的异常，可以通过 `catch` 来进行捕获。`Exception` 又可以分为 Checked Exception (受检查异常，必须处理) 和 Unchecked Exception (不受检查异常，可以不处理)。

- **`Error`** ：`Error` 属于程序无法处理的错误 ，我们没办法通过 `catch` 来进行捕获不建议通过`catch`捕获 。例如 Java 虚拟机运行错误（`Virtual MachineError`）、虚拟机内存不够错误(`OutOfMemoryError`)、类定义错误（`NoClassDefFoundError`）等 。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止。

#### 2.Checked Exception和Unchecked Exception有什么区别

**Checked Exception** 即 受检查异常 ，Java 代码在编译过程中，如果受检查异常没有被 `catch`或者`throws` 关键字处理的话，就没办法通过编译。

除了`RuntimeException`及其子类以外，其他的`Exception`类及其子类都属于受检查异常 。常见的受检查异常有： IO 相关的异常、`ClassNotFoundException` 、`SQLException`

**Unchecked Exception** 即 **不受检查异常** ，Java 代码在编译过程中 ，我们即使不处理不受检查异常也可以正常通过编译。

`RuntimeException` 及其子类都统称为非受检查异常，常见的有（建议记下来，日常开发中会经常用到）：

- `NullPointerException`(空指针错误)
- `IllegalArgumentException`(参数错误比如方法入参类型错误)
- `NumberFormatException`（字符串转换为数字格式错误，`IllegalArgumentException`的子类）
- `ArrayIndexOutOfBoundsException`（数组越界错误）
- `ClassCastException`（类型转换错误）
- `ArithmeticException`（算术错误）
- `SecurityException` （安全错误比如权限不够）
- `UnsupportedOperationException`(不支持的操作错误比如重复创建同一用户)

#### 3.Throwable常用方法

- `String getMessage()`: 返回异常发生时的简要描述
- `String toString()`: 返回异常发生时的详细信息
- `String getLocalizedMessage()`: 返回异常对象的本地化信息。使用 `Throwable` 的子类覆盖这个方法，可以生成本地化信息。如果子类没有覆盖该方法，则该方法返回的信息与 `getMessage()`返回的结果相同
- `void printStackTrace()`: 在控制台上打印 `Throwable` 对象封装的异常信息

#### 4.try-catch-finally的使用

- `try`块 ： 用于捕获异常。其后可接零个或多个 `catch` 块，如果没有 `catch` 块，则必须跟一个 `finally` 块。
- `catch`块 ： 用于处理 try 捕获到的异常。
- `finally` 块 ： 无论是否捕获或处理异常，`finally` 块里的语句都会被执行。当在 `try` 块或 `catch` 块中遇到 `return` 语句时，`finally` 语句块将在方法返回之前被执行。

## 泛型

#### 1.什么是泛型，有哪些作用

可以增强代码的可读性以及稳定性。

编译器可以对泛型参数进行检测，并且通过泛型参数可以指定传入的对象类型。比如 `ArrayList<Persion> persons = new ArrayList<Persion>()` 这行代码就指明了该 `ArrayList` 对象只能传入 `Persion` 对象，如果传入其他类型的对象就会报错。

#### 2.泛型的使用方式

泛型一般有三种使用方式:**泛型类**、**泛型接口**、**泛型方法**。

**1.泛型类**：

```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{

    private T key;

    public Generic(T key) {
        this.key = key;
    }

    public T getKey(){
        return key;
    }
}
```

如何实例化泛型类：

```java
Generic<Integer> genericInteger = new Generic<Integer>(123456);
```

**2.泛型接口** ：

```java
public interface Generator<T> {
    public T method();
}
```

实现泛型接口，不指定类型：

```java
class GeneratorImpl<T> implements Generator<T>{
    @Override
    public T method() {
        return null;
    }
}
```

实现泛型接口，指定类型：

```java
class GeneratorImpl<T> implements Generator<String>{
    @Override
    public String method() {
        return "hello";
    }
}
```

**3.泛型方法** ：

```java
   public static < E > void printArray( E[] inputArray )
   {
         for ( E element : inputArray ){
            System.out.printf( "%s ", element );
         }
         System.out.println();
    }
```

使用：

```java
// 创建不同类型数组： Integer, Double 和 Character
Integer[] intArray = { 1, 2, 3 };
String[] stringArray = { "Hello", "World" };
printArray( intArray  );
printArray( stringArray  );
```

#### 3.项目中哪些地方用到了泛型

- 自定义接口通用返回结果 `CommonResult<T>` 通过参数 `T` 可根据具体的返回类型动态指定结果的数据类型
- 定义 `Excel` 处理类 `ExcelUtil<T>` 用于动态指定 `Excel` 导出的数据类型
- 构建集合工具类（参考 `Collections` 中的 `sort`, `binarySearch` 方法）。

## 反射--没有完全理解本质

#### 0.反射的本质

![image-20221017165914453](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20221017165914453.png)

一句话概括：把java类中的各种成分映射成一个个的Java对象。

详细解释：当我们程序运行时，需要动态加载一些类。这些类之前用不到，不需要加载到jvm，但是在运行时需要加载。举个例子是项目底层可能需要mysql，也可能需要oracle，需要根据实际情况加载驱动类，这个时候反射就有用了。假设 com.java.dbtest.myqlConnection，com.java.dbtest.oracleConnection这两个类我们要用，这时候我们的程序就写得比较动态化，通过Class tc = Class.forName("com.java.dbtest.TestConnection");通过类的全类名让jvm在服务器中找到并加载这个类，而如果是oracle则传入的参数就变成另一个了。

还是很难理解，这些只有在学完jvm才能理解的比较深刻。

https://blog.csdn.net/APCSZDDXM/article/details/122006599?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-122006599-blog-107187851.pc_relevant_default&spm=1001.2101.3001.4242.1&utm_relevant_index=3

#### 1.反射的优缺点

**优点** ： 可以让咱们的代码更加灵活、为各种框架提供开箱即用的功能提供了便利

**缺点** ：让我们在运行时有了分析操作类的能力，这同样也增加了安全问题。比如可以无视泛型参数的安全检查（泛型参数的安全检查发生在编译时）。另外，反射的性能也要稍差点，不过，对于框架来说实际是影响不大的。

#### 2.获取Class对象的四种方式

**1. 知道具体类的情况下可以使用：**

```java
Class alunbarClass = TargetObject.class;
```

但是我们一般是不知道具体类的，基本都是通过遍历包下面的类来获取 Class 对象，通过此方式获取 Class 对象不会进行初始化

**2. 通过 `Class.forName()`传入类的全路径获取：**

```java
Class alunbarClass1 = Class.forName("cn.javaguide.TargetObject");
```

**3. 通过对象实例`instance.getClass()`获取：**

```java
TargetObject o = new TargetObject();
Class alunbarClass2 = o.getClass();
```

**4. 通过类加载器`xxxClassLoader.loadClass()`传入类路径获取:**

```java
ClassLoader.getSystemClassLoader().loadClass("cn.javaguide.TargetObject");
```

通过类加载器获取 Class 对象不会进行初始化，意味着不进行包括初始化等一系列步骤，静态代码块和静态对象不会得到执行

#### 3.反射的基本操作

1. 创建一个我们要使用反射操作的类 `TargetObject`。

```java
package cn.javaguide;

public class TargetObject {
    private String value;

    public TargetObject() {
        value = "JavaGuide";
    }

    public void publicMethod(String s) {
        System.out.println("I love " + s);
    }

    private void privateMethod() {
        System.out.println("value is " + value);
    }
}
```

1. 使用反射操作这个类的方法以及参数

```java
package cn.javaguide;

import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InstantiationException, InvocationTargetException, NoSuchFieldException {
        /**
         * 获取 TargetObject 类的 Class 对象并且创建 TargetObject 类实例
         */
        Class<?> targetClass = Class.forName("cn.javaguide.TargetObject");
        TargetObject targetObject = (TargetObject) targetClass.newInstance();
        /**
         * 获取 TargetObject 类中定义的所有方法
         */
        Method[] methods = targetClass.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println(method.getName());
        }

        /**
         * 获取指定方法并调用
         */
        Method publicMethod = targetClass.getDeclaredMethod("publicMethod",
                String.class);

        publicMethod.invoke(targetObject, "JavaGuide");

        /**
         * 获取指定参数并对参数进行修改
         */
        Field field = targetClass.getDeclaredField("value");
        //为了对类中的参数进行修改我们取消安全检查
        field.setAccessible(true);
        field.set(targetObject, "JavaGuide");

        /**
         * 调用 private 方法
         */
        Method privateMethod = targetClass.getDeclaredMethod("privateMethod");
        //为了调用private方法我们取消安全检查
        privateMethod.setAccessible(true);
        privateMethod.invoke(targetObject);
    }
}
```

输出内容：

```text
publicMethod
privateMethod
I love JavaGuide
value is JavaGuide
```

#### 4.反射的应用

- 动态代理的实现依赖于反射

​	使用了反射类 `Method` 来调用指定的方法

- 注解的实现依赖于反射

​	基于反射分析类，然后获取到类/属性/方法/方法的参数上的注解。你获取到注解之后，就可以做进一步的处理。

## 注解

#### 1.注解的本质

可以看作是一种特殊的注释，主要用于修饰类、方法或者变量，提供某些信息供程序在编译或者运行时使用。

注解本质是一个继承了`Annotation` 的特殊接口：

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {

}

public interface Override extends Annotation{

}
```

JDK 提供了很多内置的注解（比如 `@Override` 、`@Deprecated`），同时，我们还可以自定义注解。

#### 2.注解的解析方法

- **编译期直接扫描** ：编译器在编译 Java 代码的时候扫描对应的注解并处理，比如某个方法使用`@Override` 注解，编译器在编译的时候就会检测当前的方法是否重写了父类对应的方法。
- **运行期通过反射处理** ：像框架中自带的注解(比如 Spring 框架的 `@Value` 、`@Component`)都是通过反射来进行处理的。

## SPI

SPI 即 Service Provider Interface ，字面意思就是：“服务提供者的接口”，我的理解是：专门提供给服务提供者或者扩展框架功能的开发者去使用的一个接口。

SPI 将服务接口和具体的服务实现分离开来，将服务调用方和服务实现者解耦，能够提升程序的扩展性、可维护性。修改或者替换服务实现并不需要修改调用方。

很多框架都使用了 Java 的 SPI 机制，比如：Spring 框架、数据库加载驱动、日志接口、以及 Dubbo 的扩展实现等等。

## IO

#### 1.什么是序列化与反序列化

如果我们需要持久化 Java 对象比如将 Java 对象保存在文件中，或者在网络传输 Java 对象，这些场景都需要用到序列化。

简单来说：

- **序列化**： 将数据结构或对象转换成二进制字节流的过程
- **反序列化**：将在序列化过程中所生成的二进制字节流转换成数据结构或者对象的过程

综上：**序列化的主要目的是通过网络传输对象或者说是将对象存储到文件系统、数据库、内存中。**

![img](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/a478c74d-2c48-40ae-9374-87aacf05188c.png)

#### 2.transient关键字

`transient` 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 `transient` 修饰的变量值不会被持久化和恢复。

#### 3.IO流

Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。

- `InputStream`/`Reader`: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- `OutputStream`/`Writer`: 所有输出流的基类，前者是字节输出流，后者是字符输出流。

#### 4.为什么要区分字节流和字符流

问题本质想问：**不管是文件读写还是网络发送接收，信息的最小存储单元都是字节，那为什么 I/O 流操作要分为字节流操作和字符流操作呢？**

首先明确字节流适用于任何场景，而且有字节缓冲流，能提高读取和输入的效率，也就是BufferedOutputStream/BufferedInputStream。其操作与字节流基本都一样。--图片、音频
而字符流是为了应对汉字出现的情况。在GBK中汉字占2个字节，在UTF-8中汉字占3个字节，所以我们通过字节流读取文件的时候一般都是逐个字节转换就会导致乱码，而手动去根据不同编码去拼接则不方便，所以有字符流。--汉字

#### 5.BIO NIO AIO区别

略。



# Java集合常见面试题总结--2h