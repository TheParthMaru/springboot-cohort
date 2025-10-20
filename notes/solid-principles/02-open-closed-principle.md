# Introduction

- The Open-Closed Principle states that the classes should be open for extension but closed for modification.
- In doing so, we stop ourselves from modifying existing code and creating potential new bugs.

## Example 1

### Bad Code

#### `PaymentType.java`

```java
public enum PaymentType {
    CREDIT_CARD,
    BANK_TRANSFER,
    PAYPAL
}

// This is an enum that represents the type of payment.
```

#### `PaymentProcessor.java`

```java
public class PaymentProcessor {
    public void processPayment(PaymentType paymentType, double amount) {
        switch (paymentType) {
            case CREDIT_CARD:
                System.out.println("Process payment through CREDIT CARD");
                // Logic for processing payment through credit card
                break;

            case BANK_TRANSFER:
                System.out.println("Process payment through BANK TRANSFER");
                // Logic for processing payment through bank transfer
                break;

            case PAYPAL:
                System.out.println("Process payment through PAYPAL");
                // Logic for processing payment through paypal
                break;

            default:
                System.out.println("Invalid payment type");
                break;
        }
    }
}
```

- In order to add a new payment method, let's say Crypto payments, we need to add a new enum `CRYPTO".
- Then we need to add a new case in the switch case of the `PaymentProcessor.java`.
- This is when the OCP is violated.
- The code is not closed for modification. There will be modifications to the existing code as soon as a new payment type is introduced.
- So rather than extending the code, we are modifying a lot of existing code.

### Better code

#### `PaymentType.java`

```java
public enum PaymentType {
    CREDIT_CARD,
    BANK_TRANSFER,
    PAYPAL
}

// This is an enum that represents the type of payment.
```

#### `PaymentProcessor.java`

```java
public interface PaymentProcessor {
    PaymentType supports();

    void processPayment();
}
```

- We use interface rather than using a concrete class.

#### `BankTransferProcessor.java`

```java
public class BankTransferProcessor implements PaymentProcessor {
    @Override
    public PaymentType supports() {
        return PaymentType.BANK_TRANSFER;
    }

    @Override
    public void processPayment() {
        // Logic to process bank transfer payments
    }
}
```

- Individual concrete classes of each payment types that implements the `PaymentProcessor` interface.

#### `CreditCardProcessor.java`

```java
public class CreditCardProcessor implements PaymentProcessor {

    @Override
    public PaymentType supports() {
        return PaymentType.CREDIT_CARD;
    }

    @Override
    public void processPayment() {
        // Logic to process credit card payments
    }

}
```

#### `PaypalProcessor.java`

```java
public class PaypalProcessor implements PaymentProcessor {
    @Override
    public PaymentType supports() {
        return PaymentType.PAYPAL;
    }

    @Override
    public void processPayment() {
        // Logic to process Paypal payments
    }
}
```

#### `PaymentProcessorRegistry.java`

```java
import java.util.HashMap;
import java.util.Map;

public class PaymentProcessorRegistry {
    private final Map<PaymentType, PaymentProcessor> processors = new HashMap<>();

    public PaymentProcessorRegistry() {
        processors.put(PaymentType.CREDIT_CARD, new CreditCardProcessor());
        processors.put(PaymentType.BANK_TRANSFER, new BankTransferProcessor());
        processors.put(PaymentType.PAYPAL, new PaypalProcessor());
    }

    public PaymentProcessor getPaymentProcessor(PaymentType type) {
        return processors.get(type);
    }
}
```

- A registry is simply a Map that maps each payment type to its corresponding processor.
- This way there is no branching (if else, switch) required.
- There is no modification when new payment type appears.
- The registry map allows us to lookup behaviour rather than hard-code decisions.

#### `Main.java`

```java
public class Main {
    public static void main(String[] args) {
        PaymentProcessorRegistry paymentProcessorRegistry = new PaymentProcessorRegistry();

        // Payment through bank transfer
        PaymentProcessor bankTransferProcessor = paymentProcessorRegistry
                .getPaymentProcessor(PaymentType.BANK_TRANSFER);
        bankTransferProcessor.processPayment();

        // Payment through paypal
        PaymentProcessor paypalProcessor = paymentProcessorRegistry.getPaymentProcessor(PaymentType.PAYPAL);
        paypalProcessor.processPayment();

        // Payment through credit card
        PaymentProcessor creditCardProcessor = paymentProcessorRegistry.getPaymentProcessor(PaymentType.CREDIT_CARD);
        creditCardProcessor.processPayment();

        // In order to add a new payment method, let's say Crypto, do the following:
        // 1. Add a new enum Crypto
        // 2. Create a new class CryptoProcessor that implements PaymentProcessor.
        // 3. Add this to the PaymentProcessorRegistry map.
        // 4. Now we can just use it by the following code:
        // PaymentProcessor cryptoProcessor =
        // paymentProcessorRegistry.getPaymentProcessor(PaymentType.CRYPTO);
        // cryptoProcessor.processPayment()
    }
}
```

## How to detect if this principle is being violated?

- If we are able to add new behaviour or feature by adding new code like new classes, then the code follows the OCP.
- But we have to change existing, tested, stable code to introduce new behaviour, then OCP is violated.
- Classes, modules, and functions should be open for extension but closed for modification.

## Why map is better than the switch case in our example?

| Concept                       | `switch` Statement             | `Map` / Registry                  |
| ----------------------------- | ------------------------------ | --------------------------------- |
| **Decision control**          | Hard-coded, manual             | Data-driven, flexible             |
| **Adding new payment type**   | Modify existing code           | Add a new class and register it   |
| **OCP compliance**            | Violated                       | Satisfied                         |
| **Polymorphism used?**        | No                             | Yes                               |
| **Code coupling**             | Tight (depends on all classes) | Loose (depends on interface only) |
| **Risk of breaking old code** | High                           | Low                               |

- When a new type comes in, you don't ask "If it is this, then do that."
- You simply look it up in the directory/map and it knows what to do.

## When enums are good or bad?

- Good: When used as keys or identifiers like the `PaymentType`.
- Bad: When used inside a switch case or if-else to control the behaviour.

## Patterns used in the code

| Pattern                        | Purpose                                                                  |
| ------------------------------ | ------------------------------------------------------------------------ |
| **Strategy Pattern**           | Each payment type defines its own strategy for processing.               |
| **Factory / Registry Pattern** | Removes branching by storing and retrieving strategies dynamically.      |
| **Polymorphism**               | Allows treating all processors uniformly (`PaymentProcessor` interface). |

## Summary

| Aspect          | Non-OCP Version     | OCP-Compliant Version                |
| --------------- | ------------------- | ------------------------------------ |
| Logic control   | `switch` statements | `Map<PaymentType, PaymentProcessor>` |
| Adding new type | Edit code           | Add new class                        |
| Polymorphism    | Not used            | Fully used                           |
| Code coupling   | High                | Low                                  |
| Risk of bugs    | High                | Low                                  |
| Extensibility   | Poor                | Excellent                            |
| Enum usage      | Controls logic      | Used as identifier                   |
| Patterns used   | None                | Strategy + Registry                  |

- OCP is not about removing enums or switches entirely.
- It’s about designing systems so that new behavior can be added without modifying existing code.
- A registry (map) helps by turning “decision code” into “data configuration,” letting polymorphism do the rest.
