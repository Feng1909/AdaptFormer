# AdaptFormer

论文地址：https://arxiv.org/abs/2205.13535  
项目地址：https://aistudio.baidu.com/aistudio/projectdetail/4211700?contributionType=1

## 简介：

港大，腾讯AI实验室，港中文贡献的文章：AdaptFormer: Adapting Vision Transformers for Scalable Visual Recognition. 研究人员认为最新的transformer文章做的是**same network with task-specific weight**工作，用的是同样的网络，但是对每个下游任务都要fine-tune模型，这样的模型是不可拓展的，每搞一个数据集就要在上边fully finetune, 尤其是现在像ViT-G/14这样有18亿参数的大模型，训练时的算力和存储负担很重。所以他们要搞**same network with almost same weights**, 不仅网络要一样，应用到下游任务，权重也尽可能一样。只需要训练很少的参数，其他大部分参数是固定的，这些固定的参数就可以跨任务共享。

要做这件事需要构建一种高效的pileline去适配预训练模型到许多下游任务，他们的工作更像**VPT** (Visual Prompt Tuning)，VPT在patch embedding那里增加可学习的参数同时冻结整个主干只finetuen embedding部分，但本项目所作的工作能够大大超越VPT，如下图所示：

![](https://ai-studio-static-online.cdn.bcebos.com/f76529af9d20462d8f0ba44d48a15da578875da0808a47adb2c22863b4905f17)

**AdaptFormer**方法在SSv2数据集上**全面打败**了**VPT**

本文的方法和VPT**不同**的地方在于，**AdaptFormer**是加到Transformer的**MHSA**(multi-head self-attention layer)上：

![](https://ai-studio-static-online.cdn.bcebos.com/c2c110235c8549cca3a79e92c774440a458afbca68f841b6902881da4c5fde38)


下图为在各种数据集上本方法与VPT等方法的对比：

![](https://ai-studio-static-online.cdn.bcebos.com/e967f3621daf4ca8853f72f5e4e584ef19bc846aa5e84118aabdce340ad4a934)


最后文章希望可以激励更多研究者探索更加高效的fine-tuning方法到大型视觉模型上。

## 数据集介绍：Cifar100

链接：http://www.cs.toronto.edu/~kriz/cifar.html

![](https://ai-studio-static-online.cdn.bcebos.com/7baff8d84907478294ca778385fca688741d14db13d841c39f0125c41f0a0ac4)


CIFAR100数据集有100个类。每个类有600张大小为32 × 32 32\times 3232×32的彩色图像，其中500张作为训练集，100张作为测试集。

## 数据集介绍：Cifar10

链接：http://www.cs.toronto.edu/~kriz/cifar.html

![](https://ai-studio-static-online.cdn.bcebos.com/15a8790a113d41418d6fc8563aeb4acd10da73b4b8c6488599fa9e7a01cc0833)

**CIFAR-10**是一个更接近普适物体的彩色图像数据集。CIFAR-10 是由Hinton 的学生Alex Krizhevsky 和Ilya Sutskever 整理的一个用于识别普适物体的小型数据集。一共包含10 个类别的RGB彩色图片：**飞机**(airplane)、**汽车**(automobile)、**鸟类**(bird)、**猫**(cat)、**鹿**(deer)、**狗**(dog)、**蛙类**(frog)、马(horse)、**船**(ship)和**卡车**(truck).

每个图片的尺寸为 $32\times 32$，每个类别有**6000**个图像，数据集中一共有**50000**张训练图片和**10000**张测试图片。
