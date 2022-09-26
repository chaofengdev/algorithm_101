[TOC]



# 一、面向对象设计原则

## 设计原则总结

| 设计原则名称      | 简介                                               |
| ----------------- | -------------------------------------------------- |
| 单一职责原则  SRP | 一个对象应该只包含单一的职责。                     |
| 开闭原则 OCP      | 软件实体应该对扩展开放，对修改关闭。               |
| 里氏替换原则 LSP  | 所有引用父类的地方必须能够透明的使用其子类。       |
| 依赖倒置原则 DIP  | 高层模块不应该（直接）依赖于底层模块。             |
| 接口隔离原则 ISP  | 如果一个接口太大，则需要将大接口分割成多个小接口。 |
| 合成复用原则 CRP  | 尽量使用对象组合，而不是继承来达到复用的目的。     |
| 迪米特法则 LoD    | 一个软件实体应当尽可能少的与其他实体发生相互作用。 |

## 单一职责原则 SRP

#### 定义

一个对象应该只包含单一的职责，并且该职责被完整封装到一个类中。

#### 实例

![image-20220915110747153](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220915110747153.png)

基于Java的某系统的“登录功能”通过该登录类Login实现。

类图中省略了类的属性，Login类说明如下：

> init()方法用于初始化按钮，文本框等界面控件；
>
> display()方法用于向界面容器中添加界面控件并显示窗口；
>
> validate()方法用于提供登录按钮的事件处理方法调用，成功则进入主界面，失败则提示错误信息；
>
> findUser()方法用于获取数据库连接对象Connection，连接数据库；
>
> findUser()方法用于根据用户名和密码查询数据库中是否存在特定用户，该方法调用getConnection()并被validate()调用；

现在使用单一职责原则对其重构。

![image-20220915113020699](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220915113020699.png)

在这个例子中，类Login承担了多重职责，既包括与界面有关的方法，又包括了与数据库操作有关的方法，甚至包括了系统的入口函数main()方法。

无论是修改界面还是修改访问数据库的方式，都需要修改这个类，同时也不能直接复用这些代码，只能复制粘贴部分代码。

根本原因是类的职责过重，所以根据单一职责原则，按照功能对类进行拆分如下：

> 类LoginForm负责界面展示，只包括与界面有关的方法和事件处理方法，对应表现层；
>
> 类UserDAO负责用户表的增删改查操作，封装了对数据库表的操作代码，对应业务层；
>
> 类DButil负责数据库的连接，所有操作数据库的类都可以通过调用getConnection()方法获得数据类连接对象，对应持久层；
>
> 类MainClass负责启动系统，该类中定义了main()方法。

通过单一职责原则的重构，系统类的个数增加，但是类的复用性很好。DBUtil类可以被多个UserDAO使用，UserDAO类可以被多个界面类使用，一个类的修改不会影响其他类，系统的可维护性也将增强。

## 开闭原则 OCP

#### 定义

一个软件实体应当对扩展（提供方）开放，对修改（使用方）关闭。

当软件发生变化时，尽量通过新增代码实现变化，不要通过修改原来代码实现变化。

#### 实例

某图形界面系统提供

了各种不同形状的按钮，客户端代码可以针对这些按钮进行编程，用户可能改变需求，要求使用不同的按钮，原始设计方案如图所示：

![image-20220915114538882](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220915114538882.png)

如果界面类LoginForm需要将圆形按钮CircleButton改为方形按钮RectangleButton，则需要修改LoginForm的源代码，即修改按钮类的类名。同时因为圆形按钮和方形按钮的显示方法不同，也需要修改display()方法。

如果重构呢，其实问题很简单，LoginForm类面向具体类进行编程，每次更换具体类不得不修改源码，所以将按钮类进行抽象化，提取一个抽象按钮类，LoginForm针对抽象按钮类AbstratctButton进行编程，同时在配置文件中配置好具体类名，运行时动态生成实例对象。

![image-20220915162646606](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220915162646606.png)

使用开闭原则重构后，LoginForm对抽象类AbstractButton进行编程，如果需要增加新的菱形按钮DiamondButton，只需要增加一个类继承抽象按钮类，修改配置文件config.xml即可。这样在不变动抽象按钮类、其他具体按钮类、LoginForm类源码的同时，实现了LoginForm使用新按钮的功能，完全符合开闭原则。

