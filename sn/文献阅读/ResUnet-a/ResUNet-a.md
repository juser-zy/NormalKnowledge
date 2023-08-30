### Title
ResUNet-a: a deep learning framework for semantic segmentation of remotely sensed data

### Abstract
- Our framework consists of a novel deep learning architecture, ResUNet-a, and a novel loss function based on the Dice loss.
- ResUNet-a uses a UNet encoder/decoder backbone, in combination with residual connections, atrous convolutions, pyramid scene parsing pooling and multi-tasking inference.

### Introduction
1. A novel architecture for understanding and labeling very high resolution images for the task of semantic segmentation. 
	1. The architecture uses a UNet (Ronneberger et al.,2015) encoder/decoder backbone, in combination with
	2. residual connections (He et al., 2016), 
	3. atrous convolutions (Chen et al., 2016, 2017)
	4. pyramid scene parsingpooling (Zhao et al., 2017a) 
	5. multi tasking inference(Ruder, 2017
		we present two variants of the basic architecture, a single task and a multi-task one).
2. We analyze the performance of various flavours of the Dice coefficient for semantic segmentation. Based on our findings, we introduce a variant of the Dice loss function that speeds up the convergence of semantic segmentation tasks and improves performance. 
3. In addition, we also present a data augmentation methodology

### Related Work
- This is achieved through the downsampling operations that summarizes features.


### The ResUNet-a framework
In this section, we introduce the architecture of ResUNet-a in full detail (section 3.1)
a novel loss function design to achieve faster convergence and higher performance (section 3.2)
data augmentation methodology (section 3.3) as well as the methodology we followed on performing inference on large images
(section 3.4). The training strategy and software implementation characteristics can be found in Appendix A.

1. UNet 骨架
2. Residual blocks
3. 空洞卷积parallel atrous convolutions
4. the pyramid scene parsing pooling
5. 多任务处理

框架处理

![](images/Pasted%20image%2020230705212715.png)
![](images/Pasted%20image%2020230705212725.png)

