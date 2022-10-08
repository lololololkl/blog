---
tags: [Android]
categories: 
- Android
title: '5 Content Provider(1)'
date: 2021-02-17 13:10:36
created: '2021-02-17T13:10:36.469Z'
modified: '2021-04-19T13:31:51.758Z'
---

# 5 Content Provider(1)
### 运行时权限
```java
private void onClick(View v) {
        if (ContextCompat.checkSelfPermission(MainActivity.this,
                Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED) {//判断是否已经授权
            ActivityCompat.requestPermissions(MainActivity.this,
                    new String[]{Manifest.permission.CALL_PHONE}, 1);//获取权限
        }else{
            call();
        }
    }

    private void call() {//拨打电话的逻辑操作
        try{
            Intent intent = new Intent(Intent.ACTION_CALL);
            intent.setData(Uri.parse("tel:10086")); //Uri.parse()解析成Uri对象
            startActivity(intent);
        } catch (SecurityException e) {
            e.printStackTrace();
        }
    }
<!-- more -->

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        switch (requestCode){ //判断获取权限结果
            case 1:
                if(grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED){
                    call();
                }else  {
                    Toast.makeText(this, "获取权限失败", Toast.LENGTH_SHORT).show();
                }
                break;
            default:
        }
    }
```

