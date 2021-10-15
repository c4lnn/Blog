---
title: The 2021 ICPC Asia Regionals Online Contest (II)
date: 2021-10-01 18:58:02
tags: [树状数组,状压 DP,欧拉函数,线段树]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |   I   |   J   |   K   |   L   |   M   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  5/13  |   Ø   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   O   |   Ø   |   Ø   |   O   |

# 题目链接：

<https://pintia.cn/problem-sets/1441745686294945792/problems/type/7>

<!--more-->

# Problem A. Sort

**题意：**

一个长为 $n$ 的数组，每次你可以将其划分成不超过 $k$ 个非空连续子段并将其重新排列。

这种操作可以进行无限次。

判断是否存在一种操作方案，使得数组从小到大排序，且所有操作划分的段数和不超过 $3n$。

$n\le 1000,\sum n\le 30000$

**思路：**

- 当 $k=1$ 时，判断是否有序。
- 当 $k=2$ 时，判断是否存在一个位置前后两段交换后有序。
- 当 $k\ge 3$ 时，一定能将元素从小到大插入到数组末尾，每次分成 $3$ 段，不超过 $3n$。

当 $k\ge 3$ 时，可以用树状数组维护位置。

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
const int N=1e3+5;
int n,a[N],b[N],w[N];
void update(int x,int v) {
    for(int i=x;i<=n;i+=i&-i) w[i]+=v;
}
int query(int x) {
    int ret=0;
    for(int i=x;i;i-=i&-i) ret+=w[i];
    return ret;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        int k;cin>>n>>k;
        for(int i=1;i<=n;i++) cin>>a[i];
        if(is_sorted(a+1,a+1+n)) cout<<0<<'\n';
        else if(k==1) cout<<-2<<'\n';
        else if(k==2) {
            int cnt=0,pos=1;
            for(;pos<=n;pos++) if(a[pos]<a[pos-1]) {
                break;
            }
            if(a[n]>a[1]||!is_sorted(a+pos,a+1+n)) cout<<-2<<'\n';
            else {
                cout<<1<<'\n';
                cout<<2<<'\n';
                cout<<0<<' '<<pos-1<<' '<<n<<'\n';
                cout<<2<<' '<<1<<'\n';
            }
        }
        else {
            for(int i=1;i<=n;i++) b[i]=i;
            sort(b+1,b+1+n,[&](int x,int y) {return a[x]<a[y];});
            VI res;
            for(int i=1;i<=n;i++) w[i]=0;
            for(int i=1;i<=n;i++) {
                int pos=query(b[i])+b[i];
                if(pos==n) continue;
                res.PB(pos);
                update(b[i]+1,-1);
            }
            cout<<SZ(res)<<'\n';
            for(auto x:res) {
                if(x==1) {
                    cout<<2<<'\n';
                    cout<<0<<' '<<1<<' '<<n<<'\n';
                    cout<<2<<' '<<1<<'\n';
                }
                else {
                    cout<<3<<'\n';
                    cout<<0<<' '<<x-1<<' '<<x<<' '<<n<<'\n';
                    cout<<1<<' '<<3<<' '<<2<<'\n';
                }
            }
        }
    }
    return 0;
}
```

# Problem J. Leaking Roof

**题意：**

$n*n$ 的网格每个格子有一个高度，向每个格子倒 $m$ 单位的水，格子的水会向和它相邻的高度比他低的格子流动，问所有高度为 $0$ 的格子最后有多少水。

**思路：**

高度从大到小模拟即可。

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
struct R {
    double w;
    int x,y;
    R() {}
    R(double w,int x,int y):w(w),x(x),y(y) {}
    bool operator < (const R &T) const {
        return w<T.w;
    }
};
const int N=505;
const int DIR[4][2]={-1,0,1,0,0,-1,0,1};
double sum[N][N],a[N][N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,m;cin>>n>>m;
    priority_queue<R> q;
    for(int i=1;i<=n;i++) {
        for(int j=1;j<=n;j++) {
            cin>>a[i][j];
            q.emplace(a[i][j],i,j);
        }
    }
    while(!q.empty()) {
        auto u=q.top();
        q.pop();
        sum[u.x][u.y]+=m;
        int cnt=0;
        for(int i=0;i<4;i++) {
            int tx=u.x+DIR[i][0],ty=u.y+DIR[i][1];
            if(tx>=1&&tx<=n&&ty>=1&&ty<=n&&a[tx][ty]<a[u.x][u.y]) {
                ++cnt;
            }
        }
        for(int i=0;i<4;i++) {
            int tx=u.x+DIR[i][0],ty=u.y+DIR[i][1];
            if(tx>=1&&tx<=n&&ty>=1&&ty<=n&&a[tx][ty]<a[u.x][u.y]) {
                sum[tx][ty]+=sum[u.x][u.y]/cnt;
            }
        }
        if(cnt) sum[u.x][u.y]=0;
    }
    for(int i=1;i<=n;i++) {
        for(int j=1;j<=n;j++) {
            if(a[i][j]==0) cout<<fixed<<setprecision(6)<<sum[i][j];
            else cout<<0;
            cout<<' ';
            if(j==n) cout<<'\n';
        }
    }
    return 0;
}
```

