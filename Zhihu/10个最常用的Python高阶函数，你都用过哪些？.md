高阶函数是那些接受函数作为参数，或者返回一个函数作为结果的函数。以下是10个最常用的Python高阶函数，包括代码示例、使用场景和注意点：

**1\. map() - 对序列的每个元素应用一个函数，并返回结果**

```python
   def square(x):
       return x * x

   numbers = [1, 2, 3, 4, 5]
   squared = map(square, numbers)
   
   print(list(squared))  # 输出: [1, 4, 9, 16, 25]
```

使用场景：对序列中的每个元素进行相同的操作。

注意点：map()返回的是一个迭代器，需要用list()等函数转换为列表。

  

**2\. filter() - 过滤序列中符合条件的元素**

```python
   def is_even(x):
       return x % 2 == 0

   numbers = [1, 2, 3, 4, 5]
   even_numbers = filter(is_even, numbers)
   
   print(list(even_numbers))  # 输出: [2, 4]
```

使用场景：根据条件从序列中选择元素。

注意点：filter()返回的是一个迭代器。

  

**3\. reduce() - 对序列的所有元素应用一个函数，返回单个结果**

```python
   from functools import reduce

   def add(x, y):
       return x + y

   numbers = [1, 2, 3, 4, 5]
   result = reduce(add, numbers)
   
   print(result)  # 输出: 15
```

使用场景：对序列中的元素进行累积操作。

注意点：reduce()在Python 3中被移到了functools模块中。

  

**4\. sorted() - 对序列进行排序**

```python
   numbers = [5, 3, 1, 4, 2]
   sorted_numbers = sorted(numbers)
   
   print(sorted_numbers)  # 输出: [1, 2, 3, 4, 5]
```

使用场景：对序列进行排序。

注意点：sorted()返回一个新的排序后的列表，原始列表不会被改变。

  

**5\. enumerate() - 为序列生成索引和值的元组**

```python
   names = ["Alice", "Bob", "Charlie"]
   for index, name in enumerate(names):
       print(f"Index: {index}, Name: {name}")
```

使用场景：在遍历序列时获取元素的索引。

注意点：enumerate()返回一个枚举对象，通常用于for循环中。

  

**6\. lambda() - 创建匿名函数**

```python
   square = lambda x: x * x
   print(square(5))  # 输出: 25
```

使用场景：当需要一个简单的函数，但又不想使用\`def\`定义时。

注意点：lambda函数通常用于一行代码的小函数。

  

**7\. all() - 检查序列中的所有元素是否都为真**

```python
   numbers = [1, 2, 3, 4, 5]
   print(all(numbers))  # 输出: True
```

使用场景：用于逻辑判断，确保序列中的所有元素都满足某个条件。

注意点：如果序列为空，all()将抛出TypeError。

  

**8\. any() - 检查序列中是否有任何元素为真**

```python
   numbers = [0, 1, 2, 3, 0]
   print(any(numbers))  # 输出: True
```

使用场景：用于逻辑判断，检查序列中是否有至少一个元素满足条件。

注意点：如果序列为空，any()将抛出TypeError。

  

**9\. zip() - 将多个序列压缩为一个元组的列表**

```python
   names = ["Alice", "Bob"]
   ages = [24, 30]
   zipper = zip(names, ages)
   
   print(list(zipper))  # 输出: [('Alice', 24), ('Bob', 30)]
```

使用场景：将多个序列的对应元素打包成一个个元组。

注意点：zip()返回的是一个迭代器，可以通过list()转换为列表。

  

**10\. max() / min() - 找出序列中的最大值或最小值**

```python
    numbers = [1, 2, 3, 4, 5]
    max_value = max(numbers)  # 输出: 5
    min_value = min(numbers)  # 输出: 1
```

使用场景：找出序列中的极值。

注意点：max()和min()可以接受一个key函数参数，用于指定比较的依据。

  

这些高阶函数在日常编程中非常有用，它们可以帮助我们以更简洁和Pythonic的方式编写代码。

end-

**都看到这里了，别客气点个赞吧。活该你一夜起来就暴富、睁开眼睛就脱单！**

Hello亲爱的知友，我是 @G铭瑄 ，一名半路出家的程序员，喜欢分享成长、编码知识、好物推荐、美文收集，也在探索各种自媒体副业的路子，欢迎关注我 @G铭瑄，让我们一起成长一起暴富，争取早日为自己打工，挣睡后收入！

本文转自 <https://zhuanlan.zhihu.com/p/687608204>，如有侵权，请联系删除。