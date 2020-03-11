**数据存储SharedPerferences对象**
> &nbsp;&nbsp;&nbsp;&nbsp;我们之前写过一个登录案例，但是其中我们保存用户数据是通过输入输出流将用户信息保存在私有文本文件中来实现的，现在，我们通过Context对象
> 来创建SharedPerferences对象来存储数据。

> 于是我们现在来通过一个登录案例实现数据存储：

1）首先，我们需要配置基础的应用主题，和一些应用内需要的主题样式：`styles.xml`<br>
```xml
<resources>
    <!-- 应用基础主题 -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar"></style>
    <!--自定义输入框下划线的颜色-->
    <style name="MyEditText" parent="Theme.AppCompat.Light">
        <item name="colorControlNormal">@android:color/darker_gray</item>
        <item name="colorControlActivated">@android:color/holo_blue_bright</item>
    </style>
</resources>

```
2）应用的配置文件，里面包含一些Acvtivity跳转信息：`AndroidManifest.xml`<br>
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="my.njpji.qq">
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
        <activity android:name=".RegisterActivity"></activity>
        <activity android:name=".UserActivity"></activity>
    </application>
</manifest>
```
3）应用的布局文件：<br>
1.`activity_login.xml`<br>
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:background="@drawable/bgimg"
    android:paddingLeft="35dp"
    android:paddingRight="35dp"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:layout_height="match_parent">
    <LinearLayout
        android:layout_width="wrap_content"
        android:orientation="horizontal"
        android:layout_height="wrap_content">
        <!--QQ图标-->
        <ImageView
            android:id="@+id/qq_img"
            android:src="@drawable/qq_img"
            android:layout_width="wrap_content"
            android:padding="0dp"
            android:layout_marginTop="95dp"
            android:layout_height="wrap_content" />
        <!--QQ文本-->
        <TextView
            android:id="@+id/qq_text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:width="50dp"
            android:height="40dp"
            android:layout_marginLeft="3dp"
            android:layout_marginTop="89dp"
            android:text="QQ"
            android:textColor="@android:color/white"
            android:textSize="30dp" />
    </LinearLayout>
    <LinearLayout
        android:layout_width="match_parent"
        android:orientation="vertical"
        android:layout_marginTop="28dp"
        android:layout_height="wrap_content">
        <!--输入框：QQ号/手机号/邮箱-->
        <EditText
            android:id="@+id/username_etext"
            android:hint="QQ号/手机号/邮箱"
            android:textSize="23dp"
            android:layout_width="match_parent"
            android:theme="@style/MyEditText"
            android:maxLength="20"
            android:singleLine="true"
            android:textColor="@android:color/white"
            android:textColorHint="@android:color/secondary_text_dark"
            android:layout_height="wrap_content" />
        <!--输入框：密码-->
        <EditText
            android:id="@+id/password_etext"
            android:hint="密码"
            android:textSize="23dp"
            android:singleLine="true"
            android:maxLength="20"
            android:inputType="textPassword"
            android:layout_marginTop="3dp"
            android:theme="@style/MyEditText"
            android:textColor="@android:color/white"
            android:textColorHint="@android:color/secondary_text_dark"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />
        <!--登录按钮-->
        <Button
            android:id="@+id/login_btn"
            android:text="登录"
            android:textSize="23dp"
            android:alpha="0.9"
            android:textColor="@android:color/white"
            android:background="@drawable/btn_img"
            android:layout_marginTop="11dp"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />
    </LinearLayout>
    <RelativeLayout
        android:layout_marginTop="12dp"
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <LinearLayout
            android:layout_alignParentLeft="true"
            android:layout_width="wrap_content"
            android:orientation="horizontal"
            android:layout_height="wrap_content">
            <!--复选框：记住密码？-->
            <CheckBox
                android:id="@+id/ck_rem"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content" />
            <TextView
                android:id="@+id/rem_text"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginRight="0dp"
                android:text="记住密码？"
                android:textColor="@android:color/holo_blue_dark"
                android:textSize="16dp" />
        </LinearLayout>
        <!--新用户-->
        <TextView
            android:layout_alignParentRight="true"
            android:id="@+id/register_text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="新用户注册"
            android:textAlignment="viewStart"
            android:textColor="@android:color/holo_blue_dark"
            android:textSize="16dp" />
    </RelativeLayout>
    <LinearLayout
        android:layout_width="match_parent"
        android:orientation="horizontal"
        android:layout_height="wrap_content"
        android:layout_marginTop="320dp"
        android:gravity="center">
        <!--文本框：底部提示-->
        <TextView
            android:id="@+id/tips"
            android:text="登录即代表阅读并同意"
            android:textColor="@android:color/white"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content" />
        <!--文本框：服务条款-->
        <TextView
            android:id="@+id/server_role"
            android:text="服务条款"
            android:textColor="@android:color/holo_blue_dark"
            android:textSize="16dp"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content" />
    </LinearLayout>
</LinearLayout>
```
2.`activity_register.xml`<br>
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:orientation="vertical"
    android:paddingLeft="35dp"
    android:paddingRight="35dp"
    android:background="@drawable/rbgimg"
    android:layout_height="match_parent">
    <!--logo-->
    <ImageView
        android:id="@+id/rlogo"
        android:maxWidth="400dp"
        android:maxHeight="127dp"
        android:src="@drawable/rlogo"
        android:padding="0dp"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
    <!--输入框：手机号码-->
    <EditText
        android:id="@+id/phone_text"
        android:hint="手机号码"
        android:textSize="23dp"
        android:theme="@style/MyEditText"
        android:textColor="@android:color/white"
        android:textColorHint="@android:color/secondary_text_dark"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
    <!--输入框：密码-->
    <EditText
        android:id="@+id/pwd_etext"
        android:textSize="23dp"
        android:theme="@style/MyEditText"
        android:textColor="@android:color/white"
        android:layout_marginTop="12dp"
        android:layout_width="match_parent"
        android:textColorHint="@android:color/secondary_text_dark"
        android:hint="密码"
        android:layout_height="wrap_content" />
    <!--注册按钮-->
    <Button
        android:id="@+id/register_btn"
        android:text="注册"
        android:textSize="23dp"
        android:textColor="@android:color/white"
        android:layout_marginTop="15dp"
        android:alpha="0.77"
        android:background="@drawable/btn_img"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

