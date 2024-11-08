
![[Pasted image 20241102152756.png]]


![[Pasted image 20241102153004.png]]


![[Pasted image 20241102153100.png]]



thread t([](){

     cout<<"lambd aexpression \n";
})

[[Learn Generic Lambda in C++]]

From C++ 14 generic lambda , you are permitted to use "auto" key word

![[Pasted image 20241102153632.png]]

#include <iostream>
#include <stdexcept>

int main() {
    auto divide = [](int a, int b) {
        try {
            if (b == 0) {
                throw std::runtime_error("Division by zero!");
            }
            return a / b;
        } catch (const std::exception& e) {
            std::cerr << "Error: " << e.what() << std::endl;
            return 0; // Or any other appropriate value
        }
    };

    int result = divide(10, 0);
    std::cout << "Result: " << result << std::endl;

    return 0;
}



#include <iostream>
#include <thread>
#include <atomic> 
#include <chrono>

using namespace std;

int main()
{
    atomic<int> count = 0;
    const int ITERATIONS = 1E6;

	// Here count is passed as input parameter 
    thread t1([&count](){
        for(int i = 0; i < ITERATIONS; i++)
        {            ++count;
        }
    });

    thread t2([&count](){
        for(int i = 0; i < ITERATIONS; i++)
        {
            ++count;
        }
    });

    t1.join();
    t2.join();

    cout << count << endl;

    return 0;
}
