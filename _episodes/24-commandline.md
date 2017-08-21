---
title: "Command Line Arguments"
teaching: 20
exercises: 0
questions:
- "How can programs deal with command line arguments?"
objectives:
- "Learn how to read and interpret arguments passed to the program."
keypoints:
- "Special parameters in the main function are used to handle command-line parameters."
- "Each parameter is a string, and special processing must be performed by the program"
---
Most operating systems like Linux, Mac OS X, or Windows provide a shell that allows the user to enter commands and their associated arguments. A command is normally just a regular program, and these arguments are passed as parameters to the program at run-time by the operating system. Exactly how this is done is beyond the scope of this article (on Windows, look up CreateProcess; on UNIX and UNIX-like systems look up fork(3) and exec(3) in the manual). 

The main uses for command-line arguments are for:

* Modifying program behavior - arguments can be used to tell a program how you expect it to behave; for example, some programs have a -q (quiet) option to tell them not to output as much text.
* Having a program run without user interaction - this is especially useful for programs that are called from scripts or other programs.

## Accessing arguments

Adding the ability to parse command-line arguments to a program is very easy. Every C++ program has a `main` function, and so far we have been
using it without the capability to parse its command-line, like this:
 
~~~
int main() {
  .. ... ..
}
~~~
{: .code}

In order to be able to access the command-line arguments, two parameters must be added to `main` which are, by convention, called `argc` (argument count) and `argv` (argument vector). While the names of the parameters are optional, the types are not. The `argc` parameter must have the type `int` and the `argv` parameter must have a type of either `char**` or `char* []`. Also, the order is very important. 

Once you do this, `main` will now looks like:

~~~
int main(int argc, char* argv[]) {
  .. ... ..
}
~~~
{: .code}

The `argc` parameter tells the program how many command-line arguments were included on the command line. It is always at least 1, because the first argument is the name of the command used to invoke the program. The `argv` array contains the actual command-line arguments as an array of strings, the first of which is the program's name. 

The following example show how to display the program's name:

~~~
#include <iostream>

int main(int argc, char* argv[])
{
    std::cout << argv[0] << std::endl;
    return(0);
}
~~~
{: .code}

This program will print the name of the command you used to run it: if you called the executable "`a.exe`" (Windows) or "`a.out`" (UNIX) it would likely print "`a.exe`" or "`./a.out`" respectively.

Since `argc` contains the number of arguments passed to the program, it can be used to tell us when the user hasn't passed the correct number of arguments, and we can then inform the user of how to run our program:

~~~
#include <iostream>

int main(int argc, char* argv[])
{
    // Check the number of parameters
    if (argc < 2) {
        // Tell the user how to run the program
        std::cerr << "Usage: " << argv[0] << " NAME" << std::endl;
        /* "Usage messages" are a conventional way of telling the user
         * how to run a program if they enter the command incorrectly.
         */
        return(1);
    }
    // Print the user's name:
    std::cout << argv[0] << "says hello, " << argv[1] << "!" << std::endl;
    return(0);
}
~~~
{: .code}

This program expects at least one argument `NAME`, so we need to check if `argc` is greater than or equal to 2 (remember the name of the program is the first argument).

The output from the program with no arguments passed (assuming the program is called `say_hello`) would be:

~~~
Usage: say_hello NAME
~~~
{: .output}

The output from the program with one argument passed:

~~~
say_hello says hello, Chris!
~~~
{: .output}

## Multiple arguments

Arguments are strings passed to your program to give it information. A program for moving files, for example, may be invoked with two arguments, the source file and the destination: `move /path/to/source /path/to/destination`. 

An example program would be:

~~~
#include <iostream>

int main(int argc, char* argv[])
{
    if (argc < 3) { // We expect 3 arguments: the program name, the source path and the destination path
        std::cerr << "Usage: " << argv[0] << "SOURCE DESTINATION" << std::endl;
        return(1)
    }
    // Implementation of the move function is platform dependent
    // and beyond the scope of this article, so it is left out.
    return(move(argv[1], argv[2]));          
}
~~~
{: .code}

Suppose however, we wanted to allow the use of multiple source paths rather than only one. To do this, we could use a loop and a std::vector
as follows:

~~~
#include <iostream>
#include <string>
#include <vector>

int main(int argc, char* argv[])
{
    if (argc < 3) { // We expect 3 arguments: the program name, the source path and the destination path
        std::cerr << "Usage: " << argv[0] << "SOURCE [SOURCE...] DESTINATION" << std::endl;
        return(1);
    }
    std::vector <std::string> sources;
    std::string destination;
    for (int i = 1; i < argc; ++i) { // Remember argv[0] is the path to the program, we want from argv[1] onwards
        if (i + 1 < argc)
            sources.push_back(argv[i]); // Add all but the last argument to the vector.
        else
            destination = argv[i];
    }
    return(move(sources, destination));
}
~~~
{: .code}

