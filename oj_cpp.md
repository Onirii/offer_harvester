# 1. 
## 1. std::lower_bound(array.begin(), array.end(), val); 返回迭代器指向第一个不小于val的值  (最长上升子序列)
## 2. 最长回文子串 马拉车算法
## 3. 网易2019_001 牛牛找工作
1. std::upper_bound(array.begin(), array.end(), val); 返回一个迭代器指向第一个大于val的值
2. std::lower_bound(array.begin(), array.end(), val); 返回一个迭代器指向第一个大于等于val的值
```cpp
//返回下标
upper_bound(array.begin(), array.end(), val) - array.begin();
```
3. ```cpp
    ios::sync_with_stdio(0);
	cin.tie(0);
```
std::cin，std::cout之所以效率低，是因为先把要输出的东西存入缓冲区，再输出，导致效率降低，而上面这段语句可以来取消iostream的输入 输出缓存，可以节省许多时间，使效率与scanf与printf相差无几。
4. 

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

### 6.2 使用pair做priority_queue元素
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
	return 0;
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
