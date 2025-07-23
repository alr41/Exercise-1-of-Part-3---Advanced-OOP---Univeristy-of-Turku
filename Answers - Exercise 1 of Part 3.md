# Exercise-1-of-Part-3--Advanced-OOP--Univeristy-of-Turku

## 1. Student ID
Student ID: 2406530

## 2. What form of inheritance is involved in the following case?
### Code snippet provided
```java
abstract class CommandLineApp {
    String ask(String prompt) {
        System.out.print(prompt + ": ");
        return new java.util.Scanner(System.in).next();
    }
}

class LoginScreen extends CommandLineApp {
    void lock() {
        while(true) {
            var id = ask("Username");
            var pw = ask("Passoword");

            if (id.equals("root") && pw.equals("root"))
                return;

            try {
                Thread.sleep(1000);
            }
            catch(Exception e) {}
        }
    }
}
```
### Answer
The form of inheritance involved in the code snippet is specialization. This is evident because the LoginScreen class represents a more specific version of the CommandLineApp class, adding new behavior, such as the lock() method, that builds on the functionality provided by the superclass, particularly using the ask() method to handle username and password input.

## 3. Comment on and evaluate the following code. Why does it work / not work? What are the benefits and drawbacks associated with it? 
### Code snippet provided
```java
interface NaturalResource {
    // How much of the natural resources are left?
    // @.pre true
    // @.post RESULT == (amount let)
    float amountLeft();

    // Spend x percent
    // @.pre amount >= 0
    // @.post amountLeft() == amount + OLD(amountLeft())
    void spend(float amount);
}

class Coal implements NaturalResource {
    private float left;

    Coal(float amount) {
        left = amount;
    }

    @Override
    public float amountLeft() {
        return left;
    }

    @Override
    public void spend(float amount) {
        left -= amount;
    }
}

class Hydroelectric implements NaturalResource {
    private float left = 100;

    @Override
    public float amountLeft() {
        return left;
    }

    // @.pre cant be called
    // @.post throws Exception
    private void spend(float amount) throws Exception {
        throw new Exception("Renewable is limitless!");
    }
}
```
### Answer
The code design uses a shared interface (NaturalResource) to represent different types of resources, such as coal and hydroelectric. Overall, the interface defines the expected behaviour for the classes by providing the **amountLeft()** and **spend()** methods, which aim to track how much of the natural resources are left and what portion can be consumed.

The **Coal** class correctly implements the mentioned methods by defining the internal variable **left** that updates based on the **spend()** method.

The main issue with the code is in the **Hydroelectric** class, which defines the **spend()** method as private, as opossed to the public standard. As a result, the code will fail to compile since it will not be accessible from outside the class. In an interface, the methods declared must be implemented as public in order to function. Additionally, the thrown exception does not comply with the **spend()** methods, as it suggests that the method should not be called at all, which conflicts with the intended functionality of the interface.

The drawback of the code is particularly the **Hydroelectric** class, due to the due to the incorrect access modifier of the spend() method and its design of throwing an exception.

The benefits of the snippet include polymorphism, which allows the resources to be treated uniformly, avoiding possible inconsistencies when managing different types of resources and simplifying the code.

## 4. Comment on and evaluate the following code. Why does it work / not work? What are the benefits and drawbacks associated with it? 
### Code snippet provided
```java
class RandomGenerator {
    Object generate() {
        var random = new java.util.Random();
        return switch (random.nextInt(4)) {
            case 1 -> 1;
            case 2 -> 2;
            case 3 -> "three";
            default -> new Object();
        };
    }
}

class RandomIntegerGenerator extends RandomGenerator {
    @Override
    Integer generate() {
        return new java.util.Random().nextInt(64);
    }
}
```
### Answer
The code snippet provided consists of two classes: **RandomGenerator**, which attempts to return random objects of different types, and **RandomIntegerGenerator**, which is an extension of the previous class  and tries to override it to return an Integer.

Altough the classes may seem to work at first glance, they are not able to compile. This is mainly due to the following segment:

```java
    @Override
    Integer generate()
```

Since the **RandomGenerator** class can return an Object of various types, not exclusively an Integer, the subclass method attempting to override it with only an Integer return type leads to the compiling error.

For the code to properly work, the return type should be consistent with the superclass method, such as still returning an Object, or the superclass itself should use generics to allow subclasses to specify the return type.
