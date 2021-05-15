---
title: AtCoder Beginner Contest 200 E. Patisserie ABC 2
date: 2021-05-10 19:45:00
tags: [组合数学]
categories: 解题报告
mathjax: true
---

# 链接

<https://atcoder.jp/contests/abc200/tasks/abc200_e>

# 题意

求第 $k$ 个 $(a,b,c)$ $(0<a,b,c\le n)$，满足一下要求：

- $a+b+c$ 小的排在前面。
- $a+b+c$ 相同，$a$ 小的排在前面。
- $a+b+c$ 相同，$a$ 相同，$b$ 小的排在前面。

<!--more-->

# 思路

首先计算 $(a,b,c)$ 满足 $a+b+c=sum$ $(0<a,b,c\le n)$ 的方案数。

容斥原理：

$(a,b,c)$ 满足 $a+b+c=sum$ $(a,b,c>0)$ 的方案数为$\binom{sum-1}{2}$。

考虑减去 $a,b,c > n$ 的方案数 $\binom{sum-3n-1}{3}$，因为 $sum\le 3n$，所以不存在这样的方案。

再考虑减去一个位置和两个位置大于 $n$ 的方案数：

因为 $sum \le 3n \longrightarrow sum-n \le 2n$，所以最多只有一个位置会大于 $n$，

将一个位置 $+n$ 后，最多有两个位置大于 $n$，其中两个位置大于 $n$ 算了两遍，因此要加上 $3$ 次有两个位置大于 $n$ 的，即 $\binom{sum-2n-1}{3}$。

因此 $(a,b,c)$ 满足 $a+b+c=sum$ $(a,b,c\le n)$ 的方案数为 $\binom{sum-1}{2}-\binom{sum-n-1}{3}+3\binom{sum-2n-1}{3}$。

根据 $k$ 确定 $sum$，然后枚举 $a$ 的值求出答案。

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
LL n,k;
LL calc(LL sum) {
    LL ret=(sum-1)*(sum-2)/2;
    if(sum-n-2>0) ret-=3*(sum-n-1)*(sum-n-2)/2;
    if(sum-2*n-2>0) ret+=3*(sum-2*n-1)*(sum-2*n-2)/2;
    return ret;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n>>k;
    for(LL i=3;i<=3*n;i++) {
        LL ret=calc(i);
        if(ret<k) k-=ret;
        else {
            for(LL j=max(1ll,i-2*n);j<=n;j++) {
                LL t=min(n,i-j-1)-max(1ll,i-j-n)+1;
                if(t<k) k-=t;
                else return cout<<j<<' '<<max(1ll,i-j-n)+k-1<<' '<<i-j-(max(1ll,i-j-n)+k-1)<<'\n',0;
            }
        }
    }
    return 0;
}
```