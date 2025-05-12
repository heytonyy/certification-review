# Standard 5: Code Comprehension and Debugging - Study Guide

## Overview
Standard 5 covers code comprehension and debugging techniques including tracing, debugging tools, and testing strategies. This section represents **14% of the exam**.

## Part A: Code Tracing

### Core Concepts

#### 1. Tracing Loop Execution
```java
// Example 1: Simple for loop tracing
public static void traceForLoop() {
    int sum = 0;
    System.out.println("Initial: sum = " + sum);
    
    for (int i = 1; i <= 3; i++) {
        sum += i;
        System.out.printf("Iteration %d: i = %d, sum = %d%n", i, i, sum);
    }
    
    System.out.println("Final: sum = " + sum);
}
/* Output:
Initial: sum = 0
Iteration 1: i = 1, sum = 1
Iteration 2: i = 2, sum = 3
Iteration 3: i = 3, sum = 6
Final: sum = 6
*/

// Example 2: Nested loops tracing
public static void traceNestedLoops() {
    for (int i = 0; i < 2; i++) {
        System.out.println("Outer loop: i = " + i);
        for (int j = 0; j < 3; j++) {
            System.out.printf("  Inner loop: j = %d, i*j = %d%n", j, i*j);
        }
    }
}
/*
Trace Through Execution:
i=0, j=0: Output "Outer loop: i = 0", "  Inner loop: j = 0, i*j = 0"
i=0, j=1: Output "  Inner loop: j = 1, i*j = 0"
i=0, j=2: Output "  Inner loop: j = 2, i*j = 0"
i=1, j=0: Output "Outer loop: i = 1", "  Inner loop: j = 0, i*j = 0"
i=1, j=1: Output "  Inner loop: j = 1, i*j = 1"
i=1, j=2: Output "  Inner loop: j = 2, i*j = 2"
*/
```

#### 2. Tracing Function Calls and Returns
```java
// Example: Function call tracing
public static void main(String[] args) {
    System.out.println("1. Starting main");
    int result = factorial(3);
    System.out.println("6. Back in main, result = " + result);
}

public static int factorial(int n) {
    System.out.println("2. Entering factorial with n = " + n);
    if (n <= 1) {
        System.out.println("3. Base case reached, returning 1");
        return 1;
    }
    int result = n * factorial(n - 1);
    System.out.println("5. Returning from factorial(" + n + ") = " + result);
    return result;
}

/* Execution Trace:
Call Stack Visualization:
main() calls factorial(3)
├── factorial(3) calls factorial(2)
│   ├── factorial(2) calls factorial(1)
│   │   └── factorial(1) returns 1
│   └── factorial(2) returns 2*1 = 2
└── factorial(3) returns 3*2 = 6
*/
```

#### 3. Tracing Variable Scope Changes
```java
public class ScopeTracing {
    private static int globalVar = 10;
    
    public static void main(String[] args) {
        System.out.println("Main: globalVar = " + globalVar);  // 10
        int localVar = 5;
        System.out.println("Main: localVar = " + localVar);    // 5
        
        methodA(localVar);
        System.out.println("Main after methodA: localVar = " + localVar);  // Still 5
        System.out.println("Main after methodA: globalVar = " + globalVar); // 30
    }
    
    public static void methodA(int param) {
        System.out.println("MethodA entry: param = " + param);     // 5
        System.out.println("MethodA entry: globalVar = " + globalVar); // 10
        
        param = 20;  // Only changes local copy
        globalVar = 30;  // Changes global variable
        
        System.out.println("MethodA exit: param = " + param);       // 20
        System.out.println("MethodA exit: globalVar = " + globalVar); // 30
    }
}
```

#### 4. Tracing Object References
```java
public static void traceObjectReferences() {
    // Creating objects
    ArrayList<Integer> list1 = new ArrayList<>();
    list1.add(1);
    list1.add(2);
    System.out.println("list1 initial: " + list1);  // [1, 2]
    
    // Reference assignment
    ArrayList<Integer> list2 = list1;  // Both point to same object
    list2.add(3);
    System.out.println("list1 after list2.add(3): " + list1);  // [1, 2, 3]
    System.out.println("list2: " + list2);  // [1, 2, 3]
    
    // Creating new object
    list2 = new ArrayList<>(list1);  // list2 now points to new object
    list2.add(4);
    System.out.println("list1 after new list2: " + list1);  // [1, 2, 3]
    System.out.println("list2 after add(4): " + list2);     // [1, 2, 3, 4]
}
```

### Advanced Tracing Techniques

