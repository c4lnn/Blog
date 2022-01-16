---
title: 牛客挑战赛10 B. 小H和游戏
date: 2021-06-10 22:54:06
tags: [树形 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://ac.nowcoder.com/acm/contest/72/C>

# 题意

树形图上，对某点进行轰炸，会波及到距离该点不超过 $2$ 的点

$q$ 次轰炸，每次都要输出该点受损次数

<!--more-->

# 思路

记 $tot[x][0]$ 为 $x$ 受损的次数，$tot[x][1]$ 为 $x$ 的孩子受损的次数，$tot[x][2]$ 的孩子的孩子受损的次数

假如轰炸 $x$：

$x$ 的父亲的父亲受损：$tot[fa[fa[x]]][0]++$

$x$ 的父亲受损：$tot[fa[x]][0]++$

$x$ 以及 $x$ 的兄弟受损：$tot[fa[x]][1]++$

$x$ 的孩子受损：$tot[x][1]++$

$x$ 的孩子的孩子受损：$tot[x][2]++$

$x$ 的总受损次数为：$tot[x][0]+tot[fa[x]][1]+tot[fa[fa[x]]][2]$

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
typedef long long ll;
const int N=750005;
vector<int>g[N];
int fa[N],tot[N][3];
void dfs(int u) {
    for(auto v:g[u]) {
        if(v==fa[u]) continue;
        fa[v]=u;
        dfs(v);
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,q;
    cin>>n>>q;
    for(int i=1;i<n;i++) {
        int u,v;
        cin>>u>>v;
        g[u].pb(v);
        g[v].pb(u);
    }
    dfs(1);
    for(int i=1;i<=q;i++) {
        int x;
        cin>>x;
        tot[x][1]++,tot[x][2]++;
        if(fa[fa[x]]) tot[fa[fa[x]]][0]++;
        if(fa[x]) tot[fa[x]][0]++,tot[fa[x]][1]++;
        else tot[x][0]++;
        cout<<tot[x][0]+tot[fa[x]][1]+tot[fa[fa[x]]][2]<<endl;
    }
    return 0;
}
```