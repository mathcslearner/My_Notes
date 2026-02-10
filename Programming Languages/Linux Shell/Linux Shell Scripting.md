# Linux Shell Scripting

One of the most powerful concepts in programming is automation, that is creating scripts which we can execute to get tasks done faster and easier. Let's say we have to do something manually every day. We could write a script to do it for us. This note is about bash scripting.

The first lines should be:

```bash
#!/bin.bash
```

In the shell, we can store information as strings using variables. For example, x=1. Then, to access the variable value, we can do ${x}.

Scripts are only useful if they can take in command line arguments which allows us to change the details of their functionality. To obtain the name of the script, use ${0}. To get access to the arguments, we can use ${1}, ${2}, etc. To get the number of arguments supplied to the script, we can use ${#} and to get all arguments, we can do ${@}. One useful command is shift. Let's say we don't need the first argument anymore, then we can use shift to shift all arguments by one, so $2 becomes $1, and so on.

To read from standard input stream, we can use read input, then we can access the given input using $input. We can also give a command line prompt like read -p "Input word:". 

To do booleans, we can use square brackets, such as \[ 1 -eq 2]. This will either return 0 for success or 1 for failure. There are a bunch of boolean operators such as == and !=, -eq and -ne, -gt, -lt, !, ... We can also combine statements using && and ||, like in C.

To do if statements, here is the general syntax:

```bash
if [condition]; then

elif [condition]; then

else;

fi
```

We can also do loops in bash. The general syntax is

```bash
while [condition]; do

done
```

One thing that's useful about while loops is we can use these to iterate over the command line arguments, especially when the number is not fixed. One way we might do this is as such:

```bash
while [ "$1" != "" ]; do
  echo "Number of arguments is $#"
  echo "Argument 1 is: $1"
  shift
done
```

Whenever we are done with an argument, we can shift it and move to the next one.

In general, when writing bash scripts, one important thing is to do error handling for everything, and not assuming that the user will follow the specifications of the program. This includes checking for the number of arguments, the kind of argument provided, etc.

Finally, to run a script, just do ./filename in the terminal!
