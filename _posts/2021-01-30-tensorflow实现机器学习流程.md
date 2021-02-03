```dart
// 导入 tensorflow
import tensorflow as tf 
// 导入处理 MNIST 数据集的工具类
import input_data

// 加载 MNIST 数据集，获得一个封装好的对象 mnist
mnist = input_data.read_data_sets('MNIST_data', one_hot=True)

// 设置输入（即手写图像），placeholder 表示 x是一个占位符，
// shape 的 None 表示输入数据的第一维可以任意大小
x = tf.placeholder(tf.float32, shape=[None, 784])
// 设置针对输入图像，其期望的输出
y_ = tf.placeholder(tf.float32, shape=[None, 10])

// 定义 Variable，用于存储隐藏层的权重，W 是一个二维权重矩阵，
// W[i][j] 表示第 i 个像素属于第 j 个数字的概率；b 是一维数组，b[i] 
// 表示第 i 个数字的偏移量。因此，要求输入 x 属于各个数字的概率
// 公式为：W * x + b
W = tf.Variable(tf.zeros([784,10]))
b = tf.Variable(tf.zeros([10]))

/** 定义好需要的参数变量后，就可以设置训练过程了，训练过程主要分为 3 步：
   1. 定义隐藏层的输入输出过程
   2. 定义损失函数
   3. 选择训练方法开始训练 **/

// 1.定义隐藏层的输入输出过程 ：
//    之前我们说用 softmax 回归模型来做隐藏层，TensorFlow 已经实现了
//    softmax 的具体方法，所以我们只要一行代码就能表示整个前馈的过程
y = tf.nn.softmax(tf.matmul(x, W) + b)

// 2.定义损失函数：
//    我们使用交叉熵来衡量结果的好坏
cross_entropy = -tf.reduce_sum(y_*tf.log(y))

// 3.选择训练方法开始训练：
//    由于我们已经知道了损失函数，我们的训练目的是让损失函数最小，
//    这里我们使用梯度下降的方法求最小值
train_step = tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)

// 定义好训练过程后，就可以开始真正的训练过程了

// 初始化 session
sess = tf.Session() 

//加载所有 variable
init = tf.initialize_all_variables() 
sess.run(init)

// 使用随机梯度下降的方法，分批次多次训练
for i in range(1000): 
    batch_xs, batch_ys = mnist.train.next_batch(100)   
    //这里随机获取的 batch_xs, batch_ys 用来填充之前定义的占位符 x, y_
    sess.run(train_step, feed_dict={x: batch_xs, y_: batch_ys})
```