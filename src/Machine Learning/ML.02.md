### Machine Learning

#### 机器学习
机器学习需要的理论基础：数学，线性代数，数理统计，概率论，高等数学、凸优化理论，形式逻辑等

1.学习准备
* 数学内容的复习，高中数学、概率、线性代数等部分内容.
* Python基础语法学习.
* Google的TensorFlow深度学习开源框架.

2.Google的TensorFlow开源深度学习框架.
深度学习框架，我们可以粗略的理解为是一个“数学函数”集合和AI训练学习的执行框架。通过它，我们能够更好的将AI的模型运行和维护起来。
深度学习的框架有各种各样的版本（Caffe、Torch、Theano等等），我只接触了Google的TensorFlow，因此，后面的内容都是基于TensorFlow展开的，它的详细介绍这里不展开讲述，建议直接进入官网查看
* [TensorFlow的中文社区](http://www.tensorfly.cn/)
* [TensorFlow的英文社区](https://www.tensorflow.org/)

3.TensorFlow环境搭建
环境搭建本身并不复杂，主要解决相关的依赖。但是，基础库的依赖可以带来很多问题，因此，建议尽量一步到位，会简单很多。

* 操作系统
操作系统：CentOS 7.2 64位（GCC 4.8.5）
因为这个框架依赖于python2.7和glibc 2.17。比较旧的版本的CentOS一般都是python2.6以及版本比较低的glibc，会产生比较的多基础库依赖问题。而且，glibc作为Linux的底层库，牵一发动全身，直接对它升级是比较复杂，很可能会带来更多的环境异常问题。

* 软件环境
我目前安装的Python版本是python-2.7.5，建议可以采用yum install python的方式安装相关的原来软件。然后，再安装 python内的组件包管理器pip，安装好pip之后，接下来的其他软件的安装就相对比较简单了。

例如安装TensorFlow，可通过如下一句命令完成（它会自动帮忙解决一些库依赖问题）：pip install -U tensorflow

注意：这里需要特别注意的是，不要按照TensorFlow的中文社区的指引去安装，因为它会安装一个非常老的版本（0.5.0），用这个版本跑很多demo都会遇到问题的。而实际上，目前通过上述提供的命令安装，是tensorflow (1.0.0)的版本了。

Python（2.7.5）下的其他需要安装的关键组件：
```markdown
(1)  tensorflow (0.12.1)，深度学习的核心框架

(2) image (1.5.5)，图像处理相关，部分例子会用到

(3)PIL (1.1.7)，图像处理相关，部分例子会用到
```

除此之后，当然还有另外的一些依赖组件，通过pip list命令可以查看我们安装的python组件：
```markdown
adium-theme-ubuntu (0.3.4)
backports.ssl-match-hostname (3.5.0.1)
backports.weakref (1.0.post1)
bleach (1.5.0)
cached-property (1.3.1)
cryptography (1.7.1)
docker (2.6.1)
docker-compose (1.17.1)
docker-pycreds (0.2.1)
dockerpty (0.4.1)
docopt (0.6.2)
enum34 (1.1.6)
funcsigs (1.0.2)
functools32 (3.2.3.post2)
futures (3.2.0)
gevent (1.1.2)
greenlet (0.4.11)
gyp (0.1)
html5lib (0.9999999)
idna (2.2)
ipaddress (1.0.17)
jsonschema (2.6.0)
keyring (10.3.1)
keyrings.alt (2.2)
Markdown (2.6.10)
mock (2.0.0)
numpy (1.13.3)
pbr (3.1.1)
pip (9.0.1)
protobuf (3.5.0.post1)
pyasn1 (0.1.9)
pycrypto (2.6.1)
pygobject (3.22.0)
pyxdg (0.25)
PyYAML (3.12)
requests (2.11.1)
SecretStorage (2.3.1)
setuptools (38.2.4)
shadowsocks (3.0.0)
six (1.11.0)
tensorflow (1.4.1)
tensorflow-gpu (1.4.1)
tensorflow-tensorboard (0.4.0rc3)
texttable (0.9.1)
unity-lens-photos (1.0)
websocket-client (0.44.0)
Werkzeug (0.13)
wheel (0.30.0)
```
按照上述提供的来搭建系统，可以规避不少的环境问题。
搭建环境的过程中，我遇到不少问题。例如：在跑官方的例子时的某个报，AttributeError: ‘module’ object has no attribute ‘gfile’，就是因为安装的TensorFlow的版本比较老，缺少gfile模块导致的。而且，还有各种各样的。

更详细的安装说明：https://www.tensorflow.org/install/install_linux

* TensorFlow环境测试运行
测试是否安装成功，可以采用官方的提供的一个短小的例子，demo生成了一些三维数据, 然后用一个平面拟合它们（官网的例子采用的初始化变量的函数是initialize_all_variables，该函数在新版本里已经被废弃了）：


步骤划分：
```markdown
(1)  准备数据：获得有标签的样本数据（带标签的训练数据称为有监督学习）；

(2) 设置模型：先构建好需要使用的训练模型，可供选择的机器学习方法其实也挺多的，换而言之就是一堆数学函数的集合；

(3)损失函数和优化方式：衡量模型计算结果和真实标签值的差距；

(4)真实训练运算：训练之前构造好的模型，让程序通过循环训练和学习，获得最终我们需要的结果“参数”；

(5)验证结果：采用之前模型没有训练过的测试集数据，去验证模型的准确率。
```

其中，TensorFlow为了基于python实现高效的数学计算，通常会使用到一些基础的函数库，例如Numpy（采用外部底层语言实现），但是，从外部计算切回到python也是存在开销的，尤其是在几万几十万次的训练过程。因此，Tensorflow不单独地运行单一的函数计算，而是先用图描述一系列可交互的计算操作流程，然后全部一次性提交到外部运行（在其他机器学习的库里，也是类似的实现）。所以，上述流程图中，蓝色部分都只是设置了“计算操作流程”，而绿色部分开始才是真正的提交数据给到底层库进行实际运算，而且，每次训练一般是批量执行一批数据的。

#### 经典入门demo:识别手写数字（MNIST）
常规的编程入门有“Hello world”程序，而深度学习的入门程序则是MNIST，一个识别28*28像素的图片中的手写数字的程序。

MNIST的数据和官网：MNIST handwritten digit database, Yann LeCun, Corinna Cortes and Chris Burges

深度学习的内容，其背后会涉及比较多的数学原理，作为一个初学者，受限于我个人的数学和技术水平，也许并不足以准确讲述相关的数学原理，因此，本文会更多的关注“应用层面”，不对背后的数学原理进行展开，感谢谅解。

1.加载数据

程序执行的第一步当然是加载数据，根据我们之前获得的数据集主要包括两部分：60000的训练数据集（mnist.train）和10000的测试数据集（mnist.test）。里面每一行，是一个28*28=784的数组，数组的本质就是将28*28像素的图片，转化成对应的像素点阵。

之前我们经常听说，图片方面的深度学习需要大量的计算能力，甚至需要采用昂贵、专业的GPU（Nvidia的GPU），从上述转化的案例我们就已经可以获得一些答案了。一张784像素的图片，对学习模型来说，就有784个特征，而我们实际的相片和图片动辄几十万、百万级别，则对应的基础特征数也是这个数量级，基于这样数量级的数组进行大规模运算，没有强大的计算能力支持，确实寸步难行。当然，这个入门的MNIST的demo还是可以比较快速的跑完。

2. 构建模型

NIST的每一张图片都表示一个数字，从0到9。而模型最终期望获得的是：给定一张图片，获得代表每个数字的概率。比如说，模型可能推测一张数字9的图片代表数字9的概率是80%但是判断它是8的概率是5%（因为8和9都有上半部分的小圆），然后给予它代表其他数字的概率更小的值。

MNIST的入门例子，采用的是softmax回归(softmax regression)，softmax模型可以用来给不同的对象分配概率。

为了得到一张给定图片属于某个特定数字类的证据（evidence），我们对图片的784个特征（点阵里的各个像素值）进行加权求和。如果某个特征（像素值）具有很强的证据说明这张图片不属于该类，那么相应的权重值为负数，相反如果某个特征（像素值）拥有有利的证据支持这张图片属于这个类，那么权重值是正数。类似前面提到的房价估算例子，对每一个像素点作出了一个权重分配。

3. 损失函数和优化设置

为了训练我们的模型，我们首先需要定义一个指标来衡量这个模型是好还是坏。这个指标称为成本（cost）或损失（loss），然后尽量最小化这个指标。简单的说，就是我们需要最小化loss的值，loss的值越小，则我们的模型越逼近标签的真实结果。

4. 训练运算和模型准确度测试

5. 实时查看参数的数值的方法
