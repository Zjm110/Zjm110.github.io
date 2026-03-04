---
title: 常用控件
date: 2026-03-04 19:33:22
categories: Android
tags: 
  - Android开发
  - Java
---
# 常用控件

&emsp;&emsp;程序界面的功能是让用户观察和操作数据，能够响应用户的操作通知给程序。界面上的控件就是显示数据和响应用户操作的UI元素，控件就是数据和行为的载体。

&emsp;&emsp;Android提供了大量的UI控件，借助这些控件，我们可以方便地进行界面开发。

## TextView

作用：主要用于界面上显示一段文本信息。
常用属性：

```xml
<!-- 定义一个唯一的标识符 -->
android:id="@+id/tv"

<!-- match_parent、wrap_content和设定固定大小 -->
android:layout_width="match_parent"
android:layout_height="match_parent"

<!-- 指定显示的文本内容 -->
android:text="@string/str"
android:text="@string/str"

<!-- 文本对齐方式，
可选值：top、bottom、left、right、center -->
android:gravity="center"

<!-- 文本大小，sp作为单位 -->
android:textSize="30sp"

<!-- 文本颜色 -->
android:textColor="#ff0000"

<!-- 文本样式 -->
android:textStyle="bold"
```

{% asset_img img01.png %}

activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/tv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:text="Hello World!"
        android:textColor="#ff0000"
        android:textSize="30sp"
        android:textStyle="bold" />

</LinearLayout>
```

------------

values/strings.xml
```xml
<resources>
    <string name="app_name">My Application</string>
    <string name="str">Hello World!</string>
</resources>
```

activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/tv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:text="@string/str"
        android:textColor="#ff0000"
        android:textSize="30sp"
        android:textStyle="bold" />

</LinearLayout>
```

## Button

&emsp;&emsp;Button继承自TextView控件，用于程序和用户进行交互，最重要的作用就是响应用户的点击事件。

&emsp;&emsp;Button控件设置点击事件有四种方式。

{% asset_img img02.png %}

{% asset_img img03.png %}

### 设计点击事件1

activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="click1"
        android:text="Button1" />

    <Button
        android:id="@+id/btn2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button2" />

    <Button
        android:id="@+id/btn3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button3" />

    <Button
        android:id="@+id/btn4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button4" />

</LinearLayout>
```

MainActivity.java
```java
package com.example.buttonapp;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private Button btn2;
    private Button btn3;
    private Button btn4;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });
        btn2 = findViewById(R.id.btn2);
        btn2.setOnClickListener(new MyClickListener());
        btn3 = findViewById(R.id.btn3);
        btn3.setOnClickListener(this);
        btn4 = findViewById(R.id.btn4);
        btn4.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(MainActivity.this, "Button4点击成功！", Toast.LENGTH_LONG).show();
            }
        });
    }

    @Override
    public void onClick(View view) {
        Toast.makeText(this, "Button3点击成功！", Toast.LENGTH_LONG).show();
    }

    private class MyClickListener implements View.OnClickListener {

        @Override
        public void onClick(View view) {
            Toast.makeText(MainActivity.this, "Button2点击成功！", Toast.LENGTH_LONG).show();
        }
    }

    public void click1(View view) {
        Toast.makeText(this, // 上下文
                        "Button1点击成功！",         // 提示的文字
                        Toast.LENGTH_LONG)   // 提示的时间长短
                .show();             // 弹出信息框
    }
}
```

&emsp;&emsp;如果界面上Button比较少，用布局中设置onClick属性和匿名内部类的方式，如果比较多，用Activity类实现接口的方式。

#### 方法一：在布局文件中指定onClick属性的方式

1. 在布局文件中指定onClick属性android:onClick="click1"

activity_main.xml
```xml
<Button
    android:onClick="click1"
/>
```

2. 在Activity类中实现这个click1方法

MainActivity.java
```java
public class MainActivity extends AppCompatActivity{

    public void click1(View view) {
        
    }

}
```  

&emsp;&emsp;定义的方法名要与onClick属性值保持一致。方法首部除方法名与onClick属性值保持一致，其他都是固定写法。

&emsp;&emsp;借助Toast实现点击按钮时界面上显示提示信息。使用代码：

```java
public void click1(View view){
    Toast.makeText(this, // 上下文
        "点击成功！",         // 提示的文字
        Toast.LENGTH_LONG)   // 提示的时间长短
        .show();             // 弹出信息框
}

/* makeText方法中有3个参数：
参数1：表示应用程序上下文环境
参数2：显示信息内容
参数3：显示的时间，LENGTH_SHORT或LENGTH_LONG分别表示显示较短时间和较长时间。*/
```

&emsp;&emsp;Toast是用来在界面的最上层显示提示消息的控件，显示一段时间后会自动消失。

&emsp;&emsp;使用时，Toast类先调用makeText()设置提示信息，再调用show()将提示信息显示到界面上来。

#### 方法二：内部类实现OnClickListener接口的方法

&emsp;&emsp;原理：给Button设置一个监听器，当Button被点击时，监听器感知到点击事件，告诉给系统，系统进行事件处理。

为按钮设置监听器

```java
private Button btn2;

