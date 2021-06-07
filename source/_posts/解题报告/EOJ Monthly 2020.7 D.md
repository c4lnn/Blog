---
title: EOJ Monthly 2020.7 D. 前缀排序
date: 2021-06-07 14:58:40
tags: [二分,Hash]
categories: 解题报告
mathjax: true
---

# 链接

<https://acm.ecnu.edu.cn/contest/292/problem/D/>

# 题意

将一个 $n$ 位正整数的所有前缀自由调换顺序并首尾拼接，求拼成的最大正整数对 $998244353$ 取模的结果

<!--more-->

# 思路

Hash & 二分

$a+b>b+a$，$a$ 拼在 $b$ 前面较大

取两个前缀 $A$ 和 $B$，设 $|A|<|B|$

由于 $A$ 是 $B$ 的前缀，所以前 $|A|$ 的长度必定相等，那么设二分边界：$l=|A|$，$r=|A|+|B|$

整体再用快速排序即可

注意：二分下界取两个前缀长度最小值

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+5;
const long long MOD=998244353;
const long long HASH_BASE=13331;
const long long HASH_MOD[2]={1000000007,1000000009};
char s[N];
int n,rk[N];
long long ha[2][N],power[2][N],power_10[N],val[N];
void init_hash() {
    power[0][0]=power[1][0]=1;
    for(int i=0;i<2;i++)
        for(int j=1;j<=n;j++) {
            ha[i][j]=(ha[i][j-1]*HASH_BASE%HASH_MOD[i]+s[j])%HASH_MOD[i];
            power[i][j]=power[i][j-1]*HASH_BASE%HASH_MOD[i];
        }
}
pair<int,int> get_hash(int a,int x) {
    if(x<=a) return {ha[0][x],ha[1][x]};
    return {
        (ha[0][a]*power[0][x-a]%HASH_MOD[0]+ha[0][x-a])%HASH_MOD[0],
        (ha[1][a]*power[1][x-a]%HASH_MOD[1]+ha[1][x-a])%HASH_MOD[1]
    };
}
bool cmp(int a,int b) {
    int l=min(a,b),r=a+b;
    while(l<r) {
        int mid=l+r+1>>1;
        if(get_hash(a,mid)==get_hash(b,mid)) l=mid;
        else r=mid-1;
    }
    if(l==a+b) return a<b;
    return s[l+1<=a?l+1:l+1-a]>s[l+1<=b?l+1:l+1-b];
}
void init_val() {
    for(int i=1;i<=n;i++) val[i]=(val[i-1]*10%MOD+s[i]-'0')%MOD;
    power_10[0]=1;
    for(int i=1;i<=n;i++) power_10[i]=power_10[i-1]*10%MOD;
}
int main() {
    scanf("%s",s+1);
    n=strlen(s+1);
    init_hash();
    for(int i=1;i<=n;i++) rk[i]=i;
    sort(rk+1,rk+1+n,cmp);
    init_val();
    long long res=0;
    for(int i=1;i<=n;i++) res=(res*power_10[rk[i]]%MOD+val[rk[i]])%MOD;
    printf("%lld\n",res);
    return 0;
}
```
