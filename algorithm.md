# 1.排序
## 1.1 冒泡排序
### 1.1.1 冒泡排序的基本思想
重复进行相邻数组元素的两两比较，并按照规则进行交换，直到没有元素再需要交换，最终使得大的元素逐渐沉到数列底部，相对较小的元素浮现到数列顶部。
### 1.1.2 冒泡排序的具体步骤
1. 比较相邻的两个元素，如果第一个比第二个大，就交换位置。
2. 从第一步开始，对数组中的每一对相邻元素重复步骤1，使得最大的元素沉到数组底部。
3. 对数组中除了底部已经排序好的元素重复步骤2。(每趟排序数组的有序区都会增加一个元素)
4. 重复上述步骤直至排序完成。

### 1.1.3 冒泡排序的时间复杂度
1. **最好(数组正序排列)**：T(n)=O(n); 
2. **最差(数组逆序排列)**：T(n)=O(n^2);
3. 平均情况：T(n)=O(n^2)。

### 1.1.4 冒泡排序cpp实现
```cpp
#include<iostream>
using namespace std;

void swap(int &a, int &b) {//交换函数
    int tmp = a;
    a = b;
    b = tmp;
}

void bubbleSort(int a[],int len) {
    for (int i = 0; i < len; ++i) {//排列len趟完成排序
        for (int j = 0; j < len - i - 1; ++j) {//每趟排序的元素都从第一个元素开始到尾部没有排序好的元素。第一趟为第一个元素到最后一个元素，排出了最大元素。第二趟为第一个元素到倒数第二个元素(排除以排序好的元素)，排出第二大的元素。
            if (a[j] > a[j + 1]){
                swap(a[j], a[j + 1]);
            }
        }
    }
}

int main() {
    int a[] = { 520,0,1,9,56,100,1,85,5,3,6 };
    int len = sizeof(a) / sizeof(a[0]);
    bubbleSort(a, len);
    for (int i = 0; i < len; ++i) {
        cout << " " << a[i];
    }
    return 0;
}
```

### 1.1.5 冒泡排序的改进
1. 增加了**反向冒泡**。传统冒泡排序每一趟排序一次，找出一个最大值。改进后每一趟排序两次,正向冒泡，找出最大的元素；反向冒泡，找出最小元素，使得一次可以得到两个最终值。从而减少排序趟数。 
2. **增加了标志点flag**。目的在于记录每一趟最后一次交换的元素位置，即表示标志点之后或者之前的位置已经排好，后续无需再排，缩小排序区间，减少排序次数。

3. cpp实现

```cpp
#include<iostream>

using namespace std;

void swap(int &a, int &b) {
    int tmp = a;
    a = b;
    b = tmp;
}

void bubbleSort(int a[],int len) {
    int low = 0, high = len - 1;
    while (low < high) {//排序趟数。排序区间[low,high]。
        int flag = 0;//标志点
        for (int i = low; i < high; ++i) {//第1次正向冒泡
            if (a[i] > a[i + 1]) {
                swap(a[i], a[i + 1]);
                flag = i;
            }
        }
        high = flag;//表示标志点之后的元素已经排好，区间右值缩小。
        for (int i = high; i > low; --i) {//第2次反向冒泡
            if (a[i] < a[i - 1]) {
                swap(a[i], a[i - 1]);
                flag = i;
            }
        }
        low = flag;//表示断点之前的元素已经排好，区间左值缩小。  
    }
}

int main() {
    int a[] = {520,0,1,9,56,100,1,85,5,3,6};
    int len = sizeof(a) / sizeof(a[0]);
    bubbleSort(a, len);
    for (int i = 0; i < len; ++i) {
        cout << " " << a[i];
    }
    return 0;
}
```

## 1.2 选择排序
### 1.2.1 选择排序的基本思想
将数组分为已排序和待排序两个区间，即有序区和无序区。每次从无序区中选出最小/最大的元素与无序区的第一个元素(即有序区尾部)交换位置，直到无序区只剩下一个元素，排序完成。

### 1.2.2 选择排序的具体步骤
1. 将数组划分为有序区和无序区。(初始状态有序区为空，无序区为[0,n-1])
2. 在无序区中找出最大的那个元素，与该区间第一个元素交换。(有序区元素个数加1，无序区减1)
3. 重复步骤2 (n-1)次，排序完成。(排序趟数为n-1，最后一个元素无序排序)

