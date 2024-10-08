---
title: 2024 8月杂题记
cover: https://lucky-cloud09.github.io/img/b5.jpg
categories: 杂题
date: 2024/8
---

### [AT_joisc2017_c](https://www.luogu.com.cn/problem/AT_joisc2017_c)

> 题目描述：过于复杂，略。

答案明显具有单调性。考虑二分答案。

有一个很自然的想法，没点燃的要向正在燃的靠近，且一定以最大速度走 $T$ 秒。

对于一个区间 $[L,R]$，满足让它能用一个点燃的互相点燃。有一个条件为 $X_r - X_l \le 2 \times (r - l) \times v \times T$，转变一下，即为 $X_r - 2 \times r \times v \times T \le X_l - 2 \times l \times v \times T$。

我们就设 $a_i = X_i - 2 \times i \times v \times T$。

利用归纳法，要使 $[L,R]$ 合法，$[L+1,R]$ 或 $[L,R-1]$ 就需要合法。

我们找到一个性质最好的区间 $[L,R]$ 满足 $L \le k \le R$ 且 $a_L$ 最大，$a_R$ 最小。

我们考虑 $[k,k]$ 扩展到 $[L,R]$。

假设当前在 $[l,r]$，先考虑扩展 $l$，到[i,r]。

- $L \le i \le l \wedge a_l \le a_i$。
- $i \le j \le l \wedge a_j \ge a_r$

其他情况同理。

若不能到 $[L,R]$ 则返回 $0$。

最后，我们将 $[1,n]$ 缩小到 $[L,R]$ 即可，与上面同理。

### [[PKUWC2018] Minimax](https://www.luogu.com.cn/problem/P5298)

> 题目大意：给你一棵树，保证一个结点至多有两个子节点。给你叶子结点的权值。其余结点的值为 $p_i$ 的概率为它子节点的最大值，$1 - p_i$ 的概率为它子节点的最小值。设 $1$ 号结点的权值有 $m$ 种，第 $i$ 小的值的概率为 $D_i(D_i > 0)$，求 $\sum_{i = 1}^{m} i\times D_i \times Val_i$。

设 $f_{i,j}$ 表示 $i$ 结点值为 $j$ 的概率。

则 $f_{i,j} = f_{ls,j} \times (\sum_{k=1}^{j-1} f_{rs,k} \times p_i + \sum_{k=j+1}^{n} f_{rs,k} \times (1 - p_i)) + f_{rs,j} \times (\sum_{k=1}^{j-1} f_{ls,k} \times p_i + \sum_{k=j+1}^{n} f_{ls,k} \times (1 - p_i))$。

我们可以在线段树合并中维护。

细节比较多。

### [[HEOI2016/TJOI2016] 排序](https://www.luogu.com.cn/problem/P2824)

很有趣的题。

> 题目大意：给你个排列。做 $m$ 次排序，给区间 $[l,r]$，让你按逆序或正序排，求最后 $q$ 上的位置。

本身不好想。考虑简化，只有 $0|1$。设 $cnt1$ 为区间内 $1$ 的个数。则正序即为将 $l \sim cnt1 - 1$ 变为 $0$，$cnt1 \sim  r$ 变为 $1$。逆序同理。那么，使用线段树维护即可。

回到本题。想办法，将序列变为 $0|1$。那么，枚举 $a_q$ 为多少。按大小转化为 $0|1$。若 $a_q$ 转化后最后为 $1$，则合法。

可以注意到具有单调性，二分即可。

### [[POI2011] ROT-Tree Rotations](https://www.luogu.com.cn/problem/P3521)

> 题目大意：给一个 $n$ 个叶子结点的二叉树。叶子结点有权值 $p_i$，$p$ 是一个排列。保证一个结点有 $0|2$ 个儿子。你可以交换若干个结点的左右子树，求最后按先序排列后，叶子结点构成的序列逆序对的最小数量。

容易发现，交换一个结点，只对子树内的逆序对产生影响。

我们考虑交换一个结点后，逆序对的变化。设原来右子树对左子树的逆序对数量为 $a$，左子树大小 $sz_{ls}$，右子树大小 $sz_{rs}$。那么交换后，逆序对有 $b = sz_{ls} * sz_{rs} - a$ 个。我们只需求出 $a,b$，答案加上较小的一个即可。线段树合并就可以处理出逆序对数。只需在合并时求即可。

### [[AGC002D] Stamp Rally](https://www.luogu.com.cn/problem/AT_agc002_d)

> 题目大意：给一个连通图。$q$ 次询问。每次给 $x$，$y$。从两点出发，使得经过的点的数量等于 $z$。求经过的边最大编号最小是多少。

看到最大值最小。考虑二分。若只有一个询问。我们可以将边权小于等于 $mid$ 的边领出来。然后用并查集合并。最后看 $x$，$y$ 是否在同一个连通块内。不在的话使得 $siz_{get(x)} + siz_{get(y)} \ge z$，否则 $siz_{get(x)} \ge z$ 就返回 $1$。

多个询问。整体二分即可。

### [[NOIP2012 提高组] 疫情控制](https://www.luogu.com.cn/problem/P1084)

> 题目大意：给一棵有 $n$ 个点，以 $1$ 为根的树。有 $m$ 个军队。先给出军队的位置。可以移动军队，每过一条边花 $w_i$ 的时间。移动完毕军队所在的点不能走。军队最后不能在根。求使得 $1$ 不与任何叶子联通花费的最小时间。

