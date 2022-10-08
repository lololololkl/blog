---
tags: [other, idea]
categories: 
- other
- idea
title: "运行Maven项目时出错“Error : java 不支持发行版本5”"
date: 2021-09-17 20:03:02
---

### 修改 idea 配置

1. 点击 File -> Project Structure
<img src="/uploads/img/idea/idea_1.png">
<!-- more -->

2. 查看 Project Setting 下 Project 和Modules栏目中的Java版本是否一致，如果不一致，改成本地使用的Java版本。
<img src="/uploads/img/idea/idea_2.png">
3. 点击“Settings”-->“Bulid, Execution,Deployment”-->“Java Compiler”，Target bytecode version设为本地Java版本。（可以在Default Settings中把Project bytecode version 一劳永逸地配置成本地Java版本）
<img src="/uploads/img/idea/idea_3.png">

### 导入sql表

 * 在数据库创建一个表(在Schemas右键new一个Schema)
 * 打开sql脚本
 * 右键运行
 * 选择要导入的表