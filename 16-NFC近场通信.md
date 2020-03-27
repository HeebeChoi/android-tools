**NFC近场通信**
> 1、必须先在 AndroidManifest.xml 文件中声明以下内容，然后才能访问设备的 NFC 硬件并正确处理 NFC Intent：
```xml
<uses-permission android:name="android.permission.NFC" />
```
> 2、应用支持的最低 SDK 版本。API 级别 9 仅通过 ACTION_TAG_DISCOVERED 支持有限的标签调度，
> 并且只能通过 EXTRA_NDEF_MESSAGES extra 提供对 NDEF 消息的访问权限。无法访问其他任何标签属性或 I/O 操作。
> API 级别 10 提供全面的读取器/写入器支持以及前台 NDEF 推送功能；API 级别 14 则提供了一种更简便的方式（即，
> 使用 Android Beam 将 NDEF 消息推送到其他设备），同时提供了用于创建 NDEF 记录的其他便捷方法。
```xml
<uses-sdk android:minSdkVersion="10"/>   
```

> 3、uses-feature 元素，以便您的应用仅在那些具备 NFC 硬件的设备的 Google Play 中显示：
```xml
<uses-feature android:name="android.hardware.nfc" android:required="true" />
```

> 4、如果您的应用使用 NFC 功能，但该功能对您的应用来说并不重要，您可以省略 uses-feature 元素，并在运行时
> 通过检查 getDefaultAdapter() 是否为 null 来了解 NFC 的可用性。

> 5、nfc的intent-filter对象,您用于处理nfc的activity需要在清单文件中添加：
```xml
<!--nfc的intent对象，是哪一个activity处理，就是放在谁的里面-->
<intent-filter>
    <action android:name="android.nfc.action.NDEF_DISCOVERED" />
    <category android:name="android.intent.category.DEFAULT" />
</intent-filter>
```

**调度系统**

> NFC调度是指手机检测到NFC对象后如何处理，调度系统分为前台调度系统（Foreground Dispatch System）和标签调度系统
>（NFC Tag Dispatch System）。

> 1) 前台调度系统
> NFC前台调度系统是一种用于在运行的程序中（前台呈现的Activity）处理Tag的技术，即前台调度系统允许Activity拦截Intent对象，并且声明该Activity的优先级比其他的处理Intent对象的Activity高。前台调度系统在一些涉及需要在前台呈现的页面中直接获取或推送NFC信息时十分方便。本文的示例就是使用前台调度。
> 前台调度的使用方法如下：
```java
// 创建一个PendingIntent对象，以便Android系统能够在扫描到NFC标签时，用它来封装NFC标签的详细信息
PendingIntent pendingIntent = PendingIntent.getActivity(this, 0, new Intent(this, getClass()).addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP), 0);

// 创建Intent过滤器
IntentFilter iso = new IntentFilter(NfcAdapter.ACTION_TECH_DISCOVERED);

// 创建一个处理NFC标签技术的数组
String[][] techLists = new String[][]{new String[]{IsoDep.class.getName()}};

// 在主线程中调用enableForegroundDispatch()方法，一旦NFC标签接触到手机，这个方法就会被激活
adapter.enableForegroundDispatch(this, pendingIntent, new IntentFilter[]{iso}, techLists);
```
> 2) 标签调度系统
> NFC标签调度系统是一种通过预先定义好的Tag或NDEF消息来启动应用程序的机制，当扫描到一个NFC Tag时，如果Intent中注册对应的App，
> 那么在处理该Tag信息时就会启动该App。当存在多个可以处理该Tag信息的Apps时，系统会弹出一个Activity Choose，供用户选择开启哪个应用。
> 标签调度系统定义了3种Intent对象，按照优先级由高到低分别为ACTION_NDEF_DISCOVERED、ACTION_TECH_DISCOVERED、ACTION_TAB_DISCOVERED。
> 标签调度的基本工作方法如下：
>   用解析NFC标签时由标签调度系统创建的Intent对象（ACTION_NDEF_DISCOVERED）来尝试启动Activity。
>   如果没有对应的处理Intent的Activity，就会尝试使用下一个优先级的Intent（ACTION_TECH_DISCOVERED，继而ACTION_TAG_DISCOVERED）来启动Activity，直到有对应的App来处理这个Intent，或者是直接标签调度系统尝试了所有可能的Intent。
>   如果没有应用程序来处理任何类型的Intent，就不做任何事情。 

