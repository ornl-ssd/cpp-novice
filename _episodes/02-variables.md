---
title: "Variables and Types"
teaching: 20
exercises: 0
questions:
- "What fundamental types are available?"
- "How are variables declared and initialized?"
objectives:
- "Learn about the different data types available."
- "Practice declaring and initializing variables."
keypoints:
- "The type of each variable must be declared."
- "The fundamental data types available in C++ closely match the native system types."
- "Variables can be initialized at the time they are declared."
---
In C++, like in many other languages, data must be stored during program execution in *variables*. A C++ variable maps very
closely to the hardware of the computer that the program is running on. In fact, a variable is really just a name given to
a memory location at which the data is being stored. This concept differs significantly from other languages, such as Python.

### Identifiers

The name of a variable is known as an *identifier*. A valid identifier is a sequence of one or more letters, digits, or 
underscore characters `_`. Spaces, punctuation marks, and symbols cannot be part of an identifier. 

Identifiers must always begin with a letter or an underscore. The use of an underscore as the first character of an
identifier is generally considered to have a special meaning.

Identifiers are also used for things other than variable names. C++ uses many *keywords* as part of the language to identify 
operations and data descriptions. Identifiers created by a programmer cannot match these keywords. The standard reserved 
keywords are:

`alignas, alignof, and, and_eq, asm, auto, bitand, bitor, bool, break, case, catch, char, char16_t, char32_t, class, compl, const, 
constexpr, const_cast, continue, decltype, default, delete, do, double, dynamic_cast, else, enum, explicit, export, extern, false, 
float, for, friend, goto, if, inline, int, long, mutable, namespace, new, noexcept, not, not_eq, nullptr, operator, or, or_eq, private, 
protected, public, register, reinterpret_cast, return, short, signed, sizeof, static, static_assert, static_cast, struct, switch, 
template, this, thread_local, throw, true, try, typedef, typeid, typename, union, unsigned, using, virtual, void, volatile, wchar_t, 
while, xor, xor_eq`

Specific compilers may also have additional specific reserved keywords. Consult your compiler documentation if you are unsure.

**Very important**: The C++ language is a "case sensitive" language. That means that an identifier written in uppercase letters is not 
equivalent to another one with the same name but written in lowercase letters. Thus, for example, the `RESULT` variable is not the same 
as the `result` variable or the `Result` variable. These are three different identifiers referring to three different variables.

### Fundamental data types

The values of variables are stored somewhere in an unspecified location in the computer memory as strings of zeros and ones. A program does 
not need to know the exact location where a variable is stored since it can simply refer to it by its name. 

The program *does* need to understand the kind of data stored in the variable. Integers, floating point numbers, and characters are all
stored in very different formats, usually occupying different amounts of memory. The program must know how the data is stored so
that it can interpret it correctly.

Fundamental data types are basic types implemented directly by the language that represent the basic storage units supported natively 
by most systems. They can mainly be classified into:

Character types
: They can represent a single character, such as `A` or `$`. The most basic type is `char`, which is a one-byte character. 
Other types are also provided for wider characters.

Numerical integer types
: They can store a whole number value, such as 7 or 1024. They exist in a variety of sizes, and can either be signed or 
unsigned, depending on whether they support negative values or not.

Floating-point types
: They can represent real values, such as 3.14 or 0.01, with different levels of precision, depending on which of the three 
floating-point types is used.

Boolean type
: The boolean type, known in C++ as `bool`, can only represent one of two states, `true` or `false`.

Here is the complete list of fundamental types in C++:

