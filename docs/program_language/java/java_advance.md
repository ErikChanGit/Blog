# 高级用法

## CallBack

- 定义一个接口作为 callback

```JavaScript
public interface CallbackWash {
  void wash();
}
```

- 实现 callback

```JavaScript
// 实现再煮完之后要洗碗
static void cooking(String food, CallbackWash callback) {
  try {
      System.out.println("Cooking "+food);
      Boolean result = future.get();
      System.out.println("result "+result);
      callback.wash();
    } catch (Exception e) {
      e.printStackTrace();
    }
}
```

- 调用 callback

```JavaScript
// 具体实现再调用的时候决定， 实现之前可以获取之前的结果作为参数
cooking("egg", new CallbackWash() {
      @Override
      public void wash() {
        System.out.println("Wash bowl...");
      }
});
```

## Try-with-resource

try-catch-finally 需要自己关闭资源，当打开了大量资源时，将会导致需要一个个关闭，使得代码量庞大。

try-with-resource 可以自动关闭打开的资源。

**用法：**

```JavaScript
try(
  InputStream fis = new FileInputStream(sourceFile);
            BufferedInputStream bis = new BufferedInputStream(fis, bufferSize);
            ) {
            int len = bis.available();
            b = new byte[len];
            bis.read(b, 0, len);
)
```

# List 进阶

## List 与 Array 互转

- List 转 Array：

  ```java
  List<String> args = new ArrayListList<String>();
  String[] arrays = args.asArray(new String[args.size()]);
  ```

-  Array 转 List

  ```Java
  String[] arrays = {};
  List<String> args = Arrays.asList(arrays);
  ```

## List.stream()

### 示例

以计算最近的餐馆为例，创建一个餐馆 Restaurant 类

```java
public class Restaurant {
    String  name;
    // 距离，单位:米
    Integer distance;
    // 销量，月售
    Integer sales;
    // 价格，这里简单起见就写一个级别代表价格段
    Integer priceLevel;
    public Property(String name, int distance, int sales, int priceLevel) {
        this.name = name;
        this.distance = distance;
        this.sales = sales;
        this.priceLevel = priceLevel;
    }
    // getter setter 省略
}

public static void main(String[] args) {
    Restaurant p1 = new Restaurant("叫了个鸡", 1000, 500, 2);
    Restaurant p2 = new Restaurant("张三丰饺子馆", 2300, 1500, 3);
    Restaurant p3 = new Restaurant("永和大王", 580, 3000, 1);
    Restaurant p4 = new Restaurant("肯德基", 6000, 200, 4);
    List<Restaurant> restaurants = Arrays.asList(p1, p2, p3, p4);
    Collections.sort(restaurants, (x, y) -> x.distance.compareTo(y.distance));
    String name = properties.get(0).name;
    System.out.println("距离我最近的店铺是:" + name);
}
```

### 用法

- 过滤 filter

  ```java
  // 获取距离小于 1 公里的餐馆
  restaurants.stream().filter((p) -> p.getDistance() < 1000).forEach(
      System.out.println(p.getName());
  );
  ```

- limit 截取、skip 跳过、distinct 消除重复

  ```
  // 获取距离小于 1 公里的前2个餐馆
  restaurants.stream().filter((p) -> p.getDistance() < 1000).limit(2).forEach(
      System.out.println(p.getName());
  );
  ```

- 映射 map

  接收Lambda，将元素转换成其他形式或提取信息。接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素

  ```java
  restaurants.stream().map((p) -> p.name) // 列出所有店铺名称
  ```

- 排序 sorted

  ```java
  // 根据距离从小到大排序
  sorted = restaurants.stream().sorted((p1, p2) -> {
  	return p1.getDistance().compareTo(p2.getDistance());
  });
  sorted.forEach();
  ```

- 统计总和、最大、最小数

  ```
  restaurants.stream().max( Comparator.comparingInt(p -> p.priceLevel)).get();
  ```

- 获取结果

  ```
  restaurants.stream().max()..collect(Collectors.toList());
  ```

- Stream 并行流

  ```java
  // parallelStream
  restaurants.parallelStream().max()..collect(Collectors.toList());
  ```

- 返回判断结果

  ```java
  restaurants.stream().anymatch( item ->{ // allMatch、 noneMatch
  	return item....
  });
  ```

  