---
title: "Classes I"
teaching: 45
exercises: 0
questions:
- "What are classes and how are they defined and used in C++?"
objectives:
- "Learn how to declare and use classes."
- "Learn how to define class constructors."
- "Learn how to initialize classes."
keypoints:
- "Classes encapsulate both data and functions."
- "Classes are types, objects are variables."
- "Classes allow access to members to be controlled."
- "A constructor can be used to initialize data members."
---
Classes build on the `struct` type by adding functions in addition to the data. So, while a `struct` encapsulates data only (the what), a
`class` encapsulates both data (the what) and functions that operate on that data (the how).

A class is a type that can be thought of as a blueprint. An instantation of the blueprint is an *instance* of the class. An instance of a class
is also known as an *object*. In terms of variables, a class would be the type, and an object would be the variable.

Classes are defined using either keyword `class` or keyword `struct`, with the following syntax:

~~~
class class_name {
  access_specifier_1:
    member1;
  access_specifier_2:
    member2;
  ...
} object_names;
~~~
{: .code}

In this definition, `class_name` is a valid identifier for the class, and `object_names` are an optional list of names for objects of this class. 
The body of the declaration can contain members, which can either be data or function declarations.

In addition to the data and function declarations, a `class` declaration can also optionally include *access specifiers*.  An access specifier 
is one of the following three keywords: `private`, `public` or `protected`. These specifiers modify the access rights 
for the members that follow them:

* `private` members of a class are accessible only from within other members of the same class (or from their "friends").
* `protected` members are accessible from other members of the same class (or from their "friends"), but also from members of their derived classes.
* `public` members are accessible from anywhere where the object is visible.

By default, all members of a class declared with the `class` keyword have `private` access. Therefore, any member that is 
declared before any other access specifier has `private` access automatically. 

For example: 

~~~
class Rectangle {
    int width, height;
  public:
    void set_values (int,int);
    int area (void);
} rect;
~~~
{: .code}

This declares a class called `Rectangle` and an instance of this class, called `rect`. This class contains four members: 
two data members (`width` and `height`) with `private` access and two member functions (`set_values` and `area`) with `public` access.
The member functions have only included using their declaration, but not their definition.

Notice the difference between the class name and the object name. In the previous example, `Rectangle` was the class name (i.e., the type), 
whereas `rect` was an object of type `Rectangle` (i.e. the variable). It is the same relationship `int` and `a` have in the following declaration: 

~~~
int a;
~~~
{: .code}

Any of the `public` members of object `rect` can be accessed as if they were normal functions or
normal variables, by simply inserting a dot (.) between object name and member name. This follows the same syntax as accessing the members of 
plain `struct` types. For example: 

~~~
rect.set_values(3,4);
myarea = rect.area(); 
~~~
{: .code}

The `width` and `height` members of `rect` cannot be accessed from "outside" the class, since they have `private` access and 
can only be referred to from within other members of that same class.

Here is the complete example of class `Rectangle`:

~~~
// classes example
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    void set_values(int,int);
    int area() {return width*height;}
};

void Rectangle::set_values(int x, int y) {
  width = x;
  height = y;
}

int main() {
  Rectangle rect;
  rect.set_values(3,4);
  cout << "area: " << rect.area();
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub1"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub1').value = `// classes example
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    void set_values(int,int);
    int area() {return width*height;}
};

void Rectangle::set_values(int x, int y) {
  width = x;
  height = y;
}

int main() {
  Rectangle rect;
  rect.set_values(3,4);
  cout << "area: " << rect.area();
  return 0;
}
`;
</script>
</form>
<br>

The members `width` and `height` have `private` access, so access from outside the class is 
not allowed. Instead, we have defined a member function `set_values` to set the values for those members within the object. 
The rest of the program does not need to have direct access to these members, and controlling access to data members in this way is a very
important aspect of object oriented programming.

Notice that the definition of the member function `area` has been included directly within the definition of class `Rectangle` given its
simplicity.

The `set_values` member function is more complicated, so only its prototype is declared within the class. The actual definition
of `set_values` is outside the class, and uses the scope operator `::` to indicate that the function being defined is a member of the 
class `Rectangle` and not a regular non-member function. This grants exactly the same scope properties to the function, as if the
definition was directly included within the class. This allows the function to have access to the variables `width` and `height`, 
even though they are declared as `private` members of class.

The most important property of a class is that it is a type, and as such, we can declare multiple objects of this type in the same way we 
can declare multiple variables of type `int`. Let's declare two `Rectangle` objects instead of one:

~~~
// example: one class, two objects
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    void set_values(int,int);
    int area() {return width*height;}
};

void Rectangle::set_values(int x, int y) {
  width = x;
  height = y;
}

int main() {
  Rectangle rect_a, rect_b;
  rect_a.set_values(3,4);
  rect_b.set_values(5,6);
  cout << "rect_a area: " << rect_a.area() << endl;
  cout << "rect_b area: " << rect_b.area() << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub2"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub2').value = `// example: one class, two objects
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    void set_values(int,int);
    int area() {return width*height;}
};

