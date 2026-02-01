# Time Complexity

In real life, we want to build efficient algorithms which perform as fast as they can. Time complexity describes how the running time of an algorithm grows as the input grows. We do that by measuring how many operations the algorithm performs relative to the size of the input (n). We want as little operations performed as possible.

Time complexity helps us compare algorithms, predict how performance scales with large inputs and choose efficient solutions.

For example, let's say an algorithm is $O(n^2)$. This means that for large inputs, if the input size doubles, the speed slows down by roughly 4 times. Compare this to an $O(n)$ algorithm, for which the speed only slows down by roughly $2$ times.

Time complexity is denoted using the big O notation.

The formal definition of big O is: For functions $f$ and $g$, we say that $f \in O(g)$ if there exists $N, c > 0$ such that for all $n > N$, $$f(n) <= c*g(n)$$
The intuition is that if $f$ can be bounded above by a constant multiple of $g$ then $f$ grows slower or equal to $g$.

For algorithms in code, we consider the common operations to have constant cost $O(1)$: arithmetic, variable assignment and mutation, comparison, etc. The time complexity for more complex operations from data structures then depends on their implementation.

If we are doing an operation over the entire input, then it will be $O(n)$(for example, loops). Also, for big O, we usually consider the worst-case scenario.

Here is an example tracing through code to show how to reason about time complexity:

```Python
def example(arr):
	n = len(arr)
	count = 0
	
	for i in range(n):
		if arr[i] % 2 == 0:
			for j in range(i):
				count += 1
	
	return count
```

Since we have a double nested loop over the array of size n, the loop will be $O(n^2)$. All the other operations (conditional checking for array element being even, and incrementing count) take $O(1)$, so the overall time complexity is $O(n^2)$.

It is important to not assume that function calls are $O(1)$. For example, when we call .sort() in Python, it is actually $O(n logn)$. 
