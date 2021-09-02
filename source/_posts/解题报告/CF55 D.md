---
title: 'Codeforces Beta Round #51 D. Beautiful numbers'
date: 2021-04-19 23:22:57
tags: [数位 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://codeforces.com/contest/55/problem/D>

# 题意

$T(1\le t \le 10)$ 次询问，求$[l,r](1 \le l \le r \le 9e18)$ 内多少数能被其自身每位非零数字整除。

<!--more-->

# 思路

数位 DP。

若一个数能被其自身每位非零数字整除，那么其必能被其每位非零数字的最小公倍数整除。

离散化所有最小公倍数，总数不超过 $50$ 个。

如果枚举最小公倍数，那么我们需要多加一维数组或者每次枚举前初始化数组，会导致 MLE 或 TLE。

我们找到 $2520$ 是所有最小公倍数的最小公倍数。

易发现 当 $x\mid y$ 时，如果 $a \% x=0$，那么 $a \% y \% x =0$。

所以我们将模数定为 $2520$。

记 $dp[i][j][k]$ 为从高到低的第 $j$ 位，此前缀中出现的每位非零数的最小公倍数为 $lcm_i$ 时，此前缀模 $2520$ 余数为 $k$ 的方案数。

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
typedef long long LL;
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
//head
const LL MOD=2520;
LL dp[48][20][2520];
VI lim,d;
void init() {
    for(int i=2;i<1<<10;i++) {
        int t=1;
        for(int j=1;j<10;j++) {
            if(i&(1<<j)) {
                t=t*j/__gcd(t,j);
            }
        }
        d.PB(t);
    }
    sort(ALL(d));
    d.resize(unique(ALL(d))-d.begin());
    memset(dp,-1,sizeof dp);
}
LL dfs(int x,int mod,int st,bool a,bool b) {
    if(!x) return !a&&!(mod%st);
    int t=lower_bound(ALL(d),st)-d.begin();
    if(!a&&!b&&dp[t][x][mod]!=-1) return dp[t][x][mod];
    int mx=b?lim[x]:9;
    LL ret=0;
    for(int i=0;i<=mx;i++) ret+=dfs(x-1,(mod*10+i)%MOD,a?i:(i>1?st*i/__gcd(st,i):st),a&(!i),b&(i==mx));
    if(!a&&!b) dp[t][x][mod]=ret;
    return ret;
}
LL solve(LL x) {
    lim.clear();
    lim.PB(-1);
    while(x) lim.PB(x%10),x/=10;
    LL ret=0;
    return dfs(SZ(lim)-1,0,0,true,true);
}
int main() {
    init();
    int tt;
    scanf("%d",&tt);
    while(tt--) {
        LL l,r;
        scanf("%lld%lld",&l,&r);
        printf("%lld\n",solve(r)-solve(l-1));
    }
    return 0;
}
```