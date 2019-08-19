# appium介绍

#### [官方网站](http://appium.io/)

#### 1、特点 {#1-特点}

---

appium 是一个自动化测试开源工具，支持 iOS 平台和 Android 平台上的原生应用，web应用和混合应用。

* “移动原生应用”是指那些用iOS或者 Android SDK 写的应用（Application简称app）。

* “移动web应用”是指使用移动浏览器访问的应用（appium支持iOS上的Safari和Android上的 Chrome）。

* “混合应用”是指原生代码封装网页视图——原生代码和 web 内容交互。比如，像 Phonegap，可以帮助开发者使用网页技术开发应用，然后用原生代码封装，这些就是混合应用。

重要的是，appium是一个跨平台的工具：它允许测试人员在不同的平台（iOS，Android）使用同一套API来写自动化测试脚本，这样大大增加了iOS和Android测试套件间代码的复用性。

#### 2、appium与Selenium {#2-appium与selenium}

---

appium类库封装了标准Selenium客户端类库，为用户提供所有常见的JSON格式selenium命令以及额外的移动设备控制相关的命令，如多点触控手势和屏幕朝向。

appium客户端类库实现了Mobile JSON Wire Protocol（一个标准协议的官方扩展草稿）和W3C WebDriver spec（一个传输不可预知的自动化协议，该协议定义了MultiAction 接口）的元素。

appium服务端定义了官方协议的扩展，为appium 用户提供了方便的接口来执行各种设备动作，例如在测试过程中安装/卸载App。这就是为什么我们需要appium特定的客户端，而不是通用的Selenium 客户端。当然，appium 客户端类库只是增加了一些功能，而实际上这些功能就是简单的扩展了Selenium 客户端，所以他们仍然可以用来运行通用的Selenium会话。

#### 3、支持多平台、多语言 {#3-支持多平台-多语言}

---

appium是跨平台的，可以用在OSX，Windows以及Linux桌面系统上运行。

appium选择了Client/Server的设计模式。只要client能够发送http请求给server，那么的话client用什么语言来实现都是可以的，这就是appium及Selenium\(WebDriver\)如何做到支持多语言的原因；

appium扩展了WebDriver的协议，没有自己重新去实现一套。这样的好处是以前的WebDriver API能够直接被继承过来，以前的Selenium（WebDriver）各种语言的binding都可以拿来就用，省去了为每种语言开发一个client的工作量；

