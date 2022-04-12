---
tags: [Android]
categories: 
- Android
title: 4 Data(2)
created: '2021-02-09T13:16:24.166Z'
modified: '2021-04-19T13:31:51.711Z'
---

# 4 Data(2)
### SharedPreferences
* 保存数据(onCreate方法内)
```java
Button saveData = findViewById(R.id.save_data);
        saveData.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                SharedPreferences.Editor editor = getSharedPreferences("data", MODE_PRIVATE).edit();
                editor.putString("name", "Tom");
                editor.putInt("age", 28);
                editor.putBoolean("married", false);
                editor.apply();
            }
        });
```
* 读取数据(onCreate方法内)
```java
Button restoreData = findViewById(R.id.restore_data);
        restoreData.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                SharedPreferences pref = getSharedPreferences("data", MODE_PRIVATE);
                String name = pref.getString("name", ""); // 通过键读取值
                int age = pref.getInt("age", 0);
                boolean married = pref.getBoolean("married", true);
                Log.i("---", "name "+name);
                Log.i("---", "age "+age);
                Log.i("---", "married "+married);
            }
        });
```
