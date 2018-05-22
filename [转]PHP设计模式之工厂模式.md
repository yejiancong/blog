# PHP设计模式之工厂模式

## 概念

工厂模式是我们最常用的实例化对象模式，是用工厂方法代替new操作的一种模式。

使用工厂模式的好处是，如果你想要更改所实例化的类名等，则只需更改该工厂方法内容即可，不需逐一寻找代码中具体实例化的地方（new处）修改了。为系统结构提供灵活的动态扩展机制，减少了耦合。

根据抽象程度的不同，PHP工厂模式分为三种：

- 1.简单工厂模式
- 2.工厂方法模式
- 3.抽象工厂模式

## 讲解

### 简单工厂模式

简单工厂模式又称静态工厂方法模式，之所以可以这么说，是因为简单工厂模式是通过一个静态方法来创建对象的。

代码示例：

```php
<?php
header('Content-Type:text/html;charset=utf-8');
/**
 *简单工厂模式（静态工厂方法模式）
 */

/**
 * Interface people 人类
 */
interface  people
{
    public function  say();
}

/**
 * Class man 继承people的男人类
 */
class man implements people
{
    // 具体实现people的say方法
    public function say()
    {
        echo '我是男人<br>';
    }
}

/**
 * Class women 继承people的女人类
 */
class women implements people
{
    // 具体实现people的say方法
    public function say()
    {
        echo '我是女人<br>';
    }
}

/**
 * Class SimpleFactoty 工厂类
 */
class SimpleFactoty
{
    // 简单工厂里的静态方法-用于创建男人对象
    static function createMan()
    {
        return new man();
    }

    // 简单工厂里的静态方法-用于创建女人对象
    static function createWomen()
    {
        return new women();
    }

}

/**
 * 具体调用
 */
$man = SimpleFactoty::createMan();
$man->say();
$woman = SimpleFactoty::createWomen();
$woman->say();
```

运行结果：

```
我是男人
我是女人
```

## 工厂方法模式

定义一个用于创建对象的接口，让子类决定哪个类实例化。 他可以解决简单工厂模式中的封闭开放原则问题。

看代码：

```php
<?php
header('Content-type:text/html;charset=utf-8');
/*
 *工厂方法模式
 */

/**
 * Interface people 人类
 */
interface  people
{
    public function  say();
}

/**
 * Class man 继承people的男人类
 */
class man implements people
{
    // 实现people的say方法
    function say()
    {
        echo '我是男人-hi<br>';
    }
}

/**
 * Class women 继承people的女人类
 */
class women implements people
{
    // 实现people的say方法
    function say()
    {
        echo '我是女人-hi<br>';
    }
}

/**
 * Interface createPeople 创建人物类
 * 注意：与上面简单工厂模式对比。这里本质区别在于，此处是将对象的创建抽象成一个接口。
 */
interface  createPeople
{
    public function create();

}

/**
 * Class FactoryMan 继承createPeople的工厂类-用于实例化男人类
 */
class FactoryMan implements createPeople
{
    // 创建男人对象（实例化男人类）
    public function create()
    {
        return new man();
    }
}

/**
 * Class FactoryMan 继承createPeople的工厂类-用于实例化女人类
 */
class FactoryWomen implements createPeople
{
    // 创建女人对象（实例化女人类）
    function create()
    {
        return new women();
    }
}

/**
 * Class Client 操作具体类
 */
class  Client
{
    // 具体生产对象并执行对象方法测试
    public function test() {
        $factory = new FactoryMan();
        $man = $factory->create();
        $man->say();

        $factory = new FactoryWomen();
        $man = $factory->create();
        $man->say();
    }
}

// 执行
$demo = new Client;
$demo->test();
```

看结果：

```
我是男人-hi
我是女人-hi
```

### 抽象工厂模式

提供一个创建一系列相关或相互依赖对象的接口。

注意：这里和工厂方法的区别是：一系列（多个），而工厂方法只有一个。

代码：

