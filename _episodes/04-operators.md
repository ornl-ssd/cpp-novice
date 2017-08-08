---
title: "Operators"
teaching: 45
exercises: 5
questions:
- "What built-in operators are availble?"
objectives:
- "Learn the different types of operators available in C++."
keypoints:
- "C++ provides an extensive set of arithmetic and logical operators."
---
Operators are used to perform operations on variables and constants. There are a large number of operators in C++, so it is important
to become familiar with the different types and how they are used.

### Assignment operator `=`

The assignment operator assigns a value to a variable.

~~~
x = 5;
~~~
{: .code}

This statement assigns the integer value 5 to the variable `x`. The assignment operation always takes place from right to left, 
and never the other way around.

~~~
x = y;
~~~
{: .code}

This statement assigns the value contained in variable `y` to variable `x`. The original value of `x` at the moment this statement is executed is 
lost and replaced by the value of `y`.

This statement only transfers the value of `y` to `x`. If `y` subsequently changes, it has no effect on the value of `x`.

For example, let's have a look at the following code:

~~~
// assignment operator
#include <iostream>
using namespace std;

int main()
{
  int a, b;         // a:?,  b:?
  a = 10;           // a:10, b:?
  b = 4;            // a:10, b:4
  a = b;            // a:4,  b:4
  b = 7;            // a:4,  b:7

  cout << "a:";
  cout << a;
  cout << " b:";
  cout << b << endl;
}
~~~
{: .code}

This program generates the following output:

~~~
a:4 b:7
~~~
{: .output}

This program prints the final values of `a` and `b`, which are 4 and 7, respectively. Notice how `a` was not affected by the final modification of `b`, even though we declared `a = b` earlier.

#### Assignment operations are expressions that can be evaluated

That means that the assignment itself has a value, which for fundamental types, is the one assigned in the operation. 

For example:

~~~
y = 2 + (x = 5);
~~~
{: .code}

In this expression, `y` is assigned the result of adding 2 and the value of another assignment expression which has a value of 5. 
It is roughly equivalent to:

~~~
x = 5;
y = 2 + x;
~~~
{: .code}

The following expression is also valid in C++: 

~~~
x = y = z = 5;
~~~
{: .code}

It assigns 5 to the all three variables: `x`, `y` and `z`. This assignment is done from right-to-left. First `z` is assigned 5. The result of this
assignment is 5, which is assigned to `y`. The result of this assignment is also 5, which is finally assigned to `x`.

### Arithmetic operators `+`, `-`, `*`, `/`, `%`

The five arithmetical operations supported by C++ are: 

<table border="1">
<tr><th>operator</th><th>description</th></tr>
<tr><td>+</td><td>addition</td></tr>
<tr><td>-</td><td>subtraction</td></tr>
<tr><td>*</td><td>multiplication</td></tr>
<tr><td>/</td><td>division</td></tr>
<tr><td>%</td><td>modulo</td></tr>
</table>

Operations of addition, subtraction, multiplication and division correspond literally to their respective mathematical operators. 
The last one, modulo operator, represented by a percentage sign `%`, gives the remainder of a division of two values. For example:

~~~
x = 11 % 3;
~~~
{: .code}

This results in variable `x` containing the value 2, since dividing 11 by 3 results in 3, with a remainder of 2.

### Compound assignment `+=`, `-=`, `*=`, `/=`, `%=`, `>>=`, `<<=`, `&=`, `^=`, `|=`

Compound assignment operators modify the current value of a variable by performing an operation on it. They are equivalent to 
assigning the result of an operation to the first operand:

<table border="1">
<tr><th>expression</th><th>equivalent to...</th></tr>
<tr><td>y += x;</td><td>y = y + x;</td></tr>
<tr><td>x -= 5;</td><td>x = x - 5;</td></tr>
<tr><td>x /= y;</td><td>x = x / y;</td></tr>
<tr><td>price *= units + 1;</td><td>price = price * (units+1);</td></tr>
</table>

The same applies for all the other compound assignment operators. 

Here is an example of using compound addition:

~~~
// compound assignment operators
#include <iostream>
using namespace std;

int main()
{
  int a, b=3;
  a = b;
  a += 2;             // equivalent to a=a+2
  cout << a << endl;
}
~~~
{: .code}

The output from this program is:

~~~
5
~~~
{: .code}

Recall that when the compound addition operator `+=` is applied to a string, it causes a new string to be appended. So the type of the 
variable being used with the operator determines the actual result. You should not assume that the operation will be arithmetic unless
the variable is an arithmetic type.

### Increment and decrement `++`, `--`

The increment operator `++` and the decrement operator `--` increase or reduce the 
value stored in a variable by one. They are equivalent to `+= 1` and to `-= 1`, respectively. 

The following statements are all functionally equivalent:

