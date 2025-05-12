# Standard 4: Object-Oriented Programming - Study Guide

## Overview
Standard 4 covers object-oriented programming techniques in Java, including using built-in classes and creating user-defined classes. This section represents **15% of the exam**.

## Part A: Using Built-in Classes

### Core Concepts

#### 1. Instantiating Objects
```java
// Creating objects from built-in classes
String text = new String("Hello World");
ArrayList<Integer> numbers = new ArrayList<>();
Scanner scanner = new Scanner(System.in);
File file = new File("data.txt");
Date currentDate = new Date();
Random random = new Random();

// Different constructor options
String str1 = "Hello";                    // String literal (preferred)
String str2 = new String("Hello");        // Using constructor
ArrayList<String> list1 = new ArrayList<>();           // Default constructor
ArrayList<String> list2 = new ArrayList<>(20);         // Initial capacity
ArrayList<String> list3 = new ArrayList<>(Arrays.asList("a", "b", "c")); // From collection
```

#### 2. Using Object Data Members
```java
// Array length property
int[] array = {1, 2, 3, 4, 5};
int size = array.length;  // Note: length is a property, not a method

// String length method (contrast with array)
String text = "Hello";
int textLength = text.length();  // Note: length() is a method for String

// ArrayList size
ArrayList<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
int listSize = list.size();

// File properties
File file = new File("document.txt");
long fileSize = file.length();  // Size in bytes
boolean exists = file.exists();
boolean isFile = file.isFile();
boolean isDirectory = file.isDirectory();
```

#### 3. Using Object Member Functions (Methods)
```java
// String methods
String text = "Hello World";
String upper = text.toUpperCase();
String lower = text.toLowerCase();
boolean contains = text.contains("World");
String replaced = text.replace("World", "Java");
String[] parts = text.split(" ");
String trimmed = text.trim();

// ArrayList methods
ArrayList<String> fruits = new ArrayList<>();
fruits.add("Apple");
fruits.add("Banana");
fruits.add(1, "Orange");  // Insert at index
boolean removed = fruits.remove("Apple");
String first = fruits.get(0);
fruits.set(0, "Grape");
boolean isEmpty = fruits.isEmpty();
fruits.clear();

// Scanner methods
Scanner input = new Scanner(System.in);
int number = input.nextInt();
double decimal = input.nextDouble();
String line = input.nextLine();
boolean hasNext = input.hasNext();

// Random methods
Random rand = new Random();
int randomInt = rand.nextInt(100);  // 0-99
double randomDouble = rand.nextDouble();  // 0.0-1.0
boolean randomBoolean = rand.nextBoolean();
```

### Advanced Built-in Class Usage

#### 1. Working with Dates and Times
```java
import java.time.*;
import java.time.format.DateTimeFormatter;

// Modern date/time API (Java 8+)
LocalDate date = LocalDate.now();
LocalTime time = LocalTime.now();
LocalDateTime dateTime = LocalDateTime.now();

// Formatting
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String formatted = dateTime.format(formatter);

// Date calculations
LocalDate tomorrow = date.plusDays(1);
LocalDate lastWeek = date.minusWeeks(1);
long daysBetween = ChronoUnit.DAYS.between(date, tomorrow);
```

#### 2. Working with Collections Framework
```java
// Different collection types
Set<String> uniqueItems = new HashSet<>();
Map<String, Integer> wordCount = new HashMap<>();
Queue<String> tasks = new LinkedList<>();
Stack<Integer> numbers = new Stack<>();

// Map operations
wordCount.put("apple", 5);
wordCount.put("banana", 3);
int count = wordCount.get("apple");
boolean containsKey = wordCount.containsKey("cherry");
Set<String> keys = wordCount.keySet();
Collection<Integer> values = wordCount.values();

// Set operations
uniqueItems.add("item1");
uniqueItems.add("item2");
uniqueItems.add("item1");  // Duplicate, won't be added
boolean contains = uniqueItems.contains("item1");
```

