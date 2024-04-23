​

目录

收起

一、前言

1、JSON简介

2、模块介绍

二、常用函数

1、json.dumps()

（1）使用示例

（2）Python原始类型向JSON类型转换

（3）其他常用参数说明

2、json.loads()

（1）使用示例

（2）JSON原始类型向Python类型转换

（3）其他常用参数说明

3、json.dump()

（1）使用示例

（2）常用参数说明

4、json.load()

（1）使用示例

（2）常用参数说明

5、json.JSONEncoder()

6、json.JSONDecoder()

一、前言
----

本文介绍如何使用Python处理json文件，以及如何将数据存储为接送文件。

### 1、JSON简介

JSON是（JavaScript Object Notation）的缩写，是一种轻量级的数据交换格式，常被用于Web应用程序中，也被广泛地应用于非Web应用程序中。

### 2、模块介绍

```python
import json
```

Python的json模块是Python官方提供的一个用于解析和生成JSON数据格式的库。 JSON格式的数据由键值对组成，键是字符串，值可以是字符串、数字、布尔值、列表、字典等。

更多参考官方文档：

[Python-JSON模块官方文档](https://docs.python.org/3.11/library/json.html)

二、常用函数
------

### 1、json.dumps()

用于将Python对象序列化为JSON编码字符串。

### （1）使用示例

```python
import json


article = {
    "title": "Python文件操作(一篇就足够了!)",
    "author": "阳光欢子",
    "url": "https://zhuanlan.zhihu.com/p/659529868",
    "testNoneType": None,
    "testTrueType": False
}

json_str = json.dumps(article, ensure_ascii=False, indent=4)
print(json_str)
```

输出：

```text
{
    "title": "Python文件操作(一篇就足够了!)",
    "author": "阳光欢子",
    "url": "https://zhuanlan.zhihu.com/p/659529868",
    "testNoneType": null,
    "testTrueType": false
}
```

### （2）Python原始类型向JSON类型转换

对应关系:

| Python | JSON |
| --- | --- |
| str,unicode | string |
| int,long,float | number |
| True | true |
| False | false |
| None | null |
| dict | object |
| list,tuple | array |

### （3）其他常用参数说明

dumps(obj, _, skipkeys=False, ensure\_ascii=True, check\_circular=True, allow\_nan=True, cls=None, indent=None, separators=None, default=None, sort\_keys=False,_ \*kw)

常用函数参数说明：

| 参数 | 说明 |
| --- | --- |
| skipkeys | 如果为True的话，则只能是字典对象，否则会TypeError错误, 默认False |
| ensure\_ascii | 默认为True，它保证输出每个字符都是ASCII字符。如果有些字符不能编码为ASCII字符，他们会被转义为Unicode转义字符，只有当值为False时，汉字才可以正常显示 |
| indent | 用于控制缩进格式，默认不缩进 |
| cls | 自定义序列化类，用于自定义类的序列化，后面会讲到 |
| separators | 序列化之后的字符串中不同部分的分割符。默认为(',',':') |
| sort\_keys | 用于指定是否按照键进行排序，默认为False不排序 |

### 2、json.loads()

### （1）使用示例

用于将一个JSON编码的字符串解码为Python对象。

```python
import json

json_str = '''
{
    "user": "阳光欢子",
    "links": {
        "zhihu": "https://www.zhihu.com/people/chen-zhi-gao-45-80",
        "jianshu": "https://www.jianshu.com/u/d5e198d8f025"
    }
}
'''

python_object = json.loads(json_str)

print(type(python_object))
print(python_object)
```

输出

```text
<class 'dict'>
{'user': '阳光欢子', 'links': {'zhihu': 'https://www.zhihu.com/people/chen-zhi-gao-45-80', 'jianshu': 'https://www.jianshu.com/u/d5e198d8f025'}}
```

### （2）JSON原始类型向Python类型转换

对应关系：

| JSON | Python |
| --- | --- |
| object | dict |
| array | list |
| string | unicode |
| number(int) | int,long |
| number(float) | float |
| true | True |
| false | False |
| null | None |

### （3）其他常用参数说明

`loads(s, *, cls=None, object_hook=None, parse_float=None, parse_int=None, parse_constant=None, object_pairs_hook=None, **kw)`

常用函数参数说明：

| 参数 | 说明 |
| --- | --- |
| cls | 支持自定义类的解码器，需要继承一个JSONDecoder类并重载（复写）其中的decode方法。默认值为None |
| object\_hook | 支持自定义解码过程中的钩子函数，用于控制解码后生成的Python对象的格式和类型。如果json串是数组，对于JSON串中的每个Python对象，该函数都会被调用一次，后面有使用说明。默认值为None |
| object\_pairs\_hook | 类似object\_hook,处理的是Python对象而不是Python字典 |

### 3、json.dump()

将数据写入到json文件中。

### （1）使用示例

```python
import json


article = {
    "title": "Python文件操作(一篇就足够了!)",
    "author": "阳光欢子",
    "url": "https://zhuanlan.zhihu.com/p/659529868",
    "testNoneType": None,
    "testTrueType": False
}

with open(file='test.json',mode='w') as f:
    json.dump(article, f, ensure_ascii=False, indent=2)
```

输出： 当前目录生成一个json文件。

  

<img src="https://pic4.zhimg.com/v2-0c89e3c49d250fdcc1c0c15186e53957\_b.jpg" data-size="normal" data-rawwidth="825" data-rawheight="262" class="origin\_image zh-lightbox-thumb" width="825" data-original="https://pic4.zhimg.com/v2-0c89e3c49d250fdcc1c0c15186e53957\_r.jpg"/>

![](https://pic4.zhimg.com/v2-0c89e3c49d250fdcc1c0c15186e53957_r.jpg)

img

  

添加图片注释，不超过 140 字（可选）

### （2）常用参数说明

dump(obj, fp, _, skipkeys=False, ensure\_ascii=True, check\_circular=True, allow\_nan=True, cls=None, indent=None, separators=None, default=None, sort\_keys=False,_ \*kw)

常用函数参数说明：

| 参数 | 说明 |
| --- | --- |
| skipkeys | 如果为True的话，则只能是字典对象，否则会TypeError错误, 默认False |
| ensure\_ascii | 默认为True，它保证输出每个字符都是ASCII字符。如果有些字符不能编码为ASCII字符，他们会被转义为Unicode转义字符，只有当值为False时，汉字才可以正常显示 |
| indent | 用于控制缩进格式，默认不缩进 |
| separators | 序列化之后的字符串中不同部分的分割符。默认为(',',':') |
| sort\_keys | 用于指定是否按照键进行排序，默认为False不排序 |

### 4、json.load()

从json文件中读取数据。

### （1）使用示例

使用上面生成文件：

```python
import json

with open(file="test.json", mode='r') as f:
    article = json.load(f)
    print(type(article))
    print(article)
```

输出：

```text
<class 'dict'>
{'title': 'Python文件操作(一篇就足够了!)', 'author': '阳光欢子', 'url': 'https://zhuanlan.zhihu.com/p/659529868', 'testNoneType': None, 'testTrueType': False}
```

### （2）常用参数说明

load(fp, _, cls=None, object\_hook=None, parse\_float=None, parse\_int=None, parse\_constant=None, object\_pairs\_hook=None,_ \*kw)

常用函数参数说明：

| 参数 | 说明 |
| --- | --- |
| cls | 支持自定义类的解码器，需要继承一个JSONDecoder类并重载（复写）其中的default方法。默认值为None |
| object\_hook | 支持自定义解码过程中的钩子函数，用于控制解码后生成的Python对象的格式和类型。对于JSON串中的每个Python对象，该函数都会被调用一次。默认值为None |
| object\_pairs\_hook | 类似object\_hook,处理的是Python对象而不是Python字典 |

### 5、json.JSONEncoder()

自定义json编码，用于将自定义类序列化为json字符串。

步骤：

\- 定义自定义编码器类，继承自json.JSONEncoder类

\- 重写JSONEncoder类的default方法。

使用示例:

```python
import json

class Article():
    def __init__(self, title, author, url):
        self.title = title
        self.author = author
        self.url = url

# 自定义Encoder类
class ArticleEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, Article):
            return {
                'title': obj.title,
                'author': obj.author,
                'url': obj.url
            }
        else:
            return super().default(obj)

article = Article(title='Python文件操作(一篇就足够了!)', author='阳光欢子', url='https://zhuanlan.zhihu.com/p/659529868')
json_str = json.dumps(article, cls=ArticleEncoder, ensure_ascii=False, indent=4)
print(json_str)
```

输出：

```text
{
    "title": "Python文件操作(一篇就足够了!)",
    "author": "阳光欢子",
    "url": "https://zhuanlan.zhihu.com/p/659529868"
}
```

### 6、json.JSONDecoder()

自定义Python解码，将json字符串反序列化微微自定义类对象。

步骤：

\- 自定义反序列化类，继承json.JSONDecoder类

\- 定义函数钩子，将json反序列化的结果转化为对象

使用示例:

```python
import json

class Article():
    def __init__(self, title, author, url):
        self.title = title
        self.author = author
        self.url = url

# 自定义Decoder类
class ArticleDecoder(json.JSONDecoder):
    def __init__(self, *args, **kwargs):
        super().__init__(object_hook=self.dict_to_article, *args, **kwargs)

    def dict_to_article(self, article_dict):
        if 'title' in article_dict and 'author' in article_dict and 'url' in article_dict:
            return Article(article_dict['title'], article_dict['author'], article_dict['url'])
        else:
            return article_dict

json_str = '''
{
    "title": "Python文件操作(一篇就足够了!)",
    "author": "阳光欢子",
    "url": "https://zhuanlan.zhihu.com/p/659529868"
}
'''
article = json.loads(json_str, cls=ArticleDecoder)
print(type(article))
print(article)
```

输出：

```text
json_str = '''
{
    "title": "Python文件操作(一篇就足够了!)",
    "author": "阳光欢子",
    "url": "https://zhuanlan.zhihu.com/p/659529868"
}
'''
<class '__main__.Article'>
<__main__.Article object at 0x10d6c28b0>
```

如果json字符串是一列表的形式，

```text
<class 'list'>
[<__main__.Article object at 0x10b4448b0>]
```

本文转自 <https://zhuanlan.zhihu.com/p/690984705>，如有侵权，请联系删除。