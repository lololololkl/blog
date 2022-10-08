---
tags: [Android]
categories: 
- Android
title: 0 Activity的类文件
date: 2021-01-18 15:11:52
created: '2021-01-18T15:11:52.310Z'
modified: '2021-04-19T13:27:55.791Z'
---

#### 0 Activity的类文件

##### 活动

* 实例
  
  ```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button = (Button)findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(MainActivity.this, "You click the button", Toast.LENGTH_SHORT).show();
                    finish(); // 销毁一个活动
                sendMessage(v);
            }
        });
    }
  ```

* 创建一个Button对象实例
  
  ```java
  Button button = (Button)findViewById(R.id.button);
  ```
  <!-- more -->

  `findViewById()`获取布局中的元素

* 监听器
  
  ```java
  button.setOnClickListener(new View.OnClickListener()
  ```
  
  - 点击按钮将执行监听器中的 OnClick() 方法
  - 将 Toast 显示出来， Toast 的 makeText() 静态方法

* 显式调用 Intent
  
  ```java
    public void sendMessage(View view){
  //        调用显示Intent
        Intent intent = new Intent(this, DisplayMessageActivity.class);
        EditText editText = findViewById(R.id.editText);
        String message = editText.getText().toString();
        intent.putExtra(EXTRA_MESSAGE, message);
        startActivity(intent);
    }
  ```
  - 取出数据
  ```java
  Intent intent = new getIntent();
  String data = intent.getStringExtra(EXTRA_MESSAGE);
  ```

* 给当前活动创建菜单
  
  ```java
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.main, menu);
        return super.onCreateOptionsMenu(menu);
    }
  ```
  
  返回True表示允许创建的菜单显示出来

* 菜单响应事件
  
  ```java
    public boolean onOptionsItemSelected(@NonNull MenuItem item) {
        switch(item.getItemId()){
            case R.id.add_item:
                Toast.makeText(this, "You clicked Add", Toast.LENGTH_SHORT).show();
                break;
            case R.id.remove_item:
                Toast.makeText(this, "You clicked Remove", Toast.LENGTH_SHORT).show();
                break;
            default:
        }
        return super.onOptionsItemSelected(item);
    }
  ```
  
  ##### 活动的生命周期
  
  ##### 活动的启动模式
  
  #### xml文件
  
  ##### 控件
1. TextView

2. Button

3. EditText

4. ImageView

5. ProgressBar

6. AlertDialog

7. ProgressDialog
   
   ##### 基本布局
   
   * 线性布局 LinearLayout
   
   ```h
   排列方向：android:orientation="horizonal" 或 vertical
   长宽：android:layout_width="wrap_content",
    android:layout_height="wrap_content"
   对齐方式：gravity指定文字在控件中的对齐方式，
        layout_gravity指定控件在布局中的对齐方式
   layout_weight使用比例的方式指定空间大小
   ```
   
   * 相对布局 RelativeLayout
     layout_alignParentLeft...layout_centerInparent, layout_above...layout_toRightOf, layout_alignLeft...leyout_alignButtom

8. 帧布局 FrameLayout

9. 百分比布局 PercentFrameLayout
   layout_widthPercent, layout_heightPercent
   
   ##### 创建自定义布局
* 引入布局
  创建Title.xml， 引入布局：
  `<include layout="@layout/title" />`
  将系统还自带的标题栏隐藏：
  
  ```java
  ActionBar actionBar = getSuportActionBar();
  if(actionBar != Null){
  actionBar.hide();
  }
  ```

* 创建自定义控件
  可以为控件添加功能，如：为按钮添加响应事件
  新建TitleLayout：
  
  ```java
  public class TitleLayout extends LinearLayout {
    public TitleLayout(Context context, AttributeSet attrs){
        super(context, attrs);
        LayoutInflater.from(context).inflate(R.layout.title, this);
  }
  ```
  
  在Activity_main.xml中添加代码，引用TitleLayout：
  
  ```html
      <com.example.uitest.TitleLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
        <!--（包名不可省）-->
  ```

##### ListView 控件和 RecyclerView 控件

* ListView
  
  添加控件：
  
  ```xml
      <ListView
        android:id="@+id/list_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
  ```
  
  修改ActivitMain：
  &emsp;&emsp;适配器：借助适配器传递数组中的数据给ListView。ArrayAdapter通过指定泛型要适配的数据类型，在构造函数中将数据传入，调用setAdapter()将适配器对象传递进去。
  
  ```java
    private String[] data = {"a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m",
            "n", "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z"
    };
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>
        (MainActivity.this, android.R.layout.simple_list_item_1, data);
        ListView listView = findViewById(R.id.list_view);
        listView.setAdapter(adapter);
    }
  ```