事实上，开闭原则是我们追求的目标，代码不可能永远符合开闭原则，这个例子中，使用方依赖配置文件，变更的是配置文件，原有的java代码没有任何修改，我们认为是符合开闭原则的系统。

config.xml中配置如下：

```xml
...

<className>CircleButton<className>

...
```

## 里氏替换原则

#### 定义

所有引用父类的地方必须能够透明的使用其子类。

#### 实例

![image-20220915172526869](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220915172526869.png)

系统需要对重要数据进行加密，数据操作类DataOperator需要调用加密类CiperA和CiperB，选择其中一个实现加密操作。

这样设计的问题在于，Client类和DataOperator类都对具体子类进行编程，这样每增加一个类，都需要修改DataOperator类。

下面使用里氏替换原则进行重构。

![image-20220915173258089](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220915173258089.png)

因为子类CiperB继承了父类CiperA，所有能使用CiperA的地方都可以使用CiperB，如果新增一个CiperC，直接让CiperC继承CiperA即可，这样DataOperator类中的代码就不需要改动，Client类中每次传入需要的子类实例化对象即可。

以上是使用里氏替换原则重构的代码，其实按照开闭原则来说，最好的方式应该是抽象出抽象类Ciper，所有CiperX继承抽象类Ciper，这样DataOperator针对抽象类编程，会更好一些。

注意，里氏替换原则与开闭原则并不冲突，是实现开闭原则的重要方式之一。历史替换原则的根本是，由于使用基类对象的地方都可以使用子类对象，所以在程序中尽量使用基类类型对对象进行定义，而在运行中再确定子类类型，用子类类型替换父类对象。其实就是java中常用的多态。

## 依赖倒置原则 DIP

#### 定义

高层模块不应该（直接）依赖于底层模块。--即高层和底层中间有抽象层

抽象不应该依赖于细节，细节应该依赖于抽象。--高层应该依赖抽象层，底层实现抽象层，抽象层不动，底层随便增加新类

这里比较不太好理解，说人话：面向过程设计中，上层直接依赖下层，下层发生改变，上层也会随之产生大量改变；现在上层不依赖下层而是抽象接口，这样只要接口稳定，下层实现发生改动，不影响上层实现。大概意思是这样，但是确实没有体现出“倒置”，可能是中文翻译的问题？查了查资料貌似也没有合理的解释。

更新：https://zhuanlan.zhihu.com/p/63269696 该文章可以解释“倒置”的由来。

#### 实例

注：这个例子就纯以说明为主，不是很详细的类图设计。

某系统提供一个数据转换模块，可以将来自不同数据源的数据转换为多种格式。原始类图设计如下：

![image-20220915190804930](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220915190804930.png)

由于需求的变化，可能需要新增数据源或者数据格式，这样每次新增都会修改MainClass中的main()方法，使用依赖倒置原则对其进行重构。

![image-20220915191337781](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220915191337781.png)

图中省略了conf.xml，同上。

引入了两个抽象类，MainClass依赖于抽象类进行编程，将具体类名存在配置文件中，通过xml解析技术和java反射机制生成具体类的实例，代替MainClass类中的抽象对象，实现真正的业务处理。这个过程中也是用到了里氏替换原则，依赖倒转原则必须以里氏替换原则为基础。

新增数据源或者文件格式，只需要增加一个子类，同时修改conf.xml配置文件，更换具体类名，无须对原有类代码进行任何修改，满足开闭原则。

所以我们发现，一个类图的设计往往会满足多个设计原则。

## 接口隔离原则

#### 定义

如果一个接口太大，则需要将大接口分割成多个小接口，使用该接口的客户端只需要知道与之相关的方法即可。

换句话说，客户端不应该依赖那些不需要的接口。

#### 实例

拥有多个客户端的系统，系统中定义了一个巨大的接口AbstractService来服务所有的客户类。

![image-20220915193140555](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220915193140555.png)

这个设计有什么问题吗？显然，客户端ClientA只需要使用方法operatorA()，但是因为胖接口的存在，被迫拥有了其他两个不相关的方法，一定程度上影响了系统的封装性，可以使用接口隔离的方式来重构。

![image-20220915195532601](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220915195532601.png)

改进方法其实很简单，就是将胖接口进行细化，确保每个用户端都有对应的接口。

