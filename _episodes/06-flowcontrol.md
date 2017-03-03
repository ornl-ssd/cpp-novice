---
title: "Statements and Control of Flow"
teaching: 15
exercises: 15
questions:
objectives:
keypoints:
---
A simple C++ statement is each of the individual instructions of a program, like the variable declarations and expressions seen in 
previous sections. They always end with a semicolon, and are executed in the same order in which they appear in a program.

However, programs are not limited to a linear sequence of statements. During its process, a program may repeat segments of code, 
or take decisions to execute different sections of code. For that purpose, C++ provides flow control statements that serve to 
specify what has to be done by our program, when, and under which circumstances.

Flow control operations generally act on a block of code. This block of code may be a single C++ statement, terminated by a semicolon,
or a compound statement. A compound statement is a group of statements, each of them terminated by its own semicolon. The grouping is
indicated by surrounding the statements with curly braces as follows:

~~~
{ statement1; statement2; statement3; } 
~~~
{: .code}

C++ considers this entire block as a single statement.

### Selection statements: `if` and `else`

The `if` keyword is used to execute a statement or block, *if, and only if*, a condition is fulfilled. Its syntax is:

~~~
if (condition) statement
~~~
{: .code}

Here, `condition` is an expression that is to be evaluated. If the result of the expression is true, `statement` is executed. If it is false, 
`statement` is not executed and is simply ignored. The program continues right after the entire `if` statement.

For example, the following code fragment prints the message `x is 100`, only if the value stored in the `x` variable is indeed 100:

~~~
if (x == 100)
  cout << "x is 100" << endl;
~~~
{: .code}

Remember that C++ allows any kind of spaces or indenting, so we've moved the statement to the following line to make it more readable. 

If you want to include more than a single statement to be executed when the condition is true, 
these statements are enclosed in a block as follows:

~~~
if (x == 100)
{
   cout << "x is ";
   cout << x << endl;
}
~~~
{: .code}

As usual, indentation and line breaks in the code have no effect, so the above code is equivalent to:

~~~
if (x == 100) { cout << "x is "; cout << x; }
~~~
{: .code}

Selection statements with `if` can also specify what happens when the condition is not fulfilled, by using the`else` keyword to 
introduce an alternative statement. Its syntax is:

~~~
if (condition) statement1 else statement2
~~~
{: .code}

In this case, `statement1` is executed when `condition` is true, and `statement2` is executed when it is false.

For example:

~~~
if (x == 100)
  cout << "x is 100" << endl;
else
  cout << "x is not 100" << endl;
~~~
{: .code}

This prints `x is 100`, if indeed `x` has a value of 100. If `x` has any other value, it prints `x is not 100` instead.
The `else` keyword can be combined with `if` to check a range of conditions. For example:

~~~
if (x > 0)
  cout << "x is positive" << endl;
else if (x < 0)
  cout << "x is negative" << endl;
else
  cout << "x is 0" << endl;
~~~
{: .code}

This prints `x is positive` if `x` is greater than zero, `x is negative` if `x` is less than zero, or `x is 0` if `x` is exactly equal to zero. 

>## Abiguity
>
> Although C++ formatting is very flexible compared to languages such as Python, it can easily lead to confusion and mistakes. For example, 
> suppose the above code looked like this:
>
> ~~~ 
> if (x > 0)
>  cout << "x is positive" << endl;
>  cout << "have a great day" << endl;
> ~~~
> {: .code}
>
> In the case where `x` is less than or equal to zero, you might expect not to see any output. However, you will actually see this:
>
> ~~~
> have a great day
> ~~~ 
> {: .output}
>
> Why is this? Recall that the `if` statement takes a single statement or a block of statements. The programmer has indented both
> statements thinking that they will both be executed if the condition is true, however as they are not enclosed in braces, only
> the first statement would be executed. The second statement is executed regardless of the value of the condition.
>
> The correct code is:
>
> ~~~ 
> if (x > 0) {
>  cout << "x is positive" << endl;
>  cout << "have a great day" << endl;
> }
> ~~~
> {: .code}
{: .callout}

### Iteration statements (loops)

Loops repeat a statement a certain number of times, or while some condition is fulfilled. They are introduced by the keywords `while`, 
`do`, and `for`.

#### The `while` loop

The simplest kind of loop is the `while` loop. Its syntax is:

~~~
while (expression) statement
~~~
{: .code}

The `while` loop simply repeats statement *while* the `expression` is true. If, after any execution of `statement`, `expression` is no longer 
true, the loop ends, and the program continues right after the loop. For example, let's have a look at a countdown using a `while` loop:

~~~
// custom countdown using while
#include <iostream>
using namespace std;

int main()
{
  int n = 10;

  while (n > 0) {
    cout << n << ", ";
    --n;
  }

  cout << "liftoff!\n";
}
~~~
{: .code}

This code generates the following output:

~~~
10, 9, 8, 7, 6, 5, 4, 3, 2, 1, liftoff!
~~~
{: .output}

