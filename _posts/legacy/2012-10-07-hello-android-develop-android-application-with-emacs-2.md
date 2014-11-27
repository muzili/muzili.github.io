---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

Android 布局
============

第一个简单的UI[^1]
------------------

这个UI包含3个EditText和1个Button.

### 如何添加EditText

#### 在res/layout/main.xml中添加EditText

> \<EditText android:id="@+id/edit~ssid~"
> android:layout~width~="wrap~content~"
> android:layout~height~="wrap~content~"
> android:hint="@string/edit~ssid~" /\>

#### 在res/values/strings.xml中添加字符串

> \<string name="edit~ssid~"\>Input SSID\</string\>

#### 在MainActivity中添加EditText

添加需要引用的package

> import android.util.Log; import android.view.View; import
> android.widget.EditText;

添加成员ssid

> EditText ssid;

在创建MainActivity时初始化

> this.ssid = (EditText)findViewById(R.id.edit~ssid~);

通过下面的函数返回ssid的内容

> ssid.getText().toString()

下面是MainActivity.java的完整内容

\#+begin~verse~ package abovelink.wifi.qr;

import android.app.Activity; import android.os.Bundle; import
android.util.Log; import android.view.View; import
android.widget.EditText;

public class MainActivity extends Activity { EditText ssid; EditText
security; EditText password;

*\*\* Called when the activity is first created. \** @Override public
void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState); setContentView(R.layout.main);
this.ssid = (EditText)findViewById(R.id.edit~ssid~); this.security =
(EditText)findViewById(R.id.edit~security~); this.password =
(EditText)findViewById(R.id.edit~password~); }

*\*\* Called when the user clicks the Generate button \** public void
generateQRCode(View view) { // Do something in response to button String
qrstr = " WIFI:T:" + security.getText().toString() + ";S:" +
ssid.getText().toString() + ";P:" + password.getText().toString() +
";;"; Log.d("abovelink", "generate the QR code for " + qrstr); } }

\#+end~verse~

在模拟器中查看我们的第一个UI
----------------------------

使用android avd启动虚拟机

> android avd

使用ant debug install安装我们的第一个应用

> ant debug install

使用adb logcat 查看button是否其作用

> adb logcat

可以得到下面的message.

> ... D/abovelink( 910): generate the QR code for
> WIFI:T:ffff;S:rrr;P:hhhh;; ...

Footnotes
=========

[^1]: <http://developer.android.com/training/basics/firstapp/building-ui.html>

    {{{more}}}

