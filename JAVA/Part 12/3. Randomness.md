Java offers ready-made `Random` class for creating random numbers.

```Java
Random random = new Random();
for (int i = 0; i < 10; i++) {
	// Draw and print a random number
	int randomNumber = random.nextInt(10);
	System.out.println(randomNumber);
}

```

It has `nextInt` method, which gets an integer as a parameter. The method returns a random number between `[0, integer]` or _0.....(integer -1)_.

The program output is not always the same. One possible output is the following:
```Output
2 
2 
4 
3 
4 
5 
6 
0 
7 
8
```

A Random object can also be used to create random doubles. These can for example be used for calculating probabilities. Computers often simulate probabilities using doubles between \[0..1].

The `nextDouble` method of the Random class creates random doubles.

The `this.random.nextGaussian()` call is a regular method call. However what is interesting is that this method of the `Random` class returns a normally distributed number (normally distributed numbers can be used to for example model the heights and weights of people — if you are not interested in different kinds of randomness that is OK!).

```java
public int makeAForecast() {
    return (int) (4 * this.random.nextGaussian() - 3);
}
```