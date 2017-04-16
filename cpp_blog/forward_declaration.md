# c++前向声名 会隐藏掉类之间的继承关系

最近在看google 的c++ 编码规范时，在讲到前向声名时举了个例子来说明前向声名有可能会改变代码的用意。具体例子如下：

```c++
// b.h:
struct B {};
struct D : B {};

// good_user.cc:
#include "b.h"
void f(B*);
void f(void*);
void test(D* x) { f(x); }  // calls f(B*)
```

为了验证这个例子，我对示例代码进行了扩充：

```c++
//func.h
#include <iostream>
using namespace std;  

struct B;
struct D;

void f(B*);
void f(void*);
void test(D* x); 


//func.cpp
#include "func.h"
        
void f(B *p)                   
{ 
    cout<<"B*"<<endl;          
} 

void f(void *p)                
{ 
    cout<<"void"<<endl;        
}

void test(D *p)
{
    f(p);
}


//b.h
#include <iostream>                             
using namespace std;           

struct B                       
{ 
    virtual void output();
    int x;                     
};
  
  
struct D: B                    
{
    int y;
    virtual void output();
};


//b.cpp
#include "b.h"
                               
void B::output()               
{
    cout<<"B"<<endl;           
}
  
void D::output()               
{ 
    cout<<"D"<<endl;           
} 


//main.cpp
#include <iostream>            
#include "b.h"                 

#include "func.h"

using namespace std;           
  
  
int main()                     
{ 
    D d;
    test(&d);                  
    return 0;
}
   
```

运行上面的例子，结果输出 void，可以看出，由于使用了前向声名，在调用test函数，进而调用f函数时， 类B和类D之间的继承关系被隐藏掉了，因此调用的是void *参数的f函数。