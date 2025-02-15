---
layout: title
title: Mybatis的延迟加载
date: 2023-08-01 21:14:36
tags: mybatis
categories: java


---

# Mybatis的延迟加载

<!-- more -->

延迟加载其实就是将**数据加载时机推迟，比如推迟嵌套查询的执行时机**。在Mybatis中经常用到关联查询，但是并不是任何时候都需要立即返回关联查询结果。比如查询订单信息，并不一定需要及时返回订单对应的产品信息，查询商品分类信息并不一定要及时返回该类别下有哪些产品，这种情况一下需要一种机制，当需要查看时，再执行查询，返回需要的结果集，这种需求在Mybatis中可以使用延迟加载机制来实现。延迟加载可以实现先查询主表，按需实时做关联查询，返回关联表结果集，一定程度上提高了效率。

以商品类别category和商品product为例，一个类别下可以有多个商品，一个商品属于一种类别。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper 
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.sl.mapper.LazyLoadMapper">
        <!-- 分类信息查询 -->
        <select id="lazyLoadTest"  resultMap="lazyLoadProductsByCategoryTest">
            select * from category where id=#{id}
        </select>
        <resultMap id="lazyLoadProductsByCategoryTest" type="com.sl.po.Category">
            <id column="id" property="Id"></id>
            <result column="name" property="Name"></result>
            <result column="remark" property="Remark"></result>
            <!-- 一个分类对应多个产品，此处使用collection -->
            <collection property="productList" ofType="com.sl.po.Product"  column="id" select="selectProductsByCategoryId"></collection>
        </resultMap>
        
        <!-- 嵌套查询返回商品信息，延迟加载将要执行的sql -->
        <select id="selectProductsByCategoryId"  resultType="com.sl.po.Product">
            select * from products where categoryid=#{id} 
        </select>
        
</mapper>
```

**启用延迟加载和按需加载**

Mybatis配置文件中通过两个属性lazyLoadingEnabled和aggressiveLazyLoading来控制延迟加载和按需加载。

lazyLoadingEnabled：是否启用延迟加载，mybatis默认为false，不启用延迟加载。			lazyLoadingEnabled属性控制全局是否使用延迟加载，特殊关联关系也可以通过嵌套查询中fetchType	属性单独配置（fetchType属性值lazy或者eager）。

aggressiveLazyLoading：是否按需加载属性，默认值false，lazyLoadingEnabled属性启用时只要加载对象，就会加载该对象的所有属性；关闭该属性则会按需加载，即使用到某关联属性时，实时执行嵌套查询加载该属性。

SqlMapConfig.xml中修改配置，注册lazyLoadMapper.xml

![image-20250207214816018](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250207214816018.png)

![image-20250207214824392](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250207214824392.png)



此时没有运行嵌套查询，只允许了最外层的查询，因为运行到61之前只需要获取分类信息。

## **原理**

它的原理是，使用 CGLIB 创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用 a.getB().getName()，拦截器 invoke()方法发现 a.getB()是null 值，那么就会单独发送事先保存好的查询关联 B 对象的 sql，把 B 查询上来，然后调用 a.setB(b)，于是 a 的对象 b 属性就有值了，接着完成 a.getB().getName()方法的调用。这就是延迟加载的基本原理。