### 1.2.3 选择排序的时间复杂度
选择排序时间复杂度最稳定，无论什么时候都为：T(n) = O(n^2)。

### 1.2.4 选择排序的cpp实现
```cpp
#include<iostream>

using namespace std;

void swap(int &a, int &b) {
    int tmp = a;
    a = b;
    b = tmp;
}

void selectSort(int a[], int len) {
    for (int i = 0; i < len - 1; ++i) {//选择排序的排序趟数为n-1。
        int minIndex = i;
        for (int j = i; j < len; ++j) {//找出无序区最小元素的下标。
            if (a[j] < a[minIndex]) {
                minIndex = j;
            }
        }
        swap(a[i], a[minIndex]);//无序区第一个元素与最小值交换。
    }
}

int main() {
    int a[] = { 5,89,562,4,2,0,56512,4512,5 };
    int len = sizeof(a) / sizeof(a[0]);
    selectSort(a, len);
    for (int i = 0; i < len; ++i) {
        cout << " " << a[i];
    }
    return 0;
}
```

## 1.3 插入排序
### 1.3.1 插入排序的基本思想
插入排序跟选择排序很像，都分为有序区和无序区。但是选择排序是每次都从无序区中选出最小元素插入到有序区末尾，而插入排序是直接将数组的第一个元素作为有序区的第一个元素，每次都拿出无序区第个一元素插入到有序区合适的位置上，直到无序区为空，排序完成。
### 1.3.2 插入排序的具体步骤
1. 将数组分为有序区和无序区，有序区0,无序区[1,n-1]； 
2. 取下无序区第一个元素，保存其值。 
3. 有序区中元素从后往前与新元素比较，如果新元素更小，旧元素往后移。 
4. 重复步骤3，直到新元素大于或等于旧元素，将新元素插入该元素之后。 
5. 重复步骤234， n-1次，排序完成。

### 1.3.3 插入的时间复杂度
1. **最好(数组元素正序排列)**：T(n)=o(n)，数组元素正序排列 
2. **最坏(数组元素逆序排列)**：T(n)=o(n^2)数组元素反序排列 
3. **平均情况**：T(n)=o(n^2)

### 1.3.4 插入排序的cpp实现
```cpp
#include<iostream>

using namespace std;

void sertSort(int a[], int len) {
    for (int i = 1; i < len; ++i) {
        int key = a[i];//保存无序区第一个元素为key
        int j = i - 1;
        while (!(j <0) && a[j] > key) {//新元素在有序区寻找位置
            a[j + 1] = a[j]; //移动有序区
            j--;
        }
        a[j+1] = key;
    }
}

int main() {
    int a[] = {5,45,1,3,0,99,2,10 };
    int len = sizeof(a) / sizeof(a[0]);
    sertSort(a, len);
    for (int i = 0; i < len; ++i) {
        cout << " " << a[i];
    }
    return 0;
}
```

### 1.3.5 插入排序的改进
新元素在插入到有序区时，使用二分法查找位置而非一个一个依次查找。
```cpp
#include<iostream>

using namespace std;

void sertSort(int a[], int len) {
    for (int i = 1; i < len; ++i) {
        int key = a[i];
        int left = 0, right = i-1;
        while (!(left > right)) {//在区间内查找位置
            int middle = (left + right) / 2;
            if (a[middle] > key)
                right = middle-1;
            else left = middle+1;
        }
        for (int j = i - 1; !(j<left); --j) {
            a[j+1] = a[j]; //找到插入位置后移动有序区，为待插入元素腾出位置。
        }
        a[left] = key;//left为新元素要插入的位置。
    }
}

int main(){
    int a[] = {5,45,1,3,0,99,2,10 };
    int len = sizeof(a) / sizeof(a[0]);
    sertSort(a, len);
    for (int i = 0; i < len; ++i) {
        cout << " " << a[i];
    }
    return 0;
}
```

## 1.4 希尔排序
### 1.4.1 希尔排序的基本思想
希尔排序是以某个增量h为步长跳跃分组进行插入排序，由于增量是一个从h逐渐缩小至1的过程，所以又称缩小增量排序。 
其核心在于间隔序列设定，即增量的设定，这也是也插入排序的本质区别。插入排序始终增量为1。 
**最佳增量:k趟排序增量步长为(2^k)-1,即增量序列(2^k)-1,…15,7,3,1。**

