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

```




































