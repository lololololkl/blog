---
tags: [Android]
categories: 
- Android
title: 'Multimedia(2):Camera'
created: '2021-03-06T09:07:18.767Z'
modified: '2021-04-19T13:32:54.584Z'
---

# Multimedia(2):Camera
### 调用相机
```java
public class MainActivity extends AppCompatActivity {

    public static final int TAKE_PHOTO = 1;
    private ImageView picture;
    private Uri imageUri;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button takePhoto = findViewById(R.id.take_photo);
        picture = findViewById(R.id.picture);
        takePhoto.setOnClickListener(v -> {
            //getExternalCacheDir得到缓存目录
            File outputImage = new File(getExternalCacheDir(), "output_image.jpg");
            try {
                if (outputImage.exists()) {
                    outputImage.delete();
                }
                outputImage.createNewFile();
            } catch (IOException e) {
                e.printStackTrace();
            }
            if (Build.VERSION.SDK_INT >= 24) {
                //getUriForFile将file对象转换成一个Uri对象
                imageUri = FileProvider.getUriForFile(this, "com.example.cameratest.fileprovider", outputImage);
            } else {
                //Uri标识着真实的路径（不安全）
                imageUri = Uri.fromFile(outputImage);
            }
            Intent intent = new Intent("android.media.action.IMAGE_CAPTURE");
//            Intent intent = new Intent("android.media.ACTION_IMAGE_CAPTURE");
//            Intent intent = new Intent(android.provider.MediaStore.ACTION_IMAGE_CAPTURE);
            intent.putExtra(MediaStore.EXTRA_OUTPUT, imageUri);//putExtra指定图片输出地址
            startActivityForResult(intent, TAKE_PHOTO);
        });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
//        Log.e("requestCode", ""+requestCode);
//        Log.e("result_ok", ""+ Activity.RESULT_OK);
//        Log.e("result_code", ""+resultCode);
        switch (requestCode) {
            case TAKE_PHOTO:
                if (resultCode == RESULT_OK) {
                    try {
                        Log.e("tips", "---------------try block-------------------");
                        Bitmap bitmap = BitmapFactory.decodeStream(getContentResolver().openInputStream(imageUri));//将图片解析成Bitmap对象
                        picture.setImageBitmap(bitmap);
                    } catch (FileNotFoundException e) {
                        e.printStackTrace();
                    }
                }
                break;
            default:
                break;
        }
    }
}
```

<!-- more -->

### 从相册中选取照片
#### 申请存储权限
```java
chooseFromAlbum.setOnClickListener(v ->{
            //申请WRITE_EXTERNAL_STORAGE权限
            if (ContextCompat.checkSelfPermission(MainActivity.this, Manifest.permission.WRITE_EXTERNAL_STORAGE)!= PackageManager.PERMISSION_GRANTED) {
                ActivityCompat.requestPermissions(MainActivity.this, new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, 1);
            }else{
                openAlbum();
            }
        });
```
```java
private void openAlbum() {
        //设置Intent指定action为android.intent.action.GET_CONTENT
        Intent intent = new Intent("android.intent.action.GET_CONTENT");
        intent.setType("image/*");
        startActivityForResult(intent, CHOOSE_PHOTO);
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        switch(requestCode){
            case 1:
                if (grantResults.length>0 && grantResults[0]==PackageManager.PERMISSION_GRANTED){
                    openAlbum();
                }else{
                    Toast.makeText(this, "Access denied", Toast.LENGTH_SHORT).show();
                }
                break;
            default:
        }
    }
```
#### 处理Uri
在onActivityResult中添加case
```java
 case CHOOSE_PHOTO:
                if (resultCode == RESULT_OK){
                    if (Build.VERSION.SDK_INT >= 19){  //4.4以上系统返回的不是图片真实的Uri
                        handleImageOnKitKat(data);//需要对Uri解析
                    }else {
                        handleImageBeforeKitKat(data);
                    }
                }
```

```java
private void handleImageOnKitKat(Intent data) {
        String imagePath = null;//图片的真实路径
        Uri uri = data.getData();
        //返回的Uri是document类型，不是则用普通方式处理
        if (DocumentsContract.isDocumentUri(this, uri)){
            String docId = DocumentsContract.getDocumentId(uri);
            //Uri的Authority是media格式，在进行一次解析
            if ("com.android.providers.media.documents".equals(uri.getAuthority())){
                String id = docId.split(":")[1];
                String selection = MediaStore.Images.Media._ID+"="+id;
                imagePath = getImagePath(MediaStore.Images.Media.EXTERNAL_CONTENT_URI, selection);
            }else if ("com.android.providers.downloads.documents".equals(uri.getAuthority())){
                Uri contentUri = ContentUris.withAppendedId(Uri.parse("content://downloads/public_downloads"), Long.valueOf(docId));
                imagePath = getImagePath(contentUri, null);
            }
        }else if ("content".equalsIgnoreCase(uri.getScheme())){
            imagePath = getImagePath(uri, null);
        }else if ("file".equalsIgnoreCase(uri.getScheme())){
            imagePath = uri.getPath();
        }
        displayImage(imagePath);
    }
    private void handleImageBeforeKitKat(Intent data) {
        Uri uri = data.getData();
        String imagePath = getImagePath(uri, null);
        displayImage(imagePath);
    }
```
```java
private String getImagePath(Uri externalContentUri, String selection) {
        String path = null;
        Cursor cursor = getContentResolver().query(externalContentUri, null, selection, null, null);
        if (cursor != null){
            if (cursor.moveToFirst()){
                path = cursor.getString(cursor.getColumnIndex(MediaStore.Images.Media.DATA));
            }
            cursor.close();
        }
        return path;
    }
    private void displayImage(String imagePath) {
        if (imagePath != null){
            Bitmap bitmap = BitmapFactory.decodeFile(imagePath);
            picture.setImageBitmap(bitmap);
        }else{
            Toast.makeText(this, "failed to get Image", Toast.LENGTH_SHORT).show();
        }
    }

```

