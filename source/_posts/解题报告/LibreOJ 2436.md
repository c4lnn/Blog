---
title: LibreOJ 2436. 糖果
date: 2021-06-05 21:15:08
tags: [差分约束,图的连通性,树形 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://loj.ac/p/2436>

# 题意

- 如果 $X=1$， 表示第 $A$ 个小朋友分到的糖果必须和第 $B$ 个小朋友分到的糖果一样多

- 如果 $X=2$， 表示第 $A$ 个小朋友分到的糖果必须少于第 $B$ 个小朋友分到的糖果

- 如果 $X=3$， 表示第 $A$ 个小朋友分到的糖果必须不少于第 $B$ 个小朋友分到的糖果

- 如果 $X=4$， 表示第 $A$ 个小朋友分到的糖果必须多于第 $B$ 个小朋友分到的糖果

- 如果 $X=5$， 表示第 $A$ 个小朋友分到的糖果必须不多于第 $B$ 个小朋友分到的糖果

每个小朋友都要分到糖果，求至少准备多少糖果，不能满足所有要求，输出 $-1$

<!--more-->

# 思路

按照差分约束建图，求最长路

本题数据范围达到了 $10^5$ 级别，且数据较强，直接跑 SPFA 会超时

建完图后所有边权只有 $0$ 和 $1$，只要环中边权有 $1$ 那么该环一定是正环，无解

所以找出所有强连通分量，如果有解则缩点后按照拓扑序树形 DP 求最长路

# 代码

```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cstring>
#include <cmath>
#include <climits>
#include <string>
#include <vector>
#include <stack>
#include <queue>
#include <deque>
#include <set>
#include <map>
#include <bitset>
#define SZ(x) (int)(x).size()
#define ALL(x) (x).begin(),(x).end()
#define PB push_back
#define MP make_pair
#define FI first
#define SE second
#define int long long
using namespace std;
typedef double DB;
//typedef long long LL;
typedef unsigned long long ULL;
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
//head
const int N=1e5+5;
int n,m,dist[N],cnt,cnt_scc,top,dfn[N],low[N],stk[N],c[N],sz[N],in[N];
bool ins[N];
VPII G[N],g[N];
void tarjan(int u) {
    dfn[u]=low[u]=++cnt;
    stk[++top]=u;
    ins[u]=true;
    for(auto x:G[u]) {
        int v=x.FI;
        if(!dfn[v]) {
            tarjan(v);
            low[u]=min(low[u],low[v]);
        }
        else if(ins[v]) low[u]=min(low[u],dfn[v]);
    }
    if(dfn[u]==low[u]) {
        cnt_scc++;
        int x;
        do {
            x=stk[top--];
            ins[x]=false;
            c[x]=cnt_scc;
            sz[cnt_scc]++;
        } while(u!=x);
    }
}
bool build() {
    for(int i=1;i<=n;i++)
        for(auto x:G[i]) {
            int u=c[i],v=c[x.FI],w=x.SE;
            if(u!=v) {
                g[u].PB(MP(v,w));
                in[v]++;
            }
            else if(w>0) return false;
        }
    return true;
}
void dfs(int u,int fa) {
    for(auto x:g[u]) {
        int v=x.FI,w=x.SE;
        if(v==fa) continue;
        dist[v]=max(dist[v],dist[u]+w);
        dfs(v,u);
    }
}
signed main() {
    scanf("%lld%lld",&n,&m);
    for(int i=1;i<=m;i++) {
        int x,u,v;
        scanf("%lld%lld%lld",&x,&u,&v);
        if(x==1) G[v].PB(MP(u,0)),G[u].PB(MP(v,0));
        else if(x==2) G[u].PB(MP(v,1));
        else if(x==3) G[v].PB(MP(u,0));
        else if(x==4) G[v].PB(MP(u,1));
        else G[u].PB(MP(v,0));
    }
    for(int i=1;i<=n;i++) if(!dfn[i]) tarjan(i);
    if(!build()) puts("-1");
    else {
        for(int i=1;i<cnt_scc;i++) dist[i]=0xc0c0c0c0c0c0c0c0;
        for(int i=1;i<=cnt_scc;i++)
            if(!in[i]) dist[i]=1,dfs(i,0);
        int res=0;
        for(int i=1;i<=cnt_scc;i++) res+=dist[i]*sz[i];
        printf("%lld\n",res);
    }
    return 0;
}
```