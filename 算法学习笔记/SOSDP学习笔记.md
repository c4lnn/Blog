---
title: SOSDP学习笔记
date: 2021-07-27 00:42:24
tags: [DP]
categories: 算法学习笔记
description: ' '
mathjax: true
---

# 概念

SOS 全称为 Sum over Subsets，即子集和。

对于二进制数 $x,y$，若 $x\&y=x$，那么我们称 $x$ 是 $y$ 的子集，$y$ 是 $x$ 的超集。

假设现在有 $0\sim 2^n-1$ 这 $2n$ 个二进制数，每个数都有一个初始权值，如何维护每个二进制数的子集和？

常规的有 $O(3^n)$ 写法：

```cpp
for(int i=0;i<1<<n;i++) {
    sum[i]=a[0];
    for(int j=i;j;j=(j-1)&i) {
        sum[i]+=a[j];
    }
}
```

而 SOS DP 可以将时间复杂度降为 $O(n\log{n})$。

记 $sum_{i,j}$ 为二进制状态为 $i$，前 $j$ 位可能不同的子集和，易得如下转移方程：

$sum_{i,j}=\begin{cases}
sum_{i,j-1} & i\&(1<<j)=0 \\
sum_{i,j-1}+sum_{i\oplus(1<<j),j-1} & i\&(1<<j)=1
\end{cases}$

```cpp
for(int i=0;i<1<<n;i++) {
    sum[i][-1]=a[i];
    for(int j=0;j<n;j++) {
        sum[i][j]=sum[i][j-1];
        if(i&(1<<j)) sum[i][j]++sum[i^(1<<j)][j-1];
    }
}
```

空间降维写法：

```cpp
for(int i=0;i<n;i++) {
    for(int j=0;j<1<<n;j++) if(j&(1<<i)) {
        sum[j]+=sum[j^(1<<i)];
    }
}
```

同样我们可以维护超集和：

```cpp
for(int i=0;i<n;i++) {
    for(int j=(1<<n)-1;~j;j--) if(j&(1<<i)) {
        sum[j^(1<<i)]+=sum[j];
    }
}
```

当我们知道了子集和，我们可以倒推出本身的权值。

```cpp
for(int i=n-1;~i;i--) {
    for(int j=0;j<1<<n;j++) if(j&(1<<i)) {
        sum[j]-=sum[j^(1<<i)];
    }
}
```

当我们知道了超集和，我们可以倒推出本身的权值。

```cpp
for(int i=n-1;~i;i--) {
    for(int j=(1<<n)-1;~j;j--) if(j&(1<<i)) {
        sum[j^(1<<i)]+=sum[j];
    }
}
```

# 例题

## [CF165E Compatible Numbers](https://codeforces.com/contest/165/problem/E)

**题意**

长度为 $n$ $(1\le n\le 1e6)$ 的数组，对数组中每一个元素 $a_i$ 找到一个元素 $a_j$ 使 $a_i\&a_j=0$。$(1\le a_i\le 4e6)$

**思路**

SOS DP 求出每个 $x$ 是否存在一个 $a_i$ 为其子集。

**代码**

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
const int N=(1<<22)+1;
int a[N],mx[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    memset(mx,-1,sizeof mx);
    for(int i=1;i<=n;i++) {
        cin>>a[i];
        mx[a[i]]=a[i];
    }
    for(int i=0;i<22;i++) {
        for(int j=0;j<1<<22;j++) if(j&(1<<i)) {
            mx[j]=max(mx[j],mx[j^(1<<i)]);
        }
    }
    for(int i=1;i<=n;i++) cout<<mx[((1<<22)-1)^a[i]]<<" \n"[i==n];
    return 0;
}
```

## [CF383E Vowels](https://codeforces.com/contest/383/problem/E)

**题意**

给出 $n$ $(n\le 1e4)$ 个长度为 $3$ 的由小写字母组成的单词，一个单词是正确的当且仅当其包含至少一个元音字母。 这里的元音字母是 `a` ~ `x` 的一个子集。 对于所有元音字母集合，求这 $n$ 个单词中正确单词的数量平方的异或和。

**思路**

将每个单词的不同字符用二进制表示，并统计数量，用 SOS DP 求出每个集合的子集和。

枚举元音集合，对于一个元音集合，单词正确数量为 $n-$当前元音集合的补集的子集和。

**代码**

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
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    VI tot(1<<24);
    for(int i=1;i<=n;i++) {
        string s;cin>>s;
        sort(ALL(s));
        s.resize(unique(ALL(s))-s.begin());
        int bit=0;
        for(auto c:s) bit|=(1<<(c-'a'));
        ++tot[bit];
    }
    for(int i=0;i<24;i++) {
        for(int j=0;j<1<<24;j++) if(j&(1<<i)) {
            tot[j]+=tot[j^(1<<i)];
        }
    }
    int res=0;
    for(int i=0;i<1<<24;i++) res^=(n-tot[i^((1<<24)-1)])*(n-tot[i^((1<<24)-1)]);
    cout<<res<<'\n';
    return 0;
}
```

