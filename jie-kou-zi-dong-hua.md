

python接口自动化

接口基础知识

接口分类：http/Webservice接口

http

http请求：状态行（请求方式get/post等，url，

协议版本）、请求头（含用户代理、Cookie

等）、请求正文

http响应：状态行（含状态码）、响应头、响应

正文（响应数据格式html/xml/json等）

封装http请求 import requests,封装常用的get和post请求

unittest测试框架

unittest.TestCase--写用例，一个用例就是一个

函数

函数以test\_xx开头,只有self关键字，不能传参

数据库测试数据初始化

断言：asserEqual\(a,b\)/assetIn\(a,b\)等， 必要时断言中增加数据库校验

解决用例间的依赖关系

3

unittest.TestSuit--创建容器，收集用例

unittest.TestLoader--找用例，加载用例，存到

suite中，加载用例的三种方式

按顺序逐条加载--suite.addTest\(测试类名（用例

函数名））

从类中查找然后加载--suite.addTest\(loader.

loadTestsFromTestCase\(测试类名\)

从模块中查找然后加载--suite.addTest\(loader.

loadTestsFromModule\(模块名\)

执行用例，出具测试报告

import HTMLTestRunnerNew,利用上下文管理

器，输出html格式的报告

unittest中测试用例的初始化--setUp/tearDown,

夹心饼干成对出现，分别在每条用例的前后执行

ddt数据驱动

3

测试数据的处理

将测试数据存放在excel中，通常存放case\_id/

title/method/url/data/exceped/sql/respose/result

openpyxl封装从excel中读/写数据的类

利用pandas处理excel

将excel测试数据中某些字段使用正则替换成初始

化参数

配置文件config的设计与封装

import configparser,封装读取配置文件的类

实现执行用例的可配置

实现host/DB的可配置

实现文件路径的可配置

实现邮件发送对象等的可配置

将操作日志、邮件发送封装成类 日志、邮件

结合jenkins

