A move constructor is a [constructor](https://en.cppreference.com/w/cpp/language/constructor "cpp/language/constructor") which can be called with an argument of the same class type and copies the content of the argument, possibly mutating the argument.

struct X
{
    X(X&& other); // move constructor
//  X(X other);   // Error: incorrect parameter type
};



Move constructors typically transfer the resources held by the argument (e.g. pointers to dynamically-allocated objects, file descriptors, TCP sockets, thread handles, etc.) rather than make copies of them, and leave the argument in some valid but ==otherwise== ==indeterminate state==. Since move constructor doesn’t change the lifetime of the argument, the destructor will typically be called on the argument at a later point. For example, moving from a [std::string](https://en.cppreference.com/w/cpp/string/basic_string "cpp/string/basic string") or from a [std::vector](https://en.cppreference.com/w/cpp/container/vector "cpp/container/vector") may result in the argument being left empty.

Here meaning of otherwise indeterminate state is pointer in the source object is valid but does not reference to any valid data. 

If a vector is moved from source object to the destination object using MOVE semantic the vector is valid but you cannot rely on the contents of the vectors 

```cpp
 // Move constructor definition
    Move (Move&& A)
    {
        // It will simply help in pointing to the resource,
        // without creating a copy.
         cout << "Move constructor invoked\n";
        this->p = A.p;
        A.p = NULL;
    }
```
Here   ==A.p==  is set to NULL , but  =="p"== still exist in object "A" but pointing to NULL.



1. In a program we have several objects scattered here and there , possibly 
2. We need to copy those objects to a container  
3. all the resources associated with the objects needs to be copied to the container 
4. Copying resources associated with the container is costly , as need to do reallocation in the container and free form the object being copied 
5. So if we move the resources from source object( the object to be copied) to the container this is efficient 


To understand it more clearly let us take an example 




class Move 
{

	 

}