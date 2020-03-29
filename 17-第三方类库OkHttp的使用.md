**OkHttp第三方库**
> 首先呢，我们来介绍一下，OkHttp第三方库：
```xml
HTTP is the way modern applications network. It’s how we exchange data & media. Doing HTTP efficiently makes your stuff load faster and saves bandwidth.
OkHttp is an HTTP client that’s efficient by default:
    HTTP/2 support allows all requests to the same host to share a socket.
    Connection pooling reduces request latency (if HTTP/2 isn’t available).
    Transparent GZIP shrinks download sizes.
    Response caching avoids the network completely for repeat requests.
OkHttp perseveres when the network is troublesome: it will silently recover from common connection problems. If your service has multiple IP addresses OkHttp will attempt alternate addresses if the first connect fails. This is necessary for IPv4+IPv6 and services hosted in redundant data centers. OkHttp supports modern TLS features (TLS 1.3, ALPN, certificate pinning). It can be configured to fall back for broad connectivity.
Using OkHttp is easy. Its request/response API is designed with fluent builders and immutability. It supports both synchronous blocking calls and async calls with callbacks.
```
翻译：
```xml
HTTP是现代应用程序网络的方式。这就是我们交换数据和媒体的方式。有效地执行HTTP可使您的内容加载更快并节省带宽。
OkHttp是默认情况下有效的HTTP客户端：
    HTTP / 2支持允许对同一主机的所有请求共享一个套接字。
    连接池可减少请求延迟（如果HTTP / 2不可用）。
    透明的GZIP缩小了下载大小。
    响应缓存可以完全避免网络重复请求。
当网络出现问题时，OkHttp会坚持不懈：它将从常见的连接问题中静默恢复。如果您的服务具有多个IP地址，则在第一次连接失败时，OkHttp将尝试使用备用地址。这对于IPv4 + IPv6和冗余数据中心中托管的服务是必需的。OkHttp支持现代TLS功能（TLS 1.3，ALPN，证书固定）。可以将其配置为回退以实现广泛的连接。
使用OkHttp很容易。它的请求/响应API具有流畅的构建器和不变性。它支持同步阻塞调用和带有回调的异步调用
```
> 我们想要使用OkHttp，就需要在build.gradle文件中对应module添加依赖：
```xml
//OkHttp的依赖
implementation 'com.squareup.okhttp3:okhttp:4.4.0'
```
> 添加完之后，同步一下项目即可...

> 然后，既然是网络请求，我们就需要开启对网络访问的权限,在清单文件中添加：
```xml
<!--网络权限-->
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
```

> 然后我们通过一个简单的案例，来了解一下OkHttp的请求，我们就去获取解析一段JSON数据到我们的安卓界面：
```python
一个url: https://www.wanandroid.com/navi/json
```

> 我们把获取的数据打印到我们的ListView组件中：
```xml
布局文件：
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ListView
        android:id="@+id/list_content"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    </ListView>

</LinearLayout>

```

> 我们看一下上面的url得到的json数据是啥样的：
```json
{
  "data": [
    {
      "articles": [
        {
          "apkLink": "",
          "audit": 1,
          "author": "小编",
          "canEdit": false,
          "chapterId": 272,
          "chapterName": "常用网站",
          "collect": false,
          "courseId": 13,
          "desc": "",
          "descMd": "",
          "envelopePic": "",
          "fresh": false,
          "id": 1848,
          "link": "https://developers.google.cn/",
          "niceDate": "2018-01-07 18:59",
          "niceShareDate": "未知时间",
          "origin": "",
          "prefix": "",
          "projectLink": "",
          "publishTime": 1515322795000,
          "selfVisible": 0,
          "shareDate": null,
          "shareUser": "",
          "superChapterId": 0,
          "superChapterName": "",
          "tags": [],
          "title": "Google开发者",
          "type": 0,
          "userId": -1,
          "visible": 0,
          "zan": 0
        },
        ....
        ....
        ],
      "cid": 430,
      "name": "Flutter"
    }
  ],
  "errorCode": 0,
  "errorMsg": "aaaa"
}
```
> 简化点：
```
{
  "data":[
     {
        "article":[jsonArray对象]
        "cid":430
        "name":Flutter
     }
  ],
  "errorCode":0,
  "errorMsg":"aaaa"
}
```


> 我们主要是获取data里面的数据啊，具体的解析JSON数据的过程，看下面的业务代码：
```java
package njpji.okhttpdemo;
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;
import android.widget.Toast;
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import okhttp3.Call;
import okhttp3.Callback;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;
public class MainActivity extends AppCompatActivity {
    private ListView list_content;
    private List<String> arrayList;
    private static final String URL = "https://www.wanandroid.com/navi/json";
    private String result;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //加载布局文件
        setContentView(R.layout.activity_okhttp);
        //list_content
        list_content = findViewById(R.id.list_content);
        arrayList = new ArrayList<>();
        //OkHttp
        okhttp();

    }

    //第三方库OkHttp的使用
    public void okhttp() {
        //1、创建OkHttpClient对象
        OkHttpClient okHttpClient = new OkHttpClient();
        //2、创建Request对象
        Request request = new Request.Builder().url(URL).get().build();
        //3、创建Call对象
        Call call = okHttpClient.newCall(request);
        //4、异步请求
        call.enqueue(new Callback() {
            @Override
            public void onFailure(Call call, IOException e) {
                //当请求失败的时候
                Log.i("onFailure", e.getMessage());
            }

            @Override
            public void onResponse(Call call, Response response) throws IOException {
                //当请求得到相应之后
                result = response.body().string();
                try {
                    JSONObject json = new JSONObject(result);
                    JSONArray data_array = json.getJSONArray("data");
                    for (int i = 0; i < data_array.length(); i++) {
                        JSONObject data_obj = data_array.getJSONObject(i);
                        JSONArray articles_array = data_obj.getJSONArray("articles");
                        for (int j = 0; j < articles_array.length(); j++) {
                            JSONObject articles_obj = articles_array.getJSONObject(j);
                            String title = articles_obj.getString("title");
                            arrayList.add(title);
                        }
                        runOnUiThread(new Runnable() {
                            @Override
                            public void run() {
                                //设置适配器
                                list_content.setAdapter(new ArrayAdapter<String>(MainActivity.this,R.layout.support_simple_spinner_dropdown_item,arrayList));
                                //设置每一个选项的监听
                                list_content.setOnItemClickListener(new AdapterView.OnItemClickListener() {
                                    @Override
                                    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                                        String title = parent.getAdapter().getItem(position).toString();
                                        Toast.makeText(view.getContext(),title,Toast.LENGTH_SHORT).show();
                                    }
                                });
                            }
                        });
                    }
                } catch (JSONException e) {
                    e.printStackTrace();
                }
            }
        });
        //同步请求
//        new Thread(new Runnable() {
//            @Override
//            public void run() {
//                try {
//                    Response response = call.execute();
//                    //...后面和异步一样操作
//                } catch (IOException e) {
//                    e.printStackTrace();
//                }
//            }
//        });
    }
}
```

> 注意，UI界面更新放在子线程！！！

> 最后，我们获取的数据界面大致：<br>
<img src=""></img>

















