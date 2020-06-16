# Time Complexity
Describes the amount of time it takes to run an algorithm. Commonly expressed using asymptotic notation like **big O notation**, *O(n), O(1), O(log n)*, etc., where *n* is the input size.

![Time Complexity](../images/time-complexity.png)

## Asymptotic<sub>[[1](#note1)]</sub> Notation
A way to analyzing how fast a program's runtime grows asymptotically.
- O (Big-O): Worst case
- Ω (Big-Omega): Best case
- Θ (Big-Theta): Worst and best case are the same

### Example: Search Algorithm
- Linear search: O(n), Ω(1)
- Binary search: O(log n), Ω(1). (Search on a sorted array)
> Ex: Looking for **3** in *[1, 2, 3, 4, 5, 6, 7]*  
>
> 1. *3 < 4* → [**1, 2, 3, 4**, 5, 6, 7]  
> 2. *3 > 2* → [1, 2, **3**]  
> 3. *3 = 3* → [**3**]  
>
> Takes log<sub>2</sub> 8 = 3 times to find the target.

### Example: Bubble Sort Algorithm
Sort *[5, 4, 3, 2, 1]* in ascending order.

First round did *n-1* times.

1. [**4, 5**, 3, 2, 1]
2. [4, **3, 5**, 2, 1]
3. [4, 3, **2, 5**, 1]
4. [4, 3, 2, **1, 5**]

Second round did *n-2* times.

1. [**3, 4**, 2, 1, 5]
2. [3, **2, 4**, 1, 5]
3. [3, 2, **1, 4**, 5]

Third round did *n-3* times.

1. [**2, 3**, 1, 4, 5]
2. [2, **1, 3**, 4, 5]

Last round did only once.

1. [**1, 2**, 3, 4, 5]

So it's *(n-1) + ... + 1 = n(n-1) / 2* times.  
Hence the time complexity is O(n(n-1) / 2) = O(n²/2 - n/2) = **O(n²)**.

---

<span id="note1">[1]: 漸進的</span>