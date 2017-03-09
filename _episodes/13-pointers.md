---
title: "Pointers"
teaching: 15
exercises: 15
questions:
objectives:
keypoints:
---
In earlier chapters, variables have been explained as locations in the computer's memory which can be accessed by their identifier 
(their name). This way, the program does not need to care about the physical address of the data in memory as it simply uses the 
identifier whenever it needs to refer to the variable.

For a C++ program, the memory of a computer is like a succession of single-byte memory cells, each with a unique address. 
These memory cells are ordered in a way that allows data representations larger than one byte to occupy memory cells that 
have consecutive addresses. This ordering enables each cell to be easily located in the memory by means of its unique address. 
For example, the memory cell with the address 1776 always follows immediately after the cell with address 1775 and precedes 
the one with 1777, and is exactly one thousand cells after 776 and exactly one thousand cells before 2776.

When a variable is declared, the memory needed to store its value is assigned a specific location in memory, know as its *address*. 
Generally, C++ programs do not actively decide the exact memory addresses where its variables are stored. That task 
is left to the environment where the program is run - generally, an operating system that decides the particular memory 
locations at run-time. However, it may be useful for a program to be able to obtain the address of a variable during run-time 
in order to access data cells that are at a certain position relative to it.

### Address-of operator `&`

The memory address of a variable can be obtained by preceding the name of a variable with an ampersand sign `&`, known as an *address-of*
operator. For example: 

~~~
foo = &myvar;
~~~
{: .code}

This statement will assign the address of variable `myvar` to `foo`. By preceding the name of the variable `myvar` with the 
ampersand, we are no longer assigning the content of the variable itself to `foo`, but rather its memory address.

Let's assume that `myvar` is placed at run-time in the memory address 1776.

In this case, consider the following code fragment:

~~~
myvar = 25;
foo = &myvar;
bar = myvar;
~~~
{: .code}

First, we have assigned the value 25 to `myvar` (a variable whose address in memory we assumed to be 1776).

The second statement assigns the *address* of `myvar` (1776), rather than the *value* of `myvar`, to `foo`.

Finally, the third statement assigns the value contained in `myvar` to `bar`. This is a normal assignment operation.

A variable that stores the address of another variable, `foo` in this example, is called a *pointer*. Pointers are a very powerful 
feature of the language that has many uses in lower level programming. We will see how to declare and use pointers later.

### Dereference operator `*`

We've seen how to use a pointer to store the address of another variable. Pointers are named that way because they "point to" the variable 
whose address they store.

An interesting property of pointers is that they can be also be used to access the value of the variable they point to. This is done by 
preceding the pointer name with the dereference operator `*`. The operator itself can be read as "value pointed to by".

The following statement sets the variable baz equal to the "value pointed to by foo":

~~~
baz = *foo;
~~~
{: .code}

This statement would actually assign the value 25 to `baz`, since `foo` is 1776, and the value stored at address 1776 (using the example above) 
would be 25.

It is important to clearly differentiate that `foo` refers to the value 1776, while `*foo` `refers to the value stored at address 1776, 
which in this case is 25. Notice the difference of including or not including the dereference operator: 

~~~
baz = foo;   // baz equal to foo (1776)
baz = *foo;  // baz equal to value pointed to by foo (25)  
~~~
{: .code}

The reference and dereference operators are thus complementary:

- `&` is the address-of operator, and can be read simply as "address of"
- `*` is the dereference operator, and can be read as "value pointed to by"

Earlier, we performed the following two assignment operations:

~~~
myvar = 25;
foo = &myvar;
~~~
{: .code}

Right after these two statements, all of the following expressions would give true as result:

~~~
myvar == 25
&myvar == 1776
foo == 1776
*foo == 25
*foo == myvar
~~~
{: .code}

The first expression is quite clear, considering that the assignment operation performed on `myvar` was `myvar = 25`. The second 
uses the address-of operator `&`, which returns the address of `myvar`, which we assumed to have a value of 1776. The third is 
somewhat obvious, since the second expression was true and the assignment operation performed on `foo` was `foo = &myvar`. 
The fourth expression uses the dereference operator `*` to obtain the value pointed to by `foo`, and is indeed 25.
The final expression is true as long as the address pointed to by `foo` is the address of `myvar`.

### Declaring pointers

Since a pointer can refer to the value of the variable that it points to (by dereferencing), a pointer has to know the type of 
variable it is pointing to. It is not sufficient to just know that it points to an anonymous memory address. This means that
the declaration of the pointer must include the type of the data it is pointing to.

The declaration of pointers follows this syntax:

~~~
type * name; 
~~~
{: .code}

In the declaration, `type` is the data type pointed to by the pointer. This is not the type of the pointer itself, but the type of the 
data the pointer points to. For example:

~~~
int *number;
char *character;
double *decimals;
~~~
{: .code}

These are three declarations of pointers. Although each points to a different data type, they are 
pointers, and all use the same amount of space in memory (the size in memory of a pointer 
depends on the platform where the program runs). However, the data to which they point to does not occupy the same 
amount of space, nor are they of the same type.

Note that the asterisk `*` used when declaring a pointer should not be confused with the dereference operator. 

Let's see an example of using pointers:

~~~
// my first pointer
#include <iostream>
using namespace std;

int main()
{
  int firstvalue, secondvalue;
  int * mypointer;

  mypointer = &firstvalue;
  *mypointer = 10;
  mypointer = &secondvalue;
  *mypointer = 20;
  cout << "firstvalue is " << firstvalue << endl;
  cout << "secondvalue is " << secondvalue << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub1"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub1').value = `// my first pointer