### 1.4.2 希尔排序的具体步骤
1. 确定增量序列(t1,t2…tk)ti>tj,tk=1; 
2. 按增量序列个数k分成k趟排序 
3. 每趟排序按对应增量ti，将序列分割成若干子序列，分别进行直接插入排序。

### 1.4.3 希尔排序的时间复杂度

### 1.4.4 希尔排序的cpp实现
```cpp
#include<iostream>

using namespace std;

void swap(int &a, int &b){
    int tmp = a;
    a = b;
    b = tmp;
}

void shellSort(int a[], int len) {
    int gap = len ;
    while (gap = gap / 2) {//增量
        for (int i = gap; i < len; i++) {
            int key = a[i];//待排序元素
            int j = i - gap;
            for (; j+1>0&&a[j ] > key; j -= gap) {//插入排序
                a[j + gap] = a[j];
            }
            a[j + gap] = key;
        }
    }
}

int main() {
    int a[] = { 10,8,4,3,1,5,7,9,2,6};
    int len = sizeof(a) / sizeof(a[0]);
    shellSort(a, len);
    cout << "排序最终结果：";
    for (int i = 0; i < len; ++i) {
        cout << " " << a[i];
    }
    return 0;
}
```
## 1.5 归并排序
### 1.5.1 归并排序的基本思想
采用递归和分治的思想，首先将数组二分成多个子序列，然后两两子序列进行比较与合并，使其合并为一个完全有序的序列。不断进行比较与合并，使子序列们最终合并为一个完整有序的序列。

### 1.5.2 归并排序的具体步骤
1. 将数组二分为多个子序列 
2. 子序列进行排序(只有1个元素的序列本身就已经是排序好的序列) 
3. 排序好的子序列间进行比较与合并。 
4. 子序列完全合并为一个有序序列

### 1.5.3 归并排序的时间复杂度
1. 最佳：O(n) 
2. 最坏：O(nlogn) 
3. 平均：O(nlogn)

### 1.5.4 归并排序的cpp实现
```cpp
#include<iostream>
using namespace std;

void merge(int a1[], int na1, int a2[], int na2) {
    int tmp[1000];
    int t = 0, i = 0, j = 0,k=0;
    while( i < na1&&j < na2){
        if (a1[i] > a2[j]) {
            tmp[t++] = a2[j++];
        }
        else tmp[t++] = a1[i++];
    }
    while (i < na1) {
        tmp[t++] = a1[i++];
    }
    while (j < na2) {
        tmp[t++] = a2[j++];
    }
    while(k<t){
        a1[k] = tmp[k++];
    }
}
void mergeSort(int a[],int len) {
    if (len>1) {
        int mid = len / 2;
        int *a1 = a;
        int na1 = mid;
        int *a2 = a + mid ;
        int na2 = len - mid;
        mergeSort(a1, na1);
        mergeSort(a2, na2);
        merge(a1, na1, a2, na2);
    }
}

int main() {
    int a[] = {0,7,5,2,1,3,8,4,6,9 };
    int len = sizeof(a) / sizeof(a[0]);
    mergeSort(a,len);
    for (int i = 0; i < len; ++i) {
        cout << " " << a[i];
    }
    return 0;
}
```

## 1.6 快速排序
### 1.6.1 快速排序的基本思想
1. 快速排序的基本思想：通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。
2. 在最坏的情况下，当原序列是逆序的时候，这时快速排序需要进行N轮，时间复杂度为O(n^2)，与冒泡排序相当。为了避免这种情况的发生，可以在排序时随机选择一个元素作为基准元素。
3. 

### 1.6.2 快速排序的具体步骤 (挖坑填数法)
设排序区间的两端点下标分别是L、R。
1. i = L; j = R; 将基准数挖出形成第一个坑a[i]。
2. j--由后向前找比它小的数，找到后挖出此数填前一个坑a[i]中。
3. i++由前向后找比它大的数，找到后也挖出此数填到前一个坑a[j]中。
4. 再重复执行2，3二步，直到i==j，将基准数填入a[i]中。

### 1.6.3 快速排序的时间复杂度

