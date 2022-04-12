---
tags: [Android]
categories: 
- Android
title: 5 Content Provider(2)
created: '2021-02-28T09:05:24.135Z'
modified: '2021-04-19T13:31:51.772Z'
---

# 5 Content Provider(2)
### ContentResolver的用法/访问其他应用程序的数据/跨程序共享数据
```java
public class MainActivity extends AppCompatActivity {

    ArrayAdapter<String> adapter;
    List<String> contactsList = new ArrayList<>();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ListView contactsView = (ListView)findViewById(R.id.contacts_list);
        adapter = new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, contactsList); //设置适配器
        contactsView.setAdapter(adapter);
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_CONTACTS) != PackageManager.PERMISSION_GRANTED){
            ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.READ_CONTACTS}, 1);
        }else{
            readContacts();
        }
    }
    private void readContacts() {
        Cursor cursor = null;
        try {
          // CONTENT_URI常量是ContactsContract.CommonDataKinds.Phone类做好的封装
            cursor = getContentResolver().query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI, null, null, null, null);
            if (cursor != null){
                while(cursor.moveToNext()){
                    String displayName = cursor.getString(cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME));
                    String number = cursor.getString(cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER));
                    contactsList.add(displayName+"\n"+number); // 将数据添加到ListView
                }
                adapter.notifyDataSetChanged(); //通知刷新一下ListView
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            if(cursor != null){
                cursor.close();
            }
        }
    }
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        switch(requestCode){
            case 1:
                if (grantResults.length>0&&grantResults[0]==PackageManager.PERMISSION_GRANTED){
                    readContacts();
                }else{
                    Toast.makeText(this, "权限获取失败", Toast.LENGTH_SHORT).show();
                }
                break;
            default:
        }
    }
}
```
