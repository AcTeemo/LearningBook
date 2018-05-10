

一.numpy基础
	******np中的轴,0,1和tensorflow相反*******
	1.自建矩阵:
		np.rarray([1,2,3])
		np.asarray([1,2,3])
	2.带初始化数据的矩阵:
		np.linespace(0,4,9)#从0-4,产生9个等差数组
		np.zeros(shape)#全为0
		np.ones(shape)#全为1
		np.eye(int)#3维单位矩阵
		np.diag(narray)#以narray为对角线矩阵
		np.random. # 随机包括,ranintnotmal,
	3.矩阵拼接:
	hstack()#水平拼接
	vstack()#垂直拼接
	4.矩阵运算
	矩阵a=[[1,2,3],[1,2,3]]

	矩阵b=[[3,2,1],[3,2,1]]

	常数c=2

	a*b = [[3,4,3,],[3,4,3]]
	a*c = [[2,4,6],[6,4,2]]
	5.切片

	a=[[1,2,3]] #shape=[1,3]
	a[0,:]=[[1,2,3]]
	a[:,1]=[[2]]
	a[1,2]=3
	#如果想要得到一份数组切片的副本而不是视图，需要进行显式的复制操作
	a[1,2].copy()

	6.np.where(conda, x, y) #if conda return a else return b
	7.统计方法
		sum         对数组中全部或某轴向的元素求和
		mean        算数平均数
		std, var    标准差和方差，自由度默认为n
		min, max    最大值， 最小值
		argmin, argmax  最大和最小元素的索引
		cumsum      所有元素累积和
		cumprod     所有元素累积积
		median      中位数
		percentile  分位数,参数,整数,表示百分之多少

	8.bool型统计方法
		sum会转化为0,1机型统计为true的个数
		any 返回t,f 表示是否存在一个或多个True
		all 返回t,f 表示是否全为True

	9.排序
	10.取出多维数组中的下标,a[b==c]
