---
title: 2019ICPC沈阳网络赛 D. Fish eating fruit
date: 2021-08-27 15:18:40
tags: [换根 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://nanti.jisuanke.com/t/41403>

# 题意

求树上所有 $n*(n-1)$ 条路径的长度按模 $3$的余数分成三个集合，求各个集合的和。

<!--more-->

# 思路

记 $dp1_{i,j}$ 为以 $1$ 为根，$i$ 的子节点中距离 $i$ 的长度模 $3$为 $j$ 的长度之和。

记 $dp2_{i,j}$ 为以 $1$ 为根，$i$ 的子节点中距离 $i$ 的长度模 $3$为 $j$ 的点数。

记 $dp3_{i,j}$ 为以 $i$ 为根，距离 $i$ 的长度模 $3$为 $j$ 的长度之和。

记 $dp4_{i,j}$ 为以 $i$ 为根，距离 $i$ 的长度模 $3$为 $j$ 的点数。

以 $1$ 为根，先作一次 DFS，计算出 $dp1,dp2,dp3_{1,j},dp4_{1,j}$：

$$\begin{aligned}
dp1_{u,j}=&(\sum_{v\in son(u)}[w==j]*w)+(\sum_{v\in son(u)}dp1_{v,j-w}+dp2_{v,j-w}*w)\\
dp2_{u,j}=&(\sum_{v\in son(u)}[w==j]*w)+(\sum_{v\in son(u)}dp2_{v,j-w})\\
dp3_{1,j}=&\sum_{v=2}^n{[w==j]*w}\\
dp4_{1,j}=&\sum_{v=2}^n{[w==j]}
\end{aligned}$$

以 $1$ 为根，再作一次 DFS，从上到下作换根 DP：

$$\begin{aligned}
dp3_{v,j}=&dp3_{u,j-w}-(dp1_{v,j-2w}+dp2_{v,j-2w}*w)-[w==j-w]*w\\
&+(dp4_{u,j-w}-dp2_{v,j-2w}-[w==j-w]+[w==j])*w+dp1_{v,j}\\
dp4_{v,j}=&dp4_{u,j-w}-dp2_{v,j-2w}-[w==j-w]+[w==i]+dp2_{v,j}
\end{aligned}$$

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
const int MOD=1e9+7;
const int N=1e4+5;
int dp1[N][3],dp2[N][3],dp3[N][3],dp4[N][3],dist[N];
VPII g[N];
int add(int a,int b) {
    a+=b;
    if(a>MOD) a-=MOD;
    return a;
}
int sub(int a,int b) {
    a-=b;
    if(a<0) a+=MOD;
    return a;
}
int mul(int a,int b) {
    return 1ll*a*b%MOD;
}
void dfs1(int u,int fa) {
    for(int i=0;i<3;i++) dp1[u][i]=dp2[u][i]=dp3[u][i]=dp4[u][i]=0;
    for(auto x:g[u]) {
        int v=x.FI,w=x.SE;
        if(v==fa) continue;
        dist[v]=add(dist[u],w);
        dfs1(v,u);
        for(int i=0;i<3;i++) {
            int t=((i-w)%3+3)%3;
            dp1[u][i]=add(add(dp1[u][i],dp1[v][t]),mul(dp2[v][t],w));
            dp2[u][i]+=dp2[v][t];
        }
        dp1[u][w%3]=add(dp1[u][w%3],w);
        ++dp2[u][w%3];
        dp3[1][dist[v]%3]=add(dp3[1][dist[v]%3],dist[v]);
        ++dp4[1][dist[v]%3];
    }
}
void dfs2(int u,int fa) {
    for(auto x:g[u]) {
        int v=x.FI,w=x.SE;
        if(v==fa) continue;
        for(int i=0;i<3;i++) {
            int a=((i-w)%3+3)%3,b=((i-2*w)%3+3)%3;
            dp3[v][i]=sub(sub(dp3[u][a],add(dp1[v][b],mul(dp2[v][b],w))),w%3==a?w:0);
            dp3[v][i]=add(dp3[v][i],mul(sub(sub(dp4[u][a],dp2[v][b]),w%3==a),w));
            if(w%3==i) dp3[v][i]=add(dp3[v][i],w);
            dp3[v][i]=add(dp3[v][i],dp1[v][i]);
            dp4[v][i]=dp2[v][i]+dp4[u][a]-dp2[v][b]-(w%3==a)+(w%3==i);
        }
        dfs2(v,u);
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;
    while(cin>>n) {
        for(int i=1;i<=n;i++) g[i].clear();
        for(int i=1;i<n;i++) {
            int u,v,w;cin>>u>>v>>w;
            ++u,++v;
            g[u].EB(v,w);
            g[v].EB(u,w);
        }
        dfs1(1,0);
        dfs2(1,0);
        VI res(3);
        for(int i=1;i<=n;i++) {
            for(int j=0;j<3;j++) {
                res[j]=add(res[j],dp3[i][j]);
            }
        }
        for(int i=0;i<3;i++) cout<<res[i]<<" \n"[i==2];
    }
    return 0;
}
```