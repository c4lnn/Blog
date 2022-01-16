---
title: LibreOJ 10062. 病毒
date: 2021-06-16 19:25:14
tags: [AC 自动机,搜索]
categories: 解题报告
mathjax: true
---

# 链接

<https://loj.ac/p/10062>

# 题意

判断是否存在一个无限长的 $01$ 串不存在任何一个给出的 $01$ 串。

<!--more-->

# 思路

建立 AC 自动机。

要有无限长的 01 串，我们需要在 Trie 图上找到一个环，且不能经过任意一个给出的串。

我们给每个串的结束的位置打上标记，通过 fail 指针传递标记，然后在 Trie 图 上 DFS 即可。

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
typedef long double LD;
typedef long long LL;
typedef unsigned long long ULL;
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
// head
const int N=3e4+5;
int tr[N][2],fail[N],sz;
bool f[N],st[N],ins[N];
void insert(const string &s) {
    int u=0;
    for(auto c:s) {
        int v=c-'0';
        if(!tr[u][v]) tr[u][v]=++sz;
        u=tr[u][v];
    }
    f[u]=true;
}
void build() {
    queue<int> q;
    for(int v=0;v<2;v++) if(tr[0][v]) q.push(tr[0][v]);
    while(SZ(q)) {
        int u=q.front();
        q.pop();
        for(int v=0;v<2;v++) {
            if(tr[u][v]) {
                fail[tr[u][v]]=tr[fail[u]][v];
                f[tr[u][v]]|=f[fail[tr[u][v]]];
                q.push(tr[u][v]);
            }
            else tr[u][v]=tr[fail[u]][v];
        }
    }
}
bool dfs(int u) {
    if(ins[u]) return true;
    if(st[u]||f[u]) return false;
    ins[u]=st[u]=true;
    if(dfs(tr[u][0])) return true;
    if(dfs(tr[u][1])) return true;
    ins[u]=false;
    return false;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    for(int i=1;i<=n;i++) {
        string s;cin>>s;
        insert(s);
    }
    build();
    cout<<(dfs(0)?"TAK":"NIE")<<'\n';
    return 0;
}
```