### 1.6.3 挖坑填数法Cpp实现
```cpp
#include<iostream>
using namespace std;

void quickSort(int s[], int l, int r)
{
    if (l < r)
    {
        //Swap(s[l], s[(l + r) / 2]); //将中间的这个数和第一个数交换,避免原序列完全逆序时算法过于复杂。
        int i = l, j = r, x = s[l];
        while (i < j)
        {
            while(i < j && s[j] >= x) // 从右向左找第一个小于x的数
                j--;  
            if(i < j) 
                s[i++] = s[j];
            
            while(i < j && s[i] < x) // 从左向右找第一个大于等于x的数
                i++;  
            if(i < j) 
                s[j--] = s[i];
        }
        s[i] = x;
        quickSort(s, l, i - 1); // 递归调用 
        quickSort(s, i + 1, r);
    }
}
int main() {
    int a[] = {0,4,1,2,3,0,0,0};
    int len = sizeof(a) / sizeof(a[0]);
    quickSort(a, 0, len - 1);
    for (int i = 0; i < len; ++i) {
        cout << " " << a[i];
    }
    return 0;
}
```

```cpp
#include<iostream>
#include<vector>
#include<stack>
#include<cstdlib>
#include<algorithm>
#include<ctime> 

using namespace std;
 
//把数组分为两部分，轴pivot左边的部分都小于轴右边的部分
template <typename Comparable>
int partition(vector<Comparable> &vec,int low,int high){
    Comparable pivot=vec[low];  //任选元素作为轴，这里选首元素
    while(low<high){
        while(low<high && vec[high]>=pivot)
            high--;
        vec[low]=vec[high];
        while(low<high && vec[low]<=pivot)
            low++;
        vec[high]=vec[low];
    }
    //此时low==high
    vec[low]=pivot;
    return low;
}
 
//使用递归快速排序
template<typename Comparable>
void quicksort1(vector<Comparable> &vec,int low,int high){
    if(low<high){
        int mid=partition(vec,low,high);
        quicksort1(vec,low,mid-1);
        quicksort1(vec,mid+1,high);
    }
}


//使用栈的非递归快速排序
template<typename Comparable>
void quicksort2(vector<Comparable> &vec,int low,int high){
    stack<int> st;
    if(low<high){
        int mid=partition(vec,low,high);
        if(low<mid-1){
            st.push(low);
            st.push(mid-1);q
        }
        if(mid+1<high){
            st.push(mid+1);
            st.push(high);
        }
        //其实就是用栈保存每一个待排序子串的首尾元素下标，下一次while循环时取出这个范围，对这段子序列进行partition操作
        while(!st.empty()){
            int q=st.top();
            st.pop();
            int p=st.top();
            st.pop();
            mid=partition(vec,p,q);
            if(p<mid-1){
                st.push(p);
                st.push(mid-1);
            }
            if(mid+1<q){
                st.push(mid+1);
                st.push(q);
            }      
        }
    }
}
 
int main(){
    int len=1000000;
    vector<int> vec;
    for(int i=0;i<len;i++)
        vec.push_back(rand());
    clock_t t1=clock();
    quicksort1(vec,0,len-1);
    clock_t t2=clock();
    cout<<"recurcive  "<<1.0*(t2-t1)/CLOCKS_PER_SEC<<endl;
     
    //重新打乱顺序
    random_shuffle(vec.begin(),vec.end());
         
    t1=clock();
    quicksort2(vec,0,len-1);
    t2=clock();
    cout<<"none recurcive  "<<1.0*(t2-t1)/CLOCKS_PER_SEC<<endl;
     
    return 0;
}
```

## 1.7 堆排序
### 1.7.1 堆排序的基本思想
堆排序是将数组构建成大顶堆，即根节点是数组中最大元素，将根节点与堆底最后一个元素交换，使得最大值排到末尾，即已排序好。将剩下的n-1个元素重新调整为大顶堆，在堆顶/根节点处得到第二大的值，与堆底最后一个元素交换，便又排序好一个元素。

### 1.7.2 堆排序的具体步骤
1. 将数组构建成大顶堆 
2. 交换堆顶元素和堆底元素 
3. 调整堆，使其重新成为大顶堆 
4. 重复步骤2和步骤3 n-1次，排序完成。n为数组长度。

### 1.7.3 堆排序的时间复杂度
最好、最坏，平均都为o(nlogn)

