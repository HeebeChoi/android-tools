**下拉列表Spinne**<br>
> 通过 5 个步骤将 Spinner 初始化并进行事件处理，分别为：<br>
```java
    1、定义下拉列表的列表项内容 List<String>。 
    2、为下拉列表 Spinner 定义一个适配器 ArrayAdapter<String> ，并与列表项内容相关联。 
    3、使用 ArrayAdapter.setDropDownViewResource() 设置 Spinner 下拉列表在打开时的下拉菜单样式。 
    4、使用 Spinner. setAdapter() 将适配器数据与 Spinner 关联起来 
    5、为Spinner添加事件监听器，进行事件处理。 
```

> 布局文件:`activity_spiner.xml`<br>
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">
    <!--下拉菜单-->
    <Spinner
        android:id="@+id/spinner"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
    </Spinner>
</LinearLayout>
```

> 功能实现：`MainActivity.java`<br>
```java
package my.njpji.spinerdemo;
import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;
import android.content.DialogInterface;
import android.os.Bundle;
import android.view.MotionEvent;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.Spinner;
import java.util.ArrayList;
import java.util.List;
public class MainActivity extends AppCompatActivity {
    private Spinner spinner;
    private ArrayAdapter<String> adapter;
    private List<String> list;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //加载布局
        setContentView(R.layout.activity_spiner);
        //获取下拉菜单组件
        spinner = findViewById(R.id.spinner);
        //定义集合
        list = new ArrayList<String>();
        list.add("apple");
        list.add("banana");
        list.add("orange");
        list.add("pears");
        //为下拉菜单添加适配器
        adapter = new ArrayAdapter<String>(MainActivity.this,R.layout.support_simple_spinner_dropdown_item,list);
        //为下拉菜单组件设置适配器
        spinner.setAdapter(adapter);
        //给spinner每一个item项目设置监听器
        spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                //将选择的条目弹出对话框中，根据适配器position id获取
                alterDialog(adapter.getItem(position),"确认");
                //显示spinner
                parent.setVisibility(View.VISIBLE);
            }
            @Override
            public void onNothingSelected(AdapterView<?> parent) {
                parent.setVisibility(View.NOT_FOCUSABLE);
            }
        });

        /**
         *焦点改变事件处理
         */
        spinner.setOnFocusChangeListener(new View.OnFocusChangeListener() {
            @Override
            public void onFocusChange(View v, boolean hasFocus) {

            }
        });
        /**
         * OnTouchListener对内容选项触屏事件处理
         */
        spinner.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                //触屏选择项目后隐藏spinner
//                v.setVisibility(View.INVISIBLE);
                //触屏选择项目后不隐藏spinner
                v.setVisibility(View.VISIBLE);
                return false;
            }
        });
    }
    /**
     * @program 弹出框
     * @param message
     * @param t
     */
    public void alterDialog(String message,String t){
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setMessage(message);
        builder.setPositiveButton(t, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
            }
        });
        AlertDialog alertDialog = builder.create();
        alertDialog.show();
    }
}
```

> 上面我们提到了Spinner组件的用法，现在我们再来理解下他的过程，可能上面的稍微复杂了点
```java
package njpji.spinnerdemo;
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.Spinner;
import android.widget.Toast;
public class MainActivity extends AppCompatActivity {
    private ArrayAdapter<String> arrayAdapter;
    private String[] array;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //加载布局文件
        setContentView(R.layout.activity_spinner);
        //定义spinner控件的下拉内容
        array = new String[]{"vivo","huawei","apple"};
        //找到spinner控件
        Spinner spinner = findViewById(R.id.spinner_menu);
        //为spinner控件定义适配器adapter
        arrayAdapter = new ArrayAdapter<String>(MainActivity.this,R.layout.support_simple_spinner_dropdown_item,array);
        //将spinner控件和adapter绑定
        spinner.setAdapter(arrayAdapter);
        //为spinner控件设置监听器
        spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                //获取具体内容:根据定位position获取
                Toast.makeText(view.getContext(),parent.getAdapter().getItem(position).toString(),Toast.LENGTH_SHORT).show();
            }
            @Override
            public void onNothingSelected(AdapterView<?> parent) {

            }
        });

    }
}
```