这里有个问题需要注意，具体实现类还是“胖”类，有影响吗？其实没有影响，这里无论使用一个实现类还是三个实现类，因为Client都是针对接口编程，他们只能看到与自己相关的业务方法，子类的其他方法是看不到的。

总的来说，接口隔离比较容易理解，所谓接口隔离，就是“胖接口”按“职责”分离成多个“瘦接口”。

## 合成复用原则

#### 定义

又称为组合/聚合复用原则：尽量使用对象组合，而不是继承来达到复用的目的。

#### 实例

某个教学管理系统的数据库访问类设计如图所示。

![image-20220916002135422](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220916002135422.png)

DBUtil类用来连接数据库，提供getConnection()方法，返回Connection对象连接数据库。StudentDAO和TeacherDAO需要使用Connection对象连接数据库，所以选择了继承DBUtil类。这样做恰当吗？

其实很容易看出来，这样做是不恰当的。首先，DBUtil类可能是经常需要变动的，比如采用不同方式连接不同的数据库，父类代码发生变化的同时，子类同时继承了父类的变化；其次，单纯复用代码，采用继承会增加系统的不必要的耦合，违背开闭原则，系统扩展性很差。

如何修改呢，其实就是聚合/组合原则的应用，将继承关系改为聚合关系。这样做的好处是，如果需要修改为另一种数据库连接方式，只需要给DBUtil类添加子类，在子类中覆盖getConnection()方法，客户端调用setDBOperator()方式时注入子类对象即可。

![image-20220916003213170](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220916003213170.png)

其实更好的方式可以将DBUtil抽象出来，具体工具类作为子类，配置文件中配置具体子类的名称。这样用户无需修改任何代码，就可以完成连接方式的更新。

## 迪米特法则

#### 定义

这个是最难理解的一个设计原则。

总的来说，一个软件实体应当尽可能少的与其他实体发生相互作用。每一个软件实体对其他的实体都只有最少的知识，而且局限于那些与本实体密切相关的软件实体。

迪米特法则的初衷在于降低类之间的耦合。由于每个类尽量减少对其他类的依赖，因此，很容易使得系统的功能模块功能独立，相互之间不存在（或很少有）依赖关系。

迪米特法则不希望类之间建立直接的联系。如果真的有需要建立联系，也希望能通过它的友元类来转达。因此，应用迪米特法则有可能造成的一个后果就是：系统中存在大量的中介类，这些类之所以存在完全是为了传递类之间的相互调用关系——这在一定程度上增加了系统的复杂度。

#### 实例

迪米特法则的例子花样繁多，最典型的就是级联调用的例子。下面再举一个例子。

甲和朋友认识，朋友和陌生人认识，而甲和陌生人不认识，这时甲可以直接和朋友说话，朋友可以直接和陌生人说话，而如果甲想和陌生人说话，就必须通过朋友。

原始的设计如下。

![image-20220917001655885](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220917001655885.png)

这种方式是肯定不对的，甲根本没有通过朋友，直接引用了陌生人的方法，不符合迪米特法则。

![image-20220917002719745](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220917002719745.png)

显然这个改进，甲和陌生人没有任何关系了，但是可以通过朋友类中的play()方法调用陌生人类中的play()方法。

我不知道这个例子是不是完美符合迪米特法则，感觉并不是能完美说明？参考链接如下：

https://www.cnblogs.com/xiaobai1226/p/8670245.html

# 二、设计模式

## 类之间的关系

在讲设计模式之前，一定要明白类图中类之间的关系和相关表示：https://blog.csdn.net/weixin_42927264/article/details/82963556

## 设计模式的分类

设计模式一般有两种分类方式：

1. 根据目的（模式用来干什么），可以将设计模式分为创建型、结构型、行为型三种；
2. 根据范围（模式是处理类关系还是对象关系），可以将设计模式分为类模式、对象模式；