## [Codechef-COVERING Covering Sets CodeChef](https://www.codechef.com/problems/COVERING)

**题意**

$r_i=\sum_{i\&(a|b|c)=i}{f_a*g_b*h_c}$，求 $\sum_0^{2^n-1}{r_i}$ $(1\le n \le 20)$。

**思路**

通过 SOS DP 求出子集和后，倒推减去真子集的贡献，而每个二进制数对答案的贡献为 $2^{1的个数}$ 次。

**代码**

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
const int N=2e6+5;
LL f[N],g[N],h[N],sum[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    for(int i=0;i<1<<n;i++) cin>>f[i];
    for(int i=0;i<1<<n;i++) cin>>g[i];
    for(int i=0;i<1<<n;i++) cin>>h[i];
    for(int i=0;i<n;i++) {
        for(int j=0;j<1<<n;j++) if(j&(1<<i)) {
            f[j]=(f[j]+f[j^(1<<i)])%MOD;
            g[j]=(g[j]+g[j^(1<<i)])%MOD;
            h[j]=(h[j]+h[j^(1<<i)])%MOD;
        }
    }
    for(int i=0;i<1<<n;i++) sum[i]=f[i]*g[i]%MOD*h[i]%MOD;
    for(int i=n-1;~i;i--) {
        for(int j=0;j<1<<n;j++) if(j&(1<<i)) {
            sum[j]=(sum[j]+MOD-sum[j^(1<<i)])%MOD;
        }
    }
    LL res=0;
    for(int i=0;i<1<<n;i++) res=(res+sum[i]*(1<<__builtin_popcount(i))%MOD)%MOD;
    cout<<res<<'\n';
    return 0;
}
```

## [CF1208F Bits And Pieces](https://codeforces.com/contest/1208/problem/F)

**题意**

求 $\max_{i<j<k}\{a_i|(a_j\&a_k)\}$ $(0\le a_i \le 2e6)$。

**思路**

用 SOS DP 求出超集的最大的两个下标，枚举 $i$ 时，从高位开始贪心。

**代码**

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
const int N=1e6+5;
int a[N];
PII mx[N<<2];
void update(int x,int y) {
    if(mx[x].SE&&mx[y].SE) {
        if(mx[y].FI>mx[x].SE) mx[x]=mx[y];
        else if(mx[y].SE>mx[x].SE&&mx[x].SE>mx[y].FI) mx[x]=MP(mx[x].SE,mx[y].SE);
        else if(mx[x].SE>mx[y].SE&&mx[y].SE>mx[x].FI) mx[x]=MP(mx[y].SE,mx[x].SE);
    }
    else if(mx[x].SE&&mx[y].FI) {
        if(mx[y].FI>mx[x].SE) mx[x]=MP(mx[x].SE,mx[y].FI);
        else if(mx[y].FI>mx[x].FI) mx[x]=MP(mx[y].FI,mx[x].SE);
    }
    else if(mx[x].FI&&mx[y].SE) {
        if(mx[x].FI>mx[y].SE) mx[x]=MP(mx[y].SE,mx[x].FI);
        else if(mx[x].FI>mx[y].FI) mx[x]=MP(mx[x].FI,mx[y].SE);
        else mx[x]=mx[y];
    }
    else if(mx[x].FI&&mx[y].FI) mx[x]=MP(min(mx[x].FI,mx[y].FI),max(mx[x].FI,mx[y].FI));
    else if(mx[y].FI) mx[x]=mx[y];
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    for(int i=1;i<=n;i++) {
        cin>>a[i];
        if(!mx[a[i]].FI) mx[a[i]].FI=i;
        else if(!mx[a[i]].SE) mx[a[i]].SE=i;
        else mx[a[i]]=MP(mx[a[i]].SE,i);
    }
    for(int i=0;i<21;i++) {
        for(int j=(1<<21)-1;~j;j--) if(j&(1<<i)) {
            update(j^(1<<i),j);
        }
    }
    int res=0;
    for(int i=1;i<=n-2;i++) {
        int bit=0,t=0;
        for(int j=20;~j;j--) {
            if(a[i]&(1<<j)) bit|=(1<<j);
            else if(mx[t|(1<<j)].FI>i&&mx[t|(1<<j)].SE>i) bit|=(1<<j),t|=(1<<j);
        }
        res=max(res,bit);
    }
    cout<<res<<'\n';
    return 0;
}
```