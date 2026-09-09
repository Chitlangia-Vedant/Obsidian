# Search

We can imagine that we have an essential problem of wanting to know, “Is the number 50 inside an array?” A computer must look at each locker to be able to see if the number 50 is inside. We call this process of finding such a number, character, string, or other item _searching_.
## [Linear Search](https://cs50.harvard.edu/x/2024/notes/3/#linear-search)

**It sequentially checks each element of the list until a match is found or the whole list has been searched.**

A computer scientist could make pseudocode as follows:

```
For i from 0 to n-1
	If 50 is behind doors[i]
		Return true
Return false
```

Notice that the above is still not code, but it is a pretty close approximation of what the final code might look like.

## [Binary Search](https://cs50.harvard.edu/x/2024/notes/3/#binary-search)

- Binary search_ is another _search algorithm_ that could be employed in our task of finding the 50.
- Assuming that the values within the lockers have been arranged from smallest to largest, the pseudocode for binary search would appear as follows:
```
If no doors left
    Return false
If 50 is behind doors[middle]
    Return true
Else if 50 < doors[middle]
    Search doors[0] through doors[middle - 1]
Else if 50 > doors[middle]
    Search doors[middle + 1] through doors[n - 1]
```
# [Running Time](https://cs50.harvard.edu/x/2024/notes/3/#running-time)

_Running time_ involves an analysis using _big O_ notation. Take a look at the following graph:

![chart with: "size of problem" as x-axis; "time to solve" as y-axis; red, steep straight line from origin to top of graph close to yellow, less steep straight line from origin to top of graph both labeled "O(n)"; green, curved line that gets less and less steep from origin to right of graph labeled "O(log n)](https://cs50.harvard.edu/x/2024/notes/3/cs50Week3Slide042.png "big o graphed")

- Rather than being ultra-specific about the mathematical efficiency of an algorithm, computer scientists discuss efficiency in terms of _the order of_ various running times.

- It’s the shape of the curve that shows the efficiency of an algorithm. Some common running times we may see are:

	- <math xmlns="http://www.w3.org/1998/Math/MathML">
	  <mi>O</mi>
	  <mo stretchy="false">(</mo>
	  <msup>
	    <mi>n</mi>
	    <mn>2</mn>
	  </msup>
	  <mo stretchy="false">)</mo>
	</math>
	- <math xmlns="http://www.w3.org/1998/Math/MathML">
	  <mi>O</mi>
	  <mo stretchy="false">(</mo>
	  <mi>n</mi>
	  <mi>log</mi>
	  <mo data-mjx-texclass="NONE">&#x2061;</mo>
	  <mi>n</mi>
	  <mo stretchy="false">)</mo>
	</math>
	- <math xmlns="http://www.w3.org/1998/Math/MathML">
	  <mi>O</mi>
	  <mo stretchy="false">(</mo>
	  <mi>n</mi>
	  <mo stretchy="false">)</mo>
	</math>
	- <math xmlns="http://www.w3.org/1998/Math/MathML">
	  <mi>O</mi>
	  <mo stretchy="false">(</mo>
	  <mi>log</mi>
	  <mo data-mjx-texclass="NONE">&#x2061;</mo>
	  <mi>n</mi>
	  <mo stretchy="false">)</mo>
	</math>
	- <math xmlns="http://www.w3.org/1998/Math/MathML">
	  <mi>O</mi>
	  <mo stretchy="false">(</mo>
	  <mn>1</mn>
	  <mo stretchy="false">)</mo>
	</math>

- Linear search was of order 𝑂(𝑛) because it could take _n_ steps in the worst case to run.
- Binary search was of order 𝑂(log⁡𝑛) because it would take fewer and fewer steps to run even in the worst case.
- Programmers are interested in both the worst case, or _upper bound_, and the best case, or _lower bound_.
- The Ω symbol is used to denote the best case of an algorithm, such as Ω(log⁡𝑛).
- The Θ symbol is used to denote where the upper bound and lower bound are the same, where the best case and the worst case running times are the same.
# [Sorting](https://cs50.harvard.edu/x/2024/notes/3/#sorting)

- _Sorting_ is the act of taking an unsorted list of values and transforming this list into a sorted one.
- When a list is sorted, searching that list is far less taxing on the computer.

## Selection sort

- The algorithm for selection sort in pseudocode is:
    
    ```
    For i from 0 to n–1
        Find smallest number between numbers[i] and numbers[n-1]
        Swap smallest number with numbers[i]
    ```
    
- Summarizing those steps, the first time iterating through the list took `n - 1` steps. The second time, it took `n - 2` steps. Carrying this logic forward, the steps required could be represented as follows:
    
    ```
    (n - 1) + (n - 2) + (n - 3) + ... + 1
    ```
    
- This could be simplified to n(n-1)/2 or, more simply, 𝑂(𝑛2).

# Bubble sort

- _Bubble sort_ is another sorting algorithm that works by repeatedly swapping elements to “bubble” larger elements to the end.
- The pseudocode for bubble sort is:
    
    ```
    Repeat n-1 times
        For i from 0 to n–2
            If numbers[i] and numbers[i+1] out of order
                Swap them
        If no swaps
            Quit
    ```

Analyzing selection sort, we made only seven comparisons. Representing this mathematically, where _n_ represents the number of cases, it could be said that selection sort can be analyzed as:

```
  (n - 1) + (n - 2) + (n - 3) + ... + 1
```

or, more simply $𝑛^2/2−𝑛/2$.

- Considering that mathematical analysis, $n^2$ is really the most influential factor in determining the efficiency of this algorithm. 
	- Therefore, selection sort is considered to be of the order of 𝑂($n^2$) in the worst case where all values are unsorted. Even when all values are sorted, it will take the same number of steps. Therefore, the best case can be noted as Ω($n^2$). Since both the upper bound and lower bound cases are the same, the efficiency of this algorithm as a whole can be regarded as Θ($n^2$).
	- Analyzing bubble sort, the worst case is 𝑂($n^2$). The best case is Ω(𝑛).

## [Merge Sort](https://cs50.harvard.edu/x/2024/notes/3/#merge-sort)

The pseudocode for merge sort is quite short:

```
If only one number
    Quit
Else
    Sort left half of number
    Sort right half of number
    Merge sorted halves
```

Merge sort is a very efficient sort algorithm with a worst case of 𝑂(𝑛log⁡𝑛). The best case is still Ω(𝑛log⁡𝑛) because the algorithm still must visit each place in the list. Therefore, merge sort is also Θ(𝑛log⁡𝑛) since the best case and worst case are the same. 