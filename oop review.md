# C++面向对象编程（学长笔记基础之上）【杂碎知识点版】

## C++四大内存分区
- 程序运行前：
  - 代码区：存放程序执行的代码。
    - 全局区：存放全局变量、静态变量和常量。
- 程序运行后：
    - 栈区:存放局部变量和函数参数，生命周期受函数调用控制。
    - 堆区:存放程序通过 `new` 或类似操作动态分配的内存。

## new操作符
- 语法：指针数据类型 指针 = new 数据类型;
- 因为new的作用是动态分配内存空间，所以返回的是地址
    ```cpp
    int* a = new int(10);  // 将值初始化为10
    delete a;

    int* arr = new int[10];  // 在堆区为这些元素分配连续的内存空间
    delete[] arr;
    ```

## 引用
- 作用：给变量起别名（本质上是一个指针常量）
- 语法：数据类型 &别名 = 原名
- 注意：
    - 引用必须初始化
    - 引用在初始化后不可以改变，它只会始终引用初始化赋予的变量
    - 函数传参的时候可以用引用代替指针，且语法更清楚简单
    - 引用可以作为函数返回值，但不要返回局部变量的引用，可以返回全局变量、静态变量或通过动态内存分配（如 new）创建的对象的引用
    - 引用作为左值可以像普通变量一样被赋值。

## 函数提高
- 函数默认参数：
    - 语法：返回值类型 函数名(参数类型 参数名 = 默认值, ...);
    - 默认参数规则:
      - （从左到右的规则）如果一个参数有默认值，那么从这个参数开始，右侧所有的参数都必须有默认值。
      - （函数声明和定义中的默认值）如果在函数`声明`中给出了默认值，那么在函数`定义`中不能再给出默认值，否则会导致编译错误。
      - 传参时按顺序赋值，有默认值的话代替默认值，没传入的话使用默认值
    - 函数占位参数：
      - 语法：返回值类型 函数名 (数据类型){}
        ```cpp
        //函数占位参数 ，占位参数也可以有默认参数
        void func(int a, int) 
        {
	        cout << "this is func" << endl;
        }    

        int main() 
        {
    	    func(10,10); //占位参数必须填补
            system("pause");
            return 0;
        }
        ```
    - 函数重载：
      - 条件：
        - 同一个作用域下（所有重载函数必须在同一个上下文中定义）
        - 函数名称相同
        - 函数参数 `类型不同` 或者 `个数不同` 或者 `顺序不同`
        - 返回值不可以作为函数重载的条件
        - 引用作为重载条件（`int&` `const int&` 可以重载）
        - 如果函数有默认参数，可能会导致重载冲突，需要避免

---

## C++面向对象
- 类是 `对象` 的模板，类里面包含的是 `成员`
- 三大特性：
    - 封装
    - 继承
    - 多态

