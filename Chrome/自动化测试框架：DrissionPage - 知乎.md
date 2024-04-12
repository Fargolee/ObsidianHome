---
page-title: "自动化测试框架：DrissionPage - 知乎"
url: https://zhuanlan.zhihu.com/p/669899996
date: "2024-04-12 22:17:56"
---
如果只要控制浏览器，导入`ChromiumPage`：

```
from DrissionPage import ChromiumPage
```

如果只要收发数据包，导入`SessionPage`：

```
from DrissionPage import SessionPage
```

1、定位元素

```
from DrissionPage import ChromiumPage
 
# 创建页面对象，并启动或接管浏览器
page = ChromiumPage()
# 跳转到登录页面
page.get('https://gitee.com/login')    
# get()方法用于访问参数中的网址。它会等待页面完全加载，再继续执行后面的代码。
 
# 定位到账号文本框，获取文本框元素
ele = page.ele('#user_login')    
# ele()方法用于查找元素，它返回一个ChromiumElement对象，用于操作元素。
'#user_login'是定位符文本，#意思是按id属性查找元素。
ele()内置了等待，如果元素未加载，它会执行等待，直到元素出现或到达时限。默认超时时间 10 秒。
 
# 输入对文本框输入账号
ele.input('您的账号')
# 定位到密码文本框并输入密码
page.ele('#user_password').input('您的密码')
# 点击登录按钮
page.ele('@value=登 录').click()    # @表示按属性名查找
```

2、爬取网页

```
from DrissionPage import SessionPage
 
# 创建页面对象
page = SessionPage()
 
# 爬取3页
for i in range(1, 4):
    # 访问某一页的网页
    page.get(f'https://gitee.com/explore/all?page={i}')
    # 获取所有开源库<a>元素列表
    links = page.eles('.title project-namespace-path')    
    # 页面对象的eles()获取页面中所有class属性为'title project-namespace-path'的元素对象，
    #eles()方法用于查找多个符合条件的元素，返回由它们组成的list
    # 遍历所有<a>元素
    for link in links:
        # 打印链接信息
        print(link.text, link.link)
        # .text获取元素的文本，.link获取元素的href或src属性
```

3.下载网页

```
from DrissionPage import SessionPage 
 
url = 'https://www.baidu.com/img/flexible/logo/pc/result.png'
save_path = r'C:\download'    # 保存的路径
 
page = SessionPage()
page.download(url, save_path, 'img')  # 支持重命名，处理文件名冲突
```

4.元素查找

```
# 根据属性查找，@ 后面可跟任意属性
page.ele('@id:ele_id', timeout=2)  # 查找 id 为 ele_id 的元素，设置等待时间2秒  
page.eles('@class')  # 查找所有拥有 class 属性的元素
page.eles('@class:class_name')  # 查找所有 class 含有 ele_class 的元素 
page.eles('@class=class_name')  # 查找所有 class 等于 ele_class 的元素 
 
# 根据 class 或 id 查找
page.ele('#ele_id')  # 等价于 page.ele('@id=ele_id')
page.ele('#:ele_id')  # 等价于 page.ele('@id:ele_id')
page.ele('.ele_class')  # 等价于 page.ele('@class=ele_class')
page.ele('.:ele_class')  # 等价于 page.ele('@class:ele_class')
 
# 根据 tag name 查找
page.ele('tag:li')  # 查找第一个 li 元素  
page.eles('tag:li')  # 查找所有 li 元素  
 
# 根据 tag name 及属性查找
page.ele('tag:div@class=div_class')  # 查找 class 为 div_class 的 div 元素
page.ele('tag:div@class:ele_class') # 查找 class 含有 ele_class 的 div 元素
page.ele('tag:div@class=ele_class') # 查找 class 等于 ele_class 的 div 元素
page.ele('tag:div@text():search_text') # 查找文本含有 search_text 的 div 元素
page.ele('tag:div@text()=search_text') # 查找文本等于 search_text 的 div 元素
 
# 根据文本内容查找
page.ele('search text')  # 查找包含传入文本的元素  
page.eles('text:search text')  # 如文本以 @、tag:、css:、xpath:、text: 开头，则应在前加上 text: 避免冲突  
page.eles('text=search text')  # 文本等于 search_text 的元素
 
# 根据 xpath 或 css selector 查找
page.eles('xpath://div[@class="ele_class"]')  
page.eles('css:div.ele_class')  
 
# 根据 loc 查找
loc1 = By.ID, 'ele_id'
loc2 = By.XPATH, '//div[@class="ele_class"]'
page.ele(loc1)
page.ele(loc2)
 
# 查找下级元素
element = page.ele('@id:ele_id')
element.ele('@class:class_name')  # 在 element 下级查找第一个 class 为 ele_class 的元素
element.eles('tag:li')  # 在 ele_id 下级查找所有li元素
 
# 根据位置查找
element.parent  # 父元素  
element.next  # 下一个兄弟元素  
element.prev  # 上一个兄弟元素  
 
# 获取 shadow-root，把它作为元素对待。只支持 open 的 shadow-root
ele1 = element.shadow_root.ele('tag:div')
 
# 串连查找
page.ele('@id:ele_id').ele('tag:div').next.ele('some text').eles('tag:a')
 
# 简化写法
eles = page('@id:ele_id')('tag:div').next('some text').eles('tag:a')
ele2 = ele1('tag:li').next('some text')
```

