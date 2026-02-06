# C Basics

This note will just cover the basic syntax of C. Overall it is pretty similar to Javascript, with the change that types have to be specified everywhere.

Every program should look like this:

```C
#include <stdio.h>

int main() {
	return 0;
}
```

The main function is the one which will be run. In C, there are many data types, such as void, int, float, double, char, bool, etc.

Variables must be declared with a type:

```C
int x; // declaration
x = 5; // initialization

int y = 10; // both steps at once
```

For input and output, we have two functions, printf and scanf.

```C
printf("Value: %d", x);
scanf("%d", &x);
```

Note that for scanf, we should pass the address of x so that the function can mutate the value of x. Note here the use of %d, which specifies the format of the substitute.

%d: int, %f: float, %c: char

For operators, it's all the same as in Javascript.

- Arithmetic: + - * / % (one note about /, it does floor when we divide two ints)
- Relational: == != > < >= <=
- Logical && || !
- Assignment = += -= *= /=

In C, we can do control statements.

```C
if (x > 0) {
	printf("Positive");
} else {
	printf("Negative");
}
```

We can also do loops:

```C
// For loop
for (int i = 0; i < 5; i++) {
    printf("%d ", i);
}

// While loop
while (x > 0) {
	dostuff();
	x--;
}

// Do-while loop
do {
	dostuff();
} while (x > 0)
```

The difference between while and do-while loops is that the do-while loop will execute the body once, before checking.

To define functions, we have to put types to parameters as well as return values. The first type is the return value.

```C
int add (int a, int b) {
	return a + b;
}
```

One of the ways to store data in C is to use structs. One can think of it as a container with many fields. We can define a struct as such:

```C
struct Student {
	int id;
	int grade;
	bool isOver18;
}
```

Then, when we want to initialize a struct, we can just pass in the fields. Note that to mutate structs, we have to it field by field, or define a new one. We cannot use {} for assignment.

```C
struct Student student1 = {1, 100, true};

// To access struct fields use .
int id1 = student1.id;
int grade1 = student1.grade;

// To mutate the struct
student1.grade = 90; // Fine

struct Student student1new = {1, 90, true};
student1 = student1new; // Fine

student1 = {1, 90, true}; // Bad

