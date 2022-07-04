# slow-fast network

## 1. intro

<img src="F:\filescyq\notes\语义通信\assets\image-20220610145940180.png" alt="image-20220610145940180" style="zoom: 67%;" />



path1 低帧率帧的语义获取（稀疏帧）

path2 高帧率帧的语义获取（快速动作捕捉）

fusion



比较像SPPnet那种不同层次的融合？

fast 瞬时性；slow 更关注于空间语义

## 2. network

### 预处理

**程序内容：**

getitem 帧捕获

bedio index -> frames (list存储)

采样处理 $$channel\space\times num frames \space\times height\space\times\space width $$

numframes是每秒跳过的帧数。跳得越多，采样速度越慢

1. slow 

   以步长为$$\tau$$对视频进行抽帧

​       meta info的获取方法: dataset /vedio/frame

​      卷积层

2. fast

​        步长是$$\tau / \alpha$$    一般 $$\tau=8$$, $$\alpha$$是快慢帧数的比例

轻量化，卷积层但通道数是$$\beta=1/8$$

### lateral connections 横连接

spatial res betwork for bedio

前后使用不同的分辨率，high->low fram

high/low 分别进行卷积+最后的average pool ，之后进行concat

<img src="F:\filescyq\notes\元宇宙\视频理解\assets\image-20220615145615986.png" alt="image-20220615145615986" style="zoom: 80%;" />

## 3.experiment

### 3. 1 low

1.采样时：64取4帧(T=4)，步长是16

2.时空尺寸$$T \times S^2 $$

### 3.2 fast

$$\alpha=8; \beta=1/8$$



### 3.3 lateral

## 4. 实现

#### 4.1 模型结构

1. forward

x_s x_f fuse(使用conv+bn+relu) cat

fuse: 用fastconcat到slow的向量后面



2. *class* SlowFast(nn.Module):

   **a. def __init__**

   b.  def consruct network

   ```python
   self.s1 self.s2 s3 s4 s5 #对应jresblock块
   ```
   
   

