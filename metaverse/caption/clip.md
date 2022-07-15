[Vedio CLIP代码地址](https://github.com/pytorch/fairseq/tree/main/examples/MMPT)

粒度特征： 

sequence length

现存问题：视频和文本对，现在的模型使用精确的匹配训练的

解决方法；1. overlapped video and text clips ????? 2. 提供相似模型

基本思路：

假设有连续帧序列clip $$c_v$$

输入参数冻结的预训练视频编码器$$f_\theta$$

[VEDIO CLIP zhihu](https://zhuanlan.zhihu.com/p/423553168)

[视频摘要生成综述](https://blog.csdn.net/qq_39388410/article/details/114760719)

[知乎大合集](https://zhuanlan.zhihu.com/p/77947067)
