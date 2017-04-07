#Android O Preview

###后台限制 & 隐式广播限制
***

#####隐式广播（Implict Broadcast）
根据官方文档，所有没有直接和应用相关的广播都是隐式的。绝大多数我们监听的广播都是隐式的，比如网络连接改变，是否充电，安装了新应用等等。

* 在 Android N (7.x) 中，Google移除了三项隐式广播（CONNECTIVITY_ACTION, ACTION_NEW_PICTURE, ACTION_NEW_VIDEO）; 而在Android O 中，除了 [https://developer.android.com/preview/features/background-broadcasts.html](https://developer.android.com/preview/features/background-broadcasts.html) 里面的隐式广播，其它均被移除

* 代码中动态注册的receiver不受任何影响

如果监听了这些被移除的隐式广播，除了动态注册，也可考虑 `JobScheduler` ：JobScheduler 完美适配 Doze 和 App Standby，可以根据定义的条件来执行任务。



* 创建Foreground Service时需要使用 `NotificationManager.startServiceInForeground()` 方法，同时旧方法不再有效

* 后台运行的应用，不再能频繁的收到位置更新的信息；但具体更新频次减少到多少，还需要最终版本发布后确定 （官方说，大约每小时数次的位置更新）


***




***

* ANDROID_ID 不再是设备中所有应用共享的，而是每个应用获取到的都不一样，而且以包名和签名作为区分；卸载后重新安装也不会发生变化；但是手机恢复出厂设置后，应该和上一次的不再一致

* 画中画：Activity的显示多了一种方式，只要在AndroidManifest.xml中设置android:supportsPictureInPicture为true （以后视频相关的，如通话和会议可以这样优化）

* 自适应图标：不同设备上显示不同样式的App图标（在res/mipmap-anydpi/ic_launcher.xml，分别指定前景和背景图）

* XML支持定义字体啦

* 新加入的自动填充 API 开发出支持全局表单数据自动填写

* WebView API有点儿新变化（多进程）: [https://developer.android.google.cn/preview/features/managing-webview.html](https://developer.android.google.cn/preview/features/managing-webview.html)

* 支持更高阶的蓝牙音频解码协议，提供更高码率

* 安装应用时新增进度显示

* 其它新变化，可以参见[官网](https://developer.android.google.cn/preview/behavior-changes.html)


