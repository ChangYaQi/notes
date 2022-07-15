# From Show to Tell: A Survey on Image Captioning[2107]

## 1. Introduction

大致分类：

CNN+RNN, , attention.  reinforce, Transformers, BERT-like

本文关注：

方法，衡量，训练策略

![image-20220711143941992](https://raw.githubusercontent.com/ChangYaQi/notes/main/metaverse/assets/image-20220711143941992.png)

## 2 VISUAL ENCODING
### 2.1  Encoding

CNN最后一层用来提取特征，，特征送入后续语言模型作为条件参数： SHow and Tell

“Selfcritical sequence training for image captioning,: ResNe101 用处：保留了原来的早期提取特征

优点：简单，结构紧凑；==缺点：信息过度压缩，缺乏细粒度特征==

### 2.2 Attention over Grid of CNN

目的：提升ecoding的粒度分辨能力

#### 2.2.1 additive attention

【Transformer论文中提到的】加型注意力机制

> D. Bahdanau, K. Cho, and Y. Bengio, “Neural machine translation by jointly learning to align and translate,” in ICLR, 2014.

这篇用的是前向传传播，两组矢量表：$$x_n 和h_m$$

“加性”注意力评分的方式

$$f_{\mathrm{att}}\left(\mathbf{h}_{i}, \mathbf{x}_{j}\right)=\mathbf{W}_{3}^{\top} \tanh \left(\mathbf{W}_{1} \mathbf{h}_{i}+\mathbf{W}_{2} \mathbf{x}_{j}\right)$$

**为什么要用注意力机制：** 

![image-20220712085245760](https://raw.githubusercontent.com/ChangYaQi/notes/main/metaverse/assets/image-20220712085245760.png)

#### Bottom-up and top-down attention

用RCNN当特征提取器，再对interest region 用Pooling==（啥pooling?）==

#### other

### 2.3 Attention注意力机制的
因为 每次的图文对翻译都是一对一的，必须采用有记忆的网络，比如LSTM

top-down model的做法，使用预先学习的assumption预测后续的词，而不是等到图像检索时给初

### 2.4  Graph based

==使用图卷积神经GCN== 不懂！

> T. Yao, Y. Pan, Y. Li, and T. Mei, “Exploring Visual Relationship for Image Captioning,” in ECCV, 2018. [56] L. Guo, J. Liu, J. Tang, J. Li, W. Luo, and H. Lu, “Aligning linguistic words and visual semantic units for image captioning,” in ACM Multimedia, 2019.

1. 视觉关系场景图？scene graph

[关于视觉场景图](https://blog.csdn.net/qq_39388410/article/details/105877505)

==**使用语义网络描述关系**== （这里之后值得好好看一下）

2.。  Hierarchical trees

root: wjole image

intermediate nodes: image region and relation

leaves: segment elements

【这里需要不重一些 python写数据结构的知识？】

### 2.5 Self-Attention Encoding

#### early arrpoaches

这几年最多的，所以self-attention based 的原理需要进一步学习

==**需要看的实现方法，获取Transformer的使用实例**==

> 最早的应用：X. Yang, H. Zhang, and J. Cai, “Learning to Collocate Neural Modules for Image Captioning,” in ICCV, 2019.

> G. Li, L. Zhu, P. Liu, and Y. Yang, “Entangled Transformer for Image Captioning,” in ICCV, 2019.

大致思路：encode: self-attention + feed-forward;   decoderL fused in decoder?

#### variants

附加烤兔图元素之间关系的

> 1. Image Captioning: Transforming Objects into Words,” in NeurIPS, 2019.
>
> 2. Attention on Attention for Image Captioning: ==最后添加了权重判定们==