### 1.7.4 堆排序的cpp实现
```cpp
#include<iostream>

using namespace std;

void swap(int &a, int &b) {
    int tmp = a;
    a = b;
    b = tmp;
}

void heap(int a[],int node,int len){
    int nodecopy = a[node];
    for (int i = 2 * node + 1; i < len; i = 2 * i + 1) {//遍历node所有下属节点
        if(i + 1 < len&&a[i + 1] > a[i])//如果右子树节点存在且右子树节点大于左子树，那么将下标移到右子树位置
            i++;
        if (a[i] > a[node]) {
            a[node] = a[i]; //大节点上浮
            node = i;
        }
    }
    a[node] = nodecopy; //确定a[node]最终位置
}

void heapSort(int a[],int len) {
    for (int node = len / 2 - 1; node >= 0; --node) {
        heap(a, node, len);
    }
    for (int i = len-1;i>0; --i) {
        swap(a[0], a[i]);
        heap(a, 0, i);
    }   
}

int main() {
    int a[] = {7,0,1,2,8,5,9};
    int len = sizeof(a) / sizeof(a[0]);
    heapSort(a, len);
    for (int i = 0; i < len; ++i) {
        cout << " " << a[i];
    }
    return 0;
}
```

## 1.8 计数排序
### 1.8.1 计数排序的基本思想
非比较排序。计数排序是利用哈希原理，记录元素出现的频次。在统计结束后可以直接遍历哈希表，将数据填回数据空间。由于是空间换时间，所以适合对数据范围集中的数据使用。而且由于用数组下标表示，只适合只有正整数，0的数据。

### 1.8.2 计数排序的具体步骤
1. 统计数组中元素出现的频次并找出极值。 
2. 填充原数组。

### 1.8.3 计数排序的时间复杂度
T(n)=O(n+k)  k为范围，即新开辟的数组大小

### 1.8.4 计数排序的cpp实现
```cpp
#include<iostream>

using namespace std;

void countSort(int a[], int len) {
    int low = a[0], high = a[0];
    for (int i = 0; i < len; ++i) {//找范围
        low = a[i] < low ? a[i] : low;
        high = a[i] > high ? a[i] : high;
    }
    int range = high - low+1;
    int *count = new int[range];
    memset(count, 0, sizeof(int)*(range));
    for(int i=0;i<len;++i){//统计频次
        count[a[i]-low]++;
    }
    for (int i = 0,j=0; i < range; ++i) {//赋值
        while (count[i]--) {
            a[j++] = i+low;
        }
    }
}

int main() {
    int a[] = { 2, 2, 3, 8, 7,0,4};
    int len = sizeof(a) / sizeof(a[0]);
    countSort(a, len);
    for (int i = 0; i < len; ++i) {
        cout << " " << a[i];
    }
    return 0;
}
```

## 1.9 基数排序
### 1.2.1 基数排序的基本思想
非选择排序。将元素依次按低位到高位进行分类排序(个位，十位，百位…)与收集。即分别排序，分别收集。 (适用范围，最好小于1000；数字为0或正整数。)

### 1.2.2 基数排序的具体步骤
1. 取得数组中最大位数。 
2. 从低位开始，对数组进行排序。 
3. 将排序好的元素复制到原始数组。 
4. 重复2、3步骤到最高位。排序完成。

### 1.2.3 基数排序的时间复杂度
最好、最差、平均均为O(n\*k)  (k为位数)

### 1.2.4 基数排序的cpp实现
```cpp
#include<iostream>
#include<vector>

using namespace std;

int getnum(int a[], int len) {

    int max = a[0];
    int num = 1;
    for (int i = 1; i < len; ++i) {
        max=a[i] > max ? a[i] : max;
    }
    while (max /= 10) {
        num++;
    }
    return num;
}

void radixSort(int a[], int len) {
    int num = getnum(a, len);//获得位数
    vector<vector<int>>radix(10);
    for(int k=0;k<num;++k){
        for (int i = 0; i < len; ++i) {//存放元素
            int t = int(a[i] / pow(10, k))%10;
            radix[t].push_back(a[i]);
        }
            vector<vector<int>>::iterator p;
            vector<int>::iterator q;
            int i = 0;
            for (p = radix.begin(); p != radix.end(); ++p) {//取出元素
                for (q = (*p).begin(); q !=(*p).end(); ++q) {
                    a[i++] = *q;
                }
            }
            for (int i = 0; i < 10; ++i) {//清空容器中元素
                if(!radix[i].empty())
                        radix[i].clear();
            }
    }
}

int main() {
    int a[] = { 0,1,89,4,45,41,7,9,0,52,80,100 };
    int len = sizeof(a) / sizeof(a[0]);
    radixSort(a, len);
    for (int i = 0; i < len; ++i) {
        cout << " " << a[i];
    }
    return 0;
}
```

