优化建议:
	1.优化算法时间复杂度
		如list和set查找某一个元素的时间复杂度分别是O(n)和O(1)不同的场景有不同的优化方式，总得来说，一般有分治，分支界限，贪心，动态规划等思想。

	2.使用dict或set查找元素
	3.合理使用生成器（generator）和yield
	4.循环之外能做的事不要放在循环内
	5.优化包含多个判断表达式的顺序
		对于and，应该把满足条件少的放在前面，对于or，把满足条件多的放在前面
	6.使用join合并迭代器中的字符串
	7.不借助中间变量交换两个变量的值
		使用a,b=b,a而不是c=a;a=b;b=c;
	8.使用 if is True 而非 if == True
	9.使用级联比较x < y < z
	10.while 1 比 while True 更快
	11.使用**而不是pow
	12.使用 cProfile, cStringIO 和 cPickle等用c实现相同功能
	13.使用最佳的反序列化方式(json)
	14.并行编程(multiprocessing)
	15.终级大杀器：PyPy
	16.性能分析工具
		python -m cProfile filename.py



