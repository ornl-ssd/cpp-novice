---
title: "Constants"
teaching: 20
exercises: 0
questions:
- "What are the different types of literal constants in C++?"
- "How are constant expressions defined?"
objectives:
- "Learn about the syntax of literal constants and constant expressions."
keypoints:
- "Literals are available for all the fundamental data types."
- "Constant expressions can be defined using the const keyword."
---
Constants are expressions with a fixed value.

### Literals

Literals are the most obvious kind of constants. They are used to express particular values within the source code of a program. We have 
already used some in previous lessons to give specific values to variables, or for messages we wanted our programs to print out. 

For example:

~~~
a = 5;
~~~
{: .code}

The 5 in this piece of code is a literal constant.

Literal constants can be classified as integer, floating-point, characters, strings, Boolean, pointers, and user-defined literals.

#### Integer Literals

~~~
1776
707
-273
~~~
{: .code}

These are numerical constants that identify integer values. Notice that they are not enclosed in quotes or any other special character. 
They are a simple succession of digits representing a whole number in base 10. 

In addition to decimal numbers, C++ allows the use of octal numbers (base 8) and hexadecimal numbers (base 16) as literal constants. For 
octal literals, the digits are preceded with a 0. For hexadecimal, they are preceded by the characters "0x" or "0X", and the letters 
"a" to "f" can be either lower or upper case. For example, the following 
literal constants are all equivalent to each other: 

~~~
75         // decimal
0113       // octal
0x4B       // hexadecimal  
~~~
{: .code}

All of these represent the same number, 75.

Literal constants have a type, just like variables. By default, integer literals are of type `int`. However, certain *suffixes* may be 
appended to an integer literal to specify a different integer type:

<table border="1">
<tr><th>Suffix</th><th>Type modifier</th></tr>
<tr><td>u or U</td><td>unsigned</td></tr>
<tr><td>l or L</td><td>long</td></tr>
<tr><td>ll or LL</td><td>long long</td></tr>
</table>

The unsigned suffix may be combined with any of the other two in any order to form `unsigned long` or `unsigned long long`. For example:

~~~
75         // int
75u        // unsigned int
75l        // long
75ul       // unsigned long 
75lu       // unsigned long 
~~~
{: .code}

The suffix can be specified using either upper or lowercase letters.

#### Floating Point Numerals

Floating point numbers express real values, with decimals and/or exponents. They can include either a decimal point, an `e` character 
(that expresses 10<sup>n</sup>, where `n` is an integer value that follows the `e` character), or both a decimal point and 
an `e` character. The `e` can be upper or lower case:

~~~
3.14159    // 3.14159
6.02e23    // 6.02 x 10^23
1.6E-19    // 1.6 x 10^-19
3.0        // 3.0  
~~~
{: .code}

These are four valid numbers with decimals expressed in C++. The first number is PI, the second one is the number of Avogadro, the third is 
the electric charge of an electron (an extremely small number) -all of them approximated-, and the last one is the number three expressed as 
a floating-point numeric literal.

The default type for floating-point literals is `double`. Floating-point literals of type `float` or `long double` can be specified by adding 
one of the following suffixes:

<table border="1">
<tr><th>Suffix</th><th>Type</th></tr>
<tr><td>f or F</td><td>float</td></tr>
<tr><td>l or L</td><td>long double</td></tr>
</table>

For example:

~~~
3.14159L   // long double
6.02e23f   // float  
~~~
{: .code}

Any of the letters that can be part of a floating-point numerical constant (`e`, `f`, `l`) can be written using either lower or uppercase 
letters with no difference in meaning.

#### Character and string literals

Character and string literals are enclosed in quotes:

~~~
'z'
'p'
"Hello world"
"How do you do?"
~~~
{: .code}

The first two expressions represent single-character literals, and the following two represent string literals composed of several characters. 
Notice that to represent a single character, we enclose it between single quotes `'`, and to express a string (which consists of one or more 
characters), we enclose the characters between double quotes `"`. A single character string and a single character are different types.

Both single-character and string literals require quotation marks surrounding them to distinguish them from possible variable identifiers or 
reserved keywords. Notice the difference between these two expressions:

~~~
x
'x'
~~~
{: .code}

Here, `x` alone would refer to an identifier, such as the name of a variable or a compound type, whereas `'x'` 
would refer to the character literal `x`.

