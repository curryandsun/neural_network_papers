# Table of Contents

- [Table of Contents](#table-of-contents)
- [Contrastive Learning](#contrastive-learning)
  - [Basic](#basic)
  - [Better Understanding](#better-understanding)
  - [Better Sampler](#better-sampler)
  - [Dense](#dense)
    
# Contrastive Learning

## Basic

> 对比学习的主流方法

- [Bootstrap your own latent: A new approach to self-supervised Learning](https://arxiv.org/abs/2006.07733) (NIPS2020)
- [Exploring Simple Siamese Representation Learning](https://arxiv.org/abs/2011.10566) (arXiv2020)
    - 5分。
    - 两者都是做减法的思路，都通过一个predictor来引入不对称性，使得模型不能通过简单学得一个trivial的解来使得正样本之间的表示一致，因此带来了无需使用负样本的可能性。

- [Unsupervised Learning of Visual Features by Contrasting Cluster Assignments](https://arxiv.org/abs/2006.09882) (NIPS2020)
    - 5分。
    - SWaV。不同于对比学习中常见的直接使用表示进行对比的思想，SWaV的核心思想在于要求两个view之间cluster assignment的一致性。
    - 为什么不直接用内积后的cluster assignment交换到另外一个view而是去解一个复杂的优化问题？这是为了加上最大熵的正则化来避免全部assign到同一个cluster上的平凡解。
    - prototypes和网络参数是同时更新的。

- [Momentum Contrast for Unsupervised Visual Representation Learning](https://arxiv.org/abs/1911.05722) (CVPR2020_oral)
    - 5分
    - KaiMing He出品。指出之前的办法要么不够large，要么不够consistent。算法主要包括以下几点:
        - 采用了queue的方式构建负样本dictionary，使得其size可以大大超过mini-batch的size，而又不用像MB那样存储所有样本的表示。
        - 对于f_k的参数，采用了momentum的变化方式，使得f_q的参数可以更平滑地影响f_k。这其实也是非常重要且与1.高度耦合的一个想法。因为queue中的表示是不断更新的，如果没有momentum来平滑地影响f_k，则queue中新加入的表示将与原来的表示差异过大，从而导致模型难以收敛。以上两点也就算MoCo算法的核心思想:large and consistent。
        - shuffle batch normalization:multi-GPU的设置下，对每一个mini-batch，shuffle f_k的samples order(算loss时shuffle回来)，而f_q的保持不变，这样就保证了每个GPU上的两个encoder的用于BN的subset是不同的，防止模型因为相似的BN统计值而leak information。

- [A Simple Framework for Contrastive Learning of Visual Representations](https://arxiv.org/abs/2002.05709) (ICML2020)
    - 5分
    - Hinton出品。算法主要包括以下几点:
        - Simple体现在抛弃了memory bank等各种人工设计的模块，采用了最基本最简洁的contrastive learning框架。可以这么simple的前提在于要加大batchsize来提高负样本的采样数目，文中使用了恐怖的8096。
        - 探究了data augmentation的重要性。
        - 将常用的linear的head转化为具有激活函数的MLP，并使用MLP前的表示作为最后结果。这是因为MLP后的表示作用于最终的优化目标，对于各种数据增强表现出不变性，这会使得这个表示抛弃了与这些数据增强相关的信息，而整个MLP就相当于一个缓冲地带，在MLP之前的表示可以maintain更多与数据增强相关的信息。

- [Unsupervised Embedding Learning via Invariant and Spreading Instance Feature](https://arxiv.org/abs/1904.03436) (CVPR2019)
    - 4分
    - NPID方法用到了Memory Bank,使得t时刻encoder得到的query只能和t-1时刻得到的dictionary来构建正负pair，再进行Loss计算，这样显然效果不够好。
    - 该方法用augmentation的方法构建正pair，用mini-batch中的其他样本构建负pair，实现了真正的instance-level的对比学习。

- [Unsupervised Feature Learning via Non-Parametric Instance Discrimination](https://arxiv.org/abs/1805.01978) (CVPR2018_spotlight)
    - 5分
    - motivation:尽管监督学习的目标只是分类，但模型可以隐式地学习到类别之间的相似度，即模型学到了监督信息所没有赋予的dark knowledge。那么如果赋予每一个样本一个标签，用instance-wise代替class-wise，这样也可以学到样本的特征表示，那么这样的表示也应该同样隐含着类别的相似度信息。
    - class-wise转为instance-wise后不能简单应用CE(类别太多)，因此采用了NCE和Memory Bank。
    - 可以说是contrastive learning真正应用在大规模图像数据上的开山之作。

- [Deep Clustering for Unsupervised Learning of Visual Features](https://arxiv.org/abs/1807.05520) (ECCV2018)
    - 4分
    - DeepCluster方法，思路很简单:将通过CNN后的特征进行聚类，再用聚类的结果作为伪标签反向传播到CNN。


## Better Understanding

> 对对比学习的理解

- [Decoupled Contrastive Learning](https://arxiv.org/abs/2110.06848#) (ICLR2022 under review)
    - 4分
    - 分析了InfoNCE Loss的梯度，其中都有共同所谓NPC项，导致positive和negative是couple在一起的，这就导致两个问题：
      - When the positive sample is close to the anchor and less informative, the gradient from the negative samples are also reduced.
      - When the negative samples are far away and less informative, the learning rate from the positive sample is mistakenly reduced.
    - 因此，直接的想法就是去掉NPC项实现两者的decouple，作者通过移除分母中的postive项来实现这一点。
    - 从直觉上来说，移除分母中的postive项可以增大其中negative项的影响，从而带来了小batch-size的可能性。

- [What Should Not Be Contrastive in Contrastive Learning](https://arxiv.org/abs/2008.05659v2) (ICLR2021)
- [What Makes for Good Views for Contrastive Learning?](https://arxiv.org/abs/2005.10243) (NIPS2020)
    - 5分
    - InfoNCE Loss是在最大化v1, v2互信息的下界，那么应该怎么选取v1, v2呢？直观上来说，如果v1, v2本身互信息太大，比如两个view非常接近，模型学到的有很多Excess noise，即图片本身的特性；但若太小，比如两个view完全不一样，则模型学不到东西。
    - 那么最合适的v1, v2应该是互信息中只包含task-relevant的信息，对于不同的任务应该选择不同的view。比如下游任务是要用颜色分类，那么v1, v2就应该是除了颜色一样其他都不一样。

- [Understanding Contrastive Representation Learning through Alignment and Uniformity on the Hypersphere](https://arxiv.org/abs/2005.10242) (ICML2020)
    - 5分
    - we identify two key properties related to the contrastive loss: 
        - alignment (closeness) of features from positive pairs
        - uniformity of the induced distribution of the (normalized) features on the hypersphere

## Better Sampler

> 更好的正负采样方式

- [Incremental False Negative Detection for Contrastive Learning](https://arxiv.org/abs/2106.03719) (ICLR2022_under review)
    - 4分
    - 探究contrastive learning中存在的False Negative问题。提出的方法可以检测出False Negative，并将其从负样本集合中移除（或类似[SupCon](https://arxiv.org/abs/2004.11362)一样加入正样本集合）。显然该方法下界为SimCLR，上界为SupCon。
    - 利用聚类做False Negative检测的想法也非常直接，即特征越接近，则越有可能是False Negative。

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


## Dense

> pixel-wise contrastive learning

- [Self-Supervised Visual Representations Learning by Contrastive Mask Prediction](https://arxiv.org/abs/2108.07954) (ICCV2021)
    - 3分
    - 取region+ROI Align用在Dense CL很合适。
    - 个人没太明白用一个mask遮住图片的某一个region再加上mask prediction head去恢复的意义是什么。把两者都去掉就是典型的Dense CL的做法了。

- [Dense Contrastive Learning for Self-Supervised Visual Pre-Training](https://arxiv.org/abs/2011.09157) (CVPR2021_oral)
    - 5分
    - 效果很好的Dense CL Baseline。将通用的GAP+MLP结构换成了1*1 Conv，以保证feature map的size。因为ResNet最后输出7*7的size，最后文章选用的grid size就是7。因此这样的表示其实是patch-level的。
    - 正样本来自于增强样本所有patch表示中最接近的那一个，负样本来自于其他图片的image表示。
    - 
- [Unsupervised Learning of Dense Visual Representations](https://arxiv.org/abs/2011.05499) (NIPS2020)
    - 4分
    - 第一篇Dense CL的文章。做法也比较简单,Encoder-decoder的结构，每个pixel都有对应的表示，找到对应位置的pixel的表示作为正样本对，再用别的图片的pixel的表示作为负样本对。

  