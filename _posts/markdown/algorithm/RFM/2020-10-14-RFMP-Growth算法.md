---
title: RFMP-Growth算法
date: 2020-10-14
categories: [Notes, RFM pattern Mining]
tags: [recency, frequency, utility]     # TAG names should always be lowercase
math: true
---

写在前面：RFM模型分析在当下已经是非常常见的了，广泛运用与电商或传统商业提高自己的利润总量。什么是RFM模型？即通过分析一个客户的近期购买行为、购买的总体频率以及花费多少钱等3项指标来描述该客户的价值状况。该算法是其中之一参考度是频率，即从最简单也是最容易想到的角度出发，研究如何从客户角度来推销更多产品，而之前学习的挖掘算法都是基于产品的角度。

# Discovering valuable frequent patterns based on RFM analysis without customer identification information

## 样例

![基本样例信息](/assets/img/algorithm/RFMP-Growth算法/Example infomation.png)

## 定义

+ 交易项集 $T = \lbrace t_{\rm i}, (x_1, q_{\rm x_1}), \dots, (x_{\rm m}, q_{\rm x_m}\rbrace$，其中 $t$ 表示该交易项集发生的时间，$x$ 表示项（商品），$q_{\rm x_j}$ 表示内部效用（商品购买量）
+ 频繁度分值（**Frequency score**）：相当于以前的支持度 $sup(X)$，该文中使用 $FScore_{DB}(X)$ 表示。不同的是，该支持度必须符合 $FScore(X) \ge \mid DB\mid \times \alpha$ 才能把该项集 $X$ 当作是 $F$-$pattern$（其中 $\alpha$ 是阈值，$\mid DB\mid$ 表示数据集中包含的总交易项数量）
+ 利润分值（**Monetary score**）：相当于以前的效用值 $u(X)$，该文中使用 $MScore(X, T_{\rm j})$ 表示在交易项 $T_{\rm j}$ 中该项集 $X$ 的利润分值，定义式为 $MScore(X, T_{\rm j}) = \sum_{\rm x_i \in X \land X \subseteq T_{\rm j}}p(x_{\rm i}) \times q_{\rm x_i}$。更进一步，项集 $X$ 在整个数据集中的利润分值定义为 $MScore(X) = \sum_{T_{\rm j} \in DB}$。当 $MScore(X) \ge \beta$ 时，我们认定该项集是 $M$-$pattern$（其中 $\beta$ 是设置的阈值）
+ 近期分值（**Recency score**）：在交易项集 $T_{\rm j}$ 内，项集 $X$ 存在，定义为 $RScore(X, T_{\rm j}) = (1-\delta)^{time_{\rm current}-time_{\rm T_j}}$(其中 $\delta \in (0, 1)$ 是设定的衰减速度)，更进一步，项集 $X$ 在整个数据集中的近期分值定义为 $RScore(X) = \sum_{T_{\rm j} \in DB}RScore(X, T_{\rm j})$。当 $RScore(X) \ge \gamma$ 时，我们认定该项集是 $R$-$pattern$ （其中 $\gamma$ 是设定的阈值）

_Ps. 当某个项集 $X$ 满足以上三个度量分值都不小于各自设定的阈值时候，我们认定该项集是 $RFM$-$pattern$_

+ 项集的交易项效用值（**Transaction utility**）类似于以前的 $tu(X, T_{\rm j})$，该文中定义为 $ta(X, T_{\rm j}) = \sum_{x_{\rm i} \in T_{\rm j} \land X \subseteq T_{\rm j}}p(x_{\rm i}) \times q_{\rm x_i}$，更进一步，项集 $X$ 在整个数据集中的总交易项效用值定义为 $tta_{\rm DB}(X) = \sum_{T_{\rm j}}ta(A, T_{\rm j})$

*Ps. 当某个项集 $X$ 满足 $FScore(X) \ge \mid DB\mid \times \alpha$，$tta_{\rm DB}(X) \ge \beta$，$RScore_{\rm DB}(X) \ge \gamma$，那么我们认定该项集是 $RFT$-$pattern$ （类似于使用 **TWU** 筛选出候选项集，即需要进一步确认的项集）*

+ **RFM-pattern-tree**：在 [$FP$-$Tree$](https://juejin.im/post/6883290309980061704) 上进行改造，结构如下图所示：

  ![RFM-pattern-tree](/assets/img/algorithm/RFMP-Growth算法/RFM-pattern-tree.png)

## 性质

1. 设 $SRFM$ 和 $SRFT$ 分别代表 $RFM$-$pattern$ 集和 $RFT$-$pattern$ 集，那么有 $SRFM \subseteq SRFT$

   > 证明：
   >
   > 根据 $MScore(X, T_{\rm j}), \, ta(X, T_{\rm j})$ 的定义，我们很容易得出 $\sum_{x_{\rm i} \in T_{\rm j}}p(x_{\rm i}) \times q_{\rm x_i} \ge \sum_{x_{\rm i} \in X}p(x_{\rm i}) \times q_{\rm x_i}$（因为 $X \subseteq T_{\rm j}$）进而 $tta_{\rm DB}(X) \ge MScore_{\rm DB}(X)$。可知当项集 $X$ 是 $RFM$-$pattern$ 时，必然是 $RFT$-$pattern$

2. 当一个项集是 $RFT$-$pattern$，那么它的任何子集依然会是 $RFT$-$pattern$（**向下封闭性**）

   > 证明：
   >
   > 设项集 $X, \, X'$ 是 $R$-$pattern$ 和 $F$-$pattern$，且 $X' \subset X$。自然有 $T_{\rm X} \subset T_{\rm X'}$（反过来看 $X$ 是 $X'$ 的扩展集，那么包含 $X'$ 的交易项集不一定包含 $X$）即 $tta_{\rm DB}(X') \ge tta_{\rm DB}(X)$，得证

3. 略

## 算法

**构造 RFM-pattern-tree：**

![Construct RFM-pattern-tree](/assets/img/algorithm/RFMP-Growth算法/Construct RFM-pattern-tree.png)

**构造 RFM-header-tree 伪代码**：计算方式和之间计算 [1-utility-list](https://juejin.im/post/6844904185842892814) 是一样的，一层层遍历交易项 $T_{\rm i}$，一层层把本轮遍历计算出来的结果添加到表中，伪代码如下：

![algo_RFM-header-table](/assets/img/algorithm/RFMP-Growth算法/algo_RFM-header-table.png)

以下演示根据样例数据集计算，我们假设 $\alpha = 10\%, \, \beta = 100\%, \, \gamma = 0.95\%, \, \delta = 0.01\%, t_{\rm current} = 115$，遍历 $T_1$ 可以得到 $RScore_{\rm DB}(T_1) \approx 0.315, \, FScore_{\rm DB}(T_1) = 1, \, tta_{\rm DB}(T_1) = 312$，即下表：

| item-name | RScore | FScore | tta  | link |
| :-------: | :----: | :----: | :--: | :--: |
|     A     | 0.315  |   1    | 312  |      |
|     B     | 0.315  |   1    | 312  |      |
|     F     | 0.315  |   1    | 312  |      |

遍历交易项 $T_2$ 可以得到 $RScore_{\rm DB}(T_2) \approx 0.377, \, FScore_{\rm DB}(T_2) = 1, \, tta_{\rm DB}(T_2) = 164$，即下表：

| irem-name | RScore | FScore | tta  | link |
| :-------: | :----: | :----: | :--: | :--: |
|     A     | 0.692  |   2    | 476  |      |
|     B     | 0.692  |   2    | 476  |      |
|     F     | 0.692  |   2    | 476  |      |

...依次计算各交易项，最终得到下表（已进行过滤且按照 $FScore$ 降序排列）：

| item-name | RScore | FScore | tta  | link |
| :-------: | :----: | :----: | :--: | :--: |
|     A     | 4.976  |   8    | 1059 |      |
|     E     | 4.304  |   6    | 768  |      |
|     C     | 2.525  |   4    | 329  |      |
|     F     | 2.494  |   4    | 679  |      |
|     B     | 1.158  |   3    | 667  |      |
|     D     | 1.778  |   2    | 300  |      |

**构造 RFM-pattern-tree 伪代码**：

![algo_RFM-pattern-tree](/assets/img/algorithm/RFMP-Growth算法/algo_RFM-pattern-tree.png)

以下继续演示计算，遍历交易项 $T_1$ 时，可以得到 $RScore_{\rm DB}(T_1) \approx 0.315, \, FScore_{\rm DB}(T_1) = 1, \, tta_{\rm DB}(T_1) = 312$，分别创建三个子节点: $insert\_node([A \mid FB], root, 0.315, 312)$，$insert\_node([F \mid B], A, 0.315, 312)$，和 $insert\_node([B \mid - null], B, 0.315, 312)$ 排序后如下图(左)所示（_Ps. 文中特别强调了在创建节点后需要立即建立相应的索引_）；遍历交易项 $T_2$ 时，可以得到 $RScore_{\rm DB}(T_2) \approx 0.377, \, FScore_{\rm DB}(T_2) = 1, \, tta_{\rm DB}(T_2) = 164$，由于节点已经存在，所以直接相加重新赋值，下图(中)所示；遍历交易项 $T_3$ 时，可以得到 $RScore_{\rm DB}(T_3) \approx 0.393, \, FScore_{\rm DB}(T_3) = 1, \, tta_{\rm DB}(T_3) = 45$，因为节点 $C$ 在 $A$ 后面，所以新开一条分支，如下图(右)所示：

  <center calss="half">
      <img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/788a8831e085441c9e0c4d7a89794827~tplv-k3u1fbpfcp-watermark.image" width=100 alt="insert T1 into RFM-pattern-tree"/>
      <img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d9323e0ad0934127b77ed9ddc144b116~tplv-k3u1fbpfcp-watermark.image" width=100 alt="insert T2 into RFM-pattern-tree"/>
      <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b65bbe172814210a4833654f9f1a9b5~tplv-k3u1fbpfcp-watermark.image" width=200 alt="insert T3 into RFM-pattern-tree"/>
  </center>


...经过多次计算，可以得到最终结果如下图（构造流程也可以参考[$FP$-$Tree$](https://juejin.im/post/6883290309980061704)）：

![algo_RFM-pattern-tree_Final](/assets/img/algorithm/RFMP-Growth算法/algo_RFM-pattern-tree_Final.png)

**RFMP-growth 算法伪代码：**

![RFMP-growth algorithm](/assets/img/algorithm/RFMP-Growth算法/RFMP-growth algorithm.png)

**tree-growth 算法伪代码：**

![tree-growth algorithm](/assets/img/algorithm/RFMP-Growth算法/tree-growth algorithm.png)

# 总结

该算法基于之前的FP-Growth算法改进而成，需要注意的是由于衡量标准的增多，算法在一定程度上需要多次扫描数据集，如何设计一个好的数据结构是比较重要的，再者Up-Growth是对FP-Growth的改进，那么是否能对RFMP-Growth进行改进呢？