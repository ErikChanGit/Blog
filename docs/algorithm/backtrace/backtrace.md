## 回溯法 0/1 背包问题

#### 问题描述

输入：n件物品的价值和重量{<w1, v1>, <w2, v2>,…, <wn, vn>}和背包容量C

输出：(x1, x2, …, xn)，xi∈{0, 1}满足放入的物品重量小于背包容量的前提下价值最大

优化目标：价值最大化

#### 回溯法

回溯法（探索与回溯法）是一种选优搜索法，又称为试探法，按选优条件向前搜索，以达到目标。但当探索到某一步时，发现原先选择并不优或达不到目标，就退回一步重新选择，这种走不通就退回再走的技术为回溯法，而满足回溯[条件](https://baike.baidu.com/item/条件/1783021)的某个[状态](https://baike.baidu.com/item/状态/33204)的点称为“回溯点”。

- 核心思路
  - 初始化：目标函数置为0，将物品按价值体积比的非增顺序排序
  - 搜索：从根节点出发，尽量沿左孩子结点前进，当不能继续沿左孩子前进时，把搜索转到右子树，得到问题的一部分解。
  - 估计：由部分解所能得到的最大价值：如果估计价值高于上界，继续向下搜索，直到找到可行解保存可行解，并用可行解得到的目标函数的值更新目标函数的上界，向上回溯，寻找其他可行解。若估计值小于目标函数的上界，则丢弃当前的部分解，向上回溯。
