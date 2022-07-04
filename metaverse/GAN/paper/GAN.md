> 
>
> Deep generative models have had less of an impact, due to the difficulty of approximating many intractable probabilistic computations that arise in maximum likelihood estimation and related strategies, and due to difficulty of leveraging the benefits of piecewise linear units in the generative context.

深度生成模型影响力还很小，因为在最大似然估计和相关策略中产生的许多棘手的概率计算。

generative model & adversary model

## adversarial nets

在多层感知机模型中使用

输入： $$x$$，噪声$$p_{z}(z)$$，

建立映射$$G(z;\theta_{g})$$,G 是多层感知机表示的可微函数

建立第二个映射$$D(x;\theta_{d})$$ 输出单个向量

训练的目标是：训练D，最大化正确分配标签给G中产生的样本，训练G最小化$$log(1-D(G(z)))$$
$$
{\color{Green} \min _{G} \max _{D} V(D, G)=\mathbb{E}_{\boldsymbol{x} \sim p_{\text {data }}(\boldsymbol{x})}[\log D(\boldsymbol{x})]+\mathbb{E}_{\boldsymbol{z} \sim p_{\boldsymbol{z}}(\boldsymbol{z})}[\log (1-D(G(\boldsymbol{z})))]}
$$
如果一直只优化G\D中的一个，会导致过拟合

所以我们交替进行对它们的优化

一开始，当G性能很差时，D可以高置信度拒绝样本，因为它们与培训数据显然不同。，式(1)中的第二项过饱和。所以，我们不训练G作为最小化V的标准，而是用G去最大化$$log D(G(z))$$

"原图和生成模型最优化的时候用的log项时不一样的"

## 算法流程

![](F:\filescyq\notes\语义通信\assets\image-20220605194037378.png)

