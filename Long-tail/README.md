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
- [Long-tail](#long-tail)
    - [Reweight](#reweight)
    - [Decoupling](#decoupling)
    - [Feature Augmentation](#feature-augmentation)
    - [Others](#others)

# Long-tail

> 长尾分布问题，主要集中于图像分类。

## Reweight

> Reweight loss,通常是在大类和小类间做一个trade-off。

- [Long-Tail Learning via Logit Adjustment](https://arxiv.org/abs/2007.07314) (ICLR2021 under review)
    - 5分
    - 这篇文章的related work写的很不错，非常清晰地涵盖了最近研究长尾分布的主要论文。
    - 文中通过对平衡后验概率的建模，提出了新的后处理方法以及Loss调整方法。motivation非常自然，可以理解成LDAM考虑了pair-wise margin的Loss，且实验效果也不错。与我们最近的一个idea非常相似，很难受。

- [Learning imbalanced datasets with label-distribution-aware margin loss](https://arxiv.org/abs/1906.07413) (NIPS2019)
    - 4分
    - 从margin的角度提出了LDAMLoss，还有DRW的训练方法，有一些理论分析，被后续大量工作作为baseline。
    - 关于DRW：按照样本逆频率reweight Loss是一种常见的做法，目的是让每一类的Loss是均衡的，但这种做法往往只在训练初期管用。因为当你reweight每一类的Loss之后，小类的样本会获得更大的Loss，经过一轮的训练以后，其Loss会降得更低，重新使得各类Loss回归到不平衡的状态。
    - 因此DRW这个方法可以看成是一种刺激网络突然更加关注小类的做法，往往在具体实验中呈现出acc突然上升然后慢慢回落的现象。DRW算是一种非常tricky的方法，强行在后期做一个刺激来提高acc。

- [Class-balanced loss based on effective number of samples](https://arxiv.org/abs/1901.05555) (CVPR2019)
    - 4分
    - 文中提出在采样时会出现一些特征重复的现象，因此样本数并不能完全代表采样的覆盖情况。作者通过一些假设，简单推倒出一个有效样本数的概念，并以此代替频率对Loss进行修正。思想非常简单清晰，被后续大量工作作为baseline。


## Decoupling

> 训练特征阶段instance-balanced，训练分类器阶段class-balanced。

- [Decoupling Representation and Classifier for Long-Tailed Recognition](https://arxiv.org/abs/1910.09217) (ICLR2020)
    - 5分
    - 应该是第一次将nn中Long-tail问题分为特征提取和分类两部分。实验表明即使是在Long-tail的数据集中，nn依然可以学到良好的特征表示，但需要对分类器W做后处理。文中根据实验现象提出了W对每一类进行L2Norm的方法。

- [BBN: Bilateral-Branch Network with Cumulative Learningfor Long-Tailed Visual Recognition](https://arxiv.org/abs/1912.02413) (CVPR2020_oral)
    - 4分
    - 提出了BBN网络结构来解决Long-tail问题，本质思想与Decoupling类似，希望特征学习阶段是不平衡的而分类器学习阶段是平衡的。
	- 实验效果非常优秀，但是实际上在对比之前的方法时并不算公平，因为网络多了近乎一倍的参数量，且每个batch采样也是两倍。

## Feature Augmentation

> 在特征空间上对小类进行增广。

- [Deep Representation Learning on Long-tailed Data: A Learnable EmbeddingAugmentation Perspective](https://arxiv.org/abs/2002.10826) (CVPR2020)
- [Memory-based Jitter: Improving Visual Recognition on Long-tailed Data with Diversity In Memory](https://arxiv.org/abs/2008.09809) (arXiv2020)
    - 3分
    - [知乎原作者解析](https://zhuanlan.zhihu.com/p/112248291)
    - 以上两篇出自同一作者，本质思想类似:都是希望从特征空间层面来扩充tail类的表示。
    - 第一篇是从head类中学到一个分布再扩充到tail类；第二篇应该是受到MoCo的启发，同样存储一个特征的memory queue，但是这里的特征是带标签的，并且做了一个反频率采样，再把memory queue中的特征扩充到所有类的特征空间中，使得在特征空间中类数目比较均匀。

## Others

> 包括group和ensemble的方法。

- [Long-tailed Recognition by Routing Diverse Distribution-Aware Experts](https://arxiv.org/abs/2010.01809) (ICLR2021 under review)
    - 4分
    - 集成学习的思想。stage1学experts的参数，两个Loss要求多个experts好而不同(每个experts可以关注不同的类)。stage2学对每个instance具体的experts的选择方法。
    - 为了降低模型复杂度：网络中所有channel变为四分之一，网络前一部分参数共享，stage2专家分配。
    - 实验效果好到离谱，同时提升head类和tail类，且各大任务都能提升6%左右。

- [Learning From Multiple Experts: Self-paced Knowledge Distillation for Long-tailed Classification](https://arxiv.org/abs/2001.01536) (ECCV2020_spotlight)
    - 4分
    - motivation:如果把Long-tail数据集分为多个子集，那么在每个子集中Long-tail的程度就会减缓，分开训练后的精度就会提高。
    - 因此，首先在这些子集上训练出来多个专家。这些专家可以起到两个作用:1.通过置信度来告诉student网络每个样本的难度，也就对应文中的课程学习模块。2.对student网络进行知识蒸馏。

- [Rethinking the Value of Labels for Improving Class-Imbalanced Learning](https://arxiv.org/abs/2006.07529) (NIPS2020)
    - 3分
    - [知乎原作者解析](https://zhuanlan.zhihu.com/p/259710601)
	- 其中半监督学习框架引入了其他的unlabeled data，文中也讨论了如何选取这些unlabeled data。
	- 利用自监督对长尾数据集预训练，再使用其他任何的长尾学习算法。





