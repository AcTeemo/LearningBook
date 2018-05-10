#很Python的代码该如何写:
#严格遵循pep8代码风格
'''
像写诗一样写代码
准确无歧义
完整无废话
注意排版以引导读者
注意标点符号以帮助读者
保证可读性的前提下代码尽可能短小
'''

#1.变量值交换:
	a, b = b, a
#2.列表推导式:
    #列表
	numbers = [x**2 for x in range(20) if x%3==0]
	#集合
	numbers = {x**2 for x in range(20) if x%3==0}
	#字典
	numbers = {x:x**2 for x in range(20) if x%3==0}
#3.字符串拼接:
	a = ['i','love','u']
	b = ''.join(a)
	>>'iloveu'
#4.字符串翻转
	a[::-1]
#5.迭代对象善用enumerate类 
	cities = ['Beijing', 'Tianjin','Shanghai']  
    for  index,city in enumerate(cities,1):  #1是启始值,默认0
          print index,":"city  
#6.lambda #匿名函数,通常和map一起使用
#7.with #通过上下文对象来处理关系
#8.python中的三目运算
	a = 0 if b else 1
#9.functools.partial
	偏函数,将已知函数的参数固定死
#10.善用内置函数
#11.合理使用数据结构
#12.使用高级并发工具
#13.使用装饰器