## 1.10 桶排序
### 1.2.1 桶排序的基本思想

### 1.2.2 桶排序的具体步骤
### 1.2.3 桶排序的时间复杂度
### 1.2.4 桶排序的cpp实现

# 2. 哈希表
## 2.1 

## 2.2 哈希函数的构造方法
1. **直接定址法**
	1. 哈希函数为一次函数， H(key)=key 或者 H(key) = a * key + b
2. **数字分析法**
	1. 如果关键字由多位字符或者数字组成，就可以考虑抽取其中的 2 位或者多位作为该关键字对应的哈希地址，在取法上尽量选择变化较多的位，避免冲突发生。
3. **平方取中法**
	1. 平方取中法是对关键字做平方操作，取中间得几位作为哈希地址。
4. **折叠法**
	1. 折叠法是将关键字分割成位数相同的几部分（最后一部分的位数可以不同），然后取这几部分的叠加和（舍去进位）作为哈希地址。此方法适合关键字位数较多的情况。
5. **除留余数法**
	1. 若已知整个哈希表的最大长度m，可以取一个不大于m的数p，然后对该关键字key做取余运算，即：H(key) = key % p。
	2. 在此方法中，对于p的取值非常重要，由经验得知p可以为不大于m的质数或者不包含小于20的质因数的合数。
6. **随机数法**
	1. 是取关键字的一个随机函数值作为它的哈希地址，即：H(key)=random(key)，此方法适用于关键字长度不等的情况。
	2. 这里的随机函数其实是伪随机函数，随机函数是即使每次给定的key相同，但是H(key)都是不同；而伪随机函数正好相反，每个key都对应的是固定的H(key)。


## 2.2 哈希冲突的处理方法

1. 开放定址法
	1. H(key)=(H(key) + d) % m（其中m为哈希表的表长，d为一个增量）
	2. 当得出的哈希地址产生冲突时，选取以下3种方法中的一种获取d的值，然后继续计算，直到计算出的哈希地址不在冲突为止:
		1. 线性探测法：d=1，2，3，…，m-1
		2. 平方探测法：d=1^2，-1^2，2^2，-2^2，3^2......
		3. 伪随机数探测法：d=伪随机数
	3. 在线性探测法中，当遇到冲突时，从发生冲突位置起，每次+1，向右探测，直到有空闲的位置为止；二次探测法中，从发生冲突的位置起，按照 +1^2，-1^2，+2^2，…如此探测，直到有空闲的位置；伪随机探测，每次加上一个随机数，直到探测到空闲位置结束。
	4. 开放定址法的缺点：元素不能删除、当哈希表中元素越来越满时，效率明显下降。

2. 再哈希法
	1. 准备若干个哈希函数，当通过当前哈希函数求得的哈希地址同其他关键字产生冲突时，使用下一个哈希函数计算，直到冲突不再发生。

3. 链地址法(哈希桶法/拉链法)
	1. 将所有关键字为同义词的结点链接在同一个单链表中。
	2. 在每个位桶实现的时候，采用链表(链表+红黑树)的数据结构来去存取发生哈希冲突的输入域的关键字（也就是被哈希函数映射到同一个位桶上的关键字）。

## 2.3 哈希表的查找和删除
### 2.3.1 哈希表的查找
查找过程和造表过程一致，假设采用开放定址法处理冲突，则查找过程为： 
1. 对于给定的key，计算hash地址index = H(key)； 
2. 如果数组arr[index]的值为空则查找不成功； 
3. 如果数组arr[index] == key则查找成功；
4. 否则使用冲突解决方法求下一个地址，直到arr[index] == key 或者 arr[index] == null。

### 2.3.2 哈希表的查找效率
决定哈希表查找效率的因素有：
1. 选用的hash函数
2. 选用的处理冲突的方法
3. hash表的饱和度，装载因子α=n/m(n表示实际装载数据长度，m为表长)

### 2.3.3 哈希表删除
1. 链地址法可以直接删除元素
2. 开放地址法不可以直接删除元素，直接删除元素会导致建立表时与此元素冲突的元素再无法被查找到，所以使用开发地址法解决哈希冲突的哈希表在删除元素后应该在删除位置重置一个原来不存在的元素。


# 3
## 3.1 中缀表达式转后缀表达式
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <map>
#include <stack>
#include <algorithm>
using namespace std;