**案例：**
> 智能卡协议使用IsoDep
```java
package xixi.nfc;
import androidx.appcompat.app.AppCompatActivity;
import android.app.PendingIntent;
import android.content.Intent;
import android.content.IntentFilter;
import android.nfc.NfcAdapter;
import android.nfc.Tag;
import android.nfc.tech.IsoDep;
import android.os.Bundle;
import android.provider.Settings;
import android.widget.TextView;
import android.widget.Toast;
import java.io.IOException;
public class MainActivity extends AppCompatActivity {
    private TextView textView;
    private NfcAdapter adapter;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //加载布局文件
        setContentView(R.layout.activity_nfc);
        //获取组件textView
        textView = findViewById(R.id.text_nfc);
        //检查nfc
        nfcCheck();
    }
    //2.检测NFC的检测函数,检测手机是否支持nfc
    private void nfcCheck(){
        //获取nfc
        adapter= NfcAdapter.getDefaultAdapter(this);
        if(adapter==null){
            //设备不支持nfc
            Toast.makeText(this, "NFC is not available", Toast.LENGTH_LONG).show();
            finish();
            return;
        }else{
            //5.假如手机的nfc功能没有被打开。则跳到打开nfc功能的界面
            if(!adapter.isEnabled()) {
                Intent setNfc = new Intent(Settings.ACTION_NFC_SETTINGS);
                startActivity(setNfc);
            }
        }
    }

    //应用开启
    @Override
    protected void onResume() {
        super.onResume();
        //构造PendingInent对象封装NFC标签信息
        PendingIntent pendingIntent = PendingIntent.getActivity(this, 0,
                new Intent(this, getClass()).addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP), 0);
        //声明Intent对象的过滤器。
        IntentFilter iso = new IntentFilter(NfcAdapter.ACTION_TECH_DISCOVERED);
        //建立一个处理NFC标签技术的数组。
        String[][] techLists = new String[][]{new String[]{IsoDep.class.getName()}};
        //调用enableForegroundDispatch()方法，当nfc标签接触手机设备，激活
        adapter.enableForegroundDispatch(this, pendingIntent, new IntentFilter[]{iso}, techLists);
    }

    //应用挂起
    @Override
    protected void onPause() {
        super.onPause();
        //调用disableForegroundDispatch()方法当Activity挂起时禁用前台调用
        adapter.disableForegroundDispatch(this);
    }

    @Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);
        //通过拦截的intent对象获取标签数据tag
        Tag tag = intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);
        if(tag != null) {
           //具体交互
            try {
                NFC nfc = new NFC(IsoDep.get(tag));
                // 发送取随机数APDU命令
                byte[] resp = nfc.send();
                // TextView中显示APDU响应
                textView.setText(resp.toString());
            } catch (Exception e) {
                Toast.makeText(this, e.getMessage(), Toast.LENGTH_SHORT).show();
            }
        }
    }
}
```

```java
package xixi.nfc;
import android.nfc.tech.IsoDep;
import java.io.IOException;
public class NFC {
    // 获取随机数的APDU命令
    private static final byte[] GET_RANDOM = {0x00, (byte)0x84, 0x00, 0x00, 0x08};
    // 声明ISO-DEP协议的Tag操作实例
    private final IsoDep tag;
    public NFC(IsoDep tag) throws IOException {
        // 初始化ISO-DEP协议的Tag操作类实例
        this.tag = tag;
        tag.setTimeout(5000);
        tag.connect();
    }
    /**
     * 向Tag发送获取随机数的APDU并返回Tag响应
     * @throws IOException
     */
    public byte[] send() throws IOException{
        // 发送APDU命令
        byte[] resp = tag.transceive(GET_RANDOM);
        return resp;
    }
}
```

> 该程序读取的数据尚存在bug，待老夫琢磨琢磨...
