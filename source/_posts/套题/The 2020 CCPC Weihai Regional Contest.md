---
title: The 2020 CCPC Weihai Regional Contest
date: 2021-10-20 23:40:42
tags: []
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |   I   |   J   |   K   |   L   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  4/12  |   O   |   -   |   -   |   Ø   |   -   |   -   |   -   |   O   |   -   |   -   |   -   |   Ø   |

# 题目链接

<https://codeforces.com/gym/102798>

<!--more-->

# Problem A. Golden Spirit

**题意：**

桥两边都有 $n$ 个人，每个人都需要过桥，休息 $x$ 分钟，再返回，每次过桥需要 $t$ 分钟。

所有人都只能在你的帮助下才能过桥，求最少需要多少分钟。

**思路：**

在返回前我们先将所有人都搬到对面，然后只有两个选择：

- 将当前边第一个休息好的人返回。
- 走到对面，将对面第一个休息好的人返回。

显然只要等完你选择的开始的一边的第一个人休息好，接下来可以无需等待，将所有人原路返回。

两种情况取最小值即可。

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
        LL n,x,t;cin>>n>>x>>t;
        cout<<min(max(2*n*t,2*t+x),max((2*n+1)*t,t+x))+2*n*t<<'\n';
    }
    return 0;
}
```

# Problem D. ABC Conjecture

**题意：**

对于 $a,b,c$，满足：

- $a,b$ 互质。
- $a+b=c$。
- $c>rad(abc)^{1+\epsilon}$。

$rad(n)=\prod_{p\mid n,p\in Prime}{p}$。

$T$ $(1\le T\le 10)$ 次询问，每次询问 $c$ $(1\le c\le 1e18)$，求是否存在 $a,b$ 使不等式成立。

**思路：**

当 $c$ 的惟一分解中存在质数因子的平方时，不等式成立。

pollard_rho 大数因式分解模板。

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
LL qmul(LL a,LL b,LL m) { // b >= 0
    LL ret=0;
    while(b) {
        if(b&1) ret=(ret+a)%m;
        a=(a+a)%m;
        b>>=1;
    }
    return ret;
}
LL qpow(LL a,LL b,LL m) {
    LL ret=1;
    while(b) {
        if(b&1) ret=qmul(ret,a,m);
        a=qmul(a,a,m);
        b>>=1;
    }
    return ret;
}
bool check(LL a,LL n) {
    if(n==2) return true;
    if(n==1||!(n&1)) return false;
    LL d=n-1;
    while(!(d&1)) d>>=1;
    LL t=qpow(a,d,n);
    while(d!=n-1&&t!=1&&t!=n-1) {
        t=qmul(t,t,n);
        d<<=1;
    }
    return t==n-1||d&1;
}
bool miller_rabin(LL n) {
    static vector<LL> P={2,325,9375,28178,450775,9780504,1795265022};
    if(n<=1) return false;
    for(LL x:P) {
        if(x>n) break;
        if(!check(x,n)) return false;
    }
    return true;
}
mt19937 mt(time(0));
LL pollard_rho(LL n,LL c) {
    LL x=uniform_int_distribution<LL>(1,n-1)(mt),y=x;
    auto f=[&](LL v) {
        LL t=qmul(v,v,n)+c;
        return t<n?t:t-n;
    };
    while(1) {
        x=f(x),y=f(f(y));
        if(x==y) return n;
        LL d=__gcd(abs(x-y),n);
        if(d!=1) return d;
    }
}
VI fac; // 无序，有重复质因数
void get_fac(LL n,LL cc=19260817) {
    if(n==4) {fac.PB(2),fac.PB(2);return;}
    if(miller_rabin(n)) {fac.PB(n);return;}
    LL p=n;
    while(p==n) p=pollard_rho(n,--cc);
    get_fac(p),get_fac(n/p);
}
bool go_fac(LL n) {
    fac.clear();
    if(n<=1) return false;
    get_fac(n);
    int sz=SZ(fac);
    sort(ALL(fac));
    fac.resize(unique(ALL(fac))-fac.begin());
    return SZ(fac)<sz;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        LL n;cin>>n;
        cout<<(go_fac(n)?"yes":"no")<<'\n';
    }
    return 0;
}
```

# Problem H. Message Bomb

**题意：**

$n$ $(n\le 1e5)$ 个组，$m$ $(m\le 2e5)$ 个人，$s$ $(n\le 2e6)$ 个操作：

- $t=1$，学生 $x$ 进入 $y$ 组。
- $t=2$，学生 $x$ 退出 $y$ 组。
- $t=3$，学生 $x$ 向 $y$ 组其他学生发送一条消息。

求每个学生收到多少条消息。

**思路：**

每组是独立的，按组分别计算贡献，记录进入的时间。

因为学生退出后可能再次进入，所以在退出时要加上贡献。

做完每一组的所有操作后对还在组内的学生加上贡献。

贡献可以用前缀和 $O(1)$ 计算，显然时间复杂度是线性的。

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
int sum[N],cnt[N],res[N];
VPII a[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,m,s;cin>>n>>m>>s;
    for(int i=1;i<=s;i++) {
        int t,x,y;cin>>t>>x>>y;
        a[y].EB(t,x);
    }
    for(int i=1;i<=n;i++) {
        unordered_map<int,int> bg;
        sum[0]=0;
        for(int j=0;j<SZ(a[i]);j++) {
            int t=a[i][j].FI,x=a[i][j].SE;
            if(j) sum[j]=sum[j-1];
            if(t==1) bg[x]=j,cnt[x]=0;
            else if(t==2) res[x]+=sum[j]-sum[bg[x]]-cnt[x],cnt[x]=0,bg.erase(x);
            else ++cnt[x],++sum[j];
        }
        for(auto x:bg) res[x.FI]+=sum[SZ(a[i])-1]-sum[x.SE]-cnt[x.FI];
    }
    for(int i=1;i<=m;i++) cout<<res[i]<<'\n';
    return 0;
}
```

# Problem L. Clock Master

**题意：**

$T$ 次询问，把 $n$ 拆分为若干数的和，使得这些数的 $lcm$ 最大，输出 $\log(lcm)$。

$1\le T,n\le 30000$。

**思路：**

预处理出 $log$，将每个数拆为 $p^k$ 的形式，分组背包。


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
const int N=3e4+5;
int mn[N];
VI p;
DB lg[N],dp[N];
void prime_seive(int n) {
    for(int i=2;i<=n;i++) {
        if(!mn[i]) mn[i]=i,p.PB(i);
        for(auto x:p) {
            if(x>mn[i]||x*i>n) break;
            mn[i*x]=x;
        }
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    prime_seive(30000);
    for(int i=1;i<=30000;i++) lg[i]=log(i);
    for(auto x:p) {
        for(int i=30000;i>=2;i--) {
            for(int j=x;i-j>=0;j*=x) {
                dp[i]=max(dp[i],dp[i-j]+lg[j]);
            }
        }
    }
    int T;cin>>T;
    while(T--) {
        int n;cin>>n;
        cout<<fixed<<setprecision(9)<<dp[n]<<'\n';
    }
    return 0;
}
```