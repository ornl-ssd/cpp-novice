---
title: "Templates"
teaching: 15
exercises: 15
questions:
objectives:
keypoints:
---
Overloaded functions may have the same definition. For example:

~~~
// overloaded functions
#include <iostream>
using namespace std;

int sum(int a, int b)
{
  return a + b;
}

double sum(double a, double b)
{
  return a + b;
}

int main()
{
  cout << sum(10, 20) << '\n';
  cout << sum(1.0, 1.5) << '\n';
  return 0;
}
~~~
{: .code}

With corresponding output:

~~~
30
2.5
~~~
{: .output}

Here, `sum` is overloaded with different parameter types, but with exactly the same body.

The function `sum` could be overloaded for a lot of types, and it could make sense for all of them to have the same body. 
For cases such as this, C++ has the ability to define functions with generic types, known as *function templates*. 
Defining a function template follows the same syntax as a regular function, except that it is preceded by the template 
keyword and a series of template parameters enclosed in angle-brackets `<>` as follows:

~~~
template <template-parameters> function-declaration 
~~~
{: .code}

The template parameters are a series of parameters separated by commas. These parameters can be generic template types by s
pecifying either the `class` or `typename` keyword followed by an identifier. It makes no difference whether the generic type 
is specified with keyword `class` or keyword `typename` in the template argument list. The identifier can then be used in the 
function declaration as if it was a regular type. 

For example, a generic `sum` function could be defined as:

~~~
template <typename SomeType>
SomeType sum(SomeType a, SomeType b)
{
  return a + b;
}
~~~
{: .code}

In the code above, declaring `SomeType` this way allows it to be used anywhere in the function definition, just as if it was any other type.
It can be used as the type for parameters, as return type, or to declare new variables of this type. In all cases, it represents a generic 
type that will be determined on the moment the template is *instantiated*.

Instantiating a template is applying the template to create a function using particular types or values for its template parameters. 
This is done by calling the function template, with the same syntax as calling a regular function, but specifying the template 
arguments enclosed in angle brackets as follows:

~~~
name <template-arguments> (function-arguments)
~~~
{: .code}

For example, the `sum` function template defined above can be called with:

~~~
x = sum<int>(10,20);
~~~
{: .code}

The function `sum<int>` is just one of the possible instantiations of function template sum. In this case, by using `int` as template argument 
in the call, the compiler automatically instantiates a version of `sum` where each occurrence of `SomeTyp`e is replaced by `int`, as if it 
was defined as:

~~~
int sum(int a, int b)
{
  return a + b;
}
~~~
{: .code}

Let's see an actual example:

~~~
// function template
#include <iostream>
using namespace std;

template <class T>
T sum(T a, T b)
{
  T result;
  result = a + b;
  return result;
}

int main() {
  int i=5, j=6, k;
  double f=2.0, g=0.5, h;
  k=sum<int>(i,j);
  h=sum<double>(f,g);
  cout << k << '\n';
  cout << h << '\n';
  return 0;
}
~~~
{: .code}

Running this code results in:

~~~
11
2.5
~~~
{: .output}

In this case, we have used `T` as the template parameter name, instead of `SomeType`. It makes no difference, and `T` is actually a quite 
common template parameter name for generic types. 

In the example above, we used the function template `sum` twice. The first time with arguments of type `int`, and the second one with 
arguments of type `double`. The compiler has instantiated and then called each time the appropriate version of the function.

Note also how `T` is also used to declare a local variable of that (generic) type within sum:

~~~ 
T result;
~~~
{: .code}

This causes `result` to be a variable of the same type as the parameters `a` and `b`, and the type returned by the function.
In this specific case where the generic type `T` is used as a parameter for `sum`, the compiler is even able to deduce the 
data type automatically without having to explicitly specify it within angle brackets. Therefore, instead of explicitly 
specifying the template arguments with:

~~~
k = sum<int>(i,j);
h = sum<double>(f,g);
~~~
{: .code}

It is possible to instead simply write them without the type enclosed in angle brackets:

~~~
k = sum(i,j);
h = sum(f,g);
~~~
{: .code}

Naturally, for this to work, the use of types must be unambiguous. If `sum` is called with arguments of different types, the compiler 
may not be able to deduce the type of `T` automatically.

Templates are a powerful and versatile feature. They are not restricted to a single template parameter, and the function can still use 
regular non-templated types. For example:

~~~
// function templates
#include <iostream>
using namespace std;

template <class T, class U>
bool are_equal(T a, U b)
{
  return a == b;
}

int main()
{
  if (are_equal(10, 10.0))
    cout << "x and y are equal\n";
  else
    cout << "x and y are not equal\n";
  return 0;
}
~~~
{: .code}

Running this code will produce:

~~~
x and y are equal
~~~
{: .output}

Note that this example uses automatic template parameter deduction in the call to `are_equal`:

~~~
are_equal(10, 10.0)
~~~
{: .code}

This call is equivalent to:

~~~
are_equal<int, double>(10, 10.0)
~~~
{: .code}

In this case there is no ambiguity possible because numerical literals are always of a specific type. Unless otherwise specified with a suffix, 
integer literals always produce values of type `int`, and floating-point literals always produce values of type `double`. 

### Non-type template arguments

The template parameters can not only include types introduced by `class` or `typename` keywords, but can also include expressions of a 
particular type:

~~~
// template arguments
#include <iostream>
using namespace std;

template <class T, int N>
T fixed_multiply(T val)
{
  return val * N;
}

int main() {
  std::cout << fixed_multiply<int, 2>(10) << '\n';
  std::cout << fixed_multiply<int, 3>(10) << '\n';
}
~~~
{: .code}

The output from this program is:

~~~
20
30
~~~
{: .code}

The second argument of the `fixed_multiply` function template is of type `int`. It just looks like a regular function parameter, and can 
actually be used just like one. However, the value of template parameters is determined at *compile time* to generate a different instantiation 
of the function `fixed_multiply`. This means that the value of that argument is never passed at run time. The two calls to `fixed_multiply` 
in main essentially call two versions of the function: one that always multiplies by two, and one that always multiplies by three. 
Because of this, template arguments used in this way must always be a constant expression and cannot be passed as a variable.
 