| 模式类别                                                     | 模式名称          | 模式说明 |
| ------------------------------------------------------------ | ----------------- | -------- |
| 创建型模式（5种，主要用于创建对象）                          | 单例模式（√）     |          |
|                                                              | 工厂方法模式（√） |          |
|                                                              | 抽象工厂模式（√） |          |
|                                                              | 建造者模式        |          |
|                                                              | 原型模式          |          |
| 结构型模式（7种，主要用于处理类或对象的组合）                | 适配器模式（√）   |          |
|                                                              | 桥接模式          |          |
|                                                              | 组合模式          |          |
|                                                              | 装饰模式（√）     |          |
|                                                              | 外观模式          |          |
|                                                              | 享元模式          |          |
|                                                              | 代理模式（√）     |          |
| 行为型模式（11种，主要用于描述对类或对象怎样交互和分配职责） | 职责链模式        |          |
|                                                              | 命令模式（√）     |          |
|                                                              | 解释器模式        |          |
|                                                              | 迭代器模式（√）   |          |
|                                                              | 中介者模式        |          |
|                                                              | 备忘录模式        |          |
|                                                              | 观察者模式（√）   |          |
|                                                              | 状态模式          |          |
|                                                              | 策略模式（√）     |          |
|                                                              | 模板方法模式      |          |
|                                                              | 访问者模式        |          |



## 单例模式

课后题：现实生活中，居民身份证号码具有唯一性，同一个人不允许有多个身份证号码，第一次申请时将分配给居民一个身份证号码，如果之后因为遗失等原因补办，还是使用原来的身份证号码，不会产生新的号码。现使用单例模式模拟该场景。

要求：给出实例类图和实例代码，并作出相应解释。

<img src="https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220926080459490.png" alt="image-20220926080459490" style="zoom:100%;" />

```java
public class IdentityCardNo{
	private static IdentityCardNo instance = null;//静态私有成员变量
	private String no;//字符串表示具体身份证号码
	private IdentityCardNo() {}//私有构造方法，让客户端不能直接new对象
	public static IdentityCardNo getInstance() {//静态公有工厂方法，用于返回唯一实例
		if(instance == null) {
			System.out.println("第一次办理身份证，分配新号码");//符合题目中的要求
			instance = new IndentityCardNo();
			instance.setIdentityCardNo("MF20130012");//这里简单给个身份证编号
		}else {
			System.out.println("重复办理身份证号码，获取旧号码！");
		}
		return instance;//返回唯一实例
	}
	private void setIdentityCardNo(String no) {
        this.no = no;
    }
    public String getIdentityCardNo() {
        return this.no;
    }
}
```

```java
public class Client {
	public static void main(String[] args) {
		IdentityCardNo no1,no2;
		no1 = IdentityCardNo.getInstance();
		no2 = IdentityCardNo.getInstance();
		System.out.println("身份证号码是否一致："+ (no1 == no2));
		
		String str1,str2;
		str1 = no1.getIdentityCardNo();
		str2 = no2.getIdentityCardNo();
		System.out.println("第一次号码："+str1);
		System.out.println("第二次号码："+str2);
		System.out.println("内容是否相同："+str1.equalsIgnoreCase(str2));
		System.out.println("是否是相同对象："+(str1 == str2));//字符串比较特殊，字面值相同不代表是同一个对象。
	}
}
```



## 抽象工厂模式

课后题：一个电器工厂可以产生多种类型的电器，如海尔工厂可以生产海尔电视机、海尔空调等，毕成工厂可以生产毕成电视机、毕成空调等，相同品牌的电器构成一个产品族，而相同类型的电器构成了一个产品等级结构。现使用抽象工厂模式模拟该场景。

要求：给出实例类图和实例代码，并作出相应解释。

![image-20220926085206006](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220926085206006.png)

![image-20220926085057052](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220926085057052.png)

```java
//抽象产品类
public interface Television {
    public void play();
}
public interface AirConditioner {
    public void changeTemperature();
}
//具体产品类
public class HaierTelevision implements Television {
    public void play() {
        System.out.println("海尔电视机播放中...");
    }
}
public class BcerTelevision implements Television {
    public void play() {
        System.out.println("毕成电视机播放中...");
    }
}
public class HaierAirConditioner implements AirConditioner {
    public void changeTemperature() {
        System.out.println("海尔空调温度改变中...");
    }
}public class BcerAirConditioner implements AirConditioner {
    public void changeTemperature() {
        System.out.println("毕成空调温度改变中...");
    }
}
//抽象工厂
public interface AbstractFactory {
    public Television prodeuceTelevision();
    public AirConditioner produceAirConditioner();
}
//具体工厂--注意这里具体哪个工厂生产哪个产品，对照类图画一画，体会一下抽象工厂与工厂的区别
public class HaierFactory implements AbstractFactory {
    public Televison produceTelevision() {
        return new HaierTelevision();//返回实例化对象
    }
    public AirConditioner produceAirConditioner() {
        return new HaierAirConditioner();//返回实例化对象
    }
}
public class BcerFactory implements AbstractFactory {
    public Television produceTelevision() {
        return new BcerTelevision();//返回实例化对象
    }
    public AirConditioner produceAirConditioner() {
        return new BcerAirConditioner();//返回实例化对象
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        AbstractFactory af = new HaierFactory();//获取具体工厂
        Television tv = af.produceTelevision();//通过具体工厂生产具体产品
        AirConditioner ac = af.produceAirConditioner();//通过具体工厂生产具体产品
        tv.play();//打开电视机
        ac.changeTemperature();//打开空调
    }
}
思考：这是打开哪个品牌的电视机和空调？尝试利用工厂获取毕成品牌产品的实例并打开相关产品。
```



