---
title: LibreOJ 2674. 美食节
date: 2021-06-07 14:35:35
tags: [最小费用最大流]
categories: 解题报告
mathjax: true
---

# 链接

<https://loj.ac/p/2674>

# 题意

共有 $n$ 种不同的菜品，共有 $m$ 个厨师来制作这些菜品，厨师每次只能制作一人份

每个同学点一种菜品，记有 $p_i$ 个同学点了第 $i$ 种菜品

第 $j$ 个厨师制作第 $i$ 种菜品的时间记为 $t_{i,j}$

如果一个同学点的菜是某个厨师做的第 $k$ 道菜，则他的等待时间就是这个厨师制作前 $k$ 道菜的时间之和，而总等待时间为所有同学的等待时间之和。

求最小的总等待时间

<!--more-->

# 思路

先将源点与 $n$ 种菜品相连，容量为 $p_i$，费用为 $0$

因为做某道菜对答案的贡献取决于这是该厨师做的倒数第几倒菜，所以我们建立某点表示这是某厨师做的倒数第几道菜，总共需 $m*\sum_{i=1}^{n}{p_i}$ 个点，将每种菜品与这些点相连，容量为 $1$，费用为 $t_{ij}*k$（$i$ 为第 $i$ 种菜品，$j$ 为第 $j$ 个厨师，$k$ 为这是该厨师做的倒数第 $k$ 道菜），再将这些点和汇点相连，容量为1，费用为0

这样建图我们需要建立 $m*\sum_{i=1}^{n}{p_i}+n+2$ 个点，$n+(n+1)*m*\sum_{i=1}^{n}{p_i}$ 条边，跑最小费用最大流的话明显超时

可以发现在每轮选择最优方案中，某个厨师做了倒数第 $i$ 道菜，那么下轮要么不选这个厨师，要么选该厨师做倒数第 $i+1$ 道菜，不可能出现做第 $\ge i+1$ 道菜（费用大）

所以我们先建立每个厨师做倒数第一道菜的边，每次增广时记录哪些厨师做了菜，再建新的边

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
typedef pair<int,int> pii;
typedef vector<int> vi;
//head
const int N=33000;
const int INF=0x3f3f3f3f;
int n,m,s,t,cur[N],cnt,dist[N],cost,tt[50][200],p[1000];
bool st[N];
struct E {
    int v;
    int c,w;
    int a,b;
    E(){}
    E(int v,int c,int w,int a,int b):v(v),c(c),w(w),a(a),b(b){}
};
vector<E> e;
vi G[N];
vector<pii> ud;
void init() {
    e.clear();
    for(int i=1;i<=n;i++) G[i].clear();
}
void addedge(int u,int v,int c,int w,int a,int b) {
    e.push_back(E(v,c,w,a,b));
    e.push_back(E(u,0,-w,a,b));
    G[u].push_back(e.size()-2);
    G[v].push_back(e.size()-1);
}
bool spfa() {
    for(int i=1;i<=n;i++) dist[i]=INF,st[i]=false;
    queue<int> q;
    q.push(s);
    dist[s]=0;
    while(!q.empty()) {
        int u=q.front();
        q.pop();
        st[u]=false;
        for(auto x:G[u]) {
            int &v=e[x].v,&c=e[x].c,&w=e[x].w;
            if(c>0&&dist[v]>dist[u]+w) {
                dist[v]=dist[u]+w;
                if(!st[v]) q.push(v),st[v]=true;
            }
        }
    }
    return dist[t]!=INF;
}
int dfs(int u,int a) {
    if(u==t) return a;
    int f,flow=0;
    st[u]=true;
    for(int &i=cur[u];i<G[u].size();i++) {
        int &v=e[G[u][i]].v,&c=e[G[u][i]].c,&w=e[G[u][i]].w;
        if(st[v]||c<=0||dist[v]!=dist[u]+w||(f=dfs(v,min(a,c)))<=0) continue;
        if(v==t) ud.push_back(mp(e[G[u][i]].a,e[G[u][i]].b));
        c-=f;
        e[G[u][i]^1].c+=f;
        a-=f;
        flow+=f;
        cost+=f*w;
        if(a==0) break;
    }
    st[u]=false;
    return flow;
}
void dinic(int x) {
    n=t+cnt;
    while(spfa()) {
        for(int i=1;i<=n;i++) cur[i]=0;
        dfs(s,INF);
        for(int j=0;j<ud.size();j++) {
            n++;
            addedge(n,t,1,0,ud[j].fi,ud[j].se+1);
            for(int i=1;i<=x;i++) addedge(i,n,1,(ud[j].se+1)*tt[i][ud[j].fi],0,0);
        }
        ud.clear();
    }
    printf("%d\n",cost);
}
int main() {
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++) scanf("%d",&p[i]);
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
            scanf("%d",&tt[i][j]);
    s=n+1,t=n+2;
    for(int i=1;i<=n;i++) addedge(s,i,p[i],0,0,0);
    for(int i=1;i<=m;i++) {
        int x=t+(++cnt);
        addedge(x,t,1,0,i,1);
        for(int j=1;j<=n;j++) addedge(j,x,1,tt[j][i],0,0);
    }
    dinic(n);
    return 0;
}
```