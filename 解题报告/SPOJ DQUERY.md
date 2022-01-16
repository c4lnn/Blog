---
title: SPOJ DQUERY. D-query
date: 2021-9-7 01:39:03
tags: [主席树,树状数组,莫队]
categories: 解题报告
mathjax: true
---

# 链接

<https://www.spoj.com/problems/DQUERY/>

# 题意

区间查询不同数的个数。

<!--more-->

# 思路一

给下标建主席树，每颗主席树维护的是下标，假设建第 $x$ 颗树，在第 $x-1$ 颗树的基础上将 $a_x$ 上次出现的位置 $-1$，这次位置 $+1$，这样每棵树记录的都是到当前位置出现的数最后的位置。

对于询问 $[l,r]$，在第 $r$ 颗树上查询 $[l,r]$ 的区间和即可。

# 代码一

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
const int N=3e4+5;
const int M=1e6+5;
int pre[M],rt[N],ls[N<<5],rs[N<<5],w[N<<5],cnt;
void update(int &p,int q,int L,int R,int x,int v) {
    p=++cnt;
    ls[p]=ls[q],rs[p]=rs[q],w[p]=w[q]+v;
    if(L==R) return;
    int mid=L+R>>1;
    if(x<=mid) update(ls[p],ls[q],L,mid,x,v);
    else update(rs[p],rs[q],mid+1,R,x,v);
}
int query(int p,int L,int R,int l,int r) {
    if(L>=l&&R<=r) return w[p];
    int mid=L+R>>1;
    if(r<=mid) return query(ls[p],L,mid,l,r);
    if(l>mid) return query(rs[p],mid+1,R,l,r);
    return query(ls[p],L,mid,l,mid)+query(rs[p],mid+1,R,mid+1,r);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    for(int i=1;i<=n;i++) {
        int x;cin>>x;
        if(!pre[x]) update(rt[i],rt[i-1],1,n,i,1);
        else {
            int t;
            update(t,rt[i-1],1,n,pre[x],-1);
            update(rt[i],t,1,n,i,1);
        }
        pre[x]=i;
    }
    int q;cin>>q;
    for(int i=1;i<=q;i++) {
        int l,r;cin>>l>>r;
        cout<<query(rt[r],1,n,l,r)<<'\n';
    }
    return 0;
}

```

# 思路二

离线询问，枚举右端点 $r$，将 $a_r$ 上次出现的位置 $-1$，这次出现的位置 $+1$，查询以 $r$ 为右端点的询问的答案，用树状数组维护。

# 代码二

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
int n,a[30005],b[30005],pre[1000005],res[200005];
VPII q[200005];
void update(int x,int v) {
    for(int i=x;i<=n;i+=i&-i) b[i]+=v;
}
int query(int l,int r) {
    int ret=0;;
    for(int i=r;i;i-=i&-i) ret+=b[i];
    for(int i=l-1;i;i-=i&-i) ret-=b[i];
    return ret;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n;
    for(int i=1;i<=n;i++) cin>>a[i];
    int m;cin>>m;
    for(int i=1;i<=m;i++) {
        int l,r;cin>>l>>r;
        q[r].EB(l,i);
    }
    for(int i=1;i<=n;i++) {
        if(pre[a[i]]) update(pre[a[i]],-1);
        update(i,1);
        pre[a[i]]=i;
        for(auto x:q[i]) res[x.SE]=query(x.FI,i);
    }
    for(int i=1;i<=m;i++) cout<<res[i]<<'\n';
    return 0;
}
```

# 思路三

普通莫队模板。

# 代码三

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
int n,m,unit,ans,a[30005],cnt[1000005],res[200005];
struct Q {
    int l,r,id;
    Q() {}
    Q(int l,int r,int id):l(l),r(r),id(id) {}
    bool operator < (const Q &T) const {
        if(l/unit!=T.l/unit) return l<T.l;
        if((l/unit)&1) return r>T.r;
        return r<T.r;
    }
} q[200005];
void move(int x,int v) {
    cnt[a[x]]+=v;
    if(v==-1&&cnt[a[x]]==0) --ans;
    if(v==1&&cnt[a[x]]==1) ++ans;
}
void mo() {
    unit=int(ceil(n/pow(n,0.5)));
    sort(q,q+m);
    int l=0,r=-1;
    for(int i=0;i<m;i++) {
        while(l>q[i].l) move(--l,1);
        while(r<q[i].r) move(++r,1);
        while(l<q[i].l) move(l++,-1);
        while(r>q[i].r) move(r--,-1);
        res[q[i].id]=ans;
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n;
    for(int i=0;i<n;i++) cin>>a[i];
    cin>>m;
    for(int i=0;i<m;i++) {
        cin>>q[i].l>>q[i].r;
        --q[i].l,--q[i].r;
        q[i].id=i;
    }
    mo();
    for(int i=0;i<m;i++) cout<<res[i]<<'\n';
    return 0;
}
```