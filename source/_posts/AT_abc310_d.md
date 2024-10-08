---
title: AT_abc310_d
cover: https://lucky-cloud09.github.io/img/b6.jpg
categories: 题解
date: 2023/10/2
---


一道比较简单的爆搜题。~~虽然在考场上没写出来。~~
## 分析
我们可以直接枚举每个人加入哪个团队（如果团队中没有与他相斥的人）。然后答案加一就可以了。

为了是答案更优，我们可以小小地剪一剪枝。

1. 如果剩下的不能满足每一组都有人，就新建一个组，不考虑加入新组。
1. 如果当前组数与要求组数相同就不新建小组了。

但其实，这题的难点是在判重，我们可以用状压~~应该是吧~~去做。也就是每一个人加入的小组转为一个整数。但是，一定要注意，要边建小组边考虑加入目前建了那些组。

因为状态会爆数组所以用 `map` 标记即可。



## Code
马蜂不好，不喜勿喷。
```cpp
#include <bits/stdc++.h>
#define ll long long
#define vint vector<int>
#define ull unsigned ll
using namespace std;

int n, t, m, ans, mp[20][20];
vint v[20];
map<ll, int> mps;

void dfs(int x, int tm, ll num) {//x 传的是当前第几个人，tm 是目前有几个队伍，因为与队伍号无关，所以有多少组建多少组，num 是状态。
	if (x == n + 1 && tm == t && !mps[num]) {//处理边界，团队数要符合条件。
		ans++;
		mps[num] = 1;//标记状态。
//		cout << num << "\n";
		return;
	}
	else if(x == n + 1) {
		return;
	}
	if (t - tm < n - x + 1 && tm != 0) {//小剪枝
		for (int i = 1; i <= tm; i++) {
			if (!mp[i][x]) {//如果里面没有与他相斥的人
				for (int j = 0; j < v[x].size(); j++) mp[i][v[x][j]]++;//标记其他人不能进。注意：一定是++，不能是等于一，否则，可能其他标记了同样的人后面删除标记，就可能会使相斥的人在一个组。
				num = num * 10 + i - 1;//状压
				dfs(x + 1, tm, num);
				for (int j = 0; j < v[x].size(); j++) mp[i][v[x][j]]--;//删除标记。
				num /= 10;
			}
		}
	}
	if (tm == t) return;//如果队够了就不新建了
	++tm;
	for (int i = 0; i < v[x].size(); i++)
		mp[tm][v[x][i]]++;
	num = num * 10 + tm - 1;//状压
	dfs(x + 1, tm, num);
	for (int i = 0; i < v[x].size(); i++)
		mp[tm][v[x][i]]--;
	num /= 10;
}

int main() {
	cin >> n >> t >> m;
	for (int i = 1; i <= m; i++) {
		int x, y;
		cin >> x >> y;
		v[x].push_back(y);//标记相斥的人。
		v[y].push_back(x);//同上
	}
	dfs(1, 0, 0);
	cout << ans << endl;
	return 0;
}
```
