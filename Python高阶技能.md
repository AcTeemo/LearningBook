# Python 高阶技能

* * *

0.  将py2转py3:
    cd Python3/tool/script文件夹或者ananconda类似的文件夹下
    `py2to3.py -w f:/xxxx/xxx.py`

1.  使用@propert

    将方法当成属性调用,但是可以对属性加以条件

    单纯使用@property,属性是只读的,不可写入

    需要加入@当前属性名.setter

    例如:birth是可读可写,age只读

    ```python
    #! python
    class Student(object):
        @property
        def birth(self):
            return self._birth

        @birth.setter
        def birth(self, value):
            self._birth = value

        @property
        def age(self):
            return 2015 - self._birth
    ```

2.  import logging

    ```python
    logging.basicConfig(level=logging.INFO)#debug，info，warning，error
    # 打印日志
    ```

3.  序列化与反序列化

    ```python
    #! python
    import json# 序列化
    son.dumps(s,function)
    #反序列化
    json.load(json)
    ```

常用内建模块

1.  datatime:

    ```python
    from datetime import datetime
    #获取当前时间
    dt = datetime.now()
    #指定日期时间
    dt = datetime(2018, 1, 2, 12, 23)
    >>2018-01-02 12:23:00
    #转换为timestamp,浮点数
    dt.timestamp()
    #str 转换为datetime
    cday = datetime.strptime('2015-6-1 18:19:59', '%Y-%m-%d %H:%M:%S')
    #datetime 格式化为str
    now.strftime('%Y-%m-%d %H:%M:%S')
    #加减,导入timedelta
    now + timedelta(hours,days)
    ```

2.  collections

    1.  namedtuple,带属性名字的元祖

        ```python
        from collections import namedtuple
        Point = namedtuple('Point',['x', 'y'])
        p = Point(1, 2)
        p.x
        >>1
        p.y
        >>2
        ```

    2.  deque,加快list的插入和删除

    ```Python
    	q = deque(['a', 'b', 'c'])
    	q.append('d')
    	# 其他方法
    	# appendleft, pop, popleft
    ```

    3.  defaultdict

    ```python
    	#当前key不存在时,设置一个默认值,返回默认值,而不是报错
    	dd = defaultdict(lambda: 'N/A')
    	#注意默认值是调用函数返回的，而函数在创建defaultdict对象时传入
    ```

    4.  OrderedDict

        > 对字典的key排序

    5.  Counter

    ```python
        #对字符串字符出现的次数统计,返回字典
        >>> from collections import Counter
    	>>> c = Counter()
    	>>> for ch in 'programming':
    	...     c[ch] = c[ch] + 1
    	...
    	>>> c
    	Counter({'g': 2, 'm': 2, 'r': 2, 'a': 1, 'i': 1, 'o': 1, 'n': 1, 'p': 1})
    ```

3.  hashlib

    ```python
    	#提供了常见的摘要算法，如MD5，SHA1
    import hashlib
    md5 = hashlib.md5()
    md5.update('how to use md5 in python hashlib?'.encode('utf-8'))
    print(md5.hexdigest())
    ```

4.  itertools

````python
      #迭代器
    	#无线迭代器
    	count,repeat,circle
    	itertools.takewhile(function,iterable)
    	2)chain()#串联两个可迭代对象
    	3)groupby()#把迭代器中相邻的重复元素挑出来放在一起
    		for key, group in itertools.groupby('AAABBBCCAAA'):
    		...     print(key, list(group))
    		...
    		A ['A', 'A', 'A']
    		B ['B', 'B', 'B']
    		C ['C', 'C']
    		A ['A', 'A', 'A']
        ```
    5. urllib
    ```python
    	from urllib import request

        with request.urlopen('https://api.douban.com/v2/book/2129650') as f:
            data = f.read()
            print('Status:', f.status, f.reason)
            for k, v in f.getheaders():
                print('%s: %s' % (k, v))
            print('Data:', data.decode('utf-8'))
        # 具体参见爬虫攻略
````

6.  多线程,多进程,异步,协程
    a)python多线程是伪多线程,不建议使用,见附录代码

7.  装饰器

```python
    # 示例代码
    # -*- coding: utf-8 -*-
    import time
    import functools

    def log(condition):
        def decorate(func):
            @functools.wraps(func)
            def wrapper(*args, **kw):
                if condition:
                    time_tag = time.time()
                    func(*args, **kw)
                    print(time.time() - time_tag)
                else:
                    return func(*args, **kw)
            return wrapper
        return decorate


    @log(False)
    def add(x, y):
        print(x + y)
        time.sleep(1)

    add(1, 2)
```

8.  作用域:
    引用全局变量，不需要golbal声明，修改全局变量，需要使用global声明
    nonlocal# 一般用在嵌套函数中,表示使用上一层的变量

9.  为python命令添加参数

```python
	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument('file')#添加参数
	parser.add_argument('rate', type=float)#为参数指定参数类型
	args = parser.parse_args()
	imgpath = args.file #将参数关联到变量
```

10. 闭包

    嵌套函数,的内部函数,外部函数的变量成为自有变量

11. 异常调试:

```python
import logging
logging.basicConfig(level=logging.INFO)
```
