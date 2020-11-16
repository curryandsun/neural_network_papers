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
- [Noisy label](#noisy-label)

# Noisy Label

> 标签噪声。

- [Early-Learning Regularization Prevents Memorization of Noisy Labels](https://arxiv.org/abs/2007.00151) (NIPS2020)
    - 5分
    - motivation:DNN在noisy label的学习过程中有’early learning‘的现象:first use patterns, not brute force memorization, to fit real data，then memorize noisy label。因此，将早期模型的输出作为后期的一个正则化，有点像self-KD。
    - 在KD中，拟合target用的是KL散度，但文中用的是内积。原因是KL散度相比内积对target的拟合程度更高，是想利用到dark knowledge，对本文来说会过拟合。
    - 从梯度的角度说明了两点: (1) ensure that the contribution to the gradient from examples with clean labels remains large, and (2) neutralize the influence of the examples with wrong labels on the gradient.
    - 文章的图画的是真好，看图就非常容易明白作者的idea。
