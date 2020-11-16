# 论文阅读笔记

**记录一些读过的论文，给出个人对论文的评分情况并简述论文insight**

**评分细节：** 

- 5分：值得反复阅读的论文，非常具有启发性

- 4分：值得精读的论文，具有一定的启发性

- 3分：值得粗略阅读的论文，了解论文大概思想后会有一定收获

- 2分：价值不大的论文，读完基本没有收获

- 1分：粗制滥造的论文

> 论文将按照年份/评分依次排序，绝大多数论文将会给出[arXiv](https://arxiv.org/)的链接.


# Table of contents

- [论文阅读笔记](#论文阅读笔记)
- [Table of Contents](#table-of-contents)
- [Label Smooth](#label-smooth)

# Label Smooth

> label smooth于16年提出，本单元包括后续一系列应用以及解释(包括 calibration)的相关文章。

- [When Does Label Smoothing Help](https://arxiv.org/abs/1906.02629) (NIPS2019)
    - 5分
    - Hinton出品，从三个方面讲label smooth的作用。
    - 一是label smooth可以让正确类的特征tighter，且类template到其余类的距离会更加趋同。
    - 二是从calibration的角度说明了label smooth的作用。
    - 三是说明了虽然label smooth可以提高模型精度，但这样的模型作为知识蒸馏的teacher模型却是不好的，因为label smooth损失了类之间的相关性，而这正是知识蒸馏所需要的。

- [On Calibration of Modern Neural Networks](https://arxiv.org/abs/1706.04599) (ICML2017)
    - 4分
    - 文中指出了虽然近年来nn的acc不断提高，但是calibration却是在降低的(如Resnet)，也就是说模型最后输出的confidence并不能真的代表置信度。
    - 首先给出了一个利用柱状图思想来计算ECE的方法，可以衡量模型的calibration程度。实验表明，增加模型宽度/深度，BN层以及weight decay都会降低模型的calibration。
    - 其次在platt scaling的基础上，提出了temperature scaling方法，可以不改变测试acc。两者都是在训练集上训练后，固定模型参数再通过验证集来训练后处理方法的参数。

- [Regularizing Neural Networks By Penalizing Confident Output Distributions](https://arxiv.org/abs/1701.06548) (ICLR2017)
    - 3分
    - Hinton出品，介绍了一种神经网络Loss的正则项，具体来说就是在CE之后加了一项关于输出分布的负熵的惩罚项。输出分布某项的置信度越高，则熵越低，负熵越高，因此模型将更不容易overconfident。最后说明了uniform的label smooth也是在干类似的事情。
    - 但其实16年提出label smooth的文章里就已经提到了其与利用熵的正则化是相似的，但是没有进行实验。因此就只是干了一遍前人提出来了没干的事情?

- [Rethinking the Inception Architecture for Computer Vision](https://arxiv.org/abs/1512.00567) (CVPR2016)
    - 4分
    - label smooth诞生地。本文提出了Inception V2,其中label smooth只是论文的一小部分，做了一些经验上地分析，ICLR2017 Hinton文章里的很多思想在其中已经有所提及。在本地跑label smooth时，resnet18_cifar10可以提升0.4,resnet18_cifar100可以提升两个点。
