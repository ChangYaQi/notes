# Lesson1

## 1 Basic idea of GAN(adversarial)

a) vector->generator->image (vector里面某个参数改变的时候，output的东西就会发生偏移)

b) image -> Discriminator -> scalar(评价系统：评价样例真实的分数高)

可以看作是，学生学习，教师评价，学习，标准提高，反复提升的过程

![](F:\filescyq\notes\语义通信\assets\image-20220602105728979.png)



### Algorithm

<img src="F:\filescyq\notes\语义通信\assets\image-20220602105754563.png" style="zoom:50%;" />

#### 1.discriminator

对于公式：
$$
\tilde{V}=\frac{1}{m} \sum_{i=1}^{m} \log D\left(x^{i}\right)+\frac{1}{m} \sum_{i=1}^{m} \log \left(1-D\left(\tilde{x}^{i}\right)\right)
$$
我们希望真实值$$D(x_i)$$的项越大越好，生成项$$D(\tilde{x}^{i})$$小一点好

在gd时，因为我们希望值越大越好（评判的时候真实样例时偏大的值），所以gd的时候用“+”

#### 2.generator

$$G({z}_i)$$指的是把样例送入，生成新图片，再送到D里面，希望这个数越大越好（训练的目标就是G的判定值变大）

gd



## 2. GAN as structured learning

### structured learning(结构化学习)

在yolo里面，output是regression(scalar)+classification

structured learning : 需要output更复杂的东西

*ont-shot/zero-shot * 假设有些类别只有很少的样例，能不能学

structured lrarning: 是没有一样的类别的学习

会比较注重上下文全局的学习

### Genrator

给每个图象分配一个vector，和classifier过程刚好是相反的

#### auto encoder

对于auto-encoder 实现的流程是：

img->NN Encoder -> vector -< N Decoder -> img2 去对比img/img2的相似度看结果

ae 少了什么？ 希望img和img2越像越好，此时存在的问题是对相似度的衡量准则很模糊。ae学习时，layer中不同的netorn不能考虑pixel之间的orrlation考虑进来。

#### discriminator

生成component时不会检查关联性，但是整图检查可以

dis怎么产生图像？

穷举所有x，送入disciminator看哪个最高分，作为结果

现在存在的问题是：输入的数据源里面只有positive example，会所有都real1,**关键点是从哪里获取低分图片样例训练discriminator**

<img src="F:\filescyq\notes\语义通信\assets\image-20220604115336221.png" style="zoom:50%;" />

**存在的问题**：需要给real examples高分，其余的都给低分，但是高维空间中没有出现real example的地方很难做到完全覆盖。

