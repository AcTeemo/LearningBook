# Python基础笔记

## 安装最新的包

`conda install -c conda-forge scrapy`

## 更新pip源

linux:

	/etc/pip.con
	~/.pip/pip.conf
	~/.config/pip/pip.conf

Mac OSX:

	~/Library/Application Support/pip/pip.conf
	~/.pip/pip.conf
	/Library/Application Support/pip/pip.conf

Windows:

	%APPDATA%\pip\pip.ini
	%HOME%\pip\pip.ini
	C:\Documents and Settings\All Users\Application Data\PyPA\pip\pip.conf (Windows XP)
	C:\ProgramData\PyPA\pip\pip.conf (Windows 7及以后)

## Google Python命名规范
|写法|应用|
|:-:|:-:|
|module_name|  模块|
|package_name|  包|
|ClassName|  类|
|method_name|  方法|
|ExceptionName|   异常|
|function_name|  函数|
|GLOBAL_VAR_NAME| 全局变量|
|instance_var_name|  实例|
|function_parameter_name|   参数|
|local_var_name|  本变量|

## 基本数据类型,无需定义类型,直接赋值

```python
#! python
a=1 # 整型
b=2.3 # 浮点
# 类型转换
int(x) # 将x转换为一个整数。
float(x) # 将x转换到一个浮点数。
c="hello" # 字符串
# 整数与浮点运算,会自动转换为浮点数.
print(a, end=',') # 输出到同一行,用','隔开
print("1 到 %d 之和为: %d" % (n, sum)) # 字符串拼接变量
print(a,b) # 可以输出多个变量
# 截取字符串
# 变量[头下标:尾下标]
# Python中的字符串不能改变,包括数字,元祖,引用时是传值
# 而list, dict
c[0:2] # 不能对其赋值操作
c[2:-1] #-1为倒数第二个
c[0:] # 第一个往后所有字符
c*2 # 复制
c+"Test" # 拼接
r"asdd\s" # r表示原始字符,里面的\就是个字符
str="hello\
world!" # \也可以用作续行,连接上下两行的字符,也可用'''....'''或者"""...."""
```

2.#列表 用[]
	list = [ 'abcd', 786 , 2.23, 'runoob', 70.2 ] #不分数据类型

	list = [x ** 2 for x in range(10) if x % 3 == 0] #可以加if条件筛选
	M = [[0 for x in range(N)] for x in range(N)]#多维数组
	#列表可以赋值
	list[1:2]=[1,2]
	#步长
	list[::-1] #负数步长表示从最后一个往前
	#其他同字符串
3.#元祖 用()
	tuple = ( 'abcd', 786 , 2.23, 'runoob', 70.2  )
	tuple=(2,)#元祖中只有一个元素需要加","
	#元祖中的值不可改变,但是可以放改变的列表,字符串可看做特殊的元祖
4.#集合 用{}表示无序不重复
  student = {'Tom', 'Jim', 'Mary', 'Tom', 'Jack', 'Rose'}
  #创建一个空的set不能用set={},而是set.set()
  #可以做集合运算
  	print(a-b)       # a和b的差集
	print(a | b)     # a和b的并集
	print(a & b)     # a和b的交集
	print(a ^ b)     # a和b中不同时存在的元素
5#字典
	dict = {}
	dict['one'] = "1 - 菜鸟教程"
	dict[2]     = "2 - 菜鸟工具"
	tinydict = {'name': 'runoob','code':1, 'site': 'www.runoob.com'}
	dict.items()#返回字典的所有key,value,列表的形式
6.#运算符
	#逻辑 
	and ,or ,not
	#成员 
	in ,not in
7.#逻辑控制:
	if var1:
   		print ("1 - if 表达式条件为 true")
   		print (var1)
	#如果if只有一条语句可以放在一行
8.#循环:
	#while
	while :
		pass
	else:
		pass
	#for
	for <variable> in <sequence>:
    	<statements>#可添加if判断,break跳出
	else:
    	<statements>
    #实例:
    #遍历数组
    a=[12,12,14,14,'你好']
    for i in range(len(a)):
	    print(i, a[i])
9.#变量作用域:
	''''L （Local） 局部作用域
		E （Enclosing） 闭包函数外的函数中
		G （Global） 全局作用域
		B （Built-in） 内建作用域
		#Python 中只有模块（module），类（class）以及函数（def、lambda）才会引入新的作用域，其它的代码块（如 if/elif/else/、try/except、for/while等）是不会引入新的作用域的
		当内部作用域想修改外部作用域的变量时，就要用到global和nonlocal关键字了。'''
		global num  # 需要使用 global 关键字声明
10.#类的方法:
	#在类地内部，使用 
	def #关键字来定义一个方法，与一般函数定义不同，类方法必须包含参数 self, 且为第一个参数，self 代表的是类的实例
	class MyClass:
    """一个简单的类实例"""
    i = 12345
    def f(self):
        return 'hello world'
 
	# 实例化类
	x = MyClass()
	 
	# 访问类的属性和方法
	print("MyClass 类的属性 i 为：", x.i)
	print("MyClass 类的方法 f 输出为：", x.f())

	#构造方法:
	def __init__(self,n,a,w):
		self.n = n
		self.a = a
		self.w = w
	super(class,slef).function()#访问父类方法,参数不需要self

11.#主函数
   If  __name__=’__main__’:

        /*函数体*/

12.#内置函数:
	filter(function,iterable)#过滤函数,function---判断函数,iterable---可迭代对象(列表,字典...)
	filter(lambda x: x % 2 == 0, [1,2,3,4,5,6,7])#--过滤列表中的奇数,x为参数
	#等同于:
		def function(x):
			return x % 2 == 0

13.#函数参数-命名关键字参数:
	def person(name, age, *, city, job):
    	print(name, age, city, job)
    #命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数
	#如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了：
	def person(name, age, *args, city, job):
    	print(name, age, args, city, job)
    #命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错
	#命名关键字参数可以有缺省值，从而简化调用

	#参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数
	#可变参数
	def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

15.#匿名函数,lambda 没有return

	
