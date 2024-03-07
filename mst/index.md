# 最小生成树


  <!--more-->

## 1 Prim

思路：从任意一点初始化生成树集合，之后不断将到集合最短距离的点加入集合，表示确定该点。然后利用该点更新与之联系的各点到集合的距离。

找最短边，叠加边权、端点入集、更新。

注：这里的dist表示到生成树集合的距离；INF和dist的初始化值要同步；最小生成树中不含自环和重边。

从点的角度考虑最小生成树

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 510, INF = 0x3f3f3f3f;
int g[N][N];
int dist[N];
bool st[N];
int n, m;

int prim()
{
    memset(dist, 0x3f, sizeof dist);
    int res = 0;
    dist[1] = 0;
  
    for (int i = 0; i < n; i++) {  // 每次确定一个点
        int t = -1;
        for (int j = 1; j <= n; j++) {
            if (!st[j] && (t == -1 || dist[t] > dist[j])) t = j;
        }
    
        if (dist[t] == INF) return INF;  // 没把生成树更新完且这个时候到集合最短距离已经是INF,则说明不存在最小生成树
        res += dist[t];
        st[t] = true;
    
        for (int j = 1; j <= n; j++) {
            dist[j] = min(dist[j], g[t][j]);
        }
    }
  
    return res;
}

int main()
{
    cin >> n >> m;
    memset(g, 0x3f, sizeof g);
    
    int u, v, w;
    while (m--) {
        cin >> u >> v >> w;
        g[v][u] = g[u][v] = min(g[u][v], w);
    }
  
    int t = prim();
    if (t == INF) cout << "impossible" << endl;
    else cout << t << endl;
    return 0;
}
```

## 2 Kruskal

思路：直接对所有边进行考虑，将图中所有边按权值从小到大排列，然后逐个遍历，将端点放入生成树集合（利用并查集），最终集合中应该有n个结点。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;
const int N = 100010, M = 200010;  // N记录点数，M记录边数

int n, m;
int p[N];  // 并查集的基本存储结构

struct e  // 直接考虑边，与bellman_ford算法类似，构造结构体即可
{
    int a, b, w;
  
    bool operator<(const e& W) const  // 重载小于号，以便后续排序工作
    {
        return w < W.w;
    }
}edge[M];

int find(int x)  // 并查集查找算法
{
    if (p[x] != x) p[x] = find(p[x]);  // 具有递归思想在内
    return p[x];
}


int main()
{
    cin >> n >> m;
  
    int a, b, w;
    for (int i = 0; i < m; i++)  // 初始化所有边
    {
        cin >> a >> b >> w;
        edge[i] = {a, b, w};
    }
  
    sort(edge, edge + m);  // 边权排序
  
    for (int i = 1; i <= n; i++) p[i] = i;  // 并查集初始化
  
    int res = 0, cnt = 0;  // cnt记录边数，理论最终应有n-1条边。
    for (int i = 0; i < m; i++)  // 按边权大小从小到大遍历所有边，进行并查集更新与权值和的计算
    {
        int a = find(edge[i].a), b = find(edge[i].b), w = edge[i].w;
        if (a != b) {
            p[a] = b;
            res += w;
            cnt++;
        }
    }
  
    if (cnt < n-1) cout << "impossible" << endl;
    else cout << res << endl;
  
    return 0;
}
```


