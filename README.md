# design-patterns

What are Design Patterns?

Design patterns are reusable solutions to common problems that arise during software design and development. They are not complete solutions themselves, but rather, they provide general templates and guidelines for solving recurring design issues. Design patterns help developers create software that is more maintainable, modular, and flexible by promoting best practices and proven techniques.

Design patterns were first introduced in the book "Design Patterns: Elements of Reusable Object-Oriented Software" by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides (often referred to as the "Gang of Four" or GoF), published in 1994. This book identified 23 commonly occurring design problems in software development and provided solutions in the form of design patterns.

There are three main categories of design patterns:

1. **Creational Patterns:** These patterns focus on object creation mechanisms, abstracting the instantiation process to make it more flexible and reusable. Examples include the Singleton pattern (ensuring only one instance of a class exists), Factory pattern (centralized object creation), and Builder pattern (step-by-step construction of complex objects).

2. **Structural Patterns:** These patterns deal with the composition of classes and objects to form larger structures. They help define relationships between objects, making it easier to manage and modify the structure of a system. Examples include the Adapter pattern (interfacing incompatible classes), Decorator pattern (adding behavior dynamically), and Composite pattern (treating individual objects and compositions uniformly).

3. **Behavioral Patterns:** These patterns focus on the interaction between objects and their responsibilities. They provide solutions for communication, coordination, and collaboration among objects in a system. Examples include the Observer pattern (one-to-many dependency between objects), Strategy pattern (defining interchangeable algorithms), and Command pattern (encapsulating requests as objects).

Each design pattern has a specific intent, structure, and usage scenario. While design patterns can greatly improve the quality and maintainability of software, it's important to use them judiciously. Overusing patterns or using inappropriate ones for a given context can lead to unnecessarily complex and convoluted code.

Design patterns are an integral part of software engineering and are widely used in various programming languages and paradigms, including object-oriented programming (OOP) and functional programming. They serve as a common language for developers to communicate and share best practices when designing and implementing software systems.


Can you give a detailed example of the "Factory Method" under Creational Patterns?

Certainly! The Factory Method is a creational design pattern that provides an interface for creating objects in a super class, but allows subclasses to alter the type of objects that will be created. This pattern promotes loose coupling between the client code and the concrete classes being instantiated. In Spring Boot, this pattern is often used in scenarios where you want to create objects based on certain conditions or configurations.

Let's take an example of a simple payment processing system using Spring Boot, where we'll implement the Factory Method pattern.

1. First, let's define an interface for our payment processors:

```java
public interface PaymentProcessor {
    void processPayment(double amount);
}
```

2. Next, we'll create two concrete implementations of the `PaymentProcessor` interface: `CreditCardProcessor` and `PayPalProcessor`.

```java
public class CreditCardProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing credit card payment of $" + amount);
        // Actual credit card processing logic here
    }
}

public class PayPalProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing PayPal payment of $" + amount);
        // Actual PayPal processing logic here
    }
}
```

3. Now, we'll define the Factory Method in a class named `PaymentProcessorFactory`. This factory class will create instances of the payment processors based on a given payment method type.

```java
public class PaymentProcessorFactory {
    public PaymentProcessor createPaymentProcessor(String paymentMethod) {
        if ("creditcard".equalsIgnoreCase(paymentMethod)) {
            return new CreditCardProcessor();
        } else if ("paypal".equalsIgnoreCase(paymentMethod)) {
            return new PayPalProcessor();
        } else {
            throw new IllegalArgumentException("Invalid payment method: " + paymentMethod);
        }
    }
}
```

