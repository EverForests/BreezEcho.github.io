# ACDaily 2


# 二分与前缀和的练习

## 分巧克力

儿童节那天有 K位小朋友到小明家做客。

小明拿出了珍藏的巧克力招待小朋友们。

小明一共有 N 块巧克力，其中第 ii 块是 Hi×Wi 的方格组成的长方形。

为了公平起见，小明需要从这 N 块巧克力中切出 K 块巧克力分给小朋友们。

切出的巧克力需要满足：

1. 形状是正方形，边长是整数
2. 大小相同

例如一块 6×6 的巧克力可以切出 66 块 2×2 的巧克力或者 2 块 3×3 的巧克力。

当然小朋友们都希望得到的巧克力尽可能大，你能帮小明计算出最大的边长是多少么？

#### 输入格式

第一行包含两个整数 N 和 K。

以下 N 行每行包含两个整数 Hi 和 Wi。

输入保证每位小朋友至少能获得一块 1×1 的巧克力。

#### 输出格式

输出切出的正方形巧克力最大可能的边长。

#### 数据范围

1≤N,K≤105
1≤Hi,Wi≤105

#### 输入样例：

```
2 10
6 5
5 6
```

#### 输出样例：

```
2
```

#### 题解

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;

int n, k;
const int N = 100010;
int w[N], h[N];

bool check(int a)  // 判断条件
{
    int num = 0;
    for(int i = 0; i < n; i++){
        num += (w[i]/a)*(h[i]/a);  // 直接去掉长度不够的巧克力，同时当巧克力满足条件也可以对能分成的小巧克力数做计算。
        if(num >= k) return true;  // 若满足条件了，直接返回成立
    }
    
    return false;
}

int main()
{
    cin >> n >> k;
    
    for(int i = 0; i < n; i++) cin >> w[i] >> h[i];
    
    int l = 0, r = 1e5;
    while(l < r){
        int mid = (l + r + 1) >> 1;  // 所求目标值为红色区域右端
        if(check(mid)) l = mid;  // 红色区域更新
        else r = mid - 1;
    }
    
    cout << r;
    return 0;
}
```
+ 二分有点忘了，复习复习

## 激光炸弹

地图上有 N 个目标，用整数 Xi,Yi 表示目标在地图上的位置，每个目标都有一个价值 Wi。

注意：不同目标可能在同一位置。

现在有一种新型的激光炸弹，可以摧毁一个包含 R×R 个位置的正方形内的所有目标。

激光炸弹的投放是通过卫星定位的，但其有一个缺点，就是其爆炸范围，即那个正方形的边必须和 x，y 轴平行。

求一颗炸弹最多能炸掉地图上总价值为多少的目标。

**输入格式**
第一行输入正整数 N 和 R，分别代表地图上的目标数目和正方形的边长，数据用空格隔开。

接下来 N 行，每行输入一组数据，每组数据包括三个整数 Xi,Yi,Wi，分别代表目标的 x 坐标，y 坐标和价值，数据用空格隔开。

**输出格式**
输出一个正整数，代表一颗炸弹最多能炸掉地图上目标的总价值数目。

**数据范围**
0≤R≤109
0<N≤10000,
0≤Xi,Yi≤5000
0≤Wi≤1000
**输入样例：**

```c++
2 1
0 0 1
1 1 1
```

**输出样例：**

```c++
1
```



#### 题解

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 5010;
int p, q;
int s[N][N];

int main()
{
    int n, R;
    cin >> n >> R;
    R = min(5001, R);
    p = q = R;
    
    while(n--){
        int x, y, w;
        cin >> x >> y >> w;
        x++, y++;
        p = max(x, p);
        q = max(y, q);
        s[x][y] += w;
    }
    
    for(int i = 1; i <= p; i++)
        for(int j = 1; j <= q; j++)
            s[i][j] += s[i-1][j] + s[i][j-1] - s[i-1][j-1];
    
    int sum = 0;
    
    for(int i = R; i <= p; i++)
        for(int j = R; j <= q; j++){
            sum = max(sum, s[i][j]-s[i-R][j]-s[i][j-R]+s[i-R][j-R]);
        }
        
    cout << sum;
    return 0;
}
```
+ 巧妙利用炸弹位置的合理性处理遍历范围减少遍历范围能够优化程序运行速度

+ 二维前缀和的一个非常好的案例



## K倍区间

给定一个长度为 N 的数列，A1,A2,…AN，如果其中一段连续的子序列 Ai,Ai+1,…Aj 之和是 K 的倍数，我们就称这个区间 [i,j] 是 K 倍区间。

你能求出数列中总共有多少个 K 倍区间吗？

#### **输入格式**

第一行包含两个整数 N 和 K。

以下 N 行每行包含一个整数 Ai。

#### **输出格式**

输出一个整数，代表 K 倍区间的数目。

#### **数据范围**

1≤N,K≤100000,
1≤Ai≤100000

#### **输入样例：**

```
5 2
1
2
3
4
5
```

#### **输出样例：**

```
6
```

#### 题解

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 100010;
LL s[N], cnt[N];

int main()
{
    int n, k;
    cin >> n >> k;
    LL res = 0;
    cnt[0]++;
    
    for(int i = 1; i <= n; i++){
        cin >> s[i];
        s[i] += s[i-1];
        res += cnt[s[i]%k];
        cnt[s[i]%k]++;
    }

    printf("%lld", res);
    return 0;
}
```
本题最关键的一步在于利用前缀和将区间和表示为前缀和的差的形式，这样的话，如果区间和是k的倍数，那么两个前缀和应该是同余的，也就是说如果前缀和模k的余数相同，那么二者之间的差值一定是k的倍数。运用这种方法，有以下效果：
$$
O(n^2)->O(n)
$$
拒绝溢出。

