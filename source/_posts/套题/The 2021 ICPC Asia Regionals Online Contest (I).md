---
title: The 2021 ICPC Asia Regionals Online Contest (I)
date: 2021-09-30 22:51:26
tags: [二分,凸包,线段树]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |   I   |   J   |   K   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  7/11  |   O   |   Ø   |   -   |   Ø   |   -   |   O   |   -   |   O   |   O   |   -   |   O   |

# 题目链接

<https://pintia.cn/problem-sets/1439618253197332480/problems/type/7>

<!--more-->

# Problem A. Busiest Computing Nodes

**题意：**

$k$ 个机器，编号为 $0\sim k-1$，$n$ 个任务，编号为 $0\sim n-1$，每个任务由开始时间和耗费时间组成。

- 初始时间为 $0$。
- 按顺序给每个任务分配机器，第 $i$ 个任务从第 $i\%k$ 个机器开始检查是否空闲，若空闲，则分配给该机器。
- 保证任务的开始时间非递减。
- $1\le k,n\le 1e5$。

**思路：**

用小顶堆维护每台机器的结束时间。

枚举任务，将堆中结束时间小于等于当前任务开始时间的机器弹出，放入 set 中，二分 set，找出离第 $i\%k$ 个机器最近的那一个。


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
typedef vector<PII> VPII;
// head
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int k,n;cin>>k>>n;
    VI cnt(k);
    set<int> s;
    for(int i=0;i<k;i++) s.insert(i);
    priority_queue<PII,VPII,greater<PII>> q;
    for(int i=0;i<n;i++) {
        int a,b;cin>>a>>b;
        while(SZ(q)&&q.top().FI<=a) {
            s.insert(q.top().SE);
            q.pop();
        }
        if(!SZ(s)) continue;
        auto pos=s.lower_bound(i%k);
        if(pos==s.end()) pos=s.begin();
        ++cnt[*pos];
        q.emplace(a+b,*pos);
        s.erase(pos);
    }
    int mx=0;
    for(int i=0;i<k;i++) if(cnt[i]>mx) mx=cnt[i];
    VI res;
    for(int i=0;i<k;i++) if(cnt[i]==mx) res.PB(i);
    for(int i=0;i<SZ(res);i++) {
        cout<<res[i];
        if(i!=SZ(res)-1) cout<<' ';
    }
    return 0;
}
```

# Problem B. Convex Polygon

**题意：**

$n$ 个点，问是否能构成一个凸包，凸包上包含所有点且没有三点共线。

**思路：**

凸包模板。

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
typedef vector<PII> VPII;
// head
const int N=105;
int top;
PII p[N],stk[N];
int solve(PII a,PII b) {
    return a.FI*b.SE-a.SE*b.FI;
}
int main() {
    int n=0,x,y;
    while(~scanf("%d,%d,",&x,&y)) {
        p[++n]=MP(x,y);
    }
    if(n<=2) cout<<"ERROR";
    else {
        sort(p+1,p+1+n);
        // 有重复点需要去重，至少三个不相同的点才能构成凸包，根据题意判断点或线段的特殊情况
        for(int i=1;i<=2;i++) stk[++top]=p[i];
        for(int i=3;i<=n;i++) {
            while(top>=2&&solve(MP(stk[top].FI-stk[top-1].FI,stk[top].SE-stk[top-1].SE),MP(p[i].FI-stk[top].FI,p[i].SE-stk[top].SE))<=0) --top;
            stk[++top]=p[i];
        }
        int temp=top;
        stk[++top]=p[n-1];
        for(int i=n-2;i>=1;i--) {
            while(top>=temp&&solve(MP(stk[top].FI-stk[top-1].FI,stk[top].SE-stk[top-1].SE),MP(p[i].FI-stk[top].FI,p[i].SE-stk[top].SE))<=0) --top;
            stk[++top]=p[i];
        }
        // 1 和 top 为同一个点，凸包上共有 top - 1 个点
        if(top-1==n) {
            int mn=0x3f3f3f3f,pos=0;
            for(int i=1;i<top;i++) if(stk[i].FI*stk[i].FI+stk[i].SE*stk[i].SE<mn) {
                mn=stk[i].FI*stk[i].FI+stk[i].SE*stk[i].SE;
                pos=i;
            }
            VPII res;
            for(int i=pos;i>=1;i--) res.PB(stk[i]);
            for(int i=top-1;i>pos;i--) res.PB(stk[i]);
            for(int i=0;i<SZ(res);i++) {
                cout<<res[i].FI<<','<<res[i].SE;
                if(i!=SZ(res)-1) cout<<',';
            }
        }
        else cout<<"ERROR";
    }
    return 0;
}
```

# Problem D. Edge of Taixuan

**题意：**

$n$ 个点，$m$ 个操作。

