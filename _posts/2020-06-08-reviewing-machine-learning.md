---
layout: post
title:  "Reviewing ML"
description: Reviewing machine learning before quiz.
date:   2020-06-10 14:06:41 +023
---
# 考前抱佛脚之quiz大学习
**考后：水了水了**
***

# 神经网络
### 正则化
1. weight decay
2. 早停
3. invariance
   1. 数据增强
   2. tangent propagation
   3. 特征提取时保持不变性
   4. 在网络中加入不变性结构

CNN的正则化要素$\sim$无限强先验  
1. 局部感受野
2. 权值共享，平移不变性
3. 下采样

RNN：memory cell

# 聚类

关键问题
* 如何定义cluster
* 如何关联样本点
* 如何表征数据
* 多少个cluster
* 如何分组（算法）

## 距离
> $D(A,B)=D(B,A)$
> $D(A,A)=0$  
> $D(A,B)=0\lrArr A=B$
> $D(A,B)\leq D(A,C)+D(C,B)$

#### 衡量聚类好坏：内部/外部标准
1. 内部标准：依赖数据表征和相似度定义
2. 基于金标准（golden standard）数据，如纯度$\frac{1}{n_i}max_j\{n_{ij}\},j\in C$，即cluster中数量最多的一类标签占比

## 算法
#### 1. 层次化：
   1. 自上而下
   2. 自下而上
   
###### 衡量cluster间距：
1. Single link 最近两点
2. Complete link 最远两点
3. Centroid 中心两点
4. Average 所有点平均

优点：简单，不需要提前定义cluster数，层次关系易于解释
缺点：时间复杂度高$O(n^2logn)\to O(n^3)$，噪声敏感，可能聚成链状的树

#### 2. 基于划分
* 需要给定超参数K
* Goodness measure: $SD_{K_i}$ 平方距离
* 复杂度：$O(LKnm)$,L:iter, K:cluster, n:样本量，m：维数
* 种子选择：初始化类中心
  * 优化：启发式选择；多试几次
  
优点：简单，快，对凸的数据很合适  
缺点：K不好选，对种子敏感，噪声敏感，非凸不行

#### 3. 基于密度
**DBSCAN**
* 中心点：在$\epsilon$范围内点数$>MinPts$
* 边界点：在中心点范围内
* 噪声点：其他
* 直接密度可达，密度可达，密度连通
* 选择MinPts、$\epsilon$：以第k近作为阈值  
  
优点：可划分任意形状的cluster，可处理噪声，一次扫描快，不需要选K
* 时间复杂度$O(n^2)$，空间复杂度$O(n)$
  
缺点：无法应对变化的密度；高维数据稀疏不好识别密度；对$\epsilon$，MinPts敏感。  

#### 4. 谱聚类
**连通性而非聚集性**  
* 相似度矩阵W、度矩阵D，拉普拉斯矩阵$L=D-W$。
* RatioCut：$\sum_{i=1}^k\frac{cut(A_i,\bar{A_i})}{|A_i|}$
* Ncut：$\sum_{i=1}^k\frac{cut(A_i,\bar{A_i})}{vol(A_i)},vol(A_i)=\sum_{j\in A_i}d_i$
* 谱聚类将上述两个NP hard松弛化，Rayleigh-Ritz Theory
* $min f^TLf,s.t.f^Tf=n,f\perp 1$  

优点：适用于高维问题（相当于将$n\times n$降维到$n\times k$即k个特征向量），可解决非凸  
缺点：对参数敏感，不能解决大数据量，不均衡的数据不行，对相似度定义和k选择敏感

# 降维
### PCA：最大方差方向
> $XX^Tv=\lambda v$

优点：特征向量，无需调参，没有局部最优

缺点：局限于二阶统计量（协方差），线性投影

##### kernel PCA

### ICA
独立和不相关

准则：非高斯性
* 峭度：对outlier敏感
* 负熵
* 负熵的近似

##### Fast ICA
1. 预处理：中心化，白化
2. fast ICA algo：最大化非高斯性
3. 解混
   
优点：不需要步长，不像基于梯度的ICA

LDA、PCA、ICA的联系和区别

# EM`算法框架`
隐变量的三种情况
1. 数据本来不存在，假设有
2. 不可能被测量
3. 测不准

* 硬指派：不同cluster可能有overlap
* 软指派：
  
K-means是固定$\Sigma$为单位矩阵的EM for GMM。
 
> $inference\lrarr fully observed$  
> $\dArr$
> E-step估计隐变量$\lrarr$M-step用MLE、MAP根据估计出来的complete data更新参数

思考题：
* complete likelihood, incomplete likelihood, expected complete likelihood$<Z_m^k>$，incomplete求解不好操作，发现expected是incomplete的下界，所以max exp即max inc。

* GMM和GDA的区别？

# HMM 
三大任务
1. evaluation
2. decoding
3. learning

### Evaluation
前向$\alpha_t^k=P(x_t|y_t^k=1)\sum_i\alpha_{t-1}^ia_{i,k},where a是转移概率$
### Decoding
##### 单个时间点$P(y_t|X)$
Forward-Backward，$\beta_t^k=\sum_ia_{k,i}P(X_{t+1}|y_{t+1}^i=1)\beta_{t+1}^i$
##### $P(y_1,...,y_T|X)$
Veterbi decoding, 动态规划思想
### Learning
Baum-Weilch

# 图模型
有向图=贝叶斯网络  
无向图=马尔可夫随机场  
马尔可夫坦（？：父、子、coparent

### D-separation

### 马尔可夫场中的条件独立性

### Inference & Learning
Query 1. Likelihood
Query 2. 条件概率
Query 3. MPA最大可能性指派

### Inference Algo.
* Variable Elimination
    eg. 在HMM中，从左开始和从又开始对应了前向和后向
* 信念传播算法
  * Elimination Cliques
* Junction Tree
  * Clique tree
