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
- [OOD Detection](#OOD-detection)

# OOD Detection

> out-of-distribution detection,类似的概念有open-set recognition,novelty detection,anomaly detection等。

- [Enhancing The Reliability of Out-of-distribution Image Detection in Neural Networks](https://arxiv.org/abs/1706.02690) (ICLR2018)
    - 5分
    - 1.On in-distribution images, modern neural networks tend to produce outputs with larger variance across class labels.
    - 2.neural networks have larger norm of gradient of log-softmax scores when applied on in-distribution images.
    - 根据上述两个观察，提出了用temperature scaling和类似FGSM的input preprocessing来进一步拉大ID和OOD样本输出概率分布的差别，再用max-softmax方法来判定。
    - 文中的OOD指标相比于baseline更加清晰。

- [A Baseline for Detecting Misclassified and Out-of-Distribution Examples in Neural Networks](https://arxiv.org/abs/1610.02136) (ICLR2017)
    - 4分
    - OOD检测需要既保证in-distribution的分类准确率，又保证out-of-distribution的检测率。而后者是一个不平衡二分类问题，需要用ROC，PR等指标衡量。
    - OOD检测最直接的思路就是取softmax后最大的概率值作为判断标准，小于阈值则认为是OOD。文中用该方法建立了一系列baseline。
    - 观察发现OOD样本通常也有较大的confidence，因此这样的方法势必有局限性。
