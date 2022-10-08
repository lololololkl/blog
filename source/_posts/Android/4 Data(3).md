---
tags: [Android]
categories: 
- Android
title: 4 Data(3)
date: 2021-02-09 14:45:49
created: '2021-02-09T14:45:49.833Z'
modified: '2021-04-19T13:31:51.727Z'
---

# 4 Data(3)
### SQLite
#### 创建SQLiteOpenHelper继承类
```java
public class MyDatabaseHelper extends SQLiteOpenHelper {

    public static final String CREATE_BOOK = "create table Book ("
            +"id integer primary key autoincrement, " //autoincrement: 自增长，自动生成id列的值
            +"author text, "
            +"price real, "
            +"name text)";
    private Context mContext;
    public MyDatabaseHelper(Context context, String name, SQLiteDatabase.CursorFactory factory, int version){
        super(context, name, factory, version);
        mContext = context;
    }
    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL(CREATE_BOOK);
        Toast.makeText(mContext, "create succeeded", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {

    }
}
```

<!-- more -->

#### 创建数据库(MainActivity)
```java
dbHelper = new MyDatabaseHelper(this, "BookStore.db", null, 1);
Button createDatabase = findViewById(R.id.create_database);
createDatabase.setOnClickListener(v ->{
    dbHelper.getWritableDatabase(); // 点击按钮，检测到没有BookStore.db
    // ，会调用MyDatabaseHelper的onCreate()
});
```
#### 升级数据库
  * 添加SQL语句
  ```java
  public static final String CREATE_CATEGORY = "create table Category(" +
            "id integer primary key autoincrement, " +
            "category_name text, " +
            "category_code integer)";
  ```
  * 修改onCreate()
  ```java
  public void onCreate(SQLiteDatabase db) { // 升级数据库
        db.execSQL(CREATE_BOOK);
        db.execSQL(CREATE_CATEGORY);
        Toast.makeText(mContext, "create succeeded", Toast.LENGTH_SHORT).show();
    }
  ```
  * 修改onUpgrade()
  ```java
  public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("drop table if exists Book"); // 删除表
        db.execSQL("drop table if exists Category");
        onCreate(db);
    }
  ```
  * 修改 MyDatabaseHelper 的创建语句
  ```java
  dbHelper = new MyDatabaseHelper(this, "BookStore.db", null, 3); 
  // 已存在BookStore.db，更改版本号才能弹出Toast
  ```
#### 添加数据（insert）
```java
Button addData = findViewById(R.id.add_data);
        addData.setOnClickListener(v ->{
            SQLiteDatabase db = dbHelper.getWritableDatabase(); //getWritableDatabase()返回一个SQLiteDatabase对象
            ContentValues values = new ContentValues(); //insert()的第三个参数
            //开始组装数据
            values.put("name", "The davinci code");
            values.put("author", "dan brown");
            values.put("pages", 454);
            values.put("price", 16.96);
            db.insert("Book", null, values);
            values.clear();
        });
    }
```
#### 更新数据（update）
```java
Button updateData = findViewById(R.id.update_data);
        updateData.setOnClickListener(v ->{
            SQLiteDatabase db = dbHelper.getWritableDatabase();
            ContentValues values = new ContentValues();
            values.put("price", 10.99);
            db.update("Book", values, "name = ?",
                    new String[]{"the da vinci code"});
            // 参数2：要更新的数据，参数3：SQL的where部分，?是占位符，参数4：为参数3提供指定内容
        });
```
#### 删除数据
```java
Button deleteData = findViewById(R.id.delete_data);
        deleteData.setOnClickListener(v ->{
            SQLiteDatabase db = dbHelper.getWritableDatabase();
            db.delete("Book", "pages > ?", new String[]{"500"} );
        });
```
#### 查询数据
```java
Button queryData = findViewById(R.id.query_data);
        queryData.setOnClickListener(v ->{
            SQLiteDatabase db = dbHelper.getWritableDatabase();
            //其余为null，查这张表的所有数据 query()
            Cursor cursor = db.query("Book", null, null,
                    null, null, null, null);
            if (cursor.moveToFirst()){//指针移到第一行
                do{
                    // getColumnIndex()
                    String name = cursor.getString(cursor.getColumnIndex("name"));
                    String author = cursor.getString(cursor.getColumnIndex("author"));
                    int pages = cursor.getInt(cursor.getColumnIndex("pages"));
                    double price = cursor.getDouble(cursor.getColumnIndex("price"));
                    Log.e("aa","name="+name);
                    Log.e("aa", "author="+author);
                    Log.e("aa", "pages="+pages);
                    Log.e("aa", "price="+price);
                }while (cursor.moveToNext());//
                cursor.close();//
            }
        });
```
