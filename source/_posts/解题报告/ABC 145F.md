---
title: AtCoder Beginner Contest 145 F. Laminate
date: 2021-04-12 15:56:06
tags: [DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://atcoder.jp/contests/abc145/tasks/abc145_f>

# 题意

$n*m$ 的二维白色网格，$H_i$ 表示第 $i$ 列底端往上 $H_i$ 格需涂成黑色。

每次操作可以选择一段水平方向连续的网格涂黑。

现在可以至多改变 $k$ 列的 $H$ 值，求最小操作数使需涂黑的网格涂黑。

<!--more-->

# 思路

记 $dp_{i,j,0/1}$ 为前 $i$ 列改变了 $j$ 列的 $H$ 值且第 $i$ 列是否改变的最小操作数：

- $dp_{i,j,1}=\min(dp_{i-1,j-1,0},dp_{i-1,j-1,1})$
- $dp_{i,j,0}=\min\{dp_{k,j-(i-1-k),0}+\max(0,h[i]-h[k])\}$ $(\max(0,i-j-1) \le k < i)$

如果某列改变了 $H$ ，那么相当于把这列删除。

因此我们需要枚举在第 $i$ 列之前最后一列未改变 $H$ 的列 $k$，那么 $(k,i)$ 区间内的列全都改变了$H$，所以还得保证 $j$ 足够大。

如果第 $i$ 列比第 $k$ 列高，那么还需操作 $H_i-H_k$ 次，否则无需多余操作。

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
const int N=305;
LL h[N],dp[N][N][2];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;i++) cin>>h[i];
    memset(dp,0x3f,sizeof dp);
    dp[1][0][0]=h[1];
    dp[0][0][0]=dp[1][1][1]=0;
    for(int i=2;i<=n;i++)
        for(int j=0;j<=m;j++) {
            if(j>i) break;
            if(j) dp[i][j][1]=min(dp[i-1][j-1][0],dp[i-1][j-1][1]);
            for(int k=max(0,i-j-1);k<=i-1;k++)
            	dp[i][j][0]=min(dp[i][j][0],dp[k][j-(i-1-k)][0]+max(0ll,h[i]-h[k]));
        }
    LL mn=LLONG_MAX;
    for(int i=0;i<=m;i++) mn=min({mn,dp[n][i][0],dp[n][i][1]});
    cout<<mn<<'\n';
    return 0;
}
```