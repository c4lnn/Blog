---
title: 'Codeforces Round #742 (Div. 2)'
date: 2021-10-07 22:50:38
tags: [贪心,线段树]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: |
|  5/6   |   O   |   O   |   O   |   Ø   |   Ø   |   -   |

# 题目链接

<https://codeforces.com/contest/1567>

<!--more-->

# Problem A. Domino Disaster

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
        int n;cin>>n;
        string s;cin>>s;
        for(auto c:s) {
            if(c=='U') cout<<'D';
            else if(c=='D') cout<<'U';
            else if(c=='L') cout<<'L';
            else if(c=='R') cout<<'R';
        }
        cout<<'\n';
    }
    return 0;
}
```

# Problem B. MEXor Mixup

**题意：**

一个数组的 mex 为 $a$，异或和为 $b$，求这个数组的最小的元素个数，$t$ 组数据。

- $1\le t \le 5e4,1\le a\le 3e5,0\le b\le 3e5$

**思路：**

因为 mex 为 $a$，那么这个数组至少包含 $0\sim a-1$ 各一个，共 a 个元素，记 $0\sim a-1$ 异或和为 $xor$：

- 当 $xor=b$ 时，无需再插入元素，共 $a$ 个元素。
- 当 $xor\oplus b = a$，需要插入两个不等于 $a$ 的元素，它们的异或和为 $a$，共 $a+2$ 个元素。
- 否则，只需插入 $xor\oplus b$ 就行，数组的 mex 依旧是 $a$，共 $a+1$ 个元素。

预处理出 $xor$，询问时 $O(1)$ 输出。

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
    VI c(300001);
    for(int i=1;i<=3e5;i++) c[i]=c[i-1]^i;
    while(T--) {
        int a,b;cin>>a>>b;
        if(c[a-1]==b) cout<<a<<'\n';
        else if((c[a-1]^b)==a) cout<<a+2<<'\n';
        else cout<<a+1<<'\n';
    }
    return 0;
}
```

# Problem C. Carrying Conundrum

**题意：**

正确的两数加法操作是在 $x$ 位溢出，给 $x+1$ 位进位，而 Alice 计算错误，给 $x+2$ 位进位。

给出 Alice 的计算结果，求有多少个不同的正整数数对按照Alice 的做法能得到该结果。

**思路：**

奇数位只会给奇数位进位，偶数位只会给偶数位进位。

提取出奇偶位置的数字，如 $12345$，分成 $135$ 和 $24$。

两数之和为 $n$，共有 $n+1$ 种情况，$(0,n),(1,n-1),\dots,(n,0)$。

假设两数为 $a,b$，删去为 $0$ 的情况，共 $(a+1)(b+1)-2$ 种不同的数对。

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
        int n;cin>>n;
        VI a,b;
        for(int i=0;n;i++) {
            if(i&1) a.PB(n%10);
            else b.PB(n%10);
            n/=10;
        }
        int t=0;
        for(int i=SZ(a)-1;~i;i--) t=t*10+a[i];
        int res=t+1;
        t=0;
        for(int i=SZ(b)-1;~i;i--) t=t*10+b[i];
        cout<<res*(t+1)-2<<'\n';
    }
    return 0;
}
```

# Problem D. Expression Evaluation Error

**题意：**

给出一个 $10$ 进制数 $s$，构造 $n$ 个数，它们的十进制和为 $s$，且 $11$ 进制和最大。

**思路：**

我们要保证高位的值越多越好，每个数至少为 $1$，那么可分配的值为 $s-n$。

我们要让每个数变为 $10000\dots$ 这样，这里的 $1$ 为能分配的最高位，而低位为 $0$，保证了之后可分配的值尽量大，这样之后能分配的高位也尽量大。

按照上述方法计算出前 $n-1$ 个数，剩下的值为第 $n$ 个数。

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
int a[20];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        int s,n;cin>>s>>n;
        s-=n;
        for(int i=0;i<n-1;i++) {
            int t=1;
            while(t*10-1<=s) t*=10;
            cout<<t<<' ';
            s=s-t+1;
        }
        cout<<s+1<<'\n';
    }
    return 0;
}
```

