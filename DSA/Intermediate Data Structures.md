# Intermediate Data Structures

**Stack**

A stack is a linear data structure that follows LIFO: last in, first out. It is usually used for undoing things or recursion. It is build in a way that these operations are fast.

- Add an element to the top (push): O(1)
- Remove the top element (pop): O(1)
- Look at the top element (peek): O(1)
- Check if empty: O(1)

In practice, in Python, a stack is just implemented using a normal list. For example:

```Python
stack = []

# Push
stack.append(10)

# Pop
top = stack.pop()

# Peek
top = stack[-1]

# Is empty
is_empty = len(stack) == 0
```

**Queue**

A queue is a linear data structure that follows FIFO: first in, first out. It is usually used for scheduling. It is built in a way that these operations are fast:

- Add element to the back (enqueue): O(1)
- Remove element from the front (dequeue): O(1)
- Look at the front element (peek): O(1)
- Check if queue is empty: O(1)

In practice, in Python, we can use a queue using a deque, which will be explained in the next section.

**Deque**

A deque (double-ended queue) is a data structure that combines the advantages of stacks and queues. Adding/removing elements on both ends is efficient:

- Add element to the front: O(1)
- Add element to the back: O(1)
- Remove element from the front: O(1)
- Remove element from the back: O(1)
- Look at front element: O(1)
- Look at back element: O(1)

Here is an example of a deque in use in Python.

```Python
from collections import deque

d = deque()

d.append(10) # add to the back
d.appendleft(5) # add to the front

d.pop() # remove from the back
d.popleft() # remove from the front

front = d[0] # look at front
back = d[-1] # look at back
```

**Heap**

A heap is a data structure which allows efficient access to either the minimum or the maximum element. The top element refers to the min/max. Here are the operations:

- Insert element into heap(push): O(log n)
- Removing the top element(pop): O(log n)
- Viewing the root element (peek): O(1)
- Build heap from list(heapify): O(n)

By default, the heap in python is a minimum heap. To use a maximum heap, just insert the negative of elements. Here is an example of a heap in use in Python:

```Python
import heapq

heap = []

# Insert
heapq.heappush(heap, 1)
heapq.heappush(heap, 3)

# Peek
smallest = heap[0]

# Extract minimum
smallest = heapq.heappop(heap)

# Heapify
arr = [1, 2, 3, 4]
heapq.heapify(arr)
```

**Linked List**

A singly linked list is a linear data structure where each element is a node containing two parts:

- Data: the value of the node
- Next: a pointer to the next node

The head of a linked list is a pointer to the first node in the list.

Here are the common operations:

- Inserting at the beginning: O(1)
- Inserting at the middle/end: O(n)
- Deleting at the beginning: O(1)
- Deleting at the middle/end: O(n)
- Searching: O(n)
- Accessing a specific node: O(n)

The linked list is implemented using a Node class in python. Below is an implementation with the common operations:

```Python
class Node:
	def __init__(self, data):
		self.data = data
		self.next = None

class LinkedList:
	def __init__(self):
		self.head = none
	
	def insert_at_beginning(self, data):
		new_node = Node(data)
		new_node.next = self.head # Point new node to current head
		self.head = new_node # Update head to new node
	
	def insert_at_position(self, data, position):
		new_node = Node(data)
		if position == 0:
			self.insert_at_beginning(data)
			return
		
		current = self.head
		count = 0
		while current and count < position - 1:
			current = current.next
			count += 1
		
		new_node.next = current.next
		current.next = new_node
	
	def delete_at_beginning(self):
		if not self.head:
			print("List is empty.")
			return
			
		self.head = self.head.next
	
	def delete_at_position(self, position):
        if not self.head:
            print("List is empty.")
            return
            
        if position == 0:
            self.delete_at_beginning()
            return
            
        current = self.head
        count = 0
        while current and count < position - 1:
            current = current.next
            count += 1
            
        current.next = current.next.next
    
    # Search for a value in the linked list
    def search(self, value):
        current = self.head  # Start from the head
        while current:
            if current.data == value:  # If the value matches the current node's data
                return True  # Found the value, return True
            current = current.next  # Move to the next node
        return False  # If we reach the end of the list and didn't find the value, return False
        
    # Access a node at a specific position (index)
    def get_node_at_position(self, position):
        current = self.head  # Start from the head
        index = 0  # Initialize index to 0

        # Traverse the list until we reach the desired position
        while current:
            if index == position:
                return current.data  # Return the data of the node at the given position
            current = current.next  # Move to the next node
            index += 1  # Increment the index
        
        return "Position out of range"  # Return a message if the position is invalid

```
