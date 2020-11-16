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
- [Neural Network](#neural-network)

# Neural Network

> 有关神经网络的各类杂文，如可解释性,tricks等。

- [Bag of Tricks for Image Classification with Convolutional Neural Networks](https://arxiv.org/abs/1812.01187) (CVPR2019)
    - 4分
    - 记录了应用nn进行图像分类的各种tricks，第五章介绍的mixup,label smooth,知识蒸馏等都是非常有用的技巧。

- [mixup: BEYOND EMPIRICAL RISK MINIMIZATION](https://arxiv.org/abs/1710.09412) (ICLR2018)
    - 5分
    - mixup，实质上是给模型加上了一个线性系统的先验，实验效果非常显著。在本地跑mixup时，resnet18_cifar10可以提升一个点,resnet18_cifar100可以提升三个点。

- [Understanding deep learningrequires rethinking generalization](https://arxiv.org/abs/1611.03530) (ICLR2017_Best paper)
    - 4分
    - 实验证明了无论是输入扰动还是标签扰动，神经网络都可以百分百的拟合训练数据。那么核心问题来了，明明网络只需要暴力地去记住训练集就好，为什么实际上却有着强大的泛化能力呢?
    - 1.正则化:对泛化能力有提升，但是并不显著。
    - 2.有限样本情况下，两层Relu神经网络的表达能力。
    - 3.SGD隐式地提高了泛化能力。
    - 总的来说，文章最终也没有解释为什么，但是给人们一些新的角度重新去思考这个问题。