```php
<?php
header('Content-type:text/html;charset=utf-8');
/*
 * 抽象工厂模式
 */

/**
 * Interface people 人类
 */
interface  people
{
    public function  say();
}

/**
 * Class OneMan 第一个男人类-继承people
 */
class OneMan implements people
{
    // 实现people的say方法
    public function say()
    {
        echo '男1：我喜欢你<br>';
    }
}

/**
 * Class TwoMan 第二个男人类-继承people
 */
class TwoMan implements people{
    // 实现people的say方法
    public function say()
    {
        echo '男2：我看上你了<br>';
    }
}

/**
 * Class OneWomen 第一个女人类-继承people
 */
class OneWomen implements people {
    // 实现people的say方法
    public function say()
    {
        echo '女1：我不喜欢你<br>';
    }
}

/**
 * Class TwoWomen 第二个女人类-继承people
 */
class TwoWomen implements people {
    // 实现people的say方法
    public function say()
    {
        echo '女2：滚一边玩去<br>';
    }
}

/**
 * Interface createPeople 创建对象类
 * 注意:这里将对象的创建抽象成了一个接口。
 */
interface  createPeople
{
    // 创建第一个
    public function createOne();
    // 创建第二个
    public function createTwo();

}

/**
 * Class FactoryMan 用于创建男人对象的工厂类-继承createPeople
 */
class FactoryMan implements createPeople
{
    // 创建第一个男人
    public function createOne()
    {
        return new OneMan();
    }

    // 创建第二个男人
    public function createTwo()
    {
        return new TwoMan();
    }
}

/**
 * Class FactoryWomen 用于创建女人对象的工厂类-继承createPeople
 */
class FactoryWomen implements createPeople
{
    // 创建第一个女人
    public function createOne()
    {
        return new OneWomen();
    }

    // 创建第二个女人
    public function createTwo()
    {
        return new TwoWomen();
    }
}

/**
 * Class Client 执行测试类
 */
class  Client {

    // 具体生成对象和执行方法
    public function test() {
        // 男人
        $factory = new FactoryMan();
        $man = $factory->createOne();
        $man->say();

        $man = $factory->createTwo();
        $man->say();

        // 女人
        $factory = new FactoryWomen();
        $man = $factory->createOne();
        $man->say();

        $man = $factory->createTwo();
        $man->say();

    }
}

// 执行
$demo = new Client;
$demo->test();
```
结果：

```
男1：我喜欢你
男2：我看上你了
女1：我不喜欢你
女2：滚一边玩去
```

## 总结

### 区别

- 1.简单工厂模式(静态方法工厂模式) ： 用来生产同一等级结构中的任意产品。（不能增加新的产品）
- 2.工厂模式 ：用来生产同一等级结构中的固定产品。（支持增加任意产品）
- 3.抽象工厂 ：用来生产不同产品种类的全部产品。（不能增加新的产品，支持增加产品种类）

### 适用范围

简单工厂模式：

```
工厂类负责创建的对象较少，操作时只需知道传入工厂类的参数即可，对于如何创建对象过程不用关心。
```

工厂方法模式：

满足以下条件时，可以考虑使用工厂模式方法

当一个类不知道它所必须创建对象的类时
一个类希望由子类来指定它所创建的对象时
当类将创建对象的职责委托给多个帮助子类中得某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时
抽象工厂模式：

满足以下条件时，可以考虑使用抽象工厂模式

系统不依赖于产品类实例如何被创建，组合和表达的细节。
系统的产品有多于一个的产品族，而系统只消费其中某一族的产品
同属于同一个产品族是在一起使用的。这一约束必须在系统的设计中体现出来。
系统提供一个产品类的库，所有产品以同样的接口出现，从而使客户端不依赖于实现。
以上几种，归根结底，都是将重复的东西提取出来，以方便整体解耦和复用，修改时方便。可根据具体需求而选择使用。

[本文转摘自：青叶的PHP设计模式之工厂模式](https://segmentfault.com/a/1190000007473294)
