# e2e

#### 简单运行效果（模拟器测试手动录屏）

[http://mudu.ns.4l.hk/show/videolink/m76rb39l/origin](http://mudu.ns.4l.hk/show/videolink/m76rb39l/origin)

#### **什么是E2E：**

E2E（End To End）即端对端测试，属于黑盒测试，通过编写测试用例，自动化模拟用户操作，确保组件间通信正常，程序流数据传递如预期。

#### **典型E2E测试框架对比**

| 名称 | 断言 | 是否跨浏览器支持 | 实现 | 官网 | 是否开源 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| nightwatch | assert和Chai Expect | 是 | selenium | [http://nightwatchjs.org/](http://nightwatchjs.org/) | 是 |
| cypress | Chai Chai-jQuery等 | 否 | Chrome | [https://www.cypress.io/](https://www.cypress.io/) | 是 |
| testcafe | 自定义的断言 | 是 | 不是基于selenium实现 | [https://devexpress.github.io/testcafe/](https://devexpress.github.io/testcafe/) | 是 |
| katalon | TDD/BDD | 是 | 未知 | [https://www.katalon.com/katalon-studio/](https://www.katalon.com/katalon-studio/) | 否 |

1. nightwatch 需要安装配置 selenium，selenium-server需要安装jdk（Java Development Kit）。
2. cypress 安装方便，测试写法和单元测试一致，只支持 Chrome 类浏览器，有支持其他浏览器的计划。
3. testcafe 安装方便，测试写法与之前的单元测试差异比较大，编写测试用例时只能选择到可见的元素。
4. katalon 不是测试框架，是 IDE ，比较重，并且不开源，测试用例不是用 js 编写但是可以通过 Chrome 插件录测试用例。

#### **优秀的端到端测试工具应该有哪些特点？**

* 安装简易：我希望它非常容易安装，最好可以一行命令就可以安装完毕
* 依赖较少：我只想做个E2E测试，不想安装jdk, python之类的东西
* 速度很快：运行测试用例的速度要快
* 报错详细：详细的报错
* API完备：鼠标键盘操作接口，DOM查询接口等
* Debug方便：出错了可以很方便的调试，而不是去猜

#### Cypress基本上拥有了上面的特点之外，还有以下特点：

* `时光穿梭`
  测试运行时，Cypress会自动截图，你可以轻易的查看每个时间的截图。
* `Debug友好`
  不需要再去猜测为什么测试有失败了，Cypress提供Chrome DevTools, 所以Debug是非常方便的。
* `实时刷新`
  Cypress检测测试用例改变后，会自动刷新。
* `自动等待`
  不需要在使用wait类似的方法等待某个DOM出现，Cypress会自动帮你做这些。
* `Spies, stubs, and clocks`
  验证并控制函数，服务器响应或计时器的行为。
* `网络流量控制`
  在不涉及服务器的情况下轻松控制，存根和测试边缘案例。无论你喜欢，你都可以存储网络流量。
* `一致的结果`
  我们的架构不使用Selenium或WebDriver。向快速，一致和可靠的无剥落测试问好。
* `截图和视频`
  查看失败时自动截取的截图，或无条件运行时整个测试套件的视频。

#### 缺点：

* 它仅适用于一个客户端（语言），即仅适用于JavaScript。 因此，要使用它，您必须知道JavaScript：但是这可能是JavaScript应用程序的一个优势，同时也将是cypress的一个缺点。
* 该结构与其他Selenium端到端工具不同，之前习惯使用其它E2E测试工具的使用者来说，您可能需要花费更多时间来理解结构并找到创建脚本的最佳方法。
* 社区：由于cypress相对较新，社区规模较小。 你很难找到问题的答案等。

#### 官方教程：

[https://docs.cypress.io/guides/getting-started/installing-cypress.html\#System-requirements](https://docs.cypress.io/guides/getting-started/installing-cypress.html#System-requirements)

##### 1.安装

Installing Cypress via`npm`is easy:

```
cd /your/project/path
```

```
npm install cypress --save-dev
```

##### 2.启动

**The long way with the full path**

```
./node_modules/.bin/cypress open
```

**Or with the shortcut using**`npm bin`

```
$(npm bin)/cypress open
```

**Or by using**`npx`

**note**:[npx](https://www.npmjs.com/package/npx)is included with`npm > v5.2`or can be installed separately.

```
npx cypress open
```

**Or by using**`yarn`

```
yarn run cypress open
```

##### 3.初始化cypress

    |-- fixtures
    |-- integration
    |   `-- example_spec.js
    |-- plugins
    |   `-- index.js
    `-- support
        |-- commands.js
        `-- index.js

fixtures 文件夹存放自定义 json 文件，integration 文件夹编写测试，plugins 和 support 是非必须使用的文件夹，需要自定义指令的时候会用到。

##### 4.测试用例编写

![](/assets/1.png)

##### 5.模拟器脚本控制界面

![](/assets/2.png)

##### 6.模拟器运行调试界面

![](/assets/3.png)

##### 7.Dashboard Service\(仪表盘服务\)

通过仪表盘服务可以得到如下信息：

* 查看失败，通过，挂起和跳过测试的数量。
* 获取失败测试的整个堆栈跟踪。
* 查看测试失败或使用cy.screenshot（）时拍摄的屏幕截图。
* 在测试失败时观看整个测试运行或视频片段的视频。
* 查看您的规范文件在CI中的运行速度，包括它们是否并行运行。
* 查看相关的测试分组。
* 管理谁有权访问您记录的测试数据。
* 查看每个组织的使用详情。

![](/assets/4.png)

##### 8.运行结束后

![](/assets/5.png)

##### 9.CI运行record命令

![](/assets/6.png)

##### 10.自动生成报告，运行视频及报错截图，保存到本地

![](/assets/7.png)

##### 



