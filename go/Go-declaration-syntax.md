# Go's Declaration Syntax

Note taker: Alfarizi.

From: [Go's Decalration Syntax by Rob Pike](https://blog.golang.org/declaration-syntax)


## C Syntax

```C
// Variable
int x;

// Pointer variable
int *p;

// Array
int a[3];

// ..+ Function +..
// - Original C's function
int main( argc, argv )
    int argc;
    char* argv[];
{ /*somecode*/ }
// - Modern C's function
int main( int argc, char* argv[] ) { /*somecode*/ }
// - More simpler (if we are not going to accept any option)
int main( int, char*[] ) { /*somecode*/ }

// ..+ Function pointer +..
// - Easy level
int (*fp)(int a, int b);
// - Hard level: func pointer fp has parameter func pointer ff and integer b.
int (*fp)(int (*ff)(int x, int y), int b);
// - - More simpler: drop the parameter name
int (*fp)( int (*)(int, int), int );
// - Hard level: from example 2, we change return type from int to func pointer
int (*(fp)(int(*)(int, int), int))(int, int) // wtf
```

Note that:
- Function pointer on 2nd Hard level above has syntax that very hard to read. That's why C casting is `(int)M_PI` instead of `int(M_PI)`.
- C's read in a spiral. See [The "Clockwise/Spiral Rule" by David Anderson](http://c-faq.com/decl/spiral.anderson.html)


## Go Syntax
```Go
// Variable
x: int

// Pointer
p *int

// Array
a [3]int

// ..+ Function +..
func main( argc int, argv []string ) int { /*somecode*/ }
// - More simpler
func main( int, []string ) int { /*somecode*/ }

// ..+ Function variable (function pointer) +..
// - f takes 2 paramter (func(int, int), int, and int) then return an integer.
f func( func(int, int) ) int, int ) int
// - like above, but f return a function
f func( func(int, int) ) int, int ) func(int, int) int
// - it's kinda lamda. The sum will be 3 + 5 = 8.
sum := func(a, b int) int { return a + b } (3, 5)
```

### Go Pointer

The declaration at first is kinda reverse from C. But in further use, it act pretty similar with C.

```Go
// Array and slices
var a[] int // like reverse C
x = a[1]    // like C

// Valid pointer
var p *int  // like reverse C
x = *p      // like C

// Invalid pointer
var p *int
x = p*      // it should be *p
```