# Problem E. Non-Decreasing Dilemma

**题意：**

长度为 $n$ 的数组 $a$，有 $q$ 次操作，每次操作为如下之一：

1. 修改 $a_x=y$。
2. 查询 $[l,r]$ 内的不下降子数组个数。

- $1\le n,q\le 2e5$。

**思路：**

计算出差分数组 $b$，当子数组 $b_l\sim b_r$ 中 $b_{l+1}\sim b_r$ 均非负，那么该子数组为非降数组。

考虑用线段树维护个数，维护区间非降数组个数，左右两端连续非负的长度，右端第一个数可以为负，需 $+1$。

左右合并时非降数组个数为左右非降数组个数和加上左端的右长度 $*$ 右端的左长度。

修改操作会会影响 $b_x$ 和 $b_{x+1}$，两个线段树单点修改。

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
const int N=2e5+5;
int a[N],b[N];
struct R {
    int ls,rs,llen,rlen;
    LL sum;
    R() {}
    R(int ls,int rs,int llen,int rlen,LL sum):ls(ls),rs(rs),llen(llen),rlen(rlen),sum(sum) {}
} node[N<<2];
R merge(R &x,R &y) {
    R ret;
    ret.ls=x.ls,ret.rs=y.rs;
    ret.sum=x.sum+1ll*x.rlen*y.llen+y.sum;
    ret.llen=x.llen+(x.llen==(x.rs-x.ls+1)?y.llen:0);
    ret.rlen=y.rlen+(y.rlen==(y.rs-y.ls+1)&&b[y.ls]>=0?x.rlen:0);
    return ret;
}
void build(int p,int l,int r) {
    node[p].ls=l,node[p].rs=r;
    if(l==r) {
        node[p].sum=1;
        if(b[l]>=0) node[p].llen=node[p].rlen=1;
        else node[p].llen=0,node[p].rlen=1;
        return;
    }
    int mid=l+r>>1;
    build(p<<1,l,mid);
    build(p<<1|1,mid+1,r);
    node[p]=merge(node[p<<1],node[p<<1|1]);
}
void update(int p,int x,int v) {
    if(node[p].ls==node[p].rs) {
        node[p].sum=1;
        if(v>=0) node[p].llen=node[p].rlen=1;
        else node[p].llen=0,node[p].rlen=1;
        return;
    }
    int mid=node[p].ls+node[p].rs>>1;
    if(x<=mid) update(p<<1,x,v);
    else update(p<<1|1,x,v);
    node[p]=merge(node[p<<1],node[p<<1|1]);
}
R query(int p,int l,int r) {
    if(node[p].ls>=l&&node[p].rs<=r) return node[p];
    int mid=node[p].ls+node[p].rs>>1;
    if(r<=mid) return query(p<<1,l,r);
    if(l>mid) return query(p<<1|1,l,r);
    R ln=query(p<<1,l,r),rn=query(p<<1|1,l,r);
    return merge(ln,rn);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,q;cin>>n>>q;
    for(int i=1;i<=n;i++) cin>>a[i];
    for(int i=2;i<=n;i++) b[i]=a[i]-a[i-1];
    build(1,1,n);
    while(q--) {
        int o;cin>>o;
        if(o==1) {
            int x,v;cin>>x>>v;
            a[x]=v;
            if(x!=1) b[x]=a[x]-a[x-1],update(1,x,b[x]);
            if(x!=n) b[x+1]=a[x+1]-a[x],update(1,x+1,b[x+1]);
        }
        else {
            int l,r;cin>>l>>r;
            cout<<query(1,l,r).sum<<'\n';
        }
    }
    return 0;
}
```