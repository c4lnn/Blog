---
title: LibreOJ 10173.「一本通 5.4 练习 2」炮兵阵地
date: 2021-04-19 22:54:33
tags: [状压 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://loj.ac/p/10173>

# 题意

矩阵中有山和平地，只能在平地上放置炮兵，炮兵的射程为上下左右两格内。

必须保证所有炮兵不在别的炮兵射程内。

求最大放置炮兵数。

<!--more-->

# 思路

根据题意每行或每列两个炮兵必须相隔两格。

先预处理一行放置炮兵的所有情况，用二进制保存状态，当 $m=10$ 时，合法的只有 $60$ 种状态。

记 $dp_{i,j,k}$ 为第 $i$ 行为 $j$ 状态，$i-1$ 行为 $k$ 状态的最大放置数：

$dp_{i,j,k}=\max(dp_{i-1,k,z})$

转移时需要判断每行放置炮兵的位置是否有山以及 $x,y,z$ 是否两两合法。

同样用二进制保存山的状态，存在山的位置为 $1$，显然当两个状态按位与的值非 $0$ 时，这两个状态不能共存。

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
const int N=105;
int n,m,dp[N][N*10][N*10],tot[N*10],st[N*10];
set<int> s;
void dfs(int pos,int t,int d) {
    s.insert(t);
    tot[t]=d;
    for(int i=pos+3;i<m;i++) dfs(i,t|(1<<i),d+1);
}
bool check(int i,int j,int a,int b) {
    if(j<=0) return true;
    if(a&st[i]||b&st[j]||a&b) return false;
    return true;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n>>m;
    for(int i=1;i<=n;i++)
        for(int j=0;j<m;j++) {
            char c;
            cin>>c;
            if(c=='H') st[i]|=1<<j;
    }
    dfs(-3,0,0);
    for(int i=1;i<=n;i++)
        for(auto x:s)
            for(auto y:s) {
                if(!check(i,i-1,x,y)) continue;
                for(auto z:s)
                    if(check(i,i-2,x,z)&&check(i-1,i-2,y,z))
                        dp[i][x][y]=max(dp[i][x][y],dp[i-1][y][z]+tot[x]);
            }
    int res=0;
    for(auto x:s)
        for(auto y:s)
            res=max(res,dp[n][x][y]);
    cout<<res<<'\n';
    return 0;
}
```