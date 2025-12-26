# Liskov Substitution Principle

- The LSP principle states that “Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program.”
- In simpler words, wherever we use a parent class or an interface, we should be able to use its subclass without breaking the behaviour.

## Why it matters?

- When we violate LSP:
  1. Polymorphism breaks as the subclass doesn't truly behave like a parent class.
  2. Code that expects a base class behaves unpredictably when given to a derived one.
  3. Hidden bugs are introduced that are hard to detect because inheritance "appears" correct.

## Core idea

- Assume we have:

```java
PaymentProcessor processor = new StripeProcessor();
processor.process(100);
```

- Later, we replace it with:

```java
PaymentProcessor processor = new PaypalProcessor();
processor.process(100);
```

- Now if the program still behaves correctly, it follows the LSP.
- If it crashes or behaves differently for example, `PaypalProcessor` throws an exception for certain valid payments, it violates LSP.

### Example 1

#### Bad Code

##### `CreditCard.java`

```java
public abstract class CreditCard {

    // Props
    private String ccNumber;
    private String ownerName;
    private int cvv;

    // Setters
    public void setCCNumber(String ccNumber) {
        this.ccNumber = ccNumber;
    }

    public void setOwnerName(String ownerName) {
        this.ownerName = ownerName;
    }

    public void setCVV(int cvv) {
        this.cvv = cvv;
    }

    // Abstract Behaviours
    public abstract void tapAndPay();

    public abstract void onlineTransfer();

    public abstract void swipeAndPay();

    public abstract void mandatePayment();

    public abstract void upiPayment();

    public abstract void internationalPayment();

    // Standard behaviour
    public void displayCreditCardDetails() {
        System.out.println("CC Number: " + this.ccNumber);
        System.out.println("Owner Name: " + this.ownerName);
    }
}
```

##### `MasterCard.java`

```java
package P3_LSP.example_3.bad_code;

public class MasterCard extends CreditCard {

    @Override
    public void tapAndPay() {
        // Implementation of tap and pay
        System.out.println("Tap and pay initiated for mastercard");
    }

    @Override
    public void onlineTransfer() {
        // Implementation for online transfer
        System.out.println("Online transfer initiated for mastercard");
    }

    @Override
    public void swipeAndPay() {
        // Implementation for swipe and pay
        System.out.println("Swipe and pay initiated for mastercard");
    }

    @Override
    public void mandatePayment() {
        // Implementation for mandate payments
        System.out.println("Mandate payment initiated for mastercard");
    }

    /*
     * UPI payment is not supported by mastercard.
     * Yet we have to forcefully implement it and throw an exception.
     */
    @Override
    public void upiPayment() {
        throw new UnsupportedOperationException("UPI payment is not supported");
    }

    @Override
    public void internationalPayment() {
        // Implementation for mandate payments
        System.out.println("International payment initiated for mastercard");
    }

}
```

##### `VisaCard.java`

```java
package P3_LSP.example_3.bad_code;

public class VisaCard extends CreditCard {

    @Override
    public void tapAndPay() {
        // Implementation for tap and pay
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void onlineTransfer() {
        // Implementation for online transfer
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void swipeAndPay() {
        // Implementation for swipe and pay
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void mandatePayment() {
        // Implementation for mandate payment
        System.out.println("Swipe and pay initiated for visa card");
    }

    /*
     * UPI payment is not supported by mastercard.
     * Yet we have to forcefully implement it and throw an exception.
     */

    @Override
    public void upiPayment() {
        throw new UnsupportedOperationException("UPI payment is not supported");
    }

    @Override
    public void internationalPayment() {
        // Implementation for international payment
        System.out.println("Swipe and pay initiated for visa card");
    }

}
```

##### `RuPayCard.java`

```java
package P3_LSP.example_3.bad_code;

public class RuPayCard extends CreditCard {

    @Override
    public void tapAndPay() {
        // Implementation for tap and pay
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void onlineTransfer() {
        // Implementation for online transfer
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void swipeAndPay() {
        // Implementation for swipe and pay
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void mandatePayment() {
        // Implementation for mandate payment
        System.out.println("Swipe and pay initiated for visa card");
    }

    /*
     * UPI payment is supported by a RuPay card so it makes sense to have its
     * implementation.
     */

    @Override
    public void upiPayment() {
        // Implementation for UPI payment
        System.out.println("Swipe and pay initiated for visa card");
    }

    /*
     * But RuPay cards are not made for international payments.
     * Still we have to forcefully implement it and throw and exception.
     */

    @Override
    public void internationalPayment() {

        throw new UnsupportedOperationException("International payment is not supported on a RuPay card.");
    }

}
```

##### `Main.java`

