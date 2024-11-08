
https://www.youtube.com/watch?v=sfbkMgwZIT


<span style="color:rgb(255, 255, 0)">std::move(ptr) is a function that casts its argument to an rvalue reference.
This operation, known as move semantics, is primarily used to efficiently
transfer ownership of resources from one object to another, especially
when dealing with large objects or resources allocated on the heap.
</span>
  



#include<bits/stdc++.h>
#include<unistd.h><span style="color:rgb(255, 0, 0)">
</span>
#include<sys/types.h><span style="color:rgb(255, 0, 0)">

  </span><span style="color:rgb(255, 0, 0)">
  
</span>
using namespace std;
int main()
{

auto ptr1 = std::make_unique<int[]>(10);
//auto ptr2 = ptr1; // will throw you error

<span style="color:rgb(255, 255, 0)">//</span> <span style="color:rgb(255, 255, 0)">to print address of a unique pointer we use  "ptr.get()" as unlike normal point it an object</span>
std::cout << "ptr1\t" <<<span style="color:rgb(255, 255, 0)"> ptr1.get()</span> << std::endl;

<span style="color:rgb(255, 0, 0)">auto ptr2 = move(ptr1);</span> // Moving ptr1 to ptr2 

std::cout << "ptr2\t" << ptr2.get() << std::endl;
std::cout << "ptr1\t" << ptr1.get() << std::endl;
return 0;
} 



Rvalue references are a powerful language feature introduced in C++11 to enhance performance and flexibility.

C++

```
int&& rr = 10; // rr refers to a temporary integer object
```



#include <iostream>
#include <string>

class MyClass {
public:
    MyClass(const std::string& str) : str_(str) {
        std::cout << "Constructor called\n";
    }

    MyClass(MyClass&& other) : str_(std::move(other.str_)) {
        std::cout << "Move constructor called\n";
    }

private:
    std::string str_;
};

int main() {
    MyClass obj1("Hello");
    MyClass obj2 = std::move(obj1); // Move semantics used here
}

