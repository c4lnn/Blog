---
title: AtCoder Beginner Contest 195 F. Coprime Present
date: 2021-04-11 13:53:23
tags: [质数,欧几里得算法,状压 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://atcoder.jp/contests/abc195/tasks/abc195_f>

# 题意

从 $[a,b]$ $(b-a\le 72)$ 区间内找出不同的数构成一个升序序列，序列中任意两数都互质，求这样不同的序列有多少个。

<!--more-->

# 思路

$\gcd(n,m)=\gcd(n,n-m)\le n-m$ $(n\le m)$

因为 $b-a\le 72$ 所以只需要枚举 $[2,72]$ 之间的质因数，用二进制状态存储序列中的数包含哪些质因数。

从 $a$ 开始枚举，找出当前数中小于等于 $72$ 的质因数，与当前数中质因数没有重集的状态可以进行转移。

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
const LL P[]={2,3,5,7,11,13,17,19,23,29,31,37,41,43,47,53,59,61,67,71};
const int N=1500000;
LL dp[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    LL a,b;
    cin>>a>>b;
    dp[0]=1;
    for(LL i=a;i<=b;i++) {
        int bit=0;
        for(int j=0;j<20;j++) if(i%P[j]==0) bit|=1<<j;
        for(int j=0;j<1<<20;j++) {
            if(bit&j) continue;
            dp[bit|j]+=dp[j];
        }
    }
    LL res=0;
    for(int i=0;i<1<<20;i++) res+=dp[i];
    cout<<res<<'\n';
    return 0;
}
```