| 语言/框架 | Github地址 |
| :--- | :--- |
| Ruby | [https://github.com/appium/ruby\_lib](https://github.com/appium/ruby_lib) |
| Python | [https://github.com/appium/python-client](https://github.com/appium/python-client) |
| Java | [https://github.com/appium/java-client](https://github.com/appium/java-client) |
| JavaScript \(Node.js\) | [https://github.com/admc/wd](https://github.com/admc/wd) |
| Objective C | [https://github.com/appium/selenium-objective-c](https://github.com/appium/selenium-objective-c) |
| PHP | [https://github.com/appium/php-client](https://github.com/appium/php-client) |
| C\# \(.NET\) | [https://github.com/appium/appium-dotnet-driver](https://github.com/appium/appium-dotnet-driver) |
| RobotFramework | [https://github.com/jollychang/robotframework-appiumlibrary](https://github.com/jollychang/robotframework-appiumlibrary) |

#### 4、appium工作原理 {#4-appium工作原理}

---

在安装和介绍appium之前，非常有必要介绍一下appium是如何工作的。

## 一、appium加载的过程图解： {#一appium加载的过程图解}

![](https://img2018.cnblogs.com/blog/1418970/201811/1418970-20181109210027402-413842488.jpg)

appium的加载过程

* 1.调用Android adb完成基本的系统操作
* 2.向Android上部署bootstrap.jar
* 3.bootstrap.jar Forward Android的端口到PC机器上
* 4.PC上监听端口接收请求，使用webdriver协议
* 5.分析命令并通过forward 端口发给bootstrap.jar
* 6.bootstrap.jar接收请求并把命令发给uiautomator
* 7.ui automator执行命令

## 二、初步认识appium工作过程 {#二初步认识appium工作过程}

* 1.appium有C/S模式
* 2.appium是基于webdriver协议对移动设备自动化api扩展而成的，所有具有和webdriver一样的特性，比如多语言支持。
* 3.webdriver是基于http协议的，第一连接会建立一个session会话，并通过post发送一个json告知服务端相关测试信息。
* 4.对于Android来说，4.2以后是基于UiAutomator框架实现查找注入事件的，4.2以前则是instrumentation框架的，并封装成一个叫Selendroid提供服务。
* 5.客户端只需要发送http请求实现通讯，意味着客户端就是多语言支持的。
* 6.appium服务端是node.js写的，所以安装那个平台都是先安装node，然后npm install -g appium\(需要翻墙\)。

## 三、bootstrap {#三bootstrap}

### 1.bootstrap的作用 {#bootstrap的作用}

bootstrap是Appium运行在安卓测试机傻姑娘的一个UIAutomator测试脚本，该脚本的唯一功能就是在目标机器开启一个socket服务器来把一个session中Appium从PC端过来的命令发送给UiAutoamtor来执行处理。  
它会监听4724端口获得命令，然后交给UiAutomator来处理。

### 2.bootstrap {#bootstrap}

首先，bootstrap是uiautomator的测试脚本，它的入口类bootstrap继承于UiautomatorTestCase，所以Uiautomator可以正常运行它，它也可以正常使用uiautomator的方法，这是就是appium的命令可以转换成uiautomator的关键；  
其次，，bootstrap是一个socket服务器，专门监听4724端口过来的appium的连接和命令数据，并把appium的命令转换成uiautomator的命令来让uiautomator进行处理；  
最后，bootstrap处理的是从PC端传过来的命令，而非一个文件。

## 四、所使用的技术 {#四所使用的技术}

Android上使用了instrumentation和uiautomator两套技术  
ios使用uiautomation  
同时支持Firefox，并可扩展其他平台。  
默认开启4723端口接收webdriver请求可，4723是appium服务，专门和脚本打交道；  
默认开启4724，用于和Android设备通讯  
新版本的appium Android增加了UIautoamator2的支持，iOS换成了XCUItest

## 五、capabilities {#五capabilities}

capabilities是一些键值对的集合。客户端将键值对发送给appium服务端，用来告诉服务端怎样开始测试。

## 六、工作原理 {#六工作原理}

Appium启动时会创建一个http://127.0.0.1:4723/wd/hub服务端\(相当于一个中转站\)，脚本会告诉服务器我要做什么，服务端再去跟设备打交道。  
服务端和设备默认使用4724端口进行通信的，底层调用uiautoamator工具，在测试的时候服务端给设备扔一个bootstrap.jar，会启动这个包，启动之后会在手机上创建一个socket服务，暴露的就是4723端口；相对socket服务来说，appium服务端又是一个客户端；  
服务端收到脚本传递过来的命令之后，通过电脑上的4724端口，向设备的4724端口发送指令，bootstrap.jar收到指令后会去完成点击，滑动等操作，完成之后再给服务端一个响应。服务端收到之后再去运行脚本。



#### 5、你都需要安装什么？ {#5-你都需要安装什么}

---

首先，appium支持多语言，因为它针对流的几种语言分别开发的相应的appium库。好处就是我们可以选择自己熟悉的语言编写appium脚本。

其次，appium支持多平台，包括MAC和Windows。它针对这两大平台开发了appium-Server。

最后，appium又同时支持Android 和 iOS两个操作系统。

这就使得appium变得非常灵活。

当我在MAC平台上，通过Python（python-client ）编写了一个appium自动化脚本并执行，请求会首先到 appium.dum （MAC下的appium-Server），appium-Server通过解析，驱动iOS设备来执行appium自动化脚本。

或者，我在Windows平台上，通过Java（ java-client ）编写了一个appium自动化脚本并执行，请求会首先到 appiumForWindow.zip（Window下的appium-Server），appium-Server通过解析，驱动Android虚拟机或真机来执行appium脚本。

所以，你会看到appium的强大之处就在于此。

这才是你最关心的问题，使用appium都需要安装些什么？其实，从appium工作原理你就应该知道需要装什么了。





