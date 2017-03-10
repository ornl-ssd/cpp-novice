---
title: "Structs"
teaching: 30
exercises: 0
questions:
- "How are data types grouped together in C++?"
objectives:
- "Learn how to declare and use struct types."
keypoints:
- "Struct types are for grouping different data types together."
- "Accessing members of a struct is just like accessing a variable."
- "Arrays of structs are allowed."
- "Structs can contain any data type, including other structs."
---
A `struct` (short for structure) is a mechanism for grouping data elements together under a single name. These data elements, known as members, 
can have different types and different lengths. 

Structures are declared in C++ using the following syntax:

~~~
struct type_name {
member_type1 member_name1;
member_type2 member_name2;
member_type3 member_name3;
.
.
} object_names;
~~~
{: .code}

Where `type_name` is a name for the structure type, and `object_names` are a list of valid identifiers for objects that have the type of this structure. 
A list of the data members comprising the `struct` are included in the braces. Each data member is specified with a type and a name. If the 
`object_names` are omitted, then this declares a type only. If the `type_name` is omitted, then the objects have an *anonymous struct type*.

For example:

~~~
struct product {
  int weight;
  double price;
};

product apple;
product banana, melon;
~~~
{: .code}

This declares a `struct` type, called `product`, and defines it having two members: `weight` and `price`, each of a different fundamental type. 
This declaration creates a new type `product`, which is then used to declare three variables (also known as objects) of this type: 
`apple`, `banana`, and `melon`. 
Note how once `product` is declared, it can be used just like any other type.

If `object_names` are specified, then objects of this type can be declared directly. For example, the structure objects `apple`, `banana` and `melon` 
could be declared this way as follows: 

~~~
struct product {
  int weight;
  double price;
} apple, banana, melon;
~~~
{: .code}

The members of the structure can be accessed by inserting a dot (.) between the object name and the member name. For example, we could operate 
with any of these elements as if they were standard variables of their respective types: 

~~~
apple.weight
apple.price
banana.weight
banana.price
melon.weight
melon.price
~~~
{: .code}

Each one of these has the data type corresponding to the member they refer to: `apple.weight`, `banana.weight`, and `melon.weight` are of type `int`, 
while `apple.price`, `banana.price`, and `melon.price` are of type `double`.

Here is a real example with structure types in action:

~~~
// example about structures
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

struct movies_t {
  string title;
  int year;
} mine, yours;

void printmovie(movies_t movie);

int main()
{
  string mystr;

  mine.title = "2001 A Space Odyssey";
  mine.year = 1968;

  cout << "Enter title: ";
  getline (cin,yours.title);
  cout << "Enter year: ";
  getline(cin,mystr);
  stringstream(mystr) >> yours.year;

  cout << "My favorite movie is: " << endl;
  printmovie(mine);
  cout << "And yours is: " << endl;
  printmovie(yours);
  return 0;
}

