---
title: 设置首面
date: 2026-03-04 16:52:18
categories: Android
tags: 
  - Android开发
  - Java
---
## 设置首页

【实训】创建一个Activity，设置为首界面。

将MainActivity2设置为首页：

{% asset_img img01.png %}

{% asset_img img02.png %}

{% asset_img img03.png %}

修改AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication">
        <activity
            android:name=".MainActivity2"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name=".MainActivity"
            android:exported="true" />
    </application>

</manifest>
```