# 初识Flutter

##  Flutter 简介

Flutter 是 Google 公司2018年2月27日发布的第一个开源跨平台软件开发工具包 （SDK），支持Android、iOS两个平台，可实现高性能、高保真的应用程序开发。开发者借助 Flutter 开发平台可轻松实现自己的应用，其开发框架默认支持了 Material（类似 Google 设计风格）和 Cupertion（类似 iOS 设计风格）两种风格，且无论从UI样式、用户体验上都非常接近原生应用。经过近7个月的优化改进2018年9月19日 Google 公司在上海正式发布非常接近1.0正式版本的 Flutter Release Preview 2，基于其优越性能 Flutter 有望成为未来应用开发的主流工具。

Flutter 类似且优于 Html、React Native、Weex 等跨平台解决方案。ReactNative 将 JSX 生成的代码最终都渲染成原生组件，JS 与原生通过 JSBridge 传递数据。而 Flutter 则采用完全不同的设计，底层是一套独立的渲染引擎--Skia，所有组件也都是独立于平台的 Widget 组件，可以在最大程度上保证了跨平台体验的一致性。

![image.png](https://upload-images.jianshu.io/upload_images/2511800-f87d2f42212f6364.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##  什么是 flutter

Flutter是一个移动应用程序的软件开发工具包（SDK），具有以下特征：
  - 跨平台应用的框架，没有使用WebView或者系统平台自带的控件，使用自身的高性能渲染引擎自绘
  - 简化版的浏览器，最大限度在android和ios上统一UI，包括业务逻辑和用户体验
  - 开发语言使用dart，结合C, C++, 和Skia（2D渲染引擎）构建
  - 支持hot reload，包含着完整的控件和工具链
  - 组合大于继承，控件本身通常由许多小型、单用途的控件组成，结合起来产生强大的效果，类的层次结构是扁平的，以最大化可能的组合数量
  - 强化版的WebView，框架仅提供一个View层，大部分功能要依赖原生
  - 目前只能够运行大部分Dart代码（不能引入dart:mirrors或dart:html库）

##  运行机制
Flutter 应用运行在一个用 C++ 写的引擎中，Flutter 应用可以看做是一个游戏 App，代码都是在引擎中运行。

  1）引擎的C或C++代码是由Android NDK编译的，而框架的主要代码和应用的代码由Dart compiler编译成native code执行的。

  2）对于Android应用来说，Flutter框架在引擎中实现了一个继承于SurfaceView的 FlutterView。用户所看到的UI都是在这个SurfaceView中显示。如果要和原生平台功能交互，则可以在Activity中使用FlutterView，并通过Flutter提供的消息API和原生平台收发消息。

#  使用Flutter进行APP开发

##  Dart
Dart是Google开发一门编程语言，风格有点像Kotlin，但语法又有很多和Java相似的地方
由于Flutter采用Dart语言来开发，所以需要稍微适应一下这种又像面向对象，又像脚本的语言
## 程序入口（Entry Point）

Flutter也有一个固定的程序入口，
void main() => runApp(new MyApp());

## 组件（widget）

切皆控件，控件是每个Flutter应用程序的基本构建块，与分离视图、控制器、布局和其他属性的框架不同，Flutter具有一致的统一对象模型：控件。一个控件可以定义：结构元素（比如按钮或菜单）、风格元素（比如字体或颜色方案）、布局的方面（比如填充）、一些业务逻辑等
其中，最常用的组件有以下几个：

Text：用于展现文本
Row及Column：行/列，用于FlexBox风格的布局
Stack：一种栈式布局方式，和LinearLayout这种“栈”式布局不同，Flutter的Stack用于实现View的层次布局
Container：约束布局，类似Android的ConstraintLayout和iOS的AutoLayout
```
@override
Widget build(BuildContext context) {
  return Container(
    height: 56.0, // pixels
    padding: const EdgeInsets.symmetric(horizontal: 8.0),
    decoration: BoxDecoration(color: Colors.blue[500]),
    // Row 就相当于一个水平的LinearLayout

    child: Row(
      children: <Widget>[
        IconButton(
          icon: Icon(Icons.menu),
          tooltip: 'Navigation menu',
          onPressed: null, 
        ),
        // Expanded 里面它的孩子会填充它的控件
        Expanded(
          child: title,
        ),
        IconButton(
          icon: Icon(Icons.search),
          tooltip: 'Search',
          onPressed: null,
        ),
      ],
    ),
  );
}
```

同理：
```
// Column 相当于vertical 的LinearLayout.
  child: Column(
    children: <Widget>[
      MyAppBar(
        title: Text(
          'Example title',
          style: Theme.of(context).primaryTextTheme.title,
        ),
      ),
      Expanded(
        child: Center(
          child: Text('Hello, world!'),
        ),
      ),
    ],
  ),
```
### 使用material组件
```
void main() {
  runApp(MaterialApp(
    title: 'Flutter Tutorial',
    home: MyHome(),
  ));
}
@override
Widget build(BuildContext context) {
  // Scaffolds是 Material的主要组件.
  return Scaffold(
    appBar: AppBar(
  
      title: Text('Example title'),
      actions: <Widget>[
        IconButton(
          icon: Icon(Icons.search),
          tooltip: 'Search',
          onPressed: null,
        ),
      ],
    ),
    // body 是页面内容展示区域
    body: Center(
      child: Text('Hello, world!'),
    ),
    floatingActionButton: FloatingActionButton(
      tooltip: 'Add', 
      child: Icon(Icons.add),
      onPressed: null,
    ),
  );
}
```
### 处理手势

```
class MyButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: (){
        print('MyButton was tapped!');
      },
      child: Container(
        height: 36.0,
        padding: const EdgeInsets.all(8.0),
        margin: const EdgeInsets.symmetric(horizontal: 8.0),
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(10.0),
          color: Colors.lightGreen[500],
        ),
        child: Center(
          child: Text('Engage'),
        ),
      ),
    );
  }
}
```
- 文本控件
```
TextDecoration.lineThrough  // 删除线  
underline,   //下划线
		 overline,   //倾斜

new Text(
  '红色+黑色删除线+25号',
  style: new TextStyle(
    color: const Color(0xffff0000),
    decoration: TextDecoration.lineThrough,
    decorationColor: const Color(0xff000000),
    fontSize: 25.0,
    
  ),
),
```
- 图片控件
本地获取图片 在项目目录增加images文件夹
![image.png](https://upload-images.jianshu.io/upload_images/2511800-625d81ff21de46f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
并且在pubspec.yaml增加资源

![image.png](https://upload-images.jianshu.io/upload_images/2511800-d971ec538e4d2af1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
new Image.asset(  
  'images/lake.jpg',
  width: 600.0,
  height: 240.0,
  fit: BoxFit.cover,
),
```
 网络获取图片
```
 new Image.network(
    'https://xxxx/xxx.png',

  fit: BoxFit.fitWidth,
),
```
- 容器控件
```
new Container(
  width: 200.0,
  height: 200.0,
  decoration: new BoxDecoration(
    color: Colors.amberAccent,
    border: new Border.all(
      color: const Color(0xff6d9eeb),
      width: 8.0,
    ),
    borderRadius: const BorderRadius.all(constRadius.circular(48.0))
  ),
  child: new Text(
    'hello',
    textAlign: TextAlign.center,
  ),
)
```
- 基础列表
```
new ListView(
  children: [
new ListTile(
  	leading: new Icon(Icons.phone),
  	title: new Text('Phone'),
	,
	new ListTile(
 	 leading: new Icon(Icons.phone),
  	title: new Text('Phone'),
	),
       new Image.asset(
      	'images/lake.jpg',
     	 width: 600.0,
     	 height: 240.0,
    	  fit: BoxFit.cover,
       ),
   ],
),
```
- 水平垂直列表
```
new ListView(
    scrollDirection: Axis.horizontal, //vertical
    children: <Widget>[
      new Container(
        width: 160.0,
        color: Colors.lightBlue,
      ),
      new Container(
        width: 160.0,
        color: Colors.amber,
      ),
      new Container(
        width: 160.0,
        color: Colors.deepOrange,
        child: new Column(
          children: <Widget>[
            new Text(
              '简介',
              style: new TextStyle(
                fontWeight: FontWeight.bold,
                fontSize: 36.0,
              ),
            ),
            new Text(
              '评论',
              style: new TextStyle(
                fontWeight: FontWeight.bold,
                fontSize: 36.0,
              ),
            ),
            new Icon(Icons.phone),
          ],
        ),
      ),
     ],
  ),
),
```
- 网格列表
```
 new GridView.count(
  primary: false,
  padding: const EdgeInsets.all(20.0),
  crossAxisSpacing: 30.0,
  crossAxisCount: 3,
  children: <Widget>[
    const Text('第一行第一列'),
    const Text('第一行第二列'),
    const Text('第一行第三列'),
    const Text('第二行第一列'),
    const Text('第二行第二列'),
    const Text('第二行第三列'),
    const Text('第三行第一列'),
    const Text('第三行第二列'),
    const Text('第三行第三列'),

  ],
),
```
- Container布局
```
new Expanded(
  child: new Container(
    width:150.0,
    height: 150.0,
    decoration: new BoxDecoration(
      border: new Border.all(width: 10.0,color: Colors.black38),
      borderRadius: const BorderRadius.all(const Radius.circular(8.0)),
    ),
    margin: const EdgeInsets.all(4.0),
    child: new Image.asset('images/xx.jpeg'),
  ),
),
```
- Stack布局 -层叠布局,实现View的层次布局
```
 new Scaffold(
  appBar: new AppBar(
    title: new Text('Stack层叠布局示例'),
  ),
  body: new Center(
    child: stack,
  ),
);
ar stack = new Stack(
  alignment: const FractionalOffset(0.2, 0.2),
  children: <Widget>[
    new CircleAvatar(
      backgroundImage: new AssetImage('images/1.jpeg'),
      radius: 100.0,
    ),
    new Container(
      decoration: new BoxDecoration(
        color: Colors.black38,
      ),
      child: new Text(
        '亢少军老师',
        style: new TextStyle(
          fontSize: 22.0,
          fontWeight: FontWeight.bold,
          color: Colors.white,
        ),
      ),
    ),
  ],
);

```
### 按下手势
 1.1
```
new GestureDetector(
  onTap: (){
    final snackBar = new SnackBar(content: new Text("press"),);

    Scaffold.of(context).showSnackBar(snackBar);
  },
  child: new Container(
    padding: new EdgeInsets.all(12.0),
    decoration: new BoxDecoration(
      color: Theme.of(context).buttonColor,
      borderRadius: new BorderRadius.circular(10.0),
    ),
    child: new Text('Button'),
  ),
);
```
1.2 滑动删除手势
```
new Scaffold(
  appBar: new AppBar(
    title: new Text('滑动删除示例'),
  ),
  body: new ListView.builder(
    itemCount: items.length,
    itemBuilder: (context, index) {
      final item = items[index];

      return new Dismissible(
          key: new Key(item),
          onDismissed: (direction) {
            items.removeAt(index);

            Scaffold.of(context).showSnackBar(
                new SnackBar(content: new Text("$item 被删除了")));
          },
          child: new ListTile(title: new Text('$item'),)
      );
    },
  ),
);
```
### 自定义字体
![image.png](https://upload-images.jianshu.io/upload_images/2511800-b749d8803a9a0bef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/2511800-be42151b625b64ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
 new Text(
  '自定义字体',
  style: new TextStyle(fontFamily: 'myfont',fontSize: 36.0),
),
```
### 跳转
 - 页面跳转

```
Navigator.push(
  context,
  new MaterialPageRoute(builder: (context) => new SecondScreen())
);
```
- 页面跳转传数据
```
onTap: () {
  Navigator.push(
    context,
    new MaterialPageRoute(
        builder: (context) =>
            new SecondScreen(data: new Bean())),
  );
},

class SecondScreenextends StatelessWidget {
  final Product product;

  SecondScreen({Key key, @required this.data}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("${data.xxxx}"),
      ),
      body: new Padding(
        padding: new EdgeInsets.all(16.0),
        child: new Text('${data.xxxx}'),
      ),
    );
  }
}
```
- 页面跳转返回数据
```
navigateToSecondPage(BuildContext context) async {

  final result = await Navigator.push(
      context,
      new MaterialPageRoute(builder: (context) => new SecondPage()),
  );
  
  Scaffold.of(context).showSnackBar(new SnackBar(content: new Text("$result")));

}

SecondPage
onPressed: (){
  Navigator.pop(context,'hi flutter');
},
```
- 页面跳转动画
```
Navigator.push(context, new MaterialPageRoute(builder: (_){
  return new SecondPage();
}));
```
## 动画
```
body: new Center(
  child: new AnimatedOpacity(
      opacity: _visible ? 1.0 : 0.0,
      duration: new Duration(
        milliseconds: 1000
      ),
      child: new Container(
        width: 300.0,
        height: 300.0,
        color: Colors.deepPurple,
      ),
  ),
),
floatingActionButton: new FloatingActionButton(
    onPressed: (){

      setState(() {
        _visible = !_visible;
      });

    },
  tooltip: "显示隐藏",
  child: new Icon(Icons.flip),
),
```
# 总结
刚接触Flutter，由于之前对React Native的印象，总让我感觉有一丝熟悉，但好像又无从下手，后面准备看看一些Example，写一些小Demo练练手，然后再进一步研究一下

有一种写css + 面向对象的感觉，写熟练了UI热重载 立马可以看效果，开发速度提升。

未完待续.........
