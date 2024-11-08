
### Deleted Copy Constructor

Deleting the copy constructor makes a class non-copyable. This is useful for classes that should not be copied, such as singletons or classes managing unique system resources.

**Uses**:

- To ensure that a class can have only one instance.
- For classes that manage unique system resources where copying does not make sense or could lead to resource conflicts or leaks.

**Example**:

```cpp
class NonCopyable {
public:
    NonCopyable() = default;

    // Deleted copy constructor
    NonCopyable(const NonCopyable&) = delete;
};

void example() {
    NonCopyable obj1;
    // NonCopyable obj2 = obj1;  // This line would cause a compilation error
}
```

**Explanation**: NonCopyable explicitly deletes its copy constructor, making it impossible to create a copy of an NonCopyable object. Attempting to copy obj1 into obj2 would result in a compilation error, enforcing the non-<span style="color:rgb(255, 255, 0)">copyable</span> <span style="color:rgb(255, 255, 0)">behavior</span>.