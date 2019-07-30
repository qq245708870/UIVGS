:grin::grin::grin:哈哈哈Unity无敌！！！:grin::grin::grin: 顺便点个星！！
===================================
![00](https://github.com/qq245708870/UIVGS/blob/master/欢迎大家一起交流学习.jpg)
关于Unity —UIWidgets的详细介绍
-----------------------------------  

UIWidgets（我下面简称它UIVGS）是 Unity 的一个跨平台APP解决方案的插件包，它继承了Flutter精神。因为Flutter有自己的渲染引擎，Unity也有，所以Unity中国写了一套底层渲染API，把Flutter的意志移植到了Unity里成了UIVGS里，所以马上Unity将能开发app了。欢迎大家和我一起学习和开发 totototogether！！（unity无敌）
开源GitHub地址：
[UIWidgets](https://github.com/UnityTech/UIWidgets/)     |    [官方交流社区](https://connect.unity.com/g/uiwidgets)     |   [官方录播](https://www.bilibili.com/video/av47558897)     |    [Flutter中文文档](https://flutterchina.club/docs/)
[APPConnect](https://github.com/UnityTech/ConnectAppCN)    |   [Markdown RendererForUIWidget](https://github.com/suntabu/MarkdownRendererForUIWidgets)

# 总结官方的直播视频
##### A new way to build beautiful UI in Unity
## 1.什么是UIVGS？
*. unity的开源的插件包，将来会放到Asset store和unity的包管理器中‘
*. 是一个UI框架，是开发者根据flutter的源码和结合unity现状演变、pot过来的。
*. 可以构建app、游戏UI界面、编辑器插件的UI节目。
## 2.上手UIVGS
a.声明式的UI框架:开发者只需要C#代码里显示的定义好你的UI形状，布局需求，交互逻辑。下图是框架结构（七分精力原生开发，三分给前端）
![框架结构.png](https://upload-images.jianshu.io/upload_images/12103253-2527190890a20079.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
b.Hello World演示
```
public class StartApp : UIWidgetsPanel
{
    protected override Widget createWidget()
    {
        return new  Column(
            children:new List<Widget>
            {
                
                new Container(
                    width:300f,
                    height:300f,
                    decoration:new BoxDecoration(
                        image:new DecorationImage(
                            image:new AssetImage(
                                "doki-001")))
                    ),
                new Text("Doki: Hello UIVGS!",
                    style:new TextStyle(color: Color.white,fontSize:32))
            });
    }
}
```
![Hello World.png](https://upload-images.jianshu.io/upload_images/12103253-f3a4f7d23e6cf90f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

c.静态UI布局，UI自适应原理（Technical Details on Layout）
Bottom-up Resizing :置底向上计算每个widget的大小（确保每个父widget包含所有个子widget）
Top-down Positioning：至上而下计算每个widget的位置（确定每个父widget的位置后再根据此父widget下开发者定义的布局确定每个子widget确定相对位置）
最后换算出每个widget的绝对位置
![contain.png](https://upload-images.jianshu.io/upload_images/12103253-e4802ff44fa45cb3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
d.动态UI的布局
```
public class TestAPP : UIWidgetsPanel
{
    protected override Widget createWidget()
    {
        return new Counter();
    }

    class Counter : StatefulWidget
    {
        public override State createState()
        {
            return new CounterState();
        }
    }

    class CounterState : State<Counter>
    {
        private int countNum = 0;
        //渲染
        public override Widget build(BuildContext context)
        {
            return new GestureDetector(
                child: new Text("[TestAPP]Onclick:"+countNum, style: new TextStyle(color: Color.white, fontSize: 50)),
                onTap: () => { 
                    Debug.Log("<TestAPP>onclick debug");
                    this.setState(() =>
                    {
                        this.countNum++;
                    });
                }
            );
        }
    }
}
```
![点击更新.gif](https://upload-images.jianshu.io/upload_images/12103253-925a8760a3593762.gif?imageMogr2/auto-orient/strip)
##### 用户输入，交互Technical Details on Input
UIVGS会采集Unity EventSystem用户输入信息(touch,mouse等)，然后置顶向下传递，确定哪个widget被选中，被选中的widget会相关一个Gesture Delector的widget，Gesture Delector会记录所有的用户输入，并处理转化成更高级的用户输入（长按，点击，双击等），然后发送给所有的监听者(onTap，onPress等)
![Input.png](https://upload-images.jianshu.io/upload_images/12103253-1199af9df5bea68f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
组件树的原理通俗点讲就是：
stateful组件就是和用户交互后会有状态变化，例如滚动条Slider。
stateless组件就是交互后没有状态变化，例如显示的一个文本Text。
创建有状态的组件可以遵循以下步骤：
1.创建一个继承自StatefulWidget的类来表示你要自定义的可变控件
2.创建一个继承自State的类来处理这个可变控件的状态和显示样式（build方法）。
3.当用户交互发生，例如onPressed点击事件被触发时，可以调用setState方法告诉组件需要重绘
![只更新被标记的UIwidget.png](https://upload-images.jianshu.io/upload_images/12103253-8f63c9e92fc69042.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
e.渲染 Technical Details on Rendering技术
UIVGS会动态找到适合和相对静止不变的widgets子树，并且将他提前渲染到一个离屏的 Render Texture 上，然后缓存下来，在之后的渲染就不用一笔笔的渲染，只需要把这些不变UI的贴到屏幕上。![性能无敌.gif](https://upload-images.jianshu.io/upload_images/12103253-4f5babf3bccb4826.gif?imageMogr2/auto-orient/strip)
## 3.为什么UIVGS很重要
a.更适合做很棒的动画效果、颜色域广等的新用户体验的UI。

b.可以不仅仅局限于游戏UI界面，还可以做app、editor UI。

c.可以结合Unity，制作3d和增加粒子效果。

d.开发者只需要做前端，渲染效率交个引擎底层，解放了人力，提升了效率（运行速度快，基本上fps>60）。
因为传统用UGUI 不能太耗batches数量 资源  刷新率 Drawcall ，团队用用一个shader，需要合各种图，小图大图 合批。现在用UIWidgets动态合批，比如一个血条，他不变就不合，血条变了就合批。ugui  很耗cpu每次delete就需要合批 重画，每次tps很低，一般就建多个canvas，就把一些静态不变的图放在一个canvas，比如头像，icon等，经常放在一个canvas。UIWidgets有个树形结构，可以解决这些事，所以可以把人力解放出来。相比IMGUI，IMGUI比较原始，太底层，开发者不需要太关心表现层，但更多需要去维护底层逻辑。相比UIElement，UIElement开发模式太老，模仿的还是Html（div+css）。

## 4.UIVGS编程模式
刚上手可以能不太适应，因为习惯了C# 的链式开发，但换一种开发习惯也是很新很酷的开发体验。
#### a.关于UI as a function of state（实现业务逻辑和界面的真正的分离，方便理解和维护）
UIWidgets是和现在react和flutter一样是响应式编程,用状态生成UI。需要注意的是StatefulWidget和Building widgets这两个接口。
StatefulWidget提供两个方法：
.setState（fn）（触发状态改变的时候就会触发setState这个接口） 
.build（当开发者通过当前的触发状态自己实现和绘制新的UI，其中每次会生成widget tree，尽管每次都会新生成widgets树，但是每次新生成的树框架（framework）都会和当前的树进行比较，然后仅仅去创建、更新、删除那些底层组件。其次widgets都是一些简单的对象，构建这些对象的开销都非常小，尽管每次去setSate都会building新的widgets，但是这种开销都非常小））
![标记状态每帧过后去更新.png](https://upload-images.jianshu.io/upload_images/12103253-e8c2f386d380176b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### b.Router和Navigation(路由和导航器)
这里可以参考flutter的实现原理，[flutter Navigation文档](https://flutter.dev/docs/cookbook/navigation)，简单的说Router路由就是注册了所有页面，一个Map结构，对应的Key和value（页面），然后用Navigator用来管理堆栈功能（即push和pop）管理显示页面。
![push和pop操作.png](https://upload-images.jianshu.io/upload_images/12103253-2f9d4302ab007960.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### c.UIVGS和原生交互
提供一个了一个叫UIWidgetsMessageManager消息中心管理器的脚本（直接利用Unity和原生的交互的消息机制，做了一个直接的封装，用Unity提供的SendMessage方法发送给GameObject UIWidgetsMessageManager，然后按照不同的channel划分转发消息）。
![Unity Keyboard案例.png](https://upload-images.jianshu.io/upload_images/12103253-b0e2a750c7f53fe7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![Android原生.png](https://upload-images.jianshu.io/upload_images/12103253-aa411bbc7a290c99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### d.开发调试工具
首先在Unity editori的PlayerSettings 里加一条定义打开开发者模式![UIWidgets_DEBUG.png](https://upload-images.jianshu.io/upload_images/12103253-a6b2876912334ebe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![Editor分析器.png](https://upload-images.jianshu.io/upload_images/12103253-48389c82a83db206.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
查看每个widge(大小，权重、位置，颜色，字体等)以及对应的Render Tree的信息
##### Refersh 
手动刷新
#####Inspect Element 
具体点击UI元素的信息
##### DebugPaint
1.PaintSize：蓝色区域是内边距，淡蓝色线条轮廓线条。
2.PaintBaseLine：文本对齐的基线
3.PaintPointer：检测UI响应区域大小
4.PaintLayer：根据Layer划分widgets子树，做渲染缓存。
![2.gif](https://upload-images.jianshu.io/upload_images/12103253-2a1053c7976a619b.gif?imageMogr2/auto-orient/strip)
## 5.Roadmap
1.更高效：用新推出的DOTS技术，C#任务系统、ECS、Burst编译。
2.增加Cupertino风格
3.版本迭代更至Flutter1.2
4.用Tiny开发，更轻量级

##### 所以flutter同样适用Unity，用C#写就好
[推荐的Flutter资料](https://github.com/Solido/awesome-flutter)      |      [每周更新Flutter（外网）](http://www.youtube.com/playlist?list=PLjxrf2q8roU12XGwz3Km7sQZFTdB996iG)    |      [flutter]([https://github.com/flutter/flutter](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fflutter%2Fflutter)
)
[建了一个UIVGS交流，希望大家一起交流学习！！群二维码](https://github.com/qq245708870/UIVGS)
