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
    - 最终GCN的aggregation相当于是自身结点与邻居结点的加权和，这个权重与自身和邻居的度有关，度越大则权重越小。

- [Inductive Representation Learning on Large Graphs](https://arxiv.org/abs/1706.02216) (NIPS2017)
    - 5分
    - transductive(GCN) -> inductive(GraphSAGE)
    - 简而言之，GCN是学每个结点固定的表示，而GraphSAGE是学K个aggregation函数，因此对于测试中新出现的结点，只需要通过其邻居对其做aggregation即可，而不用重新训练整个网络。
    - Aggregate and Combine:instead of aggregating a node and its neighbors at the same time,GraphSAGE aggregates the neighbors first and then combine the resulting neighborhood representation with the node’s representation.The COMBINE step is key to this paradigm and can be viewed as a form of a ”skip connection” between different layers.

- [Graph Attention Networks](https://arxiv.org/abs/1710.10903) (ICLR2018)
    - 5分
    - GAT:weighted mean aggregation + self-attention。
    - GCN的aggregation相当于加权平均，但这个权重来自于整个图的信息，显然计算量大，且是transductive的。而GraphSAGE的mean aggregation直接取平均。于是作者想到了可以用self-attention的方法学得一个权重来进行加权平均。

## Over-smooth

> GCN中的过平滑问题。

- [DropEdge: Towards Deep Graph Convolutional Networks on Node Classification](https://openreview.net/pdf?id=Hkx1qkrKPr) (ICLR2020)
    - 3分
    - DropEdge to alleviate over-fitting and over-smoothing.
    - code中具体实现DropEdge时邻接矩阵竟然变成不对称的了，风中凌乱...

- [Deeper Insights into Graph Convolutional Networks for Semi-Supervised Learning](https://arxiv.org/abs/1801.07606) (AAAI2018_oral)
    - 5分
    - GCN每一层都在做aggregation and transformation.因此GCN和FCN的唯一区别就在于先做了一次aggregation。文中分析aggregation就是特殊的Laplacian smoothing。
    - The Laplacian smoothing computes the new features of a vertex as the weighted average of itself and its neighbors’.Since vertices in the same cluster tend to be densely connected, the smoothing makes  their features similar.于是过多次的aggregation就会导致over-smooth，即不同类的结点特征也混淆在一起了。 

- [Representation Learning on Graphs with Jumping Knowledge Networks](https://arxiv.org/abs/1806.03536) (ICML2018_oral)
    - 4分
    - Motivated by observations that reveal great differences in neighborhood information ranges for graph node embeddings, we propose a new aggregation scheme for node representation learning that can adapt neigborhood ranges to nodes individually.
    - 做法很简单，将结点1到k阶aggregation的结果做concat等操作，得到更丰富的聚合特征。

## Others

> 其它。

- [Measuring and Improving the Use of Graph Information in Graph Neural Networks](https://openreview.net/pdf/3ff628aed23920c95386567ad7acc7885d49b122.pdf) (ICLR2020)
- [Measuring and Relieving the Over-smoothing Problem for Graph Neural Networks from the Topological View](https://arxiv.org/abs/1909.03211) (AAAI2020)
    - 4分
    - 前者提出了两个图本身的指标:λf衡量邻居结点与自身的feature差异性(information gain);λl衡量邻居结点与自身的label差异性(information noise)。显然，一个图若有更大的λf和更小的λl，则更利于aggregation。后者也提出了非常类似的指标。
    - 基于上述两个指标前者提出了一个类似GAT的网络，两点不同比较有意思:
        - 1.attention系数排名小于2|E|λl的全部置0，相当于认为其是与不同label结点相连的噪声信息。
        - 2.λf用来设置hidden layer的维度，显然λf越大，则需要更大的维度来保证information gain。
    - 后者通过伪标签将不同类的边去去掉,将同类的边加上,是个非常自然的想法。

- [Graph Random Neural Network for Semi-Supervised Learning on Graphs](https://arxiv.org/abs/2005.11079) (NIPS2020)
    - 4分
    - 将MixMatch的做法用到GCN。
    - aug用的是直接随机将X矩阵某些行置0，也就是随机将一些结点的特征向量全部置0。
    - 将aggregation and transformation解耦:aug s次得到s个不同的X，每一个X都先做一次K阶平均的aggregation，再接一个MLP做transformation。得到输出后，s个输出分布取平均再sharpen作为伪标签。

    

