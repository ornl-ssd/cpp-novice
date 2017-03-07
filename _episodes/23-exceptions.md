---
title: "Exceptions"
teaching: 15
exercises: 15
questions:
objectives:
keypoints:
---
Exceptions provide a way to deal with exceptional conditions in your program. An exception is a value that is created when something unusual
happens in the program, usually in response to a runtime error condition. Exceptions are dealt with by being *caught* and transferring control 
to special functions called *handlers*.

To catch an exception, a section of code is enclosed in a special construct called a *`try` block*. 
When an exceptional circumstance arises within that block, an exception is thrown that transfers the control to the exception handler. 
If no exception is thrown, the code continues normally and all handlers are ignored.

An exception is thrown by using the `throw` keyword from inside the `try` block. Exception handlers are declared with the keyword `catch`, 
which must be placed immediately after the `try` block. 

The following example shows a simple `try` block in action.

~~~
// exceptions
#include <iostream>
using namespace std;

int main () {
  try
  {
    throw 20;
  }
  catch (int e)
  {
    cout << "An exception occurred. Exception Nr. " << e << '\n';
  }
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub1"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub1').value = `// exceptions
#include <iostream>
using namespace std;

int main () {
  try
  {
    throw 20;
  }
  catch (int e)
  {
    cout << "An exception occurred. Exception Nr. " << e << '\n';
  }
  return 0;
}
`;
</script>
</form>
<br>

The code enclosed in the `try` block is executed and if an exception occurs, control is transferred to the `catch` block. In this example 
the code simply throws an exception using the statement:

~~~
throw 20;
~~~
{: .code}

A `throw` expression accepts one parameter which is passed as an argument to the exception handler.

The exception handler is declared with the `catch` keyword immediately after the closing brace of the `try` block. The syntax for `catch` is 
similar to a regular function with one parameter. The type of this parameter is very important, since the type of the argument passed by the 
`throw` expression is checked against it, and only in the case they match, will the exception be caught by that handler.

Multiple handlers (i.e., `catch` expressions) can be chained, each one with a different parameter type. Only the handler whose argument type 
matches the type of the exception specified in the `throw` statement is executed.

If an ellipsis `...` is used as the parameter of `catch`, that handler will catch any exception no matter what the type of the exception thrown. 
This can be used as a default handler that catches all exceptions not caught by other handlers. 

The following shows an example of chaining exception handlers:

~~~
try {
  // code here
}
catch (int param) { cout << "int exception"; }
catch (char param) { cout << "char exception"; }
catch (...) { cout << "default exception"; }
~~~
{: .code}

In this case, the last handler would catch any exception thrown of a type that is neither `int` nor `char`.

After an exception has been handled the program, execution resumes after the `try-catch` block, not after the `throw` statement!.

It is also possible to nest a `try-catch` block within a `try` blocks. If the nested catch block wishes to forward the exception
to the next level, it can do so with the expression `throw;`. 

For example: 

~~~
try {
  try {
      // code here
  }
  catch (int n) {
      throw;
  }
}
catch (...) {
  cout << "Exception occurred";
}
~~~
{: .code}

### Standard exceptions

The C++ Standard library provides a base class specifically designed to declare objects to be thrown as exceptions. 
It is called `std::exception` and is defined in the `<exception>` header. This class has a virtual member function called 
`what` that returns a null-terminated character sequence (of type `char *`) and that can be overwritten in derived classes 
to contain some sort of description of the exception.

~~~
// using standard exceptions
#include <iostream>
#include <exception>
using namespace std;

class myexception: public exception
{
  virtual const char* what() const throw()
  {
    return "My exception happened";
  }
} myex;

int main() {
  try
  {
    throw myex;
  }
  catch (exception& e)
  {
    cout << e.what() << '\n';
  }
  return 0;
}
~~~
{: .code}

form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub2"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub2').value = `// using standard exceptions
#include <iostream>
#include <exception>
using namespace std;

class myexception: public exception
{
  virtual const char* what() const throw()
  {
    return "My exception happened";
  }
} myex;

int main() {
  try
  {
    throw myex;
  }
  catch (exception& e)
  {
    cout << e.what() << '\n';
  }
  return 0;
}
`;
</script>
</form>
<br>


In this example, the handler catches exception objects by reference (notice the ampersand `&` after the type). This allows the
handler to also catch classes derived from `exception`, like our `myex` object of type `myexception`.

All exceptions thrown by components of the C++ Standard library throw exceptions derived from this `exception` class. These are:

<table border="1">
<tr><th>exception</th><th>description</th></tr>
<tr><td><code>bad_alloc</code></td><td>thrown by new on allocation failure</td></tr>
<tr><td><code>bad_cast</code></td><td>thrown by dynamic_cast when it fails in a dynamic cast</td></tr>
<tr><td><code>bad_exception</code></td><td>thrown by certain dynamic exception specifiers</td></tr>
<tr><td><code>bad_typeid</code></td><td>thrown by typeid</td></tr>
<tr><td><code>bad_function_call</code></td><td>thrown by empty function objects</td></tr>
<tr><td><code>bad_weak_ptr</code></td><td>thrown by shared_ptr when passed a bad weak_ptr</td></tr>
</table>

Header `<exception>` also defines two generic exception types that are also derived from `exception`. These can be inherited by 
custom exceptions to report errors:

<table border="1">
<tr><th>exception</th><th>description</th></tr>
<tr><td><code>logic_error</code></td><td>error related to the internal logic of the program</td></tr>
<tr><td><code>runtime_error</code></td><td>error detected during runtime</td></tr>
</table>

A typical example where standard exceptions need to be checked for is on memory allocation:

~~~
// bad_alloc standard exception
#include <iostream>
#include <exception>
using namespace std;

int main () {
  try
  {
    int* myarray= new int[1000];
  }
  catch (exception& e)
  {
    cout << "Standard exception: " << e.what() << endl;
  }
  return 0;
}
~~~
{: .code}


The exception that may be caught by the exception handler in this example is a `bad_alloc`. Because `bad_alloc` 
is derived from the standard base class exception, it can be caught by using a single handler.
