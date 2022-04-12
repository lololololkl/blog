---
tags: [Android]
categories: 
- Android
title: 1 RecyclerView
created: '2021-01-26T08:01:36.212Z'
modified: '2021-04-19T13:26:59.958Z'
---

# 1 RecyclerView
### RecyclerView

- 调用
```java
    RecyclerView recyclerView = findViewById(R.id.recycler_view);
    LinearLayoutManager layoutManager = new LinearLayoutManager(this);
    recyclerView.setLayoutManager(layoutManager);
    FruitAdapter adapter = new FruitAdapter(fruitList);
    recyclerView.setAdapter(adapter);
```

  + 控件
    - id: recycler_view
    ```xml
    <androidx.recyclerview.widget.RecyclerView
      android:id="@+id/recycler_view"
      android:layout_width="match_parent"
      android:layout_height="match_parent" />
    ```
    - id: fruit_name, fruit_image
<!-- more -->

- 适配器 Adapter
  - 定义式
  ```java
  public class FruitAdapter extends RecyclerView.Adapter<FruitAdapter.ViewHolder> {}
  ```
  - 创建内部类 ViewHolder
  ```java
  static class ViewHolder extends RecyclerView.ViewHolder{
      ImageView fruitImage;
      TextView fruitName;

      public ViewHolder(@NonNull View view) {
          super(view);
          fruitName = (TextView)view.findViewById(R.id.fruit_name);
          fruitImage = (ImageView)view.findViewById(R.id.fruit_image);
      }
  }
  ```

  - 构造方法 FruitAdapter（接收数据源）
  ```java
  public FruitAdapter(List<Fruit> fruitList){
      mFruitList = fruitList;
  }
  ```

  - mFruitList: 
  ```java
  private List<Fruit> mFruitList;
  ```
  - 必须继承的方法
  `onCreateViewHolder()`
  `onBindViewHolder()`
  `getItemCount()`

    - onCreateViewHolder() 创建ViewHolder实例
    ```java 
      public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        //加载fruit_item布局
      View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.fruit_item, parent, false);
      //将布局传入ViewHolder
      ViewHolder holder = new ViewHolder(view);
      //返回ViewHolder实例
      return holder;
    }
    ```
    - onBindViewHolder()
    ```java 
      public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        //通过当前项获得Fruit实例
          Fruit fruit = mFruitList.get(position);
          //将数据设置到ViewHolder的ImageView和TextView中
          holder.fruitImage.setImageResource(fruit.getImageId());
          holder.fruitName.setText(fruit.getName());
      }
    ```
    > 用于对RecyclerView子项的数赋值，每个子项滚动到屏幕的时候执行
    - getItemCount()
    ```java 
    public int getItemCount() {
      //返回数据源长度
      return mFruitList.size();
    }
    ```