void Rectangle::set_values(int x, int y) {
  width = x;
  height = y;
}

int main() {
  Rectangle rect_a, rect_b;
  rect_a.set_values(3,4);
  rect_b.set_values(5,6);
  cout << "rect_a area: " << rect_a.area() << endl;
  cout << "rect_b area: " << rect_b.area() << endl;
  return 0;
}
`;
</script>
</form>
<br>

In this we have created to instances of the class `Rectangle` called `rect_a` and `rect_b`.

Notice that the call to `rect_a.area()` does not give the same result as the call to `rect_b.area()`. This is because each object of class
has its own variables `width` and `height`, and the function members `set_value` and `area` 
operate only on the object's own member variables.

Classes allow programming using object-oriented paradigms. Data and functions are both members of the object, reducing the need to pass and
carry handlers or other state variables as arguments to functions, because they are part of the object whose member is called. Notice that no 
arguments were passed on the calls to `rect.area` or `rectb.area`. Those member functions directly used the data members of their respective 
objects `rect_a` and `rect_b`.

### Constructors

What would happen in the previous example if we called the member function `area` before having called `set_values`? The members `width` and 
`height` have not been assigned a value at that point, so the result is unpredictable.

In order to avoid this situation, a class can include a special function called its *constructor*, which is automatically called whenever 
a new object of this class is created. The function of the constructor is to allow the class to initialize member variables or allocate storage
before the member functions are called.

A constructor function is declared just like a regular member function, but has the same name as the class name. It also doesn't have
a return type, not even void.

The `Rectangle` class above can easily be improved by implementing a constructor:

~~~
// example: class constructor
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    Rectangle(int,int);
    int area() {return (width*height);}
};

Rectangle::Rectangle(int a, int b) {
  width = a;
  height = b;
}

int main() {
  Rectangle rect_a(3,4);
  Rectangle rect_b(5,6);
  cout << "rect_a area: " << rect_a.area() << endl;
  cout << "rect_b area: " << rect_b.area() << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub3"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub3').value = `// example: class constructor
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    Rectangle(int,int);
    int area() {return (width*height);}
};

Rectangle::Rectangle(int a, int b) {
  width = a;
  height = b;
}

int main() {
  Rectangle rect_a(3,4);
  Rectangle rect_b(5,6);
  cout << "rect_a area: " << rect_a.area() << endl;
  cout << "rect_b area: " << rect_b.area() << endl;
  return 0;
}
`;
</script>
</form>
<br>

The results of this example are identical to those of the previous example. The `Rectangle` class no longer requires the member function 
`set_values`, as it is now possible to use the constructor to initialize the values of `width` and `height` when the object is created:

~~~
Rectangle rect_a(3,4);
Rectangle rect_b(5,6);
~~~
{: .code}

Constructors cannot be called explicitly. They are only executed once, automatically, when a new object of the 
class is created.

## Advanced Topics

### Overloading constructors

A constructor can be overloaded like any other function, in order to use a different number of 
parameters and/or parameters of different types. The compiler will automatically call the constructor
whose parameters match the arguments:

~~~
// overloading class constructors
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    Rectangle();
    Rectangle(int,int);
    int area (void) {return (width*height);}
};

Rectangle::Rectangle() {
  width = 5;
  height = 5;
}

Rectangle::Rectangle(int a, int b) {
  width = a;
  height = b;
}

int main () {
  Rectangle rect_a(3,4);
  Rectangle rect_b;
  cout << "rect area: " << rect.area() << endl;
  cout << "rectb area: " << rectb.area() << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub4"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub4').value = `// overloading class constructors
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    Rectangle();
    Rectangle(int,int);
    int area (void) {return (width*height);}
};

