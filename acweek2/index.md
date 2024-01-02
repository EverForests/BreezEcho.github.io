# Acweek2


  <!--more-->

> 一些算法中常用数据结构的写法。

## 1 链表

### 1.1 单链表

[原题链接](https://www.acwing.com/problem/content/828/)

对于链表，通常用类实现较为复杂的功能，但在算法题中，为了实现我们的目的，使用静态链表就好了。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5+10;

int head, e[N], ne[N], idx;

void init(){
    head = -1;
    memset(ne, -1, sizeof ne);
}

void add_to_head(int x) {
    ne[idx] = head;
    head = idx;
    e[idx++] = x;
}

void deletel(int k) {
    if (k == 0) {
        head = ne[head];
    }
    else ne[k-1] = ne[ne[k-1]];
}

void insert(int k, int x) {
    ne[idx] = ne[k-1];
    ne[k-1] = idx;
    e[idx++] = x;
}

int main()
{
    init();
    int m;
    cin >> m;
    while (m--) {
        char op;
        int k, x;
        cin >> op;
        if (op == 'H') {
            cin >> x;
            add_to_head(x);
        }

        else if (op == 'D') {
            cin >> k;
            deletel(k);
        }
    
        else {
            cin >> k >> x;
            insert(k, x);
        }
    }
  
    for (int j = head; j != -1; j = ne[j]) cout << e[j] << " ";
    return 0;
}
```

### 1.2 双链表

可以纯纯模拟，对每个操作进行细化。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5+10;
int head, e[N], ne[N], la[N], idx, tail;

void init() {
    memset(ne, -1, sizeof ne);
    memset(la, -1, sizeof la);
}

void ladd(int x) {
    e[++idx] = x;
    ne[idx] = ne[head];
    la[idx] = head;
    if (ne[head] != -1) la[ne[head]] = idx;
    ne[head] = idx;
  
    if (ne[idx] == -1) tail = idx;
}

void radd(int x) {
    e[++idx] = x;
    ne[tail] = idx;
    la[idx] = tail;
  
    tail = idx;
  
}

void deletek(int k) {
    if (ne[k] != -1) la[ne[k]] = la[k];
    else tail = la[k];
  
    ne[la[k]] = ne[k];
}

void insertl(int k, int x) {
    e[++idx] = x;
    ne[idx] = k;
    la[idx] = la[k];
    ne[la[k]] = idx;
    la[k] = idx;
}

void insertr(int k, int x) {
    e[++idx] = x;
    la[idx] = k;
    ne[idx] = ne[k];
    if (ne[k] != -1) la[ne[k]] = idx;
    ne[k] = idx;
  
    if (ne[idx] == -1) tail = idx;
}

int main()
{
    init();
  
    int m;
    cin >> m;
    while (m--) {
        string op;
        int k, x;
  
        cin >> op;
        if (op == "L") {
            cin >> x;
            ladd(x);
        }
  
        else if (op == "R") {
            cin >> x;
            radd(x);
        }
  
        else if (op == "D") {
            cin >> k;
            deletek(k);
        }
  
        else if (op == "IL") {
            cin >> k >> x;
            insertl(k, x);
        }
  
        else {
            cin >> k >> x;
            insertr(k, x);
        }
    }
  
    for (int i = ne[head]; i != -1; i = ne[i]) cout << e[i] << " ";
  
    return 0;
}
```

也可以增加辅助结点，简化操作设计：

使用头结点和尾结点，下标为0、1，数据元素从idx = 2开始记录。

```c++
#include <iostream>

using namespace std;

const int N = 100010;

int m;
int e[N], l[N], r[N], idx;

// 在节点a的右边插入一个数x
void insert(int a, int x)
{
    e[idx] = x;
    l[idx] = a, r[idx] = r[a];
    l[r[a]] = idx, r[a] = idx ++ ;
}

// 删除节点a
void remove(int a)
{
    l[r[a]] = l[a];
    r[l[a]] = r[a];
}

int main()
{
    cin >> m;

    // 0是左端点，1是右端点
    r[0] = 1, l[1] = 0;
    idx = 2;

    while (m -- )
    {
        string op;
        cin >> op;
        int k, x;
        if (op == "L")
        {
            cin >> x;
            insert(0, x);
        }
        else if (op == "R")
        {
            cin >> x;
            insert(l[1], x);
        }
        else if (op == "D")
        {
            cin >> k;
            remove(k + 1);
        }
        else if (op == "IL")
        {
            cin >> k >> x;
            insert(l[k + 1], x);
        }
        else
        {
            cin >> k >> x;
            insert(k + 1, x);
        }
    }

    for (int i = r[0]; i != 1; i = r[i]) cout << e[i] << ' ';
    cout << endl;

    return 0;
}
```

## 2 栈

### 2.1 模拟栈

[原题链接](https://www.acwing.com/activity/content/problem/content/865/)

> 用数组来模拟栈的动态变化以及常见操作。

使用基本数组做容器，使用指针来维护栈的状态。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <string>

using namespace std;

const int N = 1e5 + 10;

int q[N];
int tt = -1;

int main()
{
    string op;
    int m, x;
  
    cin >> m;
    while (m--) {
        cin >> op;
        if (op == "push") {
            cin >> x;
            q[++tt] = x;
        }
    
        else if (op == "pop") tt--;
    
        else if (op == "empty") {
            if (tt == -1) cout << "YES" << endl;
            else cout << "NO" << endl;
        }
    
        else cout << q[tt] << endl;
    }
  
    return 0;
}
```

### 2.2 表达式求值

[原题链接](https://www.acwing.com/activity/content/problem/content/3648/)

表达式求值是常见的需要使用栈来协助解决的问题。如果一个表达式被使用后缀表达式描述出来，那么做顺序扫描就可以很好地计算出结果，所以本题的关键在于如何分离数和符号，然后转换为后缀表达式的计算顺序。

扫描入栈和出栈计算这两个操作可以合在一起：

扫描到数，就将其转换为整型；

扫描到符号，需要进行讨论：

- 若为左括号，就push
- 若为右括号，对于符号栈，考虑一般情况(+，做出栈计算；考虑特殊情况(就不做操作。
- 若为*、/，就根据符号运算优先级将因式转换为数

最后符号栈只剩加减，就不断做出栈运算，对数栈中的数进行加减代数运算即可。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <string>
#include <unordered_map>
#include <stack>

using namespace std;

stack<int> num;
stack<char> op;

unordered_map<char, int> pr{{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}};

void eval() {
    auto a = num.top(); num.pop();
    auto b = num.top(); num.pop();
    auto c = op.top(); op.pop();
  
    if (c == '+') num.push(a+b);
    else if (c == '-') num.push(b-a);
    else if (c == '*') num.push(a*b);
    else num.push(b/a);
}

int main()
{
    string str;
    cin >> str;
  
    for (int i = 0; i < str.size(); i++) {
        auto c = str[i];
    
        if (isdigit(c)) {
            int x = 0, j = i;
            while (j < str.size() && isdigit(str[j])) {
                x = x*10 + str[j] - '0';
                j++;
            }
            num.push(x);
            i = j-1;
        }
    
        else if (c == '(') op.push(c);
    
        else if (c == ')') {
            while (op.top() != '(') eval();
            op.pop();
        }
    
        else {
            while (op.size() && op.top() != '(' && pr[op.top()] >= pr[c]) eval();
            op.push(c);
        }
    }
  
    while (op.size()) eval();
    cout << num.top() << endl;
  
    return 0;
}
```

> 本题重在使用头文件中提供的栈类，而非自行实现。

## 3 队列

### 3.1 模拟队列

使用数组进行存储，使用指针进行维护，与模拟栈类似，懂核心原理就好。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <string>

using namespace std;

const int N = 1e5 + 10;

int q[N];
int hh = 0, tt = -1;

int main()
{
    int m;
    cin >> m;
  
    string op;
    int x;
  
    while (m--) {
        cin >> op;
        if (op == "push") {
            cin >> x;
            q[++tt] = x;
        }
    
        else if (op == "pop") hh++;
    
        else if (op == "empty") {
            if (hh > tt) cout << "YES" << endl;
            else cout << "NO" << endl;
        }
    
        else cout << q[hh] << endl;
    }
  
    return 0;
}
```

## 4 单调栈与单调队列

### 4.1 单调栈

[原题链接](https://www.acwing.com/activity/content/problem/content/867/)

单调栈的单调性主要指的是满足一定条件下的次序单调。

与普通栈的差别主要在于：在构建栈的同时也做特性相关的维护，去掉无用的信息，使得每次询问的复杂度都为O(1)。

主要需要思考的是可不可以淘汰一些旧的、影响单调性的元素。

比如初始状态下栈的内容是这样的：1、2、5。

此时扫描到4应该如何处理。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;
int q[N], tt;

int main()
{
    int n;
    cin >> n;
  
    while (n--) {
        int x;
        cin >> x;
    
        while (tt && q[tt] >= x) tt--;
        if (!tt) cout << -1 << " ";
        else cout << q[tt] << " ";
        q[++tt] = x;
    }
  
    return 0;
}
```

### 4.2 单调队列

[滑动窗口](https://www.acwing.com/activity/content/problem/content/868/)

#### 4.2.1 从框架入手

窗口的维护无需构建新的数据结构，只需要使用指针。
 “对滑动窗口的头尾指针进行控制即可动态模拟窗口的移动。“

贪心的基石永远是扫描遍历。

不要想着一次遍历就同时找到窗口的最大最小值序列，可以拆解问题多做几次遍历，根据输出结果也可以找到一些端倪。
 “每个滑动窗口的最大和最小元素分两轮遍历确认即可。“

#### 4.2.2 深入细节

学会根据样例从0开始做模拟，找到可以优化的地方，这是解决任何问题的核心。

思考单调队列的单调性应该如何，本题主要取决于窗口移动过程中不断被舍弃的元素处在哪一端，可以想想如果窗口逆向移动，应该如何选择？

思考队列里应该存什么，是数据还是下标。它取决于当窗口移动时，如何立刻找到失效的元素并剔除。

逆向思考最终需要的效果，会帮助理清细节。

#### 4.2.3 实现

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e6+10;
int a[N], q[N];
int hh, tt;

int main()
{
    int n, k;
    cin >> n >> k;
  
    for (int i = 0; i < n; i++) cin >> a[i];
  
    tt = -1;
    for (int i = 0; i < n; i++) {
        if (hh <= tt && i-k+1 > q[hh]) hh++;  // 考虑原最值是否被舍弃
        while (hh <= tt && a[q[tt]] >= a[i]) tt--;  // 考虑是否有无用信息可以简化
        q[++tt] = i;
        if (i-k+1 >= 0) cout << a[q[hh]] << " ";
    }
  
    puts("");
  
    hh = 0, tt = -1;
    for (int i = 0; i < n; i++) {
        if (hh <= tt && i-k+1 > q[hh]) hh++;
        while (hh <= tt && a[q[tt]] <= a[i]) tt--;
        q[++tt] = i;
        if (i-k+1 >= 0) cout << a[q[hh]] << " ";
    }
  
    return 0;
}
```

## 5 KMP字符串

[字符串匹配](https://www.acwing.com/problem/content/833/)

这个问题比较复杂，但有一位老哥的分享非常透彻，通俗易懂，可以参考一哈 [关于KMP的简单理解？](https://www.ruanyifeng.com/blog/2013/05/Knuth–Morris–Pratt_algorithm.html)。

比较形象的讲解可以参考[讲解](https://www.acwing.com/solution/content/14666/)。

大致的思路：

1. 构建部分匹配表

   > 这一步的实现需要在原模式串的基础上抽象构造一个虚块，代表一种特殊的形式。

2. 遍历模版串，匹配模式串

3. 找出部分匹配 or 全部匹配

实现：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

const int N = 1e5+10, M = 1e6+10;
char s[M], p[N];  // s为字符串，p为模版串
int ne[N];

using namespace std;

int main()
{
    int n, m;
    cin >> n >> p+1 >> m >> s+1;
  
    // 让模式串自比，构建部分匹配表
    for (int i = 2, j = 0; i <= n; i++) {
        while (j && p[i] != p[j+1]) j = ne[j];  // 考虑失配的情况，兼容初始化的判别
        if (p[i] == p[j+1]) j++;  // 不失配，考虑更长的匹配
        ne[i] = j;
    }
  
    // 实际匹配与上述构建匹配表时思路相似
    for (int i = 1, j = 0; i <= m; i++) {
        while (j && s[i] != p[j+1]) j = ne[j];
        if (s[i] == p[j+1]) j++;
        if (j == n) {
            cout << i-n << " ";  // 注意题干要求下标从0计数
            j = ne[j];
        }
    }
  
  
  
    return 0;
}
```

> 值得思考的问题
>
> 1. 在构建部分匹配表时，最笨的办法就是遍历寻找每个子串的部分匹配值，那么，如何简化寻找，注意在寻找过程中有哪些相似之处。（对应于构建部分匹配表）
> 2. 代码中的i和j如何理解，尤其是它们的初始化数值，这非常重要。

## 6 Trie存储结构

### 6.1 字符串存储

用树构建基本存储体系，但在实现时需要用数组来完成对树的存储。

下面是一些理解：

树的分支：不同存储元素

树的叶子：表征某数据元素的特性数据

这种存储方式的特别之处在于对目标存储元素共性的利用，从而降低存储成本。

实现：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5+10;
int son[N][26], idx, cnt[N];  // 树的节点编号从0开始，但0表示根，而son数组的初始化数值0意味着节点不存在，26代表26个小写字母
char str[N];

void insert(char *str) {
    int p = 0;  // 表示从根开始遍历，查找子节点
    for (int i = 0; str[i]; i++) {
        int u = str[i]-'a';
        if (!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
    }
    cnt[p]++;
}

int query(char *str) {
    int p = 0;
    for (int i = 0; str[i]; i++) {
        int u = str[i]-'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
  
    return cnt[p];
}

int main() {
    int n;
    cin >> n;
    while (n--) {
        char op[2];
        cin >> op >> str;
        if (*op == 'I') insert(str);
        else cout << query(str) << endl;
    }
  
    return 0;
}
```

当然也有更简单的实现：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <string>
#include <map>

using namespace std;

map<string, int> d;

int main() {
    int n;
    cin >> n;
    while (n--) {
        char op;
        string s;
        cin >> op >> s;
        if (op == 'I') d[s]++;
        else cout << d[s] << endl;
    }
  
    return 0;
}
```

> 当然这里学习的是Trie这样一个数据结构，所以不推崇这种写法。

### 6.2 最大异或对

基于Trie的存储与比较计算（注意这不是高精度哦，c++ int型存储大小为4B，也即32位，本题中单元大小为31位，所以单个数值并没有超过int上限）。

本题关键在于如何将该问题的数据存储体抽象为Trie树，即Insert函数的实现。

注意比较对象。

实现：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5+10, M = 3100010;  // M代表了所有可能的位置标签
int a[N], son[M][2], idx;

void insert(int x) {
    int p = 0;
    for (int i = 30; i >= 0; i--) {  // 越接近树根的位置位数越高，符合题意的比较方式，最高31位
        int &s = son[p][x >> i & 1];
        if (!s) s = ++idx;
        p = s;
    }
}

int search(int x) {  // 寻找与x异或后的最大值
    int p = 0, res = 0;  // p是x的最佳异或元素的信标
    for (int i = 30; i >= 0; i--) {
        int u = x >> i & 1;
        if (son[p][!u]) {
            res += 1 << i;
            p = son[p][!u];
        }
  
        else p = son[p][u];
    }
  
    return res;
}

int main()
{
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
        insert(a[i]);
    }
  
    int res = -1;
    for (int i = 0; i < n; i++) res = max(res, search(a[i]));

    cout << res;
  
    return 0;
}
```

提取数据中的某位数可以用到的方法：

1. 取模取整
2. 移位再与特定数进行按位相与

> 由于本题是集合内任意元素比较，所以贪心思路并不适用。

## 7 并查集

解决该类问题的关键是确定如何找到集合标识元，在这过程中还需要完成哪些其它工作。

除此之外，还得明白怎样合并元素，当然最关键的还是明确p[x]的意义。

### 7.1 合并集合

模版题，其它变种都是从这里延伸出来的。

实现：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;
int s[N];

int find (int x) {
    while (s[x] != x) x = s[x];
    return x;
}

void merge (int x, int y) {
    int u = find(x), v = find(y);
    if (u == v) return;
    else if (u < v) s[v] = u;
    else s[u] = v;
}

void search (int x, int y) {
    int u = find(x), v = find(y);
    if (u == v) cout << "Yes" << endl;
    else cout << "No" << endl;
}

int main()
{
    int n, m;
    cin >> n >> m;

    for (int i = 1; i <= n; i ++ ) s[i] = i;

    char op[2];
    int x, y;
    while (m--) {
        cin >> op >> x >> y;
        if (*op == 'M') merge(x, y);
        else search(x, y);
    }

    return 0;
}
```

### 7.2 联通块中点的数量

与模版不同，并查集初始化思路不同，同时还需要进行计数。

实现：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;
int s[N];

int find (int x) {
    if (s[x] < 0) return x;
    while (s[x] > 0) x = s[x];
    return x;
}

void C (int x, int y) {
    int u = find(x), v = find(y);
    if (u == v) return;
    else if (u < v) {
        s[u] += s[v];
        s[v] = u;
    }
    else {
        s[v] += s[u];
        s[u] = v;
    }

}

void Q1 (int x, int y) {
    int u = find(x), v = find(y);
    if (u == v) cout << "Yes" << endl;
    else cout << "No" << endl;
}

void Q2 (int x) {
    cout << -s[find(x)] << endl;
}

int main()
{
    int n, m;
    cin >> n >> m;

    memset(s, -1, sizeof s);

    string op;
    int x, y;
    while (m--) {
        cin >> op >> x;

        if (op == "C") {
            cin >> y;
            C(x, y);
        }

        else if (op == "Q1") {
            cin >> y;
            Q1(x, y);
        }

        else Q2(x);
    }

    return 0;
}
```

### 7.3 食物链

一个挺高端的并查集问题。

实现：

大框架：

![截屏2023-12-27 16.46.09](assets/截屏2023-12-27 16.46.09-20231227164724-8v9736g.png)

两个判断的思路：

![截屏2023-12-27 16.42.06](assets/截屏2023-12-27 16.42.06-20231227164700-nartsi8.png)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 50010;
int p[N], d[N];

int find(int x) {  // 在并查集中，p[x]是构建维护的关键
    if (p[x] != x) {
        int t = find(p[x]);  // 迭代寻找x的最终所属根
        d[x] += d[p[x]];  // 更新d[x]，使得每个点获得到根的距离
        p[x] = t;
    }
  
    return p[x];
}

int main()
{
    int n, m;
    cin >> n >> m;
  
    for (int i = 1; i <= n; i ++ ) p[i] = i;  // 初始化并查集
  
    int res = 0;
    while (m -- ) {
        int t, x, y;
        cin >> t >> x >> y;
  
        if (x > n || y > n) res++;
        else {
            int px = find(x), py = find(y);  // 找到x和y所在的根，如果过去给出过断言，那么理应是一致的
            if (t == 1) {
                if (px == py && ((d[x] - d[y]) % 3)) res++;
                else if (px != py) {
                    p[px] = py;
                    d[px] = d[y]-d[x];
                }
            }
      
            else {
                if (px == py && ((d[x]-d[y]-1) % 3)) res++;
                else if (px != py) {
                    p[px] = py;
                    d[px] = d[y]-d[x]+1;
                }
            }
        }
    }
  
    cout << res << endl;
  
    return 0;
}
```

## 8 堆

### 8.1 堆排序

> 通过一些问题了解堆的底层函数

首先需要建立对于堆的认知，堆的本质还是二叉树。通常我们在实现堆的时候是采用线性存储的方式，用数组作为存储结构。研究二叉树中各节点的编号，就能对这种静态存储方式有一个初步的理解。

堆的构建以及各种操作主要与两个底层函数相关——down和up，down主要研究目标元素在堆中是否具有下潜的能力，up则相反。通常堆有两种类型，小根堆和大根堆。这里以小根堆为例，down就是使得以目标元素为根的子树满足堆的条件，不断比较目标元素与其左右儿子（看三个元素中哪个最小，根是否要下移），做swap交换（up就略了，差不多）。

然后基于上述两个底层函数，我们可以实现堆的各种维护操作：

![截屏2024-01-02 14.50.00](assets/截屏2024-01-02 14.50.00-20240102145006-fi940h7.png)

实现：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5+10;
int cnt, h[N];

void down(int u) {
    int t = u;
    if (u*2 <= cnt && h[u*2] <= h[t]) t = u*2;
    if (u*2+1 <= cnt && h[u*2+1] <= h[t]) t = u*2+1;
    if (u != t) {
        swap(h[u], h[t]);
        down(t);
    }
}

int main()
{
    int n, m;
    cin >> n >> m;

    cnt = n;
    for (int i = 1; i <= n; i ++ ) cin >> h[i];

    for (int i = n/2; i; i--) down(i);

    while (m--) {
        cout << h[1] << " ";
        h[1] = h[cnt];
        cnt--;
        down(1);
    }

    return 0;
}
```

### 8.2 模拟堆

本题的背景依然是堆，主要难点在于审题，即如何在不断变化的堆中找到第k个插入的元素。

对于这个问题，一个直观的想法是构建位置指针，将元素位置与插入次序相关联，这样在堆的不断swap中也不会丢失它的插入次序（由于堆中存在相等的元素值，所以将插入次序与值相关联是不可行的）。

> swap函数具体拓展了什么？
>
> ![截屏2024-01-02 14.59.13](assets/截屏2024-01-02%2014.59.13-20240102145919-315bdaw.png)

我们拓展一下swap的功能就好了，注意实现过程中的各种问题：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <string>

using namespace std;
const int N = 1e5 + 10;

int cnt, m, h[N], hp[N], ph[N];

void heap_swap(int a, int b) {  // 交换堆中a号与b号元素，同时调整与它们相关联的索引元素
    swap(ph[hp[a]], ph[hp[b]]);
    swap(hp[a], hp[b]);
    swap(h[a], h[b]);
}

int down(int u) {
    int t = u;
    if (u*2 <= cnt && h[u*2] < h[t]) t = u*2;
    if (u*2+1 <= cnt && h[u*2+1] < h[t]) t = u*2+1;
    if (u != t) {
        heap_swap(u, t);
        down(t);
    }
}

void up(int u) {
    if (u/2 && h[u/2] > h[u]) {  // 这里容易出问题的地方是等号，至于原因，还需要思考
        heap_swap(u/2, u);
        up(u/2);
    }
}

// 非递归形式
// void up(int u)
// {
//     while (u / 2 && h[u] < h[u / 2])
//     {
//         heap_swap(u, u / 2);
//         u >>= 1;
//     }
// }

int main()
{
    int n;
    cin >> n;

    string op;
    int k, x;
    while (n--) {
        cin >> op;
        if (op == "I") {
            m++;
            cin >> x;
            h[++cnt] = x;  // cnt号位（未调整堆的最后一个数）的值是x
            ph[m] = cnt;  // 第m个插入的数位于cnt号位
            hp[cnt] = m;  // 第cnt号位是第m个插入的数
            up(cnt);
        }

        if (op == "PM") cout << h[1] << endl;

        if (op == "DM") {
            heap_swap(1, cnt);
            cnt--;
            down(1);
        }

        if (op == "D") {
            cin >> k;
            k = ph[k];  // 一个至关重要的差异点，在删除掉第k个插入的元素后，就无法再根据ph[k]去寻找置换之后的元素位置了
            heap_swap(k, cnt);
            cnt--;
            down(k);
            up(k);  // 因为这里采取的是将末位数与待处理的任意数交换，因此up和down都需要考虑
        }

        if (op == "C") {
            cin >> k >> x;
            k = ph[k];
            h[k] = x;
            down(k);
            up(k);
        }
    }

    return 0;
}
```

## 9 散列表

### 9.1 模拟散列

散列表的作用主要是将我们常用的离散数据进行紧密化存储，提高空间的利用率。它有两种主要的实现：开放寻址法和拉链法。

开放寻址法实现：

主要维护的数据结构是数组，做简单线性的处理。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10, null = 0x3f3f3f3f;  // 设置散列长度，自定，足够大即可；null需要大于题目给出的数据范围。
int h[N];

int find(int x) {
    int k = (x % N + N) % N;  // 设置散列函数
  
    int t = k;
    while (h[t] != null && h[t] != x) {
        t++;
        if (t == N) t = 0;
    }
  
    return t;
}

int main()
{
    int n;
    cin >> n;
    memset(h, 0x3f, sizeof h);
  
    string op;
    int x;
  
    while (n--) {
        cin >> op >> x;
        if (op == "I") h[find(x)] = x;
        else {
            if (h[find(x)] == null) cout << "No" << endl;
            else cout << "Yes" << endl;
        }
    }
  
    return 0;
}
```

拉链法实现：

主要维护的数据结构是静态十字链表。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10, null = 0x3f3f3f3f;  // 设置散列长度，自定，足够大即可；null需要大于题目给出的数据范围。
int h[N], e[N], ne[N], idx;

void insert (int x) {
    int k = (x % N + N) % N;  // 设置散列函数
    e[idx] = x;
    ne[idx] = h[k];
    h[k] = idx++;  // 注意idx的自增
}

bool find(int x) {
    int k = (x % N + N) % N;  // 设置散列函数
    for (int i = h[k]; i != -1; i = ne[i]) {
        if (e[i] == x) return true;
    }
    return false;
}

int main()
{
    int n;
    cin >> n;
    memset(h, -1, sizeof h);
  
    string op;
    int x;
  
    while (n--) {
        cin >> op >> x;
        if (op == "I") insert(x);
        else {
            if (find(x)) cout << "Yes" << endl;
            else cout << "No" << endl;
        }
    }
  
    return 0;
}
```

### 9.2 字符串哈希

实现：

![截屏2024-01-02 16.39.12](assets/截屏2024-01-02%2016.39.12-20240102163919-oaez6hk.png)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

typedef unsigned long long ULL;  // 这里由于每个字符所对应的ASCII码可能非常大，又是非负数，所以选择扩大范围，同时溢出了也不用特殊考虑。

using namespace std;
const int N = 1e5+10, P = 131;  // 这里P的选取和ASCII码的范围0-127有关

ULL h[N], p[N];
char str[N];

int get(int l, int r) {
    return h[r] - h[l-1]*p[r-l+1];
}

int main()
{
    int n, m;
    cin >> n >> m >> str+1;
  
    p[0] = 1;  // 保证p[k]就是P的k次方
    for (int i = 1; i <= n; i++) {
        h[i] = h[i-1]*P + str[i];
        p[i] = p[i-1] *P;
    }
  
    int l1, r1, l2, r2;
    while (m--) {
        cin >> l1 >> r1 >> l2 >> r2;
        if (get(l1, r1) == get(l2, r2)) cout << "Yes" << endl;  // 不可能存在两个不同的子串哈希值相同。
        else cout << "No" << endl;
    }
  
    return 0;
}
```

