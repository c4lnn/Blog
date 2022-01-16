---
title: 2019ICPC上海网络赛 J. Stone game
date: 2021-06-07 15:13:53
tags: [DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://nanti.jisuanke.com/t/41420>

# 题意

$n$ 堆石子，每堆石子都有重量 $w$

设 $S'$ 是取的石子的重量 ，$S$ 是石子总重量

求满足 $(S' \ge S-S') \bigwedge (\forall t\in S',S'−t \le S−S')$ 的方案数

<!--more-->

# 思路

01 背包退背包

当 $a(a\in S')$ 为最小值

如果 $S′−a\le S−S′$ 成立

那么 $\forall t\in S′,S′−t\le S−S′$ 恒成立

先算 $0 1$ 背包方案数

再从小到大进行退背包，并判断是否符合不等式

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll mod=1e9+7;
int n,w[305];
ll f[150005];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin>>T;
    while(T--) {
        cin>>n;
        int s=0;
        for(int i=1;i<=n;i++) cin>>w[i],s+=w[i];
        sort(w+1,w+1+n);
        memset(f,0,sizeof f);
        f[0]=1;
        for(int i=1;i<=n;i++)
            for(int j=s;j>=w[i];j--)
                f[j]=(f[j]+f[j-w[i]])%mod;
        ll ans=0;
        for(int i=1;i<=n;i++) {
            for(int j=w[i];j<=s;j++) f[j]=(f[j]-f[j-w[i]]+mod)%mod;
            for(int j=w[i];j<=s;j++)
                if(j>=s-j&&j-w[i]<=s-j)
                    ans=(ans+f[j-w[i]])%mod;
        }
        cout<<ans<<endl;
    }
     return 0;
}
```