# [Sexy primes](https://www.codechef.com/problems/EXCG1806)

**Description**
Sexy prime is a prime number n such that n+6 is also prime.

**Input:** A single integer $n$
**Output:** The number of sexy prime factors of the given number $n$.
**Constraints:** $1\leq n \leq 100$

---

**More Info**

Time limit1 secs
Memory limit1.5 GB
Source Limit50000 Bytes

# Understanding:

```
N = 5: PRIME
N+6 = 11: PRIME

Hence, 5 is a Sexy Prime
```

```
N = 3: PRIME
N+6 = 9: NOT PRIME

Hence, 3 is NOT a Sexy Prime
```

```
N = 7: PRIME
N+6 = 13: YES

Hence, 7 is a Sexy Prime
```


5-11, 7-13, 11-17, 13-19, 17-23, 23-29 etc

Sexy primes: 5, 7, 11, 13, 17, 23 etc

# Q-1: Count the Number of "Sexy Prime Numbers" less than or equal to N

IP: 10
OP: 2 (5,7)

IP: 20
OP: 5 (5,7,11,13,17)

```cpp
int countOfSexyPrimes(int N)
{

}
```

## CODE:

```cpp
public class Main {
    
    static int numberofSexyPrimes(int N)
    {
    boolean[] prime = new boolean[N+7];

	for (int i=0; i<=N+6; i++)
		prime[i] = true;

	for (int p=2; p*p <=N+7; p++) // Euclid Theorem
		// p = 2, p = 3
	{
		if (prime[p] == true)
		{
			for (int i=p*p; i<=N+6; i+=p)
				// p = 2, i=4; i<=10; i+=2 : i = 4, i = 6, i = 8, i = 10
				// p = 3, i = 9; i<=10; i+=3: i = 9
				prime[i] = false;
		}
	}
        
    int numberofSexyPrimes = 0;
	

	// Printing Prime Numbers from [2,N] - []: Both Inclusive
	// N+1 because of keeping index range, checking for N also
	for (int i =2; i<=N; i++) // strictly less than N
		if (prime[i] && prime[i+6])
			++numberofSexyPrimes;
        
        return numberofSexyPrimes;
    }
    
    public static void main(String[] args) 
    {
        System.out.println(numberofSexyPrimes(10)); // 2
        System.out.println(numberofSexyPrimes(20)); // 5
    }
}
```

```Output
2
5
```


# Q-2: Count the Number of Sexy Prime Factors of N 

IP: 10
OP: 1 (5) 

IP: 35
OP: 2 (5,7)

```
int countOfSexyPrimeFactors(int N)
{
	
}
```

## CODE:

```cpp
public class Main {
    
    static int numberofSexyPrimeFactors(int N)
    {
    boolean[] prime = new boolean[N+7];

	for (int i=0; i<=N+6; i++)
		prime[i] = true;

	for (int p=2; p*p <=N+7; p++) // Euclid Theorem
		// p = 2, p = 3
	{
		if (prime[p] == true)
		{
			for (int i=p*p; i<=N+6; i+=p)
				// p = 2, i=4; i<=10; i+=2 : i = 4, i = 6, i = 8, i = 10
				// p = 3, i = 9; i<=10; i+=3: i = 9
				prime[i] = false;
		}
	}
        
    int numberofSexyPrimeFactors = 0;
	

	// Printing Prime Numbers from [2,N] - []: Both Inclusive
	// N+1 because of keeping index range, checking for N also
	for (int i =2; i<=N; i++) // strictly less than N
		if (prime[i] && prime[i+6] && N%i==0)
			++numberofSexyPrimeFactors;
        
        return numberofSexyPrimeFactors;
    }
    
    public static void main(String[] args) 
    {
        System.out.println(numberofSexyPrimeFactors(35)); // 2 (5,7)
    }
}
```

```Output
2
```