<table border="1">
<tr>
<th>Group</th>
<th>Type names*</th>
<th>Notes on size / precision</th>
</tr>
<tr>
<td rowspan="4">Character types</td>
<td>char</td>
<td>Exactly one byte in size. At least 8 bits.</td>
</tr>
<tr>
<td>char16_t</td>
<td>Not smaller than char. At least 16 bits.</td>
</tr>
<tr>
<td>char32_t</td>
<td>Not smaller than char16_t. At least 32 bits.</td>
</tr>
<tr>
<td>wchar_t</td>
<td>Can represent the largest supported character set.</td>
</tr>
<tr>
<td rowspan="5">Integer types (signed)</td>
<td>signed char</td>
<td>Same size as char. At least 8 bits.</td>
</tr>
<tr>
<td><em>signed</em> short <em>int</em></td>
<td>Not smaller than char. At least 16 bits.</td>
</tr>
<tr>
<td><em>signed</em> int</td>
<td>Not smaller than short. At least 16 bits.</td>
</tr>
<tr>
<td><em>signed</em> long <em>int</em></td>
<td>Not smaller than int. At least 32 bits.</td>
</tr>
<tr>
<td><em>signed</em> long long <em>int</em></td>
<td>Not smaller than long. At least 64 bits.</td>
</tr>
<tr>
<td rowspan="5">Integer types (unsigned)</td>
<td>unsigned char</td>
<td rowspan="5">(same size as their signed counterparts)</td>
</tr>
<tr>
<td>unsigned short <em>int</em></td>
</tr>
<tr>
<td>unsigned <em>int</em></td>
</tr>
<tr>
<td>unsigned long <em>int</em></td>
</tr>
<tr>
<td>unsigned long long <em>int</em></td>
</tr>
<tr>
<td rowspan="3">Floating-point</td>
<td>float</td>
<td>32-bits</td>
</tr>
<tr>
<td>double</td>
<td>64-bits</td>
</tr>
<tr>
<td>long double</td>
<td>80-bits</td>
</tr>
<tr>
<td>Boolean type</td>
<td>bool</td>
<td></td>
</tr>
<tr>
<td>Void type</td>
<td>void</td>
<td>no storage</td>
</tr>
<tr>
<td>Null pointer</td>
<td>decltype(nullptr)</td>
<td></td>
</tr>
</table>

\* The names of certain integer types can be abbreviated without their `signed` and `int` components - only the part *not* in italics is 
required to identify the type, the part in italics is optional. For example, `signed short int` can be abbreviated as `signed short`, `short int`, 
or simply `short`; they all identify the same fundamental type.

Other than `char`, which has a size of one byte, the sizes of the other fundamental types are only recommended lower limits. In most
cases these will be the sizes used by the compiler, but developers should be aware that it doesn't necessarily have to be the case. 

The properties of fundamental types on a particular system and compiler implementation can be obtained by using the `numeric_limits`
classes (see standard header `<limits>`). If types of specific size are needed, certain fixed-size 
type aliases are available in header `<cstdint>`.

The types described above (characters, integers, floating-point, and boolean) are collectively known as *arithmetic types*. 

There are two additional fundamental types: 

* `void`, which identifies the absence of a type; and
* `nullptr`, which is a special type of pointer.

Both of these types will be discussed further in the lesson on [pointers]({{ page.root }}/13-pointers).

C++ supports a wide variety of types based on the fundamental types. These are known as *compound data types*, 
and are one of the main strengths of the C++ language. We will also see them in more detail in future lessons.

### Declaration of variables

C++ is a *strongly-typed* language, and requires every variable to be declared with its type before its first use. This informs the 
compiler the size to reserve in memory for the variable and how to interpret its value. The syntax for declaring a new variable in C++ 
is to write the type followed by the variable name (i.e., its identifier). For example:

~~~
int a;
float mynumber;
~~~
{: .code}

The first statement declares a variable of type `int` with the identifier `a`. 
The second statement declares a variable of type `float` with the identifier `mynumber`. Once declared, the variables `a` and `mynumber`
can be used within the rest of their scope in the program.
More than one variable of the same type can be declared in a single statement by separating their identifiers 
with commas. For example:

~~~
int a, b, c;
~~~
{: .code}

This declares three variables `a`, `b` and `c`, all of type `int`. It has exactly the same meaning as:

~~~
int a;
int b;
int c;
~~~
{: .code}

To see what variable declarations look like in action within a program, let's have a look at an example:

~~~
// operating with variables

#include <iostream>
using namespace std;

