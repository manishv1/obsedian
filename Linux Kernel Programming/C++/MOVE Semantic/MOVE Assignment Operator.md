
## Move Assignment Operator

The move assignment operator in C++ is the rvalue counterpart of the assignment operator. The assignment operator is quite similar to a constructor, except that before stealing the resources of some other object, the object releases its resources first to prevent a memory leak. After releasing its resources, it grabs the resources of the other object, deallocates the memory of the other object's data members, and returns *this.

The following snippet shows how a move assignment operator is defined:

```cpp
// move assignment operator
A& operator=(A&& temp) {
    
    // Check against self assignment
    if (this != &temp) {
        // Free the existing resource.
        delete[] arr;

        // Copy the primitive types
        size = temp.size;
        flag = temp.flag;
        
        // Move the pointer in temporary object
        arr = temp.arr;

        temp.arr = NULL;
        
   }
   return *this;
}
```