btn2 = findViewById(R.id.btn2);
btn2.setOnClickListener();
```

内部类实现接口

```java
private  class  MyClickListener implements View.OnClickListener{

    @Override
    public void onClick(View view) {
        Toast.makeText(MainActivity.this,"Button2点击成功！",Toast.LENGTH_LONG).show();
    }
}


btn2.setOnClickListener(new MyClickListener())
```

--------------

activity_main.xml
```xml
<Button
    android:id="@+id/btn2"
/>
```

MainActivity.java
```java
public class MainActivity extends AppCompatActivity{
    private Button btn2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {      
        setContentView(R.layout.activity_main);

        btn2 = findViewById(R.id.btn2);
        btn2.setOnClickListener(new MyClickListener());
    }
    private class MyClickListener implements View.OnClickListener {

        @Override
        public void onClick(View view) {
            Toast.makeText(MainActivity.this, "Button2点击成功！", Toast.LENGTH_LONG).show();
        }
    }
}
```

#### 方法三、Activity类实现OnClickListener接口的方式

&emsp;&emsp;这种方式是基于第二种方式，区别是不用专门定义一个内部类，而是用Activity类来实现OnClickListener接口

为按钮设置监听器

```java
private Button btn3;

btn3 = findViewById(R.id.btn3);
btn3.setOnClickListener();
```

Activity类实现接口

```java
public class MainActivity extends AppCompatActivity  implements View.OnClickListener{
    @Override
        public void onClick(View view) {
            Toast.makeText(this,"Button3点击成功！",Toast.LENGTH_LONG).show();
        }
}

btn3.setOnClickListener(this);
```

---------

activity_main.xml
```xml
<Button
    android:id="@+id/btn3"
/>
```

MainActivity.java
```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private Button btn3;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        setContentView(R.layout.activity_main);
       
        btn3 = findViewById(R.id.btn3);
        btn3.setOnClickListener(this);
        
    }

    @Override
    public void onClick(View view) {
        Toast.makeText(this, "Button3点击成功！", Toast.LENGTH_LONG).show();
    }
 
}
```

#### 方法四：匿名内部类的方式

&emsp;&emsp;定义一个没有名称的类来实现接口。添加第四个Button

```xml
<Button
    android:id="@+id/btn4"
/>
```

&emsp;&emsp;接下来定义引用，Activity类中查找控件，设置监听器，直接New匿名内部类对象。

为按钮设置监听器

```java
private Button btn4;

btn4 = findViewById(R.id.btn4);
btn4.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Toast.makeText(MainActivity.this,"Button4点击成功！",Toast.LENGTH_LONG).show();
    }
});
```

----------

MainActivity.java
```java
public class MainActivity extends AppCompatActivity{
    private Button btn4;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        setContentView(R.layout.activity_main);
        
        btn4 = findViewById(R.id.btn4);
        btn4.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(MainActivity.this, "Button4点击成功！", Toast.LENGTH_LONG).show();
            }
        });
    }

}
```

### 设计点击事件2

activity_main2.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="click"
        android:text="Button1" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button2" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button3" />

    <Button
        android:id="@+id/button4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button4" />
</LinearLayout>
```

MainActivity2.java
```java
package com.example.buttonapp;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity2 extends AppCompatActivity implements View.OnClickListener { // 1 实现事件接口
    Button button2 = null;
    Button button3 = null;
    Button button4 = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main2);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });
        button4 = findViewById(R.id.button4);
        View.OnClickListener l = new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(MainActivity2.this, "Button4点击成功！", Toast.LENGTH_LONG).show();
            }
        }; // 接口的匿名实例化
        button4.setOnClickListener(l);     // 为按钮添加事件的方法
        button3 = findViewById(R.id.button3);  // 3 将当前实例传递给按钮的事件的方法
        button3.setOnClickListener(this);
        button2 = findViewById(R.id.button2);
        // 通过Activity提供的方法findViewById()，根据按钮id来找按钮。
        // R代码资源，id代表控件的id集合，button2就是布局文件中设置的按钮id
        // 结果：拿到界面上的按钮
        MyListener m = new MyListener(); // 2 创建实现类的实例
        button2.setOnClickListener(m);   // 3 将实例传递给按钮事件方法
    }

    @Override
    public void onClick(View view) {  // 2 重写onClick方法
        Toast.makeText(this, "Button3点击成功！", Toast.LENGTH_LONG).show();
    }

    // 1 创建接口的实现类
    public class MyListener implements View.OnClickListener {

        @Override
        public void onClick(View view) {
            Toast.makeText(MainActivity2.this, "Button2点击成功！", Toast.LENGTH_LONG).show();
        }
    }

    public void click(View view) {
        Toast.makeText(this, "Button1点击成功！", Toast.LENGTH_LONG).show();
    }
}
```

