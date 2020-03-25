**WebView组件加载网络页面**
> 在此之前，推荐大家看一下官方文档：[WebView](https://developer.android.google.cn/guide/webapps/webview)

> 可以看见，如果是在onCreate()页面加载网页，就需要以下思路：
```java
//activityContext当前的activity对象，例如：MainActivity.this
WebView myWebView = new WebView(activityContext);
setContentView(myWebView);
```

> 另外，也可以，利用布局文件：
```xml
<WebView
    android:id="@+id/webview"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
/>
```
```java
WebView myWebView = (WebView) findViewById(R.id.webview);
myWebView.loadUrl("http://www.example.com");
```

> 但是光是完成上面两种方法，是远远不够的，因为你会发现加载的页面无法点击链接，因为现在大部分的网页链接都是使用了ajax的，如下：
```html
<a href="javascript:void(0)" onclick="xxx()"></a>
```

> 所以，我们需要android对javascript的支持：
```java
//通过websetting对象设置webview组件对javascript文件的支持
WebSettings webSettings = webView.getSettings();
webSettings.setJavaScriptEnabled(true);
```

> 但是最后，我们需要注意的是，既然是网络，我们应用程序就需要授权访问网络，在配置文件添加：
```xml
<!--网络权限-->
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
```

> 案例:在onCreate()通过WebView加载github主页：
```java
package njpji.webviewdemo;
import androidx.appcompat.app.AppCompatActivity;

import android.Manifest;
import android.os.Bundle;
import android.webkit.WebSettings;
import android.webkit.WebView;
public class MainActivity extends AppCompatActivity {
    private WebView webView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //定义webView组件
        webView = new WebView(MainActivity.this);
        webView.loadUrl("https://github.com/");
        //通过websetting对象设置webview组件对javascript文件的支持
        WebSettings webSettings = webView.getSettings();
        webSettings.setJavaScriptEnabled(true);
        setContentView(webView);
    }
}
```
> 其中，因为我们直接在onCreate()方法使用的WebView，所以并不需要添加布局文件。

> 案例2：不在onCreate()页面加载，使用布局文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView
    android:layout_height="match_parent"
    android:layout_width="match_parent"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:orientation="vertical"
        android:layout_height="match_parent">

        <ScrollView
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
            <!--下拉菜单Spinner-->
            <Spinner
                android:id="@+id/spinner"
                android:background="@android:color/white"
                android:layout_width="match_parent"
                android:layout_height="300dp">
            </Spinner>
        </ScrollView>

        <!--WebView组件-->
        <WebView
            android:id="@+id/webView"
            android:background="@android:color/white"
            android:layout_width="match_parent"
            android:layout_height="match_parent">
        </WebView>
    </LinearLayout>
</ScrollView>
```
```java
package my.github;
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.Spinner;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Set;
public class MainActivity extends AppCompatActivity {
    private Spinner spinner;
    private WebView webView;
    private ArrayAdapter<String> arrayAdapter;
    private HashMap<String,String> map;
    private ArrayList<String> arrayList;
    private String url;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //定义map
        map = new HashMap<String,String>();
        map.put("1、Android体系结构","https://github.com/PuiPui-Java/android-tools/blob/master/01-Android%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84.md");
        map.put("2、电话拨号器","https://github.com/PuiPui-Java/android-tools/blob/master/02-%E7%94%B5%E8%AF%9D%E6%8B%A8%E5%8F%B7%E5%99%A8.md");
        map.put("3、ListView组件及常见适配器","https://github.com/PuiPui-Java/android-tools/blob/master/03-ListView%E7%BB%84%E4%BB%B6%E5%8F%8A%E5%B8%B8%E8%A7%81%E9%80%82%E9%85%8D%E5%99%A8.md");
        map.put("4、按钮监听器","https://github.com/PuiPui-Java/android-tools/blob/master/04-%E6%8C%89%E9%92%AE%E7%9B%91%E5%90%AC%E5%99%A8.md");
        map.put("05、弹出框-AlertDialog","https://github.com/PuiPui-Java/android-tools/blob/master/05-%E5%BC%B9%E5%87%BA%E6%A1%86-AlertDialog.md");
        map.put("06、路由","https://github.com/PuiPui-Java/android-tools/blob/master/06-%E8%B7%AF%E7%94%B1.md");
        map.put("07、登录案例","https://github.com/PuiPui-Java/android-tools/blob/master/07-%E7%99%BB%E5%BD%95%E6%A1%88%E4%BE%8B.md");
        map.put("08、Activity的跳转和数据传递","https://github.com/PuiPui-Java/android-tools/blob/master/08-Activity%E7%9A%84%E8%B7%B3%E8%BD%AC%E5%92%8C%E6%95%B0%E6%8D%AE%E4%BC%A0%E9%80%92.md");
        map.put("09、本地数据存储SharedPerferences","https://github.com/PuiPui-Java/android-tools/blob/master/09-%E6%9C%AC%E5%9C%B0%E6%95%B0%E6%8D%AE%E5%AD%98%E5%82%A8SharedPerferences%E5%AF%B9%E8%B1%A1.md");
        map.put("10、下拉列表Spinner","https://github.com/PuiPui-Java/android-tools/blob/master/10-%E4%B8%8B%E6%8B%89%E5%88%97%E8%A1%A8Spinner.md");
        map.put("11、Android滚动视图","https://github.com/PuiPui-Java/android-tools/blob/master/11-Android%E6%BB%9A%E5%8A%A8%E8%A7%86%E5%9B%BE.md");
        map.put("12、Android HTTP通信介绍","https://github.com/PuiPui-Java/android-tools/blob/master/12-Android%20%20HTTP%E9%80%9A%E4%BF%A1%E4%BB%8B%E7%BB%8D.md");
        map.put("13、HTTP通信-HttpURLConnection对象获取网络数据","https://github.com/PuiPui-Java/android-tools/blob/master/13-HTTP%E9%80%9A%E4%BF%A1-HttpURLConnection%E5%AF%B9%E8%B1%A1%E8%8E%B7%E5%8F%96%E7%BD%91%E7%BB%9C%E6%95%B0%E6%8D%AE.md");
        map.put("14、WebView组件加载网络页面","https://github.com/PuiPui-Java/android-tools/blob/master/14-WebView%E7%BB%84%E4%BB%B6%E5%8A%A0%E8%BD%BD%E7%BD%91%E7%BB%9C%E9%A1%B5%E9%9D%A2.md");
        //将Key用数组保存
        arrayList = new ArrayList<String>();
        Set<String> set = map.keySet();
        for(String k:set){
            arrayList.add(k);
        }
        //加载布局文件
        setContentView(R.layout.activity_main);
        //spinner
        spinner = findViewById(R.id.spinner);
        //webView
        webView = findViewById(R.id.webView);
        //给spinner设置适配器
        arrayAdapter = new ArrayAdapter<String>(this,R.layout.support_simple_spinner_dropdown_item,arrayList);
        spinner.setAdapter(arrayAdapter);
        //spinner选项监听
        spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                //获取选项的url值
                url = map.get(parent.getAdapter().getItem(position).toString());
                //子线程更新UI,让耗时进程交付给子线程处理
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        WebSettings webSettings = webView.getSettings();
                        webSettings.setJavaScriptEnabled(true);
                        webView.loadUrl(url);
                    }
                });
            }
            @Override
            public void onNothingSelected(AdapterView<?> parent) {
                //啥也不干
            }
        });
    }
}
```
> 最终效果：
<img src="https://github.com/PuiPui-Java/android-tools/blob/master/QQ%E6%88%AA%E5%9B%BE20200325183542.png"></img>

> 你可以看见，我们在此加载了我们的布局文件，然后使用runOnUiThread()去刷新我们的UI界面。



































