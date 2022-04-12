---
tags: [Android]
categories: 
- Android
title: 2 Fragment
created: '2021-01-28T09:31:11.208Z'
modified: '2021-04-19T13:31:51.651Z'
---

# 2 Fragment
> 一种嵌入到活动中的UI片段
### 碎片
* java: news, NewsContentFragment, NewsTitleFragment, activity_main, 
  xml: news_content_flag, news_content, news_title_flag, news_item
### 关联
```
left_fragment.xml-->LeftFragment.java
right_fragment.xml-->RightFragment.java
another_right_fragment.xml-->AnotherRightFragment.java
```
- 以LeftFragment为例
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:text="Button" />
</LinearLayout>
<!-- more -->

```
```java
public class LeftFragment extends Fragment {
    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.left_fragment, container, false);
        return view;
    }
}
```
LeftFragment继承自Fragment，将left_fragment加载进来。
- activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <fragment
        android:id="@+id/left_fragment"
        android:name="com.example.fragmenttest.LeftFragment"
        android:layout_width="0dp"
        android:layout_weight="1"
        android:layout_height="match_parent"
        />

    <fragment
        android:id="@+id/right_fragment"
        android:name="com.example.fragmenttest.RightFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1" />
</LinearLayout>
```
android:name属性显式指明碎片名。
### 动态添加碎片
创建another_right_fragment和AnotherRightFragment，按下左侧按钮将右侧碎片替换成AnotherRightFragment。
- replaceFragment()(在MainActivity中)
```java
public void replaceFragment(Fragment fragment){
    // 获取FragmentManager
    FragmentManager fragmentManager = getSupportFragmentManager();
    // 开启一个事务
    FragmentTransaction transaction = fragmentManager.beginTransaction();
    // 向容器内添加或替换碎片
    transaction.replace(R.id.right_layout, fragment );
    // 按下Back键返回上个碎片
    transaction.addToBackStack(null);
    // 提交事务
    transaction.commit();
}
```
### 碎片与活动之间的通信
- 活动中获取碎片
```java
RightFragment rightFragment = (RightFragment) getFragmentManager().findFragmentById(R.id.right_fragment);
```
- 碎片中获取活动
```java
MainActivity activity = (MainActivity) getActivity();
```
### 动态加载布局技巧
- 使用限定符
layout-large/activity_main, layout/activity_main
- 使用最小宽度限定符
res/layout-sw600dp/activity_main
## 碎片的生命周期
### 碎片的回调方法
- onAttach()
- onCreateView() 为碎片
- onActivityCreated()
- onDestroyView()
- onDetach()