二分答案。

二分给定一个时间限制 $lim$。考虑如何写 `check`。贪心的，除根以外，其他点均不会往下走。因为向上走（除走到根）一定能覆盖更多的叶子。

那么，我们将所有军队尽量向上移动，到根的儿子为止。可以用倍增实现。若到了根的儿子，求出剩余时间 $more_i = lim - cnt - w_{1->i}$。若有剩余时间，将这个结点（移动后）和 $more_i$ 存下来。否则打上标记。

然后，我们 dfs 即可求出哪些以根的儿子为根的子树需要从其他子树要来军队。

但是，我们注意到，有剩余时间的我们并没有打上标记。所以我们需要求出那些点留在子树内即可。

先给出结论：$more_i < w_{1->i}$ 的且这个子树的叶子没完全覆盖，这样的不用动这个点，同时，这个子树就被这个点覆盖。

感性证明：有一个满足上述条件的点 $x$，和一个可以移动的点 $y$ 且满足 $more_y \ge w_{1->i}$，那么 $x$ 之后可以移动到的点 $y$ 一定能到，$y$ 能到的，$x$ 不一定能到。那么就别浪费 $y$ 去 $x$，$x$ 去其他点。

但是根据上面结论，$more_x$ 一定比其他在同一个点的 $more_x$ 小才行，所以要排序。

最后使用双指针判断即可。

### [[NOIP2015 提高组] 运输计划](https://www.luogu.com.cn/problem/P2680)

> 题目大意：给一棵 $n$ 个点树。每过一条边的花费为 $w_i$。给你 $m$ 条 $u$ 到 $v$ 的**路径**。你可以将一条边的权值变为 $0$。求过 $m$ 条路径花费最多的最少是多少。

最大值最小。二分答案。

考虑先求出每个 $u$ 到 $v$ 的距离。然后枚举时间 $lim$。若 $dis_i > lim$，则说明 $u_i$ 到 $v_i$ 路径上需要减去一个 $dis_i - lim$ 的边权。放到链上，发现是区间覆盖问题。那么在树上，使用树上差分。若存在一条边被所有 $dis_i > lim$ 的路径覆盖，且每个 $dis_i$ 减去他的边权都 $\le lim$ 则说明 $lim$ 可行。

### [[JOI Open 2016] 销售基因链](https://www.luogu.com.cn/problem/P9196)

> 题目大意：给 $n$ 个字符串。字符串由 `A`，`G`，`U`，`C` 组成（保证每个字符串都包含四种字母）。$m$ 组查询。每次给两个字符串 $p$，$q$，求有多少个字符串存在前缀 $p$，后缀 $q$。

首先，前缀，后缀，字符串。容易想到 Trie。对 $n$ 个字符串，我们用两个 Trie，一个放前缀，一个放后缀（将原串 `reverse` 即可）。

考虑查询。先简化，若只给前缀。那么找到 $p$ 对应的结点，其子树内存在的字符串个数即是答案。回到原问题，就是那些字符串同时存在在两个子树内。那么，我们可以使用 dfn 序转化成求二维数点。

### [餐巾计划问题](https://www.luogu.com.cn/problem/P1251)

> 题目大意：略。

经典的网络流。

考虑拆点。将 $i$ 拆为 $i,i'$。

对于 $i'$ 我们将他变为接收之前用剩的毛巾。

对于 $i$ 我们将他变为使用毛巾。

考虑如何建图。

我们每天会有 $i$ 用剩的和之前用剩的。

将源点 $s$ 与 $i'$ 连一条容量为 $r_i$，权值为 $0$ 的边。表示 $i$ 用剩的。

$(i - 1)'$ 与 $i'$ 连一条容量为 $inf$，权值为 $0$ 的边，表示继承之前用剩的。

$s$ 连向 $i$ 一条 $r_i$，$p$ 的边，表示新买。

$i$ 要连向 $t$，容量为 $r_i$。

对于送去洗的。将 $i'$ 连向对应的 $j$ 即可。

最后跑最小费用最大流。

### [[CTSC1999] 家园 / 星际转移问题](https://www.luogu.com.cn/problem/P2754)

> 题目大意：略。

对我来说，想不出来。看了题解。

网络流。

在这里，汇点当然是月球。

考虑枚举天数，每次新建点，然后连边。最后若最大流跑出来的大于等于总人数则说明合法，输出答案即可。

### [Drazil and Morning Exercise](https://www.luogu.com.cn/problem/CF516D)

> 题目大意：
> - 给定一棵 $n$ 个点的树，边有边权。
> - 定义 $f_x = \max_{i=1}^n \text{dist}(x,i)$。
> - $q$ 次询问最大的满足 $\max_{x \in s} f_x - \min_{x \in s} f_x \le l$ 的连通块 $s$ 包含的点数。

比较难想，是我太菜了。

有一个很重要的性质。根据题目，显然与直径有关。因为 $f_x$ 是 $x$ 到达的最长距离，所以 $x$ 到达的一定是直径的端点，且大于等于 $f_{树的中点}$。

那么我们以树的中点为根重新建树。

我们可以发现：$f$ 是随深度单调不降的。

我们枚举 $f_{max}$，可以排序后用双指针求出 $f_{min}$，这是一段区间。我们将区间内的数合并。找到最大的连通块。

最后将 $max$ 的贡献删去即可。