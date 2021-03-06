# 1. 
## 1. std::lower_bound(array.begin(), array.end(), val); 返回迭代器指向第一个不小于val的值  (最长上升子序列)
## 2. 最长回文子串 马拉车算法
## 3. 网易2019_001 牛牛找工作
1. std::upper_bound(array.begin(), array.end(), val); 返回一个迭代器指向第一个大于val的值
2. std::upper_bound(array.begin(), array.end(), val); 返回一个迭代器指向第一个大于val的值
```cpp
//返回下标
upper_bound(array.begin(), array.end(), val) - array.begin();
```
2. ```cpp
    ios::sync_with_stdio(0);
	cin.tie(0);
```
std::cin，std::cout之所以效率低，是因为先把要输出的东西存入缓冲区，再输出，导致效率降低，而上面这段语句可以来取消iostream的输入 输出缓存，可以节省许多时间，使效率与scanf与printf相差无几。
3. 

## 4. 网易2016 路灯
	1. while(cin >> P1 >> P2){}
	2. 浮点数的输出 **#include <iomanip>**
		1. setprecision(n) 控制输出流显示浮点数的数字个数，小数部分末尾为0的部分不输出。
		```cpp
		cout<<setprecision(n)<<result<<endl;
		```
		2. showpoint()，在输出语句前声明：cout.setf(ios::showpoint); 控制输出流显示浮点数的数字个数，不够的位用0补充。
		```cpp
		cout<<setf(ios::showpoint);
		cout<<setprecision(n)<<result<<endl;
		//也可以写成下面的形式
		cout<<showpoint<<setprecision(n)<<result<<endl;
		```
		3. fixed, 在输出语句前声明： cout.setf(ios::fixed); 控制输出的小数部分为n位。
		```cpp
		cout<<setf(ios::fixed);
		cout<<setpresicison(n)<<result<<endl;
		//也可以写成下面的形式
		cout<<fixed<<setprecision(n)<<result<<endl;
		//就算后面的cout语句没有声明<<fixed，仍然会做<<fixed处理。
		```

## 5. 拼多多2020 题目1
	1. 整型数组的输入
```cpp
#include <iostream>
#include <sstream>

void split(const string &src, vector<int>& dest, const char& delimiter) {
    dest.clear();
    istringstream iss(src);
    string tmp; 
    while (getline(iss, tmp, delimiter)) {
        dest.push_back(stoi(tmp));//记得转化为int
    }
}
```

## 6. 拼多多2020 题目3
**使用优先队列**
### 6.1 基本优先队列
```cpp
//优先队列的定义
priority_queue<Type, Container, Functional>
//Type 就是数据类型，Container 就是容器类型（Container必须是用数组实现的容器，比如vector,deque等等，但不能用 list。STL里面默认用的是vector），Functional 就是比较的方式。
当需要用自定义的数据类型时才需要传入这三个参数，使用基本数据类型时，只需要传入数据类型，默认是大顶堆。
以vector<int>为例
priority_queue<int, vector<int>, greater<int> > q; //升序队列
priority_queue<int, vector<int>, less<int> > q; //降序队列
```

```cpp

```

### 6.2 使用pair做priority元素
pair的比较，先比较第一个元素，第一个相等比较第二个。
同样Functional为 greater<pair<int, int>>为升序排列， less<pair<int, int>>为降序排列
```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

int main(){
	priority_queue<pair<int, int>, vector<pair<int, int> >, greater<pair<int, int> > > a;
	pair<int, int> b(1, 2);
	pair<int, int> c(1, 3);
	pair<int, int> d(2, 5);
	a.push(d);
	a.push(c);
	a.push(b);
	while (!a.empty()) 
	{
	  cout << a.top().first << ' ' << a.top().second << '\n';
	  a.pop();
	}
	return 0;    #include<iostream>#include<cmath>using namespace std;class ARRAY{private:    int a[5];    int n;public:    ARRAY(int t[], int m);    int num(int n);    void fun();    void print();};ARRAY::ARRAY(int t[], int m){    n = m;    for (int i = 0; i < n; i++)    {        a[i] = t[i];    }}int ARRAY::num(int m){    int temp = m, tt = 0;    int count = 0;    while (temp)    {        tt = temp;        temp /= 10;        count++;    }    return (m - tt * pow(10, count - 1));}void ARRAY::fun(){    for (int i = 0; i < n; i++)    {        for (int j = i + 1; j < n ; j++)        {            if (num(a[i]) > num(a[j]))            {                int temp;                temp = a[i];                a[i] = a[j];                a[j] = temp;            }        }    }}void ARRAY::print(){    for (int i = 0; i < n; i++)    {        cout << a[i] << '\t';    }    cout << endl;}int main(){    int aa[] = { 134,445,423,233,811 };    ARRAY arr(aa, 5);    arr.fun();    arr.print();    system("pause");    return 0;}
}
```

