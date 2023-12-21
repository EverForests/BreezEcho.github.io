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

## 6 Trie字符串统计

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


