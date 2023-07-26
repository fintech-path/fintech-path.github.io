---
title: java 9 新特性
date: 2023-07-26 11:10:00
toc: true
tags:
    - java
categories:
    - java
---

<!-- toc -->

一直使用 JAVA8，用了好久好久了，最近突然有想法，能不能把后边所有版本的新特性总结一下呢？  
说干就干，今天第一篇 JAVA9 新特性。

### 一、钻石操作符支持匿名内部类
实际上可以更简洁地初始化泛型类型，但是，之前已经存在 Map.of 和 List.of/Arrays.asList，再写个这个意义何在？
```java
import java.util.*;

/**
 * Java 9 中的钻石操作符支持匿名内部类，实际上可以更简洁地初始化泛型类型
 * 但是，之前已经存在 Map.of 和 List.of/Arrays.asList，再写个这个意义何在？
 */
public class DiamondOperatorExample {

    public static void main(String[] args) {
        // JDK 8 中是new HashMap<String, Integer>
        Map<String, Integer> map = new HashMap<>() {{
            put("One", 1);
            put("Two", 2);
            put("Three", 3);
        }};

        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        // JDK 8 中是new ArrayList<String>
        List<String> names = new ArrayList<>(){{
            add("Alice");
            add("Bob");
            add("Charlie");
        }};

        for (String name : names) {
            System.out.println(name);
        }
    }
}

```
<!--more-->
### 二、stream 增强
1. takeWhile是遇到第一个不符合的元素时停止，即使后边仍然有满足的元素，并返回前面的。
2. dropWhile是遇到第一个不符合的元素时停止，丢弃前面所有满足的元素，返回后边的元素。
3. ofNullable() 方法允许我们创建一个包含单个元素的 Stream，如果传入的元素是非空的，则创建一个包含该元素的 Stream；如果传入的元素是空值（null），则创建一个空的 Stream。这对于处理可能为空的元素很有用，可以避免空指针异常。
4. iterator() 方法的重载， list.spliterator()以及StreamSupport.stream结合使用返回 stream。
5. Optional 和 Stream 的结合，实际上可以类似于 List.stream()方法，同样使用optional.stream()。
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;
import java.util.stream.Stream;
import java.util.stream.StreamSupport;

/**
 * JAVA9 stream 增强
 * 1. takeWhile是遇到第一个不符合的元素时停止，即使后边仍然有满足的元素，并返回前面的
 * 2. dropWhile是遇到第一个不符合的元素时停止，丢弃前面所有满足的元素，返回后边的元素
 * 3. ofNullable() 方法允许我们创建一个包含单个元素的 Stream，如果传入的元素是非空的
 * 则创建一个包含该元素的 Stream；如果传入的元素是空值（null）
 * 则创建一个空的 Stream。这对于处理可能为空的元素很有用，可以避免空指针异常。
 * 4. iterator() 方法的重载， list.spliterator()以及StreamSupport.stream结合使用返回 stream
 * 5. Optional 和 Stream 的结合，实际上可以类似于 List.stream()方法，同样使用optional.stream()
 *
 */
public class StreamAPIExample {

    public static void main(String[] args) {
        System.out.println("====takeWhile======");
        // takeWhile是遇到第一个不符合的元素时停止，即使后边仍然有满足的元素，并返回前面的
        Stream<Integer> stream1 = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 4);
        Stream<Integer> filteredStream1 = stream1.takeWhile(n -> n < 5);
        filteredStream1.forEach(System.out::println);

        System.out.println("=====dropWhile=====");
        // dropWhile是遇到第一个不符合的元素时停止，丢弃前面所有满足的元素，返回后边的元素
        Stream<Integer> stream2 = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 4);
        Stream<Integer> filteredStream2 = stream2.dropWhile(n -> n < 5);
        filteredStream2.forEach(System.out::println);

        System.out.println("=====ofNullable=====");
        Stream<String> stream3 = Stream.ofNullable("Hello");
        stream3.forEach(System.out::println);

        Stream<String> stream4 = Stream.ofNullable(null);
        stream4.forEach(System.out::println);

        System.out.println("=====iterator() 方法的重载=====");
        List<String> list = new ArrayList<>();
        list.add("Java");
        list.add("C++");
        list.add("Python");

        Stream<String> stream = StreamSupport.stream(list.spliterator(), false);
        stream.forEach(System.out::println);

        System.out.println("===== Optional 和 Stream 之间的结合进=====");
        Optional<String> optional = Optional.of("Hello");
        optional.stream()
                .map(String::toUpperCase)
                .forEach(System.out::println);
    }
}

