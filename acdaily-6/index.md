# ACDaily 6




# 背包问题练习与LeetCode初体验

这两天做题效率低了一些，不过背包是一块难啃的骨头，要集中一些。

## 波动数列

观察这个数列：

1 3 0 2 -1 1 -2 …

这个数列中后一项总是比前一项增加2或者减少3，且每一项都为整数。

栋栋对这种数列很好奇，他想知道长度为 n 和为 s 而且后一项总是比前一项增加 a 或者减少 b 的整数数列可能有多少种呢？

#### 输入格式

共一行，包含四个整数 n,s,a,b，含义如前面所述。

#### 输出格式

共一行，包含一个整数，表示满足条件的方案数。

由于这个数很大，请输出方案数除以 100000007 的余数。

#### 数据范围

1≤n≤1000,
−109≤s≤109,
1≤a,b≤106

#### 输入样例：

4 10 2 3

#### 输出样例：

2

#### 样例解释

两个满足条件的数列分别是2 4 1 3和7 4 1 -2。

#### 题解：

```c++
#include <iostream>
using namespace std;

const int N = 1010, MOD = 100000007;
int f[N][N];

int get_mod(int a, int b)
{
    return (a%b + b) % b;
}

int main()
{
    int n, s, a, b;
    cin >> n >> s >> a >> b;
    
    f[0][0] = 1;
    for(int i = 1; i <= n-1; i++)
        for(int j = 0; j < n; j++)
        {
            f[i][j] = (f[i-1][get_mod(j-a*(n-i), n)]+f[i-1][get_mod(j + b*(n-i), n)]) % MOD;
        }
        
    cout << f[n-1][get_mod(s, n)] << endl;
    return 0;
}
```

按照我们在[ACDaily 5](https://www.waitingfor.cn/2022/03/05/ACDaily/ACDaily-5/)提到的标准流程，先做好状态表示，再做好状态计算。

根据题意我们要求的是所有方案数，因此状态数组内存的应该是方案数。那么我们要给出的，是哪一类方案数呢。根据题意，我们可以设置一个起点数x，之后的每个数由于只比前一个数大a或少b，我们把它们记作di，表示相对于第i-1项的变化量。

在做了以上数学转换后，我们就可以得到一个等值的表达式。再来看看目的：n个数的序列之和应该是n的倍数，因此，我们可以将问题转化为：序列和s与di组合mod n达到同余要求的所有方案。

所以我们可以用`f[i][j]`表示前i个d(d1~dn-1)的所有组合同余j的方案数。

接着，我们根据化整为零的思想按照序列最后一项的区别对集合进行划分。观察到最后一项仅有`(n-i)*a`和`-(n-1)*b`两种可能，于是我们将这两种情况的方案数相加就可以得到当下情况的方案数。

再经过状态的合并来获得目标方案。

- 几点要明确的

1. C++中负数模正数是可以得到负余数的，例如`a < 0, b > 0` `a%b`就属于`-(b-1) ~ (b-1)`，因此在本题中设计了函数将负余数转化为正余数。
2. 一切从实际出发，考虑各特征的合理范围
3. 特别注意初始化起点的值
3. 由于MOD为9位数，所以对于多数相加取模问题，我们需要一步一步地先做加法再取模，也就是加一个数，取一次模，再加一个数，再取一次模，直到把所有数加完。

## LeetCode初体验

第一次参加LeetCode周赛，有许多不适应的地方，不过参加过了就有了一些了解与体会。

1. LeetCode系统会默认添加所有C++头文件，这也是很多人喜欢用C++打比赛的原因，因为STL的数据结构集成性非常好。
2. 在代码区，系统会为你提供一个Solution类，并且给出解答该题的函数设计框架，算是一种提示吧。如果需要全局变量，我们就需要设计静态成员，然后初始化；一般变量则可以直接在函数内定义。其实，规范一些，我们的所有变量都应该在数据成员区进行定义。
3. 要多打打这些有时限的算法比赛，激活自己的状态，同时也能意识到自己在哪些地方有不足，是一种很好的反馈。
4. 这次只AC了一道题，与大佬们还有很大差距，冲冲冲。

## Excel表中某个范围内的单元格

概述：给定一个Excel表格区间`letter1 i:letter2 j`并且`'A'<= letter1 <= letter2 <= 'Z', i <= j`，要求输出这个范围的所有格子，假如给定的区间为`K1:L2`,那么我们就应该输出向量`["K1", "K2", "L1", "L2"]`，可以看到，在满足条件的区域遍历的顺序为由上而下，由左而右。

原题链接可以参考[LeetCode.6016](https://leetcode-cn.com/problems/cells-in-a-range-on-an-excel-sheet/)

### 原始人做法

```c++
class Solution {
public:
    vector<string> cellsInRange(string s) {
        vector<string> list;
        char a[2];
        char b[2];
        char letter[26];
        
        for(int i = 0; i < 26; i++)
            letter[i] = i+65;
        
        char *str = (char*)s.data();
        const char *d = ":";
        char *p;
        p = strtok(str, d);
        a[0] = p[0];
        b[0] = p[1];
        p = strtok(nullptr, d);
        a[1] = p[0];
        b[1] = p[1];
        
        int num1 = b[0]-'0';
        int num2 = b[1]-'0';
        int letter1 = a[0]-65;
        int letter2 = a[1]-65;
        
        for(int i = letter1; i <= letter2; i++)
            for(int j = num1; j <= num2; j++)
            {
                const char num = j+48;
                char p[80];
                p[0] = letter[i];
                p[1] = num;
                list.insert(list.end(), p);
            }
        
        return list;
    }
};
```

### STL加持做法

```c++
class Solution {
public:
    vector<string> cellsInRange(string s) {
        char a = s[0], b = s[3];
        int p = s[1] - '0', q = s[4] - '0';
        vector<string> ans;
        for (char c = a; c <= b; ++c) {
            for (int d = p; d <= q; ++d) {
                string t{c, (char)(d + '0')};
                ans.push_back(t);
            }
        }
        return ans;
    }
};
```

STL作用太大了，需要好好学学。

其实STL对于我们学习数据结构也是很有帮助的，以STL数据结构为参照，对应其中的功能来实现我们自己的类应该是一种更好的学法，毕竟我们学习数据结构不是为了在使用时造轮子而是更好地去驾驭已经比较完善的库中的它们，毕竟STL内的数据结构实现比我们要完善的多吧doge。