#### 3. File and Path Operations
```java
import java.nio.file.*;

// Modern file operations (Java 7+)
Path path = Paths.get("document.txt");
Path absolutePath = path.toAbsolutePath();
Path parent = path.getParent();
String fileName = path.getFileName().toString();

// Reading/writing files
try {
    String content = Files.readString(path);
    Files.writeString(path, "New content");
    
    List<String> lines = Files.readAllLines(path);
    Files.write(path, Arrays.asList("Line 1", "Line 2"));
} catch (IOException e) {
    e.printStackTrace();
}
```

## Part B: Creating User-Defined Classes

### Core Concepts

#### 1. Basic Class Structure
```java
public class Student {
    // Instance variables (data members)
    private String name;
    private int age;
    private double gpa;
    
    // Constructor
    public Student(String name, int age, double gpa) {
        this.name = name;
        this.age = age;
        this.gpa = gpa;
    }
    
    // Methods (member functions)
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public boolean isHonorRoll() {
        return gpa >= 3.5;
    }
}
```

#### 2. Instance Variables (Data Members)
```java
public class BankAccount {
    // Private instance variables (encapsulation)
    private String accountNumber;
    private double balance;
    private String ownerName;
    private static int nextAccountId = 1000;  // Static variable
    
    // Different access modifiers
    public String accountType;      // Public (avoid this for data)
    protected double interestRate;  // Protected (package + subclasses)
    private Date creationDate;      // Private (encapsulated)
    
    // Final instance variable (constant for each instance)
    private final String BANK_CODE = "ABC";
}
```

#### 3. Constructors
```java
public class Rectangle {
    private double width;
    private double height;
    
    // Default constructor
    public Rectangle() {
        this.width = 1.0;
        this.height = 1.0;
    }
    
    // Parameterized constructor
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    // Constructor with single parameter (square)
    public Rectangle(double size) {
        this(size, size);  // Constructor chaining
    }
    
    // Copy constructor
    public Rectangle(Rectangle other) {
        this.width = other.width;
        this.height = other.height;
    }
}
```

#### 4. Methods (Member Functions)
```java
public class Calculator {
    private double lastResult;
    
    // Instance methods
    public double add(double a, double b) {
        lastResult = a + b;
        return lastResult;
    }
    
    public double getLastResult() {
        return lastResult;
    }
    
    // Method with validation
    public double divide(double a, double b) {
        if (b == 0) {
            throw new IllegalArgumentException("Cannot divide by zero");
        }
        lastResult = a / b;
        return lastResult;
    }
    
    // Static method (belongs to class, not instance)
    public static double power(double base, double exponent) {
        return Math.pow(base, exponent);
    }
    
    // Method overloading
    public double multiply(double a, double b) {
        return a * b;
    }
    
    public double multiply(double a, double b, double c) {
        return a * b * c;
    }
}
```

### Advanced Class Design Concepts

#### 1. Encapsulation and Access Control
```java
public class Person {
    // Private fields for encapsulation
    private String name;
    private int age;
    private String ssn;
    
    // Getter methods
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    // Setter methods with validation
    public void setName(String name) {
        if (name != null && !name.trim().isEmpty()) {
            this.name = name;
        } else {
            throw new IllegalArgumentException("Name cannot be null or empty");
        }
    }
    
    public void setAge(int age) {
        if (age >= 0 && age <= 150) {
            this.age = age;
        } else {
            throw new IllegalArgumentException("Age must be between 0 and 150");
        }
    }
    
    // Read-only property (no setter)
    public String getSSN() {
        return ssn;  // No setter provided for security
    }
}
```

#### 2. Static vs Instance Members
```java
public class Counter {
    // Static variable (class-level)
    private static int totalCount = 0;
    
    // Instance variable
    private int instanceCount;
    private String name;
    
    public Counter(String name) {
        this.name = name;
        this.instanceCount = 0;
        totalCount++;  // Increment class-level counter
    }
    
    // Instance method
    public void increment() {
        instanceCount++;
        totalCount++;
    }
    
    // Instance getter
    public int getInstanceCount() {
        return instanceCount;
    }
    
    // Static method (can't access instance variables)
    public static int getTotalCount() {
        return totalCount;
    }
    
    // Static method with parameter
    public static void resetTotalCount() {
        totalCount = 0;
    }
}

// Usage
Counter c1 = new Counter("Counter1");
Counter c2 = new Counter("Counter2");
c1.increment();
c2.increment();
c2.increment();

System.out.println(c1.getInstanceCount()); // 1
System.out.println(c2.getInstanceCount()); // 2
System.out.println(Counter.getTotalCount()); // 5 (2 from construction + 3 from increment)
```