## Numeric arguments

Often the arguments supplied to a program should be treated as numeric rather than strings. Dealing with this situation is simple, it's just
necessary to convert the string parameter to a numeric value before using it. One note of caution however. Do not assume that the arguments
will always be numeric, even if you're expecting them to be.

The following example takes two numbers and adds them together:

~~~
#include <iostream>

using namespace std;

int main(int argc, char* argv[])
{
    if (argc < 3) { // We expect 3 arguments: the program name and two numbers
        cerr << "Usage: " << argv[0] << "NUMBER NUMBER" << endl;
        return(1);
    }
    
    // Note that atoi() will try to convert anything to an integer! If the conversion
    // fails then it will return 0, which may not be expected.
    int first = atoi(argv[1]);
    int second = atoi(argv[2]);
    
    cout << "The result is: " << first + second << endl;
    return(0);
}
~~~
{: .code}

Similar functions are available for converting to long integer (`atol`) and double (`atof`) types. 

## Options

Arguments may also be used to pass options to the program. An option usually starts with a single hyphen (-) for a "short option" or a double 
hyphen (--) for a "long option". Continuing the example of the move program, the program could use a `-d` or `--destination` option to tell it 
which path is the source and which is the destination. For example:

~~~
move -d /path/to/destination /path/to/source
move --destination /path/to/destination /path/to/source
~~~
{: .bash}

By convention, an option always applies to the argument directly to the right of it (for options that take arguments).

Let's extend the previous example to use the destination option:

~~~
#include <iostream>
#include <string>
#include <vector>

int main(int argc, char* argv[])
{
    if (argc < 3) {
        std::cerr << "Usage: " << argv[0] << "-d|--destination DESTINATION SOURCE" << std::endl;
        return 1;
    }
    std::vector <std::string> sources;
    std::string destination;
    for (int i = 1; i < argc; ++i) {
        std::string arg = argv[i];
        if (arg == "-d" || arg == "--destination") {
            if (i + 1 < argc) { // Make sure we aren't at the end of argv!
                destination = argv[i++]; // Increment 'i' so we don't get the argument as the next argv[i].
            } else { // Uh-oh, there was no argument to the destination option.
                std::cerr << "--destination option requires one argument." << std::endl;
                return(1);
            }  
        } else {
            sources.push_back(argv[i]);
        }
    }
    return(move(sources, destination));
}
~~~
{: .code}

Now the parameters can be in any order as long as the destination path is immediately to the right of "--destination".

## Advanced argument processing

These methods of finding command-line arguments are tedious and result in a lot of code. They also don't handle all the standard was of processing
command line arguments. Fortunately there are library functions that can be used to deal with arguments in a standard way, called `getopt` and 
`getopt_long`. These are beyond the scope of this tutorial, but an example is included below.

~~~
#include <getopt.h>
#include <iostream>

int num = -1;
bool is_beep = false;
float sigma = 2.034;
std::string write_file = "default_file.txt";

void usage()
{
    std::cout <<
            "--num <n>:           Set number of program\n"
            "--beep:              Beep the user\n"
            "--sigma <val>:       Set sigma of program\n"
            "--writeFile <fname>: File to write to\n"
            "--help:              Show help\n";
    exit(1);
}

int main(int argc, char **argv)
{
    const char* const short_opts = "n:bs:w:h";
    const option long_opts[] = {
            {"num", 1, nullptr, 'n'},
            {"beep", 0, nullptr, 'b'},
            {"sigma", 1, nullptr, 's'},
            {"writeFile", 1, nullptr, 'w'},
            {"help", 0, nullptr, 'h'},
            {nullptr, 0, nullptr, 0}
    };

    while (true) {
        const auto opt = getopt_long(argc, argv, short_opts, long_opts, nullptr);

        if (-1 == opt)
            break;

        switch (opt) {
        case 'n':
            num = std::stoi(optarg);
            std::cout << "Num set to: " << num << std::endl;
            break;

        case 'b':
            is_beep = true;
            std::cout << "Beep is set to true\n";
            break;

        case 's':
            sigma = std::stof(optarg);
            std::cout << "Sigma set to: " << sigma << std::endl;
            break;

        case 'w':
            write_file = std::string(optarg);
            std::cout << "Write file set to: " << write_file << std::endl;
            break;

        case 'h': // -h or --help
        case '?': // Unrecognized option
        default:
            usage();
            break;
        }
    }
    
    return(0);
}
~~~
{: .code}

Refer to the manual pages for details of how to use these functions.