# Problem K. Meal

**题意：**

$n$ 个菜，$n$ 个人，每个人打一道菜，每道菜只能被打一次。

- 第 $i$ 个人对第 $j$ 个菜的喜爱度是 $a_{i,j}$。
- 按顺序给每个人打菜，若给第 $i$ 个人打菜时，第 $j$ 道菜没有被打过，那么打到该菜的概率为对该菜的喜爱度比上对所有没被打的菜的喜爱度的和。

问第 $i$ 个人打第 $j$ 个菜的概率，$n \le 20$。

**思路：**

状压 DP，$O(n2^n)$。

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
const int N=20;
int popcount[1<<N];
LL inv[N*101],a[N][N],sum[N],res[N][N],dp[1<<N],s[N][1<<N];
VI b[N+1];
unordered_map<int,int> mp;
LL qpow(LL a,LL b) {
    LL ret=1;
    while(b) {
        if(b&1) ret=ret*a%MOD;
        a=a*a%MOD;
        b>>=1;
    }
    return ret;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++) {
            cin>>a[i][j];
            sum[i]+=a[i][j];
        }
    for(int i=1;i<=2000;i++) inv[i]=qpow(i,MOD-2);
    for(int i=0;i<n;i++) mp[1<<i]=i;
    for(int i=1;i<1<<n;i++) {
        popcount[i]=popcount[i&(i-1)]+1;
        b[popcount[i]].PB(i);
        for(int j=0;j<20;j++) s[j][i]=s[j][i&(i-1)]+a[j][mp[i&-i]];
    }
    b[0].PB(0);
    dp[0]=1;
    for(int i=0;i<n;i++)
        for(auto j:b[i])
            for(int k=0;k<n;k++) if(!((j>>k)&1)) {
                res[i][k]=(res[i][k]+dp[j]*a[i][k]%MOD*inv[sum[i]-s[i][j]]%MOD)%MOD;
                dp[j|(1<<k)]=(dp[j|(1<<k)]+dp[j]*a[i][k]%MOD*inv[sum[i]-s[i][j]]%MOD)%MOD;
            }
    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++)
            cout<<res[i][j]<<" \n"[j==n-1];
    return 0;
}
```

# Problem L. Euler Function

**题意：**

给出一个长度为 $n$ 的序列 $x$，你需要维护 $m$ 次操作：

- 修改：区间乘 $w$。
- 询问：区间 $\varphi(x)$ 和。

$n\le 1e5,w,x_i\le 100$

**思路：**

$x$ 的质因数分解为 $x=\prod{p_i^{t_i}}$，那么 $\varphi(x)=\prod{(p_i-1)p_i^{t_i-1}}$。

因此，当 $x$ 拥有 $p$，$\varphi(x)=\varphi(x)*p$，否则 $\varphi(x)=\varphi(x)*(p-1)$。

$100$ 以内只有 $25$ 个质数，对 $w$ 分解质因数更新区间，用线段树维护。

如果区间内每个 $x$ 都拥有当前质因数，直接乘，否则单点修改。


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
const LL MOD=998244353;
int mn[105],phi[105],a[N],ls[N<<2],rs[N<<2],cnt[N],tag[N<<2],bit[105],tot[105][30];
LL sum[N<<2],laz[N<<2];
VI prime;
void get_phi(int n) {
    phi[1]=1;
    for(int i=2;i<=n;i++) {
        if(!mn[i]) mn[i]=i,prime.PB(i),phi[i]=i-1;
        for(auto x:prime) {
            if(x>mn[i]||x*i>n) break;
            mn[i*x]=x;
            phi[i*x]=i%x?phi[i]*(x-1):phi[i]*x;
        }
    }
}
void solve() {
    for(int i=2;i<=100;i++) {
        int t=i;
        for(int j=0;j<SZ(prime);j++) {
            int x=prime[j];
            if(t%x==0) {
                bit[i]|=1<<j;
                while(t%x==0) ++tot[i][j],t/=x;
            }
        }
    }
}
void build(int p,int l,int r) {
    ls[p]=l,rs[p]=r,laz[p]=1;
    if(l==r) {
        sum[p]=phi[a[l]];
        tag[p]=bit[a[l]];
        return;
    }
    int mid=l+r>>1;
    build(p<<1,l,mid);
    build(p<<1|1,mid+1,r);
    sum[p]=(sum[p<<1]+sum[p<<1|1])%MOD;
    tag[p]=tag[p<<1]&tag[p<<1|1];
}
void push_down(int p) {
    if(laz[p]!=1) {
        sum[p<<1]=sum[p<<1]*laz[p]%MOD;
        sum[p<<1|1]=sum[p<<1|1]*laz[p]%MOD;
        laz[p<<1]=laz[p<<1]*laz[p]%MOD;
        laz[p<<1|1]=laz[p<<1|1]*laz[p]%MOD;
        laz[p]=1;
    }
}
void update(int p,int l,int r,int x,int y) {
    if(ls[p]>=l&&rs[p]<=r&&(tag[p]>>x)&1) {
        for(int i=0;i<y;i++) {
            sum[p]=sum[p]*prime[x]%MOD;
            laz[p]=laz[p]*prime[x]%MOD;
        }
        return;
    }
    if(ls[p]==rs[p]) {
        sum[p]=sum[p]*(prime[x]-1)%MOD;
        tag[p]|=(1<<x);
        for(int i=0;i<y-1;i++) sum[p]=sum[p]*prime[x]%MOD;
        return;
    }
    push_down(p);
    int mid=ls[p]+rs[p]>>1;
    if(l<=mid) update(p<<1,l,r,x,y);
    if(r>mid) update(p<<1|1,l,r,x,y);
    sum[p]=(sum[p<<1]+sum[p<<1|1])%MOD;
    tag[p]=tag[p<<1]&tag[p<<1|1];
}
LL query(int p,int l,int r) {
    if(ls[p]>=l&&rs[p]<=r) return sum[p];
    push_down(p);
    int mid=ls[p]+rs[p]>>1;
    if(r<=mid) return query(p<<1,l,r);
    if(l>mid) return query(p<<1|1,l,r);
    return (query(p<<1,l,r)+query(p<<1|1,l,r))%MOD;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    get_phi(100);
    solve();
    int n,m;cin>>n>>m;
    for(int i=1;i<=n;i++) cin>>a[i];
    build(1,1,n);
    for(int i=1;i<=m;i++) {
        int o,l,r;cin>>o>>l>>r;
        if(!o) {
            int v;cin>>v;
            for(int j=0;j<25;j++) if(tot[v][j]) update(1,l,r,j,tot[v][j]);
        }
        else {
            cout<<query(1,l,r);
            if(i!=m) cout<<'\n';
        }
    }
    return 0;
}
```

