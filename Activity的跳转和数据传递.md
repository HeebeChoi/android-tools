**Activity的跳转和数据传递**<br>
> <p>&nbsp;&nbsp;&nbsp;&nbsp;我们之前，通过Intent对象可以显式和隐式的跳转activity对象，现在我们通过案例来具体的实现。</p>
  
配置文件：`AndroidManifest.xml`<br>
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="my.njpji.activitydemo">
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name=".UserActivity"></activity>
    </application>
</manifest>
```
布局文件：`activity_main.xml`<br>
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <!--输入框：输入的内容需要传递给UserActivity-->
    <EditText
        android:id="@+id/username"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="请输入要传入的值"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <!--确认按钮-->
    <Button
        android:id="@+id/confirm"
        android:text="确认"
        app:layout_constraintTop_toBottomOf="@+id/username"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

    <!--用来接收从UserActivity传来的数据-->
    <TextView
        android:id="@+id/textFromUserActivity"
        android:layout_width="match_parent"
        android:height="40dp"
        app:layout_constraintTop_toBottomOf="@+id/confirm"
        android:layout_height="wrap_content" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
布局文件：`activity_user.xml`<br>
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!--用来显示MainActivity传来的数据-->
    <TextView
        android:id="@+id/text_message"
        android:layout_width="match_parent"
        android:height="40dp"
        android:layout_height="wrap_content" />
</LinearLayout>
```

功能代码：`MainActivity.java`<br>
```java
public class MainActivity extends AppCompatActivity {
    private EditText username;
    private Button confirm;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //获取editText组件
        username = findViewById(R.id.username);
        //获取button组件
        confirm = findViewById(R.id.confirm);
        //给按钮设置监听器
        confirm.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                //获取输入框内容
                String message = username.getText().toString();
                //设置意图对象
                Intent intent = new Intent(MainActivity.this,UserActivity.class);
                //添加数据
                intent.putExtra("message",message);
                startActivity(intent);
            }
        });
    }
}
```
功能代码：`UserActivity.java`<br>
```java
public class UserActivity extends AppCompatActivity {
    private TextView message;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //加载布局文件
        setContentView(R.layout.activity_user);
        //接受来自MainActivity的数据
        Intent intent = getIntent();
        String text_message = intent.getStringExtra("message");
        //获取组件textView
        message = findViewById(R.id.text_message);
        //设置内容
        message.setText(text_message);
    }
}
```

> 然后，以上内容呢，是实现从入口Activity传送数据到UserActivity,那么如果我们需要从UserActivity中传送数据回到MainActivity中呢？
> 如下：






















