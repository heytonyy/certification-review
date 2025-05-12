# Standard 3: User-Defined Functions - Study Guide

## Overview
Standard 3 covers user-defined functions, scope, top-down design, and functional decomposition in Java. This section represents **23% of the exam** - the second largest portion.

## Part A: Scope and Variable Access

### Core Concepts

#### 1. Understanding Scope
```java
public class ScopeDemo {
    // Class-level (global) variables
    private static int globalCounter = 0;
    private String instanceVariable = "Hello";
    
    public static void main(String[] args) {
        // Local variables
        int localVar = 10;
        
        // Block scope
        for (int i = 0; i < 5; i++) {
            int blockVar = i * 2;
            System.out.println(blockVar);
            // blockVar is only accessible within this block
        }
        // i and blockVar are not accessible here
        
        // Method scope
        doSomething(localVar);
    }
    
    public static void doSomething(int parameter) {
        // parameter is accessible throughout this method
        int methodLocal = parameter * 2;
    }
}
```

#### 2. Local Variables
```java
public static void demonstrateLocalScope() {
    // Local variables must be initialized before use
    int x = 5;
    double y; // Declared but not initialized
    
    if (x > 0) {
        y = 3.14; // Initialize inside conditional block
        int z = 10; // z is only accessible within this if block
    }
    
    // y can be used here (initialized in all paths)
    System.out.println(y);
    // System.out.println(z); // Error: z is out of scope
}

// Variable shadowing
public class ShadowExample {
    private static int value = 100; // Class variable
    
    public static void method() {
        int value = 50; // Local variable shadows class variable
        System.out.println(value); // Prints 50, not 100
        System.out.println(ShadowExample.value); // Access class variable
    }
}
```

#### 3. Global Variables (Static Fields)
```java
public class GlobalExample {
    // Static variables are shared across all instances
    public static int globalCount = 0;
    private static String appName = "My App";
    
    // Instance variables (not truly global)
    private String instanceName;
    
    public static void incrementCounter() {
        globalCount++; // Can access static variables from static methods
    }
    
    public void setName(String name) {
        this.instanceName = name; // Instance method accessing instance variable
        GlobalExample.globalCount++; // Accessing static variable from instance method
    }
}
```

### Advanced Scope Concepts

#### 1. Variable Lifetime
```java
public class LifetimeDemo {
    private static int staticVar = 1; // Lives for entire program duration
    private int instanceVar = 2; // Lives as long as the object exists
    
    public void method() {
        int localVar = 3; // Dies when method exits
        
        for (int i = 0; i < 3; i++) {
            int loopVar = 4; // Dies when loop iteration ends
        }
    }
}
```

#### 2. Parameter Passing and Scope
```java
public class ParameterScope {
    public static void main(String[] args) {
        int[] array = {1, 2, 3};
        int primitive = 10;
        
        modifyArray(array);
        modifyPrimitive(primitive);
        
        System.out.println(Arrays.toString(array)); // Changed: [1, 2, 99]
        System.out.println(primitive); // Unchanged: 10
    }
    
    public static void modifyArray(int[] arr) {
        arr[2] = 99; // Modifies the original array (reference)
    }
    
    public static void modifyPrimitive(int num) {
        num = 99; // Only modifies the local copy
    }
}
```

## Part B: Function Inputs and Outputs

### Core Concepts

#### 1. Methods with Parameters (Arguments)
```java
// Method with multiple parameters
public static int add(int a, int b) {
    return a + b;
}

// Method with different parameter types
public static void printInfo(String name, int age, double gpa) {
    System.out.printf("Name: %s, Age: %d, GPA: %.2f%n", name, age, gpa);
}

// Variable arguments (varargs)
public static int sum(int... numbers) {
    int total = 0;
    for (int num : numbers) {
        total += num;
    }
    return total;
}

// Usage examples
public static void main(String[] args) {
    int result = add(5, 3);
    printInfo("John", 20, 3.75);
    int varSum = sum(1, 2, 3, 4, 5); // Can pass any number of arguments
}
```

#### 2. Methods without Parameters
```java
// No parameters
public static String getCurrentTime() {
    return new Date().toString();
}

public static void printWelcome() {
    System.out.println("Welcome to the application!");
}

// Getter methods
private static String applicationName = "MyApp";
public static String getApplicationName() {
    return applicationName;
}
```