# Problem M. Addition

**题意：**

$\sum_{i=0}^n{sgn_i(a_i+b_i)2^i}=\sum_{i=0}^n{sgn_ic_i2^i}$，已知 $sgn,a,b$，求 $c$。

$sgn\in\{-1,1\},a,b,c\in\{0,1\}$

**思路：**

设进位值为 $d$ $(-1 \le d \le 1)$：

- 当 $sgn_i=-1$ 时，$-3 \le sgn_i*(a_i+b_i)+d\le 1$
- 当 $sgn_i=1$ 时，$-1 \le sgn_i*(a_i+b_i)+d\le 3$

分情况讨论即可。


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
    VI sgn(n),a(n),b(n),c(n);
    for(int i=0;i<n;i++) cin>>sgn[i];
    for(int i=0;i<n;i++) cin>>a[i];
    for(int i=0;i<n;i++) cin>>b[i];
    for(int i=0,d=0;i<n;i++) {
        d+=sgn[i]*(a[i]+b[i]);
        if(sgn[i]==-1) {
            if(d==-3) c[i]=1,d=-1;
            else if(d==-2) c[i]=0,d=-1;
            else if(d==-1) c[i]=1,d=0;
            else if(d==1) c[i]=1,d=1;
        }
        else {
            if(d==3) c[i]=1,d=1;
            else if(d==2) c[i]=0,d=1;
            else if(d==1) c[i]=1,d=0;
            else if(d==-1) c[i]=1,d=-1;
        }
    }
    for(int i=0;i<n;i++) {
        cout<<c[i];
        if(i<n-1) cout<<' ';
    }
    return 0;
}
```