# Java 版本特性

## Java 1.9 特性
链接 [https://blog.csdn.net/qq_41125219/article/details/82078316](https://blog.csdn.net/qq_41125219/article/details/82078316)

#### Java 模块系统

```Java
module blog {
   exports com.pluralsight.blog;
 
   requires cms;
}
```

#### 集合工厂方法

除了更短和更好阅读之外，这些方法也可以避免您选择特定的集合实现。 事实上，从工厂方法返回已放入数个元素的集合实现是高度优化的。这是可能的，因为它们是不可变的：在创建后，继续添加元素到这些集合会导致 “UnsupportedOperationException” 。

```Java
Set<Integer> ints = Set.of( 1 , 2 , 3 );
List<String> strings = List.of( "first" , "second" );
```

#### 私有接口方法

```Java
public interface MyInterface {
 
     void normalInterfaceMethod();
 
     default void interfaceMethodWithDefault() {  init(); }
 
     default void anotherDefaultMethod() { init(); }
 
     // This method is not part of the public API exposed by MyInterface
     private void init() { System.out.println( "Initializing" ); }
}
```

如果您使用默认方法开发 API ，那么私有接口方法可能有助于构建其实现。

#### 多版本兼容 JAR

当一个新版本的 Java 出现的时候，你的库用户要花费数年时间才会切换到这个新的版本。这就意味着库得去向后兼容你想要支持的最老的 Java 版本 (许多情况下就是 Java 6 或者 7)。这实际上意味着未来的很长一段时间，你都不能在库中运用 Java 9 所提供的新特性。幸运的是，多版本兼容 JAR 功能能让你创建仅在特定版本的 Java 环境中运行库程序时选择使用的 class 版本


## Java 1.8 特性
来源：[https://www.cnblogs.com/owenma/p/8600685.html](https://www.cnblogs.com/owenma/p/8600685.html)

> **接口中可含有非抽象方式实现**

```JavaScript
interface Formula {
    double calculate(int a); // 抽象方法， 一定要实现
 
    default double sqrt(int a) { // 非抽象， 可以不实现
        return Math.sqrt(a);
    }
}
```

> **Lamda 表达式**

```JavaScript
(a, b) ->{
  // TODO:执行 a,b
}
```

示例：

```JavaScript
// 排序
Collections.sort(names, (String a, String b) -> {
    return b.compareTo(a);
}); 

// 老方法
Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return b.compareTo(a);
    }
});　
```

> **函数式接口**

一个有且仅有一个抽象方法的接口，函数式接口可以被隐式转换为 lambda 表达式。

```JavaScript
@FunctionalInterface
interface GreetingService 
{
    void sayMessage(String message);
}
转换
GreetingService greetService1 = message -> System.out.println("Hello " + message); 
```

函数式接口可以对现有的函数友好地支持 lambda。

**TODO：其他非抽象函数怎么弄？**

> **方法与构造函数引用**

方法引用就是让你根据已有的方法实现来创建 Lambda表达式。

当你需要使用方法引用时，目标引用放在分隔符::前，方法的名称放在后面。

例如， Apple::getWeight就是引用了Apple类中定义的方法getWeight。

**例子：**

|||
|-|-|
|lambda|等效的方法引用|
|(Apple a) -> a.getWeight()|Apple::getWeight|
|(String s) -> System.out.println(s)|System.out::println|
|(str, i) -> str.substring(i)|String::substring|


- 静态方法引用

```JavaScript
List<String> strList = Lists.newArrayList("1", "2", "3");

List<Integer> intList1 = strList.stream().
map(item -> Integer.parseInt(item)).collect(Collectors.toList());

List<Integer> intList2 = strList.stream().
map(Integer::parseInt).collect(Collectors.toList());

```

- 任意类型示例方法引用

```JavaScript
List<String> strList = Lists.newArrayList("xing", "guo");

strList.stream().map(item -> item.length()).forEach(System.out::println);

strList.stream().map(String::length).forEach(System.out::println);

```

- 构造函数引用

  一个参数：

```JavaScript
Function<Integer, Apple> c2 = Apple::new;
Apple a2 = c2.apply(110);

//或者
Function<Integer, Apple> c2 = (weight) -> new Apple(weight);
Apple a2 = c2.apply(110);   
```

  两个参数：

```JavaScript
BiFunction<String, Integer, Apple> c3 = Apple::new;
Apple c3 = c3.apply("green", 110); 
```

  三个参数：

```JavaScript
// 需要自己创建一个函数式接口:
public interface TriFunction<T, U, V, R>{
    R apply(T t, U u, V v);
}
 
TriFunction<Integer, Integer, Integer, Color> colorFactory = Color::new; 
```

> Lamda 作用域

访问外层的局部变量

```JavaScript
final int num = 1; // num 不允许被 lamda 修改
Converter<Integer, String> stringConverter =
        (from) -> String.valueOf(from + num);

```

> Date API

**Clock 时钟**

Clock类提供了访问当前日期和时间的方法，Clock是时区敏感的，可以用来取代 System.currentTimeMillis() 来获取当前的微秒数。某一个特定的时间点也可以使用Instant类来表示，Instant类也可以用来创建老的java.util.Date对象

```JavaScript
Clock clock = Clock.systemDefaultZone();
long millis = clock.millis();
 

Instant instant = clock.instant();
Date legacyDate = Date.from(instant);   // legacy java.util.Date
```

**Timezones 时区**

在新API中时区使用ZoneId来表示。时区可以很方便的使用静态方法of来获取到。 时区定义了到UTS时间的时间差，在Instant时间点对象到本地日期对象之间转换的时候是极其重要的。

```JavaScript
System.out.println(ZoneId.getAvailableZoneIds());
// prints all available timezone ids

ZoneId zone1 = ZoneId.of("Europe/Berlin");
ZoneId zone2 = ZoneId.of("Brazil/East");
System.out.println(zone1.getRules());
System.out.println(zone2.getRules());
```

**LocalTime 本地时间**

**LocalDateTime 本地日期时间**

> **Annotation 注解**

Java 8允许我们把同一个类型的注解使用多次，只需要给该注解标注一下@Repeatable即可。

```JavaScript
@interface Hints {
    Hint[] value();
}
 
@Repeatable(Hints.class)
@interface Hint {
    String value();
}
```

## Java 1.7 特性

