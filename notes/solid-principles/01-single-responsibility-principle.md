# Single Responsibility Principle

- As per SRP, the class should have one, and only one reason to change.
- It not only applies to a class but also to a method/function and modules.
- The "responsibility" in SRP is the reason to change the code, not neccessarily perform a single task.
- A class can perform multiple related actions as long as they all serve one clear purpose.
- If two methods change for completely different reasons (e.g., business rule vs. display rule), SRP is broken.

## How to detect if this principle is being violated?

- If there are multiple reasons to change the code, it means that the SRP was not kept in mind when writing the code.

## Example of a bad code

```java
public class Employee {

    private int id;

    // Constructor
    public Employee(int id) {
        this.id = id;
    }

    // Getter
    public int getId() {
        return this.id;
    }

    // Member functions
    public String fetchBioData() {
        return "Some bio data";
    }

    public double calculateSalary() {
        return 0;
    }

    public void printPerformanceData() {
        System.out.println("Performance data...");
    }
}
```

- This class violates SRP as there are multiple reasons to change the file.
- Following are the reasons:

1. Introducing new properties in the class will result in adding more getters and setters. This in fact will result in major code changes.
2. The logic of method `calculateSalary()` can change over time based on the change in the tax regime. This will result in code changes.
3. Assume that currently `printPerformanceData()` prints the data in pdf format. Later we might require to print it in an excel or csv format. This will result in code change.

- This will reduce the re-usability of the current Employee class and make it very complex.
- We want to avoid making unneccessary changes and updation to this particular class.
- Unit tests will be too complex as there are lot of things happening in just one particular class.
- So sharing of responsibility makes it harder to maintain.

## Improving the Employee class

```java
public class Employee {
    private int id;

    public Employee(int id) {
        this.id = id;
    }

    public int getId() {
        return this.id;
    }
}
```

- Now the `Employee` class is only responsible for employee entity management.
- For another functionalities, we can have separate classes as follows:

```java
public class EmployeeSalaryCalculator {
    public double calculateSalary(Employee employee) {
        return 0;
    }
}
```

- Only reason to change the `EmployeeSalaryCalculator` class is when the logic to calculate the salary changes.

```java
public class FetchEmployeeBioData {
    public void fetchData(Employee employee) {
        // Returns the data.
    }
}
```

- Only reason to change the `FetchEmployeeBioData` class is when the format of the data needs to be changed.

```java
public class EmployeePerformanceEvaluator {
    public void evaluatePerformance(Employee employee) {
        // Some logic to evaluate the performance
    }
}
```

- The only reason to change the `EmployeePerformanceEvaluator` class is when the logic to evaluate the performance changes.

## Signs of SRP being violated

1. The class name is vague. (`Manager`, `Helper`, `Processor`)
2. You can't summarize what the class does in one short sentence without using `and`.
3. It depends on too many external modules (high coupling).
4. Different developers or teams needs to modify it for different purposes.

## Benefits of SRP

1. It ensures high cohesion (each class focuses on one thing).
2. It reduces coupling (classes don’t depend on multiple, unrelated reasons to change).
3. It also makes it easier to follow other SOLID principles like OCP and DIP.