The first statement in main sets `n` to a value of 10. This is the first number in the countdown. Then the `while` loop begins. 
If this value fulfills the condition `n > 0`, then the block that follows the condition is executed, and repeated for as long as the 
condition remains true. The *body* of the loop does two things. First it outputs the calue of `n` followed by a comma, then it
decrements the value of `n`	by one. The loop condition is now evaluated again, and because it is still true, the loop body will again
be executed. This will continue until the value of `n` becomes 0, at which point the loop will finish, and the string `liftoff!` will
be output.

An important thing to consider with `while` loops is that the loop should end at some point, and thus the statement must alter value (or values) checked in 
the condition in some way. Otherwise, the loop will continue forever. 

#### The `do-while` loop

A very similar loop is the `do-while` loop, whose syntax is:

~~~
do statement while (condition);
~~~
{: .code}

This loop behaves like a `while` loop, except that `condition` is evaluated *after* the execution of statement instead of before. This
guarantees at least one execution of `statement`, even if `condition` is never fulfilled. For example, the following example program 
echoes any text the user introduces until the user enters "goodbye":

~~~
// echo machine
#include <iostream>
#include <string>
using namespace std;

int main()
{
  string str;
  do {
    cout << "Enter text: ";
    getline (cin,str);
    cout << "You entered: " << str << '\n';
  } while (str != "goodbye");
}
~~~
{: .code}

Sample output would be:

~~~
Enter text: hello
You entered: hello
Enter text: who's there?
You entered: who's there?
Enter text: goodbye
You entered: goodbye
~~~
{: .output}

The `do-while` loop is usually preferred over a `while` loop when the statement needs to be executed at least once, such as when the 
condition that is checked to end of the loop is determined within the loop statement itself. In the previous example, the user 
input within the block is what will determine if the loop ends. And thus, even if the user wants to end the loop as soon as possible 
by entering goodbye, the block in the loop needs to be executed at least once to prompt for input, and the condition can, in fact, 
only be determined after it is executed.

#### The `for` loop

The `for` loop is designed to iterate a number of times. Its syntax is:

~~~
for (initialize; condition; modify) statement;
~~~
{: .code}

Like the `while` loop, the `for` loop repeats `statement` while `condition` is true. However, in addition, the `for` loop provides specific 
the ability to initialize and and modify values. The `initialize` section is executed before the loop begins the first time, and 
the `modify` section is executed immediately after each iteration, but before `condition` is checked.

The `for` loop is identical to the following `while` loop:

~~~
initialize;
while (condition) { statement; modify; }
~~~
{: .code}

Here is the same countdown example using a `for` loop instead:

~~~
// countdown using a for loop
#include <iostream>
using namespace std;

int main()
{
  for (int n = 10; n > 0; n--) {
    cout << n << ", ";
  }
  cout << "liftoff!\n";
}
~~~
{: .code}

The three fields in a `for` loop are optional. They can be left empty, but in all cases the semicolon signs between them are still required. 
For example, `for (; n < 10;)` and `for (; n < 10; ++n)` are both valid `for` loops. 

A loop with no condition is equivalent to a loop with true as condition (i.e., an infinite loop).

Because each of the fields is executed in a particular time in the lifecycle of a loop, it may be useful to execute more than a single 
expression as any of `initialize`, `condition`, or `modify`. Unfortunately, these are not statements, but rather, simple expressions, 
and thus cannot be replaced by a block. It is possible to make use of the comma operator, however, as a way of separating multiple
experssions as follows:

~~~
for ( n=0, i=100 ; n!=i ; ++n, --i )
{
   // whatever here...
}
~~~
{: .code}

This loop will execute 50 times if neither `n` or `i` are modified within the loop:

#### Range-based `for` loop

The `for` loop has another syntax, which is used exclusively with ranges:

~~~
for ( declaration : range ) statement;
~~~
{: .code}

This kind of `for` loop iterates over all the elements in `range`, where `declaration` declares some variable able to take the value of an 
element in this range. Ranges are sequences of elements, including arrays, containers, and any other type supporting the functions 
`begin` and `end`. Most of these types have not yet been introduced in this tutorial, but we are already acquainted with at least one 
kind of range: strings, which are sequences of characters.

An example of range-based for loop using strings:

~~~
// range-based for loop
#include <iostream>
#include <string>
using namespace std;

int main()
{
  string str {"Hello!"};
  for (char c : str)
  {
    cout << "[" << c << "]";
  }
  cout << '\n';
}
~~~
{: .code}

This program generates the oputput:

~~~
[H][e][l][l][o][!]
~~~
{: .output}

Note how what precedes the colon in the `for` loop is the declaration of a `char` variable (the elements in a `string` are of type `char`). 
We then use this variable, `c`, in the statement block to represent the value of each of the elements in the range.

This loop is automatic and does not require the explicit declaration of any counter variable.

Range based loops usually also make use of type deduction for the type of the elements with auto. Typically, the range-based loop 
above can also be written as:

~~~
for (auto c : str)
  cout << "[" << c << "]";
~~~
{: .code}

Here, the type of `c` is automatically deduced as the type of the elements in `str`.

### Jump statements

Jump statements allow the flow of a program to be changed by performing jumps to specific locations.

#### The `break` statement