#### 1. Tracing Complex Algorithms
```java
// Example: Binary search tracing
public static int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    int iteration = 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        System.out.printf("Iteration %d: left=%d, right=%d, mid=%d, arr[mid]=%d%n", 
                         iteration, left, right, mid, arr[mid]);
        
        if (arr[mid] == target) {
            System.out.println("Found at index " + mid);
            return mid;
        }
        
        if (arr[mid] < target) {
            left = mid + 1;
            System.out.println("  Searching right half");
        } else {
            right = mid - 1;
            System.out.println("  Searching left half");
        }
        iteration++;
    }
    
    System.out.println("Not found");
    return -1;
}

// Trace for binarySearch([1,3,5,7,9,11,13], 7):
/*
Iteration 1: left=0, right=6, mid=3, arr[mid]=7
Found at index 3
*/

// Trace for binarySearch([1,3,5,7,9,11,13], 5):
/*
Iteration 1: left=0, right=6, mid=3, arr[mid]=7
  Searching left half
Iteration 2: left=0, right=2, mid=1, arr[mid]=3
  Searching right half
Iteration 3: left=2, right=2, mid=2, arr[mid]=5
Found at index 2
*/
```

#### 2. Tracing State Changes in Objects
```java
public class Counter {
    private int count;
    private String name;
    
    public Counter(String name) {
        this.name = name;
        this.count = 0;
        System.out.println("Created counter '" + name + "' with count = " + count);
    }
    
    public void increment() {
        System.out.println(name + ": Incrementing from " + count + " to " + (count + 1));
        count++;
    }
    
    public void reset() {
        System.out.println(name + ": Resetting from " + count + " to 0");
        count = 0;
    }
    
    public int getCount() {
        System.out.println(name + ": Current count is " + count);
        return count;
    }
}

// Tracing object state changes
public static void traceObjectState() {
    Counter c1 = new Counter("Counter1");
    Counter c2 = new Counter("Counter2");
    
    c1.increment();
    c2.increment();
    c2.increment();
    
    int sum = c1.getCount() + c2.getCount();
    System.out.println("Sum: " + sum);
    
    c1.reset();
    c2.getCount();
}
```

## Part B: Debugging with IDE Tools

### Core Concepts

#### 1. Setting Breakpoints
```java
public class DebuggingExample {
    public static void main(String[] args) {
        int[] numbers = {1, 5, 3, 9, 2};
        
        // Set breakpoint here to examine initial state
        int max = findMax(numbers);
        
        // Set breakpoint here to examine result
        System.out.println("Maximum: " + max);
    }
    
    public static int findMax(int[] arr) {
        // Set breakpoint here to trace through function
        int max = arr[0];
        
        for (int i = 1; i < arr.length; i++) {
            // Set conditional breakpoint: i == 2
            if (arr[i] > max) {
                max = arr[i];  // Set breakpoint to watch max change
            }
        }
        
        return max;
    }
}

/*
Debugging Steps:
1. Set breakpoint at line with findMax call
2. Run in debug mode
3. Examine variables: numbers array contents
4. Step into findMax function
5. Set breakpoint inside for loop
6. Step through each iteration
7. Watch max variable change
8. Step out to return to main
9. Examine final result
*/
```

#### 2. Using Watch Expressions
```java
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        boolean swapped;
        
        // Add watch: arr.toString()
        // Add watch: swapped
        // Add watch: i
        // Add watch: j
        
        for (int i = 0; i < n - 1; i++) {
            swapped = false;
            
            for (int j = 0; j < n - i - 1; j++) {
                // Add conditional breakpoint: arr[j] > arr[j + 1]
                if (arr[j] > arr[j + 1]) {
                    // Watch variables before swap
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                    // Watch variables after swap
                }
            }
            
            // Add breakpoint with condition: !swapped
            if (!swapped) {
                break;  // Array is already sorted
            }
        }
    }
}
```

#### 3. Call Stack Analysis
```java
public class RecursionDebugging {
    public static void main(String[] args) {
        System.out.println("Fibonacci(5) = " + fibonacci(5));
    }
    
    public static int fibonacci(int n) {
        // Set breakpoint here to see call stack
        if (n <= 1) {
            return n;
        }
        
        // Examine call stack when stepping into these calls
        int fib1 = fibonacci(n - 1);
        int fib2 = fibonacci(n - 2);
        
        return fib1 + fib2;
    }
}

/*
Call Stack at fibonacci(5):
fibonacci(5)
├── fibonacci(4)
│   ├── fibonacci(3)
│   │   ├── fibonacci(2)
│   │   │   ├── fibonacci(1) → returns 1
│   │   │   └── fibonacci(0) → returns 0
│   │   ├── fibonacci(1) → returns 1
│   │   └── returns 2
│   ├── fibonacci(2)
│   │   ├── fibonacci(1) → returns 1
│   │   └── fibonacci(0) → returns 0
│   └── returns 3
├── fibonacci(3)
│   ├── fibonacci(2)
│   │   ├── fibonacci(1) → returns 1
│   │   └── fibonacci(0) → returns 0
│   ├── fibonacci(1) → returns 1
│   └── returns 2
└── returns 5
*/
```

