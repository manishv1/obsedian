it must have an argument  passed as reference and return object as reference .


# Incorrect Syntax 

	MyClass operator = (MyClass &obj)
    {
		    MyClass temp;

			 return temp;	   
    }




<span style="color:rgb(255, 255, 0)">Temorary object binding to non const member function </span> 

Note the return value in the above assignment operator is a temp object. 

So when ever we call     x1 = x2  it is excuted as    x1.operator(x2) and values of x2 data members are assigned to the object x1 data members. 
Here return values have no mean so it work fine.

Now take the case of 

x1 = x2 =x3 

this will be executed as  x1 = (x2 = x3)

So what ever be the result of x2 = x3  is a temporary variable and it should be passed as parameter to 

x3.operator( return value of x2 = x3)

<span style="color:rgb(255, 0, 0)">As per the C++ rule temporary object binding to the non constant member function is not allowed.
</span>

<span style="color:rgb(255, 0, 0)">But if you return a reference from     


operator = ( MyClass &obj) {

}  

function , then no problem is faced </span>

So correct syntax is as follow.
# Correct Syntax 

MyClass & operator = (MyClass &obj)
{



}