The `break` statement immediately leaves a loop, even if the condition for its end is not fulfilled. It can be used to end an infinite 
loop, or to force it to end before its condition becomes false. For example, let's stop the countdown before its natural end:

~~~
// break loop example
#include <iostream>
using namespace std;

int main()
{
  for (int n=10; n>0; n--)
  {
    cout << n << ", ";
    if (n==3)
    {
      cout << "countdown aborted!";
      break;
    }
  }
}
~~~
{: .code}

Here we see the output

~~~
10, 9, 8, 7, 6, 5, 4, 3, countdown aborted!
~~~
{: .output}


#### The `continue` statement

The `continue` statement causes the program to skip the rest of the loop in the current iteration, as if the end of the statement block had 
been reached. The loop will immediately start the following iteration. For example, let's skip number 5 in our countdown:

~~~
// continue loop example
#include <iostream>
using namespace std;

int main()
{
  for (int n=10; n>0; n--) {
    if (n==5) continue;
    cout << n << ", ";
  }
  cout << "liftoff!\n";
}
~~~
{: .code}

In this case, we now see the output:

~~~
10, 9, 8, 7, 6, 4, 3, 2, 1, liftoff!
~~~
{: .output}

Notice that the number 5 is missing.

#### The `goto` statement

The `goto` statement is rarely used, but it allows an absolute jump to another point in the program. This unconditional jump ignores nesting 
levels, and does not cause any automatic stack unwinding. Therefore, it is a feature to use with care, and preferably within the same block 
of statements, especially in the presence of local variables.

The destination point of the `goto` is identified by a label, which is then used as an argument for the `goto` statement. A label is made 
of a valid identifier followed by a colon.

The `goto` statement is generally deemed a low-level feature, with no particular use cases in modern higher-level programming paradigms 
generally used with C++. But, just as an example, here is a version of our countdown loop using `goto`:

~~~
// goto loop example
#include <iostream>
using namespace std;

int main ()
{
  int n=10;
mylabel:
  cout << n << ", ";
  n--;
  if (n>0) goto mylabel;
  cout << "liftoff!\n";
}
~~~
{: .code}

### The `switch` statement

The syntax of the `switch` statement is a bit peculiar. Its purpose is to check for a value among a number of possible constant expressions. 
It is similar to concatenating `if-else` statements, but limited to checking the values against constant expressions. Its most typical 
syntax is:

~~~
switch (expression)
{
  case constant1:
     group-of-statements-1;
     break;
  case constant2:
     group-of-statements-2;
     break;
  .
  .
  .
  default:
     default-group-of-statements
}
~~~
{: .code}

The `switch` statement works in the following way. The `expression` is first evaluated and tested to see if it is equivalent to `constant1`.
If it is, the `group-of-statements-1` are executed until a `break` statement is encounted. At this point the program jumps to the end of the 
entire `switch` statement (following the closing brace).

If `expression` was not equal to `constant1`, it is then checked against `constant2`. If it is equal, then `group-of-statements-2` are executed until 
a `break` is encountered, at which point it jumps to the code following the `switch`.

Comparion proceeds for each `case` until one is matched, or if the value of `expression` does not match any of the previously specified 
constants (there may be any number of these), the program executes the statements included after the `default:` label, if it exists
(since it is optional).

Both of the following code fragments have the same behavior, demonstrating the `if-else` equivalent of a `switch` statement:

<table border="1">
<tr><th>switch example</th><th>if-else equivalent</th></tr>
<tr><td><pre>
switch (x) {
  case 1:
    cout << "x is 1";
    break;
  case 2:
    cout << "x is 2";
    break;
  default:
    cout << "value of x unknown";
  }
</pre></td>
<td><pre>
if (x == 1) {
  cout << "x is 1";
}
else if (x == 2) {
  cout << "x is 2";
}
else {
  cout << "value of x unknown";
}
</pre></td></tr>
</table>

The `switch` statement has a somewhat peculiar syntax inherited from the early times of the first C compilers, because it uses labels 
instead of blocks. In the most typical use (shown above), this means that `break` statements are needed after each group of statements 
for a particular label. If `break`1 is not included, all statements following the case (including those under any other labels) are 
also executed, until the end of the `switch` block or a jump statement (such as `break`) is reached.

If the example above lacked the `break` statement after the first group for the first `case`, the program would not jump 
automatically to the end of the `switch` block after printing `x is 1`, and would instead continue executing the 
statements in the second `case`, printing `x is 2`. It would then continue executing until a `break` statement was 
encountered, or the end of the `switch` block reached. This makes unnecessary to enclose the statements for each case in 
braces `{}`, and can also be useful to execute the same group of statements for different possible values. 

For example: 

~~~
switch (x) {
  case 1:
  case 2:
  case 3:
    cout << "x is 1, 2 or 3";
    break;
  default:
    cout << "x is not 1, 2 nor 3";
  }
~~~
{: .code}

Notice that `switch` is limited to compareing its evaluated expression against labels that are constant expressions. It is not possible 
to use variables or ranges as labels, because they are not valid C++ constant expressions. If you wish to do this,
as series of `if` and `else if` statements would be a better approach.
