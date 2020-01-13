Selenium也是一个用于Web应用程序测试的工具。Selenium测试直接运行在浏览器中，就像真正的用户在操作一样。支持的浏览器包括IE、Mozilla Firefox、Mozilla Suite等。这个工具的主要功能包括：测试与浏览器的兼容性——测试你的应用程序看是否能够很好得工作在不同浏览器和操作系统之上。测试系统功能——创建衰退测试检验软件功能和用户需求。支持自动录制动作和自动生成。Net、Java、Perl等不同语言的测试脚本。Selenium 是ThoughtWorks专门为Web应用程序编写的一个验收测试工具。Selenium基于Javascript 并结合其WebDriver来模拟用户的真实操作，它有很好的处理Ajax的能力，并且支持多种浏览器（Safari，IE，Firefox，Chrome），可以运行在多种操作系统上面。





# selenium的基本用法

**声明浏览器对象**  
上面我们知道了selenium支持很多的浏览器:  
![](/selenium2/import.png)

访问页面

```py
from selenium import webdriver#导入库
browser = webdriver.Chrome()#声明浏览器
url = 'https:www.baidu.com'
browser.get(url)#打开浏览器预设网址
print(browser.page_source)#打印网页源代码
browser.close()#关闭浏览器
```

查找元素

```py
from selenium import webdriver#导入库
browser = webdriver.Chrome()#声明浏览器
url = 'https:www.taobao.com'
browser.get(url)#打开浏览器预设网址
input_first = browser.find_element_by_id('q')
input_two = browser.find_element_by_css_selector('#q')
print(input_first)
print(input_two)
```

这里列举一下常用的查找元素方法：

find\_element\_by\_name

find\_element\_by\_id

find\_element\_by\_xpath

find\_element\_by\_link\_text

find\_element\_by\_partial\_link\_text

find\_element\_by\_tag\_name

find\_element\_by\_class\_name

find\_element\_by\_css\_selector

下面这种方式是比较通用的一种方式：这里需要记住By模块所以需要导入

from selenium.webdriver.common.by import By

**获取元素属性**

get\_attribute\('class'\)

**获取ID，位置，标签名**

id

location

tag\_name

size

========================================================================================================























