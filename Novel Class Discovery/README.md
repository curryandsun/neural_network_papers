# Table of contents

- [Table of Contents](#table-of-contents)
- [Novel Class Discovery](#novel-class-discovery)

# Novel Class Discovery

> NCD is similar but different from semi-supervised learning, because, in
the latter, the working assumption is that labeled and unlabeled sets share the same classes. Differently, in NCD, the two sets of classes are supposed to be disjoint. Moreover, differently from common clustering, scenarios, in an NCD framework, labeled data can be utilized at training time, and the challenge consists in transferring the supervised knowledge on the known classes to improve clustering of the unknown ones.

> 个人觉得NCD这个setting存在两个问题：1.现实中labeled set和unlabeled set不一定是disjoint的。2.现实中难以得知unlabeled set中的类别数目。

- [A Unified Objective for Novel Class Discovery](https://arxiv.org/abs/2108.08536) (ICCV2021_oral)
    - 4分
    - movitation在于将NCD问题中常见的多个训练目标进行统一。unlabel的部分与SWaV几乎一致，所谓的unified objective就是把label和unlabel的两组logits concat到一起。

- [Automatically Discovering and Learning New Visual Categories with Ranking Statistics](https://arxiv.org/abs/2002.05714) (ICLR2020)
    - 4分
    - 论文的novelty在于结合self-supervised learning以及自己提出的一个Ranking Statistics。训练可分为三步：
        - 1.全部数据上用RotNet预训练low-level features。
        - 2.冻结网络前面几层，用label data进行finetune。
        - 3.对于unlabel data，用Ranking Statistics得到相似矩阵，再用BCE进行训练。


