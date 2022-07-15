# ClipCap: image Captioning

2022/6/30-2022/7/5

- 工程环境：torch1.11
- 数据集： Flickr30k

[clipclap chinese](https://github.com/yangjianxin1/ClipCap-Chinese)

## 1. 大致思路

encoder: CLIP 编码得到图片向量

映射： MLP Mapping clip_embed -> fc+fc -> prefic_embeds

decoder: 用了GPT2模型[GPT2通用模型]

gpt2使用了Transformer[vit-B]

 ## 2. 具体实现

### 2.1  preprocess

#### 2.1.1 目的

图像预处理，生成.pkl。  实现内容：读取flickr30数据集id和标注caption-> encdoing为embedding

#### 2.1.2 文件
python process_flickr.py

修改代码内容：15行添加`encodind="utf-8"` 后面那个不用改

*pickle*: 通过pickle模块的序列化操作我们能够将程序中运行的对象信息保存到文件中去，永久存储；通过pickle模块的反序列化操作，我们能够从文件中创建上一次程序保存的对象。【读取序列信息，写入文件】

再次修改，在导入PIL后加这一句，跳过破损的图片： 

```python
Image.LOAD_TRUNCATED_IMAGES = True
```

并且重新解压，没有问题了

#### 2.1.3 依赖的库

1. pickle
2. tqdm: 进度条可视化库
2. clip

#### 2.1.4 structure

建立 image_id 编码 ， 描述的 List，两个存放List?

```python
image_id2embed = {}
caption_lisy = []
```



训练30k-40k的时候结果比较好，再训练loss上升

并且，finetune的结果好一点

### 2.2 model

依赖：transformer.GPT2Tokenizer, GOT2LMHeadModel

编码解码分别是：CLIP和GPT2，使用MLP(multilayer perceptiron)映射

### 2.2.1 ==使用MLP构建mapper==

MLP构建：class MLP ---> calss BertMapper

MLP全部使用前向传播的线性函数，堆叠nn.Linear

1. forward

   



## 3.资源消耗

训练环境：

训练数据集：flickr30k 训练数据集4.14GB

模型大小：GPT-2: 420.9MB ; ViT-B : 340.9MB



## 0714 train+test

train: MLP+gpt2-tuning——也就是文章的方法



inference时需要只使用GPU

top10筛选10个出来进行评分的效果里总有一个能用

今天的结果是：

对图片的描述不知道为什么有好多量词，有可能是因为英文转义上，也有可能是语料库分词的时候就有问题

