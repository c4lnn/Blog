---
title: 2021 Jiangxi Provincial Collegiate Programming Contest
date: 2021-10-26 00:51:31
tags: [DP,博弈论,二分,差分]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |   I   |   J   |   K   |   L   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  6/12  |   O   |   O   |   -   |   -   |   -   |   -   |   -   |   Ø   |   -   |   Ø   |   O   |   O   |

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

# Problem B. Continued Fraction

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

# Problem H. Hearthstone So Easy

**题意：**

A 和 B 做游戏，初始时都有 $n$ 血。

每一轮双方都能执行一次操作，当轮到某人操作时，若是第 $i$ 轮则先扣除自身 $i$ 血。

每次操作可以让自己回 $k$ 血或让对方扣 $k$ 血。

先没血的人则输。

**思路：**

每回合开始时，A 和 B 的血量必定是相同的。

我们考虑在第 $i+1$ $(i>0)$ 回合 A 刚好可以击杀 B，即在第 $i$ 回合 A 无法击杀 B。那么在第 $i$ 回合 B
一定可以击杀 A。

A 为了让 B 多扣血，一定会攻击 B，那么 B 除了在上述第 $i$ 轮攻击 A 获胜，其余轮都会回血来保持血量。

所以只要 A 第一回合无法击杀 B，那么 B 一定赢。

**代码：**

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
int main(){
    int t;cin>>t;
    while(t--){
        int n,k;
        cin>>n>>k;
        if(n>1&&n<=k+1) cout<<"pllj"<<endl;
        else cout<<"freesin"<<endl;
    }
    return 0;
}
```

# Problem J. LRU

**题意：**

如果请求的块在缓存中可用，就会发生缓存命中。

如果没有，CPU 只能从内存访问块并将块写入缓存。如果缓存未满，将块追加到缓存中。

如果缓存已经满了，缓存中最长时间没有被访问的块将被新的块替换。

现在请求块的顺序和缓存的容量已经给出，请确定缓存的最小容量，以确保至少有 $k$ 个请求命中缓存。

**思路：**

二分容量，用双端队列记录缓存中的块。

因为缓存命中会访问块，所以需要记录索引和时间戳。

每次弹出队头时间戳不是最新的索引，说明并不是最长时间没有被访问过的，用 map 记录索引的最新时间戳。

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
int n,k;
VI a;
bool check(int mid) {
    unordered_map<int,int> mp;
    deque<PII> dq;
    int sum=0,cnt=0;
    for(int i=0;i<n;i++) {
        int x=a[i];
        while(SZ(dq)&&mp[dq.front().FI]!=dq.front().SE) dq.pop_front();
        if(mp.count(x)) {
            ++sum;
            dq.EB(x,i);
            mp[x]=i;
        }
        else if(cnt<mid) {
            dq.EB(x,i);
            mp[x]=i;
            ++cnt;
        }
        else {
            mp.erase(dq.front().FI);
            dq.pop_front();
            dq.EB(x,i);
            mp[x]=i;

        }
    }
    return sum>=k;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n>>k;
    a.resize(n);
    for(int i=0;i<n;i++) cin>>a[i];
    int l=1,r=1e9;
    while(l<r) {
        int mid=l+r>>1;
        if(check(mid)) r=mid;
        else l=mid+1;
    }
    if(l==1e9) cout<<"cbddl"<<'\n';
    else cout<<l<<'\n';
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