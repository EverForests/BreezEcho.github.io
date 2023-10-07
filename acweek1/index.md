# Acweek1


 工作算法准备第一周，a new beginning！

  **<!--more-->**

#  **基础算法**

## **1 排序**

### 1.1 [快速排序](https://www.acwing.com/activity/content/problem/content/819/)

**基本思路：**

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

