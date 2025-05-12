# Standard 1: List Structures and Strings - Study Guide

## Overview
Standard 1 covers static arrays, dynamic lists (ArrayList, Vector), and string manipulation in Java. This is the most heavily weighted section at **27% of the exam**.

## Part A: Static Arrays

### Core Concepts

#### 1. Array Declaration and Initialization
```java
// Declaration and initialization in one line
int[] scores = {85, 92, 78, 96, 88};
String[] names = {"Alice", "Bob", "Charlie"};

// Declaration then initialization
double[] grades = new double[5];
grades[0] = 3.5;
grades[1] = 3.8;

// Two-dimensional arrays
int[][] matrix = new int[3][4];
int[][] grid = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
```

#### 2. Array Operations
```java
// Sorting arrays
Arrays.sort(scores);
Arrays.sort(names);

// Searching arrays
int index = Arrays.binarySearch(scores, 92);

// Copying arrays
int[] copy = Arrays.copyOf(scores, scores.length);
int[] partial = Arrays.copyOfRange(scores, 1, 4);
```

#### 3. Iteration Techniques
```java
// Traditional for loop
for (int i = 0; i < scores.length; i++) {
    System.out.println(scores[i]);
}

// Enhanced for loop (for-each)
for (int score : scores) {
    System.out.println(score);
}

// Using iterators with Arrays.asList()
List<Integer> list = Arrays.asList(scores);
Iterator<Integer> it = list.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}
```

### Advanced Extensions

#### 1. Multi-dimensional Array Processing
```java
// Sum all elements in 2D array
public static int sum2D(int[][] matrix) {
    int total = 0;
    for (int[] row : matrix) {
        for (int value : row) {
            total += value;
        }
    }
    return total;
}

// Transpose a matrix
public static int[][] transpose(int[][] matrix) {
    int rows = matrix.length;
    int cols = matrix[0].length;
    int[][] result = new int[cols][rows];
    
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            result[j][i] = matrix[i][j];
        }
    }
    return result;
}
```

#### 2. Custom Sorting with Comparator
```java
// Sort strings by length
String[] words = {"apple", "pie", "banana", "cat"};
Arrays.sort(words, (a, b) -> Integer.compare(a.length(), b.length()));

// Sort 2D array by first column
int[][] data = {{3, 1}, {1, 4}, {2, 2}};
Arrays.sort(data, (a, b) -> Integer.compare(a[0], b[0]));
```

## Part B: Dynamic Lists (ArrayList, Vector)

### Core Concepts

#### 1. ArrayList Basics
```java
// Declaration and initialization
ArrayList<String> names = new ArrayList<>();
ArrayList<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));

// Adding elements
names.add("Alice");
names.add(1, "Bob"); // Insert at index 1

// Removing elements
names.remove(0);           // Remove by index
names.remove("Alice");     // Remove by value

// Accessing elements
String first = names.get(0);
names.set(0, "Charlie");   // Update element
```

#### 2. Vector vs ArrayList
```java
// Vector (synchronized, thread-safe)
Vector<Integer> vector = new Vector<>();
vector.addElement(10);    // Legacy method
vector.size();            // Get size
vector.capacity();        // Get capacity

// ArrayList (not synchronized, faster)
ArrayList<Integer> list = new ArrayList<>();
list.add(10);
list.size();
list.ensureCapacity(100); // Pre-allocate capacity
```

#### 3. List Operations
```java
// Searching
int index = list.indexOf(value);
boolean contains = list.contains(value);

// Bulk operations
List<Integer> sublist = list.subList(1, 4);
list.addAll(Arrays.asList(7, 8, 9));
list.removeAll(Arrays.asList(2, 4));
```

#### 4. Iteration Methods
```java
// Enhanced for loop
for (String name : names) {
    System.out.println(name);
}

// Iterator
Iterator<String> it = names.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}

// ListIterator (bidirectional)
ListIterator<String> lit = names.listIterator();
while (lit.hasNext()) {
    String name = lit.next();
    if (name.equals("Bob")) {
        lit.set("Robert"); // Modify during iteration
    }
}
```

### Advanced Extensions

#### 1. Generic List Operations
```java
// Generic method for list sorting
public static <T extends Comparable<T>> void sortList(List<T> list) {
    Collections.sort(list);
}

// Generic method for finding max
public static <T extends Comparable<T>> T findMax(List<T> list) {
    if (list.isEmpty()) return null;
    T max = list.get(0);
    for (T element : list) {
        if (element.compareTo(max) > 0) {
            max = element;
        }
    }
    return max;
}
```

