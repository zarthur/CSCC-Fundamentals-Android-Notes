# Weeks 9 and 10 - Interfaces and Abstract Classes

## Corresponding Text
*Learn Java for Android Development*, pp. 167-169, 174-183

## Abstract Classes and Abstract Methods
Imaging that we wanted to create classes representing different kinds of
animals.  We could start with an *Animal* base class that contained all the
state and behavior shared by all animals and create subclasses to represent
specific animal species.  For example, we could write the following to
represent dogs and cats.

```java
package com.myname.week_09_10;

class Animal {
    private String name;
    private int age;

    Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class Dog extends Animal {
    private String breed;

    Dog(String name, int age, String breed) {
        super(name, age);
        this.breed = breed;
    }
}

class Cat extends Animal {
    private String furColor;

    Cat(String name, int age, String furColor) {
        super(name, age);
        this.furColor = furColor;
    }
}

public class Main {
    public static void main(String[] args) {
        Dog odie = new Dog("Odie", 5, "beagle");
        Cat garfield = new Cat("Garfield", 6, "orange");
    }
}
```

Here, the *Animal* base class takes care of storing an animal's name and age;
the *Dog* and *Cat* derived classes manage a property unique to each class.  
Because *Dog* and *Cat* extend *Animal*, instances of *Dog* and *Cat* have
*name* and *age* fields automatically.  What if we wanted to require that all
instances of subclasses to *Animal* have a *Speak()* method?  We could
implement the method in the *Animal* class and override it in each child class
but it might not make sense to provide an implementation in the base class
itself. One option we have it to declare the method method and leave it's body
empty in the base class.  

```java
package com.myname.week_09_10;

class Animal {
    private String name;
    private int age;

    Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void speak() {}
}

class Dog extends Animal {
    private String breed;

    Dog(String name, int age, String breed) {
        super(name, age);
        this.breed = breed;
    }
}

class Cat extends Animal {
    private String furColor;

    Cat(String name, int age, String furColor) {
        super(name, age);
        this.furColor = furColor;
    }
}

public class Main {
    public static void main(String[] args) {
        Dog odie = new Dog("Odie", 5, "beagle");
        Cat garfield = new Cat("Garfield", 6, "orange");

        odie.speak();
        garfield.speak();
    }
}
```

While instances of *Dog* and *Cat* now have a *Speak()* method, there's nothing
that requires us to specify an implementation for those classes beyond the base
class's implementation.  What we might like to do is leave the implementation
undefined in the base class but require derived classes to provide an
implementation unique to the child class.

To do this, we can make use of an abstract method.  **Abstract methods** are
declared methods lacking a body or implementation.  Abstract methods are
declared by prefixing a method header with the `abstract` reserved word.  
Abstract methods must be declared in abstract classes.  **Abstract classes**
are classes that may or may not contain abstract methods and cannot be used
to create instances directly; in order to create an instance, a subclass must
be created that provides an implementation of any abstract methods.  Classes
are declared abstract by prefixing the class declaration with the `abstract`
reserved word.  We can declare the *Animal.speak()* method as abstract allowing
us to avoid providing an implementation in the *Animal* class but requiring
that non-abstract subclasses define an implementation for the method.  Once
we declare a method as abstract, we must declare the class containing the
method as abstract as well.  

```java
package com.myname.week_09_10;

abstract class Animal {
    private String name;
    private int age;

    Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    abstract public void speak();
}

class Dog extends Animal {
    private String breed;

    Dog(String name, int age, String breed) {
        super(name, age);
        this.breed = breed;
    }

    @Override
    public void speak() {
        System.out.println("Woof!");
    }
}

class Cat extends Animal {
    private String furColor;

    Cat(String name, int age, String furColor) {
        super(name, age);
        this.furColor = furColor;
    }

    @Override
    public void speak() {
        System.out.println("Meow!");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog odie = new Dog("Odie", 5, "beagle");
        Cat garfield = new Cat("Garfield", 6, "orange");

        odie.speak();
        garfield.speak();
    }
}
```

Notice that after declaring an abstract method, we must terminate the statement
with a semicolon.  Neither the *Dog* nor the *Cat* class is abstract so both
must provide implementations of the *Speak()* method.  

Suppose we wanted to add a *Bird* and a *Bat* class.  Both these types of
animals can fly so we might want to provide a *fly()* method.  We might
consider adding the *Fly()* method to the *Animal* class as an abstract method
but it doesn't really make sense for animals that can't fly.  One option we
have is to extend the *Animal* class with an abstract subclass.  

```java
package com.myname.week_09_10;

abstract class Animal {
    private String name;
    private int age;

    Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    abstract public void speak();
}

abstract class FlyingAnimal extends Animal {
    FlyingAnimal(String name, int age) {
        super(name, age);
    }

    abstract public void fly();
}

class Dog extends Animal {
    private String breed;

    Dog(String name, int age, String breed) {
        super(name, age);
        this.breed = breed;
    }

    @Override
    public void speak() {
        System.out.println("Woof!");
    }
}

class Cat extends Animal {
    private String furColor;

    Cat(String name, int age, String furColor) {
        super(name, age);
        this.furColor = furColor;
    }

    @Override
    public void speak() {
        System.out.println("Meow!");
    }
}

class Bird extends FlyingAnimal {
    private int wingspan;

    Bird(String name, int age, int wingspan) {
        super(name, age);
        this.wingspan = wingspan;
    }

    @Override
    public void speak() {
        System.out.println("Chirp!");
    }

    @Override
    public void fly() {
        System.out.println("Flying...");
    }
}

class Bat extends FlyingAnimal {
    private int weight;
    Bat(String name, int age, int weight) {
        super(name, age);
        this.weight = weight;
    }

    @Override
    public void fly() {
        System.out.println("Flying...");
    }

    @Override
    public void speak() {
        System.out.println("Bat sounds!");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog odie = new Dog("Odie", 5, "beagle");
        Cat garfield = new Cat("Garfield", 6, "orange");
        Bird tweety = new Bird("Tweety", 2, 4);
        Bat batty = new Bat("Batty", 2, 1);

        odie.speak();
        garfield.speak();
        tweety.speak();

        batty.fly();
    }
}
```

## Interfaces
### Declaring Interface
### Implementing Interfaces
### Examples from the Standard Library
Implement the Comparable interface.
### Extending Interfaces
### Decoupling Interface from Implementation


## Exercises
1.
2. Implement the Iterable interface.  
