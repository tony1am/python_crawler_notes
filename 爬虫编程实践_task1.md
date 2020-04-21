# 爬虫编程实践

## HTTP

HTTP的请求方法有很多种，主要包括以下几个：

- GET：向指定的资源发出“显示”请求。GET方法应该只用于读取数据，而不应当被用于“副作用”的操作中（例如在Web Application中）。其中一个原因是GET可能会被网络蜘蛛等随意访问。
- HEAD：与GET方法一样，都是向服务器发出直顶资源的请求，只不过服务器将不会出传回资源的内容部分。它的好处在于，使用这个方法可以在不必传输内容的情况下，将获取到其中“关于该资源的信息”（元信息或元数据）。
- POST：向指定资源提交数据，请求服务器进行处理（例如提交表单或者上传文件）。数据被包含在请求文本中。这个请求可能会创建新的资源或修改现有资源，或二者皆有。
- PUT：向指定资源位置上传输最新内容。
- DELETE：请求服务器删除Request-URL所标识的资源，或二者皆有。
- TRACE：回显服务器收到的请求，主要用于测试或诊断。
- OPTIONS：这个方法可使服务器传回该资源所支持的所有HTTP请求方法。用“*”来代表资源名称向Web服务器发送OPTIONS请求，可以测试服务器共能是否正常。
- CONNECT：HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。通常用于SSL加密服务器的连接（经由非加密的HTTP代理服务器）。方法名称是区分大小写的。当某个请求所针对的资源不支持对应的请求方法的时候，服务器应当返回状态码405（Method Not Allowed），当服务器不认识或者不支持对应的请求方法的时候，应当返回状态码501（Not Implemented）。

## 网页基础

### 网页组成

- HTML
- CSS
- JavaScript 

#### 标签

	- 图片用 <img> 标签表示
	
	- 视频用 <video> 标签表示
	- 段落用 <p> 标签表示
	- 布局标签用 <div>标签表示

## 第一个手写网页

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Hello, World!</title>
</head>
<body>
<h1>Hello, World!</h1>
<h1>Hello, Python!</h1>
<h2>设置链接</h2>
<a href="https://www.baidu.com">一个指向百度的链接</a>
</body>
</html>
```

### HTML DOM

根据 W3C 的 HTML DOM 标准，HTML 文档中的所有内容都是节点：

- 整个文档是一个文档节点
- 每个 HTML 元素是元素节点
- HTML 元素内的文本是文本节点
- 每个 HTML 属性是属性节点
- 注释是注释节点

![](node_tree.png)

节点树中的节点彼此拥有层级关系。

父（parent）、子（child）和同胞（sibling）等术语用于描述这些关系。父节点拥有子节点。同级的子节点被称为同胞（兄弟或姐妹）。

- 在节点树中，顶端节点被称为根（root）
- 每个节点都有父节点、除了根（它没有父节点）
- 一个节点可拥有任意数量的子
- 同胞是拥有相同父节点的节点

![](relationship_of_nodes.png)

### CSS

在CSS中，我们使用CSS选择器来定位节点。例如，上例中 div 节点的 id 为 container ，那么就可以表示为 #container ，其中 # 开头代表选择 id ，其后紧跟 id 的名称。另外，如果我们想选择 class 为 wrapper 的节点，便可以使用 .wrapper ，这里以点 . 开头代表选择 class ，其后紧跟 class 的名称。

另外， CSS 选择器还支持嵌套选择，各个选择器之间加上空格分隔开便可以代表嵌套关系，如 #container .wrapper p 则代表先选择 id 为 container 的节点，然后选中其内部的 class 为 wrapper 的节点，然后再进一步选中其内部的 p 节点。另外，如果不加空格，则代表并列关系，如 div#container .wrapper p.text 代表先选择 id 为 container 的 div 节点，然后选中其内部的 class 为 wrapper 的节点，再进一步选中其内部的 class 为 text 的 p 节点。

### 使用开发者工具检查网页

chrome的开发者模式为用户提供了下面几组工具。

- Elements：允许用户从浏览器的角度来观察网页，用户可以借此看到Chrome渲染页面所需要的HTML、CSS和DOM（Document Object Model）对象。
- Network：可以看到网页向服务气请求了哪些资源、资源的大小以及加载资源的相关信息。此外，还可以查看HTTP的请求头、返回内容等。
- Source：即源代码面板，主要用来调试JavaScript。
- Console：即控制台面板，可以显示各种警告与错误信息。在开发期间，可以使用控制台面板记录诊断信息，或者使用它作为shell在页面上与JavaScript交互。
- Performance：使用这个模块可以记录和查看网站生命周期内发生的各种事情来提高页面运行时的性能。
- Memory：这个面板可以提供比Performance更多的信息，比如跟踪内存泄漏。
- Application：检查加载的所有资源。
- Security：即安全面板，可以用来处理证书问题等。

另外，通过切换设备模式可以观察网页在不同设备上的显示效果，快捷键为：Ctrl + Shift + M（或者在 Mac上使用 Cmd + Shift + M），如下图所示。

在“Element”面板中，开发者可以检查和编辑页面的HTML与CSS。选中并双击元素就可以编辑元素了，比如将“python”这几个字去掉，右键该元素，选择“Delete Element”，效果如下图所示：

当然，右击后还有很多操作，值得一提的是快捷菜单中的“Copy XPath”选项。由于XPath是解析网页的利器，因此Chrome中的这个功能对于爬虫程序编写而言就显得十分实用和方便了。

## 开始时动手

一个网络爬虫程序最普遍的过程：

1. 访问站点；
2. 定位所需的信息；
3. 得到并处理信息。

### requests.get

```
import requests
url = 'https://www.python.org/dev/peps/pep-0020/'
res = requests.get(url)
text = res.text
text
```

```
## 爬取python之禅并存入txt文件

