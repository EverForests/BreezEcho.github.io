# 树状数组和线段树


# 树状数组和线段树

树状数组相比线段树更快，代码更短

## 一.树状数组

- 树状数组的下标是从1开始的
- 基本功能
  - 在某个位置上加上一个数，即单点修改
  - 求某个前缀和，即区间查询

<div align = center>
    <img src="C:\Users\23950\AppData\Roaming\Typora\typora-user-images\image-20220312095824311.png" alt="image-20220312095824311" style="zoom:80%;" />
</div>





+ 树状数组的每个元素`c[x]表示A[x]中区间[x-lowbit(x), x]里的数的和`，`lowbit(x)`表示x的二进制形式后0的个数，也可以表示为`x&-x`

```c++
// 求和
for(int i = x; i; i -= lowbit(i), res += c[i]) return res;  // 递归形式求和

// 对某数进行修改后需要进行的调整
A[x] += v;  // 对x位置数进行加法改变
for(int i = x; i < N; i += lowbit(i), c[x] += v)  // 每次指标上升一层
```

> 解决问题需要的操作决定了数据结构的选取，比如如果要修改某个元素且需要修改某个子序列的和，那么就可以用树状数组。数据结构的难点也在这里



### 例：动态求连续区间和

给定 n 个数组成的一个数列，规定有两种操作，一是修改某个元素，二是求子数列 [a,b] 的连续和。

#### 输入格式

第一行包含两个整数 n 和 m，分别表示数的个数和操作次数。

第二行包含 n 个整数，表示完整数列。

接下来 m 行，每行包含三个整数 k,a,b （k=0，表示求子数列[a,b]的和；k=1，表示第 a 个数加 b）。

数列从 1 开始计数。

#### 输出格式

输出若干行数字，表示 k=0 时，对应的子数列 [a,b] 的连续和。

#### 数据范围

1≤n≤100000,
1≤m≤100000，
1≤a≤b≤n,
数据保证在任何时候，数列中所有元素之和均在 int 范围内。

#### 输入样例：

10 5
1 2 3 4 5 6 7 8 9 10
1 1 5
0 1 3
0 4 8
1 7 5
0 4 8

#### 输出样例：

11
30
35

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>

using namespace std;

const int N = 100010;

int n, m;
int tr[N], w[N];

int lowbit(int x)
{
    return x&-x;
}

void add(int x, int v)
{
    for(int i = x; i <= n; i += lowbit(i)) tr[i] += v;
}

int query(int x)
{
    int res = 0;
    for(int i = x; i; i -= lowbit(i)) res += tr[i];
    return res;
}

int main()
{
    scanf("%d%d", &n, &m);
    for(int i = 1; i <= n; i++) cin >> w[i];
    for(int i = 1; i <= n; i++) add(i, w[i]);
    
    while(m--)
    {
        int k, a, b;
        scanf("%d%d%d", &k, &a, &b);
        if(k == 0) cout << query(b) - query(a-1) << endl;
        else add(a, b);
    }
    
    return 0;
}
```



## 二.线段树

- 存储: 一维数组，与堆的存储方式相同。

<div align = center>
    <img src="D:\dpayment\new\source\_posts\ACDaily\树状数组和线段树.assets\image-20220316000334508.png" alt="image-20220316000334508" style="zoom: 80%;" />
</div>



- 通常有两个操作：

  1. 单点修改

  2. 区间查询

- 基本函数：

  1. pushup 用子节点信息更新当前节点信息

  2. build 在一段区间上初始化线段树

  3. modify 修改

  4. query 某区间的特定关系



### 例：数列区间最大值

输入一串数字，给你 M 个询问，每次询问就给你两个数字 X,Y，要求你说出 X 到 Y 这段区间内的最大数。

#### 输入格式

第一行两个整数 N,M 表示数字的个数和要询问的次数；

接下来一行为 N 个数；

接下来 M 行，每行都有两个整数 X,Y。

#### 输出格式

输出共 M 行，每行输出一个数。

#### 数据范围

1≤N≤105,
1≤M≤106,
1≤X≤Y≤N,
数列中的数字均不超过231−1

#### 输入样例：

10 2
3 2 4 5 6 8 1 2 9 7
1 4
3 8

#### 输出样例：

5
8

#### 题解：

本题中modify操作没有体现，之后会用线段树实现一遍动态求连续区间和，开个坑。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <climits>

using namespace std;

int m, n;
const int N = 100010;
int w[N];

struct Node
{
    int l, r;
    int maxv;
}tr[4*N];

void build(int u, int l, int r)
{
    if(l == r) tr[u] = {l, r, w[r]};
    else
    {
        tr[u] = {l, r};
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid+1, r);
        tr[u].maxv = max(tr[u << 1].maxv, tr[u << 1 | 1].maxv);
    }
}

int query(int u, int l, int r)
{
    if(tr[u].l >= l && tr[u].r <= r) return tr[u].maxv;
    int mid = tr[u].l + tr[u].r >> 1;
    int maxm = INT_MIN;
    if(l <= mid) maxm = max(maxm, query(u << 1, l, r));
    if(r > mid) maxm = max(maxm, query(u << 1 | 1, l, r));
    return maxm;
}

int main()
{
    scanf("%d%d", &n, &m);
    for(int i = 1; i <= n; i++) scanf("%d", &w[i]);
    
    build(1, 1, n);
    
    int x, y;
    while(m -- )
    {
        scanf("%d%d", &x, &y);
        printf("%d\n", query(1, x, y));
    }
    
    return 0;
}
```


