# 1. 
1. std::lower_bound(array.begin(), array.end(), val); 返回迭代器指向第一个不小于val的值  (最长上升子序列)
2. 最长回文子串 马拉车算法
3. 网易2019_001 牛牛找工作
	1. std::upper_bound(array.begin(), array.end(), val); 返回一个迭代器指向第一个大于val的值
	2. ```cpp
	    ios::sync_with_stdio(0);
    	cin.tie(0);
	```
	std::cin，std::cout之所以效率低，是因为先把要输出的东西存入缓冲区，再输出，导致效率降低，而上面这段语句可以来取消iostream的输入 输出缓存，可以节省许多时间，使效率与scanf与printf相差无几。
	3. 

4. 网易2016 路灯
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