int main()
{
  // declaring variables:
  int a, b;
  int result;

  // process:
  a = 5;
  b = 2;
  a = a + 1;
  result = a - b;

  // print out the result:
  cout << result << endl;

  // terminate the program:
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub1"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub1').value = `// operating with variables

#include <iostream>
using namespace std;

int main()
{
  // declaring variables:
  int a, b;
  int result;

  // process:
  a = 5;
  b = 2;
  a = a + 1;
  result = a - b;

  // print out the result:
  cout << result << endl;

  // terminate the program:
  return 0;
}
`;
</script>
</form>
<br>

The main thing to notice is that the variables are declared before they are used anywhere else in the program. All three variables
are of type `int` so they can be used interchangeably in expressions.

### Initialization of variables

The variables in the example above have an indeterminate value when they are first declared. They only take on an actual value when the variable
is *assigned* for the first time in a statement such as `a = 5;`. 

However, it is possible for a variable to have a specific value from the moment it is declared. This is called the initialization of the variable.

In C++, there are three ways to initialize variables. The first one, known as *c-like initialization* (because it is inherited from the C language), 
consists of appending an equal sign followed by the value to which the variable is initialized. For example, to declare a variable of type `int`
called `x` and initialize it to a value of zero from the same moment it is declared, we can write:

~~~
int x = 0;
~~~
{: .code}

A second method, known as *constructor initialization* (introduced by the C++ language), encloses the initial value between parentheses.
For example:

~~~
int x(0);
~~~
{: .code}

Finally, a third method, known as *uniform initialization*, similar to the above, but using curly braces instead of parentheses 
(this was introduced by the revision of the C++ standard, in 2011). For example:

~~~
int x {0}; 
~~~
{: .code}

The following code shows how the different initialization formats are used:

~~~
// initialization of variables

#include <iostream>
using namespace std;

int main()
{
  int a=5;               // initial value: 5
  int b(3);              // initial value: 3
  int c{2};              // initial value: 2
  int result;            // initial value undetermined

  a = a + b;
  result = a - c;
  cout << result << endl;

  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub2"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub2').value = `// initialization of variables

#include <iostream>
using namespace std;

int main()
{
  int a=5;               // initial value: 5
  int b(3);              // initial value: 3
  int c{2};              // initial value: 2
  int result;            // initial value undetermined

  a = a + b;
  result = a - c;
  cout << result << endl;

  return 0;
}
`;
</script>
</form>
<br>

### Introduction to the `string` class

The `string` class is not a fundamential data type like `float` or `int`, but rather a *compound* type known as a class. Although there is
a fundamental string type that we consider later, the `string` class provides functionality that is closer to other languages such as Python.

In order to be able to use the `string` class, your program must include the header `<string>` as follows: 

~~~
// my first string
#include <iostream>
#include <string>
using namespace std;

int main()
{
  string mystring;
  mystring = "This is a string";
  cout << mystring << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub3"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub3').value = `// my first string
#include <iostream>
#include <string>
using namespace std;

int main()
{
  string mystring;
  mystring = "This is a string";
  cout << mystring << endl;
  return 0;
}
`;
</script>
</form>
<br>

As can been seen in this example, strings can be initialized using a *string literal*. Any of the intialization
formats can also be used:

~~~
string mystring = "This is a string";
string mystring ("This is a string");
string mystring {"This is a string"};
~~~
{: .code}

Variabls of type `string` can be used just like any fundamental type, however there are a large number of operations that 
are also available:

~~~
// my first string operations
#include <iostream>
#include <string>
using namespace std;

int main()
{
  string mystring;
  mystring = "This is the initial string content";
  cout << mystring << endl;
  mystring = "This is a different string content";
  cout << mystring << endl;
  cout << "The string is " << mystring.size() << " characters in length" << endl;
  mystring.append(", with another string appended");
  cout << mystring << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub4"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub4').value = `// my first string operations
#include <iostream>
#include <string>
using namespace std;

int main()
{
  string mystring;
  mystring = "This is the initial string content";
  cout << mystring << endl;
  mystring = "This is a different string content";
  cout << mystring << endl;
  cout << "The string is " << mystring.size() << " characters in length" << endl;
  mystring.append(", with another string appended");
  cout << mystring << endl;
  return 0;
}
`;
</script>
</form>
<br>

For more details on standard C++ strings, see the [string class reference](http://www.cplusplus.com/string).

## Advanced Topics

### Type deduction: `auto` and `decltype`

By declaring a variable of type `auto`, the compiler can work out what the type the variable should be from its initializer. This 
works for initializers that are constant or other variables:

~~~
int foo = 0;
auto bar = foo;  // the same as: int bar = foo; 
~~~
{: .code}

Because `bar` is declared as having an `auto` type, it takes its type from the initializer. In this 
case the initializer is `foo`, which is type `int`, so `bar` is also of type `int`.

The `decltype` specifier can be used to do a similar thing for variables that are not initialized:

~~~
int foo = 0;
decltype(foo) bar;  // the same as: int bar; 
~~~
{: .code}

Here, `bar` is declared as having the same type as `foo`.