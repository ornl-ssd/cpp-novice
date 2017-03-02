---
title: "Basics of C++"
teaching: 15
exercises: 15
questions:
- "How can I make my results easier to reproduce?"
objectives:
- "Explain what Make is for."
- "Explain why Make differs from shell scripts."
- "Name other popular build tools."
keypoints:
- "Make allows us to specify what depends on what and how to update things that are out of date."
---

### Structure of a Program

We saw how to write a simple "Hello World" program in the C/C++ Development Tools lesson. Lets look at this program in more detail:

~~~
1 // My first Hello World program
2 #include <iostream>
3 using namespace std;
4
5 int main() 
6 {
7 	cout << "Hello World!" << endl;
8 	return 0;
9 }
~~~
{: .code}

Let's examine this program line by line:

Line 1: // My first Hello World program
: Two slash signs indicate that the rest of the line is a comment inserted by the programmer but which has no effect on the behavior of the 
program. Programmers use them to include short explanations or observations concerning the code or program. In this case, it is a brief 
introductory description of the program.

Line 2: #include <iostream>
: Lines beginning with a hash sign `#` are directives read and interpreted by what is known as the preprocessor. They are special lines 
interpreted before the compilation of the program itself begins. In this case, the directive `#include <iostream>`, instructs the preprocessor 
to include a section of standard C++ code, known as *header iostream*, that allows to perform standard input and output operations, such as 
writing the output of this program (Hello World) to the screen.

Line 3: using namespace std;
: Namespaces provide a method for preventing name conflicts in large projects. By specifying that we want to use the `std` namespace, we
don't have to prefix identifiers with `std::` in order to be able to use them. This helps improve readability of the code. 

Line 4: A blank line.
: Blank lines have no effect on a program. They simply improve readability of the code.

Line 5: int main () {
: This line initiates the declaration of a function. Essentially, a function is a group of code statements which are given a name: in this case, 
this gives the name "main" to the group of code statements that follow. Functions will be discussed in detail in a later chapter, but 
essentially, their definition is introduced with a succession of a type `int`, a name `main` and a pair of parentheses `()`, optionally 
including parameters.
The function named `main` is a special function in all C++ programs; it is the first function called when the program is started. The execution 
of all C++ programs begins with the `main` function, regardless of where the function is actually located within the code.

Lines 6 and 8: { and }
: The open brace `{` at line 6 indicates the beginning of `main`'s function definition, and the closing brace `}` at line 8, indicates its end. 
Everything between these braces is the function's body that defines what happens when main is called. All functions use braces to indicate 
the beginning and end of their definitions.

Line 7: cout &lt;&lt; "Hello World!" &lt;&lt; endl;
: This line is a C++ statement. A statement is an expression that can actually produce some effect. It is the meat of a program, specifying its 
actual behavior. Statements are executed in the same order that they appear within a function's body.
This statement has three parts: First, `cout`, which identifies the standard character output device (usually, this is the computer screen). 
If we did not specify the `std` namespace, then we would need to refer to this is `std::cout`. Second, the insertion operator `&lt;&lt;`, which indicates 
that what follows is inserted into `cout`. Finally, a sentence within quotes `"Hello world!"`, is the content inserted into the standard output.
Notice that the statement ends with a semicolon `;`. This character marks the end of the statement, just as the period ends a sentence in 
English. All C++ statements must end with a semicolon character. One of the most common syntax errors in C++ is forgetting to end a 
statement with a semicolon.