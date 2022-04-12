---
tags: [Android]
categories: 
- Android
title: 5 Content Provider(3)
created: '2021-02-28T14:51:36.378Z'
modified: '2021-04-19T13:31:51.786Z'
---

# 5 Content Provider(3)
### 在databasetest中新建一个文件`DatabaseProvider.java`/创建内容提供器
```java
public class DatabaseProvider extends ContentProvider {

    public static final int BOOK_DIR = 0;
    public static final int BOOK_ITEM = 1;
    public static final int CATEGORY_DIR =2;
    public static final int CATEGORY_ITEM = 3;
    public static final String AUTHORITY = "com.example.databasetest.provider";
    public static UriMatcher uriMatcher;
    private MyDatabaseHelper dbHelper;
    static{
        uriMatcher =  new UriMatcher(UriMatcher.NO_MATCH);
        //注册需要的Uri
        uriMatcher.addURI(AUTHORITY, "book", BOOK_DIR);//参数3是要返回的匹配码
        uriMatcher.addURI(AUTHORITY, "book/#", BOOK_ITEM);
        uriMatcher.addURI(AUTHORITY, "category", CATEGORY_DIR);
        uriMatcher.addURI(AUTHORITY, "category/#", CATEGORY_ITEM);
    }//静态代码块，对UriMatcher初始化
    //将期望匹配的3种URI格式添加进去
    //--常量 UriMatcher.NO_MATCH 表示不匹配任何路径的返回码
    //--# 号为通配符
    //--* 号为任意字符
    @Override
    public boolean onCreate() {
        // TODO: Implement this to initialize your content provider on startup.
        //创建或升级数据库
        dbHelper = new MyDatabaseHelper(getContext(), "BookStore.db", null, 2);
        return true;
    }
//    public DatabaseProvider() {
//    }
    @Override
    public Cursor query(Uri uri, String[] projection, String selection,
                        String[] selectionArgs, String sortOrder) {
        // TODO: Implement this to handle query requests from clients.
        //创建SQLiteDatabase实例
        SQLiteDatabase db = dbHelper.getReadableDatabase();
        Cursor cursor = null;
        switch(uriMatcher.match(uri)){
            case BOOK_DIR:
                cursor  = db.query("Book", projection, selection, selectionArgs, null, null, sortOrder);
                break;
            case BOOK_ITEM:
                //将URI的AUTHORITY之后的部分用 / 符号分割，放到一个列表中
                //不包含元字符串最左端的部分
                String bookId = uri.getPathSegments().get(1);
                cursor = db.query("Book", projection, "id = ?", new String[]{bookId}, null, null, sortOrder);
                break;
            case CATEGORY_DIR:
                cursor = db.query("Category", projection, selection, selectionArgs, null, null, sortOrder);
                break;
            case CATEGORY_ITEM:
                String categoryId = uri.getPathSegments().get(1);
                cursor = db.query("Category", projection, "id = ?", new String[]{ categoryId }, null, null, sortOrder);
                break;
            default:
                break;
        }
        return cursor; //返回cursor
    }
    @Override
    public Uri insert(Uri uri, ContentValues values) {
        // TODO: Implement this to handle requests to insert a new row.
        SQLiteDatabase db = dbHelper.getWritableDatabase();
        Uri uriReturn = null;
        switch(uriMatcher.match(uri)){
            case BOOK_DIR:
            case BOOK_ITEM:
                long newBookId = db.insert("Book", null, values);//返回新增的行号
                uriReturn = Uri.parse("content://"+AUTHORITY+"/book/"+newBookId);
                break;
            case CATEGORY_DIR:
            case CATEGORY_ITEM:
                long newCategoryId = db.insert("Category", null, values);
                uriReturn = Uri.parse("content://"+AUTHORITY+ "/category/"+newCategoryId);
                break;
            default:
                break;
        }
        return uriReturn;
    }
    @Override
    public int update(Uri uri, ContentValues values, String selection,
                      String[] selectionArgs) {
        // TODO: Implement this to handle requests to update one or more rows.
        SQLiteDatabase db = dbHelper.getWritableDatabase();
        int updatedRows =0;
        switch (uriMatcher.match(uri)){
            case BOOK_DIR:
                updatedRows = db.update("Book", values, selection, selectionArgs);
                break;
            case BOOK_ITEM:
                String bookId = uri.getPathSegments().get(1);
                updatedRows = db.update("Book", values, "id = ?", new String[]{bookId});
                break;
            case CATEGORY_DIR:
                updatedRows = db.update("Category", values, selection, selectionArgs);
                break;
            case CATEGORY_ITEM:
                String categoryId = uri.getPathSegments().get(1);
                updatedRows = db.update("Category", values, "id = ?", new String[]{categoryId});
                break;
            default:
                break;
        }
        return updatedRows;
    }

    @Override
    public int delete(Uri uri, String selection, String[] selectionArgs) {
        // Implement this to handle requests to delete one or more rows.
        SQLiteDatabase db = dbHelper.getWritableDatabase();
        int deletedRows =  0;
        switch(uriMatcher.match(uri)){
            case BOOK_DIR:
                deletedRows = db.delete("Book", selection, selectionArgs);
                break;
            case BOOK_ITEM:
                String bookId = uri.getPathSegments().get(1);
                deletedRows = db.delete("Book", "id = ?", new String[]{bookId});
                break;
            case CATEGORY_DIR:
                deletedRows = db.delete("Category", selection, selectionArgs);
                break;
            case CATEGORY_ITEM:
                String categoryId = uri.getPathSegments().get(1);
                deletedRows = db.delete("Category", "id = ?", new String[]{categoryId});
                break;
            default:
                break;
        }
        return deletedRows;
    }
    @Override
    public String getType(Uri uri) { //用于获取Uri对象MIME类型（字符串）
        // TODO: Implement this to handle requests for the MIME type of the data
        // at the given URI.
        switch (uriMatcher.match(uri)){
            case BOOK_DIR:
                return "vnd.android.cursor.dir/vnd.example.databasetest.provider.book";
            case BOOK_ITEM:
                return "vnd.android.cursor.item./vnd.example.databasetest.provider.book";
            case CATEGORY_DIR:
                return "vnd.android.cursor.dir/vnd.example.databasetest.provider.category";
            case CATEGORY_ITEM:
                return "vnd.android.cursor.item/vnd.example.databasetest.provider.category";
        }
        return null;
    }
}
```
<!-- more -->

