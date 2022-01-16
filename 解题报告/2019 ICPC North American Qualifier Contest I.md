---
title: 2019 ICPC North American Qualifier Contest I. Slow Leak
date: 2021-04-11 18:07:38
tags: [最短路]
categories: 解题报告
mathjax: true
---

# 链接

<https://ac.nowcoder.com/acm/contest/13168/I>

# 题意

$n$ 个点 $m$ 条无方向边，有 $t$ 个加油站，当移动距离大于 $d$ 则将油耗尽，无法移动，求从点 $1$ 走到点 $n$ 的最短距离。

<!--more-->

# 思路

先用 Floyd 更新两点之间最短距离，再重新建新图，若加油站，$1$，$n$ 这些点中两点之间最短距离小于等于$d$，则加一条边，跑 Dijkstra 求出答案。

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
const LL INF=0x3f3f3f3f3f3f3f3f;
const int N=505;
int n,m,t;
LL o,d[N][N],dist[N];
VI g[N];
VI a;
bool st[N];
void floyd() {
    for(int k=1;k<=n;k++)
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++)
                d[i][j]=min(d[i][j],d[i][k]+d[k][j]);
}
void dijkstra() {
    priority_queue<pair<LL,int>,vector<pair<LL,int>>,greater<pair<LL,int>>> q;
    for(int i=1;i<=n;i++) dist[i]=INF;
    q.emplace(0,1);
    dist[1]=0;
    while(SZ(q)) {
        int u=q.top().SE;
        q.pop();
        if(st[u]) continue;
        st[u]=true;
        for(auto v:g[u]) {
            LL w=d[u][v];
            if(!st[v]&&dist[v]>dist[u]+w) {
                dist[v]=dist[u]+w;
                q.emplace(dist[v],v);
            }
        }
    }
    if(dist[n]==INF) cout<<"stuck"<<'\n';
    else cout<<dist[n]<<'\n';
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n>>m>>t>>o;
    for(int i=1;i<=t;i++) {
        int x;
        cin>>x;
        a.PB(x);
    }
    for(int i=1;i<=n;i++) for(int j=1;j<=n;j++) d[i][j]=INF;
    for(int i=1;i<=m;i++) {
        int u,v,w;
        cin>>u>>v>>w;
        d[u][v]=d[v][u]=w;
    }
    floyd();
    a.PB(1),a.PB(n);
    for(int i=0;i<SZ(a);i++)
        for(int j=i+1;j<SZ(a);j++) {
            if(d[a[i]][a[j]]>o) continue;
            g[a[i]].PB(a[j]),g[a[j]].PB(a[i]);
        }
    dijkstra();
    return 0;
}
```