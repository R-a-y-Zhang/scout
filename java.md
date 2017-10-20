# THE QUICK AND DIRTY GUIDE TO JAVA (LIKE REALLY QUICK AND DIRTY)
## Classes and Objects
One of the most important tasks of programming is modeling real-life events or objects. Most of those objects
has its own attributes and actions that they can perform. Therefore, let us consider a Car. A car has the following
attributes: year, make, model, color, and doorCount. It can also perform the following actions: honk, drive, reverse.
Therefore, to model a Car, we have to first make a class, which will be the blueprint of a Car:
```
class Car {
    String year;
    String make, model;
    String color;
    int doorCount;
    public Car(String year, String make, String model, String color) {
        this.year = year;
        this.make = make;
        this.model = model;
        this.doorCount = doorCount;
    }

    public void setYear(String year) {
        this.year = year;
    }

    public void setMake(String make) {
        this.make = make;
    }

    public void setModel(String model) {
        this.model = model;
    }

    public String getYear() { return this.year; }
    public String getMake() { return this.make; }
    public String getModel() { reeturn this.model; }
    public int getDoorCount() { return this.doorCount; }

    private void injectFuel() { /* code */ }
}
```

Lines 1-4 are the attributes of the class, i.e. the properties of the class. Lines 5 to 10 consists of the
constructor of the class, which does what it sounds like it does: constructs the class. Notice that the
constructor does not have a return type: The thing that the constructor returns is an instance of the
class.

The methods are the functions that come after the constructor (the order does not matter, btw). Here, the
methods beginning with _set_ are _setters_ and those with _get_ are _getters_ and they do what you would
assume they do: the set and get attributes.

### Public, Protected, Private
Why do you need getters and setters? One of the philosophies of OOP is to expose only just enough so that
the user can do what needs to be done. For example, you do not need to know how much fuel to inject into
the engine to go a particular speed in order to go forward. You also do not need to know the engine model
in order to drive it.

In our class, the last method, _injectFuel_, demonstrates this principle. It is marked _private_, so it
can only be used inside of the class and no one outside can access it (if someone could control the amount
of fuel injected into your engine your fucked). Thus, attributes and methods labelled _private_ are only
accessible inside the class. Those labeled _public_ can be accessed by anyone. Those labeled _protected_
can only be accessed within the class and within any classes which inherits from the current class. If you
do not specify an access type, it will default to private. The constructor should always be public and
only in very specific circumstances would you make it private.

## Non-static and Static
Oftentimes, you will see a keyword or method that is _static_. This means that the instance of that method
or attribute is shared across all instances. For _static_ methods and attributes, you do not need to have
an instance of the class in order to access it but can just do ```<ClassName>.<static-name>```. Also, all
instance methods can preference both static and non-static methods and attributes, but static methods can
only access static methods and attributes.

## Superclasses, Interfaces, Inheritence, and Polymorphism
This is where this shit gets fun. Something very powerful is the ability for classes to inherit from other classes.
If you have a class A, another class can inherit from B meaning that B will have all the attributes and methods
of A while also having its own. This is meant so that if you have multiple classes that do similar things
with only some differences, this way you have a parent class that holds all the similarities and child classes
which each have their own unique attributes and methods. An example that all birds have wings and beaks and can
also flap their wings and open their beaks, but penguins obviously do not have a fly method and pereguine falcons
have both a fly and divebomb method. Therefore, you can have a class called Bird, that has the attributes wings and
beak with the method flapWings and openMouth, and then you can have a class called Pereguine that extends from Bird
with the added methods of _diveBomb_ and _fly_, and another class called _Penguin_ that has the methods _waddle_ and _swim_.

In java, there are three different types of classes: Normal classes which is what you are used to (idk the name lmao),
abstract classes, and interfaces. There is also something called a _final_ class, which is pretty much a normal class
but nothing can inherit from it because it's too cool.

The difference of the three is:
    A normal class must have all methods defined and can have attributes.
    An abstract class can have some methods defined and some left up to be implemented by the extending class
        (defined with a function signature). Can have attributes.
    An interface does not have any methods defined (so everything is a method signature) and cannot have any attributes.

You _extend_ classes and abstract classes and you _implements_ interfaces. So, for example, if you have ClassA, AbstractClassA,
and InterfaceA, the syntax to extend the first two and implement the interface is:

    class SomeClass extends ClassA
    class SomeClass extends AbstractClassA
    class SomeClass implements InterfaceA
    class SomeClass extends ClassA implements InterfaceA

Note that you can only extend to a single class BUT you can implement as many interfaces as you want. The logic
behind this is that if you are extending multiple classes and there is a method with the same method signature
in multiple classes then it is unknown which one you are trying to invoke. For interfaces, since you are implementing
the methods, it does not matter if multiple interfaces share the same method signature.

## Generics
Finally, we will discuss generics. Notice that certain methods in Java can take multiple arguments, such as ```System.out.println```
can take Strings, Integers, etc as can Lists, HashMaps, etc. This is accomplished through the use of generics,
which can accept multiple variable types. You can have generic classes or methods, and generic methods do not necessarily
have to be contained in a generic class and a generic class does not need to contain only generic methods.

To declare a class as generic, or to indicate that the class will accept multiple types, you would do:
```
class SomeClass<T> { /* fill in class */ }
```

The angle brackets tells the JVM that this class will accept a single generic type, and it will be accessed within the class with
the name T. You can use anything, it does not need to be a single captial letter. So, if we have an attribute and a method that uses
T, you can do the following:
```
T attribute;

public T getAttribute() { return this.attribute; }
``` 

You can also specify a function takes a generic type:
```
public<T> T get(T t) {/* code */}
```
You can specify it takes multiple generic types by specifying multiple types in the type parameter:
```
class SomeClass<T,V>
```
And you can also specify that a type implements or extends a certain class like so:
```
class SomeClass<T extends AClass, V implements AInterface>
```
OR you can say that the type that it takes is the super class of another class. So, for example, let us say that B and C extends from A
then you can do something like
```
class SomeClass<T super A>
```
and then this class would accept B and C.

When you are creating an instance of a generic class, you MUST pass in the type. For example, the List and ArrayList classes are generic
classes and you would create a list instance like so:
```
List<String> aList = new ArrayList<String>();
```

