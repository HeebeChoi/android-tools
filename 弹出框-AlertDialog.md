**安卓弹出框的使用方式：**
```java
//content_message:需要弹出的提示文本内容   btn_message:弹出的提示按钮文本内容
public void alterDialog(String content_message,String btn_message){
    AlertDialog.Builder builder = new AlertDialog.Builder(this);
    builder.setMessage(content_message);
    builder.setPositiveButton(btn_message, new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialog, int which) {
        }
    });
    AlertDialog dialog=builder.create();
    dialog.show();
}
```
