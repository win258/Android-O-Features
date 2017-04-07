# Android O Preview1

### 后台限制 & 隐式广播限制
***

##### 隐式广播（Implict Broadcast）
根据官方文档，所有没有直接和应用相关的广播都是隐式的。绝大多数我们监听的广播都是隐式的，比如网络连接改变，是否充电，安装了新应用等等。

* 在 Android N (7.x) 中，Google移除了三项隐式广播（CONNECTIVITY_ACTION, ACTION_NEW_PICTURE, ACTION_NEW_VIDEO）; 而在Android O 中，除了 [https://developer.android.com/preview/features/background-broadcasts.html](https://developer.android.com/preview/features/background-broadcasts.html) 里面的隐式广播，其它均被移除

* 代码中动态注册的receiver不受任何影响

如果监听了这些被移除的隐式广播，除了动态注册，也可考虑 `JobScheduler` ：JobScheduler 完美适配 Doze 和 App Standby，可以根据定义的条件来执行任务。
**结论：此限制仅针对面向O（targetSdkVersion>=26）开发的应用，在targetSdkVersion改为Android O之前，无影响。适配时应对使用隐式广播的代码做出修改。**
##### 后台服务在 Android O 中，新的后台管理机制规定，当应用进入后台几分钟后就会进入停滞状态，虽然进程没有被杀掉，但将不再占用包括计算、内存、通信等在内的系统资源，也将不再频繁地发送和获取 GPS 位置更新，起到省电的效果。不过 Google 也解释说有办法能够逾越这些限制，但最终目的还是能够帮助用户更加省电。* `startService()` 将抛出 IllegalStateException （but.. 对交叉唤醒基本没有施加有效的约束: 即便是target O的app，startService()这个接口在后台被废了，但是bindService()没有被限制，依旧可以用bind的形式进行唤醒）

* 创建Foreground Service时需要使用 `NotificationManager.startServiceInForeground()` 方法，同时旧方法不再有效

* 后台运行的应用，不再能频繁的收到位置更新的信息；但具体更新频次减少到多少，还需要最终版本发布后确定 （官方说，大约每小时数次的位置更新）**结论：依旧是仅针对面向O（targetSdkVersion>=26）开发的应用，在targetSdkVersion修改为Android O之前，无影响。**但不管有没有target Android O，系统还是有点约束的：Android M 和 N 上，息屏并保持静止一个小时后，doze启动，wake-lock会释放；从Android O开始，应用的进程一旦空闲（通常也就是后台服务结束）或终止，wake-lock就会立即释放。但只要还有后台服务还在运行，wake-lock就仍然要等到doze启动后才会释放。
**以上的隐式广播限制、后台限制或位置获取，在App位于前台时，均不受影响。（只是增加了部分新Api）**> 附：Android 6.x 下的doze介绍 [https://developer.android.com/training/monitoring-device-state/doze-standby.html](https://developer.android.com/training/monitoring-device-state/doze-standby.html)
### 系统通知 (Notification)
***
Google 要求开发者在开发面向 Android O 的应用程序时，为应用通知设置不同类型的通知渠道（Channel）。这样一来，当来自不同应用的同类通知（比如资讯、即时消息）同时出现时，它们能够被系统识别并整合在一起。将应用发出的通知进行细化，划分成不同的类别，可以针对频道进行操作。如用户可以屏蔽某个频道的通知，而不是这个应用的所有通知消息；开发者可以针对频道设置通知的震动、声音等。
在 Android O 开发者预览版中，当某个应用发出一条我们暂时无暇处理的通知时，只需要将这条通知向左侧或右侧轻轻滑动，即可看见「齿轮」和「时钟」两个小图标，时钟图标就是新加入的「通知延后」功能。它可以让这条通知暂时从通知栏中消失，并在规定时间后重新出现。

### 其它变化
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
以上整理自Android官网、Youtube、知乎等。