Rectangle::Rectangle() {
  width = 5;
  height = 5;
}

Rectangle::Rectangle(int a, int b) {
  width = a;
  height = b;
}

int main () {
  Rectangle rect_a(3,4);
  Rectangle rect_b;
  cout << "rect area: " << rect.area() << endl;
  cout << "rectb area: " << rectb.area() << endl;
  return 0;
}
`;
</script>
</form>
<br>

In this example, two objects of class `Rectangle` are again created. However, this time while `rect_a` is constructed with two arguments, `rect_b`
is constructed with no arguments. This is a special kind of constructor called the *default constructor*. It is special because it can't be
called with even an empty set of parenthases.

~~~
Rectangle rectb;   // ok, default constructor called
Rectangle rectc(); // oops, default constructor NOT called, defining a function instead
~~~
{: .code}

The problem is that adding an empty set of parentheses makes `rect_c` a function declaration instead of an object declaration.
It would be a function that takes no arguments and returns a value of type Rectangle.

### Uniform initialization

Calling constructors by enclosing their arguments in parentheses, as shown above, is known as functional form. However, constructors can also 
be called in variety of other ways.

Constructors with a single parameter can be called using the variable initialization syntax (an equal sign followed by the argument):

~~~
class_name object_name = initialization_value; 
~~~
{: .code}

Constructors can also be called using uniform initialization, which essentially is the same as the functional form, 
but using braces instead of parentheses:

~~~
class_name object_name { value, value, value, ... } 
~~~
{: .code}

Optionally, this last syntax can include an equal sign before the braces.

Here is an example with four ways to construct objects of a class whose constructor takes a single parameter:

~~~
// classes and uniform initialization
#include <iostream>
using namespace std;

class Circle {
    double radius;
  public:
    Circle(double r) { radius = r; }
    double circum() {return 2*radius*3.14159265;}
};

int main() {
  Circle foo (10.0);   // functional form
  Circle bar = 20.0;   // assignment init.
  Circle baz {30.0};   // uniform init.
  Circle qux = {40.0}; // POD-like

  cout << "foo's circumference: " << foo.circum() << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub5"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub5').value = `// classes and uniform initialization
#include <iostream>
using namespace std;

class Circle {
    double radius;
  public:
    Circle(double r) { radius = r; }
    double circum() {return 2*radius*3.14159265;}
};

int main() {
  Circle foo (10.0);   // functional form
  Circle bar = 20.0;   // assignment init.
  Circle baz {30.0};   // uniform init.
  Circle qux = {40.0}; // POD-like

  cout << "foo's circumference: " << foo.circum() << endl;
  return 0;
}
`;
</script>
</form>
<br>


An advantage of uniform initialization over functional form is that, unlike parentheses, braces cannot be confused with function declarations, 
and thus can be used to explicitly call default constructors:

~~~
Rectangle rectb;   // default constructor called
Rectangle rectc(); // function declaration (default constructor NOT called)
Rectangle rectd{}; // default constructor called 
~~~
{: .code}

The choice of syntax to call constructors is largely a matter of style. Most existing code currently uses functional form, and some newer 
style guides suggest to choose uniform initialization over the others, even though it also has its potential pitfalls for its preference of 
initializer_list as its type.

### Member initialization in constructors

When a constructor is used to initialize other members, these other members can be initialized directly, without resorting to statements 
in its body. This is done by inserting a colon before the constructor's body, followed by a list of initializations for class members. 
For example, consider a class with the following declaration:


~~~
class Rectangle {
    int width,height;
  public:
    Rectangle(int,int);
    int area() {return width*height;}
};
~~~
{: .code}

The constructor for this class could be defined, as usual, as:

~~~
Rectangle::Rectangle(int x, int y) { width=x; height=y; }
~~~
{: .code}

However, using member initialization, it could also be defined as:

~~~
Rectangle::Rectangle(int x, int y) : width(x) { height=y; }
~~~
{: .code}

Or even:

~~~
Rectangle::Rectangle(int x, int y) : width(x), height(y) { }
~~~
{: .code}

Note how in this last case, the constructor does nothing else than initialize its members, hence it has an empty function body.

For members of fundamental types, it makes no difference how the constructor is defined. For members that are objects (i.e. whose type is a class),
if they are not initialized after the colon, they are *default-constructed*. i.e. they are constructed by calling their default constructors.

Default-constructing all members of a class may or may always not be convenient. In some cases, it can be a waste of time, such as
when the member is then reinitialized in the constructor. In other cases, the class may not have a default constructor, so 
default-construction is not possible). In these cases, members should be initialized
in the member initialization list. For example:

~~~
// member initialization
#include <iostream>
using namespace std;

class Circle {
    double radius;
  public:
    Circle(double r) : radius(r) { }
    double area() {return radius*radius*3.14159265;}
};

class Cylinder {
    Circle base;
    double height;
  public:
    Cylinder(double r, double h) : base (r), height(h) {}
    double volume() {return base.area() * height;}
};

int main() {
  Cylinder foo (10,20);

  cout << "foo's volume: " << foo.volume() << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub6"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub6').value = `// member initialization
