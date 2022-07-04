# ClipCap: image Captioning



工程环境：torch1.11

[clipclap chinese](https://github.com/yangjianxin1/ClipCap-Chinese)

## 1. 大致思路

encoder: CLIP 编码得到图片向量

映射： MLP Mapping clip_embed -> fc+fc -> prefic_embeds

decoder: 用了GPT2模型[GPT2通用模型]

gpt2使用了Transformer[vit-B]

 ## 2. 具体实现

### 2.1  preprocess

修改代码内容：15行添加`encodind="utf-8"` 后面那个不用改

实现内容：读取flickr30数据集id和标注caption-> encdoing为embedding

*pickle*: 通过pickle模块的序列化操作我们能够将程序中运行的对象信息保存到文件中去，永久存储；通过pickle模块的反序列化操作，我们能够从文件中创建上一次程序保存的对象。【读取序列信息，写入文件】