#### 3. Object Relationships
```java
// Composition: Car HAS-A Engine
public class Engine {
    private int horsepower;
    private String type;
    
    public Engine(int horsepower, String type) {
        this.horsepower = horsepower;
        this.type = type;
    }
    
    public void start() {
        System.out.println("Engine starting...");
    }
}

public class Car {
    private Engine engine;  // Composition
    private String make;
    private String model;
    
    public Car(String make, String model, int horsepower, String engineType) {
        this.make = make;
        this.model = model;
        this.engine = new Engine(horsepower, engineType);  // Creating the engine
    }
    
    public void start() {
        engine.start();  // Delegating to the engine
        System.out.println(make + " " + model + " is starting");
    }
}

// Aggregation: University HAS-A Student (student can exist without university)
public class University {
    private List<Student> students;
    private String name;
    
    public University(String name) {
        this.name = name;
        this.students = new ArrayList<>();
    }
    
    public void enrollStudent(Student student) {
        students.add(student);
    }
    
    public void removeStudent(Student student) {
        students.remove(student);
    }
}
```

#### 4. Object Lifecycle and Memory Management
```java
public class ResourceManager {
    private List<String> resources;
    
    public ResourceManager() {
        resources = new ArrayList<>();
        System.out.println("ResourceManager created");
    }
    
    public void addResource(String resource) {
        resources.add(resource);
    }
    
    // Cleanup method (not automatic like destructor in C++)
    public void cleanup() {
        resources.clear();
        System.out.println("Resources cleaned up");
    }
    
    // finalize() method called by garbage collector (deprecated in Java 9+)
    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("ResourceManager being garbage collected");
    }
    
    // Better approach: try-with-resources (implements AutoCloseable)
    public static class ManagedResource implements AutoCloseable {
        private String name;
        
        public ManagedResource(String name) {
            this.name = name;
            System.out.println("Opening resource: " + name);
        }
        
        @Override
        public void close() {
            System.out.println("Closing resource: " + name);
        }
    }
}

// Usage with automatic cleanup
try (ResourceManager.ManagedResource resource = 
     new ResourceManager.ManagedResource("Database Connection")) {
    // Use resource
} // Automatically closed here
```

### Common Design Patterns

#### 1. Builder Pattern
```java
public class Product {
    private String name;
    private double price;
    private String category;
    private String description;
    
    private Product() {} // Private constructor
    
    // Static nested Builder class
    public static class Builder {
        private Product product = new Product();
        
        public Builder setName(String name) {
            product.name = name;
            return this;
        }
        
        public Builder setPrice(double price) {
            product.price = price;
            return this;
        }
        
        public Builder setCategory(String category) {
            product.category = category;
            return this;
        }
        
        public Builder setDescription(String description) {
            product.description = description;
            return this;
        }
        
        public Product build() {
            return product;
        }
    }
}

// Usage
Product product = new Product.Builder()
    .setName("Laptop")
    .setPrice(999.99)
    .setCategory("Electronics")
    .setDescription("High-performance laptop")
    .build();
```

#### 2. Singleton Pattern
```java
public class DatabaseConnection {
    private static DatabaseConnection instance;
    private String connectionString;
    
    // Private constructor
    private DatabaseConnection() {
        connectionString = "jdbc:mysql://localhost:3306/mydb";
    }
    
    // Thread-safe singleton
    public static synchronized DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }
    
    // Alternative: eager initialization
    private static final DatabaseConnection INSTANCE = new DatabaseConnection();
    
    public static DatabaseConnection getInstanceEager() {
        return INSTANCE;
    }
}
```

