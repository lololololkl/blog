---
tags: [Android]
categories: 
- Android
title: 3 Broadcast
date: 2021-02-07 15:11:52
created: '2021-02-07T10:15:17.527Z'
modified: '2021-04-19T13:31:51.679Z'
---

# 3 Broadcast
### 动态注册监听网络变化
* 例子
```java
public class MainActivity extends AppCompatActivity {

    private IntentFilter intentFilter;
    private NetWorkChangeReceiver netWorkChangeReceiver;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
//       intentFilter
        intentFilter = new IntentFilter();
        intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
        netWorkChangeReceiver = new NetWorkChangeReceiver();
//        registerReceiver
        registerReceiver(netWorkChangeReceiver, intentFilter);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
//        unregisterReceiver
        unregisterReceiver(netWorkChangeReceiver);
    }
//   广播接收器
    class NetWorkChangeReceiver extends BroadcastReceiver{
        @Override
        public void onReceive(Context context, Intent intent) {
//          ConnectivityManager
            ConnectivityManager connectivityManager = (ConnectivityManager) getSystemService(context.CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
            if(networkInfo != null && networkInfo.isAvailable()){
                Toast.makeText(context, "Network Available", Toast.LENGTH_SHORT).show();
            }else{
                Toast.makeText(context, "Network unavailable", Toast.LENGTH_SHORT).show();
            }
        }
    }
}
```
<!-- more -->

* 加入权限声明
```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
### 发送自定义广播
#### 发送标准广播
* 自定义广播接收器
```java
public class MyBroadcastReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context, "receiver in MyBroadcastReceiver", Toast.LENGTH_SHORT).show();
    }
}
```
* AndroidManifest.xml修改广播接收器
```xml
<receiver android:name=".MyBroadcastReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
                <action android:name="com.example.broadcasttest.MY_BROADCAST"/>
            </intent-filter>
        </receiver>
```
* MainActivity
```java
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button = findViewById(R.id.button);
        button.setOnClickListener(v -> {
            Intent intent = new Intent("com.example.broadcasttest.MY_BROADCAST");
            intent.setPackage("com.example.broadcasttest"); // 一定要设置Package，否则无法启动BroadcastReceiver
            sendBroadcast(intent);
        });
    }
```
#### 发送有序广播
* 将MainActivity修改：
```java
Intent intent = new Intent("com.example.broadcasttest.MY_ORDDER_BROADCAST0");
            intent.setPackage("com.example.broadcasttest"); // 一定要设置Package，否则无法启动BroadcastReceiver
//            sendBroadcast(intent);
            sendOrderedBroadcast(intent, null);
```
* 新建Receiver
```java
public class MyReceiver0 extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context, "receiver in MyReceiver0", Toast.LENGTH_SHORT).show();
    }
}
```
* 在Manifest.xml部署，优先级20
```xml
<receiver android:name=".MyReceiver0">
            <intent-filter android:priority="20">
                <action android:name="com.example.broadcasttest.MY_ORDDER_BROADCAST0"/>
            </intent-filter>
        </receiver>
```
* 在onReceive()添加 `abortBroadcast();`取消广播，下一个接收器不会触发
* 传递数据给下一个接收器
上一个接收器：
```java
Bundle bundle = new Bundle();
        bundle.putString("first", "this is a message");
        setResultExtras(bundle);
```
获取上一个接收器存入的数据
```java
Bundle bundle = getResultExtras(true);
        String first = bundle.getString("first");
        Toast.makeText(context, "receiver in MyReceiver1\n"+first, Toast.LENGTH_SHORT).show();
```
