
An lvalue is an object which has its permanent memory location where they are stored (and not temporary objects and literals). We can use the assignment operator to change the value of lvalue objects. Lvalues need not be a variable necessarily, such as:

```cpp
#include <iostream>

using namespace std;

int y = 200;

int& setvalue()
{
    return y;
}

int main()
{
    int x = 10; 
    setvalue() = 15;
    cout<<y<<endl;
}
```




On the other hand, an rvalue is a temporary object or a literal which doesn't have a permanent memory location. We cannot make assignments to rvalues. The following snippet has some rvalues:

```cpp
#include <iostream>

using namespace std;

string getStr()
{
    return "SCALER";
}

int main()
{
    getStr();
    int x = 10;
}
```


In the above code, <span style="color:rgb(192, 0, 0)">getStr() </span>in main function  is an <span style="color:rgb(192, 0, 0)">rvalue </span>since it returns a temporary object as well as the right-hand side of the expression <span style="color:rgb(192, 0, 0)">x = 10</span> 



Since C++11, two kinds of references have existed - lvalue and rvalue references. As the name suggests, **lvalue references** can bind to existing lvalues. They could also bind to rvalues but only when the reference variable is declared as a constant because lvalue reference does not allow rvalues to change. The syntax for lvalue reference is the same as a regular reference variable (Datatype&).

**Rvalue references, on the other hand**, can only bind to rvalues. However, the critical thing to note here is that rvalue references can modify the value of rvalues means the reference variable need not be constant, which is impossible with lvalues. An rvalue reference is declared with two ampersands instead of one(Datatype&&). Rvalue reference could not refer to lvalue.


<span style="color:rgb(255, 255, 0)">Ha Ha Ha ........ above line are good to mad anyone.

</span><span style="color:rgb(255, 255, 0)"> </span> 

Go easy start with the snippet code 


Now, let's see how we can detect temporary objects using these rvalue references. Since we know that rvalue references can only bind to rvalues, we can rest assured that whenever a function with an rvalue reference as a parameter gets called, the parameter passed in the calling function was an rvalue.

**Consider the following code:**

```cpp
#include <iostream>

using namespace std;

void func1(const int &x) {
    cout<<"Inside func1 : x = "<<x<<endl;
}

int main()
{
    int x = 10;
    func1(x);
    func1(15);
    return 0;
}
```

**Output**

```cpp
Inside func1 : x = 10
Inside func1 : x = 15
```

In the above code, func1 gets called twice irrespective of whether the passed parameter is an lvalue or rvalue. Now, let's overload this function with an <span style="color:rgb(255, 255, 0)">rvalue</span> reference and try to observe its behavior:


```cpp
#include <iostream>

using namespace std;

void func1(const int &x) {
    cout<<"Inside lvalue func1 : x = "<<x<<endl;
}

void func1(int &&x) {
    cout<<"Inside rvalue func1 : x = "<<x<<endl;
}


int main()
{
    int x = 10;
    func1(x);
    func1(15);
    return 0;
}
```


**Output**

```cpp
Inside lvalue func1 : x = 10
Inside rvalue func1 : x = 15
```

When an rvalue is passed as a parameter in the above code, the function with the signature void func1(int &&x) gets called. This implies that whenever this function gets called, the passed argument must have been an rvalue. Thus, by using two overloaded methods, we can separate the program workflow for lvalues and rvalues. Therefore, we can use an overloaded function with an rvalue reference as the parameter to process temporary objects separately.

## Move constructor

Before understanding what a move constructor in C++ is, first understand what "move" means here. In the case of primitive data types like int, float, char, etc., "move" means to copy the data value from one variable to another. However, in some cases like pointers, "move" means stealing the pointer's resources, e.g., say there is a pointer variable ptr1 which points to memory address 0x7ffe22726680. Now, let's say we have another pointer, ptr2, which wants to capture the resources of ptr1, then all we need to do is assign the address 0x7ffe22726680 to ptr2 and null out ptr1. Now, imagine ptr1 to be a temporary object, and thus, using the above technique, we can steal away the resources of the temporary object, which would be deleted soon.

