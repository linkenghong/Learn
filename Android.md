# **adb命令**

adb的全称为Android Debug Bridge，就是起到调试桥的作用，相当于debug工具。

1. **查看版本**
```
adb version
```
2. **查看连接设备**
```
adb devices
```
3. **进入设备**
```
adb shell
```
4. **复制文件至设备**
```
adb push <local> <remote>
```
5. **查看帮助文档**
```
adb help
```
6. **查看文件**
```
more 文档
```
7. **文件读写无权限**

adb push时有些文件夹会有以下提示：
```
failed to copy '~/build.prop' to '/system/build.prop': Read-only file system
```
Enter device Simply remount as rw (Read/Write):
```
# mount -o rw,remount /system
```
Once you are done making changes, remount to ro (read-only):
```
# mount -o ro,remount /system
```
8. APP需要root权限或者需要以系统APP形式安装

需要将apk放入 system/app 或 system/priv-app 文件夹中，但这两个文件夹是只读的，所以需要使用上面 7 的命令获取权限，然后用`adb push`方式把apk传到文件夹中，然后reboot。


# **Android Studio 操作** - Android Studio 3.0.1

1. **代码对比**
```
选中代码 --> 右键选择“compare with clipboard” 
```
2. **隐藏APP导航栏、APP全屏**
```
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  requestWindowFeature(Window.FEATURE_NO_TITLE);// 隐藏APP导航栏
  getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,WindowManager.
  LayoutParams.FLAG_FULLSCREEN);  // 全屏
```
3. **保持屏幕方向**
```
android:screenOrientation="landscape"   //横屏
android:screenOrientation="portrait"   //纵屏
```
4. **OutofMemoryError**

> OutOfMemoryError is the most common problem occured in android while especially dealing with bitmaps. This error is thrown by the Java Virtual Machine (JVM) when an object cannot be allocated due to lack of memory space and also, the garbage collector cannot free some space.

   
解决方法：
add below entities in your manifest file
```
android:hardwareAccelerated="false"
android:largeHeap="true"
```

5. **RecoverySystem.installPackage()**

系统升级时需要调用 `RecoverySystem.installPackage()` 函数，但使用后还是无法升级，会有以下提示
```
Android development RecoverySystem.installPackage() cannot write to /cache/recovery/command permission denied
```
该提示是因为没有权限，所以需要把app安装为系统app，方法见上面第8。