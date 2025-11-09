# Abstraction

- Abstraction means showing only essential features of an object while hiding the unnecessary implementation details.
- It helps us focus on what an object does, not how an object does.

## Why do we need Abstraction?

- Without abstraction:

  - Code becomes cluttered with low-level implementation details.
  - You expose unnecessary internal logic.
  - Maintenance becomes harder.

- With abstraction:
  - Code becomes clean, modular and re-usable.
  - You can change the internal implementation without affecting other parts of the code.
  - Devs can work on high-level concepts without worrying about the low-level details.

## How abstraction is achieved?

| Mechanism            | Description                                                                                                                   | Level of Abstraction |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| **Abstract Classes** | Classes that cannot be instantiated directly. They can have both abstract (unimplemented) and concrete (implemented) methods. | Partial abstraction  |
| **Interfaces**       | Contain only abstract methods (until Java 7). From Java 8+, they can also include `default` and `static` methods.             | Full abstraction     |

## Abstract classes

- Declared with `abstract` keyword.
- Can have:
  - Abstract methods (no body)
  - Concrete methods (with body)
  - Fields (variables)
  - Constructors

### `Animal.java`

```java
abstract class Animal {
    // Abstract method which needs to be implemented in the subclass
    abstract void sound();

    // Concrete method - Shared implementation
    void eat() {
        System.out.println("Eating...");
    }
}
```

### `Dog.java`

```java
public class Dog extends Animal{
    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

### `Cat.java`

```java
public class Cat extends Animal {
    @Override
    public void sound() {
        System.out.println("Meow");
    }
}
```

### `Main.java`

```java
public class Main {
    public static void main(String[] args) {
        Animal dog = new Dog();
        dog.sound();  // Output: Bark
        dog.eat();    // Output: Eating...
    }
}
```

- `Animal` cannot be instantiated directly.
- Subclasses must implement the abstract method.
- We can include normal methods like `eat()` to share common code.

## Interface

- An interface is used to define a contract for a class.
- This contract showcases what a class can do, but does not mention how it does it.

### `Vehicle.java`

```java
public interface Vehicle {
    void start();
    void stop();
}
```

### `Car.java`

```java
public class Car implements Vehicle{
    @Override
    public void start() {
        System.out.println("Car starting");
    }

    @Override
    public void stop() {
        System.out.println("Car stopping");
    }
}
```

### `Main.java`

```java
public class Main {
    public static void main(String[] args) {
        Vehicle car = new Car();
        car.start();
        car.stop();
    }
}
```

- Interfaces can only have abstract methods (implicitly public and abstract).
- A class that implements an interface must implement all of its methods.
- Since Java 8, an interface can have:
  - Static methods
  - Default methods (with implementations)
- Since Java 9, an interface can also have a private method.

## Abstract class vs Interface

| Feature     | Abstract Class                              | Interface                                                             |
| ----------- | ------------------------------------------- | --------------------------------------------------------------------- |
| Keyword     | `abstract`                                  | `interface`                                                           |
| Inheritance | Can extend one abstract class               | Can implement multiple interfaces                                     |
| Methods     | Can have both abstract and concrete methods | Until Java 7: only abstract; Java 8+: can have default/static methods |
| Variables   | Can have instance variables                 | Only `public static final` constants                                  |
| Constructor | Can have constructors                       | Cannot have constructors                                              |
| Use case    | When classes share common behavior          | When you want to define a contract (capabilities)                     |

- Encapsulation hides the data through access modifiers whereas abstraction hides the implementation complexity.
