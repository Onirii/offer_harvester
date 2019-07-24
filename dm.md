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