第 $i$ 个操作 将 $[l_i,r_i]$ 中每两个点之间连一条权值为 $w_i$ 的无向边。

删去一些边，使 $n$ 个点依然连通，求删去边的最大权值和。

- 最多 $10$ 组数据，$1\le n,m\le 1e5$。

**思路：**

题意可转化为求最小生成树，将操作按权值从大到小排，做区间覆盖操作，线段树维护。

线段树上每个点代表的区间为前闭后开，所有点连通需保证每个点都至少被更新过一次。

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
typedef vector<PII> VPII;
// head
template<typename T>
bool read(T &t){
    static char ch;
    int f=1;
    while(ch!=EOF&&!isdigit(ch)) {
        if(ch=='-') f=-1;
        ch=getchar();
    }
    if(ch==EOF) return false;
    for(t=0;isdigit(ch);ch=getchar()) t=t*10+ch-'0';
    t*=f;
    return true;
}
template<typename T>
void print(T t) {
    static int stk[70],top;
    if(t==0) {putchar('0');return;}
    if(t<0) {t=-t;putchar('-');}
    while(t) stk[++top]=t%10,t/=10;
    while(top) putchar(stk[top--]+'0');
}
const int N=1e5+5;
int ls[N<<2],rs[N<<2];
LL sum[N<<2],laz[N<<2];
void build(int p,int l,int r) {
    ls[p]=l,rs[p]=r,laz[p]=-1;
    if(l==r) {sum[p]=1e11;return;}
    int mid=l+r>>1;
    build(p<<1,l,mid);
    build(p<<1|1,mid+1,r);
    sum[p]=sum[p<<1]+sum[p<<1|1];
}
void push_down(int p) {
    if(laz[p]!=-1) {
        sum[p<<1]=laz[p]*(rs[p<<1]-ls[p<<1]+1);
        sum[p<<1|1]=laz[p]*(rs[p<<1|1]-ls[p<<1|1]+1);
        laz[p<<1]=laz[p<<1|1]=laz[p];
        laz[p]=-1;
    }
}
void update(int p,int l,int r,int v) {
    if(ls[p]>=l&&rs[p]<=r) {
        sum[p]=v*(rs[p]-ls[p]+1);
        laz[p]=v;
        return;
    }
    push_down(p);
    int mid=ls[p]+rs[p]>>1;
    if(l<=mid) update(p<<1,l,r,v);
    if(r>mid) update(p<<1|1,l,r,v);
    sum[p]=sum[p<<1]+sum[p<<1|1];
}
struct R {
    int l,r,v;
    bool operator < (const R &T) const {
        return v>T.v;
    }
}a[N];
int main() {
    int T;read(T);
    for(int cs=1;cs<=T;cs++) {
        int n,m;read(n),read(m);
        printf("Case #%d: ",cs);
        if(n==1) putchar('0');
        else {
            for(int i=1;i<=m;i++) read(a[i].l),read(a[i].r),read(a[i].v);
            build(1,1,n-1);
            sort(a+1,a+1+m);
            LL res=0;
            for(int i=1;i<=m;i++) {
                update(1,a[i].l,a[i].r-1,a[i].v);
                res+=1ll*(a[i].r-a[i].l+1)*(a[i].r-a[i].l)/2*a[i].v;
            }
            if(sum[1]>=1e11) printf("Gotta prepare a lesson");
            else print(res-sum[1]);
        }
        if(cs<T) putchar('\n');
    }
    return 0;
}
```

# Problem F. Land Overseer

**题意：**

二维坐标系中三个点 $O(0,0),A(a,b),B(2a,0)$。

从 $O$ 出发，到达以 $A$ 为圆心，半径为 $R$ 的圆内或圆上，再到达以 $B$ 为圆心，半径为 $R$ 的圆内或圆上。

求最短距离。

- $0<a,b,R<1e9,a^2+b^2>4R^2$

**思路：**

$OAB$ 形成一个等腰三角形，显然从 $O$ 走到 $(a,b-R)$，再往 $B$ 走距离最短。

当 $b-r\le 0$ 时，直接从 $O$ 走到 $(2a-r,0)$ 即可，且可能存在第二次无需移动的情况。

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
typedef vector<PII> VPII;
// head
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    for(int cs=1;cs<=T;cs++) {
        cout<<"Case #"<<cs<<": ";
        double a,b,r;cin>>a>>b>>r;
        double x1=a,y1=b-r;
        if(y1<=0) cout<<fixed<<setprecision(2)<<max(0.0,2*a-r);
        else {
            double t=sqrt(x1*x1+y1*y1);
            cout<<fixed<<setprecision(2)<<t+max(0.0,t-r);
        }
        if(cs<T) cout<<'\n';
    }
    return 0;
}
```