</LinearLayout>
```
3.`activity_user.xml`<br>
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:background="@drawable/rbgimg"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/userinfo"
        android:height="40dp"
        android:textSize="23dp"
        android:textColor="@android:color/white"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
</LinearLayout>
```
4）功能实现：<br>
1.`MainActivity.java`<br>
```java
package my.njpji.qq;
import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;
import android.app.Activity;
import android.content.Context;
import android.content.DialogInterface;
import android.content.Intent;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.graphics.Color;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.EditText;
import android.widget.ImageView;
import android.widget.TextView;
import java.io.Serializable;
import java.util.HashMap;
import java.util.Map;
import my.njpji.qq.pojo.User;
public class MainActivity extends AppCompatActivity {
    private SharedPreferences sharedPreferences1;
    private SharedPreferences sharedPreferences2;
    private SharedPreferences sharedPreferences3;
    private SharedPreferences sharedPreferences4;
    private SharedPreferences sharedPreferences5;
    private SharedPreferences sharedPreferences6;
    private TextView qq_text;
    private ImageView qq_img;
    private EditText username_etext;
    private EditText password_etext;
    private Button login_btn;
    private TextView rem_text;
    private CheckBox ck_rem;
    private TextView register_text;
    private TextView server_role;
    private Context context;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //加载布局文件
        setContentView(R.layout.activity_login);
        context = MainActivity.this;
        //qq_text
        qq_text = findViewById(R.id.qq_text);
        //qq_img
        qq_img = findViewById(R.id.qq_img);
        qq_img.setColorFilter(Color.WHITE);//设置图片色彩过滤器
        //username_etext
        username_etext = findViewById(R.id.username_etext);
        //password_etext
        password_etext = findViewById(R.id.password_etext);
        //login_btn
        login_btn = findViewById(R.id.login_btn);
        //rem_text
        rem_text = findViewById(R.id.rem_text);
        rem_text.setOnClickListener(new Rem_textListener());
        //ck_rem
        ck_rem = findViewById(R.id.ck_rem);
        ck_rem.setClickable(false);
        //register_text
        register_text = findViewById(R.id.register_text);
        register_text.setOnClickListener(new RegisterListener());
        //server_role
        server_role = findViewById(R.id.server_role);
        server_role.setOnClickListener(new ServerRoleListener());

        //获取ck_rem的状态
        boolean ck_flag = getCk();
        ck_rem.setChecked(ck_flag);

        //login_btn
        login_btn = findViewById(R.id.login_btn);
        login_btn.setOnClickListener(new LoginListener());

        Map<String,String> map = getUserinfo(ck_rem.isChecked());
        if(map!=null){
            username_etext.setText(map.get("name"));
            password_etext.setText(map.get("pwd"));
        }

    }
    /**
     * program:用户登录
     * programmer:蔡熙贝
     * time:2020-3
     */
    class LoginListener implements OnClickListener{
        @Override
        public void onClick(View v) {
            remCk(ck_rem.isChecked());
            //获取输入框的信息
            String u_name = username_etext.getText().toString();
            String u_pwd = password_etext.getText().toString();
            //登录验证
            User user = login(u_name);
            if(user==null){
                alterDialog("用户不存在，请先去注册","我知道了");
            }else{
                if(u_pwd.equals(user.getPassword())){
                    //同时根据记住密码复选框状态保存用户信息
                    rem_userinfo(ck_rem.isChecked(),user.getUsername(),user.getPassword());
                    //通过bundle对象传递数据
                    jumpActivityWithExra(MainActivity.this,UserActivity.class,"user",user);
                }else {
                    alterDialog("密码输入错误，请重新输入..","我知道了");
                }
            }
        }
    }
    /**
     * program:该方法用于复选框文本点击响应设置复选框选中状态
     * programmer:蔡熙贝
     * time:2020-3
      */
    class Rem_textListener implements OnClickListener{
        @Override
        public void onClick(View v) {
            //获取复选框当前的状态
            boolean checked = ck_rem.isChecked();
            //反选
            ck_rem.setChecked(!checked);
        }
    }
    /**
     * program:该程序用于跳转到新用户注册页面
     * programmer:蔡熙贝
     * time:2020-3
     */
    class RegisterListener implements OnClickListener{
        @Override
        public void onClick(View v) {
            jumpActivity(MainActivity.this, RegisterActivity.class);
        }
    }
    /**
     * program:页面跳转方法，第一个参数context,第二个参数目标activity,第三、四个参数用于数据传递
     * programmer:蔡熙贝
     * time:2020-3
     */
    public void jumpActivity(Context context,Class<?> activity){
        Intent intent = new Intent(context,activity);
        startActivity(intent);
    }

    /**
     * @program 传递数据
     * @programmer 蔡熙贝
     * @time 2020-3
     * @param context
     * @param activity
     * @param user
     */
    public void jumpActivityWithExra(Context context,Class<?> activity,String args,User user){
        Intent intent = new Intent(context,activity);
        Bundle bundle = new Bundle();
        //bundle绑定数据
        bundle.putSerializable(args,user);
        //存放数据
        intent.putExtras(bundle);
        startActivity(intent);
    }

    /**
     * program:该监听器用于弹出服务条款
     * programmer:蔡熙贝
     * time:2020-3
     */
    class ServerRoleListener implements OnClickListener{
        @Override
        public void onClick(View v) {
            alterDialog("服务条款\n" +
                    " \n" +
                    "服务条款的确认和接纳\n" +
                    "\n" +
                    "腾讯邮箱服务是由腾讯公司（下称“腾讯”）提供的，包括但不限于以下几类邮箱服务：\n" +
                    "@qq.com后缀的邮箱服务\n" +
                    "@vip.qq.com后缀的邮箱服务\n" +
                    "@foxmail.com后缀的邮箱服务\n" +
                    "腾讯域名邮箱服务\n" +
                    "腾讯企业邮箱服务\n" +
                    "当用户使用腾讯邮箱服务时，都受本协议约束；另外，各类邮箱服务有特殊约定的，同时受该约定所约束。","我知道了");
        }
    }

    /**
     * program:弹出框
     * programmer:蔡熙贝
     * time:2020-3
     */
    public void alterDialog(String message,String args){
        AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this);
        builder.setMessage(message);
        builder.setPositiveButton(args, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
            }
        });
        AlertDialog dialog = builder.create();
        dialog.show();
    }

    /**
     * program:记住复选框的状态
     * programmer:蔡熙贝
     * time:2020-3
     */
    public void remCk(boolean checked){
        //创建sharedPreferences对象，该对象通过getSharedPreferences()方法创建之后，在data/date/包/下可以找到
        sharedPreferences1 = context.getSharedPreferences("ckinfo", context.MODE_PRIVATE);
        //存入数据：第一个数据是key，第二个数据是value---以键值对保存
        Editor editor = sharedPreferences1.edit();
        if(checked==true){
            editor.putString("flag","true");
        }else{
            editor.putString("flag","false");
        }
        editor.commit();
    }
    /**
     * program:获取复选框的状态
     * programmer:蔡熙贝
     * time:2020-3
     */
    public boolean getCk(){
        //创建sharedPreferences对象，该对象通过getSharedPreferences()方法创建之后，在data/date/包/下可以找到
        sharedPreferences2 = context.getSharedPreferences("ckinfo",context.MODE_PRIVATE);
        //存入数据：第一个数据是key，第二个数据是value---以键值对保存
        String checked = sharedPreferences2.getString("flag","");
        if(checked=="true" || checked.equals("true")){
            return true;
        }else{
            return  false;
        }
    }

    /**
     * program:用户登录
     * programmer:蔡熙贝
     * time:2020-3
     */
    public User login(String username){
        User user = new User();
        //创建sharedPreferencesh读取数据
        sharedPreferences3 = context.getSharedPreferences("registerinfo",context.MODE_PRIVATE);
        //获取数据
        String pwd = sharedPreferences3.getString(username,"");   //获取到的是一个密码
        if(pwd!="" && pwd!=null){
            user.setUsername(username);
            user.setPassword(pwd);
        }
        return user;
    }
    /**
     * @program 保存登录信息
     * @programmer:蔡熙贝
     * @time 2020-3
     */
    public void rem_userinfo(boolean checked,String username,String password){
        sharedPreferences4 = context.getSharedPreferences("userinfo",context.MODE_PRIVATE);
        Editor editor = sharedPreferences4.edit();
        if(checked==true){
            editor.putString("username",username);
            editor.putString("password",password);
            editor.commit();
        }
    }
    /**
     * @program 提取登录信息
     * @programmer:蔡熙贝
     * @time 2020-3
     */
    public Map<String,String> getUserinfo(boolean checked){
        Map<String,String> map = new HashMap<String,String>();
        sharedPreferences5 = context.getSharedPreferences("userinfo",context.MODE_PRIVATE);
        if(checked==true){
            String name = sharedPreferences5.getString("username","");
            String pwd = sharedPreferences5.getString("password","");
            if(name!="" && pwd!=""){
                map.put("name",name);
                map.put("pwd",pwd);
            }
        }
        return map;
    }
}
```
2.`RegisterActivity.java`<br>
```java
package my.njpji.qq;
import android.app.Activity;
import android.content.Context;
import android.content.DialogInterface;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.EditText;
import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;
import my.njpji.qq.pojo.User;
public class RegisterActivity extends AppCompatActivity {
    private Context context;
    private EditText rusername;
    private EditText rpassword;
    private Button rbtn;
    private SharedPreferences sharedPreferences;
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //加载布局文件
        setContentView(R.layout.activity_register);
        context = RegisterActivity.this;
        //rusername
        rusername = findViewById(R.id.phone_text);
        //rpassword
        rpassword = findViewById(R.id.pwd_etext);
        //rbtn
        rbtn = findViewById(R.id.register_btn);
        //给按钮设置监听器
        rbtn.setOnClickListener(new RegisterUserListener());
    }

    /**
     * program:注册用户
     * programmer：蔡熙贝
     * time:2020-3
     */
    class RegisterUserListener implements OnClickListener{
        @Override
        public void onClick(View v) {
            //获取文本框内容
            String username = rusername.getText().toString();
            String password = rpassword.getText().toString();
            //用户封装
            User user = new User(username,password);
            //进行注册
            registerUser(user);
        }
    }

    /**
     * program:弹出框
     * programmer:蔡熙贝
     * time:2020-3
     */
    public void alterDialog(String message,String args){
        AlertDialog.Builder builder = new AlertDialog.Builder(RegisterActivity.this);
        builder.setMessage(message);
        builder.setPositiveButton(args, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
            }
        });
        AlertDialog dialog = builder.create();
        dialog.show();
    }
    /**
     * program:用户注册信息保存
     * programmer:蔡熙贝
     * time:2020-3
     */
    public void registerUser(User user){
        //创建sharedPreferences对象，该对象通过getSharedPreferences()方法创建之后，在data/date/包/下可以找到
        sharedPreferences = context.getSharedPreferences("registerinfo",Context.MODE_PRIVATE);
        //存入数据：第一个数据是key，第二个数据是value---以键值对保存
        Editor editor = sharedPreferences.edit();
        editor.putString(user.getUsername(),user.getPassword());
        editor.commit();
        alterDialog("用户注册成功，前往登录界面登录...","我知道了");
    }
}
```
3.`UserActivity.java`<br>
```java
package my.njpji.qq;
import android.content.Intent;
import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import my.njpji.qq.pojo.User;
public class UserActivity extends AppCompatActivity {
    private TextView userinfo;
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //加载布局
        setContentView(R.layout.activity_user);
        //userinfo
        userinfo = findViewById(R.id.userinfo);
        //获取传递的数据
        Intent intent = getIntent();
        Bundle bundle = intent.getExtras();
        User user = (User) bundle.getSerializable("user");
        //设置文本信息
        userinfo.setText("hello,"+user.getUsername());
    }
}
```
5）用户实例：`User.java`<br>
```java
package my.njpji.qq.pojo;
import java.io.Serializable;
public class User implements Serializable {
    private String username,password;
    public User() {
    }
    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }
    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
```
> 从我们的以上案例中，我们可以看见我们可以通过`context`全局对象创建`SharedPreferences`对象实现数据的保存：
```java
//创建sharedPreferences对象，该对象通过getSharedPreferences()方法创建之后，在data/date/包/下可以找到
sharedPreferences1 = context.getSharedPreferences("ckinfo", context.MODE_PRIVATE);
//存入数据：第一个数据是key，第二个数据是value---以键值对保存
Editor editor = sharedPreferences1.edit();
if(checked==true){
    editor.putString("flag","true");
}else{
    editor.putString("flag","false");
}
//提交保存
editor.commit();
```java
> 同时，我们也可以从``对象中，获取保存的数据：
```java
//创建sharedPreferences对象，该对象通过getSharedPreferences()方法创建之后，在data/date/包/下可以找到
sharedPreferences2 = context.getSharedPreferences("ckinfo",context.MODE_PRIVATE);
//存入数据：第一个数据是key，第二个数据是value---以键值对保存
String checked = sharedPreferences2.getString("flag","");
```
> SharedPreferences对象相关介绍：
```
SharedPreferences
SharedPreferences 是 Android 系统提供的一个通用的数据持久化框架，用于存储和读取 key-value 类型的原始基本数据对。

目前仅支持 boolean、float、int、long 和 string 等基本类型的存储，对于自定义的复合数据类型，是无法使用 SharedPreferences 进行存储的。

SharedPreferences 主要用于存储系统的配置信息，类似于 Windows 下常用的 .ini 文件。

例如上次登录的用户名、上次最后设置的信息等，通过保存上一次用户所做的修改或者自定义参数设定，当再次启动程序后依然保持原有设置。它是用键值对的方式存储的，方便管理写入和读取。

使用 SharedPreferences 的步骤如下：
1) 获取Preferences
每个 Activity 默认都有一个 SharedPreferences 对象，获取 SharedPreferences 对象的方法有两种：

1）SharedPreferences getSharedPreferences(String name, int mode)。

使用该方法获取 name 指定的 SharedPreferences 对象，并获取对该 SharedPreferences 对象的读写控制权。

当应用程序中可能使用到多个 SharedPreferences 时使用该方法。

2）SharedPreferences getPreferences(int mode)。

当应用程序中仅需要一个SharedPreferences对象时，使用该方法获取当前 Activity 对应的 SharedPreferences，而不需要指定 SharedPreferences 的名字。

其中，参数 mode 有 4 种取值，分别是：
MODE_PRIVATE：默认方式，只能被创建的应用程序或者与创建的应用程序具有相同用户 ID 的应用程序访问。
MODE_WORLD_READABLE：允许其他应用程序对该 SharedPreferences 文件进行读操作。
MODE_WORLD_WRITEABLE：允许其他应用程序对该 SharedPreferences 文件进行写操作。
MODE_MULTI_PROCESS：在多进程应用程序中，当多个进程都对同一个 SharedPreferences 进行访问时，该文件的每次修改都会被重新核对。
2) 获取 SharedPreferences.Editor
调用 edit() 方法获取 SharedPreferences.Editor，SharedPreferences 通过该接口对其内容进行更新。
3) 更新 SharedPreferences
通过 SharedPreferences.Editor 接口提供的 put 方法对 SharedPreferences 进行更新。

例如使用 putBoolean(String key, boolean value)、putFloat(String key, float value) 等方法将相应数据类型的数据与其 key 对应起来。
4) 提交
调用 SharedPreferences.Editor 的 commit() 方法将更新提交到 SharedPreferences 中。

5）SharedPreferences setinfo = getPreferences(Activity.MODE_PRIVATE);
用于获取当前 Activity 默认的 SharedPreferences 对象，该对象没有名字。当然，也可以通过 getSharedPreferences(String name, int mode) 方法来创建并获取一个带有名字的 SharedPreferences。
该 SharedPreferences 被创建后，可以在应用程序的包路径下，即 data/data/<your package name>/shared_prefs 文件夹下找到该文件。
```

