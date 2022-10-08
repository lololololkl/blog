---
tags: [Android]
categories: 
- Android
title: 4 Data(4)
date: 2021-02-10 13:22:10
created: '2021-02-10T13:22:10.333Z'
modified: '2021-04-19T13:31:51.743Z'
---

# 4 Data(4)
### LitePal 3.0
#### 添加依赖、配置
* build.gradle
`    implementation 'org.litepal.android:java:3.0.0'
`
* AndroidManifest.xml: application添加
`android:name="org.litepal.LitePalApplication"`
* 在app/src/main新建assets/litepal.xml
```xml
<?xml version="1.0" encoding="utf-8" ?>
<litepal>
<!--    数据库名称-->
    <dbname  value="LitePalDemo"/>
<!--    版本-->
    <version value="1"/>
    <list>
<!--        注册NewsBean表-->
        <mapping class="com.example.litepaltest.NewsBean"/>
    </list>
</litepal>
```
<!-- more -->

* 创建NewsBean类(Alt + inserrt)
```java
public class NewsBean extends LitePalSupport {

    private String createTime;
    private String title;
    private String content;
    private int number;

    public void setCreateTime(String createTime) {
        this.createTime = createTime;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public void setContent(String content) {
        this.content = content;
    }

    public void setNumber(int number) {
        this.number = number;
    }

    public String getCreateTime() {
        return createTime;
    }

    public String getTitle() {
        return title;
    }

    public String getContent() {
        return content;
    }

    public int getNumber() {
        return number;
    }
}
```
#### MainActivity
* 增加
```java
public void release(){
        //添加单条数据，批量可用List, LitePal.saveAll(list)
        NewsBean bean = new NewsBean();
        bean.setCreateTime("20:23");
        bean.setTitle("title");
        bean.setContent("this is content");
        bean.setNumber(200);
        if(bean.save()){
            Toast.makeText(this, "save suceeded", Toast.LENGTH_SHORT).show();
        }else{
            Toast.makeText(this, "save failed", Toast.LENGTH_SHORT).show();
        }
    }
```
* 更新
```java
private void updateNews() {
        NewsBean newsBean = new NewsBean();
//        newsBean = LitePal.find(NewsBean.class, 1);
        newsBean.setTitle("new title");
        newsBean.setContent("this is new content");
        newsBean.setToDefault("title"); // 设为 null
        if (newsBean.save()) { //save同newsBean.update(1)
            Toast.makeText(this, "update succeeded", Toast.LENGTH_SHORT).show();
        } else {
            Toast.makeText(this, "update failed", Toast.LENGTH_SHORT).show();
        }
        //newsBean.updateAll("title = ?", "title"};修改所有
        // title为"title"的记录，更新为"new title"
    }
```
* 删除
```java
private void deleteNews(int position){
//        LitePal.delete(NewsBean.class, position);
        LitePal.deleteAll(NewsBean.class);//delete all
//        LitePal.deleteAll(NewsBean.class, "number > ?","100");
//        LitePal.deleteAll(NewsBean.class, "title = ?", "张三");
//        LitePal.deleteAll(NewsBean.class, "title = ? and content = ?", "张三", "content");
    }
```
* 查询
```java
private void queryNews(){
        List<NewsBean> newsBeanList = LitePal.findAll(NewsBean.class);
        NewsBean newsBean = LitePal.find(NewsBean.class, 1);//id为1的记录
        //第一条、最后一条记录
        NewsBean first = LitePal.findFirst(NewsBean.class);
        NewsBean last = LitePal.findLast(NewsBean.class);
        //第5，10，,15条记录
        List<NewsBean> newsList1 = LitePal.findAll(NewsBean.class, 5, 10, 15);
        for(NewsBean list : newsBeanList){
            Log.e("print", "create time: "+list.getCreateTime());
            Log.e("print", "title: "+list.getTitle());
            Log.e("print", "content: "+list.getContent());
            Log.e("print", "number: "+list.getNumber());
            Log.e("print", "sum = "+newsBeanList.size());
            String str = "---------------------------------------------------------";
            System.out.println(str);
        }
    }
```