```
### 三、新增私有接口方法（private interface method）
MyInterface.java测试接口类
```java
/**
 * JAVA9 新增私有接口方法（private interface method）
 */
public interface MyInterface {
    // 公共抽象方法
    void publicMethod();

    // 默认方法
    default void defaultMethod() {
        // 调用私有接口方法
        privateMethod();
        System.out.println("This is a default method.");
    }

    // 私有接口方法
    private void privateMethod() {
        System.out.println("This is a private method.");
    }
}
```
MyClass.java，测试接口实现类
```java
public class MyClass implements MyInterface {
    @Override
    public void publicMethod() {
        System.out.println("This is a public method.");
    }

    public static void main(String[] args) {
        MyClass myObj = new MyClass();
        myObj.publicMethod();   // 调用公共抽象方法
        myObj.defaultMethod();  // 调用默认方法
    }
}
```
### 四、新增Reactive Streams API
Data.java测试实体类
```java
public class Data {
    private int id;
    private String name;

    public Data(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    @Override
    public String toString() {
        return "Data{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```
DataProcessor.java，消息订阅类
```java
/**
 * JAVA9 新增Reactive Streams API
 */
public class DataProcessor implements Subscriber<Data> {

    private Subscription subscription;

    public static void main(String[] args) throws Exception {
        // 创建数据源，例如从数据库中获取数据
        DataPublisher dataPublisher = new DataPublisher();
        // 创建数据处理器
        DataProcessor dataProcessor = new DataProcessor();
        // 连接数据源和数据处理器
        dataPublisher.subscribe(dataProcessor);
        // 发布数据
        dataPublisher.publishData(new Data(1, "Zhangsan"));
    }

    @Override
    public void onSubscribe(Subscription subscription) {
        this.subscription = subscription;
        this.subscription.request(1); // 请求一个数据项
    }

    @Override
    public void onNext(Data data) {
        // 处理接收到的数据
        System.out.println("Received data: " + data);
        // 请求下一个数据项
        subscription.request(1);
    }

    @Override
    public void onError(Throwable throwable) {
        // 处理错误情况
        throwable.printStackTrace();
    }

    @Override
    public void onComplete() {
        // 数据处理完成
        System.out.println("Data processing completed.");
    }
}
```
DataPublisher.java，消息发布类
```java
import org.reactivestreams.Publisher;
import org.reactivestreams.Subscriber;
import java.util.ArrayList;
import java.util.List;

public class DataPublisher implements Publisher<Data> {

    private List<Subscriber<? super Data>> subscribers;

    public DataPublisher() {
        this.subscribers = new ArrayList<>();
    }

    @Override
    public void subscribe(Subscriber<? super Data> subscriber) {
        subscribers.add(subscriber);
    }

    public void publishData(Data data) {
        for (Subscriber<? super Data> subscriber : subscribers) {
            subscriber.onNext(data);
        }
    }

    public void complete() {
        for (Subscriber<? super Data> subscriber : subscribers) {
            subscriber.onComplete();
        }
    }

    public void error(Throwable throwable) {
        for (Subscriber<? super Data> subscriber : subscribers) {
            subscriber.onError(throwable);
        }
    }
}
```
### 五、允许在 try-with-resources 语句中使用资源的定义和初始化
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

/**
 * Java 9 允许在 try-with-resources 语句中使用资源的定义和初始化
 */
public class TryWithResourcesExample {

    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("TO_MODIFIED_DOC_PATH"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```
### 六、总结
以上就是开发者可以使用的JAVA9 新特性了，希望能够帮助到大家，如果有什么问题，直接联系我们。