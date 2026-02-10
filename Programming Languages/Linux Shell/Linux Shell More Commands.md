# Linux Shell More Commands

Here are some more miscellaneous useful commands in the Linux Shell. They all have different functions but I grouped them together for conciseness.

**Globbing**:

Recall that using the Linux Shell, we can manipulate files. We do so by providing the filename as an argument to whatever command we are running. What if we want to, say, run a command on all files which end in .txt? This is where globbing patterns come in. They allow us to check for patterns within filenames. Here are a few common wildcard symbols:

- ?: matches any one character
- \*: matches zero or more characters
- \[]: one character from this set, for example \[1-9]
- \[!]: one character not in this set, for example \[!1-9]
- {}: indicates or, for example {cc, cpp}

Here's an example of a globbing pattern in use:

```bash
logs/202[0-3]-??-*.log
```

**Pipes**:

Recall that we can redirect the stdout of a program to a file using output redirection. It turns out we can also redirect it to the stdin of another program. This is done using a pipe, connecting the stdout of one to the stdin of the other. This allows us to connect together basic commands to achieve a more complex task.

For example, consider the following 2 tasks considering a file sample.txt:

- get the first 20 lines of the file
- count the number of words in these lines

We could do it in 2 steps like this:

```bash
head -n 20 sample.txt > first20.txt
wc -w < first20.txt
```

The problem is that we are creating a file which we do not need as an intermediate step. We can use piping to directly connect the two:

```bash
head -n 20 sample.txt | wc -w
```

**Embedded commands**:

What if instead of providing the output of a command to the stdin of another program, we wanted to provide it as an argument? We can do so using embedded commands. The syntax is $(command). Here's an example:

```bash
wc $(cat myfile.args) myfile.txt
```

**Regular expressions**:

Regular expressions are used to search the content within text files, matching lines with one or more patterns. The command to use when searching for regular expressions is grep -e regex filename. Here are the basic symbols of regular expressions:

- \[...]: matches one character from this set, for example \[1-9]
- \[^...]: matches one character not from this set, for example \[^1-9]
- \*: 0 or more times
- +: 1 or more times
- ?: 0 or 1 times
- \{n}: exactly n times
- ^: start of string
- $: end of string
- .: any character
- (): used to group patterns together
- |: used to denote or

To match special characters literally, for example we want an asterisk, use an escape backslash: \\*

Here's an example of a regex which matches for a basic email address:

```bash
^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$
```

**File Permissions**

Every file has nine characters that represent file permissions. There are three subsets of users, which are for the owner, the group users, and all other users. There are three kind of permissions, which are read, write, and execute. 

Here's an example to demonstrate the concept: rwx r-x rw-

This means:

- The owner can read, write, execute
- Group users can read or execute but not write
- All the other users can read or white but not execute

To change the owner, use the chown command. To change permissions, use the chmod command. Here's an example of how it works:

```bash
chmod o+r file.txt
```

means the other users gain read permission. For ownership, we can use u, g, o, a for owner, group, other, all respectively. + to add permission, - to revoke permission, = to set exactly.
