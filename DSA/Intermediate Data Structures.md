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

