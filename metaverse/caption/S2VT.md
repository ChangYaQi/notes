[CSDN梳理]()

# Sequence to Sequence – Video to Text

## 1. 简介

本篇论文提出了S2VT

视频帧到文本句子的端到端模型：使用LSTM模型生成句子

用一系列的LSTM模型解释每一帧（真对应编码为单词），之后生成句子。

此外还要计算连续帧对之间的光流also compute the optical flow [2] between pairs of consecutive frames.

## 2. 方法

假设：

frame和word是多对多的

input sequence : x, output sequence: y

(y_1,y_2,y_3,...y_n|x_1,x_s,x_3...,x_n)

使用的网络：

LSTM-RNN

a stack of LSTM with 1000 input

给一个输入，LSTM网络可以负责计算一系列的输出

编码过程：

1. 第一层网络输入的是视频帧，第二层是接收系列帧并且编码，补零操作，用于后续编码

2. 当所有的都输入后，第二层被输入BOS(begin sign)，开始解码隐藏层到一系列的单词

$$\theta^{*}=\underset{\theta}{\operatorname{argmax}} \sum_{t=1}^{m} \log p\left(y_{t} \mid h_{n+t-1}, y_{t-1} ; \theta\right)$$

梯度下降算法： SGD，反馈让LSTM的隐藏层学习



3. 第二的输出$$z_t$$用来获取估计的单词y
4. 分布估计： soft-max
## 3.代码结构

### S2VTModel.py

根据S2VT有两层，encoding decoding

关于词向量的维度选取：

$$n>8logN$$  ，其中N是词表大小，n是维度

#### 初始化函数

> 基本参数
> 
>  ```__init__```
> 两大部分lstm1和lstm2(rnn|cell+hidden_layer)
>
> input: cideo_framesize output: one-hpt编码的向量，所以imput_dim和out_dim相等
>
> BOS: EOS+SOS

### 前向传播函数

output1 = rnn1(input)

input2 = rnn_output1 [concat] padding_words（补充空帧以后连接第一部分的输入到第二部分）

output2= rnn2(output1)
