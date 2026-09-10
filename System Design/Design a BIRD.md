Build a S/W system where you can store information about birds.

Information
-  Properties
-  Behavior
# Version 0 (Brute Force)

```mermaid
classDiagram 
class Bird{
            -name
            -type
            -noOfWings
            -weight
	        -color
            -fly()
            -makeSound()
            -dance()
            -eat()
        }

```

```python
makeSound(){
	if type==crow
		cout<<"kaw kaw"
	elif type==sparrow
		cout<<"chaw chaw"
	elif type==pigeon
		cout<<"yaw yaw"
}
```

# Version 1

Let the `Bird` class be only responsible for generic details and not specific

```mermaid
classDiagram 
class Bird{
			<<abstract>>
            -fly()*
        }
class Crow{
            -fly()
        }
class Sparrow{
            -fly()
        }
class Pigeon{
            -fly()
        }
Bird <|-- Crow
Bird <|-- Sparrow
Bird <|-- Pigeon
```

- To add a new bird, I just have to create a new bird class
- Bird class have less reason to change
- Bird is abstract class

## Requirement: Birds which can't fly (Penguin)

### Solution 1 (Wrong)

```mermaid
classDiagram 
class Bird{
			<<abstract>>
            -fly()*
        }
class Penguin{
            -fly()
        }
Bird <|-- Penguin
```
Ways to implement this
1. Empty method
2. print "Penguin can't fly."
3. throw exception

### Solution 2 (Ideal): Version 1.5

If an entity doesn't support a behavior, it should not have a method to do the behavior.

```mermaid
classDiagram 
class Bird{
			<<abstract>>
        }
class Flying{
			<<abstract>>
            -fly()*
        }
class NonFlying{
			<<abstract>>
        }
class Crow{
            -fly()
        }
Bird <|-- Flying
Bird <|-- NonFlying
Flying <|-- Crow 
NonFlying <|-- Penguin
```
## Requirement: Bird which can't fly+dance

```mermaid
classDiagram 
class Flying_Dance{
			<<abstract>>
            -fly()*
            -dance()*
        }
class Flying_NonDance{
			<<abstract>>
            -fly()*
        }
class NonFlying_Dance{
			<<abstract>>
            -dance()*
        }
class NonFlying_NonDance{
			<<abstract>>
        }
Bird <|-- Flying_Dance
Bird <|-- NonFlying_Dance
Bird <|-- Flying_NonDance 
Bird <|-- NonFlying_NonDance 
```
### Problems
- Class Explosion (Too many class)
- How to get list of all the flying bird?
# Version 2

## Problem

Some bird demonstrate a behavior while other birds don't demonstrate that behavior 
1. Only the birds having that behavior should have that method
2. Should be able to create a list of bird that have a particular behavior

```mermaid
classDiagram

class Bird{
    <<abstract>>
    -eat()
    -makeSound()*
}

class Flying{
    <<interface>>
    -fly()
}

class Dance{
    <<interface>>
    -dance()
}

class Crow{
    
    +makeSound()
    +fly()
}

class Sparrow{
    
    +makeSound()
    +fly()
    +dance()
}

class Kiwi{
    
    +makeSound()
    +dance()
}

class Penguin{
    
    +makeSound()
}

Bird <|-- Crow
Bird <|-- Sparrow
Bird <|-- Kiwi
Bird <|-- Penguin

Flying <|-- Crow
Flying <|-- Sparrow

Dance <|-- Sparrow
Dance <|-- Kiwi
```

```python
class Sparrow extends Bird implement Flying,Dance{
	makeSound(){
		......
	}
	fly(){
		......
	}
	dance(){
		......
	}
}
```

## Make birds fly

### Version 1
```python
List<Bird> birds;
for(Bird b:birds){
	try{
		b.fly(); #Runtime exception if b is Penguin
	}catch(){
	}
}
```

**Violates [[01. SOLID Design Principle#Liskov's Substitution Principle|LSP]]**
### Version 2
```python
List<Flying> birds;
for(Flying b:birds){
	b.fly();
}
```

