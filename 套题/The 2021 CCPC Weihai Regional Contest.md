---
title: The 2021 CCPC Weihai Regional Contest
date: 2021-11-24 23:15:13
tags: [KMP]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |   I   |   J   |   K   |   L   |   M   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  3/13  |   O   |   -   |   -   |   O   |   -   |   -   |   -   |   -   |   -   |   O   |   -   |   -   |   -   |

# 题目链接

<https://codeforces.com/gym/103428>

<!--more-->

# Problem A. Goodbye, Ziyin!

**题意：**

给出一张 $n$ 个节点的树形图，问有多少节点满足以它为根能得到一颗二叉树。

**思路：**

若一个节点的邻节点 $>3$，无解。

不然统计邻节点 $=3$ 的节点的个数，这些节点不能作为根，其余节点为根都能得到一颗二叉树。

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
const int N=1e6+5;
int sz[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    for(int i=1;i<n;i++) {
        int u,v;cin>>u>>v;
        ++sz[u];
        ++sz[v];
    }
    int cnt=0;
    for(int i=1;i<=n;i++) {
        if(sz[i]>3) return cout<<0<<'\n',0;
        else if(sz[i]==3) ++cnt;
    }
    cout<<n-cnt<<'\n';
    return 0;
}
```

# Problem D. Period

**题意：**

给出字符串 $s$，$q$ 次询问，每次询问为若将 $s_x$ 变为 `#`，求新串的不同周期的个数。

**思路：**

KMP 求出所有 border，若 border 长度为 $len$，那么周期为 $|s|-len$，$s_i$ $(len+1\le i \le |s|-len)$ 改变对该周期无影响。

区间加一操作用前缀和维护，每次询问 $O(1)$ 输出。

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
const int N=1e6+5;
int nxt[N],sum[N];
void get_next(const string &s) {
    int i=0,j=-1;
    nxt[0]=-1;
    while(i<SZ(s)) {
        if(j==-1||s[i]==s[j]) nxt[++i]=++j;
        else j=nxt[j];
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    string s;cin>>s;
    get_next(s);
    for(int i=nxt[SZ(s)];i;i=nxt[i]) {
        int l=i+1,r=SZ(s)-i;
        if(l<=r) ++sum[l],--sum[r+1];
    }
    for(int i=2;i<=SZ(s);i++) {
       sum[i]+=sum[i-1];
    }
    int q;cin>>q;
    while(q--) {
        int n;cin>>n;
        cout<<sum[n]<<'\n';
    }
    return 0;
}
```

# Problem J. Circular Billiard Table

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
    int T;cin>>T;
    while(T--) {
        LL a,b;cin>>a>>b;
        LL p=2*a,q=360*b;
        cout<<q/__gcd(p,q)-1<<'\n';
    }
    return 0;
}
```