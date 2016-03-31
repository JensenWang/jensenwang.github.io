---
layout: post
title: Java开发中常见的23种设计模式
date: 2016-01-24
tags: Java
categor: Java
---

# 概论 #
_设计模式（Design pattern）_是一套被反复使用，多数人知晓的，经过分类编目的，代码设计经验的总结。使用设计模式市委了可重用代码，让代码更容易被他人理解，保证代码可靠性。
## 设计模式的分类 ##
总体来说设计模式分三大类：

**创建型模式**

共五种：工厂方法模式，抽象工厂模式，单例模式，构造者模式，原型模式

**构造型模式**

 共7种：适配器模式，装饰器模式，代理模式，外观模式，桥接模式，组合模式，享元模式。
 
**行为模式**

 共11种：策略模式，模板方法模式，观察者模式，迭代器模式，责任链模式，命令模式，备忘录模式，状态模式，访问者模式，中介者模式，解释器模式。
![](http://i3.piimg.com/6101f65132be0479.png)

## 设计模式的六大原则 ##
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
    public class FactoryTest {
	public static void main(String[] args) {
		SenderFactory factory = new SenderFactory();
		Sender mailSender = factory.prodece("mail");
		mailSender.send();
		Sender smsSender = factory.prodece("sms");
		smsSender.send();
	}
}