


1. C++ gives the thread class to create a thread .
2. The function to be executed in thread must be given as a callable object to the constructor of thread class .
3. ![[Possible Ways to Create Thread.png]]
4. ![[Callable_Class.png]]
5. Above it is shown three ways to give callable objects to the thread constructor
6. if you forgot to join or detach on join able thread, then at time of destruction std::terminate will be called  
7. if any program call the std::terminate call we refer such a program as unsafe program 
8. You can call join or detach once in life cycle of the thread.
9. Once called the join on a thread it becomes non join able.
10. "joins" introduce the synchronization  point between launched thread and the thread that is launched from 
11. Also joins blocks the execution of thread that calls join function , until the launched thread's execution finished.   

![[Thread-Synchronization-using-join.png]]

  
12. Detach separates the launched thread from the thread object when it lunched from allowing the execution to continue independently 
13. Any allocated resources will be freed once the thread exist.

![[Detech function.png]]

14.  Note the "join" function blocks the calling threads , "detech" function did not block the calling thread 
15. 