#include <iostream>
using namespace std;

int main()
{
  int firstvalue, secondvalue;
  int *mypointer;

  mypointer = &firstvalue;
  *mypointer = 10;
  mypointer = &secondvalue;
  *mypointer = 20;
  cout << "firstvalue is " << firstvalue << endl;
  cout << "secondvalue is " << secondvalue << endl;
  return 0;
}
`;
</script>
</form>
<br>

This produces the following output:

~~~
firstvalue is 10
secondvalue is 20
~~~
{: .output}


Notice that `firstvalue` and `secondvalue` are never directly set in the program, however both end up with a value.
The program works by setting `mypointer` to the address of `firstvalue` and then the value pointed 
to by `mypointer` is assigned a value of 10. Because `mypointer` is pointing to the memory location of `firstvalue`, 
it is actually the value of `firstvalue` that is changed. `mypointer` is then set to the address of `secondvalue`
and the process repeated, resulting in `secondvalue` containing 20.

Here is slightly more elaborate example:

~~~
// more pointers
#include <iostream>
using namespace std;

int main()
{
  int firstvalue = 5, secondvalue = 15;
  int * p1, * p2;

  p1 = &firstvalue;  // p1 = address of firstvalue
  p2 = &secondvalue; // p2 = address of secondvalue
  *p1 = 10;          // value pointed to by p1 = 10
  *p2 = *p1;         // value pointed to by p2 = value pointed to by p1
  p1 = p2;           // p1 = p2 (value of pointer is copied)
  *p1 = 20;          // value pointed to by p1 = 20
  
  cout << "firstvalue is " << firstvalue << endl;
  cout << "secondvalue is " << secondvalue << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub2"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub2').value = `// more pointers
#include <iostream>
using namespace std;

int main()
{
  int firstvalue = 5, secondvalue = 15;
  int *p1, *p2;

  p1 = &firstvalue;  // p1 = address of firstvalue
  p2 = &secondvalue; // p2 = address of secondvalue
  *p1 = 10;          // value pointed to by p1 = 10
  *p2 = *p1;         // value pointed to by p2 = value pointed to by p1
  p1 = p2;           // p1 = p2 (value of pointer is copied)
  *p1 = 20;          // value pointed to by p1 = 20
  
  cout << "firstvalue is " << firstvalue << endl;
  cout << "secondvalue is " << secondvalue << endl;
  return 0;
}
`;
</script>
</form>
<br>

The output from this program is:

~~~
firstvalue is 10
secondvalue is 20
~~~
{: .output}

Notice the follow line: 

~~~
int *p1, *p2;
~~~
{: .code}

This declares the two pointers `p1` and `p2` on a single line, but requires a `*` for each variable to be a pointer.
If, instead, the code was:

~~~
int *p1, p2;
~~~
{: .code}

You would end up with one pointer `p1`, nut`p2` would be of type `int`. Spaces do not matter at all for this purpose. 

To avoid this kind of ambiguity, it is good practice to declare one variable per line:

~~~
int *p1;
int *p2;
~~~
{: .code}

### Pointers and arrays

Arrays and pointers are closely related. An array can always be implicitly converted to the pointer of the proper type. 
For example, consider these two declarations:

~~~
int myarray[20];
int *mypointer;
~~~
{: .code}

The following assignment operation would be valid: 

~~~
mypointer = myarray;
~~~
{: .code}

After executing this statement, `mypointer` and `myarray` would be equivalent and in fact have very similar properties. The `mypointer`
variable can be used to reference array elements, and the `myarray` variable can be deferenced as a pointer.
In fact, the only difference is that `mypointer` can be assigned a value, while `myarray` can not, so the following assignment 
it not allowed:

~~~
myarray = mypointer;
~~~
{: .code}

Let's see an example that mixes arrays and pointers:

~~~
// more pointers
#include <iostream>
using namespace std;

int main()
{
  int numbers[5];
  int *p;
  p = numbers;
  *p = 10;
  p++;  
  *p = 20;
  p = &numbers[2];  
  *p = 30;
  p = numbers + 3;  
  *p = 40;
  p = numbers;  
  *(p+4) = 50;
  for (int n=0; n<5; n++)
    cout << numbers[n] << ", ";
  cout << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub3"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub3').value = `// more pointers
