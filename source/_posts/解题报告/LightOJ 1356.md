---
title: LightOJ 1356. Prime Independence
date: 2021-05-08 18:46:31
tags: [二分图最大匹配,质数]
categories: 解题报告
mathjax: true
---

# 链接

<http://lightoj.com/volume_showproblem.php?problem=1356>

# 题意

给出 $n$ 个数，找出一个最大质数独立子集，如果 $a=b*$ 一个质数，那么认为 $a$ 是 $b$ 的一个质数乘级，如果一个集合不存在一个数是另一个数的质数乘级，那么这就是质数独立子集。

<!--more-->

# 思路

本题难点在于二分图建图。

记 $tot_i$ 为 $i$ 的质因子个数，$p_1,p_2,p_3$ 为质数：

- 若 $a*p_1=b*p_2=c(a<b)$，那么 $a,c$ 属于不同集合，$b,c$ 属于不同集合，并且 $b$ 不整除 $a$，所以 $a$ 与 $b$ 可属于同一集合。
- 若 $c*p_3=d$，那么 $c,d$ 属于不同集合，因为 $d/a=p_1*p_3,d/b=p_2*p_3$，所以 $a,b,d$ 可属于同一集合。

因为 $tot_a+1=tot_b+1=tot_c=tot_d-1$，所以 $tot_a,tot_b,tot_d 与 tot_c$ 奇偶性不同。

总结：

二分图一边的质因子个数为奇数，一边为偶数。

因为在同一边的数，如果两个数存在倍数关系，此倍数必为质因子个数为偶数的合数，否则质因子个数奇偶性不同。

因为本题数据量很大，所以在唯一分解时建边，并用 HK 算法跑二分图最大匹配。

二分图最大独立集 $=n-$ 二分图最小点覆盖 $=n-$ 二分图最大匹配边数。

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
const int N=40005;
const int M=500005;
int n,a[N],match[N],dep[N],id[M],mn[M];
VI p,g[N],v1,v2;
void add_edge(int u,int v) {
    g[u].PB(v);
    g[v].PB(u);
}
void get_prime() {
    for(int i=2;i<=M;i++) {
        if(!mn[i]) mn[i]=i,p.PB(i);
        for(auto x:p) {
            if(x>mn[i]||x*i>M) break;
            mn[i*x]=x;
        }
    }
}
int get_factor(int x) {
    int t=x,tot=0;
    for(int i=0;i<SZ(p);i++) {
        if(p[i]*p[i]>t) break;
        if(t%p[i]==0) {
            if(id[x/p[i]]) add_edge(id[x/p[i]],id[x]);
            while(t%p[i]==0) {
                ++tot;
                t/=p[i];
            }
        }
    }
    if(t>1) {
        ++tot;
        if(id[x/t]) add_edge(id[x/t],id[x]);
    }
    return tot;
}
bool bfs() {
    queue<int> q;
    for(int i=1;i<=n;i++) dep[i]=0;
    for(auto x:v1) if(match[x]==-1) dep[x]=1,q.push(x);
    bool f=false;
    while(SZ(q)) {
        int u=q.front();
        q.pop();
        for(auto v:g[u]) {
            if(dep[v]) continue;
            dep[v]=dep[u]+1;
            if(match[v]==-1) f=true;
            else dep[match[v]]=dep[v]+1,q.push(match[v]);
        }
    }
    return f;
}
bool dfs(int u) {
    for(auto v:g[u]) {
        if(dep[v]!=dep[u]+1) continue;
        dep[v]=0;
        if(match[v]==-1||dfs(match[v])) {
            match[v]=u;
            match[u]=v;
            return true;
        }
    }
    return false;
}
int hk() {
    for(int i=1;i<=n;i++) match[i]=-1;
    int res=n;
    while(bfs()) for(int i=1;i<=n;i++) if(match[i]==-1&&dfs(i)) res--;
    return res;
}
int main() {
    //freopen("E:/OneDrive/IO/in.txt","r",stdin);
    get_prime();
    int tt;
    scanf("%d",&tt);
    for(int cs=1;cs<=tt;cs++) {
        v1.clear(),v2.clear();
        memset(id,0,sizeof id);
        scanf("%d",&n);
        for(int i=1;i<=n;i++) g[i].clear();
        for(int i=1;i<=n;i++) {
            int x;
            scanf("%d",&a[i]);
            id[a[i]]=i;
        }
        for(int i=1;i<=n;i++) {
            if(get_factor(a[i])&1) v1.PB(i);
            else v2.PB(i);
        }
        printf("Case %d: %d\n",cs,hk());
    }
    return 0;
}
```