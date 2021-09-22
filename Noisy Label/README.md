# Table of contents

- [Table of Contents](#table-of-contents)
- [Noisy Label](#noisy-label)
    - [Basic](#basic)
    - [Open-set](#open-set)


# Noisy Label

> 标签噪声。

## Basic

> 经典的noisy label问题，不考虑OOD的存在。

- [Early-Learning Regularization Prevents Memorization of Noisy Labels](https://arxiv.org/abs/2007.00151) (NIPS2020)
    - 5分
    - motivation:DNN在noisy label的学习过程中有’early learning‘的现象:first use patterns, not brute force memorization, to fit real data，then memorize noisy label。因此，将早期模型的输出作为后期的一个正则化，有点像self-KD。
    - 在KD中，拟合target用的是KL散度，但文中用的是内积。原因是KL散度相比内积对target的拟合程度更高，是想利用到dark knowledge，对本文来说会过拟合。
    - 从梯度的角度说明了两点: (1) ensure that the contribution to the gradient from examples with clean labels remains large, and (2) neutralize the influence of the examples with wrong labels on the gradient.
    - 文章的图画的是真好，看图就非常容易明白作者的idea。


## Open-set

> 考虑Noisy + OOD。

- [NGC: A Unified Framework for Learning with Open-World Noisy Data](https://arxiv.org/abs/2108.11035) (ICCV2021)
    - 4分
    - 本质做法是基于近邻的contrastive learning，针对Noisy + OOD的情况设计了一些基于graph的准确选取近邻的方法，读下来感觉有novelty且make sense。

- [Jo-SRC: A Contrastive Approach for Combating Noisy Labels](https://arxiv.org/abs/2103.13029) (CVPR2021)
    - 3分
    - 考虑到了noisy应该分为ID noisy以及OOD noisy，因此在sample selection的时候应该分两步:1.首先分为clean与noisy样本。2.再将noisy样本分为ID与OOD(衡量两个view的输出概率分布的consistency，认为差异小的是ID)。