with open('zon_of_python.txt', 'w') as f:
    f.write(text[text.find('<pre')+28:text.find('</pre>')-1])
print(text[text.find('<pre')+28:text.find('</pre>')-1])
```

利用python自带的urllib完成以上操作：

```
import urllib
url = 'https://www.python.org/dev/peps/pep-0020/'
res = urllib.request.urlopen(url).read().decode('utf-8')
print(res[res.find('<pre')+28:res.find('</pre>')-1])
```

### requests.post

我们先以金山词霸为例，有道翻译百度翻译谷歌翻译都有加密，以后可以自己尝试。

首先进入金山词霸首页http://www.iciba.com/

然后打开开发者工具下的“Network”，翻译一段话，比如刚刚我们爬到的第一句话“Beautiful is better than ugly.”

点击翻译后可以发现Name下多了一项请求方法是POST的数据，点击Preview可以发现数据中有我们想要的翻译结果。

![](iciba_translate.png)

我们目前需要用到的两部分信息是Request Headers中的User-Agent，和From Data。

接下来我们利用金山词霸来翻译我们刚刚爬出来的python之禅。

```
import requests
def translate(word):
    url="http://fy.iciba.com/ajax.php?a=fy"

    data={
        'f': 'auto',
        't': 'auto',
        'w': word,
    }
    
    headers={
        'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36',
    }#User-Agent会告诉网站服务器，访问者是通过什么工具来请求的，如果是爬虫请求，一般会拒绝，如果是用户浏览器，就会应答。
    response = requests.post(url,data=data,headers=headers)     #发起请求
    json_data=response.json()   #获取json数据
    #print(json_data)
    return json_data
    
def run(word):    
    result = translate(word)['content']['out']   
    print(result)
    return result

def main():
    with open('zon_of_python.txt') as f:
        zh = [run(word) for word in f]

    with open('zon_of_python_zh-CN.txt', 'w') as g:
        for i in zh:
            g.write(i + '\n')
            
if __name__ == '__main__':
    main()
```

### request.get进阶：爬取豆瓣电影

```
# 爬取豆瓣电影 Top 250

import requests
import re
import time
import random

pattern = re.compile("alt=\"(.+)\" src=")
pattern1 = re.compile('src=\"(.+)\" class')

headers={'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.14 Safari/537.36',}

for i in range(10):
    url = f'https://movie.douban.com/top250?start={i * 25}&filter='
    res = requests.get(url, headers=headers)
    text = res.text
    for movie in zip(pattern.findall(text), pattern1.findall(text)):
        print(movie)
    time.sleep(1 + random.random())  # 随机时间间隔，模拟人为点击翻页
```

