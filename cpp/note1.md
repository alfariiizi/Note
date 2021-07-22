# Note about C++.

It could be just my thought, information that I found on internet, or maybe just other stuffs that I think that's fun 😂.


## Default arguments should be put on header file class or the cpp ?

source: [Default Paramter](default-arguments-hpp-cpp/c++%20-%20default%20parameters%20in%20.h%20and%20.cpp%20files%20-%20Stack%20Overflow.html)

Inti dari jawabannya adalah:
> This only works because your main function is also in your test.cpp file, so it sees the default argument specified in your class' implementation. If you put your main function in a separate file that only includes test.h, this code will not compile.
> Another way of looking at it is, when some other includes test.h, all that code sees is what is declared in test.h, so default arguments put elsewhere will not be used.


## How to pass the C function pointer from member function.

source: [C function pointer to member function](https://stackoverflow.com/questions/7676971/pointing-to-a-function-that-is-a-class-member-glfw-setkeycallback)
alternative: [C function pointer to member function](c++%20-%20Pointing%20to%20a%20function%20that%20is%20a%20class%20member%20-%20glfw%20setKeycallback%20-%20Stack%20Overflow.html)

Ada beberapa solusi yang dapat diterapkan