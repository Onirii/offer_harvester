# 1. 
## 1.1
## 1.2 并发与多线程
### 1.2.1 线程运行的开始和结束
1. thread
2. join()
3. detach()
4. joinable()

### 1.2.2 创建线程的其他方式


## 1.3 线程传参详解
### 1.3.1 传递临时参数作为线程参数

### 1.3.1.1 要避免的陷阱
1. 在主线程中往子线程线程函数传递参数，子线程线程函数形参为引用的情况下仍然经历实参复制，为值传递。即使在主线程中detach了子线程，子线程中的类似参数仍然安全。但是仍然不建议使用引用传值，可以直接值传递。
2. 在主线程中往子线程线程函数传递参数，子线程线程函数形参为指针的情况下，当主线程中detach了子线程，类似参数会随着主线程的结束而失效，不安全。
3. 将要传递的参数类型有char\* 修改为 const string&， 子线程函数会引用经过转换的const string，而不是主线程中的char \*。 但是在主线程中detach了子线程的情况下，存在主线程执行结束，char \*已经被回收，子线程初始化依然没有执行，参数没有转化的情况。

**解决方法：** 在子线程构造传参过程中，用string(char\*)直接将参数显示转化，生成临时string对象传递给子线程。

**在创建线程的时候构造临时对象传递参数是可行的。**

4. 如果传递类对象，避免隐式类型转换，全部在创建线程这一行就构建出临时对象来，然后在线程函数中，用引用来传递参数，否则系统还会多构造一次类对象。临时对象在主线程中完成构造与拷贝构造。

5. 建议不使用detach()，只用join()。这样就不会存在**局部变量失效**导致线程对内存的非法引用问题。

### 1.3.1.2 线程ID概念
1. 整型，线程的特有标识
2. std::this_thread::get_id()

### 1.3.1.3 临时对象构造时机抓捕

### 1.3.1.4 传递类对象、智能指针作为线程参数
1. 将类对象作为线程参数，参数形式为引用，传递给线程的类对象经历过一次拷贝构造，并不会像形参声明那样直接引用。因此在子线程中对此类对象的修改不会影响原线程中的对应类对象。
2. 创建线程往线程函数传递类对象参数时，不管线程函数中形参是值传递还是引用传递，为了数据安全考虑，编译器统一按照拷贝方式接收参数。如果确实需要保留子线程中对类对象的修改，可使用**std::ref()**包装按照引用传值的参数，这时编译器会跳过对参数的拷贝，直接使用真正的引用传值。
3. 将类对象最为线程参数，

### 1.3.1.5 用成员函数指针做线程参数
1. std::thread thread(&className::functionName, &objectName, paralist);//第二个参数为&可以保证主线程和子线程是对同一个object进行的操作。

## 1.4. 创建多个线程
### 1.4.1 创建和等待多个线程
1. 多个线程执行顺序是乱的，与操作系统内部对线程的运行调度机制有关。
2. 主线程等待所有子线程运行结束，然后主线程结束。
3. 把多个线程对象放入容器方便进行管理。

### 1.4.2 数据共享问题
1. 共享只读数据是安全稳定的。
2. 共享有读有写的数据，需要特别处理。

### 1.4.3 共享数据的保护

### 1.4.4 std::this_thread

```cpp
std::this_thread::yield(); //当前线程放弃执行，操作系统调度另一线程继续执行。即当前线程将未使用完的“CPU时间片”让给其他线程使用，等其他线程使用完后再与其他线程一起竞争"CPU"。
std::this_thread::get_id(); //获取当前线程的线程ID。
std::this_thread::sleep_for(); //表示当前线程休眠一段时间，休眠期间不与其他线程竞争CPU，根据线程需求，等待若干时间。
std::this_thread::sleep_until() 
//阻塞当前线程直到abs_time时间点到来。
template<class Clock, class Duration>
void sleep_until(const std::chrono::time_point<Clock, Duration>& abs_time);
```

## 1.5. 互斥量及死锁
### 1.5.1 互斥量的基本概念

### 1.5.2 互斥量的用法
1. lock() unlock()

2. std::lock_guard类模板
 	1. 类模板，用做std::mutex的智能管理，当其生命周期结束析构时，unlock其所管理的std::mutex对象。

### 1.5.3 死锁
1. 死锁的一般解决方案
	1. 保证各线程中互斥量上锁的顺序一致。
