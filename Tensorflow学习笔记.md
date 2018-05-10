机器学习,tensorflow, sklearn, sklearn
0.matplotlib:

1.#升级为最新版:
  Pip insall --upgrade tensorflow
  pip添加国内原:
  pip install xxxx -i https://pypi.tuna.tsinghua.edu.cn/simple/
  
2.#学前准备
	2.1.#numpy基础知识
		2.1.1#创建数组:
			#指定元素:
			a = np.array([1, 2, 3, 4])#一个一维数组
			#指定数据类型:
			a = np.array([1, 2, 3, 4], dtype=np.int)#整型数组
			#特定数组:
			a = np.array([[1, 2, 3], [2, 3, 4]])#2行3列数组
			a = np.zeros((3, 4))#3行4列全为0 的数组
			a = np.ones((3, 4))#3行4列全为1 的数组
			a = np.empty((3, 4))#同上,数据为空
			a = np.arange(12).reshape(3, 4)
			a = np.linspace(1, 10, 20),reshape()
			#转置
			a.T or np.transpose(a)
			#某一行最大最小数的下标
			a.argmax or a.argmin
			#矩阵均值
			a.mean()
			#中位数
			a.median()
			#切片
			a[1,1:2]
			#合并,变成一列
			a[:,np.newaxis]

	2.2 #sklearn
		1th:#steps:
		1).#导入模块:
			from sklearn import datasets#导入数据库
			from sklearn.linear_model import LineraRegression #导入线性回归模型
			from  sklearn.model_selection import train_test_split #分离数据库方法
		2).#加载数据
			dataset = datasets.load_boston()#全部数据集
			data_x = dataset.data
			data_y = dataset.target
			train_x, train_y, test_x, test_y = train_test_split(data_x, data_y, 0.3)
		3).#建立模型:
			model = LineraRegression()
		4).#训练数据:
			model.fit(train_x, train_y)
		5).#测试数据
			model.predict(test_x)
		6).#检测准确率
			model.socre(data_x, data_y)
		2th:#创建数据
		1).x, y = datasets.make_regression(n_samples=100, n_features=1, n_targets=1, noise=10)
		2).#数据正规化:
			from sklearn import preprocessing
			x = preprocessing.scale(x)
		3).#交叉验证:
			from sklearn.cross_validation import cross_validation_score
			
	2.3 #keras
		1)#导入相关包
			from keras.models import Sequential
			from keras.layers import Dense, Activation
		2).#添加网络层
			
			model = Sequential([
			Dense(32, units=784),#全连接层,输出32,输入784
			Activation('relu'),#激活函数
			Dense(10),#第二层,同上
			Activation('softmax'),#第二层,同上
			])
		3).#激活模型
			model.compile(optimizer='rmsprop',#优化器,sgd,Adadelta,RMSprop

              loss='categorical_crossentropy',#损失函数,回归:mean_squared_error或mse
              										#分类:categorical_crossentropy
              metrics=['accuracy']) #返回的指标
		4).#训练数据:
			model.fit(data, labels, epochs=10, batch_size=32)
			#data:训练数据
			#labels:训练标签
			#epochs:训练的回合数
			#batch_size:每批从训练数据中拿出多少个数据
		5).#测试模型
			loss, accuracy = model.evaluate(X_test, y_test)
		6).#保存/载入模型
			1.model.save('xxx.h5')#首先安装h5py
			1.1.mode = load_model('xxx.h5')
				model.predict(data_test)
			#json格式保存载入
			2.json_string = model.to_json()
			model = model_from_json(json_string)
		7).#涉及到运算时天剑lambda:
			input1 = Input(shape=(None,2))
			input2 = Input(shape=(None,1))
			my_concat = Lambda(lambda x: x[0]+x[1])
			output = my_concat([input1,input2])
	2.4 #DQN三要素:
			1.记忆存储
			2.选择动作
			3.学习

	2.5 #gym 关闭模拟窗口,不启用env.render()

	2.6 #图像识别库
		#1.安装tesseract-ocr-setup-3.05.00dev,文件在微云
		#2.安装pytesseract
		#3.将Tesseract-OCR目录添加进PATH环境变量
		#4.将tesseract-OCR目录添加进TESSDATA_OREFIX环境变量