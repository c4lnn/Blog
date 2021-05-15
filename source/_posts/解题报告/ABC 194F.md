---
title: AtCoder Beginner Contest 194 F. Digits Paradise in Hexadecimal
date: 2021-04-12 15:59:02
tags: [数位 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://atcoder.jp/contests/abc194/tasks/abc194_f>

# 题意

求 $1 \sim n$ 中十六进制表示中严格有 $k$ 个不同的数（$0 \sim 9,a \sim f$）的个数（不包含前导 $0$）。

<!--more-->

# 思路

数位 DP，记录当前位之前存在几个不同的数即可。

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
const LL MOD=1e9+7;
const int N=2e5+5;
int k,d[N];
LL dp[N][17];
string s;
int get(char c) {
    if(isdigit(c)) return c-'0';
    else return c-'A'+10;
}
LL dfs(int x,int st,bool a,bool b) {
    if(x==SZ(s)) return !a&&d[st]==k;
    if(!a&&!b&&dp[x][d[st]]!=-1) return dp[x][d[st]];
    int mx=b?get(s[x]):15;
    LL ret=0;
    for(int i=0;i<=mx;i++) {
        if(a&!i&&d[st|(1<<i)]>k) continue;
        ret=(ret+dfs(x+1,(a&!i)?0:st|(1<<i),a&!i,b&(i==mx)))%MOD;
    }
    if(!a&&!b) dp[x][d[st]]=ret;
    return ret;
}
int main() {
    cin>>s>>k;
    for(int i=0;i<1<<16;i++) d[i]=__builtin_popcount(i);
    memset(dp,-1,sizeof dp);
    cout<<dfs(0,0,true,true)<<'\n';
    return 0;
}
```