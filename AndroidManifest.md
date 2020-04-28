## Manifest

package 包名

versionName/versionCode 已经不需要自己写，在build.gradle(Module:app)中可查看。

## Application

### allowBackup

默认为true

AllowBackup是在Android 2.2中引入的一个系统备份的功能。允许用户备份系统应用和第三方应用的apk安装包和应用数据，以便在刷机或者数据丢失后恢复应用。第三方应用开发者需要在应用的 AndroidManifest.xml 文件中配置 allowBackup 标志(默认为 true )来设置应用数据是否能能够被备份或恢复。当这个标志被设置为true时应用程序数据可以在手机未获取 ROOT 的情况下通过adb调试工具来备份和恢复，这就允许恶意攻击者在接触用户手机的情况下在短时间内启动手机 USB 调试功能来窃取那些能够受到 AllowBackup 漏洞影响的应用的数据，造成用户隐私泄露甚至财产损失。

（来源：https://www.jianshu.com/p/2cb6c55cab70）

### icon

应用图标的路径

### label

应用的名字

### activity

```xml
<intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
```

activity中包含这一段，是启动界面