### Advanced Debugging Techniques

#### 1. Debugging with Assertions
```java
public class AssertionDebugging {
    public static double calculateAverage(int[] scores) {
        // Enable assertions with -ea flag
        assert scores != null : "Scores array cannot be null";
        assert scores.length > 0 : "Scores array cannot be empty";
        
        int sum = 0;
        for (int score : scores) {
            assert score >= 0 && score <= 100 : "Score must be between 0 and 100: " + score;
            sum += score;
        }
        
        double average = (double) sum / scores.length;
        assert average >= 0 && average <= 100 : "Average should be between 0 and 100: " + average;
        
        return average;
    }
    
    public static void main(String[] args) {
        int[] scores = {85, 90, 78, 92, 88};
        double avg = calculateAverage(scores);
        System.out.println("Average: " + avg);
        
        // This will trigger assertion error if assertions are enabled
        // int[] invalidScores = {85, 90, 110, 92, 88};
        // calculateAverage(invalidScores);
    }
}
```

#### 2. Debugging Multi-threaded Code
```java
public class ThreadDebugging {
    private static int counter = 0;
    private static final Object lock = new Object();
    
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                // Set breakpoint here to examine race condition
                synchronized (lock) {
                    counter++;  // Watch counter value in different threads
                }
            }
        });
        
        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                // Set breakpoint here too
                synchronized (lock) {
                    counter--;  // Watch counter value in different threads
                }
            }
        });
        
        t1.start();
        t2.start();
        
        t1.join();
        t2.join();
        
        System.out.println("Final counter: " + counter);
    }
}
```

## Part C: Testing and Validation

### Core Concepts

#### 1. Boundary Condition Testing
```java
public class MathUtils {
    public static double divide(double a, double b) {
        if (b == 0) {
            throw new IllegalArgumentException("Cannot divide by zero");
        }
        return a / b;
    }
    
    public static int factorial(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("Factorial undefined for negative numbers");
        }
        if (n == 0 || n == 1) {
            return 1;
        }
        return n * factorial(n - 1);
    }
}

// Test boundary conditions
public class TestMathUtils {
    public static void testBoundaryConditions() {
        // Test division
        System.out.println("Testing division boundary conditions:");
        
        // Normal case
        assert divide(10, 2) == 5.0 : "Normal division failed";
        
        // Boundary: division by very small number
        assert divide(1, 0.00001) == 100000.0 : "Division by small number failed";
        
        // Error case: division by zero
        try {
            divide(10, 0);
            assert false : "Should have thrown exception for division by zero";
        } catch (IllegalArgumentException e) {
            System.out.println("Correctly caught division by zero");
        }
        
        // Test factorial
        System.out.println("Testing factorial boundary conditions:");
        
        // Boundary cases
        assert factorial(0) == 1 : "factorial(0) should be 1";
        assert factorial(1) == 1 : "factorial(1) should be 1";
        assert factorial(2) == 2 : "factorial(2) should be 2";
        
        // Normal case
        assert factorial(5) == 120 : "factorial(5) should be 120";
        
        // Error case: negative input
        try {
            factorial(-1);
            assert false : "Should have thrown exception for negative input";
        } catch (IllegalArgumentException e) {
            System.out.println("Correctly caught negative factorial");
        }
    }
}
```