### 6.3 
```cpp
#include <iostream>
#include <queue>
using namespace std;

//方法1
struct tmp1 //运算符重载<
{
    int x;
    tmp1(int a) {x = a;}
    bool operator<(const tmp1& a) const
    {
        return x < a.x; //大顶堆
    }
};

//方法2
struct tmp2 //重写仿函数
{
    bool operator() (tmp1 a, tmp1 b) 
    {
        return a.x < b.x; //大顶堆
    }
};

int main() 
{
    tmp1 a(1);
    tmp1 b(2);
    tmp1 c(3);
    priority_queue<tmp1> d;
    d.push(b);
    d.push(c);
    d.push(a);
    while (!d.empty()) 
    {
        cout << d.top().x << '\n';
        d.pop();
    }
    cout << endl;


    priority_queue<tmp1, vector<tmp1>, tmp2> f;
    f.push(b);
    f.push(c);
    f.push(a);
    while (!f.empty()) 
    {
        cout << f.top().x << '\n';
        f.pop();
    }
}
```

# 7. 中缀表达式转后缀表达式


# 8. 背包问题
## 8.1 01背包问题
1. 题目描述： 有n 个物品，它们有各自的体积w[i]和价值v[i]，现有给定容量的背包，如何让背包里装入的物品具有最大的价值总和？

```cpp
// 动态规划计算最优解
int Number;
int Capacity;
vector<int> volume(Number+1, 0);
vector<int> value(Number+1, 0);
vector<vector<int>> DP(Number+1, vector<int>(Capacity, 0));
vector<int> item(Number+1, 0);

// 动态规划寻找最优解
int FindMax()
{	
    int i, j;
    for(i=1; i<=Number; i++){
        for(j=volume[i]; j<=Capacity; j++){
            if(j < volume[i]){ //第i个物品此时已经装不下。
                DP[i][j] = DP[i-1][j];
            }
            else{ //第i个物品此时可装下	
            	DP[i][j] = max(DP[i-1][j], DP[i-1][j-volume[i]] + value[i]);
            }
        }
    }
    return DP[Number][Capacity];
}

// 最优解的具体组成
void FindWhat(int i, int j)//寻找解的组成方式
{
    if(i >= 0)
    {
        if(DP[i][j] == DP[i-1][j]){ //相等说明没装
            item[i]=0; //全局变量，标记未被选中
            FindWhat(i-1, j);
        }
        else if(j - volume[i] >= 0 && DP[i][j] == DP[i-1][j-volume[i]] + value[i]){ //说明i被装入
            item[i]=1; //标记已被选中
            FindWhat(i-1,j-w[i]); //回到装包之前的位置
        }
    }
}
```

```cpp
// 优化空间复杂度的01问题动态规划解法
int Number;
int Capacity;
vector<int> volume(Number+1, 0);
vector<int> value(Number+1, 0);
vector<int> DP(Number+1, 0);

// 动态规划寻找最优解
int FindMax()
{	
    int i, j;
    for(i=1; i<=Number; i++){
        for(j=Capacity; j<=volume[i]; j--){
            if(j < volume[i]){ //第i个物品此时已经装不下。
                DP[j] = DP[j];
            }
            else{ //第i个物品此时可装下	
            	DP[j] = max(DP[j], DP[j-volume[i]] + value[i]);
            }
        }
    }
    return DP[Number][Capacity];
}
```

## 8.2 完全背包问题
s

## 8.3 多重背包问题


## 8.4 01背包方案数问题


## 8.5 完全背包方案数问题


## 8.6 多重背包方案数问题

