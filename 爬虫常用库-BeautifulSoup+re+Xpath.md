# Python 爬虫常用库



## **Beautiful Soup库入门**

### Beautiful Soup库的基本元素

1. Beautiful Soup库的理解： Beautiful Soup库是解析、遍历、维护“标签树”的功能库，对应一个HTML/XML文档的全部内容
2. BeautifulSoup类的基本元素:
    - Tag 标签，最基本的信息组织单元，分别用`<>`和`</>`标明开头和结尾；
    - Name 标签的名字，`<p>…</p>`的名字是'p'，格式：`<tag>.name`;
    - Attributes 标签的属性，字典形式组织，格式：`<tag>.attrs`;
    - NavigableString 标签内非属性字符串，`<>…</>`中字符串，格式：`<tag>.string`;
    - Comment 标签内字符串的注释部分，一种特殊的Comment类型;

### bs 爬取、解析网页的步骤

1. get() html 网页

```python
# 导入bs4库
from bs4 import BeautifulSoup
import requests # 抓取页面

r = requests.get('https://python123.io/ws/demo.html') # Demo网址
demo = r.text  # 抓取的数据
demo
```

2. 解析 html 页面

```python
# 解析HTML页面
soup = BeautifulSoup(demo, 'html.parser')  # 抓取的页面数据；bs4的解析器
# 有层次感的输出解析后的HTML页面
print(soup.prettify())
```

#### 1）标签，用`soup.`访问获得:

- 当HTML文档中存在多个相同对应内容时，`soup.`返回第一个

```python
soup.a
```

```html
<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>
```

#### 2）标签的名字通过`soup..name`获取，字符串类型

```python
soup.a.name
```

```python
soup.a.parent.name
```

#### 3) 标签的属性,一个可以有0或多个属性，字典类型, `soup..attrs`

```python
tag = soup.a
print(tag.attrs)
print(tag.attrs['class'])
print(type(tag.attrs))
```

```python
{'href': 'http://www.icourse163.org/course/BIT-268001', 'class': ['py1'], 'id': 'link1'}
['py1']
<class 'dict'>
```

#### 4) Attributes: 标签内非属性字符串, 格式：`soup..string`, `NavigableString`可以跨越多个层次

```python
print(soup.a.string)
print(type(soup.a.string))
```

```python
Basic Python
<class 'bs4.element.NavigableString'>
```

#### 5）NavigableString: 标签内字符串的注释部分，Comment是一种特殊类型(有`-->`)

```python
print(type(soup.p.string))
```

```python
<class 'bs4.element.NavigableString'>
```

#### 6) `.prettify()`为HTML文本`<>`及其内容增加更加'\n', 有层次感的输出

```python
print(soup.a.prettify())
```

```html
<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">
 Basic Python
</a>
```

#### 7) bs4库将任何HTML输入都变成utf‐8编码



### 基于bs4库的HTML内容遍历方法

HTML基本格式:`<>…</>`构成了所属关系，形成了标签的树形结构
- 标签树的下行遍历
    * `.contents `子节点的列表，将`<tag>`所有儿子节点存入列表
    * `.children` 子节点的迭代类型，与`.contents`类似，用于循环遍历儿子节点
    * `.descendants` 子孙节点的迭代类型，包含所有子孙节点，用于循环遍历
- 标签树的上行遍
    * `.parent` 节点的父亲标签
    * `.parents` 节点先辈标签的迭代类型，用于循环遍历先辈节点
- 标签树的平行遍历
    * `.next_sibling`返回按照HTML文本顺序的下一个平行节点标签
    * `.previous_sibling`返回按照HTML文本顺序的上一个平行节点标签
    * `.next_siblings` 迭代类型，返回按照HTML文本顺序的后续所有平行节点标签
    * `.previous_siblings` 迭代类型，返回按照HTML文本顺序的前续所有平行节点标签

* 详见：https://www.cnblogs.com/mengxiaoleng/p/11585754.html#_label0 



### 基于bs4库的HTML内容的查找方法[¶](http://localhost:8888/lab#2.1.3-基于bs4库的HTML内容的查找方法)

- `<>.find_all(name, attrs, recursive, string, **kwargs)`
    - 参数：
    - `∙ name `: 对标签名称的检索字符串
    - `∙ attrs`: 对标签属性值的检索字符串，可标注属性检索
    - `∙ recursive`: 是否对子孙全部检索，默认True
    - `∙ string`: `<>…</>`中字符串区域的检索字符串
        - 简写：
        - `(..)` 等价于 `.find_all(..)`
        - `soup(..)` 等价于 `soup.find_all(..)`
- 扩展方法：
    - `<>.find()` 搜索且只返回一个结果，同`.find_all()`参数
    - `<>.find_parents()` 在先辈节点中搜索，返回列表类型，同`.find_all()`参数
    - `<>.find_parent()` 在先辈节点中返回一个结果，同`.find()`参数
    - `<>.find_next_siblings()` 在后续平行节点中搜索，返回列表类型，同`.find_all()`参数
    - `<>.find_next_sibling()` 在后续平行节点中返回一个结果，同`.find()`参数
    - `<>.find_previous_siblings()` 在前序平行节点中搜索，返回列表类型，同`.find_all()`参数
    - `<>.find_previous_sibling()` 在前序平行节点中返回一个结果，同`.find()`参数