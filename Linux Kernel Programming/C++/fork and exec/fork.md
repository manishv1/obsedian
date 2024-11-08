
https://www.youtube.com/watch?v=IFEFVXvjiHY

fork --: The fork system call i used to create a separate ,duplicate process.

exec --: When an exec() system call is invoked, the program specified in the parameter to exec() will replace the entire process - including all threads . Means after call to "exec" no further contents written after exec will be printed as whole process is replaced with the content of the  program given to exec to execute

#include<bits/stdc++.h>
#include <sys/types.h>
#include<unistd.h>
using namespace std;


int main()
{

	std::cout << "main function before call to fork" << std::endl;
	pid_t pid = fork();
	
	
	if(pid == 0) {std::cout << "*************Child Process*************** " << std::endl;}
	else {std::cout << "*********************Parent Proces***************** " << std::endl;}
	  
	
	std::cout << "Hello after call to fork" << std::endl;
	std::cout << "Process pid " << pid << std::endl;
	return 0;

}


<span style="color:rgb(0, 112, 192)">Understand the output of above program </span>


<span style="color:rgb(192, 0, 0)">
Now next part of discussion is does "exec" creates a new process ,  answer is big NO.
"exec" replaces the code,data,heap and stack of the current process with that of new program.
</span>



Threading Issues --: 

1. Fork is used for duplicating a process 
2. exec 