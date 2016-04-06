---
layout: post
title: Java开发中常见的23种设计模式
date: 2016-01-24
tags: Java
categor: Java
---

# 概论
*设计模式（Design pattern）* 一套被反复使用，多数人知晓的，经过分类编目的，代码设计经验的总结。使用设计模式市委了可重用代码，让代码更容易被他人理解，保证代码可靠性。

## 设计模式的分类
总体来说设计模式分三大类：

**创建型模式**

共五种：工厂方法模式，抽象工厂模式，单例模式，构造者模式，原型模式

**构造型模式**

 共7种：适配器模式，装饰器模式，代理模式，外观模式，桥接模式，组合模式，享元模式。

**行为模式**

 共11种：策略模式，模板方法模式，观察者模式，迭代器模式，责任链模式，命令模式，备忘录模式，状态模式，访问者模式，中介者模式，解释器模式。
![](http://i3.piimg.com/6101f65132be0479.png)

## 设计模式的六大原则

**1.开闭原则（Open Close Principle）**

开闭原则就是说对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有代码，实现一个热插拔的效果。所以一句话概况就是：为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用到接口和抽象类。

**2.里氏代换原则（Liskov Substitution Principle）**

里氏代换原则（Liskov Substitution Principle LSP）面向对象设计的基本原则之一。里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。LSP是继承复用的基石，只有当衍生类可以替换掉基类，软件单位的功能不收到影响时，基类才能真正被复用，而衍生类也能够在基类的基础上增加新的行为。里氏代换原则是对”开-闭“原则的补充。实现”开-闭“原则的关键步骤就是抽象化。而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则时对实现抽象化的具体规范。

**3.依赖倒转原则（Dependence Inversion Principle）**

这个时开闭原则的基础，具体内容：针对接口编程，依赖于抽象而不依赖具体。

**4.接口隔离原则（Interface Segregation Principle）**

这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。还是一个降低类之间的耦合度的意思，从这里我们看出，其实设计模式就是一个软件设计思想，从大型软件架构出发，为了升级和维护方便。所以上文中多次出现：降低依赖，降低耦合。

**5.迪米特原则（最少知道原则）（Demeter Principle）**

为什么叫最少知道原则，就是说：一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相互独立。

**6.合成复用原则（Composite Reuse Principle）**

原则时尽量使用合成/聚合的方式，而不是使用继承

## 工厂模式
**1.普通工厂模式**

就是简单的一个工厂类，对实现了同一接口的一些类进行实例的创建。
![](http://i4.piimg.com/06456250dd0f82b9.png)
举例如下（我们举一个发送邮件和短信的例子）
首先，创建二者的共同接口：
{% highlight java %}
public interface Sender {
    public void send();
}
{% endhighlight %}
其次，创建实现类：
{% highlight java %}
public class MailSender implements Sender{
	public void send(){
		System.out.println("This is mail sender");
	}
}
{% endhighlight %}
{% highlight java %}
public class SmsSender implements Sender{
	public void send() {
		System.out.println("This is sms sender");
	}
}
{% endhighlight %}
最后，创建工厂类：
{% highlight java %}
public class SenderFactory {
	public Sender prodece(String type) {
		if (type.equals("mail")){
			return new MailSender();
		} else if("sms".equals(type)) {
			return new SmsSender();
		} else {
			System.out.println("请输入正确的类型");
			return null;
		}
	}
}
{% endhighlight %}
测试：
{% highlight java %}
public class FactoryTest {
	public static void main(String[] args) {
		SenderFactory factory = new SenderFactory();
		Sender mailSender = factory.prodece("mail");
		mailSender.send();
		Sender smsSender = factory.prodece("sms");
		smsSender.send();
	}
}
{% endhighlight %}

**2.多个工厂模式**

多个工厂方法模式是对普通工厂方法模式的改进，在普通工厂模式中，如果传递的字符串出错，则不能正确创建对象，而多个工厂模式是提供多个工厂方法，分别创建对象。
![](http://i3.piimg.com/fe91f85d64d0329c.png)
将上面的方法修改，只需要修改SenderFactory类就行
{% highlight java %}
public class SenderFactory {
    public Sender produceMail() {
        return new MailSender();
    }

    public Sender produceSms() {
        return new SmsSender();
    }
}
{% endhighlight %}
测试如下：
{% highlight java %}
public class FactoryTest {
    public static void main(String[] args) {
        SenderFactory factory = new SenderFactory();
        Sender mail = factory.produceMail();
        mail.send();
        Sender sms = factory.produceSms();
        sms.send();
    }
}
{% endhighlight %}

**3.静态工厂模式**

将上面的多个工厂方法模式里面的方法设置为静态的，不需要创建实例，直接即可调用。
{% highlight java %}
public class SenderFactory {
    public static Sender produceMail() {
        return new MailSender();
    }

    public static Sender produceSms() {
        return new SmsSender();
    }
}
{% endhighlight %}
{% highlight java %}
public class FactoryTest {
    public static void main(String[] args) {
        Sender sms = SenderFactory.produceSms();
        sms.send();
        Sender mail = SenderFactory.produceMail();
        mail.send();
    }
}
{% endhighlight %}

**总的来说**
工厂模式适合：凡是出现了大量的产品需要来创建，并且有共同的接口时，可以通过使用工厂方法模式进行创建。在以上的三中模式中，第一种如果传入的字符串有误，不能正确的创建对象，第三种相对于第二种，不需要实例化工厂类，所以大多数情况下，，我们会选用第三种——静态工厂方法模式。

## 单例模式
单利对象（Singleton）是一种常用的设计模式。在Java应用中，单例对象能保证在一个JVM中，该对象只有一个实例存在。这样的模式有几个好处：

1. 某些类创建比较频繁，对于一些大型的对象，这是一笔很大的系统开销。
2. 省去了new操作符，降低了系统内存的使用频率，减轻GC压力。
3. 有些类比如交易所的核心交易引擎，控制着交易流程，如果该类可以创建多个的话，系统完全乱了。（比如一个军队出现了多个司令员同时指挥，肯定会乱成一团），所以只有使用单例模式，才能保证核心交易服务器控制整个流程。

首先，我们写一个简单的单例类：
{% highlight java %}
public class Singleton {
    /* 持有私有静态实例，防止被引用，此处赋值为null，目的是实现延迟加载 */
    private static Singleton instance = null;

    /* 私有构造方法，防止被实例化 */
    private Singleton() {

    }

    /* 静态工程方法，创建实例 */
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

    /* 如果该对象被用于实例化，可以保证对象在序列化前后保持一致 */
    public Object reanResolve() {
        return instance;
    }
}
{% endhighlight %}
这个类可以满足基本要求，但是这样毫无线程安全保护的类放到多线程的环境下，肯定会出问题。于是就可以对getInstance方法加上线程锁，如下：
{% highlight java %}
/* 添加synchronized关键字锁住方法，保护线程安全 */
public synchronized static Singleton getInstance() {
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
{% endhighlight %}
但是synchronized是锁住对象，每次调用getInstance()都会锁住对象，但是实际上只有第一次创建对象对的时候要加锁，之后就不需要了，所以还是要优化：
{% highlight java %}
private Singleton() {
    System.out.println("实例化一次");
}
/* 此处用一个内部类来维护单例 */
private static class SingletonFactory {
    private static Singleton instance = new Singleton();
}
/* 获取实例 */
public static Singleton getInstance() {
    return SingletonFactory.instance;
}
{% endhighlight %}
实际情况是，单例模式使用内部类来维护单例的实现，JVM内部的机制能够保证当一个类被加载的时候，这个类的加载过程是线程互斥的。这样我们第一次调用getInstance()的时候，JVM能够保证instance只被创建一次，并且会把值赋给instance的内存初始化完毕。同时该方法只会在第一次调用的时候使用互斥机制，这样就解决了低性能问题。*但是*这样的话，如果在构造函数中抛出异常，实例将永久得不到创建，也会出错。也有人这样实现：我们只需要在创建类的时候进行同步，所以只需要将创建和getInstance()分开，单独为创建加synchronized关键字也是可以的，如下：
{% highlight java %}
public class Singleton {
    private static Singleton instance = null;

    private Singleton(){
        System.out.println("实例化一次。");
    }

    private static synchronized void syncInit() {
        if(instance==null) {
            instance = new Singleton();
        }
    }

    /* 将创建对象与getInstance分开，优化性能 */
    public static Singleton getInstance() {
        if (instance  == null) {
            syncInit();
        }
        return instance;
    }
}
{% endhighlight %}

**问题：**
能否采用静态方法，实现单例模式的效果，二者有什么不同？

首先是可以使用静态方法实现单例模式的效果的，但是静态类不能实现接口。(从类的角度是可以的，但是那样就破坏了静态了。因为接口中不允许有static修饰的方法，所以即使实现了也是非静态的。)

其次，单例类可以被延迟初始化，静态类一般在第一次加载是初始化。之所以延迟加载，是因为有些类比较庞大，所以延迟加载有助于提升性能。

再次，单例类可以被继承，它的方法可以被覆写。但是静态类内部方法都是static，所以无法覆写。

最后一点，单例类比较灵活，毕竟从实现上只是一个普通的Java类，只要满足单例的基本需求，你可以在里面随心所欲的实现一些其它功能，但是静态类不行。从上面这些概括中，基本可以看出二者的区别，但是，从另一方面讲，我们上面最后实现的那个单例模式，内部就是用一个静态类来实现的，所以，二者有很大的关联，只是我们考虑问题的层面不同罢了。

## 建造者模式(Builder)
工厂模式提供的创建单个类的模式，而建造者模式（又叫生成器模式）则是将各种产品集中起来进行管理，用来创建复合对象，所谓复合对象就是指某个类具有不同的属性，其实建造者模式就是前面抽象工厂模式和最后的Test结合起来得到的。我们看一下代码:

还是和前面一样，一个Sender接口，两个实现类MailSender和SmsSender。最后，建造者类如下：
{% highlight java %}
public class Builder {
    private ArrayList<Sender> list = new ArrayList<Sender>();
    public void produceMailSender(int count) {
        for (int i = 0; i < count; i++){
            list.add(new MailSender());
        }
    }
    public void produceSmsSender(int count) {
        for (int i = 0; i < count; i++) {
            list.add(new SmsSender());
        }
    }
}
{% endhighlight %}
测试类：
{% highlight java %}
public class Test {
    public static void main(String[] args) {
        Builder builder = new Builder();
        builder.produceMailSender(10);
    }
}
{% endhighlight %}
从这点看出，建造者模式将很多功能集成到一个类里，这个类可以创造出比较复杂的东西。

所以与工厂模式的区别就是：工厂模式关注的是创建单个的产品，而建造者模式则关注创建复合对象，多个部分。

## 原型模式（Prototype）
原型模式虽然是创建型模式，但是与工厂模式没有关系，从名字即可看出，该模式的思想就是将一个对象作为原型，对其进行复制、克隆，产生一个和原对象类似的新对象。在Java中，复制对象是通过clone()实现的，先创建一个原型类：
{% highlight java %}
public class Prototype01 implements Cloneable {
    private int age;
    private String name;

    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Object clone() throws CloneNotSupportedException {
        Prototype01 proto = (Prototype01)super.clone();
        return proto;
    }

    public void print(){
        System.out.println("Age:" + this.age);
        System.out.println("Name:" + this.name);
    }

}
{% endhighlight %}
测试类：
{% highlight java %}
public class Test {
    public static void main(String[] args) {
        Prototype01 pro1 = new Prototype01();
        pro1.setAge(10);
        pro1.setName("ZhangSan");
        pro1.print();
        Prototype01 pro2 = null;
        try {
            pro2 = (Prototype01) pro1.clone();
        } catch (CloneNotSupportedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        pro2.print();
    }

}
{% endhighlight %}
很简单，一个原型类，只需要实现Cloneable接口，覆写clone()方法，此处clone方法可以改成任意名字，因为Cloneable是个空接口，你可以任意定制实现类的名称，因为此处重点是super.clone()这句话，super.clone()调用的是Object的clone()方法，而在Object类中，clone()是native的。

*复制分浅复制和深复制，以上只是浅复制。*
浅复制：将一个对象复制后，基本数据类型的变量都会重建，而引用类型，指向的还是原来对象的指向

深复制：将一个对象复制后，不论是基本数据类型还是引用类型，都是会重建的。

简单的来说，就是深复制进行了完全彻底的复制，而浅复制不彻底。

深复制:
{% highlight java %}
public class Prototype02 implements Cloneable {
    private String string;
    /* 深复制 */
    public Object deepClone() throws IOException, ClassNotFoundException{

        /* 写入当前对象的二进制流 */
        ByteArrayOutputStream bos = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream(bos);
        oos.writeObject(this);

        /* 读出二进制流产生的新对象 */
        ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
        ObjectInputStream ois = new ObjectInputStream(bis);
        return ois.readObject();
    }
    public String getString() {
        return string;
    }
    public void setString(String string) {
        this.string = string;
    }

}
{% endhighlight %}
