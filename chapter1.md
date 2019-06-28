# e2e

**什么是E2E：**

E2E（End To End）即端对端测试，属于黑盒测试，通过编写测试用例，自动化模拟用户操作，确保组件间通信正常，程序流数据传递如预期。

### 典型E2E测试框架对比 {#典型e2e测试框架对比}

| 名称 | 断言 | 是否跨浏览器支持 | 实现 | 官网 | 是否开源 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| nightwatch | assert和Chai Expect | 是 | selenium | [http://nightwatchjs.org/](http://nightwatchjs.org/) | 是 |
| cypress | Chai Chai-jQuery等 | 否 | Chrome | [https://www.cypress.io/](https://www.cypress.io/) | 是 |
| testcafe | 自定义的断言 | 是 | 不是基于selenium实现 | [https://devexpress.github.io/testcafe/](https://devexpress.github.io/testcafe/) | 是 |
| katalon | TDD/BDD | 是 | 未知 | [https://www.katalon.com/katalon-studio/](https://www.katalon.com/katalon-studio/) | 否 |

1. nightwatch 需要安装配置 selenium，selenium-server需要安装jdk（Java Development Kit）。
2. cypress 安装方便，测试写法和单元测试一致，只支持 Chrome 类浏览器，有支持其他浏览器的计划。
3. testcafe 安装方便，测试写法与之前的单元测试差异比较大，编写测试用例时只能选择到可见的元素。
4. katalon 不是测试框架，是 IDE ，比较重，并且不开源，测试用例不是用 js 编写但是可以通过 Chrome 插件录测试用例
   。

_经过测试使用对比，nightwatch和cypress是vue推荐的框架，社区活跃度较高，功能较为完备，开源，推荐二选一（nightwatch需要装jdk，准备工作多，但API丰富度更高；cypress有自己的可视化窗体，能记录运行信息，重现bug精品）。_

我选择Cypress

官方教程：

[https://docs.cypress.io/guides/getting-started/installing-cypress.html\#System-requirements](https://docs.cypress.io/guides/getting-started/installing-cypress.html#System-requirements)

1.安装

Installing Cypress via`npm`is easy:

```
cd /your/project/path
```

```
npm install cypress --save-dev
```

2.启动

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

3.测试用例编写

![](/assets/1.png)

4.运行调试

![](/assets/2.png)

![](/assets/3.png)

5.Dashboard Service

1. 查看失败，通过，挂起和跳过测试的数量。
2. 获取失败测试的整个堆栈跟踪。
3. 查看测试失败或使用cy.screenshot（）时拍摄的屏幕截图。
4. 在测试失败时观看整个测试运行或视频片段的视频。
5. 查看您的规范文件在CI中的运行速度，包括它们是否并行运行。
6. 查看相关的测试分组。
7. 管理谁有权访问您记录的测试数据。
8. 查看每个组织的使用详情。

![](/assets/4.png)

结果

![](/assets/5.png)

6.CI运行record命令

![](/assets/6.png)

会自动生成报告，运行视频及报错截图

![](/assets/7.png)

7.运行效果（模拟器测试手动录屏）

[http://mudu.ns.4l.hk/show/videolink/m76rb39l/origin](http://mudu.ns.4l.hk/show/videolink/m76rb39l/origin)

