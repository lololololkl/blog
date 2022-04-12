---
tags: [Android]
categories: 
- Android
title: 4 Data(1)
created: '2021-02-09T09:08:25.660Z'
modified: '2021-04-19T13:31:51.696Z'
---

# 4 Data(1)
### 文件存储读取
* 存储
  * save()
  ```java
  private void save(String inputText) throws IOException {
        FileOutputStream out = null;
        BufferedWriter writer = null;
        try {
            out = openFileOutput("data", Context.MODE_PRIVATE);
            writer = new BufferedWriter(new OutputStreamWriter(out));
            writer.write(inputText);
        }catch (IOException e){
            e.printStackTrace();
        }finally{
            try {
                if(writer != null){
                    writer.close();
                }
            }catch (IOException e){
                e.printStackTrace();
            }
        }
    }
  ```
  * 在onDestroy()中保存
  ```java
  protected void onDestroy() { //将输入内容存入文件中
        super.onDestroy();
        String inputText = edit.getText().toString();
        try {
            save(inputText);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
  ```
  <!-- more -->

* 读取保存的文本
  * load()方法
  ```java
  private String load() throws FileNotFoundException {
        FileInputStream in = null;
        BufferedReader reader = null;
        StringBuilder content = new StringBuilder();
        try{
            in = openFileInput("data"); //
            reader = new BufferedReader(new InputStreamReader(in)); //
            String line = "";
            while ((line = reader.readLine()) != null){ //reader.readLine()
                content.append(line);
            }
        }catch (IOException e){
            e.printStackTrace();
        }finally{
            if(reader != null){
                try {
                    reader.close();
                }catch (IOException e){
                    e.printStackTrace();
                }
            }
            return content.toString();
        }
    }
  ```
  * onCreate中添加：
  ```java
  EditText edit = findViewById(R.id.edit);
        String inputText = null;
        try {
            inputText = load();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        if (!TextUtils.isEmpty(inputText)){
            edit.setText(inputText);
            edit.setSelection(inputText.length());
            Toast.makeText(this, "Restoring succeeded", Toast.LENGTH_SHORT).show();
        }
    }
  ```