## 封装
- `封装` 指的是将数据（属性）和操作数据的方法（行为）包装在一起，形成一个独立的模块（即类）。这样可以避免直接访问或修改数据，从而提升程序的安全性和可维护性。
  - 意义：
    - 在设计类时，把属性和行为写在一起，表现生活中的事物。
    - 类在设计时，可以把属性和行为放在不同的权限下，加以控制，避免外部随意修改内部数据。
      - public     公共权限  类内可，类外可
      - protected  保护权限  类内可，类外不可（适用于需要允许继承类访问的情况）
      - private    私有权限  类内可，类外不可
      ```cpp
      // 语法：class 类名{ 访问权限： 属性 / 行为 };
        class Circle
        {    
        public:  //访问权限  公共的权限
	        //属性
	        int m_r;//半径
	        //行为
	        //获取到圆的周长
	        double calculateZC()
	        {
	        	return  2 * PI * m_r;
	        }
        };

        int main() 
        {
	        // 通过圆类，创建圆的对象
	        // c1就是一个具体的圆
	        Circle c1;
	        c1.m_r = 10; // 给圆对象的半径 进行赋值操作

	        system("pause");
	        return 0;
        }
      ```
  - `struct` vs `class` 
    - 唯一区别：默认的访问权限不同
      - struct 默认权限为公共
      - class 默认权限为私有
  - 构造函数与析构函数：
    - C++编译器默认提供三个函数：
      - 默认构造函数(无参，函数体为空)
      - 默认析构函数(无参，函数体为空)
      - 默认拷贝构造函数，对属性进行值拷贝
    - 构造函数：
      - 作用：在于创建对象时为对象的成员属性赋值，构造函数由编译器自动调用，无须手动调用。
      - 语法：类名(){}
      - 用法：
        - 没有返回值（即使是 void 也不写）
        - 支持重载，可以有参数
        - 只调用一次
        ```cpp
        // 构造函数分类:
        // 按照参数分类分为：1.有参和无参构造；2.无参又为默认构造函数
        // 按照类型分类分为：普通构造和拷贝构造
        class Person {
        public:
	        // 无参（默认）构造函数
	        Person() {
		        cout << "无参构造函数!" << endl;
	        }
	        // 有参构造函数
	        Person(int a) {
	        	age = a;
	        	cout << "有参构造函数!" << endl;
	        }
	        // 拷贝构造函数
	        Person(const Person& p) {
	        	age = p.age;
	        	cout << "拷贝构造函数!" << endl;
	        }
	        // 析构函数
	        ~Person() {
	        	cout << "析构函数!" << endl;
	        }
        public:
	        int age;
        };

        // 构造函数的调用：

        // 1.调用无参构造函数
        void test01() 
        {
	        Person p; //调用无参构造函数
        }
        // 2.调用有参的构造函数
        void test02() 
        {
	        // 1.括号法（常用）
            // ps.调用无参构造函数不能加括号，如果加了编译器认为这是一个函数声明 Person p2();
	        Person p1(10);

	        // 2.显式法
            // ps.Person(10)单独写就是匿名对象，当前行结束之后，马上析构
	        Person p2 = Person(10);  // 使用显式法，等效于 Person p2(10);
	        Person p3 = Person(p2);  // 使用拷贝构造函数创建 p3

	        // 3.隐式转换法
            // ps.不能利用拷贝构造函数初始化匿名对象，编译器认为是对象声明 Person p5(p4);
	        Person p4 = 10; // 隐式调用构造函数，等效于 Person p4 = Person(10); 
	        Person p5 = p4; // 使用拷贝构造函数初始化 p5
        } 
        ```
    - 拷贝构造函数：
      - 理解：
          核心是通过复制另一个对象来初始化当前对象，类比于一般的函数调用
      - 用法：
        ```cpp
        // 拷贝构造函数的调用；
        
        // 1.使用一个已经创建完毕的对象来初始化一个新对象
        // 2.值传递的方式给函数参数传值
        // 3.以值方式返回局部对象（同上）
        void func(Person p)  // p 是值传递，调用拷贝构造函数
        { 
            cout << "Person's ID: " << p.getID() << endl;
        }
        int main() 
        {
            Person p1(10);  // 创建 p1
            func(p1);       // 调用拷贝构造函数将 p1 传递给函数 func
        }
        // 在 func(p1) 这一行，编译器会调用拷贝构造函数，将 p1 的值拷贝到 func 函数中的 p 参数上。这意味着函数 func 内部的 p 对象是 p1 的一个副本。
        ```
      - 深拷贝与浅拷贝：
        - 浅拷贝：
          - 简单的赋值拷贝操作
        - 深拷贝：
          - 当对象中包含指向堆内存（如指针）的成员时，拷贝构造函数需要重新分配空间，以确保新对象与原对象的指针成员指向不同的内存区域，从而避免共享同一块内存（即避免浅拷贝可能导致的问题）
      - 析构函数：
        - 作用：在于对象销毁前系统自动调用，执行一些清理工作。
        - 语法：~类名(){}
        - 用法：
          - 没有返回值
          - 不能重载，不支持参数
          - 只调用一次
  - 初始化列表：
    - 语法：构造函数()：属性1(值1),属性2（值2）... {}
        ```cpp
        class Person 
        {
        public:
	        // 传统方式初始化
	        Person(int a, int b, int c) 
            {
	            m_A = a;
	            m_B = b;
	            m_C = c;
	        }
	        //初始化列表方式初始化
	        Person(int a, int b, int c) :m_A(a), m_B(b), m_C(c) {}
	    }
        private:
	        int m_A, m_B, m_C;
        };
        ```
  - 类成员：
    - 对象成员：指类中成员的类型是另一个类的对象
        ```cpp
        class A {}
        class B
        {
            A a；
        }
        ```
      - 构造的顺序是：先调用对象成员的构造，再调用本类构造
      - 析构顺序与构造相反
    - 静态成员：static+成员变量/成员函数
      - 在编译阶段分配内存，生命周期独立于对象
      - 类内声明，类外初始化
      - 静态成员函数只能访问静态成员变量
      - 所有对象共享同一份数据，全类共享一份内存/程序共享一个函数
        ```cpp
	    // 静态成员变量两种访问方式：

	    // 1.通过对象
	    Person p1;
	    p1.m_A = 100;
	    cout << "p1.m_A = " << p1.m_A << endl;

	    Person p2;
	    p2.m_A = 200;
	    cout << "p1.m_A = " << p1.m_A << endl;  // 共享同一份数据，两个都输出200
	    cout << "p2.m_A = " << p2.m_A << endl;

	    // 2.通过类名
	    cout << "m_A = " << Person::m_A << endl;
        ```
        ```cpp
        // 静态成员函数两种访问方式：
	    class Person
        {
        public:
	        static void func() {}
        };
        void test01()
        {
	        // 静态成员变量两种访问方式：

	        // 1.通过对象
	        Person p1;
	        p1.func();

	        // 2.通过类名
	        Person::func();
        }
        ```
  - C++对象模型和this指针
    - 成员变量和成员函数的存储分离：
      - 非静态成员变量占对象空间
      - 静态成员变量不占对象空间
      - 函数也不占对象空间，所有函数共享一个函数实例
    - this指针：
      - 每个非静态成员函数都有一个隐含的指针 `this`，指向调用该成员函数的对象
        ```cpp
        class Person
        {
        public:
	        Person(int age)
	        {
		        // 当形参和成员变量同名时，可用this指针来区分
		        this->age = age;
	        }
	        Person& Add(Person p)
	        {
		        this->age += p.age;
		        //返回对象本身
		        return *this;
	        }
	        int age;
        };

        void test01()
        {
	        Person p1(10);
	        cout << "p1.age = " << p1.age << endl;
            
            // 链式调用：是指在一条语句中连续调用多个成员函数
            // 每个成员函数返回对象自身的引用，允许下一个函数调用在同一个对象上进行
	        Person p2(10);
	        p2.Add(p1).Add(p1).Add(p1);  
	        cout << "p2.age = " << p2.age << endl;  // 输出为40
        }
        ```
    - 空指针：
      - 空指针可以调用成员函数，但成员函数中不能有对类成员的访问
  - const修饰成员函数：
    - 常函数：
      - 在成员函数后加上 const，表示该成员函数是常函数
      - 常函数内不可以修改成员属性
      - 成员属性声明时加关键字mutable后，在常函数中依然可以修改
    - 常对象：
      - 声明对象前加const称该对象为常对象
      - 常对象只能调用常函数
        ```cpp
        class Person {
        public:
            // 常函数，不能修改成员变量
            void ShowPerson() const 
            {
                // m_A = 100;  // 编译错误：常函数不能修改成员变量
                m_B = 200;     // 正常，因为 m_B 是 mutable
            }
        private:
            int m_A;            // 普通成员变量
            mutable int m_B;    // mutable 修饰的成员变量，可以在常函数中修改
        };

        int main() 
        {
            const Person person;  // 创建一个常对象
            // 访问常函数
            person.ShowPerson();  // 可以调用常函数，不会修改成员
            // person.SetA(30);   // 编译错误：常对象不能调用非常函数
            return 0;
        }
        ```
  - 友元：
    - 关键字为friend
    - 三种实现(要声明)：
      - 全局函数做友元  
      - 类做友元  
      - 成员函数做友元   
        ```cpp
        // 全局函数做友元
        friend void func();
        // 类做友元
        friend class Board;
        // 成员函数做友元
        friend void Board::visit();
        ```
  - 运算符重载：
    - 对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型
    - 你无法重载或改变内置数据类型（比如 int、double、char 等）的运算符行为
        ```cpp
        // +加号运算符重载
        class Person 
        {
        public:
            Person(int a, int b) : m_A(a), m_B(b) {}
    
            // 成员函数：运算符重载
            Person operator+(const Person& p) 
            {
                Person temp;
                // this 指针是类成员函数中的隐式参数，它指向当前对象（也就是调用成员函数的具体实例）
                // 在 operator+ 中，this 就是 p1，p 是传入的 p2，所以 this->m_A 和 p.m_A 是进行加法运算的两个值。
                temp.m_A = this->m_A + p.m_A;
                temp.m_B = this->m_B + p.m_B;
                return temp;
            }

        private:
            int m_A, m_B;
        };
        
        int main() 
        {
            Person p1(10, 20);
            Person p2(30, 40);
    
            Person p3 = p1 + p2;  // 这里使用了运算符重载
            return 0;
        }
        ```
        ```cpp
        // <<左移运算符重载
        // 通过重载左移运算符和使用友元函数，你可以让你的自定义数据类型（如类 Person）支持通过输出流（如 std::cout）进行输出，从而实现打印该自定义数据类型的对象信息
        class Person 
        {
        public:
            Person(int a, int b) : m_A(a), m_B(b) {}

            // 友元函数实现左移运算符重载
            // ostream& out: 这是输出流的引用，out 是一个输出流对象，通常是 std::cout（控制台输出）或者其他类型的输出流（如文件输出流）
            friend ostream& operator<<(ostream& out, const Person& p) 
            {
                out << "a:" << p.m_A << " b:" << p.m_B;  // 这一行将 p.m_A 和 p.m_B 插入到输出流中，输出到控制台或文件
                return out;  // 最后返回输出流，以便支持链式调用（例如：std::cout << p1 << p2;）
            }
        private:
            int m_A, m_B;
        };

        int main() 
        {
            Person p1(10, 20);
            cout << p1 << endl;  // 输出：a:10 b:20
            return 0;
        }
        ```
        ```cpp
        // ++递增运算符重载

        // 前置递增++obj
        // 直接修改对象自身并返回其引用，不需要额外信息
        MyInteger& operator++() 
        {
            ++m_Num;       // 先增加m_Num的值
            return *this;  // 返回增加后的对象自身（引用）
        }
        // 后置递增obj++
        // 为了返回递增前的副本，需要先保存当前的对象状态
        // 传参，无意义占位符，通常是int类型，以此来区分前置和后置递增
        MyInteger operator++(int) 
        {
            MyInteger temp = *this;  // 保存递增前的对象副本
            m_Num++;                 // 递增m_Num
            return temp;             // 返回递增前的副本（临时对象）
        }
        ```
        ```cpp
        // =赋值运算符重载
        // 赋值运算符=用于将一个对象的值赋给另一个对象。对于涉及动态内存分配的类（如指针成员），需要处理深拷贝问题
        Person& operator=(const Person& p) 
        {
            if (this == &p) return *this;  // 防止自赋值
            delete m_Age;                  // 释放内存后赋值
            m_Age = new int(*p.m_Age);     // 深拷贝
            return *this;
        }
        ```
        ```cpp
        // 关系运算符重载
        bool operator==(const Person& p)
        {
            return this->m_Name == p.m_Name && this->m_Age == p.m_Age;
        }
        ```
        ```cpp
        // 函数调用运算符重载（仿函数）
        class MyPrint
        {
        public:
            void operator()(string text)
            {
                cout << text << endl;
            }
        };
        void test01()
        {
            //重载的（）操作符 也称为仿函数
            MyPrint myFunc;
            myFunc("hello world");
        }

        class MyAdd
        {
        public:
            int operator()(int v1, int v2)
            {
                return v1 + v2;
            }
        };
        void test02()
        {
            MyAdd add;
            int ret = add(10, 10);
            cout << "ret = " << ret << endl;
        }
        ```