# Problem H. Mesh Analysis

**题意：**

$n$ 个点，$m$ 个关系，关系有两种：

1. 三点成环。
2. 两点成线。

求出每个点所在关系编号和所有相邻点。

- $1\le n\le 10000,1\le m \le 21000$

**思路：**

暴力。

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
typedef vector<PII> VPII;
// head
const int N=3e4;
int a[N][3];
string o[N];
VI g[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,m;cin>>n>>m;
    for(int i=1;i<=n;i++) {
        int x;cin>>x;
        string s;
        for(int j=1;j<=3;j++) cin>>s;
    }
    for(int i=1;i<=m;i++) {
        int x;cin>>x;
        cin>>o[i];
        if(o[i]=="203") {
            cin>>a[i][0]>>a[i][1]>>a[i][2];
            g[a[i][0]].PB(a[i][1]);
            g[a[i][0]].PB(a[i][2]);
            g[a[i][1]].PB(a[i][0]);
            g[a[i][1]].PB(a[i][2]);
            g[a[i][2]].PB(a[i][0]);
            g[a[i][2]].PB(a[i][1]);
        }
        else {
            cin>>a[i][0]>>a[i][1];
            g[a[i][0]].PB(a[i][1]);
            g[a[i][1]].PB(a[i][0]);
        }
    }
    for(int i=1;i<=n;i++) {
        sort(ALL(g[i]));
        g[i].resize(unique(ALL(g[i]))-g[i].begin());
    }
    int q;cin>>q;
    for(int i=1;i<=q;i++) {
        int x;cin>>x;
        cout<<x<<'\n';
        if(x>=1&&x<=n) {
            cout<<'[';
            for(int j=0;j<SZ(g[x]);j++) {
                cout<<g[x][j];
                if(j==SZ(g[x])-1) cout<<"]\n";
                else cout<<',';
            }
            vector<int> res;
            cout<<'[';
            for(int j=1;j<=m;j++) {
                if(o[j]=="203") {
                    if(a[j][0]==x||a[j][1]==x||a[j][2]==x) res.PB(j);
                }
                else {
                    if(a[j][0]==x||a[j][1]==x) res.PB(j);
                }
            }
            for(int j=0;j<SZ(res);j++) {
                cout<<res[j];
                if(j==SZ(res)-1) {
                    cout<<']';
                    if(i!=q) cout<<'\n';
                }
                else cout<<',';
            }
        }
        else {
            cout<<"[]\n[]";
            if(i!=q) cout<<'\n';
        }
    }
    return 0;
}
```

# Problem I. Neiborhood Search

**题意：**

给出一个集合 $S$和 $r$，询问 $x$，求出集合中属于 $[x-r,x+r]$ 区间内的元素。

- $|S|\le1e5$

**思路：**

二分或暴力。

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
typedef vector<PII> VPII;
// head
const int N=1e5+5;
int a[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int pos=0;
    VI b;
    while(cin>>a[pos]) {++pos;}
    for(int i=0;i<pos-2;i++) b.push_back(a[i]);
    sort(ALL(b));
    int x=a[pos-2],y=a[pos-1];
    int l=lower_bound(ALL(b),x-y)-b.begin();
    int r=upper_bound(ALL(b),x+y)-b.begin()-1;
    if(l<=r) {
        for(int i=r;i>=l;i--) {
            cout<<b[i]<<' ';
        }
    }
    else cout<<'\n';
    return 0;
}
```

# Problem K. Segment Routing

**题意：**

$T$ 组数据，$n$ 个点，$m$ 个询问。

给出每个点的出边，按顺序编号。

每个询问以 $S$ 为起点出发，依次给出每次移动的出边的编号，问是否存在终点。

- $1\le T \le 10,1\le n,m\le 1e5$。
- 每组数据中每个点的出边和和询问中的边和均  $\le 2e6$。

**思路：**

暴力模拟。

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
typedef vector<PII> VPII;
// head
const int N=2e6+5;
VI g[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    for(int cs=1;cs<=T;cs++) {
        cout<<"Case #"<<cs<<": \n";
        int n,m;cin>>n>>m;
        for(int i=1;i<=n;i++) {
            int t;cin>>t;
            while(t--) {
                int v;cin>>v;
                g[i].PB(v);
            }
        }
        for(int i=1;i<=m;i++) {
            int s,t;cin>>s>>t;
            int f=0;
            while(t--) {
                int x;cin>>x;
                if(f==0&&(int)SZ(g[s])>=x) s=g[s][x-1];
                else f=1;
            }
            if(f==1) cout<<"Packet Loss";
            else cout<<s;
            if(cs<T||cs==T&&i<m) cout<<'\n';
        }
        for(int i=1;i<=n;i++) g[i].clear();
    }
    return 0;
}
```