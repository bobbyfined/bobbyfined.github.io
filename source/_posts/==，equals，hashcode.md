---
title: ==，equals，hashcode
date: 2023-08-13 14:14:36
tags: 基础
categories: java
---

# ==比较的是值是否相等

 <!-- more -->

如果作用于基本数据类型的变量，则直接比较其存储的 值是否相等，

如果作用于引用类型的变量，则比较的是所指向的对象的地址是否相等。

**equals比较的是是否是同一个对象**

equals()方法不能作用于基本数据类型的变量

equals()方法存在于Object类中，在没有重写equals()方法的类中，调用equals()方法其实和使用==的效果一样，也是比较的是引用类型的变量所指向的对象的地址，不过，Java提供的类中，有些类都重写了equals()方法，重写后的equals()方法一般都是比较两个对象的值，比如String类。

String的equals源码：

```
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

比较的是值

所以示例：

```
int x = 10;
int y = 10;
String str1 = new String("abc");
String str2 = new String("abc");
System.out.println(x == y); // true
System.out.println(str1 == str2); // false
System.out.println(str1.equals(str2)); // true
```

hashCode()方法和equals()方法的作用其实是一样的，在Java里都是用来对比两个对象是否相等一致。

但是hashCode()并不是完全可靠，**有时候不同的对象他们生成的hashcode也会一样**（生成hash值得公式可能存在的问题），所以hashCode()只能说是大部分时候可靠，并不是绝对可靠

所以有：

**1.equals()相等的两个对象他们的hashCode()肯定相等，也就是用equals()对比是绝对可靠的。**

**2.hashCode()相等的两个对象他们的equal()不一定相等，也就是hashCode()不是绝对可靠的**

![img](/iamges/==，equals，hashcode/1.png)

选B。

A、D选项是对字符串内容的比较。**JVM为了减少字符串对象的重复创建，其维护了一个特殊的内存，这段内存被成为字符串常量池。**代码中出现字面量形式创建字符串对象时，JVM首先会对这个字面量进行检查，如果字符串常量池中存在相同内容的字符串对象的引用，则将这个引用返回，否则新的字符串对象被创建，然后将这个引用放入字符串常量池，并返回该引用。所以返回true。

C选项是引用地址的比较，同上也属于常量池的同一个字符串地址，所以相等返回true。
