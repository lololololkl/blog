---
tags: [Android]
categories: 
- Android
title: 'Multimedia(1): Notification'
created: '2021-03-05T14:53:55.334Z'
modified: '2021-04-19T13:32:54.563Z'
---

# Multimedia(1): Notification
### 使用通知
```java
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button sendNotice = findViewById(R.id.send_notice);
        Intent intent = new Intent(this, NotificationActivity.class); //启动NotificationActivity的意图
        PendingIntent pi = PendingIntent.getActivity(this, 0, intent, 0); //获取PendingIntent实例
        sendNotice.setOnClickListener(v ->{
                        NotificationManager manager = (NotificationManager)getSystemService(NOTIFICATION_SERVICE);
                        Notification notification = new NotificationCompat.Builder(this).setContentTitle("Content Title")
                                .setContentText("Content Text")
                                .setWhen(System.currentTimeMillis())
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher))
                                .setContentIntent(pi)
                                .setAutoCancel(true)//点击后自动取消通知
                                .build();
                        manager.notify(1, notification);
                });
        }
```
<!-- more -->

### 进阶
```java
.setSound(Uri.fromFile(new File("/system/media/audio/notifications/Antimony.ogg")))//播放音频
.setVibrate(new long[]{0, 500, 500, 500}) //振动
.setLights(Color.GREEN, 1000, 1000) //指示灯
//        .setDefaults(NotificationCompat.DEFAULT_ALL) //手机默认通知效果
```


