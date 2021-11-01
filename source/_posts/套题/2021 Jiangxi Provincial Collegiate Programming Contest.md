---
title: 2021 Jiangxi Provincial Collegiate Programming Contest
date: 2021-10-26 00:51:31
tags: [DP,差分]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |   I   |   J   |   K   |   L   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  4/12  |   O   |   O   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   O   |   O   |

# 题目链接

<https://codeforces.com/gym/103366>

<!--more-->

# Problem A. Mio visits ACGN Exhibition

**题意：**

有一个 $n*m$ 的网格，每个位置为 $0$ 或 $1$，定义只能向右或向下走。

问从左上到右下的所有路径中满足 $0$ 的数量 $\ge p$，$1$ 的数量 $\ge q$ 的路径数。

$1\le n,m\le 500,0\le p,q\le 100000$。

**思路：**

从 $(1,1)$ 走到 $(a,b)$ 的路径的长度是固定的，为 $a+b-1$ 步。

因为步数一定，所以只需记录经过 $0$ 的个数即可。

记 $dp_{i,j,k}$ 为走到第 $i$ 行第 $j$ 列经过 $k$ 个 $0$ 的方案数，因为转移时只跟前一行和当前行前列有关，所以可以用滚动数组优化。

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
const LL MOD=998244353;
const int N=505;
int a[N][N];
LL dp[2][N][N*2];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,m,p,q;cin>>n>>m>>p>>q;
    for(int i=1;i<=n;i++) {
        for(int j=1;j<=m;j++) {
            cin>>a[i][j];
        }
    }
    dp[1][1][!a[1][1]]=1;
    for(int i=1;i<=n;i++) {
        for(int j=1;j<=m;j++) {
            if(i==1&&j==1) continue;
            for(int k=0;i+j-1>=k;k++) {
                dp[i&1][j][k]=0;
                if(a[i][j]==0) {
                    if(!k) continue;
                    dp[i&1][j][k]=(dp[i&1][j-1][k-1]+dp[(i-1)&1][j][k-1])%MOD;
                }
                else if(i+j-1-k) dp[i&1][j][k]=(dp[i&1][j-1][k]+dp[(i-1)&1][j][k])%MOD;
            }

        }
    }
    LL res=0;
    for(int i=p;n+m-1-i>=q;i++) {
        res=(res+dp[n&1][m][i])%MOD;
    }
    cout<<res<<'\n';
    return 0;
}
```

# Problem

**题意：**

将分数表示成 $a_0+\frac{1}{a_1+\frac{1}{a_2+\frac{1}{\ddots+\frac{1}{a_n}}}}$。

**思路：**

$\frac{a}{b}=\lfloor\frac{a}{b}\rfloor+\frac{1}{\frac{b}{a\bmod b}}$，类似辗转相除法。

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
    int T;cin>>T;
    while(T--) {
        int x,y;cin>>x>>y;
        VI res;
        while(1) {
            res.PB(x/y);
            x%=y;
            if(x==1) break;
            swap(x,y);
        }
        cout<<SZ(res)<<' ';
        for(auto v:res) cout<<v<<' ';
        cout<<y<<'\n';
    }
    return 0;
}
```

# Problem K. Many Littles Make a Mickle

**题意：**

第 $i$ 层可容纳 $i*i*m$ 个人，求前 $i$ 层可容纳多少人。

**思路：**

平方和求和。

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
    int T;cin>>T;
    while(T--) {
        int n,m;cin>>n>>m;
        cout<<n*(n+1)*(2*n+1)/6*m<<'\n';
    }
    return 0;
}
```

# Problem L. It Rains Again

**题意：**

$n$ 个区间，求并集长度。

**思路：**

差分。

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
const int N=1e5+5;
int a[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    for(int i=1;i<=n;i++) {
        int x1,y1,x2,y2;cin>>x1>>y1>>x2>>y2;
        ++a[x1+1];
        --a[x2+1];
    }
    int res=0;
    for(int i=1,t=0;i<=1e5;i++) {
        t+=a[i];
        if(t) ++res;
    }
    cout<<res<<'\n';
    return 0;
}
```