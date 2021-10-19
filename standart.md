# CJL Language
![image](https://user-images.githubusercontent.com/91914454/137986186-43a28636-42b9-4750-aa90-6805c9058f01.png)

## CJL language features
* using of `_x` suffixies
* strong type convertions
* strong out of range control
* unique references and copies of references
* no `while` and `do while`
* additional debug friendly types, simple signed extention
* simple lambdas
* all is a class, classes as single files
* **safity is a priority!**

## Primitives
```C
sint64  // signed 63 bit integers and one sign bit
uint64  // unsigned 63 bit integers and one zero bit, which is ignored
utf8    // utf8 chars
dbl64     // double

// optional
word64      // words of utf8 chars
predicate8  // "bool" size of 8 bits
```

## Some constants
```
errptr = #0x0;
something = 1;
nothing = 0;
```

## Some rules
**No unintialized data! Or convention of default values.**
```
uint64 a; // error or warning: set to 0
uint64 b = 0; // correct
```
`this->a` or `this.a` change to `a_m` \
*Optionally*
```
if (a == 0) {} // incorrect
if (0 == a) {} // correct
```

## Lambdas
```
uint64 result = uint64_l(&uint64_1 + uint64_3 - *uint64_2); // body contains only type and ordinal of params
result(2, 3, 4); // 3
```
## Complex types
```
[]T a; // array of objects with type T with safe idx!!!
class T; // class T
struct a; // error

// optitional
<>T a; // list of objects with type T
```

Example of using array
```
[]uint64 arrA = {1, 2, 3}; // correct
[100]uint64 arrB; // correct


// resizing
[10]uint64 arrC;
arrC[4] = 5; // correct
[20]arrC; // correct, resize to 20
arrC[19] = 5; // correct
arrC[20] = 6; // error
```

## type convertions
```
uint64 a = 2;
a -= 3; // result -1

sint64 b = -3;
uint64 c = b; // result 3

10 // uint64
+10 // sint64
10. // dbl
10 + 10. // error
10. + 10. // correct

uint64 x = 0;
sint64 y = +x; // correct
sint64 z = x; // incorrect
```

```
// def uint a and uint b
if (a - b < 3) {}

dbl num = 3. _/ 5.; // using of special div for doubles

[]utf8 array = "Hello CJL!"; // correct
word64 w = 'Hello)))'; // correct

uint64 a = w; // correct
sint64 b = +w; // correct
```

## adderesses & referenses
```
&T ref // unique reference ref to object of type T (unique, copiable, non-movable)
*T ref_copy // copy of some reference of type T
#0x7fb700000000 // addr of type uint64

&[]T ref // unique reference to array of objects of type a
```

### examples:
using of `&` and `*`
```
// def class Array
Array arrA;
Array arrB;
&Array a = &arrA; // correct
&Array b = &arrB; // correct
a[0] = 1;
a = b; // correct
&a = &b; // incorrect (non-movable)

*Array smth = errptr; // correct
*Array smth = #0x1; // correct
*Array copy = &arrA; // correct

&Array f; // incorrect, initialization is needed!
&Array f = #0x1; // incorrect, addresses can be inited with * only
&Array e = &arrB; // incorrect, &arrB is already in b


copy[3] = 2;
```

## `&` and `*` validation 
```
&Array a;
if (&a) {} // always true
if (*a) {} // true if valid (not errptr)
```

erasing `&` and `*` in other scope
```
void foo(*Array a, &Array b) {
	erase(a); // incorrect
	erase(*a) // correct
	erase(&b) // incorrect
}

uint64 main() {
    Array A;
    Array B;
	&Array x = &A;
	&Array y = &B;
	
	foo(x, y);
	
	erase(&x) // correct
	erase(&y) // correct
	erase(*x) // incorrect (no copy of x in current scope)
}
```

## Loops
```
for (<idx for _start or start_condition>; <idx for _end or end_condition>; <operation for _next>) {
	
}

do {
	...
} for (...);
```
**NO WHILE and DO WHILE**

examples:
```
uint64 a = 0;
for (0; 10; 1) { // for (_i = 0; i < 10; ++i)
	a *= _i; // _i - default enumerator
}

for (; 10; ) { // for (_i = 0; i < 10; ++i)
	a *= _i;
}

for (; 10; _i*2) {
	a += _i;
}


ComplexType arr;
for (): _i = arr.enumerator() { // for (_i=arr.enumerator().start(); _i < arr.enumerator().end(); arr.enumerator().next())
	arr[_i] = 1;
}

// while loop
for () until (a < 100) {
	a = 2 * _i;
}

// also
Array smth;
for (-3; ; _i+5) until (smth[_i] > 10): _i = +_i {
	a = 2 * _i;
}
```

## Class inheritance
**Same as C++ with elements of Java**
fileA.cls:
```
public class A {
public:
    uint64 a = 0;
    [10]utf8 classname = "" property; // property means static in Java
    void method();
    void work() const;
protected:
...
private:
...
};
```
fileB.cls:
```
public class B by A {
...
}
```

* `override` is same as C++
* class modifiers - after method/field expression
* every class in a single file



