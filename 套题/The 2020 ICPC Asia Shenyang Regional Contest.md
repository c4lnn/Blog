---
title: The 2020 ICPC Asia Shenyang Regional Contest
date: 2021-11-18 00:47:50
tags: []
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |   I   |   J   |   K   |   L   |   M   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  4/13  |   -   |   -   |   -   |   -   |   -   |   Ø   |   Ø   |   -   |   Ø   |   -   |   Ø   |   -   |   -   |

# 题目链接

<https://codeforces.com/gym/103202>

<!--more-->

# Problem F. Kobolds and Catacombs

**题意：**

将长度为n的序列分段，对每一段按值从小到大排序，分段排序后要使整个序列为从小到大排序，求最多能分几段。

**思路：**

将原序列与排序后的序列比较，前缀和相同可分为一段。

**代码：**

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
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    VI a(n),b(n);
    for(int i=0;i<n;i++) {
        cin>>a[i];
        b[i]=a[i];
    }
    sort(ALL(b));
    LL res=0,sum=0;
    for(int i=0;i<n;i++) {
        sum+=a[i];
        sum-=b[i];
        if(!sum) ++res;
    }
    cout<<res<<'\n';
    return 0;
}
```

# Problem G. The Witchwood

**题意：**

**思路：**

**代码：**

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
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,k;cin>>n>>k;
    VI a(n);
    for(int i=0;i<n;i++) cin>>a[i];
    sort(ALL(a),greater<int>());
    LL res=0;
    for(int i=0;i<k;i++) res+=a[i];
    cout<<res<<'\n';
    return 0;
}
```

# Problem I. Rise of Shadows

**题意：**

**思路：**

分针的速度：$\frac{2\pi}{M}$。

时针的速度：$\frac{2\pi}{HM}$。

分针时针速度差：$\frac{2\pi(H-1)}{HM}$。

那么 $\frac{2\pi(H-1)(t\pmod {HM})}{HM}\le\frac{2\pi A}{HM}$ 或 $2\pi-\frac{2\pi(H-1)(t\pmod {HM})}{HM}\le\frac{2\pi A}{HM}$。

得：$(H-1)t \pmod {HM}\le A$ 或 $(H-1)t \pmod {HM}\ge HM-A$。

**代码：**

```cpp

```

# Problem K. Scholomance Academy

**题意：**

**思路：**

**代码：**

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
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
int tp,fp,fn,tn;
bool cmp(PII &a,PII &b) {
    return a.FI>b.FI;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    VPII a(n);
    for(int i=0;i<n;i++) {
        char c;cin>>c>>a[i].FI;
        a[i].SE=(c=='+');
        if(c=='+') ++fn;
        else ++tn;
    }
    sort(ALL(a),cmp);
    DB res=0,pre_x=0,pre_y=0;
    for(int i=0;i<n;i++) {
        if(a[i].SE) --fn,++tp;
        else --tn,++fp;
        if(i<n-1&&a[i].FI==a[i+1].FI) continue;
        res+=pre_y*(1.0*fp/(tn+fp)-pre_x);
        pre_x=1.0*fp/(tn+fp);
        pre_y=1.0*tp/(tp+fn);
    }
    cout<<fixed<<setprecision(15)<<res<<'\n';
    return 0;
}
```
