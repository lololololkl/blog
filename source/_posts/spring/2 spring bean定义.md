---
tags: [Spring]
title: Spring Bean 定义
categories: 
- Spring
date: 2021-06-06 11:09:15
---

### spring 定义
    通常情况下，Spring 的配置文件使用 XML 格式。
    Spring 配置文件支持两种格式，即 XML 文件格式和 Properties 文件格式。
    Properties 配置文件主要以 key-value 键值对的形式存在，只能赋值，不能进行其他操作，适用于简单的属性配置。
    XML 配置文件是树形结构，相对于 Properties 文件来说更加灵活。XML 配置文件结构清晰，但是内容比较繁琐，适用于大型复杂的项目。

* id	
    
    Bean 的唯一标识符，Spring 容器对 Bean 的配置和管理都通过该属性完成。id 的值必须以字母开始，可以使用字母、数字、下划线等符号。
* name	

<!-- more -->
    name 属性中可以为 Bean 指定多个名称，每个名称之间用逗号或分号隔开。Spring 容器可以通过 name 属性配置和管理容器中的 Bean。
* class	
    
    该属性指定了 Bean 的具体实现类，它必须是一个完整的类名，即类的全限定名。
* scope 	

    用于设定 Bean 实例的作用域，属性值可以为 singleton（单例）、prototype（原型）、request、session 和 global Session。其默认值是 singleton
* constructor-arg	

    `<bean>`元素的子元素，可以使用此元素传入构造参数进行实例化。该元素的 index 属性指定构造参数的序号（从 0 开始），type 属性指定构造参数的类型
* property	

    `<bean>`元素的子元素，用于调用 Bean 实例中的 setter 方法来属性赋值，从而完成依赖注入。该元素的 name 属性用于指定 Bean 实例中相应的属性名
* ref	

    `<property>` 和 `<constructor-arg>` 等元素的子元索，该元素中的 bean 属性用于指定对某个 Bean 实例的引用
* value	

    `<property>` 和 `<constractor-arg>` 等元素的子元素，用于直接指定一个常量值
* list	

    用于封装 List 或数组类型的依赖注入
* set	 
        
    用于封装 Set 类型的依赖注入
* map	

    用于封装 Map 类型的依赖注入
* entry	
    
    `<map>` 元素的子元素，用于设置一个键值对。其 key 属性指定字符串类型的键值，ref 或 value 子元素指定其值
* init-method	

    容器加载 Bean 时调用该方法，类似于 Servlet 中的 init() 方法
* destroy-method	

    容器删除 Bean 时调用该方法，类似于 Servlet 中的 destroy() 方法。该方法只在 scope=singleton 时有效
* lazy-init	

    懒加载，值为 true，容器在首次请求时才会创建 Bean 实例；值为 false，容器在启动时创建 Bean 实例。该方法只在 scope=singleton 时有效