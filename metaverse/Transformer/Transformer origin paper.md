# attention is all you need

学习代码：hugging face

首先： torch集成了transformer的各种模型

参照 source-code-notebook理解

## 1 intro&bg

目前最好的是给CNN+RNN结构上，添加了注意力机制。我们提出的是纯注意力机制

因为RNN基于迁移状态进行状态转移，所以对训练并行化不利。这对于长的序列长度至关重要。

==Transformer可以允许高度并行化   why==

是第一个不用RNN+CNN结构的

## 3 Model architecture模型结构： 

> Given z, the decoder then generates an output sequence (y1, ..., ym) of symbols one element at a time. At each step the model is auto-regressive [9], consuming the previously generated symbols as additional input when generating the next.
==这和RNN不是一样的？==

### 3.1 encoder & decoder

#### encoder

输入序列 $x_1,x_2,...x_n$ 映射到z表示

6个独立层构成，每层还有两个子层

**a. multihead-attention    b. position-wise fully-connect**

<img src="https://raw.githubusercontent.com/ChangYaQi/notes/main/metaverse/assets/image-20220715095317394.png" alt="image-20220715095317394" style="zoom:50%;" />
#### decoder



==masked multi-head attention ?==

N=6 layer  

和encoder相比多了一个sublayer: multi-head attention + add&norm




code

> *结构函数模块*（encoder decoder subconnrction）用class类表示，class里面可以def相关的功能。功能可以继承。共有的子函数用`def`预先定义出来。
>
> 循环的部分用clones 深拷贝
>
> 输出 $y_1,y_2,....,y_n$
>
> 也是6个： attention中进行预测，输出结果送到norm里防止退化，按照层batch_norm
>
> norm使用的参数用s
>
> **Q: 深拷贝是啥？**
>
> `copy.deepcopy`
>
> 要拷贝类里面所有的功能函数以及参数设置，才能实现功能循环，所以，调用：deepcopy
>
> copy的方法： `for _ in range`
>
> 用`class encoder`写：构成为初始化函数`__init__`和`forward`
>
> 其中`__init__`定义层和循环次数
>
> output:` LayerNorm()x+Sublayer(x))`
>
> 能调整的参数： 只有stack数目`N`和每一层的维度 $d_{model}$
>
> *LayerNorm*
>
> ![image-20220704144102559](D:\cyq\notes\metaverse\Transformer\Transformer origin.assets\image-20220704144102559-16569168657091.png)
>
> 这里mask的用处： 保证t时间时候不会看到T以后的输入：比如，对于query $$q_t$$它应该只能看到$$k_1,....k_{t-1}$$，因为是全局读取，无法阻止索引，所以加上mask实现attenntion
>

**注意力机制**

一种动态开关，让计算机可以学会关注重点信息，隐藏单元可以根据情况从不同单元接受输入

分类： soft/global和hard/local机制

三个组件： Query Key Value

用于NLP时，分为以下部分

1. 转换表示：将输入的词转化为embedding vector

   embedded vector: 

2. 输出特征向量列表

3. 每个timestep聚焦某个储存元素





### 3.2 atention

  ==review of attention 在另一个==

self-attention的首次提出就是在这篇文章中



#### dot-product attention 怎么实现自注意力？

提出了QKV结构描述，其中所有的keys都用来计算

**为什么可以用点积**：

$Q \cdot K$ 可以衡量相似度， $\sqrt{d_k}$ 是向量长度，

向量点积，垂直点积为0，匹配的向量如`[1,0,0]]*[1,0,0]=1`, 如果`[1,0,0]*[0,1,0]=0`

和attention有什么关系： 如何自动决定关注（实现自注意力？）：使用dot-product计算相关度，相关度高就应该“关注”

 $\operatorname{Attention}(Q, K, V)=\operatorname{softmax}\left(\frac{Q K^{T}}{\sqrt{d_{k}}}\right) V$

**公式：**

1. 类似原来的注意力机制，总的输入-输出对的权重是，所有匹配度进行加权并softmax激活的结果：$\alpha_{i j}=\frac{\exp \left(e_{i j}\right)}{\sum_{k=1}^{T_{x}} \exp \left(e_{i k}\right)}$ ，即 $softmax\ (e_{ij})$

2. 在这个模型里，QK分别充当输入和输出的匹配对，$\sqrt{d_{k}}$ 体现scaled

   其意义是：QK点积用来计算权重的结果和方差有很大关系，归一化处理以后保持梯度稳定

3. V的作用 假设有“三个字” 有Q和K

   ![image-20220713171241402](https://raw.githubusercontent.com/ChangYaQi/notes/main/metaverse/assets/image-20220713171241402.png)

表明的事情：在读当前词的时候 应该分配多少关注度给其他词，实现自注意力

