[知乎](https://zhuanlan.zhihu.com/p/380892265)

总结attention的原理，以及和传统CNN网络的区别和联系0712

## self attention for img

把图片看作是vector set

和CNN有什么区别？

1. CNN有感受野，每个nerual 只考虑recpetive field的信息，原来的感受野是人为划分出来的，现在的recpetive field的形状要么是划分的，要么是聚类生成的。【用attention去找相关的pixcel？ 原来是打分】

2. *在训练上，因为模型更加灵活，所以需要的数据量更大*

## self attention for lan

和RNN什么区别
原来RNN（比如LSTM网络），是序列化产生的
读一个vec of seq，根据时间点产生序列输出
self-attention的输出是考虑全局后输出的

## paper1： ON THE RELATIONSHIP BETWEEN SELF-ATTENTION AND CONVOLUTIONAL LAYERS

### INTRO

ViT-based:  can simultaneously attend to every word of their input sequence

注意力机制的提出：

[1]: https://arxiv.org/pdf/1409.0473.pdf	"Neural machine translation by jointly learning to align and translate. In 3rd International Conference on Learning Representations"

考量表达的距离，给出相似性的判定

给CNN加或者用attention-layer直接替换，可以提升性能

理论上讲可以达到无限精度？

### bg of attention for vision

NLP里面的一一对应关系也可以应用到`像素-标注`这样的关系对,。添加self-attention layer




## paper2 : NEURAL MACHINE TRANSLATION BY JOINTLY LEARNING TO ALIGN AND TRANSLATE

### 1.背景

大多数neural machine translation模型都是e-d模型

如何让机器理解比训练需料库更长的聚集(Corpus)

### 2.机器翻译

#### 关于序列模型

​        从统计角度，文本序列可以看作随机事件$$X_{1.T}=<X_1,X_2...>$$，变量的样本空间是词表(vocabulary)   $$\nu $$，具体会出现的词语是随机事件，概率给出符合自然规则的程度。序列的概率就是联合概率，进一步展开为条件概率的乘积

#### 2.1 RNN

建模：

c时所有前面预测的词：$${y_1,...y_{t-1}}$$

预测计算：
$$
p(\mathbf{y})=\prod_{t=1}^{T} p\left(y_{t} \mid\left\{y_{1}, \cdots, y_{t-1}\right\}, c\right)
$$


### 3.Bi-RNN的原理

#### 3.1 decoder

双向模型（依然保持单步输出）

<img src="https://raw.githubusercontent.com/ChangYaQi/notes/main/metaverse/assets/20220713081652.png" style="zoom: 67%;" />

每一步的输出由条件c和隐状态共同决定。

其中c计算所有标注($${h_i}$$)的权重和。

加权系数计算流程是：

1. 建立对齐模型(alignment model)，描述每个`j`位置上的输入和位置`i`上的输出的相似度。用前一层的隐状态和当前输入的标注计算。

2. 

   第 i-j 个位置输入-输出对  $$h_{ij}$$  权重$$\alpha$$计算公式：

   1. 计算能量e:    $$e_{ij} = a (s_{i-1},h_{j})$$ ，该项用于描述i j输入输出匹配度

   其中，计算a使用：前向传播模型，其代价函数的梯度可以反向传播，用梯度去训练模型。

   2. 计算相对应位置的权重——softmax实现

      根据softmax的公式  $$\operatorname{Softmax}\left(z_{i}\right)=\frac{e^{z_{i}}}{\sum_{c=1}^{C} e^{z_{c}}}$$ ，对所有匹配度进行加权激活，得到权重

   $$
   \alpha_{i j}=\frac{\exp \left(e_{i j}\right)}{\sum_{k=1}^{T_{x}} \exp \left(e_{i k}\right)}
   $$

   将所有权重求和得到总权重，该权重描述了输出输出总的相似度。

3. 注意力体现在哪里：<u>正因为$$c_{ij}$$是由上一个隐状态决定的，也就是说decoder决定了attention在哪里，而不用关注所有向量</u>

   ==[notes]== 正因为在计算权重的时候，代入了上一层的状态，不重要的权重就可以自动的被舍弃掉。

   

#### 3.2 decoder

双向RNN BiRNN： 正向$$x_1 \to x_T$$，以及你想顺序$$x_T \to x_1$$,将两部分的hidden state拼接在一起

### A 模型结构

#### GRU USE

gated hidden unit——借鉴 因为，gated hidden可以更好地控制将不重要的丢弃

由于设置了更新门和遗忘门，隐状态可以被控制，而不是有设定的能量参数$$e_{ij}$$直接全部参与计算加权系数

比如，当·序列很长，中间有大量间隔空间的时候，可以遗忘不重要的状态

使用1/0决定要不要更新隐状态



