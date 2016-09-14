# C++智能指针

# 引用计数技术及智能指针的简单实现 #

## 基础对象类 ##

	class Point
	
	{
	
	public:
	
	    Point(int xVal = 0, int yVal = 0) : x(xVal), y(yVal) { }
	
	    int getX() const
	    {
	        return x;
	    }
	
	    int getY() const
	    {
	        return y;
	    }
	
	    void setX(int xVal)
	    {
	        x = xVal;
	    }
	
	    void setY(int yVal)
	    {
	        y = yVal;
	    }
	
	
	
	private:
	
	    int x, y;
	
	
	
	};

## 辅助类 ##

	//模板类作为友元时要先有声明
	
	template <typename T>
	
	class SmartPtr;
	
	
	
	template <typename T>
	
	class U_Ptr     //辅助类
	
	{
	
	private:
	
	    //该类成员访问权限全部为private，因为不想让用户直接使用该类
	
	    friend class SmartPtr<T>;      //定义智能指针类为友元，因为智能指针类需要直接操纵辅助类
	
	
	
	    //构造函数的参数为基础对象的指针
	
	    U_Ptr(T *ptr) : p(ptr), count(1) { }
	
	
	
	    //析构函数
	
	    ~U_Ptr()
	    {
	        delete p;
	    }
	
	    //引用计数
	
	    int count;
	
	
	
	    //基础对象指针
	
	    T *p;
	
	};

## 智能指针类 ##

	template <typename T>
	
	class SmartPtr   //智能指针类
	
	{
	
	public:
	
	    SmartPtr(T *ptr) : rp(new U_Ptr<T>(ptr)) { }     //构造函数
	
	    SmartPtr(const SmartPtr<T> &sp) : rp(sp.rp)
	    {
	        ++rp->count;    //复制构造函数
	    }
	
	    SmartPtr &operator=(const SmartPtr<T> &rhs)      //重载赋值操作符
	    {
	
	        ++rhs.rp->count;     //首先将右操作数引用计数加1，
	
	        if (--rp->count == 0)     //然后将引用计数减1，可以应对自赋值
	
	            delete rp;
	
	        rp = rhs.rp;
	
	        return *this;
	
	    }
	
	
	
	    T &operator *()         //重载*操作符
	
	    {
	
	        return *(rp->p);
	
	    }
	
	    T *operator ->()       //重载->操作符
	
	    {
	
	        return rp->p;
	
	    }
	
	
	
	
	
	    ~SmartPtr()          //析构函数
	    {
	
	        if (--rp->count == 0)    //当引用计数减为0时，删除辅助类对象指针，从而删除基础对象
	
	            delete rp;
	
	        else
	
	            cout << "还有" << rp->count << "个指针指向基础对象" << endl;
	
	    }
	
	private:
	
	    U_Ptr<T> *rp;  //辅助类对象指针
	
	};

使用测试

	int main()
	
	{
	
	    int *i = new int(2);
	
	    {
	
	        SmartPtr<int> ptr1(i);
	
	        {
	
	            SmartPtr<int> ptr2(ptr1);
	
	            {
	
	                SmartPtr<int> ptr3 = ptr2;
	
	
	
	                cout << *ptr1 << endl;
	
	                *ptr1 = 20;
	
	                cout << *ptr2 << endl;
	
	
	
	            }
	
	        }
	
	    }
	
	    system("pause");
	
	    return 0;
	
	}

参考：

- [C++ 引用计数技术及智能指针的简单实现](http://mp.weixin.qq.com/s?__biz=MzAxNDI5NzEzNg==&mid=2651156666&idx=1&sn=4109903d526468f5913922d2f06e5a1a&scene=0#wechat_redirect)(文章写的太好了，转过了以后用)