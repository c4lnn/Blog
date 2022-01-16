---
title: 2021CCPC东北省赛 C. Vertex Deletion
date: 2021-09-14 00:01:12
tags: [树形 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://codeforces.com/gym/103145/problem/C>

# 题意

树上删除一个点集使剩下的节点没有孤立点，求方案数。

<!--more-->

# 思路

以 1 为根节点，

记 $dp_{i,0}$ 为节点 $i$ 的子树中 删除 $i$ 后合法的方案数；

记 $dp_{i,1}$ 为节点 $i$ 的子树中 保留 $i$ 且 $i$ 为孤立点的方案数；

记 $dp_{i,2}$ 为节点 $i$ 的子树中 保留 $i$ 后合法的方案数。


$$\begin{cases}
dp_{i,0}=\prod_{j\in son(i)}{dp_{j,0}+dp_{j,2}}\\
dp_{i,1}=\prod_{j\in son(i)}{dp_{j,0}}\\
dp_{i,2}=-dp_{i,1}+\prod_{j\in son(i)}{dp_{j,0}+dp_{j,1}+dp_{j,2}}\\
\end{cases}$$

$dp_{i,0}+dp_{i,1}$ 就是答案。

# 代码

```cpp
#include <bits/stdc++.h>
#define SZ(x) (int)(x).size()
#define ALL(x) (x).begin(),(x).end()
#define PB push_back
#define EB emplace_back
#define MP make_pair
#define FI first
#define SE second
using namespace std;
typedef double DB;
typedef long double LD;
typedef long long LL;
typedef unsigned long long ULL;
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
// head
const LL MOD=998244353;
const int N=1e5+5;
LL dp[N][3];
VI g[N];
void dfs(int u,int fa) {
    dp[u][0]=dp[u][1]=dp[u][2]=1;
    for(auto v:g[u]) if(v!=fa) {
        dfs(v,u);
        dp[u][0]=dp[u][0]*(dp[v][0]+dp[v][2])%MOD;
        dp[u][1]=dp[u][1]*dp[v][0]%MOD;
        dp[u][2]=dp[u][2]*(dp[v][0]+dp[v][1]+dp[v][2])%MOD;
    }
    dp[u][2]=(dp[u][2]-dp[u][1]+MOD)%MOD;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        int n;cin>>n;
        for(int i=1;i<=n;i++) g[i].clear();
        for(int i=1;i<n;i++) {
            int u,v;cin>>u>>v;
            g[u].PB(v),g[v].PB(u);
        }
        dfs(1,0);
        cout<<(dp[1][0]+dp[1][2])%MOD<<'\n';
    }
    return 0;
}
```