## 策略模式

课后题：某系统提供了一个用于对数组进行操作的类，该类封装了对数组的常见操作，如查找数组元素、对数组元素进行排序等。现以排序操作为例，使用策略模式设计该数组操作类，是的客户端可以动态地更换排序算法，可以根据需要选择冒泡排序、选择排序或插入排序，也能够灵活地增加新的排序算法。

要求：给出实例类图和实例代码，并作出相应解释。

![image-20220926093641923](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220926093641923.png)

```java
//ps：其实本质上就是依赖倒置原则的应用。
//抽象排序类--抽象策略类
public class Sort {
	 public int[] sort(int[] arr);
}
//具体排序类--具体策略类
public class BubbleSort implements Sort {
	public int[] sort(int[] arr) {
        ... //排序逻辑，略
        return arr;
    }
}
public class SelectionSort implements Sort {
    public int[] sort(int[] arr) {
        ... //排序逻辑，略
        return arr;
    }
}
public class InsertionSort implements Sort {
    public int[] sort(int[] arr) {
        ... //排序逻辑，略
        return arr;
    }
}
//上下文环境类ArrayHandler
public class ArrayHandler {
    private Sort sortObj;
    public int[] sort(int[] arr) {
        sortObj.sort(arr);
        return arr;
    }
    public void setSortObj(Sort sortObj) {
        this.sortObj = sortObj;
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        int[] arr = {1,3,8,5,4,0,9,2};
        ArrayHandler ah = new ArrayHandler();//实例化上下文对象
        Sort sort = new InsertionSort();//实例化具体策略对象
        ah.setSortObj(sort);//为上下文设置具体策略
        int[] res;
        res = ah.sort(arr);//执行具体策略
        for(int i = 0; i < res.length; i++) {//打印输出
            System.out.print(res[i] + ",");
        }
    }
}
```



## 迭代器模式

课后题：电视机遥控器是一个迭代器的实例，通过它可以实现对电视机频道集合的遍历操作，本实例将模拟电视机遥控器的实现。

补充说明：电视机有两种品牌，分为海尔电视和毕成电视；海尔电视有CCTV-1->CCTV-5等中央电视台，毕成电视有湖南卫视->江苏卫视等地方电视台；

这里我们可以看到，电视机就是抽象聚合类，电视品牌就是具体聚合类，类中保存的就是电视台清单（当然也可以理解为电视节目），目前的要求就是遍历某个品牌的电视台清单。

要求：给出实例类图和实例代码，并作出相应解释。

![image-20220926095758483](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/image-20220926095758483.png)

```java
//抽象迭代器
public interface TVIterator {
    public Object next();
    pubilc boolean hasNext();
}
//抽象聚合类
public interface Television {
    public TVIterator createIterator();
}
//具体聚合类HaierTelevision
public class HaierTelevision implements Television {
    private Object[] obj = {"CCTV-1","CCTV-2","CCTV-3","CCTV-4","CCTV-5"};
    public TVIterator createIterator() {
        return new HaierIterator();
    }
    //内部类
    private class HaierIterator implements TVIterator {
        private int index = 0;
        public Object next() {
            if(this.hasNext()) {
                return obj[index++];
            }
            return null;
        }
        public boolean hasNext() {
            if(index < obj.length) {
                return true;
            }
            return false;
        }
    }
}
//具体聚合类BcerTelevision
public class HaierTelevision implements Television {
    private Object[] obj = {"湖南卫视","江苏卫视","福建卫视","...","..."};
    public TVIterator createIterator() {
        return new BcerIterator();
    }
    //内部类
    private class BcerIterator implements TVIterator {
        private int index = 0;
        public Object next() {
            if(this.hasNext()) {
                return obj[index++];
            }
            return null;
        }
        public boolean hasNext() {
            if(index < obj.length) {
                return true;
            }
            return false;
        }
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        Television tv = new HaierTelevision();//先有具体聚合类（集合）
        TVIterator iterator = tv.createIterator();//再有迭代器对象
        //通过迭代器对象访问集合
        while(iterator.hasNext()) {
            System.out.println(iterator.next().toString());
        }
    }
}
```