void printmovie(movies_t movie)
{
  cout << movie.title;
  cout << " (" << movie.year << ")" << endl;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub1"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub1').value = `// example about structures
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

struct movies_t {
  string title;
  int year;
} mine, yours;

void printmovie(movies_t movie);

int main()
{
  string mystr;

  mine.title = "2001 A Space Odyssey";
  mine.year = 1968;

  cout << "Enter title: ";
  getline (cin,yours.title);
  cout << "Enter year: ";
  getline(cin,mystr);
  stringstream(mystr) >> yours.year;

  cout << "My favorite movie is: " << endl;
  printmovie(mine);
  cout << "And yours is: " << endl;
  printmovie(yours);
  return 0;
}

void printmovie(movies_t movie)
{
  cout << movie.title;
  cout << " (" << movie.year << ")" << endl;
}
`;
</script>
</form>
<br>

Running this code produces output like the following:

~~~
Enter title: Alien
Enter year: 1979

My favorite movie is:
 2001 A Space Odyssey (1968)
And yours is:
 Alien (1979)
~~~
{: .output}

The example shows how the members of an object act just as regular variables. For example, the member `yours.year` is a valid variable of type `int`, 
and `mine.title` is a variable of type `string`.

One of the features of structures is the ability to refer to both their members 
individually or to the entire structure as a whole. 

Because structures are types, they can also be used as the base type of arrays. Each element of the array is a different `struct` object that is
referred to using the array name and index value.

For example:

~~~
// array of structures
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

struct movies_t {
  string title;
  int year;
} films [3];

void printmovie(movies_t movie);

int main()
{
  string mystr;
  int n;

  for (n=0; n<3; n++)
  {
    cout << "Enter title: ";
    getline (cin,films[n].title);
    cout << "Enter year: ";
    getline (cin,mystr);
    stringstream(mystr) >> films[n].year;
  }

  cout << "\nYou have entered these movies:" << endl;
  for (n=0; n<3; n++)
    printmovie(films[n]);
  return 0;
}

void printmovie(movies_t movie)
{
  cout << movie.title;
  cout << " (" << movie.year << ")" << endl;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub2"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub2').value = `// array of structures
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

struct movies_t {
  string title;
  int year;
} films [3];

void printmovie(movies_t movie);

int main()
{
  string mystr;
  int n;

  for (n=0; n<3; n++)
  {
    cout << "Enter title: ";
    getline (cin,films[n].title);
    cout << "Enter year: ";
    getline (cin,mystr);
    stringstream(mystr) >> films[n].year;
  }

  cout << "\nYou have entered these movies:" << endl;
  for (n=0; n<3; n++)
    printmovie(films[n]);
  return 0;
}

void printmovie(movies_t movie)
{
  cout << movie.title;
  cout << " (" << movie.year << ")" << endl;
}
`;
</script>
</form>
<br>

Running this produces:

~~~
Enter title: Blade Runner
Enter year: 1982
Enter title: The Matrix
Enter year: 1999
Enter title: Taxi Driver
Enter year: 1976
 
You have entered these movies:
Blade Runner (1982)
The Matrix (1999)
Taxi Driver (1976)
~~~
{: .output}

## Advanced Topics

### Pointers to structures

Like any other type, structures can be pointed to by its own type of pointers:

~~~
struct movies_t {
  string title;
  int year;
};

movies_t amovie;
movies_t *pmovie;
~~~
{: .code}

Here `amovie` is an object of structure type `movies_t`, and `pmovie` is a pointer to objects of structure type `movies_t`. Therefore, 
the following code would also be valid:

~~~
pmovie = &amovie;
~~~
{: .code}

The value of the pointer pmovie would be assigned the address of object `amovie`.

Now, let's see another example that mixes pointers and structures, and will serve to introduce a new operator: the arrow operator `->`:

~~~
// pointers to structures
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

struct movies_t {
  string title;
  int year;
};

int main()
{
  string mystr;

  movies_t amovie;
  movies_t * pmovie;
  pmovie = &amovie;

  cout << "Enter title: ";
  getline (cin, pmovie->title);
  cout << "Enter year: ";
  getline (cin, mystr);
  (stringstream) mystr >> pmovie->year;

  cout << "\nYou have entered:" << endl;
  cout << pmovie->title;
  cout << " (" << pmovie->year << ")" << endl;

  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub3"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub3').value = `// pointers to structures
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

struct movies_t {
  string title;
  int year;
};

int main()
{
  string mystr;

  movies_t amovie;
  movies_t * pmovie;
  pmovie = &amovie;

  cout << "Enter title: ";
  getline (cin, pmovie->title);
  cout << "Enter year: ";
  getline (cin, mystr);
  (stringstream) mystr >> pmovie->year;

  cout << "\nYou have entered:" << endl;
  cout << pmovie->title;
  cout << " (" << pmovie->year << ")" << endl;

  return 0;
}
`;
</script>
</form>
<br>

Running this produces:

~~~
Enter title: Invasion of the body snatchers
Enter year: 1978
 
You have entered:
Invasion of the body snatchers (1978)
~~~
{: .output}


The arrow operator is a dereference operator that is used exclusively with pointers to objects that have members. This operator 
serves to access the member of an object directly from its address. 

For example:

~~~
pmovie->title
~~~
{: .code}

is, for all purposes, equivalent to: 

~~~
(*pmovie).title
~~~
{: .code}

Both expressions, `pmovie->title` and `(*pmovie).title` are valid, and both access the member `title` of the data structure referenced by a 
pointer called `pmovie`. It is definitely something different than:

~~~
*pmovie.title
~~~
{: .code}

which is equivalent to:

~~~
*(pmovie.title)
~~~
{: .code}

This would access the value pointed by a hypothetical pointer member called `title` of the structure object `pmovie` (which is not the case, 
since `title` is not a pointer type). The following panel summarizes possible combinations of the operators for pointers and for structure members:

<table border="1">
<tr><th>Expression</th><th>What is evaluated</th><th>Equivalent</th></tr>
<tr><td>a.b</td><td>Member b of object a</td><td></td></tr>
<tr><td>a->b</td><td>Member b of object pointed to by a</td><td>(*a).b</td></tr>
<tr><td>*a.b</td><td>Value pointed to by member b of object a</td><td>*(a.b)</td></tr>
</table>

### Nesting structures

Structures can also be nested in such a way that an element of a structure is itself another structure:

~~~
struct movies_t {
  string title;
  int year;
};

struct friends_t {
  string name;
  string email;
  movies_t favorite_movie;
} charlie, maria;

friends_t * pfriends = &charlie;
~~~
{: .code}

After the previous declarations, all of the following expressions would be valid:

~~~
charlie.name
maria.favorite_movie.title
charlie.favorite_movie.year
pfriends->favorite_movie.year
~~~
{: .code}

(where, by the way, the last two expressions refer to the same member). 
