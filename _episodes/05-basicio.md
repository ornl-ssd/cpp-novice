---
title: "Basic Input/Output"
teaching: 15
exercises: 15
questions:
objectives:
keypoints:
---
C++ uses a convenient abstraction called *streams* to perform input and output operations in sequential media such as the screen, the 
keyboard or a file. A stream is an entity where a program can either insert or extract characters to/from. There is no need to know 
details about the media associated to the stream or any of its internal specifications. All we need to know is that streams are a 
source/destination of characters, and that these characters are provided/accepted sequentially (i.e., one after another).

The standard library defines a handful of stream objects that can be used to access what are considered the standard sources and 
destinations of characters by the environment where the program runs:

<table border="1">
<tr><th>stream</th><th>description</th></tr>
<tr><td>cin</td><td>standard input stream</td></tr>
<tr><td>cout</td><td>standard output stream</td></tr>
<tr><td>cerr</td><td>standard error (output) stream</td></tr>
<tr><td>clog</td><td>standard logging (output) stream</td></tr>
</table>

We are going to see in more detail how `cout` and `cin` work. The streams `cerr` and `clog` are also output streams, 
so they essentially work like `cout`, with the only difference being that they identify streams for specific purposes, namely error messages 
and logging. In most cases these streams just send output to the terminal, however they can be individually redirected if necessary.

### Standard output (`cout`)

In most program environments, the standard output by default is the screen, and the C++ stream object defined to access it is `cout`.

For formatted output operations, `cout` is used together with the insertion operator, which is written as `<<`.

~~~
cout << "Output sentence"; // prints Output sentence on screen
cout << 120;               // prints number 120 on screen
cout << x;                 // prints the value of x on screen  
~~~
{: .code}

The `<<` operator inserts the data that follows it into the stream that precedes it. In the examples above, it inserted the literal 
string `Output sentence`, the number 120, and the value of variable `x` into the standard output stream cout. Notice that the sentence 
in the first statement is enclosed in double quotes (`"`) because it is a string literal, while in the last one, `x` is not.

Multiple insertion operations may be chained, and literals 
and variables can be mixed in a single statement:

~~~
cout << "I am " << age << " years old and my zipcode is " << zipcode;
~~~
{: .code}

Assuming the `age` variable contains the value 24 and the `zipcode` variable contains 90064, the output of the previous statement would be: 

~~~
I am 24 years old and my zipcode is 90064
~~~
{: .output}

What `cout` does not do automatically is add line breaks at the end, unless instructed to do so. For example, take the following two 
statements inserting into `cout`:

~~~
cout << "This is a sentence.";
cout << "This is another sentence."; 
~~~
{: .code}
The output would be in a single line, without any line breaks in between. Something like:

~~~
This is a sentence.This is another sentence. 
~~~
{: .output}

To insert a line break, a *new-line* character needs be inserted at the exact position the line should be broken. In C++, a 
new-line character can be specified as `\n` (i.e., a backslash character followed by a lowercase `n`). For example:

~~~
cout << "First sentence.\n";
cout << "Second sentence.\nThird sentence.";
~~~
{: .code}

This produces the following output: 

~~~
First sentence.
Second sentence.
Third sentence.
~~~
{: .output}

Alternatively, the `endl` manipulator can also be used to break lines. For example: 

~~~
cout << "First sentence." << endl;
cout << "Second sentence." << endl;
~~~
{: .code}

This would print:

~~~
First sentence.
Second sentence.
~~~
{: .output}

The `endl` manipulator produces a newline character, exactly as the insertion of `\n` does, but it also causes the stream's 
buffer to be flushed. This means that the output is requested to be physically written to the device, instead of being accumulated
into a buffer. Some streams are buffered and some are not, however it is generally a 
good idea to use `endl` in most situations.

### Standard input (`cin`)

In most program environments, the standard input by default is the keyboard, and the C++ stream object defined to access it is `cin`.

For formatted input operations, `cin` is used together with the extraction operator, which is written as `>>`. 
This operator is then followed by the variable where the extracted data is stored. For example:

~~~
int age;
cin >> age;
~~~
{: .code}

The first statement declares a variable of type `int` called `age`, and the second extracts from `cin` a value to be stored in it. This 
operation makes the program wait for input from `cin`, which generally means that the program will wait for the user to enter some 
sequence with the keyboard. In this case, note that the characters introduced using the keyboard are only transmitted to the program 
when the ENTER (or RETURN) key is pressed. Once the statement with the extraction operation on `cin` is reached, the program will wait 
for as long as needed until some input is introduced.

The extraction operation on `cin` uses the type of the variable after the `>>` operator to determine how it interprets the characters 
read from the input, so if it is an integer, the format expected is a series of digits, if a string a sequence of characters, etc.

~~~
// i/o example

#include <iostream>
using namespace std;

