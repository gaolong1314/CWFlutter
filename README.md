# CWFlutter
flutter开发功能，打包出framework。产物集成到iOS原生功能中。
appflutter是flutter工程，打出的产物在工程的build_for_ios文件中
demo20是ios原生功能，因为打出的产物过大，已将产物中的framework删除，如想运行，将flutter工程中的build_for_ios文件中的framework添加进工程中即可。

#接下说明产物集成的详细步骤
1，创建flutter工程，终端：flutter create xxx
2，在pubspec.yaml添加闲鱼的flutterboost插件
3，根据实际和闲鱼flutterboost技术文档，进行配置页面等相关内容
4，将脚本中的build_for_ios.sh拖入到根目录中，终端运行:bash build_for_ios.sh
5，待脚本运行完毕，在根目录下会生成一个build_for_ios文件夹，文件夹中的产物：App.framework、flutter_boost.framework、Flutter.framework、        GeneratedPluginRegistrant.h、GeneratedPluginRegistrant.m
6，将build_for_ios中的产物拖进ios原生项目工程中
7，在TARGETS下Build Phases中的link Binary With Libraries添加libc++.tdb
8，左上角加号，创建New Copy Files Phases，将产物中的framework添加进去，Destination选择Frameworks
9，将工程bitcode 置为No
10，运行工程应该没有问题，配置完毕，接下来将添加代码
11，创建Router，继承自Nsobject，#import <flutter_boost/FLBPlatform.h>和<UIKit/UIKit.h>，遵循代理<FLBPlatform>
,根据flutter_boost要求实现其代理方法即可。具体代码在ios原生工程中（DemoRouter）
12，在AppDelegate.h中，引入#import <flutter_boost/FlutterBoost.h>、#import "GeneratedPluginRegistrant.h"、#import "DemoRouter.h"，将AppDelegate继承的UIResponder,修改为FLBFlutterAppDelegate。
13，在didFinishLaunchingWithOptions方法中初始化GeneratedPluginRegistrant，FlutterBoostPlugin初始化
14，最后在跳转方法中，调用路由DemoRouter设置参数跳转即可
  
#注意
 1，文档中，出于简便，将DemoRouter中的跳转动画设置为了YES，其可以在exts字典中配置
 2，由于起名太困难，工程名随便起的，不要纠结这个，干货很重要
 3，在iOS集成Flutter脚本 文件夹中xcode_backend.sh是ios原生项目打包用的，具体：https://www.jianshu.com/p/b16ff23363c0
  
