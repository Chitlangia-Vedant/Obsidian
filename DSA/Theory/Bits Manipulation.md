- **AND (`&`)**: Compares each bit and returns 1 if both bits are 1.
- **OR (`|`)**: Compares each bit and returns 1 if at least one bit is 1.
- **XOR (`^`)**: Compares each bit and returns 1 if bits are different.
- **NOT (`~`)**: Inverts all the bits.
- **Left Shift (`<<`)**: Shifts bits to the left, effectively multiplying the number by 2.
- **Right Shift (`>>`)**: Shifts bits to the right, effectively dividing the number by 2.
# AND, OR, NOT Operators
## AND: (&&)

0 AND 0 = 0
0 AND 1 = 0
1 AND 0 = 0
1 AND 1 = 1
## OR: (||)

0 OR 0 = 0
0 OR 1 = 1
1 OR 0 = 1
1 OR 1 = 1
## NOT: (!)

!0 = 1
!1 = 0

# Left Shift and Right Shift Operators (<<, >>)

101010101

Last Digit: LSB (Least Significant Bit)
First Digit: MSB (Most Significant Bit)

Binary -----> Decimal

Right to Left and Multiply with Power of 2:

1*2^0 + 0*2^1 + 1*2^2 + 0*2^3 + .........


Note:

As you travel left, your values are increasing by multiplying with 2
As you travel right, your values are decreasing bu dividing by 2
## Left Shift Operator:
Left Shifts the Binary Values
### Left Shift by 1
int a = 10;
int b = a << 1;

$b = a * 2^1$
### Left Shift by K
int a = 10;
int b = a << 1;

$b = a * 2^K$
### CODE:

```Java
public class Main 
{
    public static void main(String[] args) 
    {
        // Left Shift
        int a = 10;
        System.out.println(a<<1); // a * 2^1 = 20
        System.out.println(a<<2); // a * 2^2 = 40
        System.out.println(a<<3); // a * 2^3 = 80
        System.out.println(a<<4); // a * 2^4 = 160
    }
}
```

```Output
20
40
80
160
```
## Right Shift Operator:
Right Shifts the Binary Values
### Right Shift by 1
int a = 10;
int b = a >> 1;

$b = a / 2^1$
### Left Shift by K
int a = 10;
int b = a >> 1;

b = a / 2^K

### CODE:

```java
public class Main 
{
    public static void main(String[] args) 
    {
        // Right Shift
        int a = 160;
        System.out.println(a>>1); //  a / 2^1 = 80
        System.out.println(a>>2); // a / 2^2 = 40
        System.out.println(a>>3); // a / 2^3 = 20
        System.out.println(a>>4); // a / 2^4 = 10
    }
}
```

```Output
80
40
20
10
```
# XOR Operators
Exclusive OR

0 XOR 0 = 0
0 XOR 1 = 1
1 XOR 0 = 1
1 XOR 1 = 0

Symbol: ^ (XOR)

Extended Version of XOR from Bits to Numbers:

TRICK:
Apply on Numbers ------> Perform Bitwise XOR Operation on Binary Values


## (1) A ^ A = 0

5 ^ 5
= 0101 ^ 0101
= 0000

```Java
public class Main 
{
    public static void main(String[] args) 
    {
        int a = -20;
        System.out.println(a^a); 
    }
}
```

```Output
0
```

## (2) Commutative

A ^ B = B ^ A

A + B = B + A //Commutative
A * B = B * A //Commutative

A - B != B - A // Not Commutative
A / B != B / A // Not Commutative

```Java
public class Main 
{
    public static void main(String[] args) 
    {
        int a = 20;
        int b = 50;
        System.out.println(a^b); 
        System.out.println(b^a); 
    }
}
```

```Output
38
38
```

## (3) A ^ 0 = A

Note:
0 is the identity of XOR

A op B = A
B ----> Identity of op

A + 0 = A
0: Additive Identity

A * 1 = A
1: Multiplicative/Product Identity

```Java
public class Main 
{
    public static void main(String[] args) 
    {
        int a = 65;
        System.out.println(a^0); 
    }
}
```

```Output
65
```

## (4) A^A^A^A^A^A^A^A^A^A^A^A............ 

### Even Times = 0
### Odd Times = A

A^A = 0
A^0 = A

A^A^A = (0)^A = A^0 = A

A^A^A^A = (0)^A^A = A^A = 0

# Examples

## **Example 1: Checking if a number is even or odd**

You can use bit manipulation to check if a number is even or odd by examining its least significant bit (LSB):

```Python
def is_even(num):     return (num & 1) == 0  # Check if LSB is 0 (even)
```

![[Pasted image 20260702185452.png]]

## **Example 2: Counting the number of set bits**

To count the number of 1s (set bits) in the binary representation of a number:

```Python
def count_set_bits(n):     count = 0     while n:        count += n & 1  # Increment count if LSB is 1         n >>= 1         # Right shift to check the next bit     return count
```

![[Pasted image 20260702185445.png]]

You can optimize set bit counting using **Brian Kernighan’s Algorithm**, which runs in `O(k)` time, where `k` is the number of set bits. The idea is to repeatedly turn off the rightmost 1 in the binary representation of a number with `n & (n - 1)` until `n` becomes zero. Each operation removes one set bit, making it more efficient than checking every bit.

```Python
def count_set_bits(n):   count = 0   while n:      n &= (n - 1)  # Turn off the rightmost set bit       count += 1   return count
```