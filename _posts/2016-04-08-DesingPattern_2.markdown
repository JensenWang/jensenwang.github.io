---
layout: post
title: Java开发中常见的23种设计模式(二)
date: 2016-04-08
tags: Java
categor: Java
---

之前讲了五种创建型模式，接下来是七种结构型模式：适配器模式、装饰模式、代理模式、外观模式、桥接模式、组合模式、享元模式。其中对象的适配器模式是各种模式的起源：
![](http://i4.piimg.com/303c321827ebe9c9.png)

## 适配器模式
适配器模式将某个类的接口转换成客户端期望的另一个接口表示，目的是消除由于接口不匹配造成的类的兼容性问题。主要分为三类：类的适配器，对象的适配器，接口的适配器模式。

**类的适配器：**
![](http://i4.piimg.com/a1851884e45be831.jpg)
核心思想就是：有一个Source类，拥有一个方法，待适配，目标接口是Targetable，通过Adapter类，将Source的功能扩展到Targetable里：
{% highlight java %}
public class Source {
    public void method1(){
        System.out.println("This is original method!");
    }

}
{% endhighlight %}
{% highlight java %}
public interface Targetable {
    // 与原类中方法相同
    public void method1();
    // 新类的方法
    public void method2();
}
{% endhighlight %}
{% highlight java %}
public class Adapter extends Source implements Targetable{
    public void method2() {
        System.out.println("This is targetable method!");
    }
}public class Adapter extends Source implements Targetable{
    public void method2() {
        System.out.println("This is targetable method!");
    }
}
{% endhighlight %}
Adapter类继承Source类，实现Targetable接口，下面是测试类：
{% highlight java %}
public class AdapterTest {

    public static void main(String[] args) {
        Targetable target = new Adapter();
        target.method1();
        target.method2();
    }
}
{% endhighlight %}

**对象的适配器模式：**

基本思路和类的适配器模式相同，只是将Adapter类作修改，这次不继承Source类，而是持有Source类的实例，已达到解决兼容性的问题。
![](http://i2.piimg.com/5ed11c50586c3225.png)
只需要修改Adapter类的源码即可：
{% highlight java %}
public class Wrapper implements Targetable{
    private Source source;

    public Wrapper(Source source) {
        super();
        this.source = source;
    }
    public void method1() {
        source.method1();
    }
    public void method2() {
        System.out.println("This is targetable method!");
    }
}
{% endhighlight %}
测试类：
{% highlight java %}
public class AdapterTest {
    public static void main(String[] args) {
        Source source = new Source();
        Targetable target = new Wrapper(source);
        target.method1();
        target.method2();
    }

}
{% endhighlight %}

**接口的适配器模式：**
接口的适配器模式是这样的：有时我们写的一个接口中有多个抽象方法，当我们写该接口的实现类时，必须实现该接口的所有方法，这明显有时比较浪费，因为并不是所有的方法都是我们需要的，有时只需要某一些，此处为了解决这个问题，我们引入了接口的适配器模式，实现了所有的方法，而我们不和原接口打交道，只和该抽象类去的联系，所以我们写一个类，重写我们需要的方法就行。
![](http://i2.piimg.com/b343f6798ada7dd5.png)
{% highlight java %}
public interface Sourceable {
    public void method1();
    public void method2();

}
{% endhighlight %}
{% highlight java %}
public abstract class Wrapper implements Sourceable{
    public void method1(){}
    public void method2(){}

}
{% endhighlight %}
{% highlight java %}
public class SourceSub1 extends Wrapper{
    public void method1(){
        System.out.println("The sourceable interface's first Sub1");
    }

}
{% endhighlight %}
{% highlight java %}
public class SourceSub2 extends Wrapper{
    public void method2(){
        System.out.println("The sourceable interface's second Sub2!");
    }

}
{% endhighlight %}
{% highlight java %}
public class WrapperTest {
    public static void main(String[] args) {
        Sourceable source1 = new SourceSub1();
        Sourceable source2 = new SourceSub2();

        source1.method1();
        source1.method2();
        source2.method1();
        source2.method2();
    }

}
{% endhighlight %}
三种适配器模式的应用场景：

类的适配器模式：当希望将一个类转换成满足另一个新接口的类时，可以使用类的适配器模式，创建一个新类，继承原有的类，实现新的接口即可。
对象的适配器模式：当希望将一个对象转换成满足另一个新接口的对象时，可以创建一个Wrapper类，持有原有类的一个实例，，在Wrapper类的方法中，调用实例的方法就行。
接口的适配器模式：当不需要实现一个接口中所有的方法时，可以创建一个抽象类Wrapper，实现所有方法，我们写别的类的时候，继承抽象类即可。

## 装饰模式（Decorator）
顾名思义，装饰模式就是给一个对象增加一些新的功能，而且是动态的，要求装饰对象和被装饰的对象实现同一个接口，装饰对象持有被装饰对象的实例，关系图如下：
![](http://i4.piimg.com/cfd11f6080d2b69e.png)
Source类是被装饰类，Decorator类是一个装饰类，可以为Source类动态添加一些功能，代码如下：
{% highlight java %}
public interface Sourceable {
    public void method();

}
{% endhighlight %}
{% highlight java %}
public class Source implements Sourceable{
    @Override
    public void method() {
        // TODO Auto-generated method stub
        System.out.println("The original method");
    }
}
{% endhighlight %}
{% highlight java %}
public class Decorator implements Sourceable{
    private Sourceable source;

    public Decorator(Sourceable source) {
        super();
        this.source = source;
    }
    public void method() {
        System.out.println("Before decorator");
        source.method();
        System.out.println("After decorator");
    }
}
{% endhighlight %}
{% highlight java %}
public class DecoratorTest {
    public static void main(String[] args) {
        Sourceable source = new Source();
        Sourceable obj = new Decorator(source);
        obj.method();
    }

}
{% endhighlight %}
**装饰模式应用的场景：**

1. 需要扩展一个类的功能
2. 动态的为一个对象增加功能，而且还能动态撤销（继承不能做到这一点，继承的功能是静态的，不能动态增删）。

	缺点：产生过多相似的对象，不易拍错

## 代理模式（Proxy）
代理模式就是多一个代理类出来，替原对象进行一些操作，比如我们在租房子的时候会去找中介，为什么呢？因为你对该地区房屋的信息掌握的不全面，希望找一个更熟悉的人帮你做，此处的代理模式就是这个意思。再比如我们有时候打官司，我们需要请律师，，因为律师在法律方面有专长，可以替我们进行操作，表达我们的想法。先来看关系图：
![](http://i4.piimg.com/c184c503ae06cd0f.png)
根据上文的阐述，代理模式就比较容易理解了，我们看代码：
{% highlight java %}
public interface Sourceable {
    public void method();

}
{% endhighlight %}
{% highlight java %}
public class Source implements Sourceable{
    @Override
    public void method() {
        // TODO Auto-generated method stub
        System.out.println("The original method");
    }
}
{% endhighlight %}
{% highlight java %}
public class Proxy implements Sourceable{
    private Source source;


    public Proxy() {
        super();
        this.source = new Source();
    }
    public void method() {
        before();
        source.method();
        after();
    }

    private void before() {
        System.out.println("before proxy");
    }
    private void after() {
        System.out.println("after proxy");
    }
}
{% endhighlight %}
测试类：
{% highlight java %}
public class ProxyTest {
    public static void main(String[] args) {
        Sourceable source =new Proxy();
        source.method();
    }
}
{% endhighlight %}
**代理模式应用场景：**
如果已有的方法在使用的时候需要对原有的方法进行改进，此时有两种方法：

1. 修改与哪有的方法来适应。这样就违反了“对扩展开发，对修改关闭”的原则。
2. 就是采用一个代理类调用原有的方法，且产生的结果进行控制。这种方法就是代理模式。

使用代理模式，可以将功能划分的更加清晰，有助于后期维护。

## 外观模式（Facade）
外观模式是为了解决类与类之间的依赖关系的，想Spring一样，可以将类和类之间的关系配置到配置文件中，而外观模式就是将他们的关系放在一个Facade类中，降低了类与类之间的耦合度，该模式中没有涉及到接口，我们模拟一个计算机的启动过程：
![](http://i4.piimg.com/b77482270b5ece94.png)
实现类：
{% highlight java %}
public class CPU {
    public void startup(){
        System.out.println("cpu startup!");
    }

    public void shutdown(){
        System.out.println("cpu showdown!");
    }

}
{% endhighlight %}
{% highlight java %}
public class Memory {
    public void startup(){
        System.out.println("memory startup!");
    }

    public void shutdown(){
        System.out.println("memory shutdown!");
    }

}
{% endhighlight %}
{% highlight java %}
public class Disk {
    public void startup(){
        System.out.println("disk startup!");
    }

    public void shutdown(){
        System.out.println("disk shtudown!");
    }
}
{% endhighlight %}
{% highlight java %}
public class User {
    public static void main(String[] args) {
        Computer computer = new Computer();
        computer.startup();
        System.out.println("--------------------------------");
        computer.shutdown();
    }
}
{% endhighlight %}

如果我们没有Computer类，那么，CPU，Memory，Disk他们之间将会相互持有实例，产生关系，这样会造成严重的依赖，修改一个类，可能会带来其他类的修改，这不是我们想要看到的，有了Computer类，他们之间的关系就被放在了Computer类里，这样就起到了解耦的作用，这就是外观模式。
