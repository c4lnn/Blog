---
title: 牛客练习赛 1 B. 树
date: 2021-06-10 22:46:42
tags: [DP]
categories: 解题报告
mathjax: true
---

# 题目链接

<https://ac.nowcoder.com/acm/contest/2/B>

# 题意

有一颗树，有k种不同颜色的染料给树染色。一个染色方案是合法的，当且仅当对于所有相同颜色的点对 $(x,y)$，$x$ 到 $y$ 的路径上的所有点的颜色都要与 $x$ 和 $y$ 相同,请统计方案数

<!--more-->

# 思路

易得一个节点要么涂和父节点一个颜色，要么涂没有出现过的颜色

假设我们遍历到了第 $x$ 个点已经用了  $y$ 个颜色了

如果要涂和父节点一个颜色，那么方案数就是前 $x-1$ 个点用了 $y$ 个颜色的方案数

如果要涂和父节点不同颜色，那么方案数就是前 $x-1$ 个点用了 $y-1$ 个颜色的方案数 $*(k-(y-1))$

我们发现方案数跟树长什么样并没有关系，我们完全可以转化为一个线性结构求解

设 $f[i][j]$ 为前 $i$ 个节点用了 $j$ 个颜色：

$f[i][j]=f[i-1][j]+f[i-1][j-1]*(k-i+1)$

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int mod=1e9+7;
ll f[400][400];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,k;
    cin>>n>>k;
    int a,b;
    for(int i=1;i<n;i++) cin>>a>>b;
    f[0][0]=1;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=k;j++) {
            f[i][j]=(f[i-1][j]+f[i-1][j-1]*(k-j+1))%mod;
        }
    ll res=0;
    for(int i=1;i<=k;i++) res=(res+f[n][i])%mod;
    cout<<res<<endl;
    return 0;
}
```