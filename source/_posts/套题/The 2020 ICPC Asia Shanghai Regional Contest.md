---
title: The 2020 ICPC Asia Shanghai Regional Contest
date: 2021-10-20 18:52:23
tags: [数位 DP,二分,Trie,树形 DP]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |   I   |   J   |   K   |   L   |   M   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  5/13  |   -   |   Ø   |   Ø   |   Ø   |   -   |   -   |   O   |   -   |   -   |   -   |   -   |   -   |   O   |

# 题目链接

<https://codeforces.com/gym/102900>

<!--more-->

# Problem B. Mine Sweeper II

**题意：**

给出两张 $n*m$ 扫雷图 $A,B$，至多修改 $B$ 中 $\lfloor\frac {nm}{2}\rfloor$ 格子，使 $B$ 中非雷格的数字之和等于 $A$。

**思路：**

数字和等于相邻（雷格，非雷格）二元组的个数，于是把整个地图全反过来这个二元组个数不变。

若 $A,B$ 偏差大于一半，把 $A$ 取反后 $A,B$ 偏差必不超过一半。

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
typedef long long LL;
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
//head
const int N=1005;
char a[N][N],b[N][N];
int main() {
    //freopen("E:/OneDrive/IO/in.txt","r",stdin);
    int n,m;
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++) scanf("%s",a[i]+1);
    for(int i=1;i<=n;i++) scanf("%s",b[i]+1);
    int tot=0;
    for(int i=1;i<=n;i++) {
        for(int j=1;j<=m;j++) {
            if(a[i][j]!=b[i][j]) tot++;
        }
    }
    if(tot<=n*m/2) for(int i=1;i<=n;i++) printf("%s\n",a[i]+1);
    else {
        for(int i=1;i<=n;i++) {
            for(int j=1;j<=m;j++) putchar(a[i][j]=='X'?'.':'X');
            puts("");
        }
    }
    return 0;
}
```

# Problem C. Sum of Log

**题意：**

$T$ 次询问，求 $\sum_{i=0}^x\sum_{j=[i=0]}^y{[i\&j=0]\lfloor\log_2{(i+j)}\rfloor+1}$。

$1\le T\le 1e5,0\le x,y\le1e9$。

**思路：**

在该式中，只有 $i\&j=0$ 时，才会产生贡献，而此时 $i+j$ 并不会产生进位，因此 $\lfloor\log_2{(i+j)}\rfloor+1$ 为 $i+j$ 二进制表示中 $1$ 的最高位置。

需要记录四个状态：$dp_{x,a,b_1,b_2}$ ，分别为第 $x$ 位，前面是否为 $0$，前面是否是 $x$ 的上限，前面是否是 $y$ 的上限。

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
const LL MOD=1e9+7;
VI lim1,lim2;
LL dp[31][2][2][2];
LL dfs(int x,bool a,bool b1,bool b2) {
    if(!x) return !a;
    if(dp[x][a][b1][b2]!=-1) return dp[x][a][b1][b2];
    int mx1=b1?lim1[x]:1;
    int mx2=b2?lim2[x]:1;
    LL ret=0;
    for(int i=0;i<=mx1;i++) {
        for(int j=0;j<=mx2;j++) if(i+j!=2) {
            if(a&&i+j>=1) ret=(ret+1ll*x*dfs(x-1,false,b1&(i==mx1),b2&(j==mx2)))%MOD;
            else ret=(ret+dfs(x-1,a,b1&(i==mx1),b2&(j==mx2)))%MOD;
        }
    }
    return dp[x][a][b1][b2]=ret;
}
LL solve(int x,int y) {
    memset(dp,-1,sizeof dp);
    lim1.clear();
    lim2.clear();
    lim1.PB(-1);
    lim2.PB(-1);
    while(x||y) {
        lim1.PB(x%2);
        lim2.PB(y%2);
        x/=2;
        y/=2;
    }
    return dfs(SZ(lim1)-1,true,true,true);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        int x,y;cin>>x>>y;
        cout<<solve(x,y)<<'\n';
    }
    return 0;
}
```

# Problem D. Walker

**题意：**

线段上有两点以各自速度随意移动，问最少经过多久两点移动的路径能覆盖整段线段。

**思路：**

二分时间。

考虑如下情况:

