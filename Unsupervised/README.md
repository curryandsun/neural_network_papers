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
- [Unsupervised](#unsupervised)

# Unsupervised

> 无监督学习(包括自监督学习,对比学习)。

- [Momentum Contrast for Unsupervised Visual Representation Learning](https://arxiv.org/abs/1911.05722) (CVPR2020_oral)
    - 5分
    - KaiMing He出品。指出之前的办法要么不够large，要么不够consistent。算法主要包括以下几点:
        - 采用了queue的方式构建负样本dictionary，使得其size可以大大超过mini-batch的size。
        - 对于f_k的参数，采用了momentum的变化方式，使得f_q的参数可以更平滑地影响f_k。以上两点也就算MoCo算法的核心思想:large and consistent。
        - shuffle batch normalization:multi-GPU的设置下，对每一个mini-batch，shuffle f_k的samples order，而f_q的保持不变，这样就保证了每个GPU上的两个encoder的subset是不同的。

- [A Simple Framework for Contrastive Learning of Visual Representations](https://arxiv.org/abs/2002.05709) (ICML2020)
    - 4分
    - Hinton出品。算法主要包括以下几点:
        - Simple体现在抛弃了memory bank等各种人工设计的模块，采用了最基本最简洁的contrastive learning框架。可以这么simple的前提在于要加大batchsize来提高负样本的采样数目，文中使用了恐怖的8096。
        - 探究了data augmentation的重要性。
        - 将常用的linear的head转化为具有激活函数的MLP。

- [Unsupervised Deep Learning by Neighbourhood Discovery](https://arxiv.org/abs/1904.11567) (ICML2019)
    - 4分
    - movitation:NPID方法，将每个样本都作为单独类，也就强制分开了本来是同类的样本，一定程度上失去了class consistency。而DeepCluster方法则是非常全局的做了一个聚类。而本文中的ND方法相当于是二者的一个中和，希望从局部(文中的AN,AN中设置label相同)选取正pair。
    - 认为低熵样本(只有少量样本与其相似)更能反映class consistency，从而只用他们选取AN。而其他样本在本轮中则仍然单独当作一个类。
    - 本文提到的consistency与MoCo方法是不同的，前者是从空间的角度，相同类的伪标签应该也是相同的。而后者是从时间的角度，不断变化的encoder_k对相同样本的结果应该是相同或类似的。

- [Local aggregation for unsupervised learning of visual embeddings](https://arxiv.org/abs/1903.12355) (ICCV2019)
    - 4分
    - Memory Bank中的采样是随机的，显然没有充分利用其中信息。本文中作者定义了两个集合Bi和Ci，可以简单理解为Bi是Memory Bank中离目标特征vi较近的集合，而Ci是更近的集合。文中Bi从V中选取了k个最近点，而Ci则是通过聚类得到。
    - 正样本选自Bi^Ci，负样本则是选自Bi-Bi^Ci。相当于从V中选择了价值更大的进行Loss计算。
    - 可以看成是DeepCluster和NPID方法的结合。
    - 仍局限于Memory Bank，但是提供了一种采样的新思路，即采样时不应该是随机采样，而应该从与vi更相近的点中采样。


- [Unsupervised Embedding Learning via Invariant and Spreading Instance Feature](https://arxiv.org/abs/1904.03436) (CVPR2019)
    - 4分
    - NPID方法用到了Memory Bank,使得t时刻encoder得到的query只能和t-1时刻得到的dictionary来构建正负pair，再进行Loss计算，这样显然效果不够好。
    - 该方法用augmentation的方法构建正pair，用mini-batch中的其他样本构建负pair，实现了真正的instance-level的对比学习。

- [Unsupervised Feature Learning via Non-Parametric Instance Discrimination](https://arxiv.org/abs/1805.01978) (CVPR2018_spotlight)
    - 5分
    - motivation:尽管监督学习的目标只是分类，但模型可以隐式地学习到类别之间的相似度。那么若每一个样本都有一个标签，用instance-wise代替class-wise，这样也可以学到样本的特征表示，同样隐含着类别的相似度信息。
    - class-wise转为instance-wise后不能简单应用CE(类别太多)，因此采用了NCE和Memory Bank。
    - 模型学到特征后可以直接用KNN分类，并且模型参数也可以作为pretrain移植到别的任务上去，这样就类似于半监督学习。
    - 可以说是Instance Discrimination这一类方法的开山之作。

- [Deep Clustering for Unsupervised Learning of Visual Features](https://arxiv.org/abs/1807.05520) (ECCV2018)
    - 4分
    - DeepCluster方法，思路很简单:将通过CNN后的特征进行聚类，再用聚类的结果作为伪标签反向传播到CNN。