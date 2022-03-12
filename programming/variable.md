# Variable


## Mutable and immutable variable

向function 传递arg时
对于mutable object (list, dict) 传递的是指针，即函数内层会影响外层参数
对于immutable object (int,float,str,tuple) 传递的是value不能因为不能修改，所以不会影响外层也


- [CSDN](https://blog.csdn.net/god_wen/article/details/78423621): definition of mutable and immutable variable.


## Global and local variable


- [W3School](https://www.w3schools.com/python/python_variables_global.asp): definition of global and local variable.
  - Variables that are created outside of a function (as in all of the examples above) are known as global variables.
  - Global variables can be used by everyone, both inside of functions and outside.
  - Normally, when you create a variable inside a function, that variable is local, and can only be used inside that function.
  - To create a global variable inside a function, you can use the global keyword.
  - Also, use the global keyword if you want to change a global variable inside a function.