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
- [Knowledge Distilling](#knowledge-distilling)
  - [Distilling From Logits](#distilling-from-logits)
  - [Distilling From Features](#distilling-from-features)
  - [Self-KD](#self-KD)

# Knowledge Distilling

> 知识蒸馏。

## Distilling From Logits

> Logits层面的蒸馏。

- [Revisiting Knowledge Distillation via Label Smoothing Regularization](https://arxiv.org/abs/1909.11723) (CVPR2020)
    - 5分
    - motivation:在实验中发现两点不符合常理的现象:1.student网络也能指导teacher网络。2.准确率很低的teacher网络也能指导student网络。
    - 非常清晰地分析了KD和label smooth之间的关系。两者形式上本身就非常相似，label smooth可以看成是人工设计的一直输出均匀分布的virtual teacher，同理KD也可以看成是更强的可学习的label smooth。
    - 由此提出了TF-KD，一种是直接拿pretrain的student网络当成teacher，一种是将soft label加上temperature。
    - 文章思路非常清晰，idea也很好，但是实验也并不能表明TF-KD就比LSR好多少，也没有去探究normal-KD比TF-KD能好在哪。

- [Distilling the Knowledge in a Neural Network](https://arxiv.org/abs/1503.02531) (NIPS2014)
    - 5分
    - Hinton出品，知识蒸馏开山之作。
    - 属于logit级别的知识蒸馏。student网络一方面用带温度的softmax去学习teacher网络的分布，一方面用hard的softmax去学习真实标签。
    - teacher网络很大很复杂，他的soft标签可以包含一些类之间的关系，而这些是hard标签所没有的，因此可以把这部分看作其学到的“知识”，再交由student小网络进行学习。

## Distilling From Features

> Features或Relations层面的蒸馏。

- [Similarity-Preserving Knowledge Distillation](https://arxiv.org/abs/1907.09682) (ICCV2019)
- [A Gift from Knowledge Distillation:Fast Optimization, Network Minimization and Transfer Learning](https://openaccess.thecvf.com/content_cvpr_2017/papers/Yim_A_Gift_From_CVPR_2017_paper.pdf) (CVPR2017)

    - 4分
    - 两篇文章都是从feature map的角度进行蒸馏。
    - 第一篇由浅层feature map(b\*c\*h\*w)生成一个表示batch内样本间相似度的相似矩阵(b\*b)，对这个矩阵进行蒸馏。蒸馏的信息：batch内样本的相似度。
    - 第二篇由一个样本两个不同层的feature map(c1\*h\*w和c2\*h\*w)生成FSP(c1\*c2),对其进行蒸馏。FSP：The extracted feature maps from two layers are used to generate the flow of solution procedure(FSP) matrix。即蒸馏不同layer间feature map的flow信息。

## Self-KD

> 自蒸馏。

- [Regularizing Class-wise Predictions via Self-knowledge Distillation](https://arxiv.org/abs/2003.13964) (CVPR2020)
    - 3分
    - self-kd,或可以理解为正则化。流程非常简单，取标签相同的两个batch，对其输出概率求KL散度作为正则项损失，即要求标签相同的输入拥有更加相近的输出概率分布。
    - 可以理解为更加真实的label smooth，因为是把相同标签的样本的输出看成soft label。可以说和Deep mutual learning结构几乎完全相同，只不过是使用了同一个网络以及pair的输入。另一方面和PairwiseConfusion完全相反。
    - 不得不感叹2020年了，还能这样水一篇CVPR。

- [Be Your Own Teacher: Improve the Performance of Convolutional Neural Networks via Self Distillation](https://arxiv.org/abs/1905.08094) (ICCV2019)
- [MSD: Multi-Self-Distillation Learning via Multi-classifiers within Deep Neural Networks](https://arxiv.org/abs/1911.09418) (arXiv2019)
    - 4分
    - 这两篇非常类似，都属于self-kd。
    - 要求每个浅层模型都具有独立判别的能力，且与深层模型接近。三项loss可以分别看成是：1.深度监督(DSN) 2.深层模型对浅层模型的输出分布监督 3.深层模型对浅层模型的feature监督。
    - 对后两项是否确实有实际意义表示存疑。
    - 后一篇实验效果远好于第一篇，但论文中读不出具体的改进。

- [Deep mutual learning](https://arxiv.org/abs/1706.00384) (CVPR2018)
    - 3分
    - 两个网络mutual的训练，将输出分布求KL散度作为Loss的一项。
    - 目标是两个网络之间相互指导训练(互相趋同)，最后两个网络都可以拿出来用。Long-tail问题中多专家框架和这个很像，但是是将多专家输出分布的KL散度之间最大化(互相不同)，然后集成起来用。
    - 一个模型学到的概率分布可以看作是模型学到了数据内部的一个本质规律，就可以用来指导另外一个模型。也可以看作一个正则化的手段，类似于一种更加贴合数据本身规律的label smooth。


