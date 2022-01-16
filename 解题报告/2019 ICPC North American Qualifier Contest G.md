---
title: 2019 ICPC North American Qualifier Contest G. Research Productivity Index
date: 2021-04-11 18:10:59
tags: [期望 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://ac.nowcoder.com/acm/contest/13168/G>

# 题意

有 $n$ 篇文章，第 $i$ 篇文章中的概率为 $a_i$，定义 $i$ 篇文章中 $j$ 篇的生产指数为 $j^{j/i}$。

现在可以自己选择投哪几篇文章，求生产指数的最大期望。

<!--more-->

# 思路

记 $f_{i,j}$ 为 $i$ 篇文章中 $j$ 篇 的生产指数，$p_{i,j}$ 为 $i$ 篇文章中 $j$ 篇的概率，期望 $E_{i}=\sum_{j=1}^if_{i,j}p_{i,j}$。

为了让概率尽可能大，将文章按中的概率从大到小排序，$p_{i,j}=p_{i-1,j}(1-a_i)+p_{i-1,j-1}a_i$。

最终答案为 $\max_{i=1}^n\{E_i\}$。

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
const int N=105;
DB a[N],dp[N][N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++) cin>>a[i],a[i]/=100;
    sort(a+1,a+1+n,greater<DB>());
    dp[0][0]=1;
    for(int i=1;i<=n;i++) dp[i][0]=dp[i-1][0]*(1-a[i]);
    for(int i=1;i<=n;i++)
        for(int j=1;j<=i;j++)
            dp[i][j]=dp[i-1][j]*(1-a[i])+dp[i-1][j-1]*a[i];
    DB res=0;
    for(int i=1;i<=n;i++) {
        DB sum=0;
        for(int j=1;j<=n;j++) sum+=dp[i][j]*pow(j,1.0*j/i);
        res=max(res,sum);
    }
    cout<<fixed<<setprecision(9)<<res<<'\n';
    return 0;
}
```