#### 2. Invalid Input Testing
```java
public class InputValidator {
    public static boolean isValidEmail(String email) {
        if (email == null || email.isEmpty()) {
            return false;
        }
        return email.contains("@") && email.contains(".");
    }
    
    public static int parseAge(String ageStr) {
        if (ageStr == null || ageStr.trim().isEmpty()) {
            throw new IllegalArgumentException("Age cannot be empty");
        }
        
        int age = Integer.parseInt(ageStr);
        if (age < 0 || age > 150) {
            throw new IllegalArgumentException("Age must be between 0 and 150");
        }
        return age;
    }
}

// Test invalid inputs
public class TestInputValidator {
    public static void testInvalidInputs() {
        // Test email validation
        System.out.println("Testing email validation:");
        
        // Valid emails
        assert isValidEmail("test@example.com") : "Valid email failed";
        assert isValidEmail("user.name@domain.co.uk") : "Valid email with subdomain failed";
        
        // Invalid emails
        assert !isValidEmail(null) : "Null email should be invalid";
        assert !isValidEmail("") : "Empty email should be invalid";
        assert !isValidEmail("notanemail") : "Email without @ should be invalid";
        assert !isValidEmail("missing@dotcom") : "Email without . should be invalid";
        
        // Test age parsing
        System.out.println("Testing age parsing:");
        
        // Valid ages
        assert parseAge("25") == 25 : "Valid age parsing failed";
        assert parseAge("0") == 0 : "Boundary age 0 failed";
        assert parseAge("150") == 150 : "Boundary age 150 failed";
        
        // Invalid ages - string inputs
        String[] invalidAgeStrings = {null, "", "  ", "abc", "-5", "200"};
        for (String ageStr : invalidAgeStrings) {
            try {
                parseAge(ageStr);
                assert false : "Should have thrown exception for: " + ageStr;
            } catch (Exception e) {
                System.out.println("Correctly caught error for age: " + ageStr);
            }
        }
    }
}
```

#### 3. Robustness Testing
```java
public class FileProcessor {
    public static String readFile(String filename) {
        if (filename == null || filename.trim().isEmpty()) {
            throw new IllegalArgumentException("Filename cannot be null or empty");
        }
        
        try (Scanner scanner = new Scanner(new File(filename))) {
            StringBuilder content = new StringBuilder();
            while (scanner.hasNextLine()) {
                content.append(scanner.nextLine()).append("\n");
            }
            return content.toString();
        } catch (FileNotFoundException e) {
            throw new RuntimeException("File not found: " + filename, e);
        }
    }
    
    public static List<Integer> parseNumbers(String data) {
        List<Integer> numbers = new ArrayList<>();
        if (data == null || data.trim().isEmpty()) {
            return numbers;  // Return empty list for empty input
        }
        
        String[] parts = data.split(",");
        for (String part : parts) {
            try {
                numbers.add(Integer.parseInt(part.trim()));
            } catch (NumberFormatException e) {
                System.err.println("Warning: Skipping invalid number: " + part);
                // Continue processing instead of failing completely
            }
        }
        return numbers;
    }
}

// Robustness testing
public class TestRobustness {
    public static void testRobustness() {
        // Test edge cases that shouldn't crash the program
        
        // Empty data handling
        List<Integer> result = parseNumbers("");
        assert result.isEmpty() : "Empty input should return empty list";
        
        // Mixed valid/invalid data
        result = parseNumbers("1,2,abc,4,xyz,6");
        assert result.equals(Arrays.asList(1, 2, 4, 6)) : "Should parse valid numbers and skip invalid";
        
        // All invalid data
        result = parseNumbers("abc,def,ghi");
        assert result.isEmpty() : "All invalid data should return empty list";
        
        // Test file reading with various scenarios
        try {
            readFile("nonexistent.txt");
            assert false : "Should throw exception for non-existent file";
        } catch (RuntimeException e) {
            System.out.println("Correctly handled non-existent file");
        }
        
        try {
            readFile("");
            assert false : "Should throw exception for empty filename";
        } catch (IllegalArgumentException e) {
            System.out.println("Correctly handled empty filename");
        }
    }
}
```

#### 4. Performance Testing
```java
public class PerformanceTesting {
    public static long timeOperation(Runnable operation) {
        long startTime = System.nanoTime();
        operation.run();
        long endTime = System.nanoTime();
        return endTime - startTime;
    }
    
    public static void compareSearchAlgorithms() {
        int[] largeArray = new int[100000];
        for (int i = 0; i < largeArray.length; i++) {
            largeArray[i] = i;
        }
        
        int target = 75000;
        
        // Test linear search
        long linearTime = timeOperation(() -> {
            for (int i = 0; i < largeArray.length; i++) {
                if (largeArray[i] == target) break;
            }
        });
        
        // Test binary search (array must be sorted)
        long binaryTime = timeOperation(() -> {
            Arrays.binarySearch(largeArray, target);
        });
        
        System.out.printf("Linear search time: %d nanoseconds%n", linearTime);
        System.out.printf("Binary search time: %d nanoseconds%n", binaryTime);
        System.out.printf("Binary search is %.2fx faster%n", (double)linearTime / binaryTime);
    }
    
    public static void testMemoryUsage() {
        Runtime runtime = Runtime.getRuntime();
        
        // Measure memory before
        long memoryBefore = runtime.totalMemory() - runtime.freeMemory();
        
        // Create large data structure
        List<String> largeList = new ArrayList<>();
        for (int i = 0; i < 100000; i++) {
            largeList.add("String " + i);
        }
        
        // Measure memory after
        long memoryAfter = runtime.totalMemory() - runtime.freeMemory();
        
        System.out.printf("Memory used: %d bytes%n", memoryAfter - memoryBefore);
        
        // Clean up
        largeList.clear();
        runtime.gc();  // Suggest garbage collection
    }
}
```

