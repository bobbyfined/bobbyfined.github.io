---
layout: title
title: Mockito单元测试
date: 2023-02-15 21:32:36
tags: java
categories: java

---

# Mockito单元测试

<!-- more -->

使用Mockito可以mock对象，定义对象中代码逻辑的执行结果，mock对象对逻辑执行也只是做记录而不会真正执行，使用spy创建spy对象的话则会执行真实的代码逻辑，如果打桩就会返回定义的结果，不打桩就返回真实执行的返回结果。

## 一、api

org.mockito.Mockito是mockito提供的核心api，提供了大量的静态方法，用于帮助我们来mock对象，验证行为等等，然后需要注意的是，很多方法都被封装在了MockitoCore类里面

- mock：构建一个我们需要的对象；可以mock具体的对象，也可以mock接口。

- spy：构建监控对象

- verify：验证某种行为

- when：当执行什么操作的时候，一般配合thenXXX 一起使用。表示执行了一个操作之后产生什么效果。

- doReturn：返回什么结果

- doThrow：抛出一个指定异常

- doAnswer：做一个什么相应，需要我们自定义Answer；

- times：某个操作执行了多少次

- atLeastOnce：某个操作至少执行一次

- atLeast：某个操作至少执行指定次数

- atMost：某个操作至多执行指定次数

- atMostOnce：某个操作至多执行一次

- doNothing：不做任何处理

- doReturn：返回一个结果

- doThrow：抛出一个指定异常

- doAnswer：指定一个操作，传入Answer

- doCallRealMethod：返回真实业务执行的结果，只能用于监控对象


## 二、ArgumentMatchers

用于进行参数匹配，减少很多不必要的代码

- 
  anyInt：任何int类型的参数，类似的还有anyLong/anyByte等等。

- 
  eq：等于某个值的时候，如果是对象类型的，则看toString方法

- 
  isA：匹配某种类型

- 
  matches：使用正则表达式进行匹配


## 三、OngoingStubbing

- OngoingStubbing用于返回操作的结果。

- thenReturn：指定一个返回的值

- thenThrow：抛出一个指定异常

- then：指定一个操作，需要传入自定义Answer；

- thenCallRealMethod：返回真实业务执行的结果，只能用于监控对象。


## **mock对象**

顾名思义，就是模仿一个对象，对象的交互都会被模仿，因此可以根据自己的需求选择相应交互去验证或设置返回自己想要的结果，但是对于对象的行为只会被记录，不会实现

```
@Test
void testBehavior() {
 
    //构建moock数据
    List<String> list = mock(List.class);
    list.add("1");
    list.add("2");
 
    System.out.println(list.get(0)); // 会得到null ，前面只是在记录行为而已，没有往list中添加数据
 
    verify(list).add("1"); // 正确，因为该行为被记住了
    verify(list).add("3");//报错，因为前面没有记录这个行为
 
}
```

## **stub 打桩**

用于预先说明当执行了什么操作的时候，产生一个什么响应，一旦测试桩函数被调用，该函数将会一致返回固定的值

常用打桩函数：when 、thenReturn、then、thenThrow、thenAnswer这几个函数。

```
@Test
void testStub() {
    List<Integer> l = mock(ArrayList.class);
 
    when(l.get(0)).thenReturn(10);
    when(l.get(1)).thenReturn(20);
    when(l.get(2)).thenThrow(new RuntimeException("no such element"));
 
    assertEquals(l.get(0), 10);
    assertEquals(l.get(1), 20);
    assertNull(l.get(4));
    assertThrows(RuntimeException.class, () -> {
        int x = l.get(2);
    });
}
```

void函数存根

主要用到doThrow(), doAnswer(), doNothing(), doReturn() and doCallRealMethod()等函数，当然，这些函数也可以用于有返回值的函数的存根；需要注意的是，doCallRealMethod不能用于Mock对象，只能用于监察对象。

```
@Test
void testVoidStub(){
    List<Integer> l = mock(ArrayList.class);
    doReturn(10).when(l).get(1);
    doThrow(new RuntimeException("you cant clear this List")).when(l).clear();
 
    assertEquals(l.get(1),10);
    assertThrows(RuntimeException.class,()->l.clear());
}
```

## **spy**

spy类的原理是，如果不打桩默认都会执行真实的方法，如果打桩则返回桩实现。


对 spy 变量打桩时，如果使用 when 去设置模拟值时，他里面的代码逻辑依然会被执行，只是mock了返回结果；

```
@Test
void testBehavior() {
 
    //构建moock数据
    List<String> list = mock(List.class);
    list.add("1");
    list.add("2");
    List<String> list2 = spy(List.class);
    list2.add("1");
    list2.add("2");
 
    System.out.println(list.get(0)); // 会得到null ，前面只是在记录行为而已，没有往list中添加数据
    System.out.println(list2.get(0)); // 会得到1，执行了真实的代码逻辑
 
    verify(list).add("1"); // 正确，因为该行为被记住了
    verify(list).add("3");//报错，因为前面没有记录这个行为
 
}
```