5.元素操作

```
element.click(by_js)  # 点击元素，可选择是否用 js 方式点击
element.input(value)  # 输入文本
element.run_script(js)  # 对元素运行 JavaScript 脚本
element.submit()  # 提交
element.clear()  # 清空元素
element.screenshot(path, filename)  # 对元素截图(是不是可以截取验证码？？？)
element.select(text)  # 根据文本选择下拉列表
element.set_attr(attr, value)  # 设置元素属性值
element.remove_attr(attr)  # 删除属性
element.drag(x, y, speed, shake)  # 拖动元素相对距离，可设置速度和是否随机抖动
element.drag_to(ele_or_loc, speed, shake)  # 拖动元素到另一个元素或某个坐标，可设置速度和是否随机抖动
element.hover()  # 在元素上悬停鼠标
```

6.获取元素属性

```
element.html  # 返回元素 outerHTML
element.inner_html  # 返回元素 innerHTML
element.tag  # 返回元素 tag name
element.text  # 返回元素 innerText 值
element.comments  # 返回元素内注释列表
element.link  # 返回元素 href 或 src 绝对 url
element.texts()  # 返回元素内所有直接子节点的文本，包括元素和文本节点，可指定只返回文本节点
element.attrs  # 返回元素所有属性的字典
element.attr(attr)  # 返回元素指定属性的值
element.css_path  # 返回元素绝对 css 路径
element.xpath  # 返回元素绝对 xpath 路径
element.parent  # 返回元素父元素
element.next  # 返回元素后一个兄弟元素
element.prev  # 返回元素前一个兄弟元素
element.parents(num)  # 返回第 num 级父元素
element.nexts(num, mode)  # 返回后面第几个元素或节点
element.prevs(num, mode)  # 返回前面第几个元素或节点
element.ele(loc_or_str, timeout)  # 返回当前元素下级第一个符合条件的子元素、属性或节点文本
element.eles(loc_or_str, timeout)  # 返回当前元素下级所有符合条件的子元素、属性或节点文本
```

## 1、SessionPage

1）SessionPage传递控制权

当须要使用多个页面对象共同操作一个页面时，可在页面对象创建时接收另一个页面间对象传递过来的Session对象，以达到多个页面对象同时使用一个Session对象的效果。

```
# 创建一个页面
page1 = SessionPage()
# 获取页面对象内置的Session对象
session = page1.session
# 在第二个页面对象初始化时传递该对象
page2 = SessionPage(session_or_options=session)
```

2）SessionPage与requests的一些对比

