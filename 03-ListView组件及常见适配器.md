**ListView组件及常见适配器的使用案例**<br>
布局文件:`activity_main.xml`<br>
```xml
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ListView
        android:id="@+id/list_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintBottom_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

    </ListView>
</androidx.constraintlayout.widget.ConstraintLayout>
```

适配器的实现：<br>
`1、自定义适配器`<br>
`2、数组适配器ArrayAdapter`<br>
1、自定义适配器实现BaseAdapter类<br>
```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //加载布局文件
        setContentView(R.layout.activity_main);
        //获取ListView组件
        ListView listView = findViewById(R.id.list_view);
        //设置适配器
        listView.setAdapter(new MyAdapter());
    }
    //自定义适配器
    class MyAdapter extends BaseAdapter{
        //返回10条item
        @Override
        public int getCount() {
            return 10;
        }
        @Override
        public Object getItem(int position) {
            return null;
        }
        @Override
        public long getItemId(int position) {
            return 0;
        }
        //设置需要添加进ListView组件的item
        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
            TextView textView = new TextView(MainActivity.this);
            textView.setText("item"+position);
            return textView;
        }
    }
}
```

2、数组适配器ArrayAdapter<br>
```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //加载布局文件
        setContentView(R.layout.activity_main);
        //获取ListView组件
        ListView listView = findViewById(R.id.list_view);
        //设置适配器
        listView.setAdapter(new ArrayAdapter<String>(this,R.layout.support_simple_spinner_dropdown_item,getTexts()));
    }
    //定义String数组
    public String[] getTexts(){
       String[] texts = {"apple","pear","banana","orange"};
        return texts;
    }
}
```
或者：<br>
```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //获取ListView组件
        ListView listView = new ListView(this);
        //设置适配器
        listView.setAdapter(new ArrayAdapter<String>(this,R.layout.support_simple_spinner_dropdown_item,getTexts()));
        setContentView(listView);
    }
    //定义String数组
    public String[] getTexts(){
       String[] texts = {"apple","pear","banana","orange"};
        return texts;
    }
}
```
虽然两种写法都是可以的，但我们显然可以看到，第二种是通过新建ListView组件加载，并没有使用我们的配置文件，故此，我更为推荐第一种加载布局文件的写法。
