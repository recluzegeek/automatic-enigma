# Order of Growth

BigO Notation lets you compare the number of operations. It tells you how fast the algorithms grows, as the input size grows. BigO establishes the worst-case run time

Recall fibonacci series,

$1, 1, 2, 3, 5, 8, 13, 21, 34,...$

$fib(n) = fib(n-1) + fib(n-2)$

We had two alternative for computing $fib(n)$, let's compute their execution time differences.

```python
# using recursion
def fib1(n):
    if(n <= 1):
        return 1
    return fib1(n-2) + fib1(n-1)

# with for loop
def fib2(n):
    a, b = 1, 1
    for i in range(n):
        a, b = b, a + b
    return a

# compute execution time of program
import time

def compute_time(fn, limit):
    l = []
    for i in range(limit):
        start_time = int(round(time.time() * 1000))

        fn(i) 

        time_taken = int(round(time.time() * 1000))
        difference = time_taken - start_time

        l.append(difference)

    return l

limit = 40
fib1_times = compute_time(fib1, limit)
fib2_times = compute_time(fib2, limit)

print(fib1_times)
print(fib2_times)

# plot the difference
import matplotlib.pyplot as plt

plt.figure()
plt.title("Comparison of two fib. techniques")
plt.xlabel("fib(n)")
plt.ylabel("time (ms)")

plt.plot(fib1_times, label="Fib. using recursion")
plt.plot(fib2_times, label="Fib. using for loop")

plt.legend()
plt.show()
```

and the results are as follows:

![fib_tech_comparisons.png](imgs/fib_tech_comparisons.png)

See the exponential rise, as the $n$ of $fib(n)$ increases, the time cost for using recursive to calculate fib. sum grows exponentially.

## Binary Search

- Works on sorted data, in O(log N) complexity

Here's python implementation:

```python
def binary_search(list, item):
    # low and high keep track of which part of the 
    # list you'll search in
    low = 0
    high = len(list) - 1

    # while you haven't narrowed down to single element
    while(low <= high):
        
        # check the middle element
        mid = (low + high) // 2
        guess = list[mid]

        # found the item
        if guess is item:
            return mid
        
        # the guess was too high
        if guess > item:
            high = mid -1
        # the guess was too low
        else:
            low = mid + 1
    
l = [1, 3, 5, 7, 9]
print(binary_search(l, 7))
print(binary_search(l, 8))
```

## Arrays vs Lists

|           | **Arrays**                                           | **Lists**                                       |
|----------------------|---------------------------------------------------------|---------------------------------------------------------|
| **Reading**          | O(1) (accessing an element by index is constant time) | O(n) (in a singly linked list, you may need to traverse the list) |
| **Insertion**          | O(n) (due to shifting elements when inserting at arbitrary positions) | O(1) (insertion at the head/tail of the list, assuming no traversal is needed) |
| **Deletion**          | O(n) (requires shifting elements after deletion) | O(1) (deleting the node if you have a reference to it, or deleting from the head/tail) |

## Selection sort

For each element in the list:

- **Find the smallest element:** In each iteration, the algorithm searches for the smallest (or largest, depending on sorting order) element in the unsorted part of the list.

- **Place it in the sorted portion:** Once the smallest element is found, it’s moved to the sorted part of the list (by popping from the original list and appending to the new list).

```python
# return the index of the smallest value in list
def find_smallest(arr):
    
    smallest_index = 0
    smallest = arr[0]

    for i in range(1, len(arr)):

        if arr[i] < smallest:
            smallest = arr[i]
            smallest_index = i

    return smallest_index

l = [3, 4, 1, 5, 6, 0]

def selection_sort(arr):
    new_arr = []

    for i in range(len(arr)):
        # find index of the smallest value
        smallest_index = find_smallest(arr)
        # pop the value from the list and
        # store in the new array
        new_arr.append(arr.pop(smallest_index))
    return new_arr

print(selection_sort([5, 3, 6, 2, 10]))
print(selection_sort([10, 9, 8, 7, 6, 5]))
```
