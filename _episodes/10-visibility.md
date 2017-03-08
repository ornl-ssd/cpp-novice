---
title: "Name Visibility"
teaching: 15
exercises: 15
questions:
objectives:
keypoints:
---
### Scopes

Named entities, such as variables, functions, and compound types need to be declared before being used in C++. The point in the program where 
this declaration happens influences its *visibility*. There are two types of visibility, or *scope*:

Global scope
: An entity declared outside any block has *global scope*, meaning that its name is valid anywhere in the code. 

Block scope
: An entity declared within a block, such as a function or a selective statement, has *block scope*, and is only visible within the 
specific block in which it is declared, but not outside it.

Variables with global scope are known as *global variables*, and variables with block scope are known as *local variables*.

For example, a variable declared in the body of a function is a local variable that extends until the end of the the function 
(i.e., until the brace } that closes the function definition), but not outside it:

~~~
int foo;        // global variable - outside any block

int some_function()
{
  int bar;      // local variable - inside function block
  bar = 0;
}

int other_function()
{
  foo = 1;  // ok: foo is a global variable
  bar = 2;  // wrong: bar is not visible from this function
}
~~~
{: .code}

In each scope, a name can only represent one entity. For example, there cannot be two variables with the same name in the same scope:

~~~
int some_function()
{
  int x;
  x = 0;
  double x;   // wrong: name already used in this scope
  x = 0.0;
}
~~~
{: .code}

The visibility of an entity with block scope extends until the end of the block, including inner blocks. Nevertheless, an inner block, 
because it is a different block, can re-use a name from an outer scope and make it refer to a different entity. When this is done, the name 
will refer to a different entity only within the inner block, hiding the entity it names outside. While outside the inner block, the name
will still refer to the original entity. 

For example:

~~~
// inner block scopes
#include <iostream>
using namespace std;

