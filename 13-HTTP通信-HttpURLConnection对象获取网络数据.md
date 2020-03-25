**HTTP通信-HttpURLConnection对象获取网络数据**
>URLConnection 内建对多种网络协议的支持，如 HTTP/HTTPS、File、FTP 等。
>在创建连接之前，可以对连接的一些属性进行设置，如下所示。
```java
URLConnection属性 属性名称 	属性描述
setReadTimeout(3000) 	设置读取数据的超时时间为 3 秒钟
setUseCaches(false) 	设置当前连接是否允许使用缓存
setDoOutput(true) 	设置当前连接是否允许建立输出流
setDoInput(true) 	设置当前连接是否允许建立输入流
```

> HttpURLConnection 继承于 URLConnection 类，二者都是抽象类，所以无法直接实例化，其对象主要通过 URL 的 openConnection 方法获得。
> URLConnection 可以直接转换成 HttpURLConnection，以便于使用一些 HTTP 连接特定的方法，如 getResponseMessage()、setRequestMethod() 等。

> 案例如下：内部含有Handler的使用（消息处理机制）
```java
package njpji.httpdemo;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
public class MainActivity extends AppCompatActivity {
    private TextView textView;
    private String data;
    private Button button;
    @Override
    protected void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        //加载布局文件
        setContentView(R.layout.activity_main);
        //获取text组件
        textView = findViewById(R.id.text);
        button = findViewById(R.id.button);
        //按钮
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //子线程，通知主线程更新界面
                Thread thread = new Thread(new Runnable() {
                    @Override
                    public void run() {
                        //访问网路资源
                        try {
                            //定义url
                            URL url = new URL("https://www.baidu.com/");
                            //创建HttpUrlConnect对象
                            HttpURLConnection httpURLConnection = (HttpURLConnection) url.openConnection();
                            httpURLConnection.setDoInput(true);
                            httpURLConnection.setDoOutput(true);
                            httpURLConnection.setRequestMethod("GET");
                            //连接
                            httpURLConnection.connect();
                            //输入流
                            InputStream inputStream = httpURLConnection.getInputStream();
                            //读取输入流
                            InputStreamReader inputStreamReader = new InputStreamReader(inputStream);
                            //缓冲流
                            BufferedReader bufferedReader = new BufferedReader(inputStreamReader);
                            data = "";
                            while ((bufferedReader.readLine())!=null){
                                data += bufferedReader.readLine();
                            }
                            //消息处理，将data数据绑定到message中
                            Message message = new Message();
                            message.obj = data;
                            //向handler对象发送数据
                            handler.sendMessage(message);
                        }catch (Exception e){
                            e.getStackTrace();
                        }
                    }
                });
                thread.start();
            }
        });
    }
    //Handler接收来自子线程的信息,并用于更新UI界面
    Handler handler = new Handler(){
        @Override
        public void handleMessage(@NonNull Message msg) {
            String s = (String)msg.obj;
            textView.setText("内容："+s);
        }
    };
}
```
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ScrollView
        android:id="@+id/scrollView2"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="64dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/button">
        <!--文本框-->
        <TextView
            android:id="@+id/text"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_marginTop="64dp"
            android:layout_marginBottom="581dp"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/button" />
    </ScrollView>


    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="4dp"
        android:layout_marginBottom="16dp"
        android:text="获取"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />


</androidx.constraintlayout.widget.ConstraintLayout>
```

> 除了让子线程通过handlerMessage()让主线程更新界面，还可以通过runOnUiThread(Runable action)
```java
package njpji.httpdemo;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
public class MainActivity extends AppCompatActivity {
    private TextView textView;
    private String data;
    private Button button;
    @Override
    protected void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        //加载布局文件
        setContentView(R.layout.activity_main);
        //获取text组件
        textView = findViewById(R.id.text);
        button = findViewById(R.id.button);
        //按钮
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //子线程
                Thread thread = new Thread(new Runnable() {
                    @Override
                    public void run() {
                        //访问网路资源
                        try {
                            //定义url
                            URL url = new URL("https://www.baidu.com/");
                            //创建HttpUrlConnect对象
                            HttpURLConnection httpURLConnection = (HttpURLConnection) url.openConnection();
                            httpURLConnection.setDoInput(true);
                            httpURLConnection.setDoOutput(true);
                            httpURLConnection.setRequestMethod("GET");
                            //连接
                            httpURLConnection.connect();
                            //输入流
                            InputStream inputStream = httpURLConnection.getInputStream();
                            //读取输入流
                            InputStreamReader inputStreamReader = new InputStreamReader(inputStream);
                            //缓冲流
                            BufferedReader bufferedReader = new BufferedReader(inputStreamReader);
                            data = "";
                            while ((bufferedReader.readLine())!=null){
                                data += bufferedReader.readLine();
                            }
                            Message message = new Message();
                            message.obj = data;
//                            handler.sendMessage(message);
                            //更新UI
                            runOnUiThread(new Runnable() {
                                @Override
                                public void run() {
                                    textView.setText(data);
                                }
                            });
                        }catch (Exception e){
                            e.getStackTrace();
                        }
                    }
                });
                thread.start();
            }
        });
    }
//    //接收来自子线程的信息
//    Handler handler = new Handler(){
//        @Override
//        public void handleMessage(@NonNull Message msg) {
//            String s = (String)msg.obj;
//            textView.setText("内容："+s);
//        }
//    };
}
```
> 但个人推荐使用Thread+handler的方式去实现异步更新UI操作。