---

## 继承
- `继承`是面向对象编程（OOP）中的一个重要特性，它使得一个类能够继承另一个类的属性和方法，从而避免了代码的重复，提升了代码的复用性和可维护性。
- `继承`允许我们创建一个新的类（子类）来继承一个现有类（父类）的属性和方法。子类可以继承父类的公共成员，并可以扩展或修改它们，从而实现代码复用和功能扩展。
  - 语法：
    - class 子类 : 继承方式 父类
      - class A : public B;
      - A 类称为 `子类` 或 `派生类`
      - B 类称为 `父类` 或 `基类`
    - class 子类 : 继承方式 父类1 ， 继承方式 父类2...（多继承）
      - 实际开发中不建议多继承
      - 多继承中如果父类中出现了同名情况，子类使用时候要加作用域
    - 三种继承方式：
      - 公共继承：
        - 子类可以访问父类的公共 `public` 和受保护 `protected` 成员，但不能访问父类的私有成员
      - 保护继承：
        - 子类可以访问父类的公共 `public` 和受保护 `protected` 成员，但对外界来说，所有继承的成员都变为保护成员 `protected`
      - 私有继承：
        - 子类无法直接访问父类的公共或保护成员，它们在子类中变为私有成员 `private`
        - 子类可以利用父类的实现来帮助完成自己的功能，但父类的功能不会暴露给外界，外界无法直接通过子类来访问父类的功能。
      - ps.父类中私有成员也是被子类继承下去了，只是由编译器给隐藏后访问不到
  - 继承中构造和析构顺序：
    - 继承中,先调用父类构造函数，再调用子类构造函数，析构顺序与构造相反
  - 继承同名成员处理方式：
    - 访问子类同名成员 直接访问即可
    - 访问父类同名成员 需要加作用域
      - 如果子类和父类有同名的成员（无论是属性还是方法），子类会隐藏父类的成员
    ```cpp
    s.func();
    s.Base::func();
    // 若同名静态（static）成员，多一种访问方法（通过类名访问）
    Son::func();	
    Son::Base::func();
    ```
  - 菱形继承/钻石继承：
    - 具体来说，假设有一个基类 A，然后有两个类 B 和 C 分别继承自 A，最后有一个类 D 同时继承自 B 和 C。在这种情况下，类 D 会通过 B 和 C 两条路径继承类 A，而导致类 A 被重复继承
    - 使用虚继承的解决方案
    - 当一个类被多个类继承，并且它们有共同的父类时，会导致父类成员被多次继承，造成二义性。通过使用虚继承，可以确保共同的父类只被继承一次
    ```cpp
    class Animal 
    {
    public:
        int m_Age;
    };
    // 继承前加virtual关键字后，变为虚继承
    // 此时公共的父类Animal称为虚基类
    // 使用虚继承，确保 Animal 类只有一个实例
    class Sheep : virtual public Animal {};
    class Tuo : virtual public Animal {};
    class SheepTuo : public Sheep, public Tuo {};
    ```

