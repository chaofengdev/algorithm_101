# HashMap的7种遍历方式（非原）

参考资料：https://juejin.cn/post/6844904144331866119

本文**先从 HashMap 的遍历方法讲起，然后再从性能、原理以及安全性等方面，来分析 HashMap 各种遍历方式的优势与不足**，本文主要内容如下图所示：

![image.png](https://typora-1256823886.cos.ap-nanjing.myqcloud.com//2021/171c4a0d0966394e~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

## 7种遍历方式

> HashMap **遍历从大的方向来说，可分为以下 4 类**：
>
> 1. 迭代器（Iterator）方式遍历；
> 2. For Each 方式遍历；
> 3. Lambda 表达式遍历（JDK 1.8+）;
> 4. Streams API 遍历（JDK 1.8+）。
>
> 
>
> 但每种类型下又有不同的实现方式，因此具体的遍历方式又可以分为以下 7 种：
>
> 1. 使用迭代器（Iterator）EntrySet 的方式进行遍历；
> 2. 使用迭代器（Iterator）KeySet 的方式进行遍历；
> 3. 使用 For Each EntrySet 的方式进行遍历；
> 4. 使用 For Each KeySet 的方式进行遍历；
> 5. 使用 Lambda 表达式的方式进行遍历；
> 6. 使用 Streams API 单线程的方式进行遍历；
> 7. 使用 Streams API 多线程的方式进行遍历。
>
> 
>
> 接下来我们来看每种遍历方式的具体实现代码。

### 迭代器EntrySet

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        Iterator<Map.Entry<Integer, String>> iterator = map.entrySet().iterator();
        while (iterator.hasNext()) {
            Map.Entry<Integer, String> entry = iterator.next();
            System.out.println(entry.getKey());
            System.out.println(entry.getValue());
        }
    }
}
```

### 迭代器KeySet

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        Iterator<Integer> iterator = map.keySet().iterator();
        while (iterator.hasNext()) {
            Integer key = iterator.next();
            System.out.println(key);
            System.out.println(map.get(key));
        }
    }
}
```

### ForEach EntrySet(*)

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        for (Map.Entry<Integer, String> entry : map.entrySet()) {
            System.out.println(entry.getKey());
            System.out.println(entry.getValue());
        }
    }
}
```

### ForEach KeySet

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        for (Integer key : map.keySet()) {
            System.out.println(key);
            System.out.println(map.get(key));
        }
    }
}
```

### Lambda表达式

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        map.forEach((key, value) -> {
            System.out.println(key);
            System.out.println(value);
        });
    }
}
```

### Streams API 单线程

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        map.entrySet().stream().forEach((entry) -> {
            System.out.println(entry.getKey());
            System.out.println(entry.getValue());
        });
    }
}
```

### Streams API 多线程

```java
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        map.entrySet().parallelStream().forEach((entry) -> {
            System.out.println(entry.getKey());
            System.out.println(entry.getValue());
        });
    }
}
```



## 性能测试

测试过程略；

结论是，从性能方面考虑，我们应该尽量使用 `lambda` 或者是 `entrySet` 来遍历 `Map` 集合

## 源码分析

暂略。

## 安全性测试

我们不能在遍历中使用集合 `map.remove()` 来删除数据，这是非安全的操作方式，但我们可以使用迭代器的 `iterator.remove()` 的方法来删除数据，这是安全的删除集合的方式。同样的我们也可以使用 Lambda 中的 `removeIf` 来提前删除数据，或者是使用 Stream 中的 `filter` 过滤掉要删除的数据进行循环，这样都是安全的，当然我们也可以在 `for` 循环前删除数据在遍历也是线程安全的。