int main() {
  int x = 10;
  int y = 20;
  {
    int x;   // ok, inner scope.
    x = 50;  // sets value to inner x
    y = 50;  // sets value to (outer) y
    cout << "inner block:" << endl;
    cout << "x: " << x << endl;
    cout << "y: " << y << endl;
  }
  cout << "outer block:" << endl;
  cout << "x: " << x << endl;
  cout << "y: " << y << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub1"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub1').value = `// inner block scopes
#include <iostream>
using namespace std;

int main() {
  int x = 10;
  int y = 20;
  {
    int x;   // ok, inner scope.
    x = 50;  // sets value to inner x
    y = 50;  // sets value to (outer) y
    cout << "inner block:" << endl;
    cout << "x: " << x << endl;
    cout << "y: " << y << endl;
  }
  cout << "outer block:" << endl;
  cout << "x: " << x << endl;
  cout << "y: " << y << endl;
  return 0;
}
`;
</script>
</form>
<br>

Running this program results in:

~~~
inner block:
x: 50
y: 50
outer block:
x: 10
y: 50
~~~
{: .output}

Note that `y` is not hidden in the inner block, and thus accessing `y` still accesses the outer variable.

Variables in declarations that introduce a block, such as function parameters and variables declared in loops and conditions 
(such as those declared on a `for` or an `if`) are local to the block they introduce.

### Namespaces

Only one entity can exist with a particular name in a particular scope. This is seldom a problem for local names, since blocks tend to be 
relatively short. However, names with global scope are much more likely to result in collisions, particularly when used by libraries that may 
declare many functions, types, and variables.

*Namespaces* provide a mechanism to group named entities that otherwise would have global scope into narrower scopes, giving them *namespace scope*.
This provides a means of organizing the elements of programs into different logical scopes that can be referred to by names.

The syntax for declaring a namespaces is:

~~~
namespace identifier
{
  named_entities
}
~~~
{: .code}

Where `identifier` is any valid identifier and `named_entities` is the set of variables, types and functions that are included within 
the namespace. 

For example:

~~~
namespace myNamespace
{
  int a, b;
}
~~~
{: .code}

Here, the variables `a` and `b` are normal variables declared within a namespace called `myNamespace`. These variables can be accessed from 
within the namespace by using just their identifier (i.e. `a` or `b`), but if accessed from outside the namespace they have to be 
properly qualified with the scope operator `::`. For example, to access the previous variables from outside `myNamespace` they 
must be qualified as follows:

~~~
myNamespace::a
myNamespace::b 
~~~
{: .code}

Namespaces are particularly useful to avoid name collisions. For example:

~~~
// namespaces
#include <iostream>
using namespace std;

namespace foo
{
  int value() { return 5; }
}

namespace bar
{
  const double pi = 3.1416;
  double value() { return 2 * pi; }
}

int main () {
  cout << foo::value() << endl;
  cout << bar::value() << endl;
  cout << bar::pi << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub2"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub2').value = `// namespaces
#include <iostream>
using namespace std;

namespace foo
{
  int value() { return 5; }
}

namespace bar
{
  const double pi = 3.1416;
  double value() { return 2 * pi; }
}

int main () {
  cout << foo::value() << endl;
  cout << bar::value() << endl;
  cout << bar::pi << endl;
  return 0;
}
`;
</script>
</form>
<br>

Running this program results in:

~~~
5
6.2832
3.1416
~~~
{: .output}


In this case, there are two different functions with the same name, `value`. One function is defined within the namespace `foo`, and the other 
function in the namespace `bar`. Notice also how `pi` is accessed in an unqualified manner from within namespace `bar`, but when it is 
accessed in `main`, it must be qualified as `bar::pi`.

Namespaces do not need to be continuous. That is, two separate segments of a code can be declared to be in the same namespace as follows:

~~~
namespace foo { int a; }
namespace bar { int b; }
namespace foo { int c; }
~~~
{: .code}

This declares three variables: `a` and `c` are in namespace `foo`, while `b` is in namespace `bar`. 

Namespaces can even extend across different translation units (i.e., across different files of source code).

### The `using` keyword

The keyword `using` introduces a name from a namespace into the current declarative region (such as a block), thus avoiding the 
need to qualify the name.

For example:

~~~
// using
#include <iostream>
using namespace std;

namespace first
{
  int x = 5;
  int y = 10;
}

namespace second
{
  double x = 3.1416;
  double y = 2.7183;
}

int main () {
  using first::x;
  using second::y;
  cout << x << endl;
  cout << y << endl;
  cout << first::y << endl;
  cout << second::x << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub2"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub2').value = `// using
#include <iostream>
using namespace std;

namespace first
{
  int x = 5;
  int y = 10;
}

namespace second
{
  double x = 3.1416;
  double y = 2.7183;
}

int main () {
  using first::x;
  using second::y;
  cout << x << endl;
  cout << y << endl;
  cout << first::y << endl;
  cout << second::x << endl;
  return 0;
}
`;
</script>
</form>
<br>

The code produces the output:

~~~
5
2.7183
10
3.1416
~~~
{: .output}

The `using` statments in `main` cases the variable `x` (without any name qualifier) to refer to `first::x`, and `y` to refer to `second::y`.
The variables `first::y` and `second::x` can still be accessed, but require fully qualified names.

The keyword `using` can also be used as a directive to introduce an entire namespace:

~~~
// using
#include <iostream>
using namespace std;

namespace first
{
  int x = 5;
  int y = 10;
}

namespace second
{
  double x = 3.1416;
  double y = 2.7183;
}

int main () {
  using namespace first;
  cout << x << endl;
  cout << y << endl;
  cout << second::x << endl;
  cout << second::y << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub3"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub3').value = `// using
#include <iostream>
using namespace std;

namespace first
{
  int x = 5;
  int y = 10;
}

namespace second
{
  double x = 3.1416;
  double y = 2.7183;
}

int main () {
  using namespace first;
  cout << x << endl;
  cout << y << endl;
  cout << second::x << endl;
  cout << second::y << endl;
  return 0;
}
`;
</script>
</form>
<br>

This produces the output:

~~~
5
10
3.1416
2.7183
~~~
{: .code}

In this case, by declaring that we were `using namespace first`, all direct uses of `x` and `y` without name qualifiers 
will refer to variables in the local scope and in namespace `first`.

The `using` and `using namespace` keywords have validity only in the same block in which they are used or in the entire source code file if 
they are used in the global scope. For example, it is possible to first use the objects of one namespace and then those of another 
one by splitting the code in different blocks:

~~~
// using namespace example
#include <iostream>
using namespace std;

namespace first
{
  int x = 5;
}

namespace second
{
  double x = 3.1416;
}

int main () {
  {
    using namespace first;
    cout << x << endl;
  }
  {
    using namespace second;
    cout << x << endl;
  }
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub4"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub4').value = `// using namespace example
#include <iostream>
using namespace std;

namespace first
{
  int x = 5;
}

namespace second
{
  double x = 3.1416;
}

int main () {
  {
    using namespace first;
    cout << x << endl;
  }
  {
    using namespace second;
    cout << x << endl;
  }
  return 0;
}
`;
</script>
</form>
<br>

This produces the output:

~~~
5
3.1416
~~~
{: .code}

### Namespace aliasing

Existing namespaces can be aliased with new names, with the following syntax:

~~~
namespace new_name = current_name;
~~~
{: .code}

### The `std` namespace

All the entities (variables, types, constants, and functions) of the standard C++ library are declared within the `std` namespace. 
You have probably noticed that most examples in this tutorial include the following line in global scope:

~~~
using namespace std;
~~~
{: .code}

This introduces direct visibility of all the names of the `std` namespace into the code. This is done in these tutorials to 
facilitate comprehension and shorten the length of the examples, but many programmers prefer to qualify each of the elements of 
the standard library used in their programs. For example, instead of:

~~~
cout << "Hello world!";
~~~
{: .code}

It is common to instead see:

~~~ 
std::cout << "Hello world!";
~~~
{: .code}

Whether the elements in the std namespace are introduced with using declarations or are fully qualified on every use does not 
change the behavior or efficiency of the resulting program in any way. It is mostly a matter of style preference, although 
for projects mixing libraries, explicit qualification tends to be preferred.

### Storage classes

The storage for variables with global or namespace scope is allocated for the entire duration of the program. This is known as 
*static storage*, and it contrasts with the storage for local variables, which is known as *automatic storage*. The storage for 
local variables is only available during the block in which they are declared. Once the block is completed, the storage is freed and
made available of any other use.

Another difference between variables with static storage and variables with automatic storage is that the former case variables that
are not explicitly initialized are automatically initialized to zeroes. Variables with automatic storage that are not explicitly initialized 
are left uninitialized, and thus have an indeterminate value.

For example:

~~~
// static vs automatic storage
#include <iostream>
using namespace std;

int x;

int main ()
{
  int y;
  cout << x << endl;
  cout << y << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub5"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub5').value = `// static vs automatic storage
#include <iostream>
using namespace std;

int x;

int main ()
{
  int y;
  cout << x << endl;
  cout << y << endl;
  return 0;
}
`;
</script>
</form>
<br>

Running this code results in:

~~~
0
1639268406
~~~
{: .output}

Running this code a second time results in:

~~~
0
1657405494
~~~
{: .output}

Only the value of `x` is guaranteed to be zero. The value of `y` can contain any legal value (including zero).