2. std::lock()
	1. 一次锁住两个或者两个以上的互斥量，可避免多线程中因为加锁顺序引起的死锁。
	2. 如果对多个mutex的加锁过程中有个别没有成功，则std::lock()会释放已经加锁的mutex。
	3. std::lock(mutex1, mutex2, ...);
3. std::lock_guard的std::adopt_lock参数 
	1. std::lock() + std::lock_guard

## 1.6 unique_lock
### 1.6.1 unique_lock取代lock_guard
1. unique_lock 是一个类模板。
2. unique_lock比lock_guard灵活一些，但效率上差一些，内存占用也会多一点。
3. unique_lock可以直接取代lock_guard

### 1.6.2 unique_lock的第二个参数
1. std::adopt_lock 可用作lock_guard或者unique_lock的第二个参数，表示此互斥量已经上锁。表示，调用方线程已经拥有了互斥锁的多有权，unique_lock和lock_guard不需要在构造函数中对mutex加锁。（mutex必须提前加锁）
2. std::try_to_lock 尝试用mutex的lock()去锁定，如果没有锁定成功，立即返回，线程并不会阻塞。判断std::try_to_lock的返回结果，做相应处理。
3. std::defer_lock 初始化一个没有加锁的mutex。

## 1.6.3 unqiue_lock的成员函数
1. lock() 加锁，不用手动unlock()。
2. unlock() 
3. try_lock() 尝试给mutex加锁，如果加锁失败，返回false，如果加锁成功，返回true。
4. release() 返回其所管理的mutex对象指针，并释放所有权，即unique_lock和mutex不再有关系。

## 1.6.4 unique_lock所有权的传递
1. unique_lock 对mutex的所有权可以转移，不可复制。 移动语义(std::move())， 右值


## 1.7 单例设计模式共享数据分析

### 1.7.1 单例模式
整个项目中，有某个或某些特殊的类，属于该类的对象，只能创建1个。

### 1.7.2 单例设计模式共享数据分析

### 1.7.3 std::call_once() C++11
1. 第二个参数为函数，std::call_once()保证参数函数只被调用一次。
2. std::call_once() 通过 std::once_flag 标记来决定是否执行参数中的函数，调用std::call_once()成功后，call_once()就把这个标记设置成一种已调用状态。后续再调用call_once()，只要once_flag被设置成了已调用状态，参数中的函数就不会被调用。 

## 1.8 条件变量
### 1.8.1 std::condition_variable、wait()、notify_one()
1. std::condition_variable实际是一个与条件相关的类，工作过程是等待一个条件的满足。std::condition_variable需要和std::mutex来配合工作，使用时需要生成对象。

2. wait() 
	1. wait(std::mutex) wait()将解锁mutex对象，并堵塞到本行，直到其他某个线程调用notify_one()成员函数为止。
	2. wait(std::mutex对象，可执行对象)如果可执行对象的返回值为false，wait()将解锁mutex对象，并堵塞到本行，知道其他某个线程调用notify_one()成员函数为止。如果可执行对象的返回值为true，wait()将直接返回。

3. notify_one() 唤醒wait()所在的线程
	1. 当其他线程用notify_one()将wait()所在线程唤醒后，wait()所在线程开始执行：
		1. wait()不断尝试获取mutex锁，如果获取不到，那么线程仍然会堵塞在wait()，如果获取到了，wait()继续执行。
		2. wait()重新获取到mutex对象后。如果可执行对象的返回结果为false，那么wait()所在线程继续休眠，等待被再次唤醒；如果可执行对象的返回结果为true，则wait()直接返回，程序继续执行。
		3. 如果wait()没有第二个参数。

### 1.8.2 notify_all()

## 1.9 
### 1.9.1 **std::async、std::future创建后台任务并返回值**
1. std::async是个函数模板，用来启动一个异步任务，启动异步任务之后，返回一个std::future对象。启动一个异步任务就是自动创建一个线程，并开始执行相应的线程入口函数，返回一个std::future对象。
2. std::future对象里含有**线程入口函数的返回结果**(线程的返回结果)，可以通过调用future对象的成员函数get()来获取结果。std::future提供一种访问异步操作结果的机制。
3. 
	1. std::future<Type> result = std::async(线程入口函数);
	2. std::future<Type> result = std::async(&className::functionName, &object, para...); 第二个参数为引用，才能保证子线程与主线程使用同一个类对象。