#include <iostream>
using namespace std;

int main()
{
  int numbers[5];
  int *p;
  p = numbers;
  *p = 10;
  p++;  
  *p = 20;
  p = &numbers[2];  
  *p = 30;
  p = numbers + 3;  
  *p = 40;
  p = numbers;  
  *(p+4) = 50;
  for (int n=0; n<5; n++)
    cout << numbers[n] << ", ";
  cout << endl;
  return 0;
}
`;
</script>
</form>
<br>

The output is:

~~~
10, 20, 30, 40, 50, 
~~~
{: .output}

Recall that brackets `[]` are used to specify the index of an element of the array. These brackets are actually a dereferencing operator 
known as *offset operator*. This operator dereferences a variable just as `*` does, but 
also adds the number between brackets to the address being dereferenced. For example:

~~~
a[5] = 0;       // a [offset of 5] = 0
*(a+5) = 0;     // pointed to by (a+5) = 0  
~~~
{: .code}

These two expressions are equivalent, and `a` can be either a pointer or an array.

### Pointer initialization

Pointers can be initialized to point to specific locations when they are defined:

~~~
int myvar;
int *myptr = &myvar;
~~~
{: .code}

The resulting state of variables after this code is the same as after the following:

~~~
int myvar;
int * myptr;
myptr = &myvar;
~~~
{: .code}

Pointers can be initialized either to the address of a variable (such as in the case above), or to the value of another pointer (or array):

~~~
int myvar;
int *foo = &myvar;
int *bar = foo;
~~~
{: .code}

### Pointer arithmetic

Addition and subtraction operations on pointers work slightly differently than regular integer types, as it depends on
the size of the data type to which they point.

We can see this behavior using the following example which defines three pointers to types of different sizes: 

~~~
char *mychar;
short *myshort;
long *mylong;
~~~
{: .code}

Let's suppose they point to the memory locations 1000, 2000, and 3000, respectively, and execute the following statements:

~~~
++mychar;
++myshort;
++mylong;
~~~
{: .code}

We would find that `mychar` contains the value 1001, `myshort` contains the value 2002, and `mylong` contains the value 3004, even though 
they each were incremented only once. The reason is that, when adding one to a pointer, the pointer is made to point to the following 
element *of the same type*. To achieve this, the size (in bytes) of the type it points to is added to the pointer.

This is applicable both when adding and subtracting pointers with any number. 

The increment `++` and decrement `--` operators, are commonly used with pointers as the provide a convenient way of moving the pointer
to the next or previous value respectively. Recall that they can be used either as a prefix or a suffix of an expression. When used with
pointers, this can have a subtle difference, as the result of the prefix operation is the new value, while the result of the suffix operation
is the previous value.

The following operations are equivalent:

~~~
*p++
*(p++)
~~~
{: .code}

This operation will increase the value of p (so it now points to the next element), but because `++` is used in suffix form, the expression 
`p++` evaluates to the value pointed to before being incremented. This means that `*p++` references the location before the increment.

Essentially, these are the four possible combinations of the dereference operator with both the prefix and suffix versions of the 
increment operator (the same applies to the decrement operator):

~~~
*p++   // same as *(p++): increment pointer, and dereference unincremented address
*++p   // same as *(++p): increment pointer, and dereference incremented address
++*p   // same as ++(*p): dereference pointer, and increment the value it points to
(*p)++ // dereference pointer, and post-increment the value it points to 
~~~
{: .code}

Note that parenthesis are required in the last case as the `++` (and `--`) operators have higher precendence than `*`, so they get applied
to the expression first.

Muliple operators can be combined into a single statement like the following:

~~~
*p++ = *q++;
~~~
{: .code}

The value assigned to *p is *q before both p and q are incremented, then both are incremented. It is equivalent to:

~~~
*p = *q;
++p;
++q;
~~~
{: .code}

### Pointers and const

A pointer can be used to access a variable by its address, and this access may include modifying the variable value. Sometimes it is
useful to be able to declare pointers that can read a value, but not modify it, and this is achieved by qualifying the declaration
with `const`. For example:

~~~
int x;
int y = 10;
const int *p = &y;
x = *p;          // ok: reading p
*p = x;          // error: modifying p, which is const-qualified 
~~~
{: .code}

Here `p` points to a variable, but points to it in a const-qualified manner, which only permits read access to the value.
The expression `&y` is of type `int*` (pointer to an `int`), but this is assigned to a pointer of type `const int*`. This is allowed
as a pointer to non-`const` can be implicitly converted to a pointer to `const`. The reverse is not permitted as this would
allow the constant value to be modified.

A common use of pointers to `const` elements is as function parameters. Without this, a function that takes a pointer as 
a parameter can modify the value passed as an argument. Declaring the parameter as `const` prevents it from being modified
(accidentally or otherwise).

~~~
// pointers as arguments:
#include <iostream>
using namespace std;

void increment_all(int* start, int* stop)
{
  int *current = start;
  while (current != stop) {
    ++(*current);  // increment value pointed
    ++current;     // increment pointer
  }
}

void print_all(const int* start, const int* stop)
{
  const int *current = start;
  while (current != stop) {
    cout << *current << endl;
    ++current;     // increment pointer
  }
}

int main()
{
  int numbers[] = {10,20,30};
  increment_all(numbers,numbers+3);
  print_all(numbers,numbers+3);
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub4"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub4').value = `// pointers as arguments:
#include <iostream>
using namespace std;