* 定制ListView界面
   - 新建类Fruit,fruit_itme.xml
  - 创建自定义适配器
    
    ```java
    public class FruitAdapter extends ArrayAdapter<Fruit> {
    private int resourceId;
    public FruitAdapter(Context context, int textViewResourceId,
                        List<Fruit> objects){
        super(context, textViewResourceId, objects);
        resourceId = textViewResourceId;
      }
    public View getView(int position, View convertView, ViewGroup parent){
        Fruit fruit = getItem(position);
        View view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
        ImageView fruitImage = view.findViewById(R.id.fruit_image);
        TextView fruitName = view.findViewById(R.id.fruit_name);
        fruitImage.setImageResource(fruit.getImageId());
        fruitName.setText(fruit.getName());
        return view;
      } // getView 在每个子项滚动到屏幕内的时候调用
    }
    ```
  - 修改 MainActivity

    ```java
    public class MainActivity extends AppCompatActivity {
    private List<Fruit> fruitList = new ArrayList<>();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initFruits();
        FruitAdapter adapter = new FruitAdapter(MainActivity.this, R.layout.fruit_item, fruitList);
        ListView listView = findViewById(R.id.list_view);
        listView.setAdapter(adapter);
    }}
    ```
  - 提升ListView运行效率
      + 修改getView()：convertView布局的缓存
      ```java
        View view;
        if(convertView == null) {
                view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
        }
        else {
            view = convertView;
        }
      ```
      + 修改getView()：viewHolder对控件实例的缓存
      ```java
        class ViewHolder{
        ImageView fruitImage;
        TextView fruitName;
    }
      ```
      ```java
        View view;
        ViewHolder viewHolder;
        if(convertView == null) {
                view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
                viewHolder = new ViewHolder();
                viewHolder.fruitImage = view.findViewById(R.id.fruit_image);
                viewHolder.fruitName = view.findViewById(R.id.fruit_name);
                view.setTag(viewHolder); //viewHolder存储在view中
        }
        else {
            view = convertView;
            viewHolder = (ViewHolder) view.getTag();
        }
        viewHolder.fruitImage.setImageResource(fruit.getImageId());
        viewHolder.fruitName.setText(fruit.getName());
        return view;
      ```
      + ListView 点击事件
      ```java
      listView.setOnItemClickListener(new AdapterView.OnItemClickListener(){
      public void onItemClick(AdapterView<?>parent, View view, int position, long id){
                Fruit fruit = fruitList.get(position);
                Toast.makeText(MainActivity.this, fruit.getName(), Toast.LENGTH_SHORT).show();
            }
        });
      ```
* RecyclerView
  - FruitAdapter文件
  ```java
  public class FruitAdapter extends RecyclerView.Adapter<FruitAdapter.ViewHolder> {

    private List<Fruit> mFruitList;


    static class ViewHolder extends RecyclerView.ViewHolder{
        ImageView fruitImage;
        TextView fruitName;

        public ViewHolder(@NonNull View view) {
            super(view);
            fruitName = (TextView)view.findViewById(R.id.fruit_name);
            fruitImage = (ImageView)view.findViewById(R.id.fruit_image);
        }
    }
    public FruitAdapter(List<Fruit> fruitList){
        mFruitList = fruitList;
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.fruit_item, parent, false);
        ViewHolder holder = new ViewHolder(view);
        return holder;
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        Fruit fruit = mFruitList.get(position);
        holder.fruitImage.setImageResource(fruit.getImageId());
        holder.fruitName.setText(fruit.getName());
    }

    @Override
    public int getItemCount() {
        return mFruitList.size();
    }
  }  
  ```
  - MainActivity文件
    ```java
        protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initFruits();
        RecyclerView recyclerView = findViewById(R.id.recycler_view);
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        recyclerView.setLayoutManager(layoutManager);
        FruitAdapter adapter = new FruitAdapter(fruitList);
        recyclerView.setAdapter(adapter);
    }
    ```
  - 横向排列(MainActivity)
      ```java
      protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initFruits();
        RecyclerView recyclerView = findViewById(R.id.recycler_view);
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        layoutManager.setOrientation(LinearLayoutManager.HORIZONTAL); //横向排列
        recyclerView.setLayoutManager(layoutManager);
        FruitAdapter adapter = new FruitAdapter(fruitList);
        recyclerView.setAdapter(adapter);
    }
    ```
  - 瀑布流布局
    ```java
      protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initFruits();
        RecyclerView recyclerView = findViewById(R.id.recycler_view);
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        //瀑布流布局
        StaggeredGridLayoutManager layoutManager = 
                new StaggeredGridLayoutManager(3, StaggeredGridLayoutManager.VERTICAL);
        recyclerView.setLayoutManager(layoutManager);
        FruitAdapter adapter = new FruitAdapter(fruitList);
        recyclerView.setAdapter(adapter);
    }
    ```


      