SessionPage基于 requests 进行网络连接，因此可使用 requests 内置的所有请求方式，包括get()、post()、head()、options()、put() 、patch()、delete()。

get()方法语法与 requests 的get()方法一致，在此基础上增加了连接失败重试功能。与 requests 不一样的是，它不返回Response对象。

SessionPage目前只对get()和post()做了封装和优化，其余方式可通过调用页面对象内置的Session对象使用。

```
from DrissionPage import SessionPage
 
page = SessionPage()
# 获取内置的 Session 对象
session = page.session
# 以 head 方式发送请求
response = session.head('https://www.baidu.com')
print(response.headers)
```

## 2、ChormiumPage

1）传递控制权  
当须要使用多个页面对象共同操作一个页面时，可在对象间传递驱动器。  
这可以实现多个页面对象共同控制一个浏览器。

```
# 创建一个页面
page1 = ChormiumPage()
# 获取页面对象的浏览器控制器
driver = page1.driver
# 把控制器对象在第二个页面对象初始化时传递进去
page2 = ChormiumPage(driver_or_options=driver)
```

2）页面截图

```
# 对整页截图并保存
page.get_screenshot(path='D:\\page.png', full_page=True)
```

path：保存图片的完整路径，文件后缀可选'jpg'、'jpeg'、'png'、'webp'，为None时以 jpg 格式保存在当前文件夹

full\_page：是否整页截图，为True截取整个网页，为False截取可视窗口

3）页面跳转

```
page.back(2)  # 后退两个网页
page.forward(2)  # 前进两步
page.refresh()  # 刷新页面
```

4）执行脚本或命令

```
# 用传入参数的方式执行 js 脚本显示弹出框显示 Hello world!
page.run_js('alert(arguments[0]+arguments[1]);', 'Hello', ' world!')
```

5）窗口管理

```
page.set.window.maximized()    # 窗口最大化
 
page.set.window.minimized()    # 窗口最小化
 
page.set.window.fullscreen()    # 用于使窗口切换到全屏模式
 
page.set.window.normal()    # 用于使窗口切换到普通模式
 
page.set.window.size(500, 500)    # 用于设置窗口大小。
 
page.set.window.location(500, 500)    # 用于设置窗口位置
 
```

6）滚动页面

```
page.scroll.to_top()    # 用于滚动页面到顶部，水平位置不变
 
page.scroll.to_bottom()    # 用于滚动页面到底部，水平位置不变
 
page.scroll.to_half()    # 用于滚动页面到垂直中间位置，水平位置不变
 
page.scroll.to_rightmost()    # 用于滚动页面到最右边，垂直位置不变
 
page.scroll.to_leftmost()    # 用于滚动页面到最左边，垂直位置不变
 
page.scroll.to_location(300, 50)    # 用于滚动页面到滚动到指定位置
 
page.scroll.up(30)    # 用于使页面向上滚动若干像素，水平位置不变
 
page.scroll.down(30)    # 用于使页面向下滚动若干像素，水平位置不变
 
page.scroll.right(30)    # 用于使页面向右滚动若干像素，垂直位置不变
 
page.scroll.left(30)    # 用于使页面向左滚动若干像素，垂直位置不变

scroll.to_see()，用于滚动页面直到元素可见。
# 滚动到某个已获取到的元素
ele = page.ele('tag:div')
page.scroll.to_see(ele)
 
# 滚动到按定位符查找到的元素
page.scroll.to_see('tag:div')
```

7）处理弹出消息

handle\_alert()  
此方法 用于处理提示框。  
它能够设置等待时间，等待提示框出现才进行处理，若超时没等到提示框，返回False。  
也可只获取提示框文本而不处理提示框。

```
# 确认提示框并获取提示框文本
txt = page.handle_alert()
 
# 点击取消
page.handle_alert(accept=False)
 
# 给 prompt 提示框输入文本并点击确定
paeg.handle_alert(accept=True, send='some text')
 
# 不处理提示框，只获取提示框文本
txt = page.handle_alert(accept=None)
```

8）关闭浏览器