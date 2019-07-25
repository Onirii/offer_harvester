# 1. 

# 2. 单例模式
单例模式（Singleton Pattern）属于创建型模式，它提供了一种创建对象的最佳方式。这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。
## 2.1 什么是饿汉式和懒汉式？
1. 饿汉式：即类产生的时候就创建好实例对象，这是一种空间换时间的方式。饿汉式的对象在类产生的时候就创建了，一直到程序结束才释放。即对象的生存周期和程序一样长，因此该实例对象需要存储在内存的全局数据区，故使用static修饰。
2. 懒汉式：即在需要的时候，才创建对象，这是一种时间换空间的方式。

## 2.2 实现单例模式
### 2.2.1 饿汉式
```cpp
//直接定义静态对象
//.h文件
class Singleton
{
public:
  static Singleton& GetInstance();
private:
  Singleton(){};
  Singleton(const Singleton&);
  Singleton& operator= (const Singleton&);
private:
  static Singleton m_Instance;
};

//CPP文件
Singleton Singleton::m_Instance;//类外定义
Singleton& Singleton::GetInstance()
{
   return m_Instance;
}
//函数调用
Singleton& instance = Singleton::GetInstance();
```
1. 优点：实现简单，多线程安全。
2. 缺点：
	1. 如果存在多个单例对象且这几个单例对象相互依赖，可能会出现程序崩溃的危险。原因:对编译器来说，静态成员变量的初始化顺序和析构顺序是一个未定义的行为。
	2. 在程序开始时，就创建类的实例，如果Singleton对象产生很昂贵，而本身有很少使用，这种方式单从资源利用效率的角度来讲，比懒汉式单例类稍差些。但从反应时间角度来讲，则比懒汉式单例类稍好些。
3. 使用条件：
	1. 当肯定不会有构造和析构依赖关系的情况。
	2. 想避免频繁加锁时的性能消耗。


```cpp
//静态指针 + 类外初始化时new空间实现
class Singleton
{
protected:
	Singleton(){}
private:
	Singleton(const Singleton& other);
	Singleton& operator=(const Singleton&);
	static Singleton* m_Instance;
public:
	static Singleton* GetInstance();
};

Singleton* Singleton::m_Instance = new Singleton;
Singleton* Singleton::GetInstance()
{
	return m_Instance;
}
```

### 2.2.2 懒汉式
```cpp
class Singleton  
{  
public:  
static Singleton* GetInstance()  
{  
     if (m_Instance == nullptr)    
         m_Instance = new Singleton();  
     return m_Instance;  
}  
private:  
    Singleton();
    Singleton(const Singleton& other);
    Singleton& operator=(const Singleton&);
    static Singleton *m_Instance;  
};
```
懒汉式的Singleton::GetInstance()使用懒惰初始化，也就是说其返回值是当这个函数首次被访问时才创建的。所有GetInstance()之后的调用都返回相同实例的指针。

### 2.2.3 多线程下的单例模式
```cpp
//单检查锁
class Singleton
{
private:
	static Singleton* m_Instance;
	Singleton(){}
	Singleton(const Singleton& other);
	Singleton& operator=(const Singleton&);
public:
	static Singleton* GetInstance(){
		Lock lock; //lock为局部变量，其生存周期为定义至函数结束，故对函数剩余部分加锁。
		if(m_Instance == nullptr){ //检查
			m_Instance = new Singleton();
		}
		return m_Instance;
	}
}
//单检查锁的代价过高： m_Instance创建之后，线程对m_Instance的读操作不需要加锁。在高并发情况下，多个线程的m_Instance的读操作仍然加锁的话会造成资源浪费。
```

```cpp
//双检查锁
class Singleton
{
private:
	static Singleton* m_Instance;
	Singleton(){}
	Singleton(const Singleton& other);
	Singleton& operator=(const Singleton&);
public:
	static Singleton* GetInstance(){
		if(m_Instance == nullptr){ //第一次检查
			Lock lock; //加锁
			if(m_Instance == nullptr){ //第二次检查
				m_Instance = new Singleton();
			}
		}
		return m_Instance;
	}
}

//双检查锁两次检查的意义：
//第一次检查：第一次检查负责检查是不是对m_Instance的读操作，如果m_Instance此时已经被创建(非nullptr)，则多线程只读m_Instance不需要加锁。
//第二次检查：如果没有第二次检查，则在m_Instance未完全创建时，凡是通过第一次检查的线程均能获得lock，可能有多个m_Instance被创建。
```


```cpp
//双检查锁的改进(内存读写reorder不安全)
//以m_Instance = new Singleton(); 为例：定义m_Instance

class Singleton
{
private:
	Singleton* m_Instance;
	Singleton(){}
	Singleton(const Singleton& other);
	Singleton& operator=(const Singleton&);
public:
	std::atomic<Singleton*> Singleton::m_Instance;
	std::mutex Singleton::m_mutex;

	Singleton* GetInstance(){
		Singleton* temp = m_Instance.load(std::memory_order_relaxed);
		std::atomic_thread_fence(std::memory_order_acquire);
		if(temp == nullptr){
			std::lock_guard<std::mutex> lock(m_mutex);
			temp = m_Instance.load(std::memory_order_relaxed);
			if(temp == nullptr){
				temp = new Singleton();
				std::atomic_thread_fence(std::memor_order_release);
				m_Instance.store(temp, std::memory_order_relaxed);
			}
		}
		return temp;
	}
}
```