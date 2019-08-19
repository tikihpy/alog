# [【自动化测试】Python - unittest单元测试框架](https://segmentfault.com/a/1190000016315201) {#articleTitle}



* [框架学习](https://segmentfault.com/t/%E6%A1%86%E6%9E%B6%E5%AD%A6%E4%B9%A0/blogs)
* [单元测试](https://segmentfault.com/t/%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95/blogs)
* [![](https://avatar-static.segmentfault.com/252/177/2521771040-54cb53b372821_small)python](https://segmentfault.com/t/python/blogs)

5k 次阅读  ·  读完需要 35 分钟

4

---

## **一、测试模型** {#articleHeader0}

下面这部分来自于某书籍资料，拿过来，按需参考一下：

> **测试模型**
>  
> **（1）线性测试**
>  
> 1、概念：
>  
> 通过录制或编写对应应用程序的操作步骤产生的线性脚本。单纯的来模拟用户完整的操作场景。（操作，重复操作，数据）都混合在一起。
>  
> 2、优点：每个脚本相对独立，且不产生其他依赖和调用。任何一个测试用例脚本拿出来都可以单独执行。
>  
> 3、缺点：开发成本高，用例之间存在重复的操作。比如重复的用户登录和退出。
>  
> 维护成本高，由于重复的操作，当重复的操作发生改变时，则需要逐一进行脚本的修改。
>  
> 4.线性测试实例：用户登录
>  
> **（2）模块化驱动测试**
>  
> 1、概念：
>  
> 将重复的操作独立成功共模块，当用例执行过程中需要用到这一模块操作时则被调用。
>  
> 操作+（重复操作，数据）混合在一起。例如，自动化测试的执行需要保持测试用例的独立性和完整性，所以每一条用例在执行时都需要登录和退出操作，so可以把登录和退出的操作封装为公共函数。
>  
> 2、优点：由于最大限度消除了重复，从而提高了开发效率和提高测试用例的可维护性。
>  
> 3、缺点：虽然模块化的步骤相同，但是测试数据不同。比如说重复的登录模块，如果登录用户不同，依旧要重复编写登录脚本。
>  
> 4.实例：对公共模块，例如登陆和退出进行模块化封装
>  
> **（3）数据驱动测试**
>  
> 1、概念：它将测试中的测试数据和操作分离，数据存放在另外一个文件中单独维护。
>  
> 通过数据的改变从而驱动自动化测试的执行，最终引起测试结果的改变。
>  
> 操作+重复操作+数据分开。
>  
> 2、优点：
>  
> 通过这种方式，将数据和重复操作分开，可以快速增加相似测试，完成不同数据情况下的测试。
>  
> 3、实例从excel表格读取用户名密码，登录邮箱。

---

## **二、unittest框架** {#articleHeader1}

用Python搭建自动化测试框架，需要组织用例以及测试执行，大部分推荐的是unittest。现在用的也是这个，随着了解，也有其他的框架，有时间再多去学习，保持持续学习哦~  
附上官方文档地址：链接描述[https://docs.python.org/2.7/l...](https://docs.python.org/2.7/library/unittest.html#)

unittest是Python自带的单元测试框，可以用来作自动化测试框架的用例组织执行框架。优点：提供用例组织与执行方法；提供比较方法；提供丰富的日志、清晰的报告。  
**大致流程：**

* 写好TestCase
* 由TestLoader加载TestCase到TestSuite
* 然后由TextTestRunner来运行TestSuite，运行的结果保存在TextTestResult中。
 
  通过命令行或者unittest.main\(\)执行时，main会调用TextTestRunner中的run\(\)来执行，或者可以直接通过TextTestRunner来执行用例。
 
  在Runner执行时，默认将执行结果输出到控制台，我们可以设置其输出到文件，在文件中查看结果。

**unittest中最核心的部分是：TestFixture、TestCase、TestSuite、TestRunner。**

### **1、Test fixture** {#articleHeader2}

用于一个测试环境的准备和销毁还原。  
当测试用例每次执行之前需要准备测试环境，每次测试完成后还原测试环境，比如执行前连接数据库、打开浏览器等，执行完成后需要还原数据库、关闭浏览器等操作。这时候就可以启用testfixture。

* setUp\(\)：准备环境，执行每个测试用例的前置条件；
* tearDown\(\)：环境还原，执行每个测试用例的后置条件；
* setUpClass\(\)：必须使用@classmethod装饰器，所有case执行的前置条件，只运行一次；
* tearDownClass\(\)：必须使用@classmethod装饰器，所有case运行完后只运行一次；

例如：

```
# 重写TestCase的setUp() tearDown()方法：在每个测试方法执行前以及执行后各执行一次
def
setUp
(
self
)
:    
# 钩子方法

    print(
"do something before test : prepare environment"
)

def
tearDown
(
self
)
: 
# 钩子方法

    print(
"do something after test : clean up "
)
```

### **2、TestCase** {#articleHeader3}

类，unittest.TestCase  
一个类class继承 unittest.TestCase，就是一个测试用例。一个TestCase的实例就是一个测试用例，就是一个完整的测试流程。  
包括测试前环境准备setUp\(\)\|setUpClass\(\)、执行代码run\(\)、测试环境后的还原tearDown\(\)\|tearDownClass\(\)。  
**继承自unittest.TestCase的类中，测试方法的名称要以test开头。且只会执行以test开头定义的方法（测试用例）。**

例如：【先准备待测试的方法function.py】

```
#!/usr/bin/python3
# -*- coding:utf-8 -*-
def
add
(a,b)
:
    
return
 a+b

def
minus
(a,b)
:
    
return
 a-b

def
multi
(a,b)
:
    
return
 a*b

def
divide
(a,b)
:
    
return
 a/b
```

【测试脚本】：

```
import unittest
from A_UnitTest_basicDemo_ok.function import *


class
TestFunc
(
unittest
.
TestCase
):
# 继承自unittest.TestCase
# 重写TestCase的setUp()、tearDown()方法：在每个测试方法执行前以及执行后各执行一次
def
setUp
(
self
)
:
        print(
"do something before test : prepare environment"
)

    
def
tearDown
(
self
)
:
        print(
"do something after test : clean up "
)
    
    
# 测试方法均已test开头，否则是不被unittest识别的
def
test_add
(
self
)
:
        print(
"add:"
)
        
self
.assertEqual(
3
,add(
1
,
2
))

    
def
test_minus
(
self
)
:
        print(
"minus"
)
        
self
.assertEqual(
3
,minus(
5
,
2
))

    
def
test_multi
(
self
)
:
        print(
"multi"
)
        
self
.assertEqual(
6
,multi(
2
 ,
3
))

    
def
test_divide
(
self
)
:
        print(
"divide"
)
        
self
.assertEqual(
2
,divide(
4
,
2
))


if
 __name_
_
 == 
"__main__"
:
    
# 在main()中加verbosity参数，可以控制输出的错误报告的详细程度
# verbosity=*：默认是1；设为0，则不输出每一个用例的执行结果；2-输出详细的执行结果

    unittest.main(verbosity=
2
)
```

或者也可以使用setUpClass\(\) & tearDownClass\(\)方法：

```
# 如果想在所有case执行之前准备一次测试环境，并在所有case执行结束后再清理环境
@classmethod
def
setUpClass
(cls)
:

    print(
"this setupclass() method only called once"
)

@classmethod
def
tearDownClass
(cls)
:

    print(
"this teardownclass() method only called once too"
)
```

【verbosity】  
在测试用例文件的末尾加上如下代码：

```
if
 __name__ == 
"__main__"
:
    unittest.main(verbosity=
2
)  
# 输出详细的错误报告
```

在unittest.main\(\)中加参数verbosity可以控制错误报告的详细程度：默认为1。0，表示不输出每一个用例的执行结果；2表示详细的执行报告结果。

【执行结果】：输出到控制台

```
this
setupclass() method only called once
test_add
(__main__.TestFunc) ... ok
add
:
test_divide
(__main__.TestFunc) ... ok
divide
test_minus
(__main__.TestFunc) ... ok
minus
test_multi
(__main__.TestFunc) ... ok
multi
this
teardownclass() method only called once too
----------------------------------------------------------------------
Ran
4 tests in 0.000s
OK
```

### **3、TestSuite** {#articleHeader4}

上述简单的测试会产生两个问题，可不可以控制test测试用例的执行顺序？若不想执行某个测试用例，有没有办法可以跳过？  
对于执行顺序，默认按照test的 A-Z、a-z的方法执行。若要按自己编写的用例的先后关系执行，需要用到testSuite。  
把多个测试用例集合起来，一起执行，就是testSuite。testsuite还可以包含testsuite。  
一般通过addTest\(\)或者addTests\(\)向suite中添加。case的执行顺序与添加到Suite中的顺序是一致的。  
例如：run\_main.py

```
if __name__ == 
"__main__"
:
    suite = unittest.TestSuite()

# 定义list，按照list里的顺序执行测试用例

tests=[TestFunc(
"test_add"
),TestFunc(
"test_minus"
),TestFunc(
"test_multi"
),TestFunc(
"test_divide"
)]
suite.addTests(tests)
runner = unittest.TextTestRunner(verbosity=2)
runner.run(suite)
```

TestSuite可以再包含testsuite，示例如下：

```
suite1
 = module.TheTestSuite()

suite2
=module.TheTestSuite()

alltests
=unittest.TestSuite([suite1],[suite2])
```

#### **跳过某个case：skip装饰器**

若想让某个测试用例不执行，有没有办法呢？当然是有的，可以使用skip装饰器。  
例如：

```
@unittest.skip(
"i don't want to run this case -
>
 test_minus() ... "
)

def
test_minus
(
self
)
:
    print(
"minus"
)
    
self
.assertEqual(
3
,minus(
5
,
2
))
```

加上“@unittest.skip\(\)”后，执行看看，对比控制台的输出结果就可以明显看出区别了。  
Skip装饰器有如下几种情况：  
（1）skip\(\):无条件跳过  
`@unittest.skip("i don't want to run this case. ")`  
（2）skipIf\(condition,reason\):如果condition为true，则 skip  
`@unittest.skipIf(condition,reason)`  
（3）skipUnless\(condition,reason\):如果condition为False,则skip  
`@unittest.skipUnless(condition,reason)`

（4）还可以使用TestCase.skipTest\(reason\)。例如：

```
def
test_divide
(
self
)
:
    
self
.skipTest(
'do not run  test_divide()'
)
    print(
"divide"
)
    
self
.assertEqual(
2
,divide(
4
,
2
))
```

控制台输出（部分）：

```
test_divide (__main__.TestFunc) ... skipped '
do
not
run test_divide()
'
```

### **4、TestLoader** {#articleHeader5}

TestLoadder用来加载TestCase到TestSuite中。  
loadTestsFrom_\*_\(\)方法从各个地方寻找testcase，创建实例，然后addTestSuite，再返回一个TestSuite实例。  
该类提供以下方法：

```
unittest
.
TestLoader
().loadTestsFromTestCase(testCaseClass)

unittest
.
TestLoader
().loadTestsFromModule(
module
)
unittest.TestLoader().loadTestsFromName(
name
,
module
=
None
)
unittest.TestLoader().loadTestsFromNames(
names
,
module
=
None
)
unittest.TestLoader().getTestCaseNames(
testCaseclass
)
unittest.TestLoader().discover()
```

**TestLoader 之discover：**  
用discover\(\)来加载测试多个测试用例，再用TextRunner类的run\(\)方法去一次执行多个脚本的用例，达到批量执行的效果。  
`discover`方法里面有三个参数：  
`-case_dir`:这个是待执行用例的目录。  
`-pattern`：这个是匹配脚本名称的规则，test\*.py意思是匹配test开头的所有脚本。  
`-top_level_dir`：这个是顶层目录的名称，一般默认等于None就行了。

**TextTestRunner\(\)**：执行测试用例。runner.run\(test\)会执行TestSuite、TestCase中的run\(result\)方法。  
如下：run\_main.py示例

```
import
 unittest

import
 os

# 用例的路径

case_path = os.path.join(os.getcwd(),
"case"
)

# 报告存放的路径

report_path = os.path.join(os.getcwd(),
"report"
)

def
all_cases
()
:

    discover= unittest.defaultTestLoader.discover(case_path,pattern=
"test*.py"
,top_level_dir=
None
)
    print(discover)
    
return
 discover

if
 __name__ == 
"__main__"
:
    runner = unittest.TextTestRunner(verbosity=
2
)
    runner.run(all_cases())
```

### **5、生成测试报告** {#articleHeader6}

* **生成TXT测试报告**

代码示例：

```
if
 __name__ == 
"__main__"
:
    suite = unittest.TestSuite()
    
# 生成.txt的测试报告（控制台的输出写入到文件中）

    suite.addTests(unittest.TestLoader().loadTestsFromTestCase(TestFunc))
    
with 
open
(
'UnittestTextReport.txt'
,
'a'
) 
as
 f:
        runner
 = unittest.TextTestRunner(stream=f,verbosity=
2
)
        runner.run(suite)
```

可以看到在目录下，生成了UnittestTextReport.txt文件。  
但是txt格式的文件太过于简陋。我们可以借助与第三方提供的库来输出更加形象的html报告，也可以自定义输出自己想要格式的html格式的报告。

* **生成HTML测试报告**
* 先下载HTMLTestRunner.py（注意Python的版本），
 
  [http://tungwaiyip.info/softwa...](http://tungwaiyip.info/software/HTMLTestRunner.html)
  。然后放在Python的Lib目录下；
* 在run\_main.py文件中加入：from HTMLTestRunner import HTMLTestRunner

`HTMLTestRunner()`方法有三个参数：  
`--stream`:测试报告写入文件的存储区域  
`--title`:测试报告的主题  
`--description`：测试报告的描述

代码示例：

```
if
 __name__ == 
"__main__"
:
    suite = unittest.
TestSuite
()
    # 生成
HTML
格式的具体测试报告
    
with
 open(
'HtmlReport
.html',
'w
b') as f:  # 在python3，要写成
'w
b' or 
'w
r'
        runner = 
HTMLTestRunner
(stream=f,title=
'function
 test   
report',description=
'generated
 by 
HTMLTestRunner
',verbosity=
2
)
        runner.run(suite)
```

## **三、代码示例** {#articleHeader7}

* **function.py**

```
#!/usr/bin/python3
# -*- coding:utf-8 -*-
def
add
(a,b)
:
    
return
 a+b

def
minus
(a,b)
:
    
return
 a-b

def
multi
(a,b)
:
    
return
 a*b

def
divide
(a,b)
:
    
return
 a/b
```

* **Test\_function.py**

```
#!/usr/bin/python3
# -*- coding:utf-8 -*-
import
 unittest

from
 UnitTest_1.function 
import
 *  
# from..import ... :要指定文件的路径
class
TestFunc
(unittest.TestCase)
:
# unittest.TestCase
# 如果想在所有case执行之前准备一次测试环境，并在所有case执行结束后再清理环境
    @classmethod
def
setUpClass
(cls)
:

        print(
"this setupclass() method only called once"
)

    @classmethod
def
tearDownClass
(cls)
:

        print(
"this teardownclass() method only called once too"
)

    
# 测试方法均已test开头，否则是不被unittest识别的
def
test_add
(self)
:

        print(
"add:"
)
        self.assertEqual(
3
,add(
1
,
2
))
    
def
test_minus
(self)
:

        print(
"minus"
)
        self.assertEqual(
3
,minus(
5
,
2
))
    
# 如果想临时跳过某个case：skip装饰器
    @unittest.skip("i don't want to run this case. ")
def
test_multi
(self)
:

        print(
"multi"
)
        self.assertEqual(
6
,multi(
2
,
3
))
    
def
test_divide
(self)
:

        print(
"divide"
)
        self.assertEqual(
2
,divide(
5
,
2
))


if
 __name__ == 
"__main__"
:
    
# 在main()中加verbosity参数，可以控制输出的错误报告的详细程度
# verbosity=*：默认是1；设为0，则不输出每一个用例的执行结果；2-输出详细的执行结果

    unittest.main(verbosity=
2
)
```

* **Test\_suite.py**

```
#!/usr/bin/python3
# -*- coding:utf-8 -*-
import
 unittest

from
 UnitTest_1.test_function 
import
 TestFunc

from
 HTMLTestRunner 
import
 HTMLTestRunner

# 在Python3中已经没有 StringIO，所以引用的时候要注意
from
 io 
import
 StringIO


if
 __name__ == 
"__main__"
:
    suite = unittest.TestSuite()
    
# 定义list，按照list里的顺序执行测试用例

    tests = [TestFunc(
"test_add"
),TestFunc(
"test_minus"
),TestFunc(
"test_multi"
),TestFunc(
"test_divide"
)]
    suite.addTests(tests)
    
'''
    runner = unittest.TextTestRunner(verbosity=2)
    runner.run(suite)
    '''
# 生成.txt的测试报告（控制台的输出写入到文件中）
'''
    suite.addTests(unittest.TestLoader().loadTestsFromTestCase(TestFunc))
    with open('UnittestTextReport.txt','a') as f:
        runner = unittest.TextTestRunner(stream=f,verbosity=2)
        runner.run(suite)
    '''
# 生成HTML格式的具体测试报告

    with open(
'HtmlReport.html'
,
'wb'
) 
as
 f:  
# 在python3，要写成'wb' or 'wr'

        runner = HTMLTestRunner(stream=f,title=
'function test report'
,description=
'generated by 
                    HTMLTestRunner'
,verbosity=
2
)
        runner.run(suite)
```

  


