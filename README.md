# KChatHttp2SDKGuidance

真正的KChat项目底层实现为http2，这个技术非常新，能给我们带来很多的新特性，但是这个版本的SDK非常挑运行时，基本上除非是原生App，几乎没有能够使用的地方，所谓有利有弊吧。

我们还提供了KChatSDK的Http1.1实现，请往这里：https://github.com/czf0613/KChatWebSocketSDKGuidance

Http1.1版本的实现是完全开源的，也非常欢迎大家一起添砖加瓦，但是Http1.1终究还是有些功能有所牺牲。

## 建议&警告
我们强烈建议，除非您使用的编程语言能够提供原生级别的Http2实现，否则不要使用这个版本，会给您带来很多麻烦。
此外，由于Http2强行要求比较高版本的TLS传输层加密，因此只能兼容TLS 1.2以上的传输层加密，这个基本上已经枪毙了市面上绝大多数的设备了。

### 能使用的场景
1，Android 7.0以上，并且只能使用OkHttp库作为请求发起的工具。

2，Apple系列中能支持Swift-NIO与Swift 5.6的设备。（也就是说基本上只有iOS 15+，以及macOS Catalina之后的设备才能用，应用面极其窄）

3，Windows 10以上的64位系统。（只有这个版本及以上的系统动态链接库才带有Http2的支持）

4，Golang、python的某些第三方库可以实现原生Http2请求，这个也是可以拿来使用的，但一般没有人拿这些语言搞GUI。

5，（您只要有本事提供原生Http2的支持，比如说C++编译一个libhttp2，然后FFI）

### 不建议使用的场景
当然，您要是有本事从Socket开始徒手写http2的实现，以下平台倒也不是不能搞。

1，浏览器

2，各类小程序（本质上它们也是浏览器）

3，Electron等套壳桌面应用程序（它们本质上也是浏览器，但是它们可以通过引用一些动态库从而在系统里获得Http2的实现能力，但是我不建议使用这样的方式）

4，低于Windows 10的电脑（会因为找不到动态链接库而报错，但是您可以通过自己的程序携带静态链接库而规避这个问题）

5，大部分的低于iOS15（同年代的）苹果系产品都不太能用（最新系版统中可用，但是这些用户实在是太少了）

6，低于安卓7.0的设备（也不是说就一定不行，但是兼容性无法保证）

7，非主流编程语言（……

# 接口文档

请看根目录下的protos文件夹下的proto文件，内有非常详细的接口注释。

SDK使用流程请参考下图：

``` mermaid
graph TD;
contact[联系我们获取AppId与AppKey]
download[下载对应系统的SDK或参照SDKGuidance]
contact --> register(调用user.proto中定义的createUser方法注册当前用户)
download --> register
register --用户不存在则新建--> guid[返回ChatSDKUserGuid]
register --> guid
guid --> success(可以成功调用chat.proto里面的服务了)
```

