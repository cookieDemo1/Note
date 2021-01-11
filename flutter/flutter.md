###### 1.运行到夜神模拟器

```bash
# 1.在夜神模拟器目录运行
nox_adb.exe connect 127.0.0.1:62001


adb devices

# 断开夜神模拟器连接
nox_adb.exe disconnect 127.0.0.1:62001。

# 2.打开flutter目录运行
flutter run

http://www.mamicode.com/info-detail-2547925.html
```

```dart
r Hot reload.
R Hot restart.
h Repeat this help message.
d Detach (terminate "flutter run" but leave application running).
c Clear the screen
q Quit (terminate the application on the device).
```

###### 1.第一个例子

```dart
import 'package:flutter/material.dart';

void main(List<String> args) {
  runApp(Center(
    child: Text(
      '你好flutter,r键热重载',
      textDirection: TextDirection.ltr,
    ),
  ));
}
```

###### 2自定义组件

- 在flutter中自定义组件其实就是一个类,这个类需要继承StatelessWidget或者StatefulWidget
- StatelessWidget是无状态组件,状态不可改变的widget
- StatefulWidget是有状态组件,持有的状态可能在widget生命周期改变

```dart
import 'package:flutter/material.dart';

// 将自定义组件抽离出去,然后在这里引用,dart中创建一个类 new关键字可以省略
void main(List<String> args) {
  runApp(MyApp());
}

// 自定义组件
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        '你好flutter,r键热重载??',
        textDirection: TextDirection.ltr,
        style: TextStyle(fontSize: 40.0, color: Colors.yellow),
      ),
    );
  }
}

```

###### 自定义导航栏和主题颜色

```dart
import 'package:flutter/material.dart';

void main(List<String> args) {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // MaterialApp和Scaffold这两个widget实现
    return MaterialApp(
      // Scaffold里面是显示的内容
      home: Scaffold(
        // 指定顶部导航栏的文字  
        appBar: AppBar(
          title: Text("flutter demo"),
        ),
        // 指定主体
        body: HomeContent(),
      ),
      // 指定主题颜色为blue
      theme: ThemeData(primarySwatch: Colors.blue),
    );
  }
}

// 我们以后都只修改HomeContent
class HomeContent extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        '你好flutter,r键热重载.',
        textDirection: TextDirection.ltr,
        style: TextStyle(fontSize: 40.0, color: Colors.black),
      ),
    );
  }
}

```

###### container组件

```dart
class HomeContent extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // Conteiner容器组件,相当于前端的div
    return Center(
        child: Container(
      // 容器的内容
      child: Text("我是一个文本."),
      // 容器的宽度
      height: 300.0,
      width: 300.0,
      // decoration指定容器的背景颜色和边框等
      decoration: BoxDecoration(
          // 容器的背景颜色
          color: Colors.yellow,
          // 容器的边框颜色和边框宽度
          border: Border.all(color: Colors.blue, width: 2.0)),
          borderRadius: BorderRadius.all(Radius.circular(40)),
       ),
      // padding, all指定四个方向的padding都是40
      // padding: EdgeInsets.all(40),
      // formLTRB() 左上右下的padding
      padding: EdgeInsets.fromLTRB(20, 10, 30, 50),
        
      // transform 位移,三个值为x y z对应的值
      // transform: Matrix4.translationValues(0, 30, 0),
      // 旋转,按z轴旋转40度, rotationY则按y轴旋转
      transform: Matrix4.rotationZ(40),
        
      // 元素的显示位置, 左下角
      alignment: Alignment.bottomLeft,
    ));
  }
}

```

###### Text文本组件

```dart
Text(
        // 文本内容
        "我是一个文本本本本本本本本本本本本本本本本本本本本本本本本",
        // textAlign对齐方式
        textAlign: TextAlign.left,
        // style指定文字样式,
        // fontSize字体大小,
        // color字体颜色, Color.ARGB(A,R,G,B), A代表透明度
        // fontWeight       字体加粗
        // fontStyle        字体是否倾斜
        // decoration       文本修饰线
        // decorationColor 修实线的颜色
        style: TextStyle(
            fontSize: 20.0,
            color: Colors.green,
            fontWeight: FontWeight.w700,
            fontStyle: FontStyle.italic,
            decoration: TextDecoration.underline,
            decorationColor: Colors.black),
        // overflow文本溢出怎么显示
        //    ellipsis表示溢出显示...
        //    clip直接裁剪
        overflow: TextOverflow.ellipsis,
        // maxLines指定文本最多显示几行
        maxLines: 1,
        // textScaleFactor指定文本放大几倍
        textScaleFactor: 2,
      ),
```

###### 图片组件,图片组件有多个构造函数,我们这里只讲两个

```dart
- image.asset		本地图片
- image.network    远程图片

// 远程图片
class HomeContent extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // Conteiner容器组件,相当于前端的div
    return Center(
        // 容器的内容,为一个文本组件
        child: Container(
      	child: Image.network(
            // 图片地址
            "https://www.baidu.com/img/PCfb_5bf082d29588c07f842ccde3f97243ea.png",
            // 图片显示位置.默认水平垂直都局中
            alignment: Alignment.topLeft,
            // 设置图片颜色
            color: Colors.yellow,
            // 设置混合颜色模式
            colorBlendMode: BlendMode.screen,
            // 图片的显示方式,
            // cover全屏显示
            // fill充满屏幕,图片会变形
            fit: BoxFit.cover,
            // repeate平铺, x轴平铺
            repeat: ImageRepeat.repeatX,
          ),
      width: 400,
      height: 400,
      decoration: BoxDecoration(color: Colors.blue),
    ));
  }
}
```