---

## 多态
- 多态的本质是通过继承和虚函数机制来实现的，使得同一个函数名在不同的类中可以有不同的实现
- 多态就是“同一操作，不同表现”
  - 静态多态（编译时多态）：
    - 主要通过函数重载和运算符重载来实现
    - 编译时绑定（静态绑定/早绑定）
  - 动态多态（运行时多态）：
    - 通过虚函数和继承来实现。
    - 运行时绑定（动态绑定/晚绑定）
    ```cpp
    class Animal 
    {
    public:
        // 虚函数，不会在编译时调用
        virtual void speak() 
        { 
            cout << "动物在说话" << endl;
        }
    };

    class Cat : public Animal 
    {
    public:
        // 重写父类的虚函数
        void speak() override 
        { 
            cout << "小猫在说话" << endl;
        }
    };

    class Dog : public Animal 
    {
    public:
        // 重写父类的虚函数
        void speak() override 
        { 
            cout << "小狗在说话" << endl;
        }
    };

    // C++ 会在运行时根据对象的实际类型来决定调用哪个版本的函数
    void DoSpeak(Animal& animal) 
    {
        animal.speak();  
    }    

    int main() 
    {
        Cat cat;
        Dog dog;
        DoSpeak(cat);  // 输出：小猫在说话
        DoSpeak(dog);  // 输出：小狗在说话
        return 0;
    }
    ```
  - 条件：
    - 有继承关系
    - 子类重写父类中的虚函数
      - 函数返回值类型 函数名 参数列表 完全一致称为重写
    - 父类指针或引用指向子类对象
  - 纯虚函数和抽象类：
    - 纯虚函数是没有实现的虚函数，它的声明以 = 0 结尾
    - 当一个类中存在纯虚函数时，类变成抽象类（即无法实例化），只有继承该类的子类，重写了所有纯虚函数后，才可以实例化对象。
    - 语法：virtual 返回值类型 函数名 （参数列表）= 0 ;
  - 虚析构和纯虚析构：
    - 多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码
      - 在多态的情况下，如果父类指针指向一个子类对象，当我们通过父类指针删除子类对象时，需要确保子类的析构函数被正确调用，否则子类特有的资源（例如堆区分配的内存）可能不会被释放，造成内存泄漏。
      - 如果子类中有动态分配的堆内存（例如 new 创建的对象或数组），那么当通过父类指针删除子类对象时，父类的析构函数如果没有声明为虚析构函数，那么只有父类的析构函数会被调用，子类的析构函数就不会被调用，从而导致堆内存无法释放。
    - 解决方式：
      - 虚析构：
        - 语法：virtual ~类名(){}
      - 纯虚析构：
        - 语法：
          - virtual ~类名() = 0;
          - 类名::~类名(){}
        - 注意：
          - 声明：在类内声明析构函数时，使用 = 0 来指明它是纯虚的。
          - 定义：在类外定义析构函数，通常我们会在类外定义其实现。
          - 拥有纯虚析构函数的类也属于抽象类
  