~~~
++x;
x += 1;
x = x + 1;
~~~
{: .code}

One peculiarity of these operators is that they can be used both as a prefix and as a suffix to a variable, for example `++x` and `x++`. These 
have an important difference in their meaning. The prefix version of the operator, `++x`, results in an expression that evaluates to the 
*final* value of `x` (i.e. *after* it is increased by one in this case). The suffix version of the operator, `x++`, results in an
expression that evaluates to the value of `x` *before* being increased.

The following examples show the difference:

<table border="1">
<tr><th>Example 1</th><th>Example 2</th></tr>
<tr><td rowspan="3">x = 3;<br>y = ++x;<br>// x contains 4, y contains 4</td>
<td>x = 3;<br>y = x++;<br>// x contains 4, y contains 3</td></tr>
</table>

> ## Challenge
> Which of the following statements are valid? If `i` starts out with the value of 5, what is it's value after each valid statement?
>
> ~~~
> ++(i = 6);
> ++i = 6;
> (++i)++;
> ++(i++);
> ++i++;
> ~~~
> {: .code}
>
> > ## Solution
> > The first three are valid. The value of `i` after each statement (assuming it is 5 before the statement is executed) 
> > will be 7, 6, and 7 respectively.
> >
> {: .solution}
{: .challenge}

### Comparison operators `==`, `!=`, `>`, `<`, `>=`, `<=`

Two expressions can be compared using comparison operators. The result of such an operation is either `true` or `false` (i.e., a Boolean value).

The relational operators in C++ are:

<table border="1">
<tr><th>operator</th><th>description</th></tr>
<tr><td>==</td><td>Equal to</td></tr>
<tr><td>!=</td><td>Not equal to</td></tr>
<tr><td><</td><td>Less than</td></tr>
<tr><td>></td><td>Greater than</td></tr>
<tr><td><=</td><td>Less than or equal to</td></tr>
<tr><td>>=</td><td>Greater than or equal to</td></tr>
</table>

Here there are some examples, assuming that `a=2`, `b=3` and `c=6`. Notice that you can add parentheses around expressions. These can
be used to ensure that the expressions are evaluated in the order you expect.

~~~
(7 == 5)     // evaluates to false
(5 > 4)      // evaluates to true
(3 != 2)     // evaluates to true
(6 >= 6)     // evaluates to true
(5 < 5)      // evaluates to false 
(a == 5)     // evaluates to false, since a is not equal to 5
(a*b >= c)   // evaluates to true, since (2*3 >= 6) is true
(b+4 > a*c)  // evaluates to false, since (3+4 > 2*6) is false
((b=2) == a) // evaluates to true 
~~~
{: .code}

> ## Assignment versus equality
> Note that the equality operator '==' is different from the assignment operator '=' to avoid ambiguity. In the last expression `((b=2) == a)`, 
> the value 2 is first assigned to `b` (the result of which is the value 2) and then compared to `a`.
>
> This distinction is particularly important in C++, since the use of assignment in a logical expression is legal. Other languages, such as
> Python, do not allow assignment to be used in this way.
{: .callout}

### Logical operators `!`, `&&`, `||`

The `!` operator corresponds to the logical operation NOT. It has only one operand, to its right. The result of the operator is
as follows:

<table border="1">
<tr><th colspan="3">! (NOT)</th></tr>
<tr><th>a</th><th>!a</th></tr>
<tr><td>true</td><td>false</td></tr>
<tr><td>false</td><td>true</td></tr>
</table>

For example:

~~~
!(5 == 5)   // evaluates to false because the expression at its right (5 == 5) is true
!(6 <= 4)   // evaluates to true because (6 <= 4) would be false
!true       // evaluates to false
!false      // evaluates to true 
~~~
{: .code}


The operators `&&` and `||` correspond to the logical operations AND and OR respectively, and both take two operands. 
The following tables show the results:

<table border="1">
<tr><th colspan="3">&& (AND)</th></tr>
<tr><th>a</th><th>b</th><th>a && b</th></tr>
<tr><td>true</td><td>true</td><td>true</td></tr>
<tr><td>true</td><td>false</td><td>false</td></tr>
<tr><td>false</td><td>true</td><td>false</td></tr>
<tr><td>false</td><td>false</td><td>false</td></tr>
</table>

<table border="1">
<tr><th colspan="3">|| (OR)</th></tr>
<tr><th>a</th><th>b</th><th>a || b</th></tr>
<tr><td>true</td><td>true</td><td>true</td></tr>
<tr><td>true</td><td>false</td><td>	true</td></tr>
<tr><td>false</td><td>true</td><td>true</td></tr>
<tr><td>false</td><td>false</td><td>false</td></tr>
</table>

For example:

