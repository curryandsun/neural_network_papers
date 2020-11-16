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
- [Semi-supervised](#semi-supervised)

# Semi-supervised

> 半监督学习。

- [Self-training with Noisy Student improves ImageNet classification](https://arxiv.org/abs/1911.04252) (CVPR2020)
    - 4分
    - 与传统self-training或者KD的区别在于两点：Our  key  improvements  lie  in  adding  noise  to  the  student and using student models that are not smaller than the teacher.
    - When applied to unlabeled data, noise has an important benefit of enforcing invariances in the decision function on both labeled and unlabeled data.

- [Temporal Ensembling for Semi-Supervised Learning](https://arxiv.org/abs/1610.02242) (ICLR2017)
- [Virtual Adversarial Training: A Regularization Method for Supervised and Semi-Supervised Learning](https://arxiv.org/abs/1704.03976) (arXiv2017)
- [Mean teachers are better role models: Weight-averaged consistency targets improve semi-supervised deep learning results](https://arxiv.org/abs/1703.01780) (NIPS2017)
- [MixMatch: A Holistic Approach to Semi-Supervised Learning](https://arxiv.org/abs/1905.02249) (NIPS2019)
    - 5分
    - semi-supervised learning必读经典。



