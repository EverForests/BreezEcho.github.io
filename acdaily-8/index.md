# ACDaily 8




# 枚举、模拟问题续

## 1.移动距离

X星球居民小区的楼房全是一样的，并且按矩阵样式排列。

其楼房的编号为  1,2,3… 
当排满一行时，从下一行相邻的楼往反方向排号。

比如：当小区排号宽度为 6 时，开始情形如下：

1  2  3  4  5  6
12 11 10 9  8  7
13 14 15 .....
我们的问题是：已知了两个楼号 m 和 n，需要求出它们之间的最短移动距离（不能斜线方向移动）。

#### 输入格式

输入共一行，包含三个整数 w,m,n，w 为排号宽度，m,n 为待计算的楼号。

#### 输出格式

输出一个整数，表示 m,n 两楼间最短移动距离。

#### 数据范围

1≤w,m,n≤10000,

#### 输入样例：

6 8 2

#### 输出样例：

4

#### 题解：

考虑枚举每个数，按照排列方式将单个数字转化为对应的二维坐标，进行计算即可。

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>

using namespace std;

int main()
{
    int w, m, n;
    cin >> w >> m >> n;
    
    int mx, my, nx, ny;
    for(int i = 0; i < 10000; i++)
    {
        int min = w*i+1, max = w*(i+1);
        if(m >= min && m <= max && i%2 == 0) mx = m-min, my = i;
        if(m >= min && m <= max && i%2 != 0) mx = max-m, my = i;
        if(n >= min && n <= max && i%2 == 0) nx = n-min, ny = i;
        if(n >= min && n <= max && i%2 != 0) nx = max-n, ny = i;
    }
    
    cout << abs(mx-nx)+abs(my-ny) << endl;
    return 0;
}
```



## 2.日期问题

小明正在整理一批历史文献。这些历史文献中出现了很多日期。

小明知道这些日期都在1960年1月1日至2059年12月31日。

令小明头疼的是，这些日期采用的格式非常不统一，有采用年/月/日的，有采用月/日/年的，还有采用日/月/年的。

更加麻烦的是，年份也都省略了前两位，使得文献上的一个日期，存在很多可能的日期与其对应。

比如02/03/04，可能是2002年03月04日、2004年02月03日或2004年03月02日。

给出一个文献上的日期，你能帮助小明判断有哪些可能的日期对其对应吗？

#### 输入格式

一个日期，格式是”AA/BB/CC”。

即每个’/’隔开的部分由两个 0-9 之间的数字（不一定相同）组成。

#### 输出格式

输出若干个不相同的日期，每个日期一行，格式是”yyyy-MM-dd”。

多个日期按从早到晚排列。

#### 数据范围

0≤A,B,C≤9

#### 输入样例：

02/03/04

#### 输出样例：

2002-03-04
2004-02-03
2004-03-02

#### 题解：

架构：

1. 选择规格化输入，可以避免很多麻烦
2. 已知日期范围，枚举日期种数，简化代码逻辑
3. 按题意对日期进行自由组合
4. 判别日期的合理性
5. 规格化输出，注意补前导零

```C++
#include <cstdio>
#include <iostream>

using namespace std;

int days[13] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

bool check_valid(int year, int month, int day)
{
    if (month == 0 || month > 12) return false;
    if (day == 0) return false;
    if (month != 2)
    {
        if (day > days[month]) return false;
    }
    else
    {
        int leap = ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0));
        if (day > days[month] + leap) return false;
    }

    return true;
}

int main()
{
    int a, b, c;
    scanf("%d/%d/%d", &a, &b, &c);

    for (int date = 19600101; date <= 20591231; date ++ )
    {
        int year = date / 10000, month = date % 10000 / 100, day = date % 100;
        if (check_valid(year, month, day))
        {
            if ((year % 100 == a && month == b && day == c) ||  // 年/月/日
                (month == a && day == b && year % 100 == c) ||  // 月/日/年
                (day == a && month == b && year % 100 == c) )   //月/日/年
                printf("%d-%02d-%02d\n", year, month, day);  //补前导0
        }
    }

    return 0;
}
```



## 3.航班时间

小 h 前往美国参加了蓝桥杯国际赛。

小 h 的女朋友发现小 h 上午十点出发，上午十二点到达美国，于是感叹到“现在飞机飞得真快，两小时就能到美国了”。

小 h 对超音速飞行感到十分恐惧。

仔细观察后发现飞机的起降时间都是当地时间。

由于北京和美国东部有 12 小时时差，故飞机总共需要 14 小时的飞行时间。

不久后小 h 的女朋友去中东交换。

小 h 并不知道中东与北京的时差。

但是小 h 得到了女朋友来回航班的起降时间。

小 h 想知道女朋友的航班飞行时间是多少。

对于一个可能跨时区的航班，给定来回程的起降时间。

假设飞机来回飞行时间相同，求飞机的飞行时间。

#### 输入格式

一个输入包含多组数据。

输入第一行为一个正整数 T，表示输入数据组数。

每组数据包含两行，第一行为去程的起降时间，第二行为回程的起降时间。

起降时间的格式如下:

h1:m1:s1 h2:m2:s2
h1:m1:s1 h3:m3:s3 (+1)
h1:m1:s1 h4:m4:s4 (+2)
第一种格式表示该航班在当地时间h1时m1分s1秒起飞，在当地时间当日h2时m2分s2秒降落。

第二种格式表示该航班在当地时间h1时m1分s1秒起飞，在当地时间次日h2时m2分s2秒降落。

第三种格式表示该航班在当地时间h1时m1分s1秒起飞，在当地时间第三日h2时m2分s2秒降落。

#### 输出格式

对于每一组数据输出一行一个时间hh:mm:ss，表示飞行时间为hh小时mm分ss秒。

注意，当时间为一位数时，要补齐前导零，如三小时四分五秒应写为03:04:05。

#### 数据范围

保证输入时间合法（0≤h≤23,0≤m,s≤59），飞行时间不超过24小时。

#### 输入样例：

3
17:48:19 21:57:24
11:05:18 15:14:23
17:21:07 00:31:46 (+1)
23:02:41 16:13:20 (+1)
10:19:19 20:41:24
22:19:04 16:41:09 (+1)

#### 输出样例：

04:09:05
12:10:39
14:22:05

#### 题解：

架构：

1. 解决时差与计算飞行用时的问题。考虑地球自转与飞机飞行，我们可以类比小学奥数的小船过河问题。也就是说，我们已知去程与返程的抵达时间与出发时间的差值s1和s2，由于`飞行时间=目的地时间-出发地时间+时差（可正可负）`而来回的时差一定是互为相反数的，因此我们可以令`(s1+s2)/2`得到我们需要的飞行时间。
2. 本题的难点其实不在上述逻辑，而在于数据的读入与规格化。我们发现读入的数据是有一定格式的，并且有的数据带+1，有的却不带，因此我们需要为不带+的数据进行人为添加（+0）以使得数据规格化。然后要学会用sscanf来对规格化后的数据进行提取，因为时分秒的时间格式是难以进行加减运算的，所以我们需要设置一个基准，即距离当天0点所过去的秒数。利用我们提取的数据，就可以做到非常好的转换。

```c++
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <string>
#include <cstring>

