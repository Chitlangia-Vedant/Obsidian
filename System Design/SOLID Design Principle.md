Design Principle: A set of guidelines that helps a SWE design better S/W system

S -> Single Responsibility Principle
O ->  Open-Close Principle
L -> Lisleov's Substitution Principle
I -> Interface Segregation Principle
D -> Dependency Inversion Principle

A S/W system is considered better based on the following factors:
- Maintainability
- Extensibility
- Scalability
- Modularity
- Reusability

# Single Responsibility Principle

Every code unit (Class/Method/Package) should exactly have one responsibility

There should be exactly one reason to change the code of that unit

## How to identify the violation of SRP

1. Method with multiple `if else`
	-  Not always true: If the `if else` is part of business logic, then its okay
	- Problem due to too many `if else if`
		1. Understandability
		2. Difficult to test
		3. Difficult for developers to working in parallel
		4. Code duplication
		5. Less code reuse
		6. Violates SRP
2. Monster Method
	- Method that does more than its name suggest
3. Common/Utilities
	- It end up becoming a garbage place for all the code that an engineer doesn't know where to put

# Open-Closed Principle

Code base should be open for extension but closed for modification

Code base should be extensible 
	Easy to add new feature but adding new feature shouldn't require changing written code 

Rather than modifying already written code unit add more code unit

Adding new feature in a code base should require very less, if no change, in already written code

# Liskov's Substitution Principle

Object of any child class should be as-is substitutable in the variable of the parent type, without requiring any code change

No special treatment to any child class

# Interface Segregation Principle

Interface should be as light as possible (as less method as possible)

# Dependency Inversion Principle

No 2 concrete classes should directly depend on each other 

If they need to depend they should depend via an interface
