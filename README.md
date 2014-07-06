Commandeer
==========

Take command of your command line.

Commandeer easily deals with getting data from the command line to variables.
Period. It does this little thing well and lets *you* deal with the rest.


Usage
-----

**In code**

```nimrod
## myCLApp.nim

import commandeer

commandline:
  argument number, int
  argument squareIt, bool
  option times, int, "times", "t"
  option debug, bool, "debug", "d"
  exitoption "help", "h", "Usage: program [--times=<int>|--debug|--help] <int> <bool>"

echo("Number = ", number)

if squareIt:
  number = number*number
  echo("Squared = ", number)

if times != 0:
  echo("Times ", times, " = ", number*times)

if debug:
  echo("Debugging enabled")

```

**On the command line**

```
$ myCLApp 3 yes --times=5 --debug
Number = 3
Squared = 9
Times 5 = 45
Debugging enabled
$ myCLApp 3 yes --times=5 --debug --help
Usage: program [--times=<int>|--debug|--help] <int> <bool>
```

See the `tests` folder for other examples.

That's all. It's not much and it doesn't pretend to be a magical experience.
Although, it would be much cooler if it was. It should Just Work.


Installation
------------

There are 2 ways to install commandeer:

**babel**

Install [babel](https://github.com/nimrod-code/babel). Then do:

    babel install commandeer

This will install the latest tagged version of commandeer.

**raw**

Copy the commandeer.nim file to your project and import it.

When I go this way for Nimrod libraries, I like to create a `libs/`
folder in my project and put third-party files in it. I then add the
line `path = "libs"` to my `nimrod.cfg` file so that the `libs/`
directory is looked into at compile time.


Documentation
-------------

**commandline**

`commandline` is used to delimit the space where you define the command line
arguments and options you expect. All other commandeer constructs (described below) are placed under it.


**argument `identifier`, `type`**

It declares a variable named `identifier` of type `type` initialized with
the value of the corresponding command line argument converted to type `type`.
Correspondence works as follows: the first occurrence of `argument` corresponds
to the first argument, the second to the second argument and so on.

**arguments `identifier`, `type` `[, atLeast1]`**

It declares a variable named `identifier` of type `seq[type]` initialized with
the value of the sequential command line arguments that can be converted to type `type`.
By default `atLeast1` is `true` which means there must be at least one argument of type
`type` or else an error is thrown. Passing `false` there allows for 0 or more arguments of the
same type to be stored at `identifier`.

*Warning*: `arguments myListOfStrings, string` will eat all arguments on the command line. The same 
applies to other situations where one type is a supertype of another type in terms of conversion e.g., floats eat ints.


**option `identifier`, `type`, `long option`, `short option`**

It declares a variable named `identifier` of type `type` initialized with
the value of the corresponding command line option converted to type `type`
if it is present. Otherwise `identifier` is initialized to its default type value.

The command line option syntax follows Nimrod's one i.e., `--times=3`, `--times:3`, `-t=3`, `-t:3` are all valid.

Syntactic sugar is provided for boolean options such that only the presence of the option is needed to give a true value.


**exitoption `long option`, `short option`, `exit message`**

It declares a long and short option pattern for which the application
will immediately output `exit message` and exit.

This is mostly used for printing the version or the help message.


**Valid types for `type` are:**
- pre-defined integer
- pre-defined floating point
- string
- boolean
- character


Design
------

- Keep as much logic out of the module as possible and into the hands of
  the developer
- No magical variables should be made implicitly available. All created
  variables should be explicitly chosen by the developer.
-  Keep it simple and streamlined. Command line parsers can do a lot for you, but I prefer to
  be in full control.


TODO and Contribution
---------------------

- Better tests!
- Use and see what needs to be added.
