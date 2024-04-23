---
page-title: "python 爬虫爬取读万卷小说网站小说 - 『编程语言区』 - 吾爱破解 - LCG - LSG |安卓破解|病毒分析|www.52pojie.cn"
url: https://www.52pojie.cn/thread-1903989-1-1.html
date: "2024-04-11 20:47:28"
---
[![](https://avatar.52pojie.cn/images/noavatar_middle.gif)](https://www.52pojie.cn/home.php?mod=space&uid=1381015)

*[锋芒初露](https://www.52pojie.cn/home.php?mod=spacecp&ac=usergroup&gid=10)*

![Rank: 1](https://static.52pojie.cn/static/image/common/star_level1.gif)

精华

0

威望

0 点

热心值

3 点

悬赏值

0 点

听众

[1](https://www.52pojie.cn/home.php?mod=follow&do=follower&uid=1381015)

贡献值

0 点

吾爱币

127 CB

违规

0 次

注册时间

2020-3-13

-   [收听TA](https://www.52pojie.cn/home.php?mod=spacecp&ac=follow&op=add&hash=81853efd&fuid=1381015 "收听TA")

*本帖最后由 Mly2580 于 2024-3-21 16:00 编辑*

之前看到吾爱python爬虫一位大佬写了一个爬取笔趣阁的代码，之后最近也有了点兴趣，于是自己尝试爬了一下读万卷的小说网站，并且打包了一个exe程序可直接执行。原贴链接：https://www.52pojie.cn/thread-1894044-1-1.html，感谢@chenmuting[我用夸克网盘分享了「读万卷.exe」，点击链接即可保存。打开「夸克APP」，无需下载在线播放视频，畅享原画5倍速，支持电视投屏。 链接：https://pan.quark.cn/s/df492af360ed 提取码：xZAm](https://www.52pojie.cn/%E6%88%91%E7%94%A8%E5%A4%B8%E5%85%8B%E7%BD%91%E7%9B%98%E5%88%86%E4%BA%AB%E4%BA%86%E3%80%8C%E8%AF%BB%E4%B8%87%E5%8D%B7.exe%E3%80%8D%EF%BC%8C%E7%82%B9%E5%87%BB%E9%93%BE%E6%8E%A5%E5%8D%B3%E5%8F%AF%E4%BF%9D%E5%AD%98%E3%80%82%E6%89%93%E5%BC%80%E3%80%8C%E5%A4%B8%E5%85%8BAPP%E3%80%8D%EF%BC%8C%E6%97%A0%E9%9C%80%E4%B8%8B%E8%BD%BD%E5%9C%A8%E7%BA%BF%E6%92%AD%E6%94%BE%E8%A7%86%E9%A2%91%EF%BC%8C%E7%95%85%E4%BA%AB%E5%8E%9F%E7%94%BB5%E5%80%8D%E9%80%9F%EF%BC%8C%E6%94%AF%E6%8C%81%E7%94%B5%E8%A7%86%E6%8A%95%E5%B1%8F%E3%80%82%20%E9%93%BE%E6%8E%A5%EF%BC%9Ahttps://pan.quark.cn/s/df492af360ed%20%E6%8F%90%E5%8F%96%E7%A0%81%EF%BC%9AxZAm) 代码如下：

from selenium import webdriver  
import requests, re, os, time, shutil, threading, queue  
from lxml import etree  
import pandas as pd  
import randomdef get\_user\_agent():  
    headers\_list = \[  
        "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; AcooBrowser; .NET CLR 1.1.4322; .NET CLR 2.0.50727)",  
        "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0; Acoo Browser; SLCC1; .NET CLR 2.0.50727; Media Center PC 5.0; .NET CLR 3.0.04506)",  
        "Mozilla/4.0 (compatible; MSIE 7.0; AOL 9.5; AOLBuild 4337.35; Windows NT 5.1; .NET CLR 1.1.4322; .NET CLR 2.0.50727)",  
        "Mozilla/5.0 (Windows; U; MSIE 9.0; Windows NT 9.0; en-US)",  
        "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0; .NET CLR 3.5.30729; .NET CLR 3.0.30729; .NET CLR 2.0.50727; Media Center PC 6.0)",  
        "Mozilla/5.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0; WOW64; Trident/4.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; .NET CLR 1.0.3705; .NET CLR 1.1.4322)",  
        "Mozilla/4.0 (compatible; MSIE 7.0b; Windows NT 5.2; .NET CLR 1.1.4322; .NET CLR 2.0.50727; InfoPath.2; .NET CLR 3.0.04506.30)",  
        "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN) AppleWebKit/523.15 (KHTML, like Gecko, Safari/419.3) Arora/0.3 (Change: 287 c9dfb30)",  
        "Mozilla/5.0 (X11; U; Linux; en-US) AppleWebKit/527+ (KHTML, like Gecko, Safari/419.3) Arora/0.6",  
        "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.8.1.2pre) Gecko/20070215 K-Ninja/2.1.1",  
        "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.9) Gecko/20080705 Firefox/3.0 Kapiko/3.0",  
        "Mozilla/5.0 (X11; Linux i686; U;) Gecko/20070322 Kazehakase/0.4.5",  
        "Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.9.0.8) Gecko Fedora/1.9.0.8-1.fc10 Kazehakase/0.5.6",  
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.56 Safari/535.11",  
        "Mozilla/5.0 (Macintosh; Intel Mac OS X 10\_7\_3) AppleWebKit/535.20 (KHTML, like Gecko) Chrome/19.0.1036.7 Safari/535.20",  
        "Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; fr) Presto/2.9.168 Version/11.52",  
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/536.11 (KHTML, like Gecko) Chrome/20.0.1132.11 TaoBrowser/2.0 Safari/536.11",  
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.71 Safari/537.1 LBBROWSER",  
        "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident/5.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E; LBBROWSER)",  
        "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; QQDownload 732; .NET4.0C; .NET4.0E; LBBROWSER)",  
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.84 Safari/535.11 LBBROWSER",  
        "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.1; WOW64; Trident/5.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E)",  
        "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident/5.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E; QQBrowser/7.0.3698.400)",  
        "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; QQDownload 732; .NET4.0C; .NET4.0E)",  
        "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; SV1; QQDownload 732; .NET4.0C; .NET4.0E; 360SE)",  
        "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; QQDownload 732; .NET4.0C; .NET4.0E)",  
        "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.1; WOW64; Trident/5.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E)",  
        "Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.89 Safari/537.1",  
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.89 Safari/537.1",  
        "Mozilla/5.0 (iPad; U; CPU OS 4\_2\_1 like Mac OS X; zh-cn) AppleWebKit/533.17.9 (KHTML, like Gecko) Version/5.0.2 Mobile/8C148 Safari/6533.18.5",  
        "Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:2.0b13pre) Gecko/20110307 Firefox/4.0b13pre",  
        "Mozilla/5.0 (X11; Ubuntu; Linux x86\_64; rv:16.0) Gecko/20100101 Firefox/16.0",  
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11",  
        "Mozilla/5.0 (X11; U; Linux x86\_64; zh-CN; rv:1.9.2.10) Gecko/20100922 Ubuntu/10.10 (maverick) Firefox/3.6.10",  
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36",  
    \]  
    return headers\_list  
'''def get\_proxy():  
    proxy = \[        'http://182.140.244.163:8118',        'http://113.124.86.180:9999',       'http://117.64.237.42:9999',        'http://182.34.102.48:9999',        'http://183.236.123.242:8060',        'http://27.192.203.80:9000',        'http://114.231.8.242:8888',        'http://36.134.91.82:8888',        'http://222.132.57.105:9000',        'http://61.216.156.222:60808',        'http://182.34.20.110:9999',        'http://60.205.132.71:80',    \]    return proxy/'''  
headers = {  
    'user-agent': random.choice(get\_user\_agent()),  
}  
'''proxy = {  
   'http': random.choice(get\_proxy()),}/'''  
  
def extract\_link\_suffix(url):  
    \# 查找最后一个斜杠的位置  
    last\_slash\_index = url.rfind('/')  
    if last\_slash\_index != -1:  
        \# 提取斜杠之后的部分作为后缀  
        return url\[last\_slash\_index + 1:\]  
    else:  
        \# 如果没有斜杠，则直接返回整个URL（这种情况可能很少见）  
        return url\# 搜索小说，并选择所需要下载的小说  
def search\_novel():  
    chrome\_options = webdriver.ChromeOptions()  
    #后台静默运行  
    chrome\_options.add\_argument('--headless')  
    print('浏览器已打开')  
    browser = webdriver.Chrome(options\=chrome\_options)  
    #browser = webdriver.Chrome()  
    name\_input = input('输入小说名或作者：')  
    browser.get(f'http://www.duwanjuan.info/modules/article/search.php?q={name\_input}')  
    time.sleep(6)  
    \# 输出网页源代码  
    html = browser.page\_source  
    browser.close()  
    \# print('浏览器已关闭')  
    html = etree.HTML(html)  
    name = html.xpath("//div\[@id='jieqi\_page\_contents'\]/div\[@class='c\_row'\]/div/div/span\[@class='c\_subject'\]/a/text()")\[:10\]  
    chapter = html.xpath("//div\[@class='c\_tag'\]/span\[@class='c\_value'\]/a/text()")\[:10\]  
    link = html.xpath("//div\[@id='jieqi\_page\_contents'\]/div\[@class='c\_row'\]/div/div/a/@href")\[:10\]  
    \# 提取每个链接的后缀部分  
    link\_suffixes = \[extract\_link\_suffix(l) for l in link\]  
    author = html.xpath("//div\[@class='c\_tag'\]/span\[contains(text(), '作者：')\]/following-sibling::span\[1\]/text()")\[:10\]  
    num = \[i + 1 for i in range(0, len(name))\]  
    data = {'序号': num, '小说': name, '作者': author,'最新章节':chapter,'链接':link\_suffixes}  
    df = pd.DataFrame(data)  
    if df.empty:  
        print('搜索数据为空，请重新搜索')  
        search\_novel()  
    else:  
        print(df)  
        sx\_input = int(input('请输入序号选择下载的小说：'))  
        novel\_link = link\[sx\_input - 1\]  
        return novel\_link\# 定义一个函数来获取小说章节目录的URL和章节名  
def get\_chapter\_urls(url, visited\_urls, value):  
    global tot\_title  
    global book\_name  
    response = requests.get(url, headers\=headers)  
    response.encoding = response.apparent\_encoding  
    html = etree.HTML(response.text)  
    chapter\_elements = html.xpath("//div\[@class='index'\]//li\[@class='chapter'\]/a")  
    chapter\_elements.pop(10)  
    tot\_title = html.xpath("//div\[@class='index'\]//li\[@class='chapter'\]/a/text()")  
    bk = html.xpath("//div\[@class='main'\]/div\[@class='headlink cf'\]/h1/text()\[1\]")  
    \# 从列表中提取字符串  
    if bk:  \# 确保bk不为空  
        text = bk\[0\]  \# 提取列表中的第一个元素  
    else:  
        text = ""  \# 如果bk为空，则设置text为空字符串  
  
    \# 正则表达式，匹配方括号及其内容，但使用括号捕获括号内的内容  
    pattern = r"\\\['(.\*?)'\\\]"  
    \# 使用re.search来查找匹配项，如果找到，则提取捕获组中的内容  
    match = re.search(pattern, text)  
    if match:  
        book\_name = match.group(1)  \# 提取捕获组中的内容  
    else:  
        book\_name = text  \# 如果没有找到匹配项，则保留原始text值  
    chapter\_urls = \[\]  
    for element in chapter\_elements:  
        chapter\_name = element.text  
        chapter\_url = element.get('href')  
        if chapter\_url not in visited\_urls:  
            value += 1  
            chapter\_urls.append((chapter\_name, chapter\_url, value))  
            visited\_urls.add(chapter\_url)  
    return chapter\_urls\# 定义一个函数来获取小说具体章节的内容  
def get\_chapter\_content(url):  
try:  
    response = requests.get(url, headers\=headers,verify\=False,timeout\=15)  
    response.encoding = response.apparent\_encoding  
    html = etree.HTML(response.text)  
    content\_element = html.xpath("//div\[@id='acontent'\]/text()")  
    pattern = r'\\r\\n     \\xa0\\xa0\\xa0\\xa0|\\s|\\(|\\)|\\读万卷 www.duwanjuan.info'    content = \[re.sub(pattern, '', sub\_text) for sub\_text in content\_element\]  
    return content  
except requests.RequestException as e:  
     print(f"Error occurred while fetching content from {url}: {e}")  
     return \[\]\# 定义一个函数来处理每个章节的爬取任务  
def process\_chapter(chapter\_queue):  
    global time\_start  
    time\_start = time.time()  
    while not chapter\_queue.empty():  
        chapter\_name, chapter\_url, value = chapter\_queue.get()  
        print("正在爬取章节：", chapter\_name)  
        try:  
            content = get\_chapter\_content(chapter\_url)  
        except Exception as e:  
            print(f"获取章节内容失败：{e}")  
            content = \[\]  
        \# 在这里可以将内容保存到文件或进行其他处理  
        folder\_path = f'{book\_name}'  
        if not os.path.exists(folder\_path):  
            os.makedirs(folder\_path)  
        with open(f'{book\_name}/{value}.txt', 'w', encoding\='utf-8') as f:  
            f.write('\\n' \+ chapter\_name + '\\n')  
            for data in content:  
                f.write(data + '\\n')  
            f.write('\\n\\n')  
        chapter\_queue.task\_done()  
        time.sleep(6)\# 合并下载的TXT文件  
def merge\_txt\_files(folder\_path, output\_file):  
    txt\_files = \[f for f in os.listdir(folder\_path) if f.endswith('.txt')\]  
    txt\_files.sort(key\=lambda x: int(x\[:-4\]))with open(output\_file, 'w', encoding\='utf-8') as outfile:  
        for txt\_file in txt\_files:  
            with open(os.path.join(folder\_path, txt\_file), 'r', encoding\='utf-8') as infile:  
                content = infile.read()  
                outfile.write(content)def search\_continue():  
    input\_continue = input('请输入y/n选择是否继续下载小说：')  
    if input\_continue == 'y':  
        main()  
    else:  
        return  
  
def main():  
    directory\_url = search\_novel()  
    \# 获取小说章节目录的URL和章节名  
    visited\_urls = set()  
    value = 0  
    chapter\_urls = get\_chapter\_urls(directory\_url, visited\_urls, value)  
    \# 创建一个队列来存储待爬取的章节信息  
    chapter\_queue = queue.Queue()  
    for chapter\_name, chapter\_url, value in chapter\_urls:

        chapter\_queue.put((chapter\_name

, chapter\_url, value))  
    \# 创建多个线程来并发爬取章节内容  
    print('=' \* 64)  
    print('线程数建议在10-30之间，避免对目标服务器造成过大压力')  
    sum = int(input('输入线程数：'))  
    num\_threads = sum  \# 设置线程数量，根据需要进行调整  
    threads = \[\]  
    for i in range(num\_threads):  
        thread = threading.Thread(target\=process\_chapter, args\=(chapter\_queue,))  
        thread.daemon = False  
        thread.start()  
        threads.append(thread)  
    \# 等待所有线程完成任务  
    chapter\_queue.join()  
    \# 等待所有线程结束  
    for thread in threads:  
        thread.join()  
    print("所有章节爬取完成！")  
    time\_end = time.time()  
    print('章节爬取花费时间：', time\_end - time\_start)  
    print('=' \* 64)  
    print('开始合并所有TXT文件')  
    folder\_path\_1 = f'{book\_name}/'  \# 请替换为实际文件夹路径  
    output\_file = f'{book\_name}.txt'  \# 输出文件名  
    merge\_txt\_files(folder\_path\_1, output\_file)  
    print('合并所有TXT文件成功')  
    print(f'{book\_name}下载成功')  
    shutil.rmtree(book\_name)  
    print('=' \* 64)  
    search\_continue()\# 主程序入口  
if \_\_name\_\_ == "\_\_main\_\_":

    main()

### 免费评分

[参与人数 2](https://www.52pojie.cn/forum.php?mod=misc&action=viewratings&tid=1903989&pid=49846888 "查看全部评分")

吾爱币 *+2*

热心值 *+2*

收起 *理由*

[![](https://avatar.52pojie.cn/data/avatar/001/78/23/55_avatar_small.jpg)](https://www.52pojie.cn/home.php?mod=space&uid=1782355) [xlln](https://www.52pojie.cn/home.php?mod=space&uid=1782355)

\+ 1

\+ 1

我很赞同！

[![](https://avatar.52pojie.cn/data/avatar/002/22/44/16_avatar_small.jpg)](https://www.52pojie.cn/home.php?mod=space&uid=2224416) [rookie12138](https://www.52pojie.cn/home.php?mod=space&uid=2224416)

\+ 1

\+ 1

热心回复！

[查看全部评分](https://www.52pojie.cn/forum.php?mod=misc&action=viewratings&tid=1903989&pid=49846888 "查看全部评分")