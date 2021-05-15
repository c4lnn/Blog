---
title: LibreOJ 6223.「网络流 24 题」汽车加油行驶问题
date: 2021-04-17 20:56:35
tags: [最短路]
categories: 解题报告
mathjax: true
---

# 链接

<https://loj.ac/p/6223>

# 题意

给定一个 $N*N$ 的方形网格，设其左上角为起点，坐标为 $(1,1)$，$X$ 轴向右为正，$Y$ 轴向下为正，每个方格边长为 $1$。

一辆汽车从起点出发驶向右下角终点，其坐标为 $(N,N)$。

在若干个网格交叉点处，设置了油库，可供汽车在行驶途中加油。汽车在行驶过程中应遵守如下规则：

- 汽车只能沿网格边行驶，装满油后能行驶 $K$ 条网格边。出发时汽车已装满油，在起点与终点处不设油库；
- 汽车经过一条网格边时，若其 $X$ 坐标或 $Y$ 坐标减小，则应付费用 $A$，否则免付费用；
- 汽车在行驶过程中遇油库则应加满油并付加油费用 $B$；
- 在需要时可在网格点处增设油库，并付增设油库费用 $C$（不含加油费用 $A$）。

设计一个算法，求出汽车从起点出发到达终点的一条所付费用最少的行驶路线。

<!--more-->

# 思路

分层图最短路。

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
//head
const int DIR[4][2]={-1,0,1,0,0,-1,0,1};
const int N=105;
int n,k,a,b,c,s[N][N],dist[N][N][11];
bool st[N][N][11];
struct R {
    int w,x,y,t;
    R() {}
    R(int w,int x,int y,int t):w(w),x(x),y(y),t(t) {}
    bool operator < (const R &T) const {
        return w>T.w;
    }
};
int dijkstra() {
    priority_queue<R> q;
    memset(dist,0x3f,sizeof dist);
    q.emplace(0,1,1,k);
    dist[1][1][k]=0;
    while(SZ(q)) {
        auto u=q.top();
        q.pop();
        if(u.x==n&&u.y==n) return u.w;
        if(u.t==0||st[u.x][u.y][u.t]) continue;
        st[u.x][u.y][u.t]=true;
        for(int i=0;i<4;i++) {
            int tx=u.x+DIR[i][0],ty=u.y+DIR[i][1];
            if(tx<=0||tx>n||ty<=0||ty>n) continue;
            if(!s[tx][ty]) {
                if(!st[tx][ty][u.t-1]&&u.w+(i==0||i==2?b:0)<dist[tx][ty][u.t-1]) {
                    dist[tx][ty][u.t-1]=u.w+(i==0||i==2?b:0);
                    q.emplace(dist[tx][ty][u.t-1],tx,ty,u.t-1);
                }
                if(!st[tx][ty][k]&&u.w+(i==0||i==2?b:0)+c+a<dist[tx][ty][k]) {
                    dist[tx][ty][k]=u.w+(i==0||i==2?b:0)+c+a;
                    q.emplace(dist[tx][ty][k],tx,ty,k);
                }
            }
            else if(!st[tx][ty][k]&&u.w+(i==0||i==2?b:0)+a<dist[tx][ty][k]) {
                dist[tx][ty][k]=u.w+(i==0||i==2?b:0)+a;
                q.emplace(dist[tx][ty][k],tx,ty,k);
            }
        }
    }
    return 0x3f3f3f3f;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n>>k>>a>>b>>c;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=n;j++)
            cin>>s[i][j];
    cout<<dijkstra()<<'\n';
    return 0;
}
```