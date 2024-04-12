---
page-title: "整合，python多线程批量下载网页内的图片 - 『编程语言区』 - 吾爱破解 - LCG - LSG |安卓破解|病毒分析|www.52pojie.cn"
url: https://www.52pojie.cn/thread-1626489-1-2.html
date: "2024-04-11 20:53:18"
---
01

02

03

04

05

06

07

08

09

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55

56

57

58

59

60

61

62

63

64

65

66

67

68

69

70

71

72

73

74

75

76

77

78

79

80

81

82

83

84

85

86

87

88

89

90

`import` `re`

`import` `requests`

`from` `threading` `import` `Thread`

`from` `queue` `import` `Queue`

`import` `time`

`from` `bs4` `import` `BeautifulSoup as bsp`

`q` `=` `Queue(``100000``)`

`class` `FastRequests:`

    `def` `__init__(`

            `self``,threads``=``20``,headers``=``{`

            `'User-Agent'``:``'Mozilla/5.0 (X11; Linux aarch64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.188 Safari/537.36 CrKey/1.54.250320 Edg/99.0.4844.74'``,`

            `'Cookie'``:''`

        `}`

    `):`

        `self``.threads` `=` `threads`

        `self``.headres` `=` `headers`

        `self``.imgUrl` `=` `r``'https://www.tuba555.net/m/htm9/43816.html'`

    `def` `getImg(``self``):`

        `eleGet``=``requests.get(url``=``self``.imgUrl, headers``=``{``"User-Agent"``:` `"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:86.0) Gecko/20100101 Firefox/86.0"``})`

        `soup``=``bsp(eleGet.content.decode(``'gb2312'``),` `'lxml'``)`

        `imgs``=``soup.find_all(``'img'``)`

        `for` `i` `in` `imgs:`

            `i``=``str``(i)`

            `if` `"tupian_img"` `in` `i:`

                `kaishi``=` `i.find(``"src"``)``+``5`

                `jieshu``=` `i.find(``"jpg"``)``+``3`

                `img``=` `i[kaishi:jieshu]`

                `q.put(img)`

    `def` `run(``self``):`

        `for` `i` `in` `range``(``self``.threads):`

            `t` `=` `Consumer(``self``.headres)`

            `t.start()`

`class` `Consumer(Thread):`

    `def` `__init__(``self``,headers):`

        `Thread.__init__(``self``)`

        `self``.headers` `=` `headers`

        `self``.size` `=` `0`

        `self``.time` `=` `0`

    `def` `run(``self``):`

        `while` `True``:`

            `if` `q.qsize()` `=``=` `0``:`

                `break`

            `self``.download(q.get())`

    `def` `validateTitle(``self``,title):`

        `rstr` `=` `r``"[\/\\\:\*\?\"\<\>\|]"` 

        `new_title` `=` `re.sub(rstr,` `"_"``, title)` 

        `return` `new_title`

    `def` `sizeFormat(``self``,size, is_disk``=``False``, precision``=``2``):`

        `formats` `=` `[``'KB'``,` `'MB'``,` `'GB'``,` `'TB'``,` `'PB'``,` `'EB'``,` `'ZB'``,` `'YB'``]`

        `unit` `=` `1000.0` `if` `is_disk` `else` `1024.0`

        `if` `not` `(``isinstance``(size,` `float``)` `or` `isinstance``(size,` `int``)):`

            `raise` `TypeError(``'a float number or an integer number is required!'``)`

        `if` `size <` `0``:`

            `raise` `ValueError(``'number must be non-negative'``)`

        `for` `i` `in` `formats:`

            `size` `/``=` `unit`

            `if` `size < unit:`

                `return` `f``'{round(size, precision)}{i}'`

        `return` `f``'{round(size, precision)}{i}'`

    `def` `download(``self``,info):`

        `title` `=` `info.split(r``'/'``)[``-``1``]`

        `link` `=` `info`

        `if` `title` `=``=` `'':`

            `title` `=` `self``.validateTitle(link.split(``'/'``)[``-``1``])`

        `start_time` `=` `time.time()`

        `response` `=` `requests.get(url``=``link, headers``=``self``.headers, stream``=``True``).content`

        `end_time` `=` `time.time()`

        `self``.time` `=` `end_time` `-` `start_time`

        `self``.size` `+``=` `response.__sizeof__()`

        `with` `open``(``"D:\\"` `+` `title,``'wb'``) as f:`

            `f.write(response)`

            `f.close()`

        `print``(f``'{title} {self.sizeFormat(self.size)} 耗时：{round(self.time,3)}s'``)`

`if` `__name__` `=``=` `'__main__'``:`

    `fr` `=` `FastRequests()`

    `fr.getImg()`

    `fr.run()`