vector<string> infix2suffix(vector<string> record){

	vector<string> oper{"(", "+", "-", "*", "/" , ")"};

	map<string, int> priority;
	priority["("] = 1;
	priority["+"] = 2;
	priority["-"] = 2;
	priority["*"] = 3;
	priority["/"] = 3;

	vector<string> result;
	stack<string> opeaStack;
	for (int i = 0; i<record.size(); ++i){
		vector<string>::iterator iter = find(oper.begin(), oper.end(), record[i]);
		if (iter == oper.end()) result.push_back(record[i]);
		else if (opeaStack.empty() || record[i] == "(") opeaStack.push(record[i]);
		else if (record[i] == ")"){
			while (!opeaStack.empty() && opeaStack.top() != "("){
				result.push_back(opeaStack.top());
				opeaStack.pop();
			}
			opeaStack.pop();
		}
		else{
			while (!opeaStack.empty() && priority[opeaStack.top()] >= priority[record[i]]){
				result.push_back(opeaStack.top());
				opeaStack.pop();
			}
			opeaStack.push(record[i]);
		}
	}
	if(!opreStack.empty()) 
	while (!opeaStack.empty()){
		result.push_back(opeaStack.top());
		opeaStack.pop();
	}
	return result;
}
```

# 4. 重写string类
```cpp
//MyString.h
#ifndef  MYSTRING_H_
#define  MYSTRING_H_
#include <stddef.h>
#include <iostream>
 
class MyString
{
public:
	MyString(const char* str = NULL);
	MyString(const MyString & another);
	MyString & operator = (const MyString & another);
	MyString operator + (const MyString & another);
 
	bool operator > (const MyString & another);
	bool operator < (const MyString & another);
	bool operator == (const MyString & another);
 
	char& operator[](int idx);
	
	void dis();
 
	~MyString(void);
private:
	char* m_data;
};
 
#endif
```

```cpp
//MyString.cpp

#include "MyString.h"
 
// 默认构造器
MyString::MyString(const char* str)
{
	if (NULL == str){
		m_data = new char[1];
		*m_data = '\0';
	}
	else{
		int len = strlen(str);
		m_data = new char[len + 1];
		strcpy_s(m_data, len + 1, str);
	}
}

// 拷贝构造器
MyString::MyString(const MyString & another)
{
	int len = strlen(another.m_data);
	m_data = new char[len + 1];
	strcpy_s(m_data, len + 1, another.m_data);
}

// 析构函数
MyString::~MyString()
{
    delete []m_data;
    m_data = NULL;
}

// 赋值运算符重载
MyString & MyString::operator = (const MyString & another)
{
	// 自赋值，出现错误
	if (this == &another){
		return *this;
	}
	// 先删除自己开辟的空间
	delete []m_data;
	m_data = NULL;

	//分配新的内存资源，复制内容
	int len = strlen(another.m_data);
	this->m_data = new char[len + 1];
	strcpy_s(m_data, len + 1, another.m_data);
	//返回对本对象的引用
	return *this;
}
// 加法运算符重载
MyString MyString::operator + (const MyString & another)
{
	int len = strlen(m_data) + strlen(another.m_data);
	//创建拼接结果，释放其原有内存
	MyString str;
	delete []str.m_data;
	str.m_data = NULL;

	
	str.m_data = new char[len +  1];
	memset(str.m_data, 0, len + 1);
	int len1 = strlen(this->m_data) + 1;  
	strcat_s(str.m_data, len1, this->m_data);
	// 源串长度 + 目标串长度 + 结束符
	int len2 = strlen(this->m_data) + strlen(another.m_data) + 1;
	strcat_s(str.m_data, len2, another.m_data);
	return str;
}
// ==关系运算符重载
bool MyString::operator==(const MyString &other)
{
	if(strcmp(m_data,other.m_data) == 0)
		return true;
	else
		return false;
}
// >关系运算符重载
bool MyString::operator>(const MyString &other)
{
	if(strcmp(m_data, other.m_data) > 0)
		return true;
	else
		return false;
}
// <运算符重载
bool MyString::operator<(const MyString &other)
{
	if(strcmp(m_data,other.m_data) < 0)
		return true;
	else
		return false;
}
// []运算符重载
char& MyString::operator[](int idx)
{
	return m_data[idx];
}
// 打印函数
void MyString::dis()
{
	using namespace std;
	for (size_t i = 0; i < strlen(this->m_data); ++i)
	{
		cout << _str[i];
	}
	cout << endl;
}
```
