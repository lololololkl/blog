---
tags: [Spring]
title: IOC创建对象的方法
categories: 
- Spring
date: 2022-01-20 17:09:15
---

### 无参构造
```xml
<bean id="user" class="com.example.demo.bean.User"></bean>
```
### 有参构造
* 下标构造
```xml
<bean id="user" class="com.kuang.pojo.user">
    <constructor-arg index="0" value="值"/>
</bean>
```
* 类型(不建议使用)
```xml
<bean id="user" class="com.kuang.pojo.user">
    <constructor-arg type="java.lang.String" value="值"/>
</bean>
```
* 直接通过参数名
```xml
<bean id="user" class="com.kuang.pojo.user">
    <constructor-arg name="name" value="值"/>
</bean>
```