#### 3. Methods with Return Values
```java
// Return primitive types
public static int calculateArea(int length, int width) {
    return length * width;
}

public static double calculateCircleArea(double radius) {
    return Math.PI * radius * radius;
}

// Return object types
public static String[] splitName(String fullName) {
    return fullName.split(" ");
}

public static ArrayList<Integer> getEvenNumbers(int limit) {
    ArrayList<Integer> evens = new ArrayList<>();
    for (int i = 0; i <= limit; i += 2) {
        evens.add(i);
    }
    return evens;
}

// Multiple return points
public static String getGrade(int score) {
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    if (score >= 60) return "D";
    return "F";
}
```

#### 4. Methods without Return Values (void)
```java
// Void methods perform actions but don't return values
public static void printArray(int[] array) {
    for (int value : array) {
        System.out.print(value + " ");
    }
    System.out.println();
}

public static void logMessage(String message) {
    System.out.printf("[%s] %s%n", new Date(), message);
}

// Void method with early return
public static void validateAge(int age) {
    if (age < 0) {
        System.out.println("Invalid age");
        return; // Exit method early
    }
    System.out.println("Valid age: " + age);
}
```

#### 5. Default Parameters (Overloading)
```java
// Java doesn't have true default parameters, use method overloading
public static void drawRectangle(int width, int height) {
    drawRectangle(width, height, '*'); // Call overloaded version with default char
}

public static void drawRectangle(int width, int height, char fillChar) {
    for (int i = 0; i < height; i++) {
        for (int j = 0; j < width; j++) {
            System.out.print(fillChar);
        }
        System.out.println();
    }
}

// Alternative: use Optional for truly optional parameters
public static void createUser(String name, Optional<Integer> age) {
    System.out.println("Name: " + name);
    System.out.println("Age: " + age.orElse(18)); // Default age is 18
}
```

### Advanced Input/Output Concepts

#### 1. Generic Methods
```java
// Generic methods work with different types
public static <T> T getMax(T a, T b) {
    if (a instanceof Comparable && b instanceof Comparable) {
        @SuppressWarnings("unchecked")
        Comparable<T> compA = (Comparable<T>) a;
        return compA.compareTo(b) > 0 ? a : b;
    }
    return a; // Default return
}

// Multiple type parameters
public static <T, U> void printPair(T first, U second) {
    System.out.println("First: " + first + ", Second: " + second);
}
```

#### 2. Functional Interfaces and Lambda Expressions
```java
import java.util.function.*;

// Using built-in functional interfaces
public static void processNumbers(List<Integer> numbers, Function<Integer, Integer> processor) {
    for (int i = 0; i < numbers.size(); i++) {
        numbers.set(i, processor.apply(numbers.get(i)));
    }
}

public static void main(String[] args) {
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    
    // Double each number
    processNumbers(numbers, x -> x * 2);
    
    // Square each number
    processNumbers(numbers, x -> x * x);
}
```

## Part C: Functional Decomposition

### Core Concepts

#### 1. Identifying Repetitive Code
```java
// Before decomposition - repetitive code
public static void main(String[] args) {
    // Calculate rectangle area
    int rectLength = 10;
    int rectWidth = 5;
    int rectArea = rectLength * rectWidth;
    System.out.println("Rectangle area: " + rectArea);
    
    // Calculate triangle area
    double base = 8.0;
    double height = 6.0;
    double triangleArea = 0.5 * base * height;
    System.out.println("Triangle area: " + triangleArea);
    
    // Calculate circle area
    double radius = 7.0;
    double circleArea = Math.PI * radius * radius;
    System.out.println("Circle area: " + circleArea);
}

// After decomposition - extracted functions
public static int calculateRectangleArea(int length, int width) {
    return length * width;
}

public static double calculateTriangleArea(double base, double height) {
    return 0.5 * base * height;
}

public static double calculateCircleArea(double radius) {
    return Math.PI * radius * radius;
}

public static void main(String[] args) {
    System.out.println("Rectangle area: " + calculateRectangleArea(10, 5));
    System.out.println("Triangle area: " + calculateTriangleArea(8.0, 6.0));
    System.out.println("Circle area: " + calculateCircleArea(7.0));
}
```