---

## 文件操作
- 三类文件操作（需 `#include < fstream >`）
  - 写文件：使用 ofstream （输出流对象）
  - 读文件：使用 ifstream （输入流对象）
  - 读写文件：使用 fstream （同时支持输入输出操作）
- 文本文件：文件内容是以可读的 `ASCII` 码存储，通常是我们能直接看到的文字内容
  - 写文件：
  ```cpp
    // 写文件步骤：
    // #include <fstream>               // 1.包含头文件
    // ofstream ofs;                    // 2.创建流对象
    // ofs.open("文件路径",打开方式);    // 3.打开文件
    // ofs << "写入的数据";              // 4.写数据
    // ofs.close();                     // 5.关闭文件
    #include <fstream>
    using namespace std;

    void test01() 
    {
        ofstream ofs;                    // 创建写文件的流对象
        ofs.open("test.txt", ios::out);  // 打开文件
        ofs << "姓名：张三" << endl;      // 向文件写入数据
        ofs.close();                     // 关闭文件
    }
  ```
    - 文件打开方式：
      - 文件打开方式可以配合使用，利用 `|` 操作符
        - 例如：用二进制方式写文件 `ios::binary | ios:: out`
  
    | 打开方式       | 解释                                         |
    |----------------|---------------------------------------------|
    | `ios::in`      | 以读模式打开文件                             |
    | `ios::out`     | 以写模式打开文件                             |
    | `ios::ate`     | 设置文件的初始位置为文件尾                    |
    | `ios::app`     | 以追加模式打开文件（写操作从文件末尾开始）      |
    | `ios::trunc`   | 如果文件存在，先删除原文件，再创建新文件       |
    | `ios::binary`  | 以二进制模式打开文件                          | 

  - 读文件：
  ```cpp
  // 读文件步骤：
  // #include <fstream>               // 1.包含头文件
  // ifstream ifs;                    // 2.创建流对象
  // ifs.open("文件路径",打开方式);    // 3.打开文件并判断文件是否打开成功
  // 四种方式读取                      // 4.读数据
  // ifs.close();                     // 5.关闭文件
  #include <fstream>
  #include <string>
  
  void test01()
  {
      ifstream ifs;
      ifs.open("test.txt", ios::in);

      // 利用is_open函数可以判断文件是否打开成功
      if (!ifs.is_open())
      {
          cout << "文件打开失败" << endl;
          return;
      }

      // 第一种（逐个读取）
      char buf[1024] = { 0 };
      while (ifs >> buf)
      {
          cout << buf << endl;
      }

      // 第二种（按行读取）
      char buf[1024] = { 0 };
      while (ifs.getline(buf,sizeof(buf)))
      {
          cout << buf << endl;
      }

      // 第三种
      string buf;
      while (getline(ifs, buf))
      {
          cout << buf << endl;
      }

      // 第四种（逐字符读取）
      char c;
      while ((c = ifs.get()) != EOF)
      {
          cout << c;
      }

      ifs.close();
  }
  ```
