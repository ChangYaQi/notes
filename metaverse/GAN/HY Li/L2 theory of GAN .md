# theory of GAN

## 1. Generation

为什么需要去寻找分布-> 在高维空间中，只有固定的distribution区域是生成合适的图片，我们的任务就是寻找这样合适的distribution

最大似然估计(MLE)

**KEY:** 寻找  $$\theta$$ 使得 $$P_{G}(x;\theta)$$ 和真实分布 $$P_{data}(x;\theta)$$ 尽可能地相近

步骤：

1. 从 $$ P_{data}(x) $$ 里面采样$${{x}^1,{x}^2,..,{x}^m}$$ 
2. 计算$$P_{G}({x}^i;\theta)$$   //给定一组$$\theta $$ 去计算
3. max似然L： $$L=\prod_{i=1}^{m} P_{G}\left(x^{i} ; \theta\right)$$

***Max likelihood Estimation = Minimize KL Divergence***

<img src="F:\filescyq\notes\语义通信\assets\image-20220605200658190.png" style="zoom:50%;" />

<img src="F:\filescyq\notes\语义通信\assets\image-20220605200917954.png" style="zoom:50%;" />

$$log P_{data}$$对寻找 $$\theta$$ 没有影响，这里这一行的意义就是为了整理式子

生成：映射线性变换->随机分布[-1,1]

判别：二分类 计算loss 给分值

损失：真的损失+假的损失