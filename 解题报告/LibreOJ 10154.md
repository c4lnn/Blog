---
title: LibreOJ 10154. 选课
date: 2021-06-10 16:20:45
tags: [树形 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://loj.ac/p/10154>

# 题意

给出 $n$ 个带权节点，每个节点可能有父节点

如果你要选择一个节点，那么你也必须选择它的父节点

求选择 $m$ 个节点能得到最大权值是多少

<!--more-->

# 思路

背包类树形 DP

$n$ 个节点构成了一个森林结构，构造一个 $0$ 节点让森铃转化以 $0$ 节点为根的树

将本题转化为分组背包模型：

对于节点 $x$，总体积为 $t$，有 $p=son(x)$ 组物品，每组物品有 $t-1$ 个，第 $i$ 组的第 $j$ 个物品体积为 $j$：

$dp[x][t]=w[x]+\max\limits_{t-1=\sum_{i=1}^{p}{c_i}} \sum_{i=1}^{p}{dp[son(x)_{i}][c_i]}$

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=305;
int n,m,w[N],dp[N][N];
vector<int>son[N];
void dfs(int u) {
    for(auto x:son[u]) {
        dfs(x);
        for(int t=m;t;t--)
            for(int j=1;j<=t;j++)//当u为0时，u不占体积，因此j可以等于t
                dp[u][t]=max(dp[u][t],dp[u][t-j]+dp[x][j]);
    }
    if(u) for(int t=m;t;t--) dp[u][t]=dp[u][t-1]+w[u];//当u不为0时，u占一格体积
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin>>n>>m;
    for(int i=1;i<=n;i++) {
        int a,b;
        cin>>a>>b;
        son[a].push_back(i);
        w[i]=b;
    }
    dfs(0);
    cout<<dp[0][m]<<endl;
    return 0;
}
```