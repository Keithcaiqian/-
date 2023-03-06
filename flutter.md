# 安装

+ jdk

https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

+ 配置jdk

![image-20200806145458528](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200806145458528.png)

+ 配置环境变量

![image-20200806152247365](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200806152247365.png)

+ 电脑上下载安装 Android Studio
    https://developer.android.google.cn/studio

+ 三、电脑上面下载配置 Flutter Sdk
    1、下载 FlutterSDK
    https://flutter.dev/docs/development/tools/sdk/releases#windows
    2、把下载好的 FlutterSDK 随便减压到你想安装 Sdk 的目录如（E:\flutter_windows\flutter）
    3、把 Flutter 安装目录的 bin 目录配置到环境变量。
    如上图所示需要把 E:\flutter_windows\flutter\bin 目录配置到 path 环境变量里面
+ 四、电脑上配置 Flutter 国内镜像
    搭建环境过程中要下载很多资源文件，当一些资源下载不了的时候，可能会报各种错误。在 国内访问 Flutter 的时候有可能会受到限制。Flutter 官方为我们提供了国内的镜像
    https://flutter-io.cn/
    https://flutter.dev/community/china
    拉到 Flutter 中文网最下面有配置方式，把下面两句配置到环境变量里面
    FLUTTER_STORAGE_BASE_URL:https://storage.flutter-io.cn PUB_HOSTED_URL:https://pub.flutter-io.cn
+ 五、运行 flutter doctor 命令检测环境是否配置成
+ 六、打开 Android Studio 安装 Flutter 插件
    