int main()
{
  int i;
  cout << "Please enter an integer value: ";
  cin >> i;
  cout << "The value you entered is " << i;
  cout << " and its double is " << i*2 << ". << endl";
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub1"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub1').value = `// i/o example

#include <iostream>
using namespace std;

int main()
{
  int i;
  cout << "Please enter an integer value: ";
  cin >> i;
  cout << "The value you entered is " << i;
  cout << " and its double is " << i*2 << "." << endl;
  return 0;
}
`;
</script>
</form>
<br>

This program will produce the following output, assuming "702" is entered at the prompt:

~~~
Please enter an integer value: 702
The value you entered is 702 and its double is 1404.
~~~
{: .output}

As you can see, extracting from `cin` seems to make the task of getting input from the standard input pretty simple and straightforward. 
But this method also has a big drawback. What happens in the example above if the user enters something else that cannot be interpreted 
as an integer? Well, in this case, the extraction operation fails. And this, by default, lets the program continue without setting a 
value for variable `i`, producing undetermined results if the value of `i` is used later.

This is very poor program behavior. Most programs are expected to behave in an expected manner no matter what the user types, handling 
invalid values appropriately. Only very simple programs should rely on values extracted directly from cin without further checking. 
A little later we will see how `stringstreams` can be used to have better control over user input. 

Extractions on `cin` can also be chained to request more than one item in a single statement:

~~~
cin >> a >> b;
~~~
{: .code}

This is equivalent to:

~~~
cin >> a;
cin >> b;
~~~
{: .code}

In both cases, the user is expected to introduce two values, one for variable `a`, and another for variable `b`. Any kind of space is 
used to separate two consecutive input operations; this may either be a space, a tab, or a new-line character.

### `cin` and strings

The extraction operator can be used on `cin` to get strings of characters in the same way as with fundamental data types:

~~~
string mystring;
cin >> mystring;
~~~
{: .code}

However, `cin` extraction always considers spaces (whitespaces, tabs, new-line...) as terminating the value being extracted, and 
thus extracting a string means it will always extract a single word, not a phrase or an entire sentence.

To get an entire line from `cin`, a function called `getline` needs to be used. This function takes stream as first argument, and 
the string variable used to hold the value, as second. For example:

~~~
// cin with strings
#include <iostream>
#include <string>
using namespace std;

int main()
{
  string mystr;
  cout << "What's your name? ";
  getline(cin, mystr);
  cout << "Hello " << mystr << "." << endl;
  cout << "What is your favorite team? ";
  getline(cin, mystr);
  cout << "I like " << mystr << " too!" << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub2"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub2').value = `// cin with strings
#include <iostream>
#include <string>
using namespace std;

int main()
{
  string mystr;
  cout << "What's your name? ";
  getline(cin, mystr);
  cout << "Hello " << mystr << "." << endl;
  cout << "What is your favorite team? ";
  getline(cin, mystr);
  cout << "I like " << mystr << " too!" << endl;
  return 0;
}
`;
</script>
</form>
<br>

Output from this program would look something like:

~~~
What's your name? Homer Simpson
Hello Homer Simpson.
What is your favorite team? The Isotopes
I like The Isotopes too!
~~~
{: .output}

Notice how in both calls to `getline`, we used the same string identifier `mystr`. What the program does in the second call is simply replace 
the previous content with the new one that is introduced.

The standard behavior that most users expect from a console program is that each time the program queries the user for input, the user 
types a string then presses ENTER (or RETURN). Therefore, unless you have a strong reason 
not to, you should always use `getline` to get input in your console programs instead of extracting from `cin`.

### `stringstream`

The standard header `<sstream>` defines a type called `stringstream` that allows a string to be treated as a stream, and thus allowing 
extraction or insertion operations from/to strings in the same way as they are performed on `cin` and `cout`. This feature is most useful 
to convert strings to numerical values and vice versa. For example, in order to extract an integer from a string we can write:

~~~
string mystr ("1204");
int myint;
stringstream(mystr) >> myint;
~~~
{: .code}

This declares a string with initialized to a value of 1204, and a variable of type `int`. Then, the third line uses this variable to 
extract from a `stringstream` constructed from the string. This piece of code stores the numerical value 1204 in the variable called `myint`.

~~~
// stringstreams
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

int main()
{
  string mystr;
  float price=0;
  int quantity=0;

  cout << "Enter price: ";
  getline(cin,mystr);
  stringstream(mystr) >> price;
  cout << "Enter quantity: ";
  getline(cin,mystr);
  stringstream(mystr) >> quantity;
  cout << "Total price: " << price*quantity << endl;
  return 0;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub3"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub3').value = `// stringstreams
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

int main()
{
  string mystr;
  float price=0;
  int quantity=0;

  cout << "Enter price: ";
  getline(cin,mystr);
  stringstream(mystr) >> price;
  cout << "Enter quantity: ";
  getline(cin,mystr);
  stringstream(mystr) >> quantity;
  cout << "Total price: " << price*quantity << endl;
  return 0;
}
`;
</script>
</form>
<br>

An example session when running this program is:

~~~
Enter price: 22.25
Enter quantity: 7
Total price: 155.75
~~~
{: .output}

In this example, we acquire numeric values from the standard input indirectly. Instead of extracting numeric values directly from `cin`, however,
we store the lines into a string object `mystr`, and then we extract the values from this string into the variables price and quantity. Once 
these stored in numeric variables, arithmetic operations can be performed on them, such as multiplying them to obtain a total price.

With this approach of getting entire lines and extracting their contents, we separate the process of getting user input from its interpretation 
as data, allowing the input process to be what the user expects, and at the same time gaining more control over the transformation of its ]
content into useful data by the program.
