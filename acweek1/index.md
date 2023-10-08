# Acweek1


 工作算法准备第一周，a new beginning！<!--more-->

##  基础算法

### 1 排序

#### 1.1 快速排序

[原题链接](https://www.acwing.com/activity/content/problem/content/819/)

**基本思路**：

1. 确定一个参照数据，初始化头尾指针，分别移动指针，向参照数据逼近。
2. 先移动左指针，找到大于参照元素的位置；再移动右指针，找到小于参照元素的位置。
3. 如果合法，即左指针位置小于右指针，就交换两个位置上的元素。
4. 重复上述3步，分治参照数据左右两边的数列，直至找到所有元素应该在的位置。

**代码：**

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

**拓展：**

1. `sort(q, l, j)`在分治时要注意数列头尾描述的合理性。不能写成`sort(q, l, i)`，例如数列`1 0`在这样处理时会发生死循环。不能想当然地认为l与j是相等的。
2. 数列越有序，快排性能越低；越无序，快排性能越高。

#### 1.2 归并排序

[原题链接](https://www.acwing.com/activity/content/problem/content/821/)

**基本思路：**

与快排的思路相似，两者都以分治算法为核心，只不过归并排序是先分组再处理，快排顺序相反罢了。注意每一步处理的数据对象是什么，哪些步骤可以同步处理，哪些步骤可以异步处理。

**代码：**

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

**拓展**：

归并排序是稳定算法。

### 2 二分

[原题链接](https://www.acwing.com/activity/content/problem/content/823/)

**基本思路：**

基于可重有序数列，不断取中部数据进行判断，缩小目标元素所在范围，常见问题是寻找重复数据的左部位置和右部位置。

**代码：**

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


