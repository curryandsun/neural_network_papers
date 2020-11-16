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
- [Fine-grained](#fine-grained)

# Fine-grained

> 细粒度识别。

- [Pairwiseconfusion for fine-grained visual classification](https://arxiv.org/abs/1705.08016) (ECCV2018)
- [natural world distribution via adaptive confusion energy regularization](https://openreview.net/forum?id=kKwFlM32HV5) (ICLR2021 under review)
    - 3分
    - motivation:两篇文章的出发点非常类似，只不过后者考虑了Long-tail的情况。在细粒度识别中，类间本身就非常相似，这就导致网络的输入非常相似，但是输出却是不同的标签，容易使得网络过于关注样本级别的特征，造成过拟合。因此提出了PairwiseConfusion模块，混淆pair中两个输出的概率分布。
    - 个人感觉这里的PairwiseConfusion方法和motivation的关联并不是那么紧密，反而更像是一种label smooth。只不过label smooth中以均匀分布作为一个正则化项，而这个方法使用pair中另一个分布的输出作为正则化项，但本质都是通过防止网络overconfident来抑制过拟合的情况。

- [Fine-Grained Visual Classification viaProgressive Multi-Granularity Training ofJigsaw Patches](https://arxiv.org/abs/2003.03836) (ECCV2020)
    - 3分
    - motivation:we further argue that, one not only needs to identify parts and their most discriminative granularities, but meanwhile how parts at different granularities can be effectively merged.
    - 文章的表述有些复杂，总而言之就是通过各种方法使得各种粒度的特征做融合。