Character and string literals can also represent special characters that are difficult or impossible to express otherwise in the source 
code of a program, like newline or tab characters. These special characters are preceded by a backslash character `\`.

The following table provides a list of some of the commonly used single character escape codes: 

<table border="1">
<tr><th>Escape code</th><th>Description</th></tr>
<tr><td>\n</td><td>newline</td></tr>
<tr><td>\r</td><td>carriage return</td></tr>
<tr><td>\t</td><td>tab</td></tr>
<tr><td>\v</td><td>vertical tab</td></tr>
<tr><td>\b</td><td>backspace</td></tr>
<tr><td>\f</td><td>form feed (page feed)</td></tr>
<tr><td>\a</td><td>alert (beep)</td></tr>
<tr><td>\'</td><td>single quote</td></tr>
<tr><td>\"</td><td>double quote</td></tr>
<tr><td>\?</td><td>question mark</td></tr>
<tr><td>\\</td><td>backslash</td></tr>
</table>

For example:

~~~
'\n'
'\t'
"Left \t Right"
"one\ntwo\nthree"
~~~
{: .code}

Internally, computers represent characters as numerical codes. Usually this is an extension of the ASCII character encoding 
system (see [ASCII codes](http://www.cplusplus.com/ascii) for more info). Characters can also be represented in literals by using
a numerical code by writing a backslash character followed by the code expressed as an octal (base-8) or hexadecimal (base-16) number. 
For an octal value, the backslash is followed directly by the digits. For hexadecimal, an `x` character is inserted between the 
backslash and the hexadecimal digits themselves (for example: `\x20` or `\x4A`).

Several string literals can be concatenated to form a single string literal simply by separating them by one or more blank spaces, 
including tabs, newlines, and other valid blank characters. For example:

~~~
"this forms" "a single"     " string "
"of characters"
~~~
{: .code}

The above is a string literal equivalent to:

~~~
"this formsa single string of characters"
~~~
{: .code}

Note how spaces within the quotes are part of the literal, while those outside them are not.

A backslash at the end of line can also be used to manage long strings. This is a
line-continuation character that merges both the line and the subsequent line into a single line. The following shows two equivalent forms of code:

~~~
x = "this is a very very long \
string"
x = "this is a very very long string"
~~~
{: .code}

All the character literals and string literals described above are comprised of characters of type `char`. A different character type can be 
specified by using one of the following prefixes:

<table border="1">
<tr><th>Prefix</th><th>Character Type</th></tr>
<tr><td>u</td><td>char16_t</td></tr>
<tr><td>U</td><td>char32_t</td></tr>
<tr><td>L</td><td>wchar_t</td></tr>
</table>

Note that, unlike type suffixes for integer literals, these prefixes are *case sensitive*.

There are two additional prefixes available for string literals:

<table border="1">
<tr><th>Prefix</th><th>Description</th></tr>
<tr><td>u8</td><td>The string literal is encoded in the executable using UTF-8</td></tr>
<tr><td>R</td><td>The string literal is a raw string</td></tr>
</table>

In raw strings, backslashes and single and double quotes are all valid characters. The content of the literal is delimited by an initial 
`R"`*sequence*`(` and a final `)`*sequence*`"`, where *sequence* is any sequence of characters (including an empty sequence). The content of 
the string is what is inside the parenthesis, ignoring the delimiting sequence itself. For example:

~~~
R"(string with \backslash)"
R"&%$(string with \backslash)&%$"
~~~
{: .code}

Both strings above are equivalent to `"string with \\backslash"`. The `R` prefix can be combined with any other prefixes, such as `u`, `L` or `u8`.

#### Other literals

Three keyword literals exist in C++: 

* `true` and `false` -  the two possible values for variables of type `bool`
* `nullptr` -  is the null pointer value.

~~~
bool foo = true;
bool bar = false;
int* p = nullptr;
~~~
{: .code}

### Typed constant expressions

Sometimes, it is just convenient to give a name to a constant value:

~~~
const double pi = 3.1415926;
const char tab = '\t';
~~~
{: .code}

These are just like initiliazed variables, however their value cannot be changed. We can then use these names instead of the 
literals themselves:

~~~
#include <iostream>
using namespace std;

const double pi = 3.14159;
const char newline = '\n';

int main()
{
  double r=5.0;               // radius
  double circle;

  circle = 2 * pi * r;
  cout << circle;
  cout << newline;
}
~~~
{: .code}

<form target="_blank" method="post" action="http://cpp.sh/">
<input type="hidden" name="source" id="sub1"/>
<input type="submit" value="Try running it"/>
<script type="text/javascript">
document.getElementById('sub1').value = `#include <iostream>
using namespace std;

const double pi = 3.14159;
const char newline = '\n';

int main()
{
  double r=5.0;               // radius
  double circle;

  circle = 2 * pi * r;
  cout << circle;
  cout << newline;
}
`;
</script>
</form>
<br>
