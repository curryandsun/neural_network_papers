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
- [GCN](#gcn)
    - [Basic](#basic)
    - [Over-smooth](#over-smooth)
    - [Others](#others)

# GCN

> 图卷积神经网络。

## Basic

> GCN基础。

- [Semi-Supervised Classification with Graph Convolutional Networks](https://arxiv.org/abs/1609.02907) (ICLR2017)
    - 5分
    - GCN开山之作。
    - [知乎上一篇很好的解析](https://zhuanlan.zhihu.com/p/120311352)

## Over-smooth

> GCN中的过平滑问题。

- [Deeper Insights into Graph Convolutional Networks for Semi-Supervised Learning](https://arxiv.org/abs/1801.07606) (AAAI2018_oral)
    - 5分
    - GCN每一层都在做propagation and transformation.因此GCN和FCN的唯一区别就在于先做了一次propagation。文中分析propagation就是特殊的Laplacian smoothing。
    - The Laplacian smoothing computes the new features of a vertex as the weighted average of itself and its neighbors’.Since vertices in the same cluster tend to be densely connected, the smoothing makes  their features similar.于是过多次的propagation就会导致over-smooth，即不同类的结点特征也混淆在一起了。 

## Others

> 其它。

- [Graph Random Neural Network for Semi-Supervised Learning on Graphs](https://arxiv.org/abs/2005.11079) (NIPS2020)
    - 4分
    - 将MixMatch的做法用到GCN。
    - aug用的是直接随机将X矩阵某些行置0，也就是随机将一些结点的特征向量全部置0。
    - 将propagation and transformation解耦:aug s次得到s个不同的X，每一个X都先做一次K阶平均的propagation，再接一个MLP做transformation。得到输出后，s个输出分布取平均再sharpen作为伪标签。

