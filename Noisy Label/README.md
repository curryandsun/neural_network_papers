# Table of Contents

- [Table of Contents](#table-of-contents)
- [Noisy Label](#noisy-label)
  - [Robust Loss or Regularization](#robust-loss-or-regularization)
  - [Loss Adjustment](#loss-adjustment)
  - [Sample Selection](#sample-selection)
  - [Open-set](#open-set)


# Noisy Label

> 标记噪声问题。[一篇很好的综述](https://arxiv.org/abs/2007.08199)。

## Robust Loss or Regularization

> 设计对噪声数据更加鲁棒的loss或者regularization。
  
- [Early-Learning Regularization Prevents Memorization of Noisy Labels](https://arxiv.org/abs/2007.00151) (NIPS2020)
    - 4分
    - motivation:DNN在noisy label的学习过程中有’early learning‘的现象:first use patterns, not brute force memorization, to fit real data，then memorize noisy label。因此，将早期模型的输出作为后期的一个正则化，有点像self-KD。
    - 在KD中，拟合target用的是KL散度，但文中用的是内积。原因是KL散度相比内积对target的拟合程度更高，是想利用到dark knowledge，对本文来说会过拟合。
    - 从梯度的角度说明了两点: (1) ensure that the contribution to the gradient from examples with clean labels remains large, and (2) neutralize the influence of the examples with wrong labels on the gradient.
    - 文章的图画的是真好，看图就非常容易明白作者的idea。

- [Symmetric Cross Entropy for Robust Learning with Noisy Labels](https://arxiv.org/abs/1908.06112) (ICCV2019)
    - 4分
    - RCE是MAE的一般情况，本质上还是symmetric loss那一套。实际上在没有noise的情况下，RCE会降低性能，因此其主要还是解决noise，而文中所提到的的hard和easy的说法偏向于讲故事。

- [Generalized Cross Entropy Loss for Training Deep Neural Networks with Noisy Labels](https://arxiv.org/abs/1805.07836) (NIPS2018)
    - 5分
    - MAE虽然noise-tolerant，但是在优化上对于所有样本有相同的梯度，遇到大规模数据集难以优化。而CE则implicitly weighed more on samples with predictions that agree less with provided labels，也因此对noise更敏感。因此很自然的想法就是结合两者的优点，设计一个既robust又易优化的loss。

- [Robust Loss Functions under Label Noise for Deep Neural Networks](https://arxiv.org/abs/1712.09482) (AAAI2017)
    - 5分
    - noise-tolerant: the loss function L is noise-tolerant意味着使用L和ERM原则在clean data D<sub>1</sub>以及noisy data D<sub>2</sub>上训练得到的两个分类器在D<sub>1</sub>上有着相同的错分率。
    - symmetric loss function: 对于所有类的one-hot label计算得到的loss值求和为一个定值，如MAE Loss。文章从不同的noisy type出发，理论分析了symmetric loss更强的鲁棒性。


## Loss Adjustment

> Loss correction, or label correction.

- [Training Deep Neural Networks on Noisy Labels with Bootstrapping](https://arxiv.org/abs/1412.6596) (ICLR2015)
- [Dimensionality-Driven Learning with Noisy Labels](https://arxiv.org/abs/1806.02612) (ICML2018)
- [Unsupervised Label Noise Modeling and Loss Correction](https://arxiv.org/abs/1904.11238) (ICML2019)
    - 4分
    - Label correction.其通用套路是将noisy label与pseudo label做一个线性加权，而权重系数则是设计的重点。

- [Making Deep Neural Networks Robust to Label Noise: a Loss Correction Approach](https://arxiv.org/abs/1609.03683) (CVPR2017)
    - 5分
    - Loss correction.先估计噪声转移矩阵T，再通过两种方式来做correction：
        - backward:计算得到loss之后再乘T<sup>-1</sup>。
        - forward:计算loss时将T乘在输出概率分布p上。

- [Using trusted data to train deep networks on labels corrupted by severe noise](https://arxiv.org/abs/1609.03683) (CVPR2017)
    - 4分
    - Loss correction.先用noisy data训练一个model，再在anchor points上估计噪声转移矩阵T。


## Sample Selection

> 样本筛选。

- [Decoupling "when to update" from "how to update"](https://arxiv.org/abs/1706.02613) (NIPS2017)
- [Co-teaching: Robust Training of Deep Neural Networks with Extremely Noisy Labels](https://arxiv.org/abs/1804.06872) (NIPS2018)
- [How does Disagreement Help Generalization against Label Corruption?](https://arxiv.org/abs/1901.04215) (ICML2019)
- [Combating noisy labels by agreement: A joint training method with co-regularization](https://arxiv.org/abs/2003.02752) (CVPR2020)
    - 3分
    - Co-teaching系列。个人感觉这系列文章方法都相当简单。

- [Understanding and Utilizing Deep Neural Networks Trained with Noisy Labels](https://arxiv.org/abs/1905.05040) (ICML2019)
    - 4分
    - 有意思的理论分析：In [Zhang et al. (2017)](https://arxiv.org/abs/1611.03530), it has
    been empirically found that the generalization performance of DNNs is highly dependent on the noise ratio. In this paper, the authors theoretically and empirically find that the test accuracy can be quantitatively characterized in terms of the noise ratio.
    - 接下来很自然的想法就是用cross-validation得到test acc再用于估计noise ratio。这样就可以解决Co-teaching方法必须事先获得noise ratio的问题。

- [DivideMix: Learning with Noisy Labels as Semi-supervised Learning](https://arxiv.org/abs/2002.07394) (ICLR2020)
    - 5分
    - 用GMM拟合loss分成clean set和noisy set，再利用mixmatch做半监督训练。

## Open-set

> 考虑Noisy + OOD。

- [Learning from Noisy Data with Robust Representation Learning](https://openaccess.thecvf.com/content/ICCV2021/papers/Li_Learning_From_Noisy_Data_With_Robust_Representation_Learning_ICCV_2021_paper.pdf) (ICCV2021)
- [MoPro: Webly Supervised Learning with Momentum Prototypes](https://arxiv.org/abs/2009.07995)(ICLR2021)
    - 4分
    - 一作都是[[PCL](Prototypical Contrastive Learning of Unsupervised Representations)](https://arxiv.org/abs/2005.04966)的作者，这两篇文章相当于给PCL找了个在open-set noise上的应用。

- [NGC: A Unified Framework for Learning with Open-World Noisy Data](https://arxiv.org/abs/2108.11035) (ICCV2021_oral)
    - 4分
    - 提出一个新setting：在open-set noise的基础上再考虑inference时存在OOD样本。是一个make sense的setting。
    - 本质做法是基于近邻的contrastive learning，针对Noisy + OOD的情况设计了一些基于graph的准确选取近邻的方法。其中，选取LCC的这一步类似于[TopoFilter](https://arxiv.org/abs/2012.04835)，不同的是利用confidence删边替代了异类删边+LCC外围剔除的操作。

- [Jo-SRC: A Contrastive Approach for Combating Noisy Labels](https://arxiv.org/abs/2103.13029) (CVPR2021)
    - 3分
    - 考虑到了noisy应该分为ID noisy以及OOD noisy，因此在sample selection的时候应该分两步:1.首先分为clean与noisy样本。2.再将noisy样本分为ID与OOD(衡量两个view的输出概率分布的consistency，认为差异小的是ID)。

- [Iterative Learning with Open-set Noisy Labels](https://arxiv.org/abs/1804.00092) (CVPR2018)
    - 5分
    - 第一篇考虑open-set noise的文章。方法的本质是利用feature做noisy data detection。用feature是考虑到open-set noise的存在，没办法像close-set一样做loss correction或label corretion。
    - Cons: 没有对ID noise和OOD noise做分离以及不同的训练，而是直接把这两者同时检测出来当成是noisy data。