- 某点能覆盖整段线段。
- 左边的点向右移动，右边的点向左移动。
- 左边的点向左移动，右边的点向右移动（两点可先向中间移动再折返）：
  - 都不先向中间移动。
  - 左边的点先向中间移动再折返，右边的点向右移动。
  - 右边的点先向中间移动再折返，左边的点向右移动。
  - 两点都先向中间移动再折返。

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
typedef long long LL;
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
//head
DB n,p1,p2,v1,v2;
bool check(DB t) {
    DB a,b;
    a=min(p2-p1,max(0.0,v1*t-2*p1));
    b=min(p2-p1,max(0.0,v2*t-2*(n-p2)));
    if(v1*t>=p1&&v2*t>=n-p2&&a+b>=p2-p1) return true;
    if(v1*t>=p1&&v2*t>=n-p2+2*(p2-p1-a)) return true;
    if(v1*t>=p1+2*(p2-p1-b)&&v2*t>=n-p2) return true;
    a=min(p2-p1,max(0.0,(t-p1/v1)*v1/2));
    b=min(p2-p1,max(0.0,(t-(n-p2)/v2)*v2/2));
    if(t*v1>=p1&&t*v2>=n-p2&&a+b>=p2-p1) return true;
    return false;
}
int main() {
    //freopen("E:/OneDrive/IO/in.txt","r",stdin);
    int tt;
    scanf("%d",&tt);
    while(tt--) {
        scanf("%lf%lf%lf%lf%lf",&n,&p1,&v1,&p2,&v2);
        if(p1>p2) swap(p1,p2),swap(v1,v2);
        DB l=0,r=min({min(n+p1,n+n-p1)/v1,min(n+p2,n+n-p2)/v2,max((n-p1)/v1,p2/v2)});
        for(int i=0;i<200;i++) {
            DB mid=(l+r)/2.0;
            if(check(mid)) r=mid;
            else l=mid;
        }
        printf("%.10f\n",r);
    }
    return 0;
}
```

# Problem G. Fibonacci

**题意：**

计算 $\sum_{i=1}^n\sum_{j=i+1}^ng(f_i,f_j)$。

$g(x,y)=[xy\%2=0]$，$f$ 为斐波那契数。

**思路：**

当 $f_i,f_j$ 中至少有一个偶数时，$g(f_i,f_j)=1$，才会对答案产生贡献。

斐波那契数前 $n$ 项有 $k=\lfloor \frac n 3\rfloor$ 个偶数。

- 当 $f_i$ 为奇数时：$f_j$ 必须为偶数，总贡献为：$k*(k+1)$。

- 当 $f_i$ 为偶数时：$f_j$ 奇偶无所谓，总贡献为：$k*(n-3)+\frac{3k(k-1)}{2}$。

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
typedef long long LL;
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
//head
int main() {
    //freopen("E:/OneDrive/IO/in.txt","r",stdin);
    LL n;
    scanf("%lld",&n);
    LL k=n/3;
    printf("%lld\n",k*(k-2+n)-k*(k-1)/2*3);
    return 0;
}
```

# Problem M. Gitignore

**题意：**

您的项目是一个文件夹。文件夹可以包含文件和子文件夹，没有空文件夹。

最初，git 软件将同步项目中的所有文件。但是，您可以在设置中指定一些文件和文件夹，以将它们从同步中排除。

对于 gitignore 中的每一行，您可以指定一个文件或文件夹中的所有文件。你不能忽略整个项目文件夹。

你会得到项目中所有文件的路径，以及它们是否应该被忽略。你的任务是计算 gitignore 的最小行数。

**思路：**

当一个文件夹内（包括所有子文件夹内）所有文件都需要排除时，则可贪心地排除这个文件夹；否则单独排除直接位于这个文件夹内需要排除的文件，并递归求解子文件夹。

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
typedef long long LL;
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
//head
const int N=1e3+5;
int cnt,tr[N][80],w[N];
bool st[N],vis[N][N];
VI g[N];
void insert(const string &s) {
    int u=1,pre=1;
    for(int i=0;s[i];i++) {
        int v=s[i]-'/';
        if(!tr[u][v]) tr[u][v]=++cnt;
        if(!v) {
            if(!vis[pre][u]) {
                vis[pre][u]=true;
                g[pre].PB(u);
            }
            pre=u;
        }
        u=tr[u][v];
    }
    vis[pre][u]=true;
    g[pre].PB(u);
}
void query(const string &s) {
    int u=1;
    for(int i=0;s[i];i++) {
        int v=s[i]-'/';
        if(!tr[u][v]) break;
        if(!v) st[u]=true;
        u=tr[u][v];
    }
}
void dfs(int u,int fa) {
    if(u!=1&&!st[u]) {w[u]=1;return;}
    w[u]=0;
    for(auto v:g[u]) dfs(v,u),w[u]+=w[v];
}
int main() {
    //freopen("E:/OneDrive/IO/in.txt","r",stdin);
    int tt;
    scanf("%d",&tt);
    while(tt--) {
        int n,m;
        scanf("%d%d",&n,&m);
        for(int i=1;i<=cnt;i++) st[i]=false,g[i].clear();
        for(int i=1;i<=cnt;i++)
            for(int j=1;j<=cnt;j++)
                vis[i][j]=false;
        memset(tr,0,sizeof tr);
        cnt=1;
        for(int i=1;i<=n;i++) {
            string s;
            cin>>s;
            insert(s);
        }
        for(int i=1;i<=m;i++) {
            string s;
            cin>>s;
            query(s);
        }
        dfs(1,0);
        printf("%d\n",w[1]);
    }
    return 0;
}
```