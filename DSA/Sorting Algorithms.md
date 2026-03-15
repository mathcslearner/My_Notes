# Sorting Algorithms

Sorting is one of the quintessential problems in the field of computer science. If we sort data into a specific order, then it becomes easier to work with. For example, we can search in O(log n) instead of O(n) using binary search. Many other algorithms rely on the data we work with being sorted.

We will discuss four different algorithms for now: selection sort, insertion sort, quick sort, and merge sort.

## Selection Sort

The basic idea behind selection sort is that at step k, we find the element which should be in position k (1-indexed) in the array. Assuming that the first k - 1 elements in the array are already correct, to find the kth element it suffices to search in array\[k:] and find the minimum. Then, we swap array\[k] and the minimum element, so that it is at the correct position.

Here's a visualization of how selection sort works:

![[Pasted image 20260315174812.png]]

Here's an implementation in Python:

```Python
def selection_sort(nums):
	n = len(nums)
	
	for i in range(n):
		pos = i
		for j in range(i + 1, n):
			if a[j] < a[pos]:
				pos = j
		
		a[i], a[pos] = a[pos], a[i]
```

The time complexity is O(n^2) worst case and best case.
## Insertion Sort

Insertion sort can be viewed as a conceptual "opposite" to selection sort. In selection sort, we look through the array and select the correct element to keep the first k elements sorted. In insertion sort, we directly take the kth element and insert it at the correct position to keep the first k elements sorted. To insert it at the right position, it suffices to keep moving it forward until it is larger than the element in front of it.

Here's a visualization of how insertion sort works:

![[Pasted image 20260315175528.png]]

Here's an implementation in Python:

```Python
def insertion_sort(nums):
	n = len(nums)
	
	for i in range(n):
		j = i
		while j > 0 and a[j - 1] > a[j]:
			a[j], a[j - 1] = a[j - 1], a[j]
```

The time complexity is O(n^2) in the worst case and O(n) in the best case, which makes it slightly better than selection sort.
## Quick Sort

Quicksort is an example of a sorting algorithm which relies on the "divide and conquer" principle.

First, we select an element of the array, which we will call the pivot. Then, we divide the array into two sub-groups: elements which are less than the pivot, S1, and elements which are greater than the pivot, S2. Then we return the result of appending sort(S1) + pivot + sort(S2).

A simple algorithm would be: select the first element as the pivot, move all elements larger than the pivot to the back, swap the pivot into the correct position, then do the recursive step.

Here's an implementation in Python:

```Python
def quick_sort_range(nums, first, last):
	if last <= first:
		return
	
	pivot = nums[first]
	pos = last
	
	for i in range(last, first, -1):
		if nums[i] > pivot:
			a[pos], a[i] = a[i], a[pos]
			pos -= 1
	
	a[first], a[pos] = a[pos], a[first]
	
	quick_sort_range(nums, first, pos - 1)
	quick_sort_range(nums, pos + 1, last)
	
def quick_sort(nums):
	quick_sort_range(nums, 0, len(nums) - 1)
```

This algorithm is O(nlogn) in the best case and O(n^2) in the worst case. However, people still use quicksort a lot because by using more sophisticated pivot selection methods, we can achieve O(nlogn) average runtime.
## Merge Sort

Merge sort also relies on the "divide and conquer" principle.

Here I will describe a top-down merge sort, which is easier to implement in imperative languages. The basic idea is that you split your array in the middle to obtain a left and a right subarray. We sort the left and right subarray recursively. Then, we will merge them back together, by picking the smallest element at each step until we have all the elements.

Here's an implementation in Python:

```Python
def merge(arr, l, m, r):
	L = arr[l:m+1]
    R = arr[m+1:r+1]

    i = j = 0
    k = l

    while i < len(L) and j < len(R):
        if L[i] <= R[j]:
            arr[k] = L[i]
            i += 1
        else:
            arr[k] = R[j]
            j += 1
        k += 1

    # copy remaining
    arr[k:r+1] = L[i:] + R[j:]

def mergeSort(arr, l, r):
	if l < r:
		m = l + (r - l) // 2
		mergeSort(arr, l, m)
		mergeSort(arr, m + 1, r)
		merge(arr, l, m, r)

def sortArray(nums):
	mergeSort(nums, 0, len(nums) - 1)
	return nums
```

The advantage of merge sort is that it is always O(nlogn) even in the worst case.