4. Finally, in our Spring Boot application, we can use the factory to create and process payments:

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class PaymentApp {

    public static void main(String[] args) {
        SpringApplication.run(PaymentApp.class, args);

        PaymentProcessorFactory processorFactory = new PaymentProcessorFactory();

        PaymentProcessor creditCardProcessor = processorFactory.createPaymentProcessor("creditcard");
        creditCardProcessor.processPayment(100.0);

        PaymentProcessor payPalProcessor = processorFactory.createPaymentProcessor("paypal");
        payPalProcessor.processPayment(50.0);
    }
}
```

In this example, the `PaymentProcessorFactory` encapsulates the creation logic for payment processors, allowing us to easily switch between different payment methods without modifying the client code. This follows the Factory Method design pattern, where the factory method (`createPaymentProcessor` in this case) handles the object creation based on different conditions.

Remember that this is just a simplified example to illustrate the Factory Method pattern using Spring Boot. In real-world scenarios, you might have more complex object creation logic and additional layers of abstraction in your application.

com
└── yourcompany
    └── paymentapp
        ├── controller
        │   └── PaymentController.java
        ├── service
        │   └── PaymentService.java
        ├── model
        │   └── Payment.java
        ├── factory
        │   └── PaymentProcessorFactory.java
        └── PaymentApp.java (Spring Boot main application class)



Can you give a detailed example of the "Abstract Factory" under Creational Patterns and give the differences between Abstract Factory Method and Factory Method?

Certainly! The Abstract Factory is another creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes. It's used when a system must be independent of how its objects are created, composed, and represented. This pattern is particularly useful when you need to create multiple related objects that belong to the same family.

Let's use an example of a user authentication system using Spring Boot to illustrate the Abstract Factory pattern.

1. First, let's define the abstract factory interface for creating authentication components:

```java
public interface AuthenticationFactory {
    AuthenticationProvider createAuthenticationProvider();
    TokenGenerator createTokenGenerator();
}
```

2. Next, we'll create concrete implementations of the `AuthenticationFactory` interface: `DefaultAuthenticationFactory` and `LDAPAuthenticationFactory`.

```java
public class DefaultAuthenticationFactory implements AuthenticationFactory {
    @Override
    public AuthenticationProvider createAuthenticationProvider() {
        return new DefaultAuthenticationProvider();
    }

    @Override
    public TokenGenerator createTokenGenerator() {
        return new DefaultTokenGenerator();
    }
}

public class LDAPAuthenticationFactory implements AuthenticationFactory {
    @Override
    public AuthenticationProvider createAuthenticationProvider() {
        return new LDAPAuthenticationProvider();
    }

    @Override
    public TokenGenerator createTokenGenerator() {
        return new JWTTokenGenerator();
    }
}
```

3. Now, let's define the authentication components: `AuthenticationProvider` and `TokenGenerator`, along with their concrete implementations.

```java
public interface AuthenticationProvider {
    boolean authenticate(String username, String password);
}

public class DefaultAuthenticationProvider implements AuthenticationProvider {
    @Override
    public boolean authenticate(String username, String password) {
        // Default authentication logic
        return true;
    }
}

public class LDAPAuthenticationProvider implements AuthenticationProvider {
    @Override
    public boolean authenticate(String username, String password) {
        // LDAP authentication logic
        return true;
    }
}

public interface TokenGenerator {
    String generateToken(String username);
}

public class DefaultTokenGenerator implements TokenGenerator {
    @Override
    public String generateToken(String username) {
        // Default token generation logic
        return "default_token";
    }
}

public class JWTTokenGenerator implements TokenGenerator {
    @Override
    public String generateToken(String username) {
        // JWT token generation logic
        return "jwt_token";
    }
}
```

4. In our Spring Boot application, we can use the abstract factory to create authentication components:

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class AuthApp {

    public static void main(String[] args) {
        SpringApplication.run(AuthApp.class, args);

        AuthenticationFactory authenticationFactory = new DefaultAuthenticationFactory();
        AuthenticationProvider authProvider = authenticationFactory.createAuthenticationProvider();
        TokenGenerator tokenGenerator = authenticationFactory.createTokenGenerator();

        // Use authProvider and tokenGenerator for authentication and token generation
    }
}
```

**Differences between Factory Method and Abstract Factory:**

1. **Intent:**
   - Factory Method: Provides an interface for creating objects in a super class, allowing subclasses to alter the type of objects being created.
   - Abstract Factory: Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

2. **Granularity:**
   - Factory Method focuses on creating individual objects (single product).
   - Abstract Factory focuses on creating families of related objects (multiple products).

3. **Usage:**
   - Factory Method is suitable when you need to create different objects of a single type based on specific conditions.
   - Abstract Factory is used when you need to create multiple related objects that belong to different families.

4. **Structure:**
   - Factory Method involves a single method in the super class that subclasses override to create objects.
   - Abstract Factory involves multiple factory methods in an interface or an abstract class, each responsible for creating a family of related objects.

5. **Dependency:**
   - Factory Method typically has a one-to-one relationship between the creator and the product.
   - Abstract Factory has a one-to-many relationship between the abstract factory and the multiple product families.

Both Factory Method and Abstract Factory patterns help to achieve loose coupling and flexibility in object creation, but they are applied to different scenarios based on the level of abstraction and complexity of the objects being created.