void increment_all(int* start, int* stop)
{
  int *current = start;
  while (current != stop) {
    ++(*current);  // increment value pointed
    ++current;     // increment pointer
  }
}

void print_all(const int* start, const int* stop)
{
  const int *current = start;
  while (current != stop) {
    cout << *current << endl;
    ++current;     // increment pointer
  }
}

int main()
{
  int numbers[] = {10,20,30};
  increment_all(numbers,numbers+3);
  print_all(numbers,numbers+3);
  return 0;
}
`;
</script>
</form>
<br>

The following output is generated by this program:

~~~
11
21
31
~~~
{: .output}

In this example, `print_all` uses pointers that point to constant values. These pointers are not constant, so the pointers themselves
can still be incremented or assigned different addresses.

In addition to pointing to constant values, it is also possible to declare the pointers as constant. The following code shows the
different combinations for using `const`:

~~~
int x;
int *p1 = &x;  // non-const pointer to non-const int
const int *p2 = &x;  // non-const pointer to const int
int * const p3 = &x;  // const pointer to non-const int
const int * const p4 = &x;  // const pointer to const int 
~~~
{: .code}

The `const` qualifier can either precede or follow the pointed to type:

~~~
const int * p2a = &x;  //      non-const pointer to const int
int const * p2b = &x;  // also non-const pointer to const int 
~~~
{: .code}

### Pointers and string literals

String literals (enclosed in quotes `"`) are essentially arrays containing null-terminated character sequences. The type
of a string literal is an array of `const char` (as the elements of the literal cannot be modified).

For example:

~~~ 
const char * mystring = "hello"; 
~~~
{: .code}

This declares an array with the literal representation for `"hello"`, and `mystring` is assigned a pointer to its first element.
The pointer `mystring` points to an array of characters, and because pointers and arrays behave essentially in the same way 
in expressions, `mystring` can be used to access the characters in the same way arrays of null-terminated character sequences are. 

For example:

~~~
*(mystring+4)
mystring[4]
~~~
{: .code}


Both expressions have a value of `'o'` (the fifth element of the array).

### Pointers to pointers

C++ allows the use of pointers that point to other pointers, that in turn, point to data (or even to other pointers). 
The syntax simply requires an asterisk `*` for each level of indirection in the declaration of the pointer:

~~~
char a;
char *b;
char **c;
a = 'z';
b = &a;
c = &b;
~~~
{: .code}

This, assuming the randomly chosen memory locations for each variable of 7230, 8092, and 10502, could be represented as:

 
With the value of each variable represented inside its corresponding cell, and their respective addresses in memory represented by the 
value under them.

The new thing in this example is variable c, which is a pointer to a pointer, and can be used in three different levels of indirection, 
each one of them would correspond to a different value:

