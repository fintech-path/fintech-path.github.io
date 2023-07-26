---
title: java 10 新特性
date: 2023-07-26 11:10:00
toc: true
tags:
    - java
categories:
    - java
---

<!-- toc -->

一直使用 JAVA8，用了好久好久了，最近突然有想法，能不能把后边所有版本的新特性总结一下呢？  
说干就干，今天第二篇 JAVA10 新特性。
### 一、集合新增 copyOf 方法
```java
/**
 * JAVA10 集合新增 copyOf 方法
 */
public class CopyOfTest {

    public static void main(String[] args) {
        // 通过 of 方法创建结合，均不可修改，后边均会报错
        List<String> list = List.of("apple", "banana", "orange");
        Set<Integer> set = Set.of(1, 2, 3, 4);
        Map<String, Integer> map = Map.of("one", 1, "two", 2, "three", 3);
        try {
            list.add("123");
        } catch (Exception e){
            e.printStackTrace();
        }
        try {
            set.add(5);
        } catch (Exception e){
            e.printStackTrace();
        }
        try {
            map.put("four", 4);
        } catch (Exception e){
            e.printStackTrace();
        }
        // copyOf方法，用于创建一个不可变集合的副本
        List<String> originalList = new ArrayList<>();
        originalList.add("apple");
        originalList.add("banana");
        originalList.add("orange");

        List<String> copyList = List.copyOf(originalList);

        // forEach方法的增强
        List<Integer> numbers = List.of(1, 2, 3, 4, 5);
        numbers.parallelStream().forEach(System.out::println);
    }
}

```
<!--more-->
### 二、var 关键字(局部变量类型推断)
新增 var 关键字，只能用于局部变量，不能用于方法的参数、方法返回值、类的字段等。
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

/**
 *  JAVA10 新增 var 关键字，只能用于局部变量，不能用于方法的参数、方法返回值、类的字段等。
 */
public class VarTest {
    public static void main(String[] args) {
        System.out.println("=======");
        var number = 10;
        var pi = 3.14;
        System.out.println("number:" + number +";pi=" + pi);
        System.out.println("=======");
        var message = "Hello, World!";
        var list = new ArrayList<String>();
        list.add("123");
        list.add("234");
        System.out.println("message:" + message);
        list.forEach(System.out::println);
        System.out.println("=======");
        var numbers = List.of(1, 2, 3, 4, 5);
        var map = new HashMap<Integer, String>();
        map.put(123, "123");
        numbers.forEach(System.out::println);
        map.forEach((key, value) -> System.out.println(map.get(key)));
    }
}

```
### 三、总结
以上就是开发者可以使用的JAVA 10 新特性了， 因为是个过渡版本，所以实际新增的特性并不多，希望能够帮助到大家，如果有什么问题，直接联系我们。