using namespace std;

int get_second(int h, int m, int s)
{
    return h*3600+60*m+s;
}

int get_time()
{
    string line;
    getline(cin, line);
    
    int h1, m1, s1, h2, m2, s2, d;
    if(line.back() != ')') line += " (+0)";
    sscanf(line.c_str(), "%d:%d:%d %d:%d:%d (+%d)", &h1, &m1, &s1, &h2, &m2, &s2, &d);
    return get_second(h2, m2, s2) - get_second(h1, m1, s1) + 24*60*60*d;
}

int main()
{
    int n;
    cin >> n;
    string line;
    getline(cin, line);
    
    while(n--)
    {
        int time = (get_time() + get_time())/2;
        
        int hour, minute, second;
        hour = time/3600;
        minute = time%3600/60;
        second = time%60;
        
        printf("%02d:%02d:%02d\n", hour, minute, second);
    }
    
    return 0;
}
```



## 4.外卖店优先级

“饱了么”外卖系统中维护着 N 家外卖店，编号 1∼N。

每家外卖店都有一个优先级，初始时 (0 时刻) 优先级都为 0。

每经过 1 个时间单位，如果外卖店没有订单，则优先级会减少 1，最低减到 0；而如果外卖店有订单，则优先级不减反加，每有一单优先级加 2。

如果某家外卖店某时刻优先级大于 5，则会被系统加入优先缓存中；如果优先级小于等于 3，则会被清除出优先缓存。

给定 T 时刻以内的 M 条订单信息，请你计算 T 时刻时有多少外卖店在优先缓存中。

#### 输入格式

第一行包含 3 个整数 N,M,T。

以下 M 行每行包含两个整数 ts 和 id，表示 ts 时刻编号 id 的外卖店收到一个订单。

#### 输出格式

输出一个整数代表答案。

#### 数据范围

1≤N,M,T≤105,
1≤ts≤T,
1≤id≤N

#### 输入样例：

2 6 6
1 1
5 2
3 1
6 2
2 1
6 2

#### 输出样例：

1

#### 样例解释

6 时刻时，1 号店优先级降到 3，被移除出优先缓存；2 号店优先级升到 6，加入优先缓存。

所以是有 1 家店 (2 号) 在优先缓存中。

#### 题解：

架构：

1. 暴力做法，按照时间推移，对每家店的优先级进行变化和分析，但这样的话时间复杂度为100亿，超时在所难免，所以我们需要对算法进行优化。
2. 考虑到订单量相比店家量理论是更少的，因此大多数店家在大多数时刻都接收不到新订单，也就是说，大多数店家的优先级减小时刻是紧密相连的，也就是以减小块的形式出现，因此我们可以记下在每个时间点每家店最后接收订单的时刻。由此我们就可以非常简单地处理优先级减小问题，将考虑的对象聚焦在获得订单的那些店上，问题就被简化了。
3. 利用成熟的数据结构进行数据的存储

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 100010;
int score[N], last[N];
bool st[N];

PII order[N];

int main()
{
    int n, m, T;
    cin >> n >> m >> T;
    for(int i = 0; i < m; i++) scanf("%d%d", &order[i].x, &order[i].y);
    sort(order, order+m);
    
    for(int i = 0; i < m;)
    {
        int j = i;
        while(j < m && order[i] == order[j]) j++;
        int t = order[i].x, id = order[i].y, cnt = j-i;
        i = j;
        
        score[id] -= t - last[id] - 1;
        if(score[id] < 0) score[id] = 0;
        if(score[id] <= 3) st[id] = false;
        
        score[id] += cnt*2;
        if(score[id] > 5) st[id] = true;
        last[id] = t;
    }
    
    for(int i = 1; i <= n; i++)
    {
        if(last[i] < T)
        {
            score[i] -= T-last[i];
            if(score[i] <= 3) st[i] = false;
        }
    }
    
    int res = 0;
    for(int i = 1; i <= n; i++)
    {
        res += st[i];
    }
    
    cout << res << endl;
    return 0;
}
```