~~~
( (5 == 5) && (3 > 6) )  // evaluates to false ( true && false )
( (5 == 5) || (3 > 6) )  // evaluates to true ( true || false ) 
~~~
{: .code}

Note that when evaluating logical operators, C++ uses *short-circuit* evaluation. This means that the operands are evaluated
from left-to-right, but as soon as the result is known, evaluation stops. For the `&&` operator this occurs when the first
operand evalutes to `false`, since at this point the result will always be `false` regardless of the value of the second operand. For the
`||` operator, the same thing occurs when the first operand is `true.

In the last example `( (5 == 5) || (3 > 6) )` above, first `5 == 5 `is evaluated. The result is `true`, so the comparison `3 > 6` is never
done. 

Here is a summary of short-circuit evaluation:

<table border="1">
<tr><th>operator</th><th>short-circuit</th></tr>
<tr><td>&&</td><td>if the left-hand side expression is false, result is false</td></tr>
<tr><td>||</td><td>if the left-hand side expression is true, the result is true</td></tr>
</table>

Although this may seem somewhat abstract, it becomes very important when the right-hand operand has side effects, such as 
in the following example:

~~~
if ( (i<10) && (++i<n) ) { /*...body...*/ }   // note that the condition increments variable i 
~~~
{: .code}

Here, the combined conditional expression will increment `i` by one only if the condition on the left of `&&` is `true`.
If the condition is `false` then the right-hand side `(++i<n)` is never evaluated.

> ## Challenge
> Suppose that `i` has the value 5 and `n` has the value 5. Will the body `if` statement be executed? What will the values
> of `i` and `n` be after the statement is executed?
>
> > ## Solution
> > No, the body will not be executed. Since `i` is less than 10, `i<10` is true and the second part of the `&&` expression 
> > will be evaluated. The `++i` expression will cause the value of `i` to be incremented by 1 to 6, so it will be greater than `n` 
> > and so `++i<n` will be false. The result of `true && false` is `false`, so the body of the `if` statement is not executed.
> >
> > At the end of the statement, the value of `i` will be 6 and the value of `n` will be 5.
> {: .solution}
{: .challenge}

## Advance Topics 

### Conditional ternary operator: `?`

The conditional operator evaluates an expression, returning one value if that expression evaluates to true, and a different one if 
the expression evaluates as false. Its syntax is:

~~~
condition ? result1 : result2 
~~~
{: .code}

If `condition` is true, the entire expression evaluates to `result1`, and otherwise it evaluates to `result2`.

~~~
7==5+2 ? 4 : 3   // evaluates to 4, since 7 is equal to 5+2.
5>3 ? a : b      // evaluates to the value of a, since 5 is greater than 3.
a>b ? a : b      // evaluates to whichever is greater, a or b.  
~~~
{: .code}

For example:

~~~
// conditional operator
#include <iostream>
using namespace std;

int main()
{
  int a,b,c;

  a=2;
  b=7;
  c = (a>b) ? a : b;

  cout << c << endl;
}
~~~
{: .code}

The result from running this program is:

~~~
7
~~~
{: .output}

In this example, `a` is 2, and `b` is 7, so the expression being evaluated `(a>b)` is `false`. The conditional operator returns
the second value (the one after the colon) in this case, which was `b` (with a value of 7).

### Comma operator: `,`

The comma operator is used to separate two or more expressions where only one expression is expected. The expressions are 
executed in order from left to right, and the result of the operator is the value of the right-most expression.

For example:

~~~
a = (b=3, b+2);
~~~
{: .code}

This code would first assign the value 3 to `b`, and then assign `b+2` to variable `a`.

### Bitwise operators: `&`, `|`, `^`, `~`, `<<`, `>>`

Bitwise operators are used to manipulate variables as if they were sequences of bits. These are low level operations that are
typically used for implementing programs that operate close to the computer's hardware. The following table shows the operations
that are available:

<table border="1">
<tr><th>operator</th><th>asm equivalent</th><th>description</th></tr>
<tr><td>&</td><td>AND</td><td>Bitwise AND</td></tr>
<tr><td>|</td><td>OR</td><td>Bitwise inclusive OR</td></tr>
<tr><td>^</td><td>XOR</td><td>Bitwise exclusive OR</td></tr>
<tr><td>~</td><td>NOT</td><td>Unary complement (bit inversion)</td></tr>
<tr><td><<</td><td>SHL</td><td>Shift bits left</td></tr>
<tr><td>>></td><td>SHR</td><td>Shift bits right</td></tr>
</table>

### Explicit type casting operator: `( type )`

Type casting operators enable a value to be converted from one type to another type. There are several ways to do this in C++. The simplest one, 
which has been inherited from the C language, is to precede the expression to be converted by the new type enclosed between parentheses:

~~~
int i;
float f = 3.14;
i = (int) f;
~~~
{: .code}

The previous code converts the floating-point number 3.14 to an integer value 3. Since the integer can only represent integral values, the
remaining decimals are lost. Here, the type casting operator 
was `(int)`. Type casting can only be performed with compatible types.

C++ also provides a functional notation for type casting, as follows:
 
~~~
i = int(f);
~~~
{: .code}

Both ways of casting types are valid in C++.

### `sizeof` operator

This operator accepts one parameter, which can be either a type or a variable, and returns the size in bytes of that type or object:

~~~
x = sizeof (char);
~~~
{: .code}

Here, `x` is assigned the value 1, because char is a type with a size of one byte.

The value returned by `sizeof` is a compile-time constant, so it is always determined before program execution.

### Precedence of operators

A single expression may have multiple operators. For example:

~~~
x = 5 + 7 % 2;
~~~
{: .code}

In C++, the above expression always assigns 6 to variable `x`, because the `%` operator has a higher *precedence* than the `+` operator.
The precedence determines in which order the operators are evaluated. Operators with the highest precendence are evaluated first,
then the next highest, and so on. Operators of the same precendence are evaluated in the order specified by their *grouping*.
 
Precendence order can be overridden by encolsing an expression in parenthesis. The parenthasised expressons are always evaluated first
in grouping order. Notice the difference:

~~~
x = 5 + (7 % 2);    // x = 6 (same as without parenthesis)
x = (5 + 7) % 2;    // x = 0 
~~~
{: .code}

The following table shows the precendence for all C++ operators:

<table border="1">
<tr><th>Precedence order</th><th>Precedence group</th><th>Operator</th><th>Description</th><th>Grouping</th></tr>
<tr><td>1</td><td>Scope</td><td>::</td><td>scope qualifier</td><td>Left-to-right</td></tr>
<tr><td rowspan="4">2</td><td rowspan="4">Postfix (unary)</td><td>++ --</td><td>postfix increment / decrement</td><td rowspan="4">Left-to-right</td></tr>
<tr><td>()</td><td>functional forms</td></tr>
<tr><td>[]</td><td>subscript</td></tr>
<tr><td>. -></td><td>member access</td></tr>
<tr><td rowspan="7">3</td><td rowspan="7">Prefix (unary)</td><td>++ --</td><td>prefix increment / decrement</td><td rowspan="7">Right-to-left</td></tr>
<tr><td>~ !</td><td>bitwise NOT / logical NOT</td></tr>
<tr><td>+ -</td><td>unary prefix</td></tr>
<tr><td>& *</td><td>reference / dereference</td></tr>
<tr><td>new delete</td><td>allocation / deallocation</td></tr>
<tr><td>sizeof</td><td>parameter pack</td></tr>
<tr><td>(type)</td><td>C-style type-casting</td></tr>
<tr><td>4</td><td>Pointer-to-member</td><td>.* ->*</td><td>access pointer</td><td>Left-to-right</td></tr>
<tr><td>5</td><td>Arithmetic: scaling</td><td>* / %</td><td>multiply, divide, modulo</td><td>Left-to-right</td></tr>
<tr><td>6</td><td>Arithmetic: addition</td><td>+ -</td><td>addition, subtraction</td><td>Left-to-right</td></tr>
<tr><td>7</td><td>Bitwise shift</td><td><< >></td><td>shift left, shift right</td><td>Left-to-right</td></tr>
<tr><td>8</td><td>Relational</td><td>< > <= >=</td><td>comparison operators</td><td>Left-to-right</td></tr>
<tr><td>9</td><td>Equality</td><td>== !=</td><td>equality / inequality</td><td>Left-to-right</td></tr>
<tr><td>10</td><td>And</td><td>&</td><td>bitwise AND</td><td>Left-to-right</td></tr>
<tr><td>11</td><td>Exclusive or</td><td>^</td><td>bitwise XOR</td><td>Left-to-right</td></tr>
<tr><td>12</td><td>Inclusive or</td><td>|</td><td>bitwise OR</td><td>Left-to-right</td></tr>
<tr><td>13</td><td>Conjunction</td><td>&&</td><td>logical AND</td><td>Left-to-right</td></tr>
<tr><td>14</td><td>Disjunction</td><td>||</td><td>logical OR</td><td>Left-to-right</td></tr>
<tr><td rowspan="3">15</td><td rowspan="3">Assignment-level expressions</td><td>= *= /= %= += -=</td><td rowspan="2">assignment / compound assignment</td><td rowspan="3">Right-to-left</td></tr>
<tr><td>>>= <<= &= ^= |=</td></tr>
<tr><td>?:</td><td>conditional operator</td></tr>
</table>

Enclosing all sub-statements in parentheses (even those unnecessary because of their precedence) improves code readability.
