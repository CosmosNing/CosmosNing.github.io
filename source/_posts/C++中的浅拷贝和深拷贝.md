---
title: C++中的浅拷贝和深拷贝
toc: true
mathjax: false
date: 2020-05-18 09:18:20
description: 介绍浅拷贝和深拷贝的含义以及在 C++ 中的实现
tags:
- C++
- 浅拷贝
- 深拷贝
categories:
- C++
---

> （导读）短文一篇。主要介绍浅拷贝和深拷贝的含义以及在 C++ 中的实现

![封面图片](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/05/cristian-lozan-lb5yppoIEWk-unsplash.jpg)

Photo by [Cristian Lozan](https://unsplash.com/@chrisslozan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/t/nature?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 问题引入

先来看一个例子

```C++
class ShallowCopyObject
{
public:
	int basicType;
	int *refType;
	ShallowCopyObject()
	{
		basicType = 10;
		refType = new int[10];
		for (int i = 0; i < 10; i++)
		{
			refType[i] = i;
		}
	}
};
```

我定义了一个 `ShallowCopyObject` 类，它包括两个成员变量

1. `basicType` ：类型为语言内置基本类型的 `int`
2. `refType` ：类型为指针，指向 `int` 类型数据

与此同时，我还定义了一个无参构造函数，用于初始化 `basicType` 和 `refType` 的值。其中 `basicType` 初始化为 10；`refType` 初始化为一个大小为 10 的数组，依次存储 0~9 的整数。

## 问题一

假设主函数 `main` 中有如下语句，那么终端会输出什么呢？

```C++
ShallowCopyObject a;
ShallowCopyObject b = a;

cout << "Before changing value of b.basicType" << endl;
cout << "b.basicType " << b.basicType << endl;
b.basicType = 233;
cout << "After changing value of b.basicType" << endl;
cout << "a.basicType " << a.basicType << endl;
cout << "b.basicType " << b.basicType << endl;
```

先自己分析一下，再上机敲一敲，看看自己的思路对不对。

## 问题二

类似的，下面的语句会输出什么呢？

```C++
ShallowCopyObject a;
ShallowCopyObject b = a;

cout << "Before changing value of b.refType[6]" << endl;
cout << "b.refType[6] " << b.refType[6] << endl;
b.refType[6] = 666;
cout << "After b.refType[6]" << endl;
cout << "a.refType[6] " << a.refType[6] << endl;
cout << "b.refType[6] " << b.refType[6] << endl;
```

再试着自己分析一下，上机敲一敲，看看自己的思路对不对。

# 输出结果

```C++
ShallowCopyObject a;
ShallowCopyObject b = a;

cout << "Before changing value of b.basicType" << endl;
cout << "b.basicType " << b.basicType << endl;
b.basicType = 233;
cout << "After changing value of b.basicType" << endl;
cout << "a.basicType " << a.basicType << endl;
cout << "b.basicType " << b.basicType << endl;
```

第一个问题，也就是上面的代码，会输出如下结果

{% note default %}

Before changing value of b.basicType
b.basicType 10
After changing value of b.basicType
a.basicType 10
b.basicType 233

{% endnote %}

由此可见，**两个对象基本数据类型的成员变量是相互独立的，不会相互影响**

```C++
ShallowCopyObject a;
ShallowCopyObject b = a;

cout << "Before changing value of b.refType[6]" << endl;
cout << "b.refType[6] " << b.refType[6] << endl;
b.refType[6] = 666;
cout << "After b.refType[6]" << endl;
cout << "a.refType[6] " << a.refType[6] << endl;
cout << "b.refType[6] " << b.refType[6] << endl;
```

而第二个问题，它的结果可能出乎意料：

{% note default %}

Before changing value of b.refType[6]
b.refType[6] 6
After b.refType[6]
a.refType[6] 666
b.refType[6] 666

{% endnote %}

我们惊奇的发现，对 `b` 对象的成员变量修改，竟然影响到了 `a` 对象中的值！

这一切的原因，都是由于在C++ 中，默认对象之间的拷贝（包括默认复制构造函数和默认赋值语句）是浅拷贝。


# 什么是浅拷贝？

那么，什么是浅拷贝呢？

这里给出维基百科的定义：

> One method of copying an object is the shallow copy. In that case a new object B is created, and the fields values of A are copied over to B. This is also known as a field-by-field copy, field-for-field copy, or field copy. If the field value is a reference to an object (e.g., a memory address) it copies the reference, hence referring to the same object as A does, and if the field value is a primitive type it copies the value of the primitive type. In languages without primitive types (where everything is an object), all fields of the copy B are references to the same objects as the fields of original A. The referenced objects are thus shared, so if one of these objects is modified (from A or B), the change is visible in the other. Shallow copies are simple and typically cheap, as they can be usually implemented by simply copying the bits exactly.
>
> (https://en.wikipedia.org/wiki/Object_copying#Shallow_copy)

简单的来说，浅拷贝就是逐个字节的拷贝。也就是说，拷贝后每一个成员变量的值都相同。如果该值是基本数据类型，那么该值被拷贝；如果该值是引用数据类型（如对象、指针等），那么该值（注意：这里的值是指**地址**）也会被拷贝。

由此可知，针对第一个问题，由于改变的是基本类型的数据，它是独立的一份拷贝，因而另一个对象值的修改并不会影响被拷贝的对象；然而，在第二个问题中，由于浅拷贝，`b` 中 `refType` 指向了和 `a` 对象 `refType` 相同的位置（因为拷贝了地址），因而在 `b` 中修改 `refType` 数组中的值，会影响到对象 `a` 。

{% note info %}

## 一句话描述

**浅拷贝会共享引用数据类型成员变量（指针指向同一个地址），而不共享原始数据类型的成员变量**

{% endnote %}

# 什么是深拷贝？

有时候，我们并不希望拷贝对象时，其引用成员变量指向同一个引用数据类型的数据对象，而希望它们指向不同的位置，但是这些位置存储的值是相同的。这就需要用到深拷贝。

在维基百科中，深拷贝是这样定义的：

> An alternative is a deep copy, meaning that fields are dereferenced: rather than references to objects being copied, new copy objects are created for any referenced objects, and references to these placed in B. The result is different from the result a shallow copy gives in that the objects referenced by the copy B are distinct from those referenced by A, and independent. Deep copies are more expensive, due to needing to create additional objects, and can be substantially more complicated, due to references possibly forming a complicated graph.
>
> (https://en.wikipedia.org/wiki/Object_copying#Deep_copy)

{% note info %}

## 一句话描述

**深拷贝不会共享引用数据类型成员变量（它们的指针指向不同地址，但是拷贝后指针指向地址所存储的值是相等的），也不共享原始数据类型的成员变量**

{% endnote %}

## 实现深拷贝

在 C++ 中可以自定义复制构造函数、重载赋值运算符，实现深拷贝。对于本文开头提出的问题，可以做如下改进。

```C++
// 自定义复制构造函数
DeepCopyObject(const DeepCopyObject &obj)
{
    basicType = obj.basicType;
    refType = new int[10];        // 引用类型成员变量重新申请空间
    for (int i = 0; i < 10; i++)  // 将值逐个拷贝到新申请的空间中
    {
        refType[i] = obj.refType[i];
    }
}
```

```C++
// 重载赋值运算符
DeepCopyObject &operator=(const DeepCopyObject &obj)
{
    basicType = obj.basicType;
    if (this == &obj) // obj = obj; 情况
        return *this;
    delete[] refType;
    refType = new int[10];
    for (int i = 0; i < 10; i++)
    {
        refType[i] = obj.refType[i];
    }

    return *this;
}
```



# 总结

* 浅拷贝会**共享**引用数据类型成员变量（指针指向**同一个地址**），而**不共享**原始数据类型的成员变量
* 深拷贝**不会共享**引用数据类型成员变量（它们的指针指向**不同**地址，但是拷贝后指针指向地址所存储的**值是相等的**），也**不共享**原始数据类型的成员变量
* 在 C++ 中可以**自定义复制构造函数**、**重载赋值运算符**，**实现深拷贝**

# 附：实现深拷贝完整代码

```C++
#include <iostream>

using namespace std;

class DeepCopyObject
{
public:
	int basicType;
	int *refType;

	DeepCopyObject()
	{
		basicType = 10;
		refType = new int[10];
		for (int i = 0; i < 10; i++)
		{
			refType[i] = i;
		}
	}

	DeepCopyObject(const DeepCopyObject &obj)
	{
		basicType = obj.basicType;
		refType = new int[10];
		for (int i = 0; i < 10; i++)
		{
			refType[i] = obj.refType[i];
		}
	}

	~DeepCopyObject()
	{
		if (refType)
			delete[] refType;
	}

	DeepCopyObject &operator=(const DeepCopyObject &obj)
	{
		basicType = obj.basicType;
		if (this == &obj) // obj = obj; 情况
			return *this;
		delete[] refType;
		refType = new int[10];
		for (int i = 0; i < 10; i++)
		{
			refType[i] = obj.refType[i];
		}

		return *this;
	}
};

int main()
{

	DeepCopyObject a;
	DeepCopyObject b = a; // 调用 DeepCopyObject(const DeepCopyObject & obj)

	// b = a; // 调用 DeepCopyObject & operator=(const DeepCopyObject & obj)

	cout << "Before changing value of b.basicType" << endl;
	cout << "b.basicType " << b.basicType << endl;
	b.basicType = 233;
	cout << "After changing value of b.basicType" << endl;
	cout << "a.basicType " << a.basicType << endl;
	cout << "b.basicType " << b.basicType << endl;

	cout << "Before changing value of b.refType[6]" << endl;
	cout << "b.refType[6] " << b.refType[6] << endl;
	b.refType[6] = 666;
	cout << "After b.refType[6]" << endl;
	cout << "a.refType[6] " << a.refType[6] << endl;
	cout << "b.refType[6] " << b.refType[6] << endl;

	return 0;
}
```