#### 2. Advanced List Algorithms
```java
// Binary search in sorted list
public static int binarySearch(List<Integer> list, int target) {
    int left = 0, right = list.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (list.get(mid) == target) return mid;
        if (list.get(mid) < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}

// Remove duplicates while preserving order
public static <T> List<T> removeDuplicates(List<T> list) {
    return list.stream().distinct().collect(Collectors.toList());
}
```

## Part C: String Manipulation

### Core Concepts

#### 1. String Comparison
```java
String str1 = "Hello";
String str2 = "hello";

// Case-sensitive comparison
boolean equal1 = str1.equals(str2);           // false
boolean equal2 = str1.equalsIgnoreCase(str2); // true

// Lexicographic comparison
int comparison = str1.compareTo(str2);
int comparisonIgnoreCase = str1.compareToIgnoreCase(str2);
```

#### 2. String Length and Properties
```java
String text = "Hello World";
int length = text.length();           // 11
boolean empty = text.isEmpty();       // false
boolean blank = text.isBlank();       // false (Java 11+)
```

#### 3. String Copying and Extraction
```java
// Copying
String copy = new String(text);
String copy2 = String.valueOf(text);

// Substring extraction
String sub1 = text.substring(0, 5);   // "Hello"
String sub2 = text.substring(6);      // "World"
```

#### 4. String Concatenation
```java
// Using + operator
String greeting = "Hello" + " " + "World";

// Using concat() method
String greeting2 = "Hello".concat(" World");

// Using StringBuilder (more efficient for multiple operations)
StringBuilder sb = new StringBuilder();
sb.append("Hello").append(" ").append("World");
String result = sb.toString();
```

#### 5. Finding Substrings
```java
String text = "The quick brown fox";

// Finding positions
int index = text.indexOf("quick");        // 4
int lastIndex = text.lastIndexOf("o");    // 17
boolean contains = text.contains("brown"); // true

// Finding with starting position
int index2 = text.indexOf("o", 10);       // 17
```

#### 6. String Insertion and Replacement
```java
// Using StringBuilder for insertion
StringBuilder sb = new StringBuilder("Hello World");
sb.insert(6, "Beautiful ");  // "Hello Beautiful World"

// Replacement
String text = "Hello World";
String replaced = text.replace("World", "Java");
String replacedRegex = text.replaceAll("\\w+", "***");
```

### Advanced Extensions

#### 1. Regular Expressions
```java
String text = "Email: john@example.com, Phone: 123-456-7890";

// Pattern matching
Pattern emailPattern = Pattern.compile("\\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Z|a-z]{2,}\\b");
Matcher matcher = emailPattern.matcher(text);
if (matcher.find()) {
    System.out.println("Email found: " + matcher.group());
}

// Split with regex
String[] parts = text.split("[,:;]\\s*");
```

#### 2. String Formatting
```java
// String.format
String formatted = String.format("Name: %s, Age: %d, GPA: %.2f", "John", 20, 3.75);

// MessageFormat
String template = "At {1,time} on {1,date}, there was {2} on planet {0}.";
Object[] args = {"Mars", new Date(), "a disturbance"};
String message = MessageFormat.format(template, args);
```

#### 3. Advanced String Algorithms
```java
// Palindrome check
public static boolean isPalindrome(String str) {
    int left = 0, right = str.length() - 1;
    while (left < right) {
        if (str.charAt(left) != str.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}

// Longest common subsequence
public static int lcs(String text1, String text2) {
    int m = text1.length(), n = text2.length();
    int[][] dp = new int[m + 1][n + 1];
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[m][n];
}
```

## Key Exam Tips

1. **Know the difference** between arrays (fixed size) and lists (dynamic size)
2. **Understand when to use** each data structure based on the problem requirements
3. **Practice iteration methods** - both traditional and enhanced for loops
4. **Master string operations** especially comparison, searching, and substring extraction
5. **Remember** that strings are immutable - operations create new strings
6. **Understand** the performance implications of StringBuilder vs String concatenation
7. **Practice** common algorithms like sorting, searching, and string manipulation

## Common Pitfalls

1. **Array IndexOutOfBoundsException** - always check array bounds
2. **NullPointerException** with strings - check for null before operations
3. **Modifying collections** during iteration - use Iterator.remove() or collect to new list
4. **String comparison** using `==` instead of `.equals()`
5. **Memory inefficiency** with repeated string concatenation
6. **Confusing capacity and size** in ArrayList/Vector

## Practice Exercises

1. Implement a method to rotate an array left by k positions
2. Find the most frequent element in an ArrayList
3. Write a method to reverse words in a string
4. Merge two sorted arrays into one sorted array
5. Implement a simple text search with highlighting
6. Create a method to flatten a 2D array into a 1D array
7. Write a string compression algorithm (e.g., "aaabbc" → "a3b2c1")