4. std::future获得std::async创建的线程对象之后，在其get()函数调用处会等待子线程执行完毕，然后获得返回结果。
5. get()(移动语义)只能调用一次，wait()等待线程执行结束，不获取返回值。
6. 通过额外向std::async传递std::launch类型的参数
	1. std::launch::deffered 表示线程入口函数调用被延迟到std::future的wait()或者get()函数调用才执行。 (如果std::future的wait()或者get()函数没有被调用，线程不被创建，不被执行)。实际情况是线程入口函数直接在主线程中被调用，并没有创建新线程。

### 1.9.2 std::packaged_task
1. 类模板，模板参数是各种可调用对象，通过std::packaged_task可以把各种可调用对象包装起来，方便作为线程入口函数。
2. 

```cpp
std::packaged_task<returnType(paralist)> packagedName(functionName); //创建package_task
std::thread threadName(std::ref(packagedName), paralist); //创建thread
threadName.join(); //等待线程执行结束
std::future<returnType> result = packagedName.get_future(); //获取异步线程执行结果
result.get(); //获取异步线程执行结果
```
3. 包装lambda表达式
4. std::packaged_task对象本身就是可调用对象，可以直接调用，这时相当于函数调用。
5. std::packaged_task支持move(std::move())，但不支持拷贝(copy)。

### 1.9.3 std::promise
1. 类模板，在某个线程中可以给std::promise赋值，然后可以在其他线程中可以读取其值。
2. 

```cpp
std::promise<returnType> promiseName;  //创建std::promise
std::thread threadName(functionName, std::ref(promiseName), paralist); //创建线程
threadName.join(); //等待线程执行结束

std::future<returnType> futureName = promise.get_future();
auto result = futureName.get(); //读取promise。
```

3. 子线程之间传递信息用std::promise + std::future
std::promise是一个模板类: template<typename R> class promise。其泛型参数R为std::promise对象保存的值的类型，R可以是void类型。std::promise保存的值可被与之关联的std::future读取，读取操作可以发生在其它线程。std::promise允许move语义(右值构造，右值赋值)，但不允许拷贝(拷贝构造、赋值)，std::future亦然。std::promise和std::future合作共同实现了多线程间通信。
```cpp

```


## 1.10 std::future、std::shared_future、atomic

### 1.10.1 std::future的其他成员函数
1. std::future_status 枚举类型 表示std::future对象调用wait_for()或者wait_until()的返回结果。
	1. std::future_status::ready      //表示线程成功返回，共享数据已经就绪。
	2. std::future_status::timeout    //超时，线程还没有执行结束，数据还没有准备好。
	3. std::future_status::deferred   //如果std::async的第一个参数设置成std::launch::deffered，则此条件成立，线程被延迟执行(直接在主线程中调用子线程的入口函数，并不创建新线程)。

### 1.10.2 std::shared_future
1. std::future的get()函数是一个移动语义，故其只能被调用一次。
2. std::shared_future的get()不是转移数据，是复制数据，所以可以多次调用。

```cpp
int mythread1(int mypara){
	std::chrono::milliseconds duration(5000);
	std::this_thread::sleep_for(duration);
	return mypara;
}

void mythread2(std::shared_future<int> &tmpf){
	auto result = tmpf.get(); //std::shared_future的get()函数是一个复制语义，故可以多次调用。
	return ;
}

int main(){
	std::packaged_task<int(int)> myPT(mythread1); //把线程入口函数通过std::packaged_task包装起来。
	std::thread thread1(std::ref(myPT), 1); //线程开始执行，第二个参数作为线程入口函数的参数。
	thread1.join(); //等待子线程执行结束

	std::future<int> result = myPT.get_future(); //获取异步线程执行结果。
	bool canget = result.vaild(); //Returns whether the future object is currently associated with a shared state.
	// 判断当前std::future对象是否有效。
	// C++中std::future 只有与1. std::async,2. std::promise::get_future, 3. std::packaged_task::get_future这其中一种生产方(Provider)通过共享状态关联后，其vaild()才会有效。

	std::shared_future<int> result_shared(result.shared()); //将std::future转换成std::shared_future，可以实现共享值的多次读取。

	canget = result_shared.vaild();

	auto threadResult = result_shared.get(); 


	std::thread thread2(mythread2, std::ref(result_shared)); //创建新线程，将上个线程的std::shared_future对象作为参数传入，实现线程间的信息传输。
	thread2.join(); //等待子线程执行结束。

	return 0;
}
```