* `c` is of type `char**` and a value of 8092
* `*c` is of type `char*` and a value of 7230
* `**c` is of type `char` and a value of 'z'

### `void` pointers

The `void` type of pointer is a special type of pointer. In C++, `void` represents the absence of type. Therefore, `void` pointers 
are pointers that point to a value that has no type (and thus also an undetermined length and undetermined dereferencing properties).

This gives `void` pointers a great flexibility, by being able to point to any data type, from an integer value or a float to a 
string of characters. In exchange, they have a great limitation: the data pointed to by them cannot be directly dereferenced 
(which is logical, since we have no type to dereference to), and for that reason, any address in a void pointer needs to be 
transformed into some other pointer type that points to a concrete data type before being dereferenced.

One of its possible uses may be to pass generic parameters to a function. For example: 

~~~
// increaser
#include <iostream>
using namespace std;

void increase (void* data, int psize)
{
  if ( psize == sizeof(char) )
  { char* pchar; pchar=(char*)data; ++(*pchar); }
  else if (psize == sizeof(int) )
  { int* pint; pint=(int*)data; ++(*pint); }
}

int main ()
{
  char a = 'x';
  int b = 1602;
  increase (&a,sizeof(a));
  increase (&b,sizeof(b));
  cout << a << ", " << b << endl;
  return 0;
}
~~~
{: .code}

~~~
y, 1603
~~~
{: .output}


The `sizeof` operator is integrated in the C++ language that returns the size in bytes of its argument. For non-dynamic data types, this value 
is a constant. Therefore, for example, sizeof(char) is 1, because char has always a size of one byte. 

### Invalid pointers and null pointers

In principle, pointers are meant to point to valid addresses, such as the address of a variable or the address of an element in an array. 
But pointers can actually point to any address, including addresses that do not refer to any valid element. Typical examples of this are 
uninitialized pointers and pointers to nonexistent elements of an array:

~~~
int * p;               // uninitialized pointer (local variable)

int myarray[10];
int * q = myarray+20;  // element out of bounds 
~~~
{: .code}

Neither `p` nor `q` point to addresses known to contain a value, but none of the above statements causes an error. In C++, pointers are 
allowed to take any address value, no matter whether there actually is something at that address or not. What can cause an error is to 
dereference such a pointer (i.e., actually accessing the value they point to). Accessing such a pointer causes undefined behavior, ranging 
from an error during runtime to accessing some random value.

But, sometimes, a pointer really needs to explicitly point to nowhere, and not just an invalid address. For such cases, there exists a 
special value that any pointer type can take: the null pointer value. This value can be expressed in C++ in two ways: either with an 
integer value of zero, or with the nullptr keyword:

~~~
int * p = 0;
int * q = nullptr;
~~~
{: .code}

Here, both `p` and `q` are null pointers, meaning that they explicitly point to nowhere, and they both actually compare equal: all 
null pointers compare equal to other null pointers. It is also quite usual to see the defined constant `NULL` be used in older code to 
refer to the null pointer value:

~~~
int * r = NULL;
~~~
{: .code}

`NULL` is defined in several headers of the standard library, and is defined as an alias of some null pointer constant value (such as 0 or nullptr).

Do not confuse null pointers with void pointers! A null pointer is a value that any pointer can take to represent that it is pointing to 
"nowhere", while a void pointer is a type of pointer that can point to somewhere without a specific type. One refers to the value stored in the 
pointer, and the other to the type of data it points to.

### Pointers to functions

C++ allows operations with pointers to functions. The typical use of this is for passing a function as an argument to another function. 
Pointers to functions are declared with the same syntax as a regular function declaration, except that the name of the function is enclosed 
between parentheses () and an asterisk (*) is inserted before the name:

~~~
// pointer to functions
#include <iostream>
using namespace std;

int addition (int a, int b)
{ return (a+b); }

int subtraction (int a, int b)
{ return (a-b); }

int operation (int x, int y, int (*functocall)(int,int))
{
  int g;
  g = (*functocall)(x,y);
  return (g);
}

int main ()
{
  int m,n;
  int (*minus)(int,int) = subtraction;

  m = operation (7, 5, addition);
  n = operation (20, m, minus);
  cout <<n;
  return 0;
}
~~~
{: .code}

~~~
8
~~~
{: .output}

In the example above, minus is a pointer to a function that has two parameters of type int. It is directly initialized to point to the 
function subtraction:

~~~
int (* minus)(int,int) = subtraction;
~~~
{: .code}

(note: both c_str and data members of string are equivalent)

