---
title: AtCoder Beginner Contest 194 E. Mex Min
date: 2021-04-12 16:02:43
tags: [枚举]
categories: 解题报告
mathjax: true
---

# 链接

<https://atcoder.jp/contests/abc194/tasks/abc194_e>

# 题意

定义 $mex(x_1,x_2,\dots,x_k)$ 为在 $x_1,x_2,\dots,x_k$ 中未出现的最小自然数。

求 $\min_{i=0}^{n-m}{[mex(a_{i+1},a_{i+2},\dots,a_{i+m})]}$。

数据范围：

- $0 \le m \le n \le 1.5*10^6$
- $0 \le a_i <n$

<!--more-->

# 思路

记录每个值出现的下标，枚举 $[0,n]$，判断是否存在长度大于 $m$ 的区间中未出现该值。

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
const int N=1500005;
VI pos[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;i++) {
        int x;
        cin>>x;
        pos[x].PB(i);
    }
    for(int i=0;i<=n;i++) {
        if(!SZ(pos[i])) return cout<<i<<'\n',0;
        for(int j=0;j<SZ(pos[i]);j++) {
            if(!j&&pos[i][j]>m) return cout<<i<<'\n',0;
            if(j&&pos[i][j]-pos[i][j-1]-1>=m) return cout<<i<<'\n',0;
            if(j==SZ(pos[i])-1&&n-pos[i][j]>=m) return cout<<i<<'\n',0;
        }
    }
    return 0;
}
```