> 以下是一点补充。
>
> 迭代器模式在java集合中广泛应用。比如Collection集合有很多子类ArrayList、LinkedList，Collection中有一个方法iterator()，返回Iterator类型的迭代器对象，通过这个对象就可以遍历集合中的所有元素。Collection集合返回抽象迭代器对象，其子类返回具体迭代器对象。
>
> 并且我们很少使用自定义迭代器，一般都是使用JDK内置的迭代器。下面是JDK源码。
>
> ```java
> //源码比较复杂，细枝末节很多，这里只是大意
> //AbstractList.java
> public ListIterator<E> listIterator() {
> 	return listIterator(0);
> }
> //Iterator.java
> public interface Iterator<E> {
>     /**
>      * Returns {@code true} if the iteration has more elements.
>      * (In other words, returns {@code true} if {@link #next} would
>      * return an element rather than throwing an exception.)
>      *
>      * @return {@code true} if the iteration has more elements
>      */
>     boolean hasNext();
> 
>     /**
>      * Returns the next element in the iteration.
>      *
>      * @return the next element in the iteration
>      * @throws NoSuchElementException if the iteration has no more elements
>      */
>     E next();
> 
>     /**
>      * Removes from the underlying collection the last element returned
>      * by this iterator (optional operation).  This method can be called
>      * only once per call to {@link #next}.
>      * <p>
>      * The behavior of an iterator is unspecified if the underlying collection
>      * is modified while the iteration is in progress in any way other than by
>      * calling this method, unless an overriding class has specified a
>      * concurrent modification policy.
>      * <p>
>      * The behavior of an iterator is unspecified if this method is called
>      * after a call to the {@link #forEachRemaining forEachRemaining} method.
>      *
>      * @implSpec
>      * The default implementation throws an instance of
>      * {@link UnsupportedOperationException} and performs no other action.
>      *
>      * @throws UnsupportedOperationException if the {@code remove}
>      *         operation is not supported by this iterator
>      *
>      * @throws IllegalStateException if the {@code next} method has not
>      *         yet been called, or the {@code remove} method has already
>      *         been called after the last call to the {@code next}
>      *         method
>      */
>     default void remove() {
>         throw new UnsupportedOperationException("remove");
>     }
> 
>     /**
>      * Performs the given action for each remaining element until all elements
>      * have been processed or the action throws an exception.  Actions are
>      * performed in the order of iteration, if that order is specified.
>      * Exceptions thrown by the action are relayed to the caller.
>      * <p>
>      * The behavior of an iterator is unspecified if the action modifies the
>      * collection in any way (even by calling the {@link #remove remove} method
>      * or other mutator methods of {@code Iterator} subtypes),
>      * unless an overriding class has specified a concurrent modification policy.
>      * <p>
>      * Subsequent behavior of an iterator is unspecified if the action throws an
>      * exception.
>      *
>      * @implSpec
>      * <p>The default implementation behaves as if:
>      * <pre>{@code
>      *     while (hasNext())
>      *         action.accept(next());
>      * }</pre>
>      *
>      * @param action The action to be performed for each element
>      * @throws NullPointerException if the specified action is null
>      * @since 1.8
>      */
>     default void forEachRemaining(Consumer<? super E> action) {
>         Objects.requireNonNull(action);
>         while (hasNext())
>             action.accept(next());
>     }
> }
> //算了，看源码有点累，没耐心看了。以后有空重新整理这部分的内容。
> //注意迭代器模式里的Interator类要和Collection的父接口Iterable区分。
> //https://www.cnblogs.com/litexy/p/9744241.html
> ```

