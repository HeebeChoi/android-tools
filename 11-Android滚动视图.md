**Android滚动视图**<br>
> 我们通常为了让界面方便观看，就需要界面是可以滚动的，于是，这时候我们就需要使用滚动视图组件ScollView,要使用该组件，只需要在LinerLayout外声明该
> 布局可以滚动即可.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <!--下拉菜单-->
        <Spinner
            android:id="@+id/spinner"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
        </Spinner>
    </LinearLayout>

</ScrollView>


```