## Key Exam Tips

1. **Know the difference** between built-in and user-defined classes
2. **Understand encapsulation** - use private fields with public getters/setters
3. **Master constructors** - default, parameterized, and copy constructors
4. **Practice method overloading** within classes
5. **Understand static vs instance** members
6. **Know common built-in classes** - String, ArrayList, Scanner, File, Date
7. **Practice object instantiation** and method chaining

## Common Pitfalls

1. **Public instance variables** - breaks encapsulation
2. **Not providing constructors** - relying only on default constructor
3. **Confusing static and instance** members
4. **Not validating input** in setter methods
5. **Memory leaks** with static collections
6. **Comparing objects with ==** instead of equals()
7. **Not overriding toString()** for better debugging

## Best Practices

1. **Always use encapsulation** - private fields, public methods
2. **Validate input** in constructors and setters
3. **Provide meaningful toString()** methods
4. **Follow naming conventions** - classes start with uppercase
5. **Keep classes focused** - single responsibility
6. **Use composition over inheritance** when possible
7. **Make classes immutable** when appropriate

## Practice Exercises

1. Create a `Book` class with appropriate fields and methods
2. Implement a `BankAccount` class with deposit/withdrawal methods
3. Design a `Vehicle` class hierarchy with different vehicle types
4. Create a `Library` class that manages a collection of books
5. Implement a `Grade` class with letter grade conversion
6. Design a `Point` class for 2D coordinates with distance calculation
7. Create a `ShoppingCart` class that holds multiple `Item` objects
8. Implement a `Timer` class that tracks elapsed time
9. Design a `Student` class with course enrollment capabilities
10. Create a `Matrix` class for basic mathematical operations

## Object-Oriented Design Example

```java
// Complete example: Simple Library Management System
public class Book {
    private String isbn;
    private String title;
    private String author;
    private boolean isCheckedOut;
    
    public Book(String isbn, String title, String author) {
        this.isbn = isbn;
        this.title = title;
        this.author = author;
        this.isCheckedOut = false;
    }
    
    // Getters
    public String getIsbn() { return isbn; }
    public String getTitle() { return title; }
    public String getAuthor() { return author; }
    public boolean isCheckedOut() { return isCheckedOut; }
    
    // Book-specific methods
    public boolean checkOut() {
        if (!isCheckedOut) {
            isCheckedOut = true;
            return true;
        }
        return false;
    }
    
    public boolean checkIn() {
        if (isCheckedOut) {
            isCheckedOut = false;
            return true;
        }
        return false;
    }
    
    @Override
    public String toString() {
        return String.format("Book{isbn='%s', title='%s', author='%s', checkedOut=%s}", 
                           isbn, title, author, isCheckedOut);
    }
}

public class Library {
    private ArrayList<Book> books;
    private String name;
    
    public Library(String name) {
        this.name = name;
        this.books = new ArrayList<>();
    }
    
    public void addBook(Book book) {
        books.add(book);
    }
    
    public Book findBookByIsbn(String isbn) {
        for (Book book : books) {
            if (book.getIsbn().equals(isbn)) {
                return book;
            }
        }
        return null;
    }
    
    public List<Book> searchByAuthor(String author) {
        List<Book> result = new ArrayList<>();
        for (Book book : books) {
            if (book.getAuthor().contains(author)) {
                result.add(book);
            }
        }
        return result;
    }
    
    public boolean checkOutBook(String isbn) {
        Book book = findBookByIsbn(isbn);
        return book != null && book.checkOut();
    }
    
    public boolean checkInBook(String isbn) {
        Book book = findBookByIsbn(isbn);
        return book != null && book.checkIn();
    }
    
    public void printAllBooks() {
        System.out.println("Books in " + name + ":");
        for (Book book : books) {
            System.out.println(book);
        }
    }
}

// Usage
Library library = new Library("City Library");
library.addBook(new Book("978-1234567890", "Java Programming", "John Doe"));
library.addBook(new Book("978-0987654321", "Data Structures", "Jane Smith"));

library.checkOutBook("978-1234567890");
library.printAllBooks();
```