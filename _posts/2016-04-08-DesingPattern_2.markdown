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
