# Acweek1


 工作算法准备第一周，a new beginning！<!--more-->

## 1 排序

### 1.1 快速排序

[原题链接](https://www.acwing.com/activity/content/problem/content/819/)

基本思路：

1. 确定一个参照数据，初始化头尾指针，分别移动指针，向参照数据逼近。
2. 先移动左指针，找到大于参照元素的位置；再移动右指针，找到小于参照元素的位置。
3. 如果合法，即左指针位置小于右指针，就交换两个位置上的元素。
4. 重复上述3步，分治参照数据左右两边的数列，直至找到所有元素应该在的位置。

代码：

```c++
#include <iostream>
#include <cstdio>
#include <cstring>

using namespace std;

const int N = 1e5+10;
int a[N];

int n;

void sort(int *q, int l, int r) {
    if (l >= r) return;
    int x = q[l + r >> 1];
    int i = l-1, j = r+1;
    while (i < j) {
        do i++; while (q[i] < x);
        do j--; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    sort(q, l, j);
    sort(q, j+1, r);
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i++) cin >> a[i];
    sort(a, 0, n-1);
    for (int i = 0; i < n; i++) cout << a[i] << " ";
    return 0;
}
```

拓展：

1. sort(q, l, j)在分治时要注意数列头尾描述的合理性。不能写成sort(q, l, i)，例如数列1 0在这样处理时会发生死循环。不能想当然地认为l与j是相等的。
2. 数列越有序，快排性能越低；越无序，快排性能越高。

### 1.2 归并排序

[原题链接](https://www.acwing.com/activity/content/problem/content/821/)

基本思路：

与快排的思路相似，两者都以分治算法为核心，只不过归并排序是先分组再处理，快排顺序相反罢了。注意每一步处理的数据对象是什么，哪些步骤可以同步处理，哪些步骤可以异步处理。

代码：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;
int a[N], b[N];

int n;

void sort(int *q, int l, int r) {
    if (l >= r) return;
    int mid = (l + r >> 1);
    sort(q, l, mid);
    sort(q, mid+1, r);
  
    int i = l, j = mid+1, s = l;
    while (i <= mid && j <= r) {
        if (q[i] <= q[j]) b[s] = q[i++];
        else b[s] = q[j++];
        s++;
    }
    for (int u = i; u <= mid; u++) b[s] = q[u], s++;
    for (int u = j; u <= r; u++) b[s] = q[u], s++;
  
    for (int u = l; u <= r; u++) q[u] = b[u];
}

int main() {
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> a[i];
    sort(a, 0, n-1);
    for (int i = 0; i < n; i ++ ) cout << a[i] << " ";
    return 0;
}
```

拓展：

归并排序是稳定算法。

## 2 二分

[原题链接](https://www.acwing.com/activity/content/problem/content/823/)

基本思路：

基于可重有序数列，不断取中部数据进行判断，缩小目标元素所在范围，常见问题是寻找重复数据的左部位置和右部位置。

代码：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5+10;
int a[N];
int n, q, k;

int main()
{
    cin >> n >> q;
    for (int i = 0; i < n; i ++ ) cin >> a[i];
    while (q--) {
        cin >> k;
  
        int l = 0, r = n-1;
        while (l < r) {
            int mid = (l + r >> 1);
            if (a[mid] >= k) r = mid;  // 寻找左部数据
            else l = mid + 1;
        }
  
        int left = l;
  
        if (a[left] != k) cout << "-1 -1" << endl;
        else {
            l = 0, r = n-1;
            while (l < r) {
                int mid = (l + r + 1 >> 1);  // 避免进入死循环，若不加1对于“1 0”就不合法，找1会进入死循环，找0则找不到。
                if (a[mid] > k) r = mid - 1;  // 寻找右部数据
                else l = mid;
            }
      
            int right = l;
            cout << left << " " << right << endl;
        }
    } 
}
```

## 3 高精度

### 3.1 高精度加法

原题链接

基本思路：

1. 对大长度数据使用向量逆序存储。
2. 逆向思考，注意最终结果每一位是由哪些部分组成的，包含上一位的进位、两数对应位的和。
3. 注意向量vector这一数据结构的特性，它是动态数组，需要避免访问越界问题。

代码：

```C++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>
#include <string>

using namespace std;

vector<int> a, b;

void add(vector<int> a, vector<int> b) {
    vector<int> c;
    int t = 0;
    for (int i = 0; i < a.size(); i++) {
        t += a[i];
        if (i < b.size()) t += b[i];
        c.push_back(t%10);
        t /= 10;
    }
  
    if (t) c.push_back(t);
  
    for (int i = c.size()-1; i >= 0; i--) cout << c[i];
}

int main()
{
    string s1, s2;
    cin >> s1 >> s2;
  
    int n1 = s1.length(), n2 = s2.length();
    for (int i = n1-1; i >= 0; i--) a.push_back(s1[i]-'0');
    for (int i = n2-1; i >= 0; i--) b.push_back(s2[i]-'0');
  
    if (n1 >= n2) add(a, b);
    else add(b, a);
  
    return 0;
}
```

> 先关注思路和逻辑，再想想哪些地方可以优化。

### 3.2 高精度减法

原题链接

基本思路：

- 与高精度加采取的数据结构一致，只是操作规则不同
- 先判断两数的大小关系，再做相减，某种情况下加上负号

代码：

```c++
#include <iostream>
#include <vector>

using namespace std;

bool cmp(vector<int> &A, vector<int> &B) 比较两数大小，从位数、各位数大小分析
{
    if (A.size() != B.size()) return A.size() > B.size();

    for (int i = A.size() - 1; i >= 0; i -- )
        if (A[i] != B[i])
            return A[i] > B[i];

    return true;
}


vector<int> sub(vector<int> &A, vector<int> &B)  两数相减
{
    vector<int> C;
    int t = 0;
    for(int i = 0; i < A.size(); i++)
    {
        t = A[i] - t;
        if(i < B.size()) t -= B[i];
        C.push_back((t + 10) % 10);
        if(t < 0) t = 1;
        else t = 0;
    }
    while(C.size() > 1 && C.back() == 0) C.pop_back();  考虑减完后高位为0的情况
    return C;
}

int main()
{
    string a, b;
    cin >> a >> b;
    vector<int> A, B, C;
    for(int i = a.size() - 1; i >= 0; i--) A.push_back(a[i] - '0');
    for(int i = b.size() - 1; i >= 0; i--) B.push_back(b[i] - '0');
    if(cmp(A, B)) C = sub(A, B);
    else 
    {
        C = sub(B, A);
        cout << '-';
    }
    for(int i = C.size() - 1; i >= 0; i--) cout << C[i];
    return 0;
}
```

拓展：

- 注意去掉计算过程中产生的前导零
- 可以对代码做进一步优化

### 3.3 高精度乘法

[原题链接](https://www.acwing.com/activity/content/problem/content/827/)

基本思路：

注意乘法基本原理即可，其它方面与一般高精度运算相当。

代码：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>
#include <string>

using namespace std;

vector<int> a;

void acc(vector<int> a, int b) {
    int t = 0;
    vector<int> c;
    for (int i = 0; i < a.size(); i ++ ) {  // 注意乘法运算的基理: a*b = a[0]*b + a[1]*10*b + a[2]*100*b + ...
        t += a[i]*b;
        c.push_back(t%10);
        t /= 10;
    }
    if (t) c.push_back(t);
    while (c.back() == 0 && c.size() != 1) c.pop_back();  // 注意vector相关函数操作
    for (int i = c.size()-1; i >= 0; i -- ) cout << c[i];
}

int main()
{
    string s;
    int b;
    cin >> s >> b;
    int n = s.length();
    for (int i = n-1; i >= 0; i --) a.push_back(s[i]-'0');
  
    if (a[n-1] == 0 || b == 0) cout << 0;
    else acc(a, b);
  
    return 0;
}
```

### 3.4 高精度除法

[原题链接](https://www.acwing.com/activity/content/problem/content/828/)

基本思想：

与常规除法运算一致，注意结果的处理。

代码：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>
#include <string>

using namespace std;

vector<int> a;

void div(vector<int> a, int b) {
    int t = 0;
    vector<int> c;
    for (int i = a.size()-1; i >= 0; i--) {  // 注意此处与常规除法运算顺序一致，但核心仍然是以各位为核心。
        t = t*10 + a[i];
        c.push_back(t/b);
        t %= b;
    }
    reverse(c.begin(), c.end());  // 考虑到除法结果的特殊性，选择对结果做倒置
    while (c.back() == 0 and c.size() != 1) c.pop_back();
    for (int i = c.size()-1; i >= 0; i--) cout << c[i];
    cout << endl << t;
}

int main()
{
    string s;
    int b;
    cin >> s >> b;
    int n = s.length();
    for (int i = n-1; i >= 0; i --) a.push_back(s[i]-'0');
  
    if (a[n-1] == 0) cout << 0 << endl << 0;
    else div(a, b);
  
    return 0;
}
```

## 4 前缀和

### 4.1 模板

原题链接

基本思路：

简单对前n项进行累加，再使用前缀和做相关处理。

代码：

```c++
#include <iostream>

using namespace std;

const int N = 100010;
int a[N], s[N];

int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    for(int i = 1; i <= n; i++) scanf("%d", &a[i]);
    for(int i = 1; i <= n; i++) s[i] = s[i-1] + a[i];
    while(m--)
    {
        int l, r;
        scanf("%d%d", &l, &r);
        printf("%d\n", s[r] - s[l - 1]); // - 1是离散性的体现
    }
    return 0;
}
```

### 4.2 二维前缀和

原题链接

基本思路：

构建多维前缀和：考虑每个以原点和各坐标处数据为对角元的矩阵内数据的和

| 0, 0 |        |        |
| ---- | ------ | ------ |
|      |        | x-1, y |
|      | x, y-1 | x, y   |

操作前缀和：思路相当，剪完再补

| x1-1, y1-1 |        | x1-1, y2 |
| ---------- | ------ | -------- |
|            | x1, y1 |          |
| x2, y1-1   |        | x2, y2   |

代码：

```c++
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 1010;
int a[N][N], s[N][N];

int main()
{
    int n, m, q, k;
    int x1, y1, x2, y2;
    cin >> n >> m >> q;
  
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= m; j++){
            cin >> k;
            a[i][j] = k;
        }
    
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= m; j++){
            s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + a[i][j];
        }
    
    while(q--){
        cin >> x1 >> y1 >> x2 >> y2;
        cout << s[x2][y2]-s[x1-1][y2]-s[x2][y1-1]+s[x1-1][y1-1] << endl;
    }
  
    return 0;
}
```

### 4.3 差分

原题链接

基本思路：

将每一个基本数据元素看作是差分序列的前缀和，将对原数据的操作复杂度从O(n)转变为O(1)。

注意题目中描述的l、r均是数组中第l、r个元素。

代码:

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5+10;
int a[N], b[N];

int n, m;

void insert(int l, int r, int c) {
    b[l] += c;
    b[r+1] -= c;
}

int main()
{
    cin >> n >> m;
  
    for (int i = 0; i < n; i++) cin >> a[i];
  
    for (int i = 0; i < n; i++) insert(i, i, a[i]);
  
    while (m--) {
        int l, r, c;
        cin >> l >> r >> c;
        insert(l-1, r-1, c);
    }
  
    for (int i = 1; i < n; i ++ ) b[i] += b[i-1];
    for (int i = 0; i < n; i ++ ) cout << b[i] << " ";
  
    return 0;
}
```

### 4.4 二维差分

原题链接

基本思路：

基于差分，与普通差分的主要差异在于操作函数的具体步骤，需要多画图测试，使得每个数据的值能够与一定规则下的前缀和一致。

构造前缀和：参考4.2 二维前缀和

操作函数的设计：主要设置区域内的差分数值和区域外的差分数值，保证以原点和这些元素为对角元的子矩阵数据和能够表征原矩阵该位置的数值。

|      |          |      |        |            |
| ---- | -------- | ---- | ------ | ---------- |
|      | x1, y1   |      |        | x1, y2+1   |
|      |          |      |        |            |
|      |          |      | x2, y2 |            |
|      | x2+1, y1 |      |        | x2+1, y2+1 |

代码：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1010;
int a[N][N], b[N][N];

int n, m, q;

void insert(int x1, int y1, int x2, int y2, int c) {
    b[x1][y1] += c;
    b[x1][y2+1] -= c;
    b[x2+1][y1] -= c;
    b[x2+1][y2+1] += c;
}

int main()
{
    cin >> n >> m >> q;
    for (int i = 1; i <= n; i++)  // 注意这里从1号位置而非0号位置开始存储，这样在做二维前缀和时才不会发生越界。
        for (int j = 1; j <= m; j++)
            cin >> a[i][j];
      
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            insert(i, j, i, j, a[i][j]);
  
    while (q--) {
        int x1, y1, x2, y2, c;
        cin >> x1 >> y1 >> x2 >> y2 >> c;
        insert(x1, y1, x2, y2, c);
    }
  
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            b[i][j] = b[i-1][j] + b[i][j-1] - b[i-1][j-1] + b[i][j];
  
    for (int i = 1; i <= n; i++) {
        if (i != 1) puts("");
        for (int j = 1; j <= m; j++) {
            cout << b[i][j] << " ";
        }
    }
  
    return 0;
}
```

## 5 双指针

### 5.1 最长连续不重复子序列

原题链接

基本思路：

该题为双指针的第一种应用场景，单数列双指针。

从模拟出发，遍历每个连续子序列。过程中需要用到双指针，但注意指针的移动存在依赖性，以序列尾部对子序列进行划分，可以发现每个指针的移动方向是唯一的。假设i < j，如果已有[i, j]序列满足条件，那么不可能有[i-k, j+1]满足条件。基于上述性质，我们可以将整个过程的遍历复杂度由O(n^2)降至O(n)。

双指针基础模板:

```c++
for (int i = 0, j = 0; i < n; i++) {
	while (j < i && check(i, j)) j++;
	进行统计计算...
}
```

代码：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5+10;
int a[N], s[N];  // s[N]统计了序列中各数值出现的次数
int n;

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> a[i];
  
    int j = 0, res = 0;
    for (int i = 0; i < n; i++) {
        s[a[i]]++;  // 加入a[i]，统计数值出现次数
        while (j < i && s[a[i]] > 1) s[a[j++]]--;  // 将a[j]移出序列，统计数值出现次数
        res = max(res, i - j + 1);
    }
  
    cout << res << endl;
  
    return 0;
}
```

### 5.2 数组元素的目标和

原题链接

基本思路：

找准双指针的移动方向，利用指针之间的相互影响对某个指针的移动范围进行缩小，筛掉不必要的步骤，也就找到了优化的方向。

代码：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;
int a[N], b[N];

int n, m, x;

int main()
{
    cin >> n >> m >> x;
  
    for (int i = 0; i < n; i ++ ) cin >> a[i];
    for (int i = 0; i < m; i ++ ) cin >> b[i];
  
    for (int i = 0, j = m-1; i < n; i++) {
        while (j >= 1 && a[i] + b[j] > x) j--;
        if (j >= 0 && a[i] + b[j] == x) {
            cout << i << " " << j;
        }
    }
  
    return 0;
}
```

### 5.3 判断子序列

原题链接

基本思路：

与5.2 数组元素的目标和解法相当，只不过初始化时两个指针都需要放在数组的头部。

代码：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5+10;
int a[N], b[N];
int n, m;

int main()
{
    cin >> n >> m;
  
    for (int i = 0; i < n; i ++ ) cin >> a[i];
    for (int i = 0; i < m; i ++ ) cin >> b[i];
  
    int ant = 0;
    for (int i = 0, j = 0; i < n; i++) {
        while (j < m && a[i] != b[j]) j++;
        if (j < m && a[i] == b[j]) {
            ant++;
            j++;
            continue;
        }
    }
  
    if (ant == n) cout << "Yes";
    else cout << "No";
  
    return 0;
}
```

## 6 位运算、离散化、区间合并

### 6.1 二进制数中1的个数

原题链接

基本思路：

熟悉二进制与十进制之间相互转化的算法，利用该原理将二进制形式下的每一位拆出来就行。

代码：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5+10;
int a[N], b[N];
int n;

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> a[i];
  
    for (int i = 0; i < n; i ++ ) {
        int ant = 0, t = a[i];
        while (t != 0) {
            if (t%2) ant++;
            t /= 2;
        }
        b[i] = ant;
    }
  
    for (int i = 0; i < n; i ++ ) cout << b[i] << " ";
  
    return 0;
}
```

### 6.2 区间和（离散化）

原题链接

基本思路：

对于大范围数据，采用离散化思路，将它们放在一定长度的有序数列。

1. 暂存各种修改与请求操作，并记录这些操作的元素下标。
2. 对存储的元素下标进行排序和去重，保证数据的前后顺序一致。
3. 再次遍历修改操作，直接作用于离散化数组，然后在此基础上预处理离散化前缀和。
4. 再次遍历查询操作，利用前缀和序列计算结果。

简单来说就是经历了数据离散化、预处理前缀和、利用前缀和求解这三个过程。

代码：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

typedef pair<int, int> PII;
const int N = 300010;

int a[N], s[N];
vector<PII> add, query;
vector<int> alls;

int n, m;

int find(int x) {
    int l = 0, r = alls.size()-1;
    while (l < r) {
        int mid = (l + r >> 1);
        if (alls[mid] >= x) r = mid;
        else l = mid+1;
    }
    return r+1;
}

int main()
{
    cin >> n >> m;
  
    while (n--) {
        int x, c;
        cin >> x >> c;
        add.push_back({x, c});
        alls.push_back(x);
    }
  
    while (m--) {
        int l, r;
        cin >> l >> r;
        query.push_back({l, r});
        alls.push_back(l);
        alls.push_back(r);
    }
  
    sort(alls.begin(), alls.end());
    alls.erase(unique(alls.begin(), alls.end()), alls.end());
  
    for (auto item: add) {
        int u = item.first, v = item.second;
        int x = find(u);
        a[x] += v;
    }
  
    for (int i = 1; i <= alls.size(); i++) s[i] = s[i-1] + a[i]; 
  
    for (auto item: query) {
        int u = find(item.first), v = find(item.second);
        cout << s[v] - s[u-1] << endl;
    }
  
    return 0;
}
```

### 6.3 区间合并

原题链接

基本思路：

1. 存储区间，并按左端点进行排序，这样对于两个区间只需要考虑3种而非4种情况。
2. 贪心，分析区间的左右端点即可，遍历每个区间，分情况讨论即可。

代码：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>


using namespace std;

typedef pair<int, int> PII;

vector<PII> segs;

int n;

void merge(vector<PII> &segs) {
    vector<PII> res;
    int st = -2e9, ed = 2e9;
    for (auto seg: segs) {
        if (st == -2e9 || seg.first > ed) {
            res.push_back(seg);
            st = seg.first;
            ed = seg.second;
        }
    
        else ed = max(ed, seg.second);
    }
  
    segs = res;
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) {
        int l, r;
        cin >> l >> r;
        segs.push_back({l, r});
    }
  
    sort(segs.begin(), segs.end());
  
    merge(segs);
  
    cout << segs.size();
  
    return 0;
}
```