- 二进制文件：文件内容是以 `二进制` 的形式存储，通常不能直接看到内容，需要程序进行解析
  - 写文件：
  ```cpp
    // 写文件步骤：
    // 1.包含头文件
    #include <fstream>
    #include <string>

    class Person
    {
    public:
        char m_Name[64];
        int m_Age;
    };

    void test()
    {
        // 2.创建输出流对象
        ofstream ofs("person.txt", ios::out | ios::binary);
        // 3.打开文件
        ofs.open("person.txt", ios::out | ios::binary);
        Person p = {"张三"  , 18};
        // 4.写文件
        // 函数原型：ostream& write(const char * buffer,int len);
        // 字符指针buffer指向内存中一段存储空间，len是读写的字节数
        ofs.write((const char *)&p, sizeof(p));
        // 5.关闭文件
        ofs.close();
    }
        // 文件输出流对象，可以通过write函数，以二进制方式写数据
  ```
  - 读文件：
  ```cpp
    // 读文件步骤：
    // 1.包含头文件
    #include <fstream>
    #include <string>

    class Person
    {
    public:
        char m_Name[64];
        int m_Age;
    };

    void test()
    {
        // 2.创建输入流对象
        ifstream ifs("person.txt", ios::in | ios::binary);
        // 3.打开文件
        if (!ifs.is_open())
	      {
            cout << "文件打开失败" << endl;
        }
        Person p;
        // 4.读文件
        // 函数原型：istream& read(char *buffer,int len);
        // 字符指针buffer指向内存中一段存储空间，len是读写的字节数
        ifs.read((char *)&p, sizeof(p));
        // 5.关闭文件
        ifs.close();
    }
        // 文件输入流对象 可以通过read函数，以二进制方式读数据
  ```

