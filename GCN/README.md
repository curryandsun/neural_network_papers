# Table of contents

- [Table of Contents](#table-of-contents)
- [GCN](#gcn)
    - [Basic](#basic)
    - [Over-smooth](#over-smooth)
    - [Better Aggregation](#better-aggregation)
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
    - transductive(训练过程中val和test都是可见的:GCN) -> inductive(训练过程中val和test都是unseen的,只有train set的结点和边:GraphSAGE)
    - 简而言之，GCN是学每个结点固定的表示，而GraphSAGE是学K个aggregation函数。因此对于测试中的新结点或新图，只需要通过其邻居对其做aggregation即可，而不用重新训练整个网络。
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
    - 5分
    - Motivated by observations that reveal great differences in neighborhood information ranges for graph node embeddings, we propose a new aggregation scheme for node representation learning that can adapt neigborhood ranges to nodes individually.
    - JKnet uses dense skip connections to combine the node features of each layer to preserve the locality of the node representations.

## Better Aggregation

> 探索更好的聚合方式。

- [Measuring and Improving the Use of Graph Information in Graph Neural Networks](https://openreview.net/pdf/3ff628aed23920c95386567ad7acc7885d49b122.pdf) (ICLR2020)
- [Measuring and Relieving the Over-smoothing Problem for Graph Neural Networks from the Topological View](https://arxiv.org/abs/1909.03211) (AAAI2020)
    - 4分
    - 前者提出了两个图本身的指标:λf衡量邻居结点与自身的feature差异性(information gain);λl衡量邻居结点与自身的label差异性(information noise)。显然，一个图若有更大的λf和更小的λl，则更利于aggregation。后者也提出了非常类似的指标。
    - 基于上述两个指标前者提出了一个类似GAT的网络，两点不同比较有意思:
        - 1.attention系数排名小于2|E|λl的全部置0，相当于认为其是与不同label结点相连的noise信息。
        - 2.λf用来设置hidden layer的维度，显然λf越大，则需要更大的维度来保证information gain。
    - 后者通过伪标签将不同类的边去去掉,将同类的边加上。是个非常自然的想法,但是复杂度过高。

- [When Do GNNs Work: Understanding and Improving Neighborhood Aggregation](https://www.ijcai.org/Proceedings/2020/181) (IJCAI2020)
    - 4分
    - 与上述两篇非常相似，提出了对于每个结点两个指标:一个来衡量邻居的标签差异性，一个来衡量自身与邻居的标签相似性，然后认为前者过大是harmful的，后者过小则是useless的。
    - 做法很有意思，根据这两个指标计算出每个结点每次aggregation的权重，因此可能出现不同结点aggregation次数不一样的情况，这也是文章的motivation之一，即不同的结点显然需要不同次数的aggregation。

- [Simplifying Graph Convolutional Networks](https://arxiv.org/abs/1902.07153) (ICML2019)
- [MixHop: Higher-Order Graph Convolutional Architectures via Sparsified Neighborhood Mixing](https://arxiv.org/abs/1905.00067) (ICML2019)
    - 5, 4分
    - Higher-Order aggregation。
    - 前者假设GCN中的非线性层(ReLU)意义不大，因此直接用S<sup>k</sup>X作为aggregation features。文中指出k>1时,S<sup>k</sup> act as a low-pass-type filters。notice:SGC would fail when the feature input is nonlinearly-separable because the graph convolution part does not contribute to non-linear manifold learning.这也解释了为什么在random split的情况下，SGC无法收敛。
    - 而后者类似JKnet，将S<sup>k</sup>XW<sub>k</sub>从1到k做concat,与JKnet的不同:MixHop是每次aggregation时都使用了Higher-Order的filter，而JKnet每次aggregation仍是1阶的，只是最后将每层的feature都拿出来作为最后的representation。

## Others

> 其它。

- [Graph Random Neural Network for Semi-Supervised Learning on Graphs](https://arxiv.org/abs/2005.11079) (NIPS2020)
    - 4分
    - 将MixMatch的做法用到GCN。
    - aug用的是直接随机将X矩阵某些行置0，也就是随机将一些结点的特征向量全部置0。
    - 将aggregation and transformation解耦:aug s次得到s个不同的X，每一个X都先做一次K阶平均的aggregation，再接一个MLP做transformation。得到输出后，s个输出分布取平均再sharpen作为伪标签。

- [Revisiting Graph Neural Networks:All We Have is Low-Pass Filters](https://arxiv.org/abs/1905.09550) (CoRR2019)
- [PairNorm: Tackling Oversmoothing in GNNs](https://arxiv.org/abs/1909.12223) (ICLR2020)
- [Revisiting Graph Convolutional Network on Semi-Supervised Node Classification from an Optimization Perspective](https://arxiv.org/abs/2009.11469) (arXiv2020)
    - 4分。
    - 三篇文章都包含一个相同的优化问题:graph-regularized least squares (GRLS)，从这个角度可以得到GCN是其闭式解的一阶近似。
    - 第一篇对于SGC中的loss-pass filter有更详细的解释。后两篇都考虑了不相邻结点的正则化。







    

