---
title: AtCoder Beginner Contest 195 E. Lucky 7 Battle
date: 2021-04-11 17:58:46
tags: [DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://atcoder.jp/contests/abc195/tasks/abc195_e>

# 题意

字符串 $S$ 只包含数字，字符串 $X$ 只包含 `A` 和 `T`，字符串 T 是空串。

当 $X_i$ 为 `A` 时，Aoki 操作，否则 Takahashi 操作。

每次操作可将 $0$ 或 $S_i$ 放在 $T$ 末尾。

如果 $T$ 是 $7$ 的倍数则 Takahashi 获胜，否则 Aoki 获胜。

<!--more-->

# 思路

回合编号从 $0$  开始，总共 $n$ 个回合。

记 $dp_{i,j}$ 为当前为第 $i$ 个回合，该回合之前余数为 $j$ 时 Takahashi 是否能获胜。

若Takahashi 获胜，$dp_{n,0}=1$，然后进行倒推：

- $X_i=$`T`，$dp_{i,j}=dp_{i+1,(j*10+s_i)\%7}|dp_{i+1,j*10\%7}$
当前为第 $i$ 回合且 Takahashi 操作，两种放置中有一种 Takahashi 能获胜即可。
- $X_i=$`A`，$dp_{i,j}=dp_{i+1,(j*10+s_i)\%7}\&dp_{i+1,j*10\%7}$
当前为第 $i$ 回合且 Aoki 操作，必须保证两种放置后 Takahashi 都能获胜。

最后只要判断 $dp_{0,0}$ 是否为 $1$。

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
const int N=2e5+5;
int n,dp[N][7];
string s,x;
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n>>s>>x;
    dp[n][0]=1;
    for(int i=n-1;~i;i--) {
        for(int j=0;j<7;j++) {
            int a=dp[i+1][(j*3+s[i]-'0')%7],b=dp[i+1][j*3%7];
            if(x[i]=='T') dp[i][j]=a|b;
            else dp[i][j]=a&b;
        }
    }
    cout<<(dp[0][0]?"Takahashi":"Aoki")<<'\n';
    return 0;
}
```