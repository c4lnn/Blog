---
title: 2021CCPC四川省赛 E. Don’t Really Like How The Story Ends
date: 2021-06-27 23:33:20
tags: [搜索]
categories: 解题报告
mathjax: true
---

# 链接

<https://codeforces.com/gym/103117/problem/E>

# 题意

给定一张无向图，问最少添加多少条边，可以使得 $1, 2,\dots, n$ 是这张图的一个合法 DFS 序列。

<!--more-->

# 思路

从 $1$ 开始 DFS，按照与当前点相邻的点从小到大访问。

当要访问 $v$ 时，存在比 $v$ 小的点还没访问，此时必须加边访问还没访问到的点，

在结束 DFS 时，即在 $1$ 退出前，如果还有没访问到的点，则必须加边。

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
const int N=1e5+5;
VI g[N];
int n,t,cnt;
bool st[N];
void dfs(int u) {
    st[u]=true;
    for(auto v:g[u]) if(!st[v]) {
        while(t<v) ++cnt,dfs(t++);
        if(t>v) continue;
        dfs(t++);
    }
    if(u==1) while(t!=n+1) ++cnt,dfs(t++);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        int m;cin>>n>>m;
        for(int i=1;i<=n;i++) {
            g[i].clear();
            st[i]=false;
        }
        for(int i=1;i<=m;i++) {
            int u,v;cin>>u>>v;
            g[u].PB(v);
            g[v].PB(u);
        }
        t=2,cnt=0;
        for(int i=1;i<=n;i++) sort(ALL(g[i]));
        dfs(1);
        cout<<cnt<<'\n';
    }
    return 0;
}
```