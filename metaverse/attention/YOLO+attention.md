#### x!   **compare to orignial YOLO?——一点总结**



##### (1) YOLO:基本思想

1. yolov1:

   1. 划分输入图像为$$S \times S$$ ，图像中心点落在哪一个gird cellL里面，哪一个gird cell就负责预测；

   2. **每个** gird cell都预测 B 个 bouding box 并给出得分，代表置信度

   计算方式：$$\operatorname{Pr}(\text { Object }) * \mathrm{IOU}_{\text {pred }}^{\text {truth }}$$

   3. 每个box预测负责的区域C个类别分别给出得分，+给分的内容包括4个坐标信息（x,y,）中心点，(w,h )+ ——相对全图的尺寸【2里面做的事偏移量】置信度

   因此，给出的prediction应该一共有：$$S \times S \times (B * 5 + C)$$

2. yolov2+v3——bounding box regression

   1. v2使用维度聚类生成中心点，然后给每个检测狂预测4个量$$t_x,t_y,t_w,t_h$$

      <img src="https://raw.githubusercontent.com/ChangYaQi/notes/main/metaverse/assets/image-20220712101322396.png" alt="image-20220712101322396" style="zoom:50%;" />

   2. darknet19-darjnet53

   3. dark53：conv+res 堆叠 -> avgpool -> fc \> softmax

   4. 给出的预测tensor是$$S \times S \times (3 * 5 + C)$$

==attention based 预测==: 可以看 DETR

为什么要用Transformer?(CNN attention)

##### (2)给YOLO网络加SElayer/CBAM双通道注意力机制的方法