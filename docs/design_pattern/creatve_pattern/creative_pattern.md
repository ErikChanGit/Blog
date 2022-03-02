# 创建型模式

创建型模式的作用就是创建对象，说到创建一个对象，最熟悉的就是 new 一个对象，然后 set 相关属性。但是，在很多场景下，我们需要给客户端提供更加友好的创建对象的方式，尤其是那种我们定义了类，但是需要提供给其他开发者用的时候。

## 简单工厂模式

一个工厂类 XxxFactory，里面有一个静态方法，根据我们不同的参数，返回不同的派生自同一个父类（或实现同一接口）的实例对象。一个工厂可以生产不同的产品

```java
public class FoodFactory {

    public static Food makeFood(String name) {
        if (name.equals("noodle")) {
            Food noodle = new LanZhouNoodle();
            noodle.addSpicy("more");
            return noodle;
        } else if (name.equals("chicken")) {
            Food chicken = new HuangMenChicken();
            chicken.addCondiment("potato");
            return chicken;
        } else {
            return null;
        }
    }
}
```

在这里，我们强调**职责单一** 原则，一个类只提供一种功能，FoodFactory 的功能就是只要负责生产各种 Food。

## 工厂模式

简单工厂模式很简单，如果它能满足我们的需要，我觉得就不要折腾了。之所以需要引入工厂模式，是因为每个工厂都有可能制作相同的食物，但是制作出来的结果往往不同。 我们需要使用两个或两个以上的工厂。

定义一个用于创建对象的接口，让子类决定实例化哪一个类

```java
public interface FoodFactory {
    Food makeFood(String name);
}
public class ChineseFoodFactory implements FoodFactory {

    @Override
    public Food makeFood(String name) {
        if (name.equals("A")) {
            return new ChineseFoodA();
        } else if (name.equals("B")) {
            return new ChineseFoodB();
        } else {
            return null;
        }
    }
}
public class AmericanFoodFactory implements FoodFactory {

    @Override
    public Food makeFood(String name) {
        if (name.equals("A")) {
            return new AmericanFoodA();
        } else if (name.equals("B")) {
            return new AmericanFoodB();
        } else {
            return null;
        }
    }
}
```

**核心在于，我们需要在第一步选好我们需要的工厂** 

## 抽象工厂模式

一个工厂接口中实现多个产品，这个可以根据需要生产多个产品

对于客户端来说，不需要去选择具体产品，只需要指定一个大厂，让大厂去做

```java
public static void main(String[] args) {
  // 第一步就要选定一个“大厂”
  ComputerFactory cf = new AmdFactory();
  // 从这个大厂造 CPU
  CPU cpu = cf.makeCPU();
  // 从这个大厂造主板
  MainBoard board = cf.makeMainBoard();
   // 从这个大厂造硬盘
   HardDisk hardDisk = cf.makeHardDisk();

  // 将同一个厂子出来的 CPU、主板、硬盘组装在一起
  Computer result = new Computer(cpu, board, hardDisk);
}
```

但是如果需要添加新的产品，就需要在每个工厂中都实现这个产品

## 单例模式

调用一个对象时，永远都只会存在一个示例，保证这个对象的唯一性

将构造函数设为私有，只能在对象的静态方法中调用

```
public class Singleton {
    private Singleton() {};
    private static Singleton instance = new Singleton();

    public static Singleton getInstance() {
        return instance;
    }
    public static Date getDate(String mode) {return new Date();}
}
```