### 1.10.3 原子操作std::atomic
1. 原子操作概念
	1. std::mutex用作多线程编程中保护共享数据：加锁->操作共享数据->解锁。
	2. 多线程共享读取一个全局变量，某个线程对变量的写操作进行过程在汇编层面可能分为多个语句执行，执行过程如果遇到线程轮换，可能产生不可预料的中间结果，非线程安全。
	3. 原子操作是在多线程中不会被打断的程序片段。
	4. std::mutex互斥量加锁一般是针对一个代码段，而std::atomic原子操作一般是针对一个变量。
2. std::atomic
	1. std::atomic<T>
	2. std::atomic_flag

```cpp
#include <iostream>       // std::cout
#include <atomic>         // std::atomic, std::atomic_flag, ATOMIC_FLAG_INIT
#include <thread>         // std::thread, std::this_thread::yield
#include <vector>         // std::vector

std::atomic<bool> ready (false);
std::atomic_flag winner = ATOMIC_FLAG_INIT; //

void count1m (int id) {
  while (!ready) { std::this_thread::yield(); }      // wait for the ready signal
  for (volatile int i=0; i<1000000; ++i) {}          // go!, count to 1 million
  if (!winner.test_and_set()) { std::cout << "thread #" << id << " won!\n"; }
};

int main ()
{
  std::vector<std::thread> threads;
  std::cout << "spawning 10 threads that count to 1 million...\n";
  for (int i=1; i<=10; ++i) threads.push_back(std::thread(count1m,i));
  ready = true;
  for (auto& th : threads) th.join();

  return 0;
}
```

3. std::atomic
	1. 一般的ato::atomic原子操作，针对++、--、+=、-=、&=、|=、^= 是支持的，其他运算操作可能不支持。


## 1.11 std::async 创建异步任务
### 1.11.1 std::async 参数详述
1. std::async用来创建一个异步任务。
2. 如果系统资源紧张，那么std::thread创建新线程有可能失败，会导致程序崩溃。
3. std::async 和 std::thread 最大的区别是std::async有时候并不创建新线程。(std::launch::deffered参数，延迟到std::future对象调用wait()或者get()时，才会直接执行线程入口函数，此时并不创建新线程。)
4. std::launch::async参数强制异步任务要在新线程中执行，这意味着系统必须创建新线程来执行线程入口函数。
5. std::launch::async | std::launch::deffered 同时加入两个参数，结果可能是创建新线程并立即执行，或者没有创建新线程并且延迟调用。
6. (std::launch::async | std::launch::deffered)为默认参数，也就是说系统自行决定对新任务是异步执行还是同步执行。

### 1.11.2 std::async 和 std::thread的区别
1. 如果系统资源紧张，那么std::thread创建新线程有可能失败，这样会导致程序崩溃。
2. 对于std::async的默认参数(std::launch::async | std::launch::deffered)，其可以根据系统资源情况选择对任务是同步执行还是异步执行(是否创建新线程)。
3. std::async可以和std::future结合获得线程的返回值。
4. std::async在默认参数情况下有没有创建新线程？ 使用std::future调用wait_for()函数的返回值std::future_status的具体值判断。


## 1.12
### 1.12.1 Windows临界区

### 1.12.2 自动析构技术
1. RAII
2. std::lock_guard<std::mutex>
3. Windows下的自定义std::lock_guard<std::mutex>

```cpp
//RAII类
class CWINLock
{
public:
	CWINLock(CRITICAL_SECTION *m_pCritemp){
		m_pCritical = m_pCritemp;
		EnterCriticalSection(m_pCritical); //构造函数中进入临界区。
	}
	~CWINLock(){
		LeaveCriticalSection(m_pCritical); //析构函数中离开临界区。
	}
private:
	CRITICAL_SECTION *m_pCritical;
}
```

### 1.12.3 std::recursive_mutex 递归的独占互斥量
1. C++11中不允许同一线程中对同一std::mutex多次上锁，否则会报异常。
2. 在同一线程中，函数之间的调用可能会让同一个std::mutex被lock()多次。
3. std::recursive_mutex 递归的独占式互斥量 允许在同一个线程中，同一个互斥量被多次lock();


### 1.12.4 带超时的互斥量std::timed_mutex和std::recursive_timed_mutex
1. std::timed_mutex
	1. try_lock_for() 等待一段时间，判断是否成功上锁，可以根据返回结果设计程序流程。
	2. try_lock_until() 等待到具体时间点，判断是否成功上锁，可以根据返回结果设计程序流程。
2. std::recursive_timed_mutex

## 1.13 补充知识
### 1.13.1 虚假唤醒
1. 当wait()被唤醒时，没有数据可以操作。
2. 多次调用notify_one();
3. notify_all();
4. wait()的第二个参数可以用来处理虚假唤醒。

