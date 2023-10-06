# 双指针、BFS和图论




# 双指针算法、BFS与图论

## 1.双指针算法

通常考虑遍历所有区间时会用到这种算法，比如需要枚举一段时间内的所有定长时间段，由于在时间轴上两个时间段会有大部分位置的重合，因此每次可以减少一个头部时刻，增加一个尾部时刻来实现时间段的时序枚举。实际上，这个算法是对类似以下语法结构的优化：

```c++
for(int i = 0; i < n; i++)
	for(int j = 0; j < k; j++)
```

### eg.日志统计

小明维护着一个程序员论坛。现在他收集了一份”点赞”日志，日志共有 N 行。

其中每一行的格式是：

ts id  
表示在 ts 时刻编号 id 的帖子收到一个”赞”。

现在小明想统计有哪些帖子曾经是”热帖”。

如果一个帖子曾在任意一个长度为 D 的时间段内收到不少于 K 个赞，小明就认为这个帖子曾是”热帖”。

具体来说，如果存在某个时刻 T 满足该帖在 [T,T+D) 这段时间内(注意是左闭右开区间)收到不少于 K 个赞，该帖就曾是”热帖”。

给定日志，请你帮助小明统计出所有曾是”热帖”的帖子编号。

#### 输入格式

第一行包含三个整数 N,D,K。

以下 N 行每行一条日志，包含两个整数 ts 和 id。

#### 输出格式

按从小到大的顺序输出热帖 id。

每个 id 占一行。

#### 数据范围

1≤K≤N≤105,
0≤ts,id≤105,
1≤D≤10000

#### 题解：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 100010;

int n, d, k;
PII logs[N];
bool st[N];
int cnt[N];

int main()
{
    scanf("%d%d%d", &n, &d, &k);
    for(int i = 0; i < n; i++) scanf("%d%d", &logs[i].x, &logs[i].y);
    
    sort(logs, logs + n);
    
    
    for(int i = 0, j = 0; i < n; i++)
    {
        int id = logs[i].y;
        cnt[id]++;
        
        while(logs[i].x - logs[j].x >= d)
        {
            cnt[logs[j].y]--;
            j++;
        }
        
        if(cnt[id] >= k) st[id] = true;
    }
    
    for(int i = 0; i <= 100000; i++)
    {
        if(st[i]) cout << i << endl;
    }

    
    return 0;
}
```

## 2.宽度优先搜索BFS

一组数，整合为数状，使用队列来辅助操作，从树根出发，若队列非空，取队头，用队头的两个儿子来入队更新队列，直至队列为空，这样就对树进行了宽度优先搜索。

> bfs常用于迷宫问题，bfs相对于dfs可以找到一条最短路径



### eg.献给阿尔吉侬的花束

阿尔吉侬是一只聪明又慵懒的小白鼠，它最擅长的就是走各种各样的迷宫。

今天它要挑战一个非常大的迷宫，研究员们为了鼓励阿尔吉侬尽快到达终点，就在终点放了一块阿尔吉侬最喜欢的奶酪。

现在研究员们想知道，如果阿尔吉侬足够聪明，它最少需要多少时间就能吃到奶酪。

迷宫用一个 R×C 的字符矩阵来表示。

字符 S 表示阿尔吉侬所在的位置，字符 E 表示奶酪所在的位置，字符 # 表示墙壁，字符 . 表示可以通行。

阿尔吉侬在 1 个单位时间内可以从当前的位置走到它上下左右四个方向上的任意一个位置，但不能走出地图边界。

#### 输入格式

第一行是一个正整数 T，表示一共有 T 组数据。

每一组数据的第一行包含了两个用空格分开的正整数 R 和 C，表示地图是一个 R×C 的矩阵。

接下来的 R 行描述了地图的具体内容，每一行包含了 C 个字符。字符含义如题目描述中所述。保证有且仅有一个 S 和 E。

#### 输出格式

对于每一组数据，输出阿尔吉侬吃到奶酪的最少单位时间。

若阿尔吉侬无法吃到奶酪，则输出“oop!”（只输出引号里面的内容，不输出引号）。

每组数据的输出结果占一行。

#### 数据范围

1<T≤10,
2≤R,C≤200

#### 输入样例：

3
3 4
.S..
###.
..E.
3 4
.S..
.E..
....
3 4
.S..
####
..E.

#### 输出样例：

5
1
oop!

#### 题解：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <queue>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 210;

int t, r, c;
char g[N][N];
int dist[N][N];
int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

int bfs(PII start, PII end)
{
    queue<PII> q;
    memset(dist, -1, sizeof dist);
    
    dist[start.x][start.y] = 0;
    q.push(start);
    
    while(q.size())
    {
        auto t = q.front();
        q.pop();
        
        for(int i = 0; i < 4; i++)
        {
            int x = t.x + dx[i], y = t.y + dy[i];
            if(x < 0 || x >= r || y < 0 || y >= c) continue;
            if(g[x][y] == '#') continue;
            if(dist[x][y] != -1) continue;
            dist[x][y] = dist[t.x][t.y] + 1;
            if(make_pair(x, y) == end) return dist[x][y];
            q.push({x, y});
        }
        
    }
    
    return -1;
}

int main()
{
    scanf("%d", &t);
    while(t--)
    {
        scanf("%d%d", &r, &c);
        for(int i = 0; i < r; i++) scanf("%s", g[i]);
        
        PII start, end;
        for(int i = 0; i < r; i++)
            for(int j = 0; j < c; j++)
            {
                if(g[i][j] == 'S') start = {i, j};
                if(g[i][j] == 'E') end = {i, j};
            }
            
        int distance = bfs(start, end);
        if(distance == -1)
        {
            cout << "oop!" << endl;
            continue;
        }
        
        printf("%d\n", distance);
    }
    
    return 0;
}
```

## 3.图论

判重数组使得每个点最多只会被遍历一次

### eg.交换瓶子

有 N 个瓶子，编号 1∼N，放在架子上。

比如有 5 个瓶子：

2 1 3 5 4
要求每次拿起 2 个瓶子，交换它们的位置。

经过若干次后，使得瓶子的序号为：

1 2 3 4 5
对于这么简单的情况，显然，至少需要交换 2 次就可以复位。

如果瓶子更多呢？你可以通过编程来解决。

#### 输入格式

第一行包含一个整数 N，表示瓶子数量。

第二行包含 N 个整数，表示瓶子目前的排列状况。

#### 输出格式

输出一个正整数，表示至少交换多少次，才能完成排序。

#### 数据范围

1≤N≤10000,

#### 输入样例1：

5
3 1 2 5 4

#### 输出样例1：

3

#### 输入样例2：

5
5 4 3 2 1

#### 输出样例2：

2

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 10010;
int w[N];
bool st[N];

int main()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i ++ ) cin >> w[i];

    int cnt = 0;
    for(int i = 1; i <= n; i++)
    {
        if(!st[i])
        {
            cnt++;
            for(int j = i; !st[j]; j = w[j]) st[j] = true;
        }
    }
    
    printf("%d", n - cnt);
    return 0;
}
```