#### 2. Understanding Abstraction
```java
// Low-level details abstracted into methods
public class StudentGradeCalculator {
    
    // Abstract the grade calculation logic
    public static double calculateFinalGrade(double[] assignments, double[] exams, 
                                           double finalExam) {
        double assignmentAvg = calculateAverage(assignments);
        double examAvg = calculateAverage(exams);
        
        // Weighted average: 30% assignments, 40% exams, 30% final
        return assignmentAvg * 0.3 + examAvg * 0.4 + finalExam * 0.3;
    }
    
    private static double calculateAverage(double[] scores) {
        double sum = 0;
        for (double score : scores) {
            sum += score;
        }
        return sum / scores.length;
    }
    
    // Abstract input validation
    public static boolean isValidGrade(double grade) {
        return grade >= 0 && grade <= 100;
    }
    
    // Abstract the conversion logic
    public static String convertToLetterGrade(double grade) {
        if (grade >= 90) return "A";
        if (grade >= 80) return "B";
        if (grade >= 70) return "C";
        if (grade >= 60) return "D";
        return "F";
    }
}
```

#### 3. Function Decomposition Steps
```java
// Example: Processing student records
// Step 1: Identify the main task
public static void processStudentFile(String filename) {
    // Step 2: Break down into subtasks
    List<Student> students = readStudentsFromFile(filename);
    calculateGrades(students);
    generateReport(students);
    saveResults(students, "results.txt");
}

// Step 3: Implement each subtask as a separate function
public static List<Student> readStudentsFromFile(String filename) {
    List<Student> students = new ArrayList<>();
    try (Scanner scanner = new Scanner(new File(filename))) {
        while (scanner.hasNextLine()) {
            students.add(parseStudentRecord(scanner.nextLine()));
        }
    } catch (FileNotFoundException e) {
        System.err.println("File not found: " + filename);
    }
    return students;
}

public static Student parseStudentRecord(String record) {
    String[] parts = record.split(",");
    return new Student(parts[0], Integer.parseInt(parts[1]), 
                      Double.parseDouble(parts[2]));
}

public static void calculateGrades(List<Student> students) {
    for (Student student : students) {
        double grade = calculateIndividualGrade(student);
        student.setFinalGrade(grade);
    }
}

public static double calculateIndividualGrade(Student student) {
    // Detailed grade calculation logic
    return student.getScore(); // Simplified for example
}
```

### Advanced Decomposition Concepts

#### 1. Well-Defined Function Characteristics
```java
// 1. Single Responsibility Principle
// Good: Each function has one clear purpose
public static boolean isPrime(int number) {
    if (number < 2) return false;
    for (int i = 2; i <= Math.sqrt(number); i++) {
        if (number % i == 0) return false;
    }
    return true;
}

// 2. Clear and Descriptive Names
// Good
public static double convertCelsiusToFahrenheit(double celsius) {
    return celsius * 9.0 / 5.0 + 32;
}

// 3. Appropriate Length (generally 20-30 lines max)
// Break large functions into smaller ones
public static void processLargeDataset(List<Integer> data) {
    validateData(data);
    List<Integer> cleanedData = cleanData(data);
    List<Integer> sortedData = sortData(cleanedData);
    generateStatistics(sortedData);
}

// 4. Minimal Dependencies
// Functions should depend on parameters, not global state
public static double calculateTax(double income, double taxRate) {
    return income * taxRate; // Clear dependencies
}

// 5. Predictable Behavior
// Same inputs always produce same outputs
public static int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

#### 2. Top-Down Design Example
```java
// Main problem: Create a simple banking system
public class BankingSystem {
    
    // Level 1: High-level operations
    public static void runBankingSystem() {
        Account account = createAccount();
        while (true) {
            int choice = displayMenu();
            processChoice(account, choice);
        }
    }
    
    // Level 2: Medium-level operations
    public static Account createAccount() {
        String name = getUserInput("Enter account holder name: ");
        double initialBalance = getDoubleInput("Enter initial balance: ");
        return new Account(name, initialBalance);
    }
    