### 1.13.2 atomic
1. 原子对象不可拷贝。
2. load() 以原子方式读取std::atomic对象的值
3. store() 以原子方式给std::atomic对象写入值


### 1.13.3 线程池

### 1.13.4 线程创建数量

### 1.13.5 总结

# 2. 多线程实例
## 2.1 
1. 子线程循环10次，接着主线程循环100次，接着又回到子线程循环10次，接着再回到主线程又循环100次，如此循环50次，试写出代码。

```cpp
#include <iostream>
#include <mutex>
#include <condition_variable>


```

2. 编写一个程序，开启3个线程，这3个线程的ID分别为A、B、C，每个线程将自己的ID在屏幕上打印10遍，要求输出结果必须按ABC的顺序显示；如：ABCABC….依次递推。
```cpp

```

3. 有四个线程1、2、3、4。线程1的功能就是输出1，线程2的功能就是输出2，以此类推.........现在有四个文件ABCD。初始都为空。现要让四个文件呈如下格式：
A：1 2 3 4 1 2....

B：2 3 4 1 2 3....

C：3 4 1 2 3 4....

D：4 1 2 3 4 1....

```cpp

```

4. 有一个写者很多读者，多个读者可以同时读文件，但写者在写文件时不允许有读者在读文件，同样有读者读时写者也不能写。
```cpp

```

5. STL中的queue是非线程安全的，一个组合操作：front(); pop()先读取队首元素然后删除队首元素，若是有多个线程执行这个组合操作的话，可能会发生执行序列交替执行，导致一些意想不到的行为，因此请重新设计线程安全的queue的接口。

```cpp
// 
#include <mutex>
#include <condition_variable>
#include <queue>

template<typename T>
class threadsafe_queue
{

private:
    mutable std::mutex mut;
    std::queue<T> data_queue;
    std::condition_variable data_cond;

public:
	//默认构造函数。
    threadsafe_queue() {}
    //赋值构造函数。
    threadsafe_queue(threadsafe_queue const& other)
    {
        std::lock_guard<std::mutex> lk(other.mut);
        data_queue = other.data_queue;
    }
    // 获得mutex后进行写操作
    void push(T new_value)//入队操作  
    {
        std::lock_guard<std::mutex> lk(mut);
        data_queue.push(new_value);
        data_cond.notify_one();
    }
    // 直到有元素可以删除为止，引用传值将pop的元素赋值给非局部变量。
    void wait_and_pop(T& value)
    {

        std::unique_lock<std::mutex> lk(mut);
        //如果队列为空，则wait()继续堵塞，释放mutex。
        //如果队列不为空，则继续执行，队首出队。
        data_cond.wait(lk, [this] {return !data_queue.empty(); });
        value = data_queue.front();
        data_queue.pop();
    }
    // 直到有元素可以删除为止，返回指向被pop()元素的shared_ptr指针。
    std::shared_ptr<T> wait_and_pop()
    {
        std::unique_lock<std::mutex> lk(mut);
        data_cond.wait(lk, [this] {return !data_queue.empty(); });
        std::shared_ptr<T> res(std::make_shared<T>(data_queue.front()));
        data_queue.pop();
        return res;
    }
    // 尝试pop()操作，不成功返回false，pop()成功返回true，并将被pop()元素传递给非局部变量。
    bool try_pop(T& value)//不管有没有队首元素直接返回  
    {
        std::lock_guard<std::mutex> lk(mut);
        if (data_queue.empty())
            return false;
        value = data_queue.front();
        data_queue.pop();
        return true;
    }

    std::shared_ptr<T> try_pop()
    {
        std::lock_guard<std::mutex> lk(mut);
        if (data_queue.empty())
            return std::shared_ptr<T>();
        std::shared_ptr<T> res(std::make_shared<T>(data_queue.front()));
        data_queue.pop();
        return res;
    }
    bool empty() const
    {
        std::lock_guard<std::mutex> lk(mut);
        return data_queue.empty();
    }
};
```

6. 编写程序完成如下功能：
	1. 有一int型全局变量g_Flag初始值为0；
	2. 在主线称中起动线程1，打印“this is thread1”，并将g_Flag设置为1；
	3. 在主线称中启动线程2，打印“this is thread2”，并将g_Flag设置为2；
	4. 线程序1需要在线程2退出后才能退出；
	5. 主线程在检测到g_Flag从1变为2，或者从2变为1的时候退出。

```cpp

```