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
- [Papers](#papers)
  - [Neural Network](#neural-network)
  - [Label Smooth](#label-smooth)
  - [Long-tail](#long-tail)
  - [Unsupervised](#unsupervised)
  - [Semi-supervised](#semi-supervised)
  - [Knowledge Distilling](#knowledge-distilling)
  - [Fine-grained](#fine-grained)


# Papers

## Neural Network

> 有关神经网络的各类杂文，如可解释性,tricks等。

- [Bag of Tricks for Image Classification with Convolutional Neural Networks](https://arxiv.org/abs/1812.01187) (CVPR2019)
    - 4分
    - 记录了应用nn进行图像分类的各种tricks，第五章介绍的mixup,label smooth,知识蒸馏等都是非常有用的技巧。

- [mixup: BEYOND EMPIRICAL RISK MINIMIZATION](https://arxiv.org/abs/1710.09412) (ICLR2018)
    - 5分
    - mixup，实质上是给模型加上了一个线性系统的先验，实验效果非常显著。在本地跑mixup时，resnet18_cifar10可以提升一个点,resnet18_cifar100可以提升三个点。


## Label Smooth

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


## Long-tail

> 长尾分布问题，主要集中于图像分类。

- [Long-Tail Learning via Logit Adjustment](https://arxiv.org/abs/2007.07314) (ICLR2021 under review)
    - 5分
    - 这篇文章的related work写的很不错，非常清晰地涵盖了最近研究长尾分布的主要论文。
    - 文中通过对平衡后验概率的建模，提出了新的后处理方法以及Loss调整方法。motivation非常自然，可以理解成LDAM考虑了pair-wise margin的Loss，且实验效果也不错。与我们最近的一个idea非常相似，很难受。

- [Long-tailed Recognition by Routing Diverse Distribution-Aware Experts](https://arxiv.org/abs/2010.01809) (arXiv2020)
    - 4分
    - 集成学习的思想。stage1学experts的参数，两个Loss要求多个experts好而不同(每个experts可以关注不同的类)。stage2学对每个instance具体的experts的选择方法。
    - 为了降低模型复杂度：网络中所有channel变为四分之一，网络前一部分参数共享，stage2专家分配。
    - 实验效果好到离谱，同时提升head类和tail类，且各大任务都能提升6%左右。

- [Deep Representation Learning on Long-tailed Data: A Learnable EmbeddingAugmentation Perspective](https://arxiv.org/abs/2002.10826) (CVPR2020)
- [Memory-based Jitter: Improving Visual Recognition on Long-tailed Data with Diversity In Memory](https://arxiv.org/abs/2008.09809) (arXiv2020)
    - 3分
    - [知乎原作者解析](https://zhuanlan.zhihu.com/p/112248291)
    - 以上两篇出自同一作者，本质思想类似:都是希望从特征空间层面来扩充tail类的表示。
    - 第一篇是从head类中学到一个分布再扩充到tail类；第二篇应该是受到MoCo的启发，同样存储一个特征的memory queue，但是这里的特征是带标签的，并且做了一个反频率采样，再把memory queue中的特征扩充到所有类的特征空间中，使得在特征空间中类数目比较均匀。

- [Decoupling Representation and Classifier for Long-Tailed Recognition](https://arxiv.org/abs/1910.09217) (ICLR2020)
    - 5分
    - 应该是第一次将nn中Long-tail问题分为特征提取和分类两部分。实验表明即使是在Long-tail的数据集中，nn依然可以学到良好的特征表示，但需要对分类器W做后处理。文中根据实验现象提出了W对每一类进行L2Norm的方法。

- [BBN: Bilateral-Branch Network with Cumulative Learningfor Long-Tailed Visual Recognition](https://arxiv.org/abs/1912.02413) (CVPR2020_oral)
    - 4分
    - 提出了BBN网络结构来解决Long-tail问题，本质思想与Decoupling类似，希望特征学习阶段是不平衡的而分类器学习阶段是平衡的。
	- 实验效果非常优秀，但是实际上在对比之前的方法时并不算公平，因为网络多了近乎一倍的参数量，且每个batch采样也是两倍。

- [Rethinking the Value of Labels for Improving Class-Imbalanced Learning](https://arxiv.org/abs/2006.07529) (NIPS2020)
    - 4分
    - [知乎原作者解析](https://zhuanlan.zhihu.com/p/259710601)
	- 其中半监督学习框架引入了其他的unlabeled data，文中也讨论了如何选取这些unlabeled data。
	- 利用自监督对长尾数据集预训练，再使用其他任何的长尾学习算法。

- [Learning From Multiple Experts: Self-paced Knowledge Distillation for Long-tailed Classification](https://arxiv.org/abs/2001.01536) (ECCV2020_spotlight)
    - 4分
    - motivation:如果把Long-tail数据集分为多个子集，那么在每个子集中Long-tail的程度就会减缓，分开训练后的精度就会提高。
    - 因此，首先在这些子集上训练出来多个专家。这些专家可以起到两个作用:1.通过置信度来告诉student网络每个样本的难度，也就对应文中的课程学习模块。2.对student网络进行知识蒸馏。

- [Learning imbalanced datasets with label-distribution-aware margin loss](https://arxiv.org/abs/1906.07413) (NIPS2019)
    - 4分
    - 从margin的角度提出了LDAMLoss，还有DRW的训练方法，有一些理论分析，被后续大量工作作为baseline。

- [Class-balanced loss based on effective number of samples](https://arxiv.org/abs/1901.05555) (CVPR2019)
    - 4分
    - 文中提出在采样时会出现一些特征重复的现象，因此样本数并不能完全代表采样的覆盖情况。作者通过一些假设，简单推倒出一个有效样本数的概念，并以此代替频率对Loss进行修正。思想非常简单清晰。


## Unsupervised

> 无监督学习(包括自监督学习)。
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


## Semi-supervised

> 半监督学习。


## Knowledge Distilling

> 知识蒸馏。

- [Distilling the Knowledge in a Neural Network](https://arxiv.org/abs/1503.02531) (NIPS2014)
    - 5分
    - Hinton出品，知识蒸馏开山之作。
    - 属于logit级别的知识蒸馏。student网络一方面用带温度的softmax去学习teacher网络的分布，一方面用hard的softmax去学习真实标签。
    - teacher网络很大很复杂，他的soft标签可以包含一些类之间的关系，而这些是hard标签所没有的，因此可以把这部分看作其学到的“知识”，再交由student小网络进行学习。

- [Deep mutual learning](https://arxiv.org/abs/1706.00384) (CVPR2018)
    - 3分
    - 两个网络mutual的训练，将输出分布求KL散度作为Loss的一项。
    - 目标是两个网络之间相互指导训练(互相趋同)，最后两个网络都可以拿出来用。Long-tail问题中多专家框架和这个很像，但是是将多专家输出分布的KL散度之间最大化(互相不同)，然后集成起来用。
    - 一个模型学到的概率分布可以看作是模型学到了数据内部的一个本质规律，就可以用来指导另外一个模型。也可以看作一个正则化的手段，类似于一种更加贴合数据本身规律的label smooth。

- [Regularizing Class-wise Predictions via Self-knowledge Distillation](https://arxiv.org/abs/2003.13964) (CVPR2020)
    - 3分
    - self-kd,或可以理解为正则化。流程非常简单，取标签相同的两个batch，对其输出概率求KL散度作为正则项损失，即要求标签相同的输入拥有更加相近的输出概率分布。
    - 可以理解为更加真实的label smooth，因为是把相同标签的样本的输出看成soft label。可以说和Deep mutual learning结构几乎完全相同，只不过是使用了同一个网络以及pair的输入。另一方面和PairwiseConfusion完全相反。
    - 不得不感叹2020年了，还能这样水一篇CVPR。

- [Revisiting Knowledge Distillation via Label Smoothing Regularization](https://arxiv.org/abs/1909.11723) (CVPR2020)
    - 5分
    - motivation:在实验中发现两点不符合常理的现象:1.student网络也能指导teacher网络。2.准确率很低的teacher网络也能指导student网络。
    - 非常清晰地分析了KD和label smooth之间的关系。两者形式上本身就非常相似，label smooth可以看成是人工设计的一直输出均匀分布的virtual teacher，同理KD也可以看成是更强的可学习的label smooth。
    - 由此提出了TF-KD，一种是直接拿pretrain的student网络当成teacher，一种是将soft label加上temperature。
    - 文章思路非常清晰，idea也很好，但是实验也并不能表明TF-KD就比LSR好多少，也没有去探究normal-KD比TF-KD能好在哪。


## Fine-grained

> 细粒度识别。

- [Pairwiseconfusion for fine-grained visual classification](https://arxiv.org/abs/1705.08016) (ECCV2018)
- [natural world distribution via adaptive confusion energy regularization](https://openreview.net/forum?id=kKwFlM32HV5) (ICLR2021 under review)
    - 3分
    - motivation:两篇文章的出发点非常类似，只不过后者考虑了Long-tail的情况。在细粒度识别中，类间本身就非常相似，这就导致网络的输入非常相似，但是输出却是不同的标签，容易使得网络过于关注样本级别的特征，造成过拟合。因此提出了PairwiseConfusion模块，混淆pair中两个输出的概率分布。
    - 个人感觉这里的PairwiseConfusion方法和motivation的关联并不是那么紧密，反而更像是一种label smooth。只不过label smooth中以均匀分布作为一个正则化项，而这个方法使用pair中另一个分布的输出作为正则化项，但本质都是通过防止网络overconfident来抑制过拟合的情况。