```java
package P3_LSP.example_3.bad_code;

import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<CreditCard> creditCards = new ArrayList<>();

        for (CreditCard card : creditCards) {
            if (card instanceof RuPayCard) {
                card.upiPayment();
            }
        }

        /*
         * The above code loses the possibility of abstraction.
         * This module needs to depend on CreditCard and doesn't need to know what are
         * the different possible instance of CreditCard.
         *
         * The if checks are going to be painful because if I want to do international
         * payment, I need to again loop through the cards and match the ones that can
         * perform an international payment.
         * These includes the ones which doesn't support internation payment as well.
         * So we ned to do checks if the card is an instance of MasterCard or VisaCard
         * only then international payment is possible.
         *
         * Ideally, if we want to process a UPI Payment, all we need is:
         * List<UPISupportedCreditCards> upiCards = new ArrayList<>();
         * Now, all the cards will definitely support UPI Payments and all we can do is
         * card.makePayment().
         *
         * This makes a good use of Abstraction Principle.
         */
    }
}
```

#### Better Code

##### `CreditCard.java`

```java
public abstract class CreditCard {

    // Props
    private String ccNumber;
    private String ownerName;
    private int cvv;

    // Setters
    public void setCCNumber(String ccNumber) {
        this.ccNumber = ccNumber;
    }

    public void setOwnerName(String ownerName) {
        this.ownerName = ownerName;
    }

    public void setCVV(int cvv) {
        this.cvv = cvv;
    }

    // Abstract Behaviours
    public abstract void tapAndPay();

    public abstract void onlineTransfer();

    public abstract void swipeAndPay();

    public abstract void mandatePayment();

    public abstract void upiPayment();

    public abstract void internationalPayment();

    // Standard behaviour
    public void displayCreditCardDetails() {
        System.out.println("CC Number: " + this.ccNumber);
        System.out.println("Owner Name: " + this.ownerName);
    }
}
```

##### `InternationalCompatiblePaymentCards.java`

```java
package P3_LSP.example_3.better_code;

public interface InternationalCompatiblePaymentCreditCards {
  void internationalPayment();
}
```

##### `UPICompatiblePaymentCards.java`

```java
package P3_LSP.example_3.better_code;

public interface UPICompatiblePaymentCreditCard {
  void upiPayment();
}
```

##### `VisaCard.java`

```java
package P3_LSP.example_3.better_code;

public class VisaCard extends CreditCard implements UPICompatiblePaymentCreditCard {

    @Override
    public void tapAndPay() {
        // Implementation for tap and pay
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void onlineTransfer() {
        // Implementation for online transfer
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void swipeAndPay() {
        // Implementation for swipe and pay
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void mandatePayment() {
        // Implementation for mandate payment
        System.out.println("Swipe and pay initiated for visa card");
    }

    /*
     * UPI payment is not supported by mastercard.
     * Yet we have to forcefully implement it and throw an exception.
     */

    @Override
    public void upiPayment() {
        throw new UnsupportedOperationException("UPI payment is not supported");
    }
}
```

##### `MasterCard.java`

```java
package P3_LSP.example_3.better_code;

public class MasterCard extends CreditCard implements InternationalCompatiblePaymentCreditCards {

    @Override
    public void tapAndPay() {
        // Implementation of tap and pay
        System.out.println("Tap and pay initiated for mastercard");
    }

    @Override
    public void onlineTransfer() {
        // Implementation for online transfer
        System.out.println("Online transfer initiated for mastercard");
    }

    @Override
    public void swipeAndPay() {
        // Implementation for swipe and pay
        System.out.println("Swipe and pay initiated for mastercard");
    }

    @Override
    public void mandatePayment() {
        // Implementation for mandate payments
        System.out.println("Mandate payment initiated for mastercard");
    }

    @Override
    public void internationalPayment() {
        // Implementation for mandate payments
        System.out.println("International payment initiated for mastercard");
    }

}
```

##### `RuPayCard.java`

```java
package P3_LSP.example_3.better_code;

public class RuPayCard extends CreditCard implements UPICompatiblePaymentCreditCard {

    @Override
    public void tapAndPay() {
        // Implementation for tap and pay
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void onlineTransfer() {
        // Implementation for online transfer
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void swipeAndPay() {
        // Implementation for swipe and pay
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void mandatePayment() {
        // Implementation for mandate payment
        System.out.println("Swipe and pay initiated for visa card");
    }

    @Override
    public void upiPayment() {
        // Implementation for UPI payment
        System.out.println("Swipe and pay initiated for visa card");
    }
}
```

##### `Main.java`

```java
package P3_LSP.example_3.better_code;

import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<UPICompatiblePaymentCreditCard> upiCards = new ArrayList<>();

        for (UPICompatiblePaymentCreditCard upiCard : upiCards) {
            upiCard.upiPayment();
        }

        List<InternationalCompatiblePaymentCreditCard> internationalCards = new ArrayList<>();

        for(InternationalCompatiblePaymentCreditCard intlCard : internationalCards) {
          intlCard.internationalPayment();
        }
    }
}
```

## Problems due to inheritance

- When developers use inheritance, their major reason is to implement code reusability.
- Any method that is public or protected in the super class will also be available in the sub class.

## How to identify LSP violations?

1. Conditional logic (using the `instanceof` operator or `object.getClass().getName()` to identify the actual subclass) in client code.
2. Empty, do-nothing implementations of one or more methods in subclasses.
3. Throwing an `UnsupportedOperationException` or some other unexpected exception from a subclass method.