Now, assume we have a class with many complex data members. Imagine we are creating a vector for the elements of this class. So, every time we call the vector::push_back() method for an rvalue of this class, the [copy constructor](https://www.scaler.com/topics/copy-constructor-in-cpp/) gets called, and all the data members have to be copied, which may turn out to be quite expensive.

**Let's understand this with a more straightforward example.**

Consider the following class:
```cpp
class A {
    int *arr, size;
    bool flag;
    
    public:
        // parameterized constructor
        A(int len) {
            size = len;
            arr = new int[size];
            flag = false;
        }    
        
        // copy constructor
        A (const A& temp) {
            size = temp.size;
            arr = new int[temp.size];
            for ( int i = 0; i < temp.size; ++i ) {
                arr[ i ] = temp.arr[ i ];
            }
            flag = temp.flag;
        }
        
        // destructor
        ~A () {
            delete [] arr;
        }
    
};
```

Assume that we have a temporary object of class A and want to copy its contents to another object. In such a case, the copy constructor will get called, and the entire array inside the class will get copied, i.e., if the size of the array is 100000, there will be 100000 copy operations. This is shown in the following snippet:

```cpp
int main()
{
    A obj(100000);
    A obj1(obj);    // Copy constructor get's called and 100000 elements of the array are copied

    return 0;
}
```

We can create another constructor to handle temporary objects specifically to avoid this. This new constructor is known as the "move constructor" which is shown as follows:

```cpp
class A {
    int *arr, size;
    bool flag;
    
    public:
        // parameterized constructor
        A(int len) {
            size = len;
            arr = new int[size];
            flag = false;
        }    
        
        // copy constructor
        A (const A& temp) {
            size = temp.size;
            arr = new int[temp.size];
            for ( int i = 0; i < temp.size; ++i ) {
                arr[ i ] = temp.arr[ i ];
            }
            flag = temp.flag;
        }
        
        // move constructor
        A (A&& temp) {
            arr = temp.arr;
            size = temp.size;
            flag = temp.flag;
            temp.arr = NULL;
        }
    
        // destructor
        ~A () {
            delete [] arr;
        }
    
};
```

The move constructor has been added in the code above, which copies the value of flag and size from temp since they are primitive types. Still, in the case of arr, only the pointer's value is stored, and a separate memory is not allocated, unlike the copy constructor. Also, the arr of temp has been set to null. This is because, when the temporary object gets destroyed, the class's destructor will get called, which would deallocate the memory pointed to by arr of the temporary object. So, if we try to use the moved arr, we would run into a segmentation fault because that memory has already been deallocated.

One thing to note here is the parameter list of the move constructor. It takes an rvalue reference as a parameter which ensures that all rvalues get passed to the move constructor and not the copy constructor. Another important observation is that the rvalue reference in the parameter is non-const. This is necessary because otherwise, we wouldn't be able to change the pointer to NULL.

**Note**: If the temporary object itself is constant, the copy constructor will get called instead of the move constructor.

There is another case that we haven't discussed yet related to the move constructor, but we'll come to that later once we have introduced the std::move function.


## std::move

Before discussing the std::move, let's highlight a problem with the move constructor that we defined previously. Consider the following code:

```cpp
#include <iostream>

using namespace std;

class A {
    int *arr, size;
    bool flag;
    
    public:
        // parameterized constructor
        A(int len) {
            cout<<"Inside parameterized constructor\n";
            size = len;
            arr = new int[size];
            flag = false;
        }    
        
        // copy constructor
        A (const A& temp) {
            cout<<"Inside copy constructor\n";
            size = temp.size;
            arr = new int[temp.size];
            for ( int i = 0; i < temp.size; ++i ) {
                arr[ i ] = temp.arr[ i ];
            }
            flag = temp.flag;
        }
        
        // move constructor
        A (A&& temp) {
            cout<<"Inside move constructor\n";
            arr = temp.arr;
            size = temp.size;
            flag = temp.flag;
            temp.arr = NULL;
        }
    
        // destructor
        ~A () {
            delete [] arr;
        }
    
};

int main()
{
    A a1(100);
    A a2(a1);

    return 0;
}
```

**Output**

```cpp
Inside parameterized constructor
Inside copy constructor
```

As evident from the output above, the object a2 receives the lvalue a1 as an input which triggers the invocation of the copy constructor instead of the move constructor. This is because the move constructor only gets invoked when the constructor argument is an rvalue. This is where the std::move function comes into play from the utility header file. This function converts an lvalue to an rvalue.

Let's take a look at another example using std::move():

```cpp
#include <iostream>

using namespace std;

class A {
    int *arr, size;
    bool flag;
    
    public:
        // parameterized constructor
        A(int len) {
            cout<<"Inside parameterized constructor\n";
            size = len;
            arr = new int[size];
            flag = false;
        }    
        
        // copy constructor
        A (const A& temp) {
            cout<<"Inside copy constructor\n";
            size = temp.size;
            arr = new int[temp.size];
            for ( int i = 0; i < temp.size; ++i ) {
                arr[ i ] = temp.arr[ i ];
            }
            flag = temp.flag;
        }
        
        // move constructor
        A (A&& temp) {
            cout<<"Inside move constructor\n";
            arr = temp.arr;
            size = temp.size;
            flag = temp.flag;
            temp.arr = NULL;
        }
    
        // destructor
        ~A () {
            delete [] arr;
        }
    
};

int main()
{
    A a1(100);
    A a2(std::move(a1));

    return 0;
}
```

**Output**

```cpp
Inside parameterized constructor
Inside move constructor
```

Now move constructor is get called as we passed the rvalue at the time of the creation of a new object of class A.

### How Does std::move Work?

std::move gets the job done using typecasting. It is a static_cast from an lvalue to an rvalue. The typecast is necessary to safeguard against the accidental binding of rvalue to an lvalue.

## Move Constructors and Implicitly Generated Constructors

In C++, if the user defines any constructor for the class, the [online C++ compiler](https://www.scaler.com/topics/cpp/online-cpp-compiler/) abstains from adding a default constructor. The same condition applies here. If a move constructor is defined for the class, it is the programmer's responsibility to add a default constructor as well (if required). However, the compiler will still implicitly generate a copy constructor and standard assignment operator even if the user has defined the move constructor or move assignment operator.

## When to Use Move Semantics?

It is generally a good idea to overload functions to work with both lvalues and rvalues. We use move semantics when methods take rvalue as a parameter. With rvalues, if a heavy object gets passed as a parameter, we can move around the resources of that rvalue in the overloaded function instead of copying all the data members. One example of such a scenario is that some STL containers have two versions of the push_back() method: one having the parameter const T& obj and the other having T&& obj.

## When Not to Use Move Semantics?

There are some scenarios where the optimization provided by modern compilers can perform better than the usage of std::move. In such cases, we must abstain from using move semantics in C++.

**Let's understand one such scenario with the following code:**

```cpp
#include <iostream>

using namespace std;

class A {
    int *arr, size;
    
    public:
    
    A(int len) {
        size = len;
        arr = new int[len];
    }
    A(const A& temp) {
        size = temp.size;
        delete [] arr;
        arr = new int[size];
        for(int i=0;i<size;i++)
            arr[i] = temp.arr[i];
    }
    A(A&& temp) {
        size = temp.size;
        arr = temp.arr;
        temp.arr = NULL;
    }
    ~A() {
        delete [] arr;
    }
};

A getObj() {
    A obj(10);
    return std::move(obj);
}

int main()
{
    A obj = getObj();

    return 0;
}
```

In the above code, when the function getObj() is called, two instances of the class are created: the first being a temporary object that is being returned by the function, and the other is the object inside the main() function on the left side of the assignment operator.

In reality, the compiler uses <span style="color:rgb(255, 0, 0)">Return Value Optimization(RVO)</span>, <span style="color:rgb(255, 0, 0)">which prevents copies of temporary objects.</span> So, while using RVO, only one copy of the object exists, which is better than using std::move.

## Returning an Explicit Rvalue-reference from a Function

First, let's understand the difference between returning an object by value and returning an rvalue reference.

**Consider the following snippet:**

```cpp
float f;


float&& getObjRef() {
    return std::move(f);
}

float getObj() {
    return f;    
}
```

Here, the function getObj() returns a copy of the global variable, whereas the function getObjRef() explicitly returns an rvalue with the same address as that of the global variable.

This method of returning explicit rvalue references must be used carefully to avoid the risk of dangling references where the reference would continue to exist, but the temporary object gets destroyed. One such example could be when the getObjRef() function in the above code returns a local object instead of a global object.

## Move Semantics and Rvalue Reference Compiler Support

GCC, Intel compiler, and MSVC are the compilers that support rvalue references and hence, move semantics as well.

## Conclusion

- Move semantics is a compelling feature of C++, but as mentioned before, we should use it cautiously so as not to interfere with compiler optimizations.
- It is recommended to declare a move constructor and move assignment operator for a class that may have large data members to avoid expensive copy operations.
- Generally, one should only move around resources in the move constructor and move assignment operator functions. These functions shouldn't allocate any memory or invoke any other function. So, we can mark the move constructor and move assignment operator with the noexcept keyword, which optimizes the function never to throw an exception.