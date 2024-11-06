---
layout:     post
title:      "PyTorch Fundamentals - Using MINIST Handwritten Digit Recognition as an Example"
subtitle:   "Pytorch基础--以MINIST手写识别为例"
date:       2024-11-06 20:40:00
author:     "pqqqYa"
header-img: img/post/2024-11-06-PytorchFundamental.jpg
catalog: true
tags:
    - Pytorch
    - MINIST
    - ML
    - Python
    - CNN
---




> 深度学习的基础：神经网络，通过构建多层深度神经网络来模拟人类大脑的信息处理与学习机制

> 可以使用[Bohrium](https://bohrium.dp.tech/)进行在线模型训练学习


# 0. Pytorch

## 0.1 Pytorch是什么？

[PyTorch](https://github.com/pytorch/pytorch) 是由 Facebook 开发和维护的用于深度学习的开源 Python 库。该项目于 2016 年启动，并迅速成为开发人员和研究人员中流行的框架。

[Torch](https://github.com/torch/torch7) （*Torch7*）是一个用 C 编写的用于深度学习的开源项目，通常通过 Lua 接口使用。它是 PyTorch 的前身项目，不再积极开发。 PyTorch 在名称中包含 _“Torch”_ 以感谢先前的 torch 库，使用 "*Py*" 作为前缀以表明聚焦于 Python。

PyTorch API 简单灵活，使其成为学术界和研究人员开发新的深度学习模型和应用程序的最爱之一。同时，广泛的应用衍生了许多针对特定应用（例如文本、计算机视觉和音频数据）的扩展，并且可作为预训练模型直接使用。因此，它可能是学术界使用最多的库。

与 [Keras](https://machinelearningmastery.com/tensorflow-tutorial-deep-learning-with-tf-keras/) 等更简单的界面相比，PyTorch 的灵活性是以易用性为代价的，尤其是对于初学者而言。选择 PyTorch 而不是 Keras 的意味着放弃了一些易用性、需要面对更陡峭的学习曲线、以及使用更多的代码以获得更大的灵活性，也许还有一个更有活力的学术社区。

## 0.2 Pytorch深度学习模型的建立范式

1. 准备数据：导入不同的数据集（数据集常使用 Pandas 自动下载）
2. 定义模型：包括Convolution Layer卷积层、Pooling Layer池化层和Full Connection Layer全连接层
3. 训练模型：将训练模型的大量已经标定的数据输入模型，计算损失并调整更新模型参数，重复多个周期，指导模型更好的学习训练数据里面的特征
4. 评估模型：包括损失函数和优化器
5. 做出预测：随着一系列步骤精炼和提取图像特征，随着数据量的减少，特征的精确度逐渐增高


# 1. 神经网络的架构

input → Weights → Summation and Bias → Activation → Output

* input输入
* Weights权重
* Summation and Bias每个数据与不同权重相乘后加和偏差，权重矩阵乘输入数据加偏置项 $$ z = W · x +b $$ 
* Activation激活函数
* Output

当损失函数收敛到最小后得到最终结果

# 2. CNN卷积神经网络

卷积神经网络（简称 CNN）是一种专为图像输入而设计的网络，它们由具有[卷积层](https://machinelearningmastery.com/convolutional-layers-for-deep-learning-neural-networks/)的模型组成，这些卷积层可以提取特征（称为特征图），而[池化层](https://machinelearningmastery.com/pooling-layers-for-convolutional-neural-networks/)会将特征提取到最突出的元素。

一般模型训练的全部过程
1. 数据收集
2. 初始化
3. 前向传播
4. 计算误差
5. 反向传播和参数更新
6. 重复训练

# 3. 建立单层数字识别CNN神经网络模型

## 3.1 准备数据

数据下载，数据格式转换，数据集划分：准备数据，将数据变成适合神经网络训练的格式并加载数据

### 3.1.1 转换数据格式

最初是以PIL图像格式存在，通常是像素数组的形式，每个像素包含了图像的一部分信息，比如颜色信息，这些信息可以是彩色的包含RGB通道，或者是单色的灰度图像。

为了更方便的使用，需要将这些图像转换为PyTorch Tensor图像，把0-255转换为0-1之间的浮点数。

### 3.1.2 数据集划分

每次遍历整个数据集会导致内存不足的问题，深度学习中需要将其分解为更小的batch，即将所有的总数据集分成了一块一块的子数据集。

这样可以快速多次的对模型进行调整，而不是等很长时间进行一次大的调整。

### 3.1.3 可视化训练集示例

随机抽取一些进行展示

## 3.2 特征提取

### 3.2.1 卷积

卷积是一种线性运算，将一组权重与输入相乘生成新的二位数组

卷积层数增多就可以提高更高级的特征

### 3.2.2 池化

缩小表示空间的大小，提高计算效率。

最大池化：捕捉数组的最大值，减少计算所需的值的数量

### 3.2.3 代码解读

定义两个卷积层和两个池化层

~~~python
# 模型定义
class CNN(Module):
    # 定义模型属性
    def __init__(self, n_channels):
        super(CNN, self).__init__()
        # 输入到卷积层 1
        self.hidden1 = Conv2d(n_channels, 32, (3,3))
        kaiming_uniform_(self.hidden1.weight, nonlinearity='relu')
        self.act1 = ReLU() 
        # 池化层 1
        self.pool1 = MaxPool2d((2,2), stride=(2,2))
~~~


**I. 定义这个方法，后面直接使用即可**

`class CNN(Module):`定义一个名为CNN的类，继承自torch.nn.Module，是PyTorch中所有神经网络模块的基类（父类）

`def __init__(self, n_channels):`CNN类的构造函数，包含2个参数，`self`代表地址，一般不会修改，`n_channels`代表输入图像的通道数

`super(CNN, self).__init__()`调用父类Module的初始化方法，确保正确的初始化模型

**II. 卷积层1**
`self.hidden1 = Conv2d(n_channels, 32, (3,3))`第一个卷积层接收`n_channels`个输入通道，并输出32个特征图（输出通道数为32，即这里有32个卷积层用来处理训练数据集，每个卷积核可以提取一种特征，最终得到32个映射），使用的是3×3的卷积核进行卷积运算；`Conv2d`是PyTorch中用于创建二维卷积层的类

`kaiming_uniform_(self.hidden1.weight, nonlinearity='relu')`使用`kaiming_uniform_`初始化权重并设置激活函数为ReLu，深度学习模型训练过程的本质是对weight（即参数）进行更新，这需要每个参数有对应的初始值，`self.hidden1.weight`表示卷积层1的Tensor张量

`self.act1 = ReLU() `创建一个ReLU激活函数并将其赋值给act1属性

**III. 池化层1**

`self.pool1 = MaxPool2d((2,2), stride=(2,2))`将输入的特征图分割成(2，2)大小的区域，并从每个区域中去最大值，`stride=(2,2)`是步长，决定了池化核在输入特征图上的滑动速度。

通过池化，在保留最重要特征的同时，图的尺寸再一次缩小，减少了模型的计算量和参数数量，同时增加在不同模型之间的通用性（减少过拟合，提高泛化能力）

![](https://github.com/pqqqYa/pqqqYa.github.io/blob/main/img/post/2024-11-06/%E6%B1%A0%E5%8C%96%E5%B1%82.png)


# 4. 建立多层数字识别CNN神经网络模型

在单层神经网络的基础上添加新的卷积层和池化层

~~~python
# 模型定义
class CNN(Module):
    # 定义模型属性
    def __init__(self, n_channels):
        super(CNN, self).__init__()
        # 输入到卷积层 1
        self.hidden1 = Conv2d(n_channels, 32, (3,3))
        kaiming_uniform_(self.hidden1.weight, nonlinearity='relu')
        self.act1 = ReLU()
        # 池化层 1
        self.pool1 = MaxPool2d((2,2), stride=(2,2))
        # 卷积层 2
        self.hidden2 = Conv2d(32, 32, (3,3))
        kaiming_uniform_(self.hidden2.weight, nonlinearity='relu')
        self.act2 = ReLU()
        # 池化层 2
        self.pool2 = MaxPool2d((2,2), stride=(2,2))
        # 全连接层hidden3
        self.hidden3 = Linear(5*5*32, 100)
        kaiming_uniform_(self.hidden3.weight, nonlinearity='relu')
        self.act3 = ReLU()
        # 全连接层hidden4，同时也是输出层
        self.hidden4 = Linear(100, 10)
        xavier_uniform_(self.hidden4.weight)
        self.act4 = Softmax(dim=1)
~~~


**I. 卷积层2**

`卷积层 2`第二层卷积进一步提取特征从第一层卷积和池化后得到的特征图中的特征

`self.hidden2 = Conv2d(32, 32, (3,3))`中接收的通道数修改为32，即上一层中输出的32个输出通道

**II. 池化层2**

`池化层2`通过减少特征图的空间尺寸来降低后续网络层的计算负载并在一定程度上增强模型的抗噪声能力

`self.pool2 = MaxPool2d((2,2), stride=(2,2))`和之前一样使用最大池化策略

**III. 第一个全连接层hidden3**

`全连接层`对前面卷积层提取的特征进行整合以进行最终的分类决策，经过2次的卷积和池化图像的特征已被有效提取并压缩，全连接层的作用类似于神经网络中的“连接器”，能够将前一层的所有神经元与当前层的每个神经元相连接

`self.hidden3 = Linear(5*5*32, 100)`接受了来自第二个池化层的800个视觉信号，这些信号已经被整理成了一维的线索，处理中心的目的就是将这些线索转化为100个有意义的概念

`kaiming_uniform_(self.hidden3.weight, nonlinearity='relu')`使用`kaiming_uniform`来初始化处理中心的连接权重

`self.act3 = ReLU()`引入`ReLU激活函数`给对全连接层的输出进行非线性变换

**IV. 第二个全连接层hidden4 / 输出层**

`self.hidden4 = Linear(100, 10)`将之前的100个概念简化为10个最终的决策，每个决策都对应着一个手写数字的类别（0至9）

`xavier_uniform_(self.hidden4.weight)`使用`xavier_uniform`方法来初始化权重，为神经网络的每个连接都赋予了合适的权重

`self.act4 = Softmax(dim=1)`引入`Softmax激活函数`将决策中心的输出转换为概率分布，能够根据概率最高的类别来做出最终的预测


# 5. 前向传播(Forward Propagation)

## 5.1 前向传播的原理

前向传播是数据在神经网络中的流动方向即从输入层经过隐藏层最终到达输出层的过程

前向传播是神经网络处理输入数据并生成预测结果的过程，是神经网络的基础也是整个机器学习中的核心概念之一

### 5.1.1 前向传播的作用

1. 计算输出:根据输入数据和网络结构计算神经网络输出
2. 损失函数评估:使用输出数据与真实标签计算损失函数以评估模型的性能
3. 反向传播准备:为反向传播阶段提供必要的中间信息，如每层的输入、权重和激活函数的导数等

### 5.1.2 CNN架构和前向传播之间的关系

* 构建CNN架构：定义了神经网络的各个层及其参数，卷积层、激活函数、池化层、全连接层和输出层。定义了前向传播的路径和操作顺序。


* 前向传播：数据在已构建的CNN架构中流动和处理的过程依次经过每一层的处理，逐步提取特征，生成最终的输出结果。定义了路径上执行具体的数据处理层与层之间的数据传递和参数计算。


## 5.1.2 反向传播

在前向传播之后进行的，神经网络会通过反向传播算法来调整权重和偏置，以减少损失函数的值，目的是计算损失函数关于网络参数的梯度，以便通过梯度下降等优化算法更新网络的权重

## 5.1.3 前向传播的代码实现

~~~python
# 前向传播
def forward(self, X):
    # 输入到隐层 1
    X = self.hidden1(X)
    X = self.act1(X)
    X = self.pool1(X)
    # 隐层 2
    X = self.hidden2(X)
    X = self.act2(X)
    X = self.pool2(X)
    # 扁平化
    X = X.view(-1, 4*4*50)
    # 隐层 3
    X = self.hidden3(X)
    X = self.act3(X)
    # 输出层
    X = self.hidden4(X)
    X = self.act4(X)
    return X
~~~

`def forward(self, X)`这个函数中每个语句的输入输出都是训练集X，每个语句都控制训练集在之前定义的模型层中进行训练，`self`和后续语句的`self`都表示我们在上一步中定义的模型实例

这些神经网络层接受我们的训练集（自变量）并返回经过训练的自变量，以此类推，这便是在pyTorch中定义前向传播的过程。

在前向传播过程中，每个步骤都是一个神经网络层，数据流转方式相应的由我们定义的方法所确定

**I. 数据进入第一卷积层并处理**

`X = self.hidden1(X)`将输入数据X输入卷积层1，卷积层会对图像进行过滤操作

`X = self.act1(X)`激活函数，使用`ReLU激活函数`，使用的激活函数类型则在CNN架构中被定义，将卷积层的输出进行非线性变换

`X = self.pool1(X)`池化操作

**II. 数据进入第二卷积层并处理**

**III. 数据扁平化**

`X = X.view(-1, 4*4*50)`Flattening数据扁平化具体来说，-1 表示自动计算的批次大小，5×5×32 表示每个样本的数据维度。

**IV. 数据通过全连接层**

**V. 数据通过输出层**

`Softmax激活函数`的并且所有输出值的总和为1

**VI. 返回结果**

`return X`

前向传播的最终结果是经过所有层处理后的输出，这些输出将用于模型的预测或损失计算

# 6. 计算误差

引入损失函数，用于衡量模型预测的准确性，帮助我们了解模型当前表现，指导我们如何改进

# 7. 反向传播和参数更新

通过反向传播算法，将误差从输出层逐层传递回输入层并根据这些误差调整每一层的参数

反向传播：负责计算损失函数相对于每个参数的梯度，这是优化器所需的关键信息

优化器：用于调整模型的参数以最小化损失函数，提高预测性能

# 8. 训练模型

~~~python
# 训练模型
def train_model(train_dl, model):
    # 定义优化器
    criterion = CrossEntropyLoss()
    optimizer = SGD(model.parameters(), lr=0.01, momentum=0.9)
    # 枚举 epochs
    for epoch in range(10):
        # 枚举 mini batches
        for i, (inputs, targets) in enumerate(train_dl):
            # 梯度清除
            optimizer.zero_grad()
            # 计算模型输出
            yhat = model(inputs)
            # 计算损失
            loss = criterion(yhat, targets)
            # 贡献度分配
            loss.backward()
            # 升级模型权重
            optimizer.step()
~~~

**I. 定义损失函数和优化器**

` criterion = CrossEntropyLoss()`定义了一个交叉熵损失函数（CrossEntropyLoss），是分类问题中常用的损失函数

`optimizer = SGD(model.parameters(), lr=0.01, momentum=0.9)`定义了一个随机梯度下降（SGD）优化器，`model.parameters()`表示优化器将更新模型的所有参数，`lr=0.01`学习率（learningrate）决定了每次参数更新的步长大小，`momentum=0.9`动量(momentum)通过在更新方向上加速，帮助优化器在鞍点附近更快地收敛并减小震荡，目的是为了加速收敛并避免陷入局部最优解

`for epoch in range(10):`循环遍历10个训练周期（epoch），一个epoch指的是一个数据集完整地进行一次前向和后向传播的过程，多次遍历可以使模型更好地学习数据的特征，提高模型的泛化能力。通过多次epoch后TrainLoss训练损失降低，TrainACC训练准确度和TestACC测试准确率提升。

`for i, (inputs, targets) in enumerate(train_dl):`遍历训练数据加载器中的每一个mini batch。mini batch是一小部分训练样本的集合，其有利于模型稳定训练提高计算效率，每个mini batch包含输入数据（inputs）和对应的标签（targets）

`optimizer.zero_grad()`在每次mini batch的训练之前必须清除上一次mini batch所累积的梯度。因为PyTorch中的梯度是累积的（默认行为），如果不清除，上一次的训练会对本次训练产生影响。

`yhat = model(inputs)`前向传播，将输入数据（inputs）输入模型，计算模型输出（yhat）

`loss = criterion(yhat, targets)`计算损失，将模型的输出（yhat）与真实的目标标签（targets）传入损失函数，计算损失值（loss），loss衡量了模型的预测与真实标签之间的差距，是模型优化的目标

` loss.backward()`反向传播，计算损失函数相对于模型参数的梯度，这些梯度将用于更新模型的参数从而减小损失函数值

`optimizer.step()`更新模型权重，使用先前计算得到的梯度更新模型的参数，优化器根据设定的学习率和动量等参数调整参数，以最小化损失函数值

# 9. 评估模型

## 9.1 评价指标

在实际训练中，可以采用已有广泛实践的评价指标并将其应用于我们自己的数据集和模型上

回归模型的评估指标主要用于衡量模型的预测值和实际值之间的差距

1. 均方误差（MSE, Mean Squared Error） $$ \frac{1}{n} \sum_{i=1}^{n}(Y_i-\hat{Y_i}  )^2 $$ 其中n为数据量，y为实际值，y方为估计预测值，衡量预测值与实际值之间的平均平方误差，值越小模型的预测效果越好
2. 均方根误差（RMSE, Root Mean Squared Error） $$ RMSE = \sqrt{MSE} $$ 
3. 平均绝对误差（MAE, Mean Absolute Error） $$ MAE=\frac{1}{n}\sum_{i=1}^{n}\lvert  Y_i-\hat{Y_i}\rvert   $$ 衡量预测值与实际值之间的平均绝对误差，值越小模型的预测效果越好
4. 拟合优度(R方，也称为判定系数) $$ R^2=1-\frac{\sum_{i=1}^{n}(Y_i-\bar{Y_i}  )^2}{\sum_{i=1}^{n}(Y_i-\bar{Y_i}  )^2} $$ 表示模型解释变量方差的比例，范围在0-1之间，值越接近1，模型的解释能力越强

分类模型的评估指标主要用于衡量模型的分类效果

1. 混淆矩阵（Confusion Matrix），以矩阵形式展现分类结果，包括TP、TN、FP、FN便于直观分析分类结果的细节。ConfusionMatrix混淆矩阵是机器学习中统计分类模型预测结果的表，以矩阵的形式将数据集中的记录，按照真实的类别与分类模型预测的类别进行汇总。
2. ROC曲线（Receiver Operating Characteristic Curve）与AUC（Area Under the Curve），ROC曲线横轴表示 $$ FPR=\frac{FP}{FP+TN} $$ 即错误的预测为正例的概率，纵轴表示 $$ TPR=\frac{TP}{TP+FN} $$ 即正确地预测为正例的概率。AUC衡量ROC曲线下的面积，AUC越大，表示越有可能将正样本排在负样本前面，模型的分类性能越好
3. 准确率（Precision） $$ Accuracy=\frac{TP+TN}{TP+TN+FP+FN} $$ 衡量模型预测正确的样本数占总样本数的比例适用于类别分布相对均衡的情况
4. 召回率（Recall） $$ Recall=\frac{TP}{TP+FN} $$ 衡量模型实际为正类的样本中被正确预测为正类的比例，适用于关注假阴性较多的情况
5. F1分数（F1 Score） $$ F1 Score=\frac{2\cdot Precision \cdot Recall}{Precision + Recall} $$ 精确率与召回率的调和平均，综合考虑了两者的表现，适用于类别不平衡的情况下

## 9.2 代码解读

~~~python
# 评估模型
def evaluate_model(test_dl, model):
    predictions, actuals = list(), list()
    for i, (inputs, targets) in enumerate(test_dl):
        # 在测试集上评估模型
        yhat = model(inputs)
        # 转化为 numpy 数据类型
        yhat = yhat.detach().numpy()
        actual = targets.numpy()
        # 转化为类标签
        yhat = argmax(yhat, axis=1)
        # 为 stack 格式化数据集
        actual = actual.reshape((len(actual), 1))
        yhat = yhat.reshape((len(yhat), 1))
        # 保存
        predictions.append(yhat)
        actuals.append(actual)
    predictions, actuals = vstack(predictions), vstack(actuals)
    # 计算准确度
    acc = accuracy_score(actuals, predictions)
    return acc
# 评估模型
acc = evaluate_model(test_dl, model)
print('Accuracy: %.3f' % acc)
~~~



`def evaluate_model(test_dl, model):`中`test_dl`测试数据加载器，提供测数据集，`model`待评估的深度学习模型

`predictions, actuals = list(), list()`初始化两个空列表predictions和actuals，存放模型的预测结果和实际标签

`for i, (inputs, targets) in enumerate(test_dl):`遍历数据加载器，返回索引i以及从数据加载器中获取的一批数据inputs和targets，inputs→题目信息，targets→题目正确的答案

`yhat = model(inputs)`让模型输出预测结果的概率分布

`yhat = yhat.detach().numpy()`与`actual = targets.numpy()`将模型的输出yhat和实际标签targets，从PyTorch张量（tensor）转换为NumPy数组，`detach()`方法将张量从计算图中分离出来使其不再参与梯度计算

`yhat = argmax(yhat, axis=1)`使用`argmax函数`沿着第二个维度找到概率类别最大的索引作为模型的预测类标签

`actual = actual.reshape((len(actual), 1))`和`yhat = yhat.reshape((len(yhat), 1))`将 actual和 yhat 都重新调整形状为`(len(actual),1)`即每个样本一个行，确保它们能在后续步骤中正确堆营

`predictions.append(yhat)`和`actuals.append(actual)`将将本批次的预测结果vhat和实际标签actual添加到各自的列表predictions和actuals中

`predictions, actuals = vstack(predictions), vstack(actuals)`使用`vstack函数`将所有批次的预测结果和实际标签分别垂直堆叠起来形成完整的预测和实际标盆数组


`acc = accuracy_score(actuals, predictions)`使用`accuracy_score函数`计算模型在测试几上的准确度

`acc = evaluate_model(test_dl, model)`调用评估函数评估模型并返回准确度acc

# 10. 完整项目代码

~~~python
# PyTorch｜建立图像分类的卷积神经网络模型
from numpy import vstack
from numpy import argmax
from pandas import read_csv
from sklearn.metrics import accuracy_score
from torchvision.datasets import MNIST
from torchvision.transforms import Compose
from torchvision.transforms import ToTensor
from torchvision.transforms import Normalize
from torch.utils.data import DataLoader
from torch.nn import Conv2d
from torch.nn import MaxPool2d
from torch.nn import Linear
from torch.nn import ReLU
from torch.nn import Softmax
from torch.nn import Module
from torch.optim import SGD
from torch.nn import CrossEntropyLoss
from torch.nn.init import kaiming_uniform_
from torch.nn.init import xavier_uniform_
 
# 模型定义
class CNN(Module):
    # 定义模型属性
    def __init__(self, n_channels):
        super(CNN, self).__init__()
        # 输入到隐层 1
        self.hidden1 = Conv2d(n_channels, 32, (3,3))
        kaiming_uniform_(self.hidden1.weight, nonlinearity='relu')
        self.act1 = ReLU()
        # 池化层 1
        self.pool1 = MaxPool2d((2,2), stride=(2,2))
        # 隐层 2
        self.hidden2 = Conv2d(32, 32, (3,3))
        kaiming_uniform_(self.hidden2.weight, nonlinearity='relu')
        self.act2 = ReLU()
        # 池化层 2
        self.pool2 = MaxPool2d((2,2), stride=(2,2))
        # 全连接层
        self.hidden3 = Linear(5*5*32, 100)
        kaiming_uniform_(self.hidden3.weight, nonlinearity='relu')
        self.act3 = ReLU()
        # 输出层
        self.hidden4 = Linear(100, 10)
        xavier_uniform_(self.hidden4.weight)
        self.act4 = Softmax(dim=1)
 
    # 前向传播
    def forward(self, X):
        # 输入到隐层 1
        X = self.hidden1(X)
        X = self.act1(X)
        X = self.pool1(X)
        # 隐层 2
        X = self.hidden2(X)
        X = self.act2(X)
        X = self.pool2(X)
        # 扁平化
        X = X.view(-1, 4*4*50)
        # 隐层 3
        X = self.hidden3(X)
        X = self.act3(X)
        # 输出层
        X = self.hidden4(X)
        X = self.act4(X)
        return X
 
# 准备数据集
def prepare_data(path):
    # 定义标准化
    trans = Compose([ToTensor(), Normalize((0.1307,), (0.3081,))])
    # 加载数据集
    train = MNIST(path, train=True, download=True, transform=trans)
    test = MNIST(path, train=False, download=True, transform=trans)
    # 为训练集和测试集创建 DataLoader
    train_dl = DataLoader(train, batch_size=64, shuffle=True)
    test_dl = DataLoader(test, batch_size=1024, shuffle=False)
    return train_dl, test_dl
 
# 训练模型
def train_model(train_dl, model):
    # 定义优化器
    criterion = CrossEntropyLoss()
    optimizer = SGD(model.parameters(), lr=0.01, momentum=0.9)
    # 枚举 epochs
    for epoch in range(10):
        # 枚举 mini batches
        for i, (inputs, targets) in enumerate(train_dl):
            # 梯度清除
            optimizer.zero_grad()
            # 计算模型输出
            yhat = model(inputs)
            # 计算损失
            loss = criterion(yhat, targets)
            # 贡献度分配
            loss.backward()
            # 升级模型权重
            optimizer.step()
 
# 评估模型
def evaluate_model(test_dl, model):
    predictions, actuals = list(), list()
    for i, (inputs, targets) in enumerate(test_dl):
        # 在测试集上评估模型
        yhat = model(inputs)
        # 转化为 numpy 数据类型
        yhat = yhat.detach().numpy()
        actual = targets.numpy()
        # 转化为类标签
        yhat = argmax(yhat, axis=1)
        # 为 stack 格式化数据集
        actual = actual.reshape((len(actual), 1))
        yhat = yhat.reshape((len(yhat), 1))
        # 保存
        predictions.append(yhat)
        actuals.append(actual)
    predictions, actuals = vstack(predictions), vstack(actuals)
    # 计算准确度
    acc = accuracy_score(actuals, predictions)
    return acc
 
# 准备数据
path = '~/.torch/datasets/mnist'
train_dl, test_dl = prepare_data(path)
print(len(train_dl.dataset), len(test_dl.dataset))
# 定义网络
model = CNN(1)
# # 训练模型
train_model(train_dl, model)  # 该步骤运行约需 5 分钟。
# 评估模型
acc = evaluate_model(test_dl, model)
print('Accuracy: %.3f' % acc)
~~~

