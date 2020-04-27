# Python爬虫编程实践之爬取腾讯新闻

### 总体思路

腾讯新闻热点精选属于动态加载内容，使用ajax技术。故单单通过get的方式获得html网页所能得到的新闻标题列表有限。这个时候就必须考虑模拟浏览操作了。具体步骤如下：

1. 使用selenium 的 webdriver 模块控制chrome来打开网页
2. 循环执行窗口滚动脚本
3. 获取网页源码
4. 解析获得的网页源码，提取需要的信息

```python
# 导库
import time
import random
from selenium import webdriver
```

### 使用selenium 的 webdriver 模块控制chrome来打开网页

```python
driver = webdriver.Chrome(executable_path="../chromedriver.exe")
# 控制浏览器打开url
driver.get("https://news.qq.com")
```

### 执行窗口滚动脚本

```python
for i in range(1,100):
    time.sleep(random.random() + 2)  # 随机间隔
    driver.execute_script("window.scrollTo(window.scrollX, %d);"%(i*200))  # 执行滚动窗口脚本
```

### 获取网页源码

```python
from bs4 import BeautifulSoup

html = driver.page_source
bsObj=BeautifulSoup(html,"lxml")
```

### 解析获得的网页源码

```python
# 通过"jx-tit"定位"热点精选" ->平行遍历下一个节点，即list容器 -> find_all() 找到列表每一行的标题
jxtits = bsObj.find_all("div",{"class":"jx-tit"})[0].find_next_sibling().find_all("li")

# 打印结果
print("index, title, url")
for i,jxtit in enumerate(jxtits):
#     print(jxtit)
    
    try:
        text=jxtit.find_all("img")[0]["alt"]
    except:
        text=jxtit.find_all("div",{"class":"lazyload-placeholder"})[0].text
    try:
        url=jxtit.find_all("a")[0]["href"]
    except:
        print(jxtit)
    print(f'{i+1}, {text}, {url}) 
```

```python
# 写入 csv文件
with open('txNews.txt', 'w') as fp:
    fp.write("index, title, url\n")
    
    for i, jxtit in enumerate(jxtits):
        try:
            text=jxtit.find_all("img")[0]["alt"]
        except:
            text=jxtit.find_all("div",{"class":"lazyload-placeholder"})[0].text
        try:
            url=jxtit.find_all("a")[0]["href"]
        except:
            print(jxtit)
        fp.write(', '.join([str(i+1), text, url]) + '\n')
```

```python
# 关闭窗口
driver.quit()
```