    public static int displayMenu() {
        System.out.println("\n1. Check Balance");
        System.out.println("2. Deposit");
        System.out.println("3. Withdraw");
        System.out.println("4. Exit");
        return getIntegerInput("Choose an option: ");
    }
    
    public static void processChoice(Account account, int choice) {
        switch (choice) {
            case 1: showBalance(account); break;
            case 2: handleDeposit(account); break;
            case 3: handleWithdrawal(account); break;
            case 4: System.exit(0); break;
            default: System.out.println("Invalid choice");
        }
    }
    
    // Level 3: Low-level operations
    public static void showBalance(Account account) {
        System.out.printf("Current balance: $%.2f%n", account.getBalance());
    }
    
    public static void handleDeposit(Account account) {
        double amount = getDoubleInput("Enter deposit amount: ");
        if (isValidAmount(amount)) {
            account.deposit(amount);
            System.out.println("Deposit successful");
        }
    }
    
    // Level 4: Utility functions
    public static boolean isValidAmount(double amount) {
        return amount > 0;
    }
    
    public static String getUserInput(String prompt) {
        System.out.print(prompt);
        return new Scanner(System.in).nextLine();
    }
}
```

#### 3. Function Composition
```java
// Combining simple functions to create complex behavior
public class FunctionComposition {
    
    // Basic functions
    public static int square(int x) { return x * x; }
    public static int cube(int x) { return x * x * x; }
    public static boolean isEven(int x) { return x % 2 == 0; }
    
    // Composed function
    public static List<Integer> getEvenCubes(List<Integer> numbers) {
        return numbers.stream()
                .filter(FunctionComposition::isEven)
                .map(FunctionComposition::cube)
                .collect(Collectors.toList());
    }
    
    // Higher-order function
    public static <T> List<T> filterAndTransform(List<T> list, 
                                                Predicate<T> filter,
                                                Function<T, T> transformer) {
        return list.stream()
                .filter(filter)
                .map(transformer)
                .collect(Collectors.toList());
    }
}
```

## Key Exam Tips

1. **Understand scope levels**: method, block, class, and package
2. **Know the difference** between parameters and arguments
3. **Practice method overloading** for "default parameters"
4. **Identify opportunities** for functional decomposition
5. **Write functions** with single responsibilities
6. **Use descriptive names** for methods and parameters
7. **Understand when** to use return vs void methods

## Common Pitfalls

1. **Scope confusion** - trying to access variables outside their scope
2. **Variable shadowing** - local variables hiding instance/class variables
3. **Not returning values** when methods are declared to return something
4. **Modifying parameters** and expecting changes in calling method (primitives)
5. **Creating overly complex methods** that do multiple things
6. **Poor naming** that doesn't indicate what the method does
7. **Not handling** all execution paths in methods with return values

## Design Principles

### 1. SOLID Principles in Function Design
```java
// Single Responsibility Principle
// Each method should have one reason to change
public static boolean validateEmail(String email) {
    return email.contains("@") && email.contains(".");
}

public static void sendEmail(String email, String message) {
    // Send email logic
}

// Open/Closed Principle
// Methods should be open for extension, closed for modification
public interface DiscountCalculator {
    double calculate(double amount);
}

public static double applyDiscount(double amount, DiscountCalculator calculator) {
    return calculator.calculate(amount);
}
```

### 2. Pure Functions
```java
// Pure functions: same input always produces same output, no side effects
public static int add(int a, int b) {
    return a + b; // No side effects, deterministic
}

// Impure function: modifies external state
private static int counter = 0;
public static int incrementAndGet() {
    return ++counter; // Has side effect, not deterministic
}
```

## Practice Exercises

1. Refactor a large method with multiple responsibilities into smaller functions
2. Create a calculator class with methods for basic operations (+, -, *, /)
3. Implement a text processing system using functional decomposition
4. Write a recursive function and convert it to iterative
5. Create a utility class with pure functions for common operations
6. Design a library management system using top-down design
7. Implement function composition to process data through multiple transformations
8. Create a game with well-decomposed functions for different game logic
9. Write overloaded methods to handle different parameter combinations
10. Design a modular program where each function can be tested independently