---

## 泛型编程
- 模板：
  - 特点：
    - 模板是一种建立通用框架的工具，不能直接使用，需要在实际调用时具体化
    - 模板的通用并非万能
      - 如果模板函数执行了不适合某些数据类型的操作（如数组赋值操作或自定义类型的比较），编译时可能会报错
      - 对于数组，`T a, T b` 形式的模板函数不能直接处理数组赋值，因为模板没有意识到这是数组类型；而对于 Person 类型等自定义类型，模板可能无法处理非基础类型的比较操作
  - 两种模板机制：
    - 函数模板：
        - 语法：
        ```cpp
        // 每个模板函数前都必须明确声明为模板函数
        // template — 声明创建模板
        // typename — 表面其后面的符号是一种数据类型，可以用class代替
        // T — 通用的数据类型，名称可以替换，通常为大写字母
        template <typename T>
        // 函数声明或定义
        void func(T a, T b) {}
        // 使用函数模板两种方式：
        // 1.自动类型推导	
        // 必须推导出一致的数据类型T，才可以使用
        // 模板必须要确定出T的数据类型，才可以使用
        mySwap(a, b);	
        // 2.显示指定类型	
        mySwap<int>(a, b);
        ```
        - 普通函数 vs 函数模板：
          - 区别：
            - 普通函数调用时可以发生自动类型转换（隐式类型转换）
              - 只能处理特定类型的数据
              - e.g.`char` 类型可以被隐式转换为 `int` 类型
            - 函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换；如果利用显示指定类型的方式，可以发生隐式类型转换
              - 建议使用显示指定类型的方式，调用函数模板，因为可以自己确定通用类型T
          - 调用规则：
            - 优先调用普通函数（如果普通函数和模板函数都能匹配）
            - 可以通过空模板参数列表来强制调用函数模板
              - myPrint<>(a, b);
            - 函数模板也可以发生重载
            - 模板匹配的优先级（如果函数模板能提供更好的匹配，编译器会优先调用模板函数）
            - ps.既然提供了函数模板，最好就不要提供普通函数，否则容易出现二义性
    - 类模板：
      - 语法同函数模板
      - 特点：
        - 类模板不能像函数模板一样使用自动类型推导，必须显式指定类型
        - 类模板的参数可以指定默认值，而函数模板的参数不行
        - 类模板中的成员函数在调用时才创建（延迟实例化），而普通类中的成员函数一开始就可以创建
      - 类模板对象作函数参数：
        - 指定传入的类型（最常用）
        - 参数模板化
        - 整个类模板化
        ```cpp
        #include <string>
        //类模板
        template<class NameType, class AgeType = int> 
        class Person
        {
        public:
            Person(NameType name, AgeType age)
            {
                this->mName = name;
                this->mAge = age;
            }
        public:
            NameType mName;
            AgeType mAge;
        };
        // 1.指定传入的类型 — 直接显示对象的数据类型
        void printPerson1(Person<string, int> &p) {}
        void test1()
        {
            Person <string, int >p("孙悟空", 100);
            printPerson1(p);
        }
        // 2.参数模板化 — 将对象中的参数变为模板进行传递
        template <class T1, class T2>
        void printPerson2(Person<T1, T2>&p){}
        void test2()
        {
            Person <string, int >p("猪八戒", 90);
            printPerson2(p);
        }
        // 3.整个类模板化 — 将这个对象类型模板化进行传递
        template<class T>
        void printPerson3(T & p){}
        void test3()
        {
            Person <string, int >p("唐僧", 30);
            printPerson3(p);
        }
        ```
      - 类模板与继承：
        - 指定父类模板的类型：
          - 当我们继承一个类模板时，子类需要显式地指定父类模板的类型
            ```cpp
            template <class T> class Base {T m;};
            // 必须指定Base<int>，否则编译时无法知道Base的类型，无法给子类分配内存
            class Son : public Base<int> {};
            ```
        - 子类模板化：
          - 如果希望让子类模板化，允许在继承时灵活指定类型，可以在子类定义时也采用模板
            ```cpp
            template <class T1, class T2> 
            // 可以在Son2中指定父类Base的模板类型
            class Son2 : public Base<T2> {};
            ```
      - 类模板成员函数的类外实现：
        - 当类模板中有成员函数时，我们可以将其实现放到类外部。在实现时，需要显式地指定模板参数列表
        - 类内声明，类外实现
        ```cpp
        template<class T1, class T2> 
        class Person 
        {
            Person(T1 name, T2 age);
        };
        // 类外实现
        template<class T1, class T2> 
        Person<T1, T2>::Person(T1 name, T2 age) 
        {
            this->m_Name = name;
            this->m_Age = age;
        }
        ```
        - 分文件编写问题：
          - 解决类模板成员函数的延迟实例化
          - 两种做法：
            - 直接包含.cpp源文件
              - 违反代码分离的设计原则
            - 将模板代码（声明和实现）写入 .hpp 文件
              - ps. `#pragma once` 是一种头文件保护机制，用于防止头文件被重复包含（多次编译），从而避免重复定义错误或其他问题
      - 类模板与友元：
        - 暂只讨论全局函数
        - 两种实现：
          - 类内实现：
            - 直接实现
            ```cpp
            template<class T1, class T2>
            class Person 
            {
                // 类内实现友元函数
                friend void printPerson(Person<T1, T2>& p) 
                {
                    cout << "姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
                }
            };
            ```
          - 类外实现：
            - 提前声明
            ```cpp
            template<class T1, class T2>
            void printPerson2(Person<T1, T2>& p) 
            {
                cout << "类外实现 ---- 姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
            }

            template<class T1, class T2>
            class Person 
            {
                // 类外实现的友元函数需要在类内用 `friend` 声明
                friend void printPerson2<>(Person<T1, T2>& p);
            };
            ```

---