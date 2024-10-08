---
title: ARC140B 题解
cover: https://lucky-cloud09.github.io/img/b2.jpg
categories: 题解
date: 2023/10/2
---

## 分析
自然，我们可以想到利用贪心去解题。

我们可以证明，$\texttt{ARC}$ 左右两边 $\texttt{A}$ 和 $\texttt{C}$ 个数多的比少的变为 $\texttt{R}$ 贡献能更多，第奇数次操作比第偶数次能使操作次数更多。

于是，我们可以得出这样的一个算法：

1. 若为奇数次操作那我们将现有的 $\texttt{ARC}$ 中左右两边 $\texttt{A}$ 与 $\texttt{C}$ 最多的取出，将 $\texttt{AC}$ 的个数减一。
1. 若为偶数次操作那我们将现有的 $\texttt{ARC}$ 中左右两边 $\texttt{A}$ 与 $\texttt{C}$ 最少的取出，将其删除。

## Code

为了实现以上操作，我们采用 `multiset` 实现。

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 2e5 + 100;
int n, x;
multiset<int> arc;
string s;

int main() {
	cin >> n;
	cin >> s;
	x = 2;
	for (int i = 1; i < s.size() - 1; i++) {//从一开始，避免数组越界
		if (s[i] == 'R') {//以 R 为中心，统计 A 与 C 的个数
			int k = 0, l = i - 1, r = i + 1;
			while (l >= 0 && r < s.size() && s[l] == 'A' && s[r] == 'C') {
				k++;
				l--, r++;//向两边扩展
			}
			if (k)//如果有 A 与 C
			arc.insert(k);
		}
	}
	int c = 0;
	while (arc.size()) {//只要不为空，那么我们还可以操作
		c++;
		if (c % x == 0) {
			arc.erase(arc.begin());//删除最小的，保个数多的，使答案最多
		}
		else {
			auto it = arc.end();
			it--;
			int y = *it;
			arc.erase(it);//取出最大的
			y--;
			if (y)  arc.insert(y);//若还能操作，就把他插回去
		}
	}
	cout << c << "\n";
	return 0;
}
```