内容提供器一定要在Manifest中注册才能使用。用快捷方式创建内容提供器时，注册已经完成了。现在程序已经拥有跨程序共享功能了。
### 创建一个新应用去实现跨程序访问
```java
public class MainActivity extends AppCompatActivity {

    public String newId;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button addData = findViewById(R.id.add_data);
        addData.setOnClickListener(v ->{
            Uri uri = Uri.parse("content://com.example.databasetest.provider/book");
            ContentValues values = new ContentValues();
            //把要添加数据存放到ContentValues对象中
            values.put("name", "A Clash Of Kings");
            values.put("author", "george martin");
            values.put("pages", 1040);
            values.put("price", 22.85);
            Uri newUri = getContentResolver().insert(uri, values);
            Log.e("newUri", "is "+newUri);
            newId = newUri.getPathSegments().get(1);
        });
        Button queryData = findViewById(R.id.query_data);
        queryData.setOnClickListener(v ->{
            Uri uri = Uri.parse("content://com.example.databasetest.provider/book");
            Cursor cursor = getContentResolver().query(uri, null, null, null, null);
            Log.e("newId", "-------------------query--------------------------"+newId);
            if (cursor != null){
                while(cursor.moveToNext()){
                    String name = cursor.getString(cursor.getColumnIndex("name"));
                    String author = cursor.getString(cursor.getColumnIndex("author"));
                    int pages = cursor.getInt(cursor.getColumnIndex("pages"));
                    double price = cursor.getDouble(cursor.getColumnIndex("price"));
                    Log.e("--->", "book name: "+name);
                    Log.e("--->", "author: "+author);
                    Log.e("--->", "pages: "+pages);
                    Log.e("--->", "price: "+price);
                }
            }
        });
        Button updateData = findViewById(R.id.update_data);
        updateData.setOnClickListener(v ->{
            Uri uri = Uri.parse("content://com.example.databasetest.provider/book/"+newId);
            ContentValues values = new ContentValues();
            Log.e("newId", "-------------------update--------------------------"+newId);
            values.put("name", "a storm of swords");
            values.put("pages", 100);
            values.put("price", 100.23);
            values.put("author", "no author");
            getContentResolver().update(uri, values, null, null);
        });
        Button deleteData = findViewById(R.id.delete_data);
        deleteData.setOnClickListener(v ->{
            Log.e("newId", "-------------------update--------------------------"+newId);
            Uri uri = Uri.parse("content://com.example.databasetest.provider/book/"+newId);
            getContentResolver().delete(uri, null, null);
        });
    }
}
```

