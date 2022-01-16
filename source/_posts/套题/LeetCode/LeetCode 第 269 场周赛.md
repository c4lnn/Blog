---
title: LeetCode 第 269 场周赛
date: 2021-12-06 01:50:12
tags: []
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |
| :----: | :---: | :---: | :---: | :---: |
|  Done  |   Ø   |   Ø   |   Ø   |   Ø   |

# 题目链接

<https://leetcode-cn.com/contest/weekly-contest-269/>

<!--more-->

# Problem A. 找出数组排序后的目标下标

**题意：**

**思路：**

**代码：**

```cpp
class Solution {
public:
    vector<int> targetIndices(vector<int>& a, int n) {
        sort(a.begin(), a.end());
        vector<int> ret;
        for (int i = 0; i < a.size(); i++) {
            if(a[i] == n) {
                ret.push_back(i);
            }
        }
        return ret;
    }
};
```

# Problem B. 半径为 k 的子数组平均值

**题意：**

**思路：**

**代码：**

```cpp
class Solution {
public:
    vector<int> getAverages(vector<int>& a, int k) {
        vector<long long> sum(a.size());
        sum[0] = a[0];
        for (int i = 1; i < a.size(); i++) {
            sum[i] = sum[i - 1] + a[i];
        }
        vector<int> ret;
        for (int i = 0; i < a.size(); i++) {
            if (i - k < 0 || i + k >= a.size()) {
                ret.push_back(-1);
            }
            else if (i - k == 0) {
                ret.push_back(sum[i + k] / (2 * k + 1));
            }
            else {
                ret.push_back((sum[i + k] - sum[i - k - 1]) / (2 * k + 1));
            }
        }
        return ret;
    }
};
```

# Problem C. 从数组中移除最大值和最小值

**题意：**

**思路：**

**代码：**

```cpp
class Solution {
public:
    int minimumDeletions(vector<int>& x) {
        int mn = INT_MAX, mx = INT_MIN, a, b;
        for (int i = 0; i < x.size(); i++) {
            if (x[i] < mn) {
                mn = x[i];
                a = i;
            }
            if (x[i] > mx) {
                mx = x[i];
                b = i;
            }
        }
        if (a > b) {
            swap(a, b);
        }
        ++a;
        ++b;
        return min({b, (int)x.size()- a + 1, a + (int)x.size() - b + 1});
    }
};
```

# Problem D. 找出知晓秘密的所有专家

**题意：**

有 $n$ 个专家从 $0$ 到 $n - 1$ 编号，$meetings[i] = [xi, yi, timei]$ 表示专家 $x_i$ 和专家 $y_i$ 在时间 $time_i$ 要开一场会。一个专家可以同时参加多场会议 。最后，给你一个整数 $firstPerson$ 。

专家 $0$ 有一个秘密 ，最初，他在时间 $0$ 将这个秘密分享给了专家 $firstPerson$。

每次会议，如果专家 $x_i$ 在时间 $time_i$ 时知晓这个秘密，那么他将会与专家 $y_i$ 分享这个秘密，反之亦然。

秘密共享是瞬时发生的。也就是说，在同一时间，一个专家不光可以接收到秘密，还能在其他会议上与其他专家分享。

在所有会议都结束之后，返回所有知晓这个秘密的专家列表。

**思路：**

将会议按时间排序，同一时间内从知晓秘密的专家出发 BFS。

**代码：**

```cpp
// #include <bits/stdc++.h>
#define SZ(x) (int)(x).size()
#define ALL(x) (x).begin(), (x).end()
#define PB push_back
#define EB emplace_back
#define MP make_pair
#define FI first
#define SE second
// using namespace std;
typedef double DB;
typedef long double LD;
typedef long long LL;
typedef unsigned long long ULL;
typedef pair<int, int> PII;
typedef vector<int> VI;
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
const int N = 1e5 + 5;
bool vis[N], st[N];
class Solution {
public:
    VPII c[N];
    vector<int> findAllPeople(int n, vector<vector<int>>& a, int s) {
        VI ret, b;
        memset(vis, false, sizeof vis);
        memset(st, false, sizeof st);
        st[0] = st[s] = true;
        for (auto x : a) {
            b.PB(x[2]);
        }
        sort(ALL(b));
        b.resize(unique(ALL(b)) - b.begin());
        for (auto x : a) {
            c[lower_bound(ALL(b), x[2]) - b.begin()].EB(x[0], x[1]);
        }
        for (int i = 0; i < SZ(b); i++) {
            queue<int> q;
            unordered_set<int> t;
            unordered_map<int, VI> g;
            for (auto x : c[i]) {
                t.insert(x.FI);
                t.insert(x.SE);
                g[x.FI].PB(x.SE);
                g[x.SE].PB(x.FI);
            }
            for (auto x : t) {
                if (st[x]) {
                    vis[x] = true;
                    q.push(x);
                } else {
                    vis[x] = false;
                }
            }
            while (SZ(q)) {
                int u = q.front();
                q.pop();
                st[u] = true;
                for (auto v : g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        q.push(v);
                    }
                }
            }
        }
        for (int i = 0; i < n; i++) {
            if (st[i]) {
                ret.PB(i);
            }
        }
        return ret;
    }
};
// int main() {
//     ios::sync_with_stdio(false);
//     cin.tie(nullptr);
//     Solution::;
//     return 0;
// }
```