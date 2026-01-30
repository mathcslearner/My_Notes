# Basic Data Structures

**List**

In Python, a list is simply initialized using \[]. For example, \[1, 2, 3, 4] would be the syntax for a list. Here are the common operations for lists along with their time efficiency:

- Accessing/modifying element: O(1)
- Length of the list: O(1)
- Searching: O(n)
- Counting number of occurrences of element: O(n)
- Adding an element at the end: O(1)
- Adding an element in the middle: O(n)
- Removing the last element: O(1)
- Removing an element in the middle: O(n)
- Slicing: O(n)
- Concatenation: O(n)

One thing to keep in mind about lists in Python is that they are mutable. If you pass a list to a function and the function mutates it, the original list will be changed. If we don't want to mutate it, we have to create a copy first.

Here is a code snippet illustrating how to use the above operations in Python:

```Python
numbers = [1, 2, 3, 4, 5]

# Accessing/modifying
second = numbers[1]
numbers[1] = 4
numbers[1] = 2

# Length
length = len(numbers)

# Searching
is_five_in = (5 in numbers)

# Counting
numbers.count(4) # returns 1

# Adding to the end
numbers.append(6)

# Adding in the middle
numbers.insert(1, 6) # Mutates to [1, 6, 2, 3, 4, 5]

# Removing at the end
numbers.pop() # Returns the last element

# Removing in the middle
numbers.pop(1) # Removes element at index 1
numbers.remove(3) #Removes the first occurence of the element 3

# Slicing (left endpoint included, right endpoint not included)
numbers[1:4] # Returns [2, 3, 4]

# Concatenation
numbers_2 = [6, 7, 8, 9, 10]
numbers_1 += numbers_2 # Returns [1 to 10]
```

**Set**

In Python, a set is initialized using set() or {}. For example, {1, 2, 3} is a set. Here are the common operations:

- Adding an element: O(1)
- Removing an element: O(1)
- Searching for an element: O(1)
- Creating a set from a list: O(n)
- Length of a set: O(n)

Here is a code snippet showing how to use the various set operations in Python:

```Python
numbers = set()

# Adding elements
numbers.add(1)
numbers.add(2)
numbers.add(3)

print(numbers) # {1, 2, 3}

# Converting from list to set
numbers_2 = set([1, 2, 3])
print(numbers_2) # {1, 2, 3}

# Searching for an element
print(3 in numbers) # True
print(4 in numbers) # False

# Removing an element
numbers.remove(2)
len(numbers) # 2
```