## Common Debugging Scenarios

### 1. Off-by-One Errors
```java
public class OffByOneErrors {
    // Common error: loop condition
    public static void printArrayWrong(int[] arr) {
        // ERROR: should be i < arr.length, not i <= arr.length
        for (int i = 0; i <= arr.length; i++) {
            System.out.println(arr[i]);  // ArrayIndexOutOfBoundsException
        }
    }
    
    // Correct version
    public static void printArrayCorrect(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
    }
    
    // Common error: substring bounds
    public static String extractMiddleWrong(String str) {
        // ERROR: doesn't handle even-length strings correctly
        int mid = str.length() / 2;
        return str.substring(mid - 1, mid + 1);  // May go out of bounds
    }
    
    // Correct version
    public static String extractMiddleCorrect(String str) {
        if (str.length() < 2) return str;
        int start = (str.length() - 1) / 2;
        int end = start + 1;
        return str.substring(start, end);
    }
}
```

### 2. Logic Errors
```java
public class LogicErrors {
    // Error: incorrect boolean logic
    public static boolean isValidGradeWrong(int grade) {
        // ERROR: uses OR instead of AND
        return grade >= 0 || grade <= 100;  // Always true!
    }
    
    // Correct version
    public static boolean isValidGradeCorrect(int grade) {
        return grade >= 0 && grade <= 100;
    }
    
    // Error: incorrect loop logic
    public static int findMaxWrong(int[] arr) {
        int max = 0;  // ERROR: should be arr[0]
        for (int i = 1; i < arr.length; i++) {  // ERROR: should start from 0
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        return max;
    }
    
    // Correct version
    public static int findMaxCorrect(int[] arr) {
        if (arr.length == 0) throw new IllegalArgumentException("Empty array");
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        return max;
    }
}
```

### 3. Race Conditions in Multithreading
```java
public class RaceConditionExample {
    private static int count = 0;
    
    // Unsafe version (race condition)
    public static void incrementUnsafe() {
        count++;  // Not atomic!
    }
    
    // Safe version
    public static synchronized void incrementSafe() {
        count++;
    }
    
    // Alternative safe version
    private static final AtomicInteger atomicCount = new AtomicInteger(0);
    public static void incrementAtomic() {
        atomicCount.incrementAndGet();
    }
}
```

## Key Exam Tips

1. **Practice tracing manually** - don't rely only on debugger
2. **Understand call stack** visualization
3. **Know how to set breakpoints** effectively
4. **Test edge cases** and boundary conditions
5. **Handle null inputs** and empty collections
6. **Verify loop conditions** for off-by-one errors
7. **Use assertions** for debugging and validation

## Common Debugging Tools and Techniques

1. **Print statements** for quick debugging
2. **Debugger breakpoints** for step-by-step execution
3. **Watch expressions** to monitor variable changes
4. **Conditional breakpoints** for specific scenarios
5. **Call stack analysis** for recursion and method calls
6. **Unit tests** for systematic verification
7. **Logging frameworks** for production debugging

## Practice Exercises

1. Trace through a binary search implementation manually
2. Debug a recursive factorial function with negative input
3. Find and fix off-by-one errors in array processing
4. Debug a sorting algorithm implementation
5. Trace object references through a linked list operation
6. Debug a multi-threaded program with race conditions
7. Test boundary conditions in a mathematical function
8. Debug a file reading operation with various error scenarios
9. Trace through nested loops with early termination
10. Debug a recursive tree traversal algorithm

## Testing Checklist

### Before Testing:
- ✓ Understand the expected behavior
- ✓ Identify edge cases and boundary conditions
- ✓ Prepare test data including invalid inputs
- ✓ Set up appropriate debugging tools

### During Testing:
- ✓ Test normal cases first
- ✓ Test boundary conditions
- ✓ Test error conditions and invalid inputs
- ✓ Verify error messages are appropriate
- ✓ Check memory usage for large inputs
- ✓ Test performance with various data sizes

### After Testing:
- ✓ Document any bugs found
- ✓ Verify fixes don't break other functionality
- ✓ Update test cases based on findings
- ✓ Consider adding automated tests