#include <iostream>
using namespace std;

class Circle {
    double radius;
  public:
    Circle(double r) : radius(r) { }
    double area() {return radius*radius*3.14159265;}
};

class Cylinder {
    Circle base;
    double height;
  public:
    Cylinder(double r, double h) : base (r), height(h) {}
    double volume() {return base.area() * height;}
};

int main() {
  Cylinder foo (10,20);

  cout << "foo's volume: " << foo.volume() << endl;
  return 0;
}
`;
</script>
</form>
<br>


In this example, class `Cylinder` has a member object `base` whose type is another `Circle`. Objects of class `Circle` can only be constructed 
with a parameter, so the `Cylinder` constructor needs to call the `Circle` constructor. The only way to do this is in the member initializer list.

These initializations can also use uniform initializer syntax, using braces {} instead of parentheses ():

~~~ 
Cylinder::Cylinder (double r, double h) : base{r}, height{h} { }
~~~
{: .code}

### Pointers to classes

Objects can also be referenced by pointers. A class is just like any other type, so it can be used in pointer declarations. 

The following example shows how to declare a pointer to an object of class `Rectangle`:

~~~
Rectangle * prect;
~~~
{: .code}

As with `struct` types, the members of an object can be accessed directly from a pointer by using the arrow operator `->`. Here is an example 
with some possible combinations:

~~~
// pointer to classes example
#include <iostream>
using namespace std;

class Rectangle {
  int width, height;
public:
  Rectangle(int x, int y) : width(x), height(y) {}
  int area(void) { return width * height; }
};


int main() {
  Rectangle obj (3, 4);
  Rectangle * foo, * bar, * baz;
  foo = &obj;
  bar = new Rectangle(5, 6);
  baz = new Rectangle[2] { {2,5}, {3,6} };
  cout << "obj's area: " << obj.area() << endl;
  cout << "*foo's area: " << foo->area() << endl;
  cout << "*bar's area: " << bar->area() << endl;
  cout << "baz[0]'s area:" << baz[0].area() << endl;
  cout << "baz[1]'s area:" << baz[1].area() << endl;       
  delete bar;
  delete[] baz;
  return 0;
}	
~~~
{: .code}

This example makes use of several operators to operate on objects and pointers (operators `*`, `&`, `.`, `->`, `[]`). 

They can be interpreted as:

<table border="1">
<tr><th>expression</th><th>can be read as</th></tr>
<tr><td><code>*x</code></td><td>pointed to by x</td></tr>
<tr><td><code>&x</code></td><td>address of x</td></tr>
<tr><td><code>x.y</code></td><td>member y of object x</td></tr>
<tr><td><code>x->y</code></td><td>member y of object pointed to by x</td></tr>
<tr><td><code>(*x).y</code></td><td>member y of object pointed to by x (equivalent to the previous one)</td></tr>
<tr><td><code>x[0]</code></td><td>first object pointed to by x</td></tr>
<tr><td><code>x[1]</code></td><td>second object pointed to by x</td></tr>
<tr><td><code>x[n]</code></td><td>(n+1)th object pointed to by x</td></tr>
</table>

### Classes defined with `struct` and `union`

Classes can also be defined using the keywords `struct` and `union`.

The keyword `struct` can be used intechangeably with the keyword `class`. The only difference between both is that members of classes 
declared with the keyword `struct` have public access by default, while members of classes declared with the keyword class have 
private access by default. For all other purposes both keywords are equivalent in this context.

A class declared with the keyword `union` behaves like a `union` in that all data members share the same storage. However, like a class,
it is possible to define member functions. The default access in union classes is public.