#### 方法1：通过按钮熟悉onClick设置按钮事件的方法

步骤1：在xml布局文件中找到Button，为按钮设置属性onClick的值为click。

```xml
<Button
    android:onClick="click"/>
```

步骤2：回到界面的java文件中，编写click方法。

```java
public class MainActivity2 extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        setContentView(R.layout.activity_main2);
    }

    public void click(View view) {
        Toast.makeText(this, "Button1点击成功！", Toast.LENGTH_LONG).show();
    }
}
```

#### 方法2：创建接口实现类的实例，并传递给按钮事件参数

步骤1：设置按钮的id

```xml
<Button
    android:id="@+id/btn2"/>
```

步骤2：创建实现类，创建实例，传递给按钮事件方法。

```java
public class MainActivity2 extends AppCompatActivity{ // 1 实现事件接口
    Button button2 = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        setContentView(R.layout.activity_main2);

        button2 = findViewById(R.id.button2);
        // 通过Activity提供的方法findViewById()，根据按钮id来找按钮。
        // R代码资源，id代表控件的id集合，button2就是布局文件中设置的按钮id
        // 结果：拿到界面上的按钮
        MyListener m = new MyListener(); // 2 创建实现类的实例
        button2.setOnClickListener(m);   // 3 将实例传递给按钮事件方法
    }

    // 1 创建接口的实现类
    public class MyListener implements View.OnClickListener {

        @Override
        public void onClick(View view) {
            Toast.makeText(MainActivity2.this, "Button2点击成功！", Toast.LENGTH_LONG).show();
        }
    }
}
```

#### 方法3：让当前Activity实现事件接口，将当前实例传递给按钮事件方法。

步骤1：给按钮设置id

```xml
<Button
    android:id="@+id/button4"/>
```

步骤2：让当前Activity实现接口View.onClickListener,重写方法onClick,并传递当前Activity实例给按钮事件方法。

```xml
<Button
    android:id="@+id/button3"/>
```

步骤2：创建实现类，创建实例，传递给按钮事件方法。

```java
public class MainActivity2 extends AppCompatActivity implements View.OnClickListener { // 1 实现事件接口
    Button button3 = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        setContentView(R.layout.activity_main2);
        
        button3 = findViewById(R.id.button3);  // 3 将当前实例传递给按钮的事件的方法
        button3.setOnClickListener(this);
    }

    @Override
    public void onClick(View view) {  // 2 重写onClick方法
        Toast.makeText(this, "Button33点击成功！", Toast.LENGTH_LONG).show();
    }

    
}
```

#### 方法4：通过按钮的事件接口的匿名实例来实现按钮事件。

步骤1：为界面上的按钮设置id

```xml
<Button
    android:id="@+id/button4"/>
```

步骤2：java代码中找到按钮的引用，创建按钮点击事件接口的匿名实例，并传递给按钮事件。

```java
public class MainActivity2 extends AppCompatActivity{ 
    Button button4 = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        setContentView(R.layout.activity_main2);
        
        button4 = findViewById(R.id.button4);
        View.OnClickListener l = new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(MainActivity2.this, "Button4点击成功！", Toast.LENGTH_LONG).show();
            }
        }; // 接口的匿名实例化
        button4.setOnClickListener(l);     // 为按钮添加事件的方法
    }

}
```

## EditText

&emsp;&emsp;EditText文本框，程序和用户交互的另一个重要的控件，这个控件允许用户输入和编辑信息。在发送短信、拨打电话这些操作时，都需要使用它。

```xml
<!-- 提示信息 -->
android:hint

<!-- 显示最大行 -->
android:maxLines

<!-- 输入类型限制 -->
android:inputType

<!-- 设置字间的间隔 -->
android:textScaleX
android:textScaleY
```

```xml
<EditText
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="please input name"
    android:inputType="textPassword"
    android:singleLine="true"
    android:textColorHint="#91A" />
```

{% asset_img img04.png %}

【案例】在界面上点击Button按钮时，取出文本框中输入的内容，显示在Toast上。

{% asset_img img05.png %}

actitity2.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <EditText
        android:id="@+id/et_name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="请输入姓名"
        android:singleLine="true"
        android:textColorHint="#95A"/>

    <Button
        android:id="@+id/btn_getName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="获取姓名" />
</LinearLayout>
```

MainActivity.java
```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private EditText etName;
    private Button btnGetName;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        setContentView(R.layout.activity2);

        etName = findViewById(R.id.et_name);
        btnGetName = findViewById(R.id.btn_getName);
        btnGetName.setOnClickListener(this);
    }

    @Override
    public void onClick(View view) {
        String name = etName.getText().toString();
        Toast.makeText(this,"您输入的姓名是："+name,Toast.LENGTH_LONG).show();
    }
}
```

## ImageView

## RadioButton

## Checkbox