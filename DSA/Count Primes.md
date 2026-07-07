# [204. Count Primes](https://leetcode.com/problems/count-primes/)

Given an integer `n`, return _the number of prime numbers that are strictly less than_ `n`.

**Example 1:**

**Input:** n = 10
**Output:** 4
**Explanation:** There are 4 prime numbers less than 10, they are 2, 3, 5, 7.

**Example 2:**

**Input:** n = 0
**Output:** 0

**Example 3:**

**Input:** n = 1
**Output:** 0

**Constraints:**

- `0 <= n <= 5 * 106`

```cpp
class Solution {
public:
    int countPrimes(int n) {
        
    }
};
```
# Primality Test
**To Check if a Number N is PRIME or Not?**
Prime: Only 2 factors: 1 and N
# (1) Check for All Numbers from 1 to N - Count the Number of Factors

Prime: Only 2 Factors: 1 and N

Run a Loop from 1 to N:
- if count == 2, PRIME
- else: NOT PRIME
## CODE:

```cpp
public class Main 
{
    static boolean isPrime(int N)
    {
        int cnt = 0;
        for (int i=1; i<=N; i++)
        {
            if (N % i == 0)
                ++cnt;
        }
        
        return (cnt == 2);
    }
    
    public static void main(String[] args) 
    {
        System.out.println(isPrime(5));   
        System.out.println(isPrime(10));   
        System.out.println(isPrime(31));   
        System.out.println(isPrime(1));   
    }
}
```

```Output
true
false
true
false
```

TC: O(N)
SC: O(1)
# (2) Check for All Numbers from 2 to N-1:

1,2,3,4.........N-1,N: 2 Factors: 1 and N

2,3,4..........N-1: 0 Factors: Prime

Run a Loop from 2 to N-1:
- If Any Factor, NOT PRIME
- else: Prime
## CODE:

```cpp
public class Main 
{
    static boolean isPrime(int N)
    {
        for (int i=2; i<N; i++)
        {
            if (N % i == 0)
                return false;
        }
        
        return true;
    }
    
    public static void main(String[] args) 
    {
        System.out.println(isPrime(5));   
        System.out.println(isPrime(10));   
        System.out.println(isPrime(31));   
        System.out.println(isPrime(2));   
    }
}
```

```Output
true
false
true
true
```

TC: O(N-2)
SC: O(1)
# (3) Check for All Numbers from 2 to N/2

1.......N
2.......N-1

2 to N-1: Factor
2 to N/2: Factor

## Intuition:

If N is NOT a PRIME,
Its Factor MUST lie between 2 to N/2

N/2 is the BIGGEST Factor of any Number which is NOT Prime
in Range (2....N-1)

Hence, from 2 to N/2: At least 1 Factor MUST Exist

N = 50
Range: 2 to N/2
Factors: 2,5,10,25

N = 10
Range: 2 to 5
Factors: 2,5

i = Loop from 2 to N/2
Any Factor: Not Prime, else Prime

## CODE:

```
public class Main 
{
    static boolean isPrime(int N)
    {
        for (int i=2; i<=N/2; i++)
        {
            if (N % i == 0)
                return false;
        }
        
        return true;
    }
    
    public static void main(String[] args) 
    {
        System.out.println(isPrime(5));   
        System.out.println(isPrime(10));   
        System.out.println(isPrime(31));   
        System.out.println(isPrime(2));   
    }
}
```

```Output
true
false
true
true
```

TC: O(N/2)
SC: O(1)
# (4) Check for All Numbers from 2 to sqrt(N)

## Euclid Theorem:
- Any Number if it does not have any factor till sqrt(N)
- It wont have any factor after

N = 10
sqrt(10) = 3.1868

2...3: 2 is factor of 10 and 2 < sqrt(10)
- Not Prime

N = 50
sqrt(50) = 7.182

2...7: 2 and 5 are factors of 50 and 2,5 < sqrt(50)
- Not Prime

N = 97
sqrt(97) = 9.81234

2...9: No factor of 97
- Prime

## CODE:

```
public class Main 
{
    static boolean isPrime(int N)
    {
        // i <= sqrt(N) OR i*i<=N
        for (int i=2; i*i<=N; i++)
        {
            if (N % i == 0)
                return false;
        }
        
        return true;
    }
    
    public static void main(String[] args) 
    {
        System.out.println(isPrime(5));   
        System.out.println(isPrime(10));   
        System.out.println(isPrime(31));   
        System.out.println(isPrime(2));   
    }
}
```

```Output
true
false
true
true
```

TC: O(sqrt(N))
SC: O(1)

if (i%2!=0 && i%3!=0)
Certain Range

if (i%2!=0 && i%3!=0 && i%5!=0)
Higher Range

if (i%2!=0 && i%3!=0 && i%5!=0 && i%7!=0)
More High Range
# (5) Seive of Eratosthenes

Eratosthenes: Mathematician
Use Case: Find Prime Numbers between Range of Values: [L,R]
- WAP to Print Prime Numbers till N (2 to N)

N = 5
OP: [2 3 5]

N = 11
OP: [2 3 5 7 11]

Approach:
(1) Use Euclid Theorem for finding prime in Optimised Time (sqrt(N))
(2) If a Number is Prime, Its Multiple cannot Be Prime
Multiple of Prime ------> Marked them as false

## CODE:

```cpp
void seiveOfEratosthenes(int N)
{
	bool prime[N+1];
	int count=0;
	for (int i=0; i<=N; i++)
		prime[i] = true;
	for (int p=2; p*p<=N; p++) // Euclid Theorem
	{
		// p = 2, p = 3
		if (prime[p]) // Marked all values as true initially
		{
			for (int i=p*p; i<=N; i+=p) 
				prime[i] = false;
			// p = 2, i=4, i<=10, i+=2: i = 4, 6, 8, 10
			// p = 3, i=9, i<=10, i+=3: i = 9
		}
	}

	// Printing Prime Numbers from [2,N]: Both Inclusive
	// N+1: Because of keeping Index Range, check for N as well
	for (int i=2; i<=N; i++)
		if (prime[i] == true)
			System.out.print(i + " ");
}
```

```
N = 10
OP: [2 3 5 7]
```
## DRY RUN:

```
for (int p=2; p*p<=10; p++)
```

2: TRUE
3: TRUE
4: FALSE
5: TRUE
6: FALSE
7: TRUE
8: FALSE
9: FALSE
10: FALSE

```
OP: [2 3 5 7] - Expected OP
```