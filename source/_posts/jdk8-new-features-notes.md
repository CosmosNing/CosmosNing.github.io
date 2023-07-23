---
title: jdk8-new-features-notes
toc: true
mathjax: true
permalink: jdk8-new-features-notes/
date: 2023-07-23 22:51:43
description: 整理 Java 8 新特性知识点
tags:
- Java
- Lambda
- 函数式接口
- Stream API
- Optional
- 接口默认方法
- 接口静态方法
categories:
- 笔记
---

## Lambda 表达式

* 一般格式：

```Java
(参数列表)->{方法体}
```

* 作用：相当于一个匿名函数，可用于函数参数传递等
* 注意事项：
  * 若参数列表只有一个参数，那么`()`可省略
  * 参数列表中的参数类型一般可省略不写，编译器能够依据上下文进行推断
  * 若方法体只有一句话，那么`{}`可省略

## 函数式接口

### 什么是函数式接口？

* 仅有一个方法的接口，叫做函数式接口

### 四大内置函数接口

#### Consumer

* 抽象方法

```Java
void accept(T t);
```

对一个类型为`T`的参数`t`执行相应的操作，**无返回值**（与`Fuction`函数接口区别）。

#### Supplier

* 抽象方法

```Java
T get();
```

返回一个类型为`T`的对象。

#### Function

* 抽象方法

```Java
R apply(T t);
```

对一个类型为`T`的参数`t`执行相应的操作，**返回类型为`R`的对象**（与`Consumer`函数接口区别）。

#### Predicate

* 抽象方法

```Java
boolean test(T t);
```

对一个类型为`T`的参数`t`执行判断，返回`boolean`类型变量。

### 应用

比如可以将上述函数式接口作为方法（函数）的参数，抽象实现功能方法；再结合Lambda表达式完成具体方法。

## 方法引用与构造器引用

### 方法引用

* 一般格式：
  * `类::静态方法名`
  * `实例::实例方法名`
  * `类::实例方法名`
    * 当Lambda表达式参数列表的第一个参数是实例方法的调用者，第二个是实例方法的参数，可使用该写法。

### 构造器引用

* 一般格式
  * `类名::new`
* 注意
  * 具体调用哪一个构造函数，应与Lambda表达式结合Java代码上下文推断确定

## Stream API

### 创建流

* 有限流

```Java
集合.stream();//获取集合流
Arrays.stream();// 获取数组流
Stream.of(集合参数列表);
```

* 无限流

```Java
stream.generate(Supplier接口);
stream.iterate(起始值, Function接口);
```

* 并行流

```Java
集合.parallelStream();
集合.stream().parallel();
Stream.of(集合).parallel();
```

P.S.并行流底层实现的原理是**ForkJoinPool**并行处理框架

### 中间操作

#### 筛选与切片

* filter

```Java
流.filter(Predicate接口);
```

按照Predicate接口中的判断情况过滤数据流中的元素。

* limit

```Java
流.limit(n);
```

取数据流中的前n项数据。

* skip

```Java
流.skip(n);
```

跳过数据流中的n个元素。

* distinct

```Java
流.distinct(n);
```

依据数据流中数据的hashCode()以及equals()判重，去除重复元素。

#### 映射
* map

```Java
流.map(Function接口);
```

将数据流中的的数据按照Function接口的规则，映射到新的流。

* flatMap

```Java
流.flatMap(Function接口);
```

与`map`类似，但是把所有得到的流连接成一个流。

#### 排序
* sorted
```Java
流.sorted();// 自然排序(依据类中对Comparable接口的compareTo的实现排序)
流.sorted(Comparator);// 指定排序
```

按照排序规则排序数据流。

### 终止操作

#### 查找与匹配
|   方法名    |   参数说明    | 返回值        |       功能       |
| :---------: | :-----------: | ------------- | :--------------: |
| `allMatch`  | Predicate接口 | 布尔          | 是否匹配所有元素 |
| `anyMatch`  | Predicate接口 | 布尔          | 是否至少匹配一个 |
| `noneMatch` | Predicate接口 | 布尔          |   是否没有匹配   |
| `findFirst` |      无       | Optional\<T\> |  返回第一个元素  |
|  `findAny`  |      无       | Optional\<T\> |   返回任意元素   |
|   `count`   |      无       | Long          |     返回个数     |
|    `max`    |  Comparator   | Optional\<T\> |    返回最大值    |
|    `min`    |  Comparator   | Optional\<T\> |    返回最小值    |

#### 规约与收集

* reduce

```Java
流.reduce(初始值, BinaryOperator);
流.reduce(BinaryOperator);
```

按照`BinaryOperator`接口实现，对流中的数据进行汇总运算。

* collect

常见用法如下：

1. 转换为其他集合
   * 转换为通用集合
     * `流.collect(Collectors.to集合)`
   * 转换为指定集合
     * `流.collect(Collectors.toCollection(XXX集合::new))` 
2. 分组（按照某一值，注意与分区区别）
   * 单级分组
     * `流.collect(Collectors.groupingBy(Function接口))`
   * 多级分组
     * `流.collect(Collectors.groupingBy(Function接口,Collectors.groupingBy(Function接口)))`
3. 分区（按照某一判断条件，注意与分组区别）
   * `流.collect(Collectors.partitioningBy(Predicate接口))`按照Predicate接口规则进行分区
4. 聚集函数运算
   * `流.collect(Collectors.聚集函数)`，其中常见聚集函数如下：
     * `counting`
     * `averaging`
     * `summming`
     * `maxBy`
     * `minBy`
   * `流.collect(Collectors.summarizing)`获取汇总数据，可通过这个对象的方法获取最大、最小、平均值等参数
   * `流.collect(Collectors.joining)` 对字符串进行连接操作（类似`String.join`）



## Optional 类

### 作用

用于封装对象，可减少空指针异常。

### 常见方法

|               方法               |                             含义                             |
| :------------------------------: | :----------------------------------------------------------: |
|        `Optional.of(T t)`        |                       创建Optional对象                       |
|        `Optional.empty()`        |                     创建空的Optional对象                     |
|    `Optional.ofNullable(T t)`    |   若t为空则创建空的Optional对象；否则依据t创建Optional对象   |
|          `isPresent()`           |                        判断是否包含值                        |
|      `Optional.orElse(T t)`      |   返回Optional包裹的对象；或者当该对象为空时，返回默认值t    |
| `Optional.orElseGet(Supplier s)` | 返回Optional包裹的对象；或者当该对象为空时，按照Supplier接口生成默认值返回 |
|    `Optional.map(Function f)`    | 返回按照f处理后的Optional对象；或者当Optional对象为空时，返回Optional.empty() |
|  `Optional.flatMap(Function f)`  |               与map类似，但是返回值类型有区别                |

## 接口默认方法与静态方法

### 接口默认方法

* 接口中可编写默认方法，用 `default` 关键字标识。

注意事项：

* “类优先”：若父类有与父接口默认方法同名的方法，子类继承后默认调用父类中的实现。
* “接口覆写”：若父接口中包含多个同名的默认方法，子类实现时需要自行指定实现。

### 接口静态方法

* 接口中可编写静态方法，与类相似。

## 参考

* [【Java】Java8新特性-Lambda表达式-Stream API等_尚硅谷__李贺飞](https://www.bilibili.com/video/BV1ut411g7E9/)

* [谨慎使用 Java8 新特性 parallelStream](https://zhuanlan.zhihu.com/p/338900757)
* [拥抱 Java 8 并行流：执行速度飞起 ！](https://zhuanlan.zhihu.com/p/339472446)