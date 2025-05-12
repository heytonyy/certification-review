# Standard 2: Sequential Files - Study Guide

## Overview
Standard 2 covers sequential file operations in Java including creating, reading, writing, and updating files. This section represents **9% of the exam**.

## Core Concepts

### 1. File Creation and Initialization

#### Using PrintWriter and FileWriter
```java
import java.io.*;

// Creating a new file with PrintWriter
PrintWriter writer = new PrintWriter(new FileWriter("data.txt"));
writer.println("Hello, World!");
writer.close();

// Creating with FileWriter (character-based)
FileWriter fw = new FileWriter("output.txt");
fw.write("Sample text");
fw.flush();
fw.close();

// Using try-with-resources (recommended)
try (PrintWriter writer = new PrintWriter(new FileWriter("data.txt"))) {
    writer.println("Line 1");
    writer.println("Line 2");
} // Automatically closes the file
```

#### Using BufferedWriter for Better Performance
```java
import java.io.*;

// Buffered writing for better performance
try (BufferedWriter bw = new BufferedWriter(new FileWriter("large_file.txt"))) {
    for (int i = 0; i < 1000; i++) {
        bw.write("Line " + i);
        bw.newLine();
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

### 2. Writing Data to Sequential Files

#### Writing Different Data Types
```java
try (PrintWriter writer = new PrintWriter(new FileWriter("student_data.txt"))) {
    // Writing strings
    writer.println("John Doe");
    
    // Writing numbers
    writer.println(95);
    writer.println(3.75);
    
    // Writing formatted data
    writer.printf("Name: %s, Grade: %d, GPA: %.2f%n", "Jane Smith", 87, 3.45);
    
    // Writing arrays
    int[] scores = {85, 92, 78, 96};
    for (int score : scores) {
        writer.print(score + " ");
    }
    writer.println(); // New line after array
} catch (IOException e) {
    e.printStackTrace();
}
```

#### Writing Objects (Serialization Alternative)
```java
// Writing object data as text
public class Student {
    private String name;
    private int age;
    private double gpa;
    
    // Constructor, getters, setters...
    
    public String toFileFormat() {
        return name + "," + age + "," + gpa;
    }
}

// Writing student objects to file
try (PrintWriter writer = new PrintWriter(new FileWriter("students.txt"))) {
    Student[] students = {
        new Student("Alice", 20, 3.8),
        new Student("Bob", 19, 3.2)
    };
    
    for (Student student : students) {
        writer.println(student.toFileFormat());
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

### 3. Reading Data from Sequential Files

#### Using Scanner for Reading
```java
import java.io.*;
import java.util.Scanner;

// Reading line by line
try (Scanner scanner = new Scanner(new File("data.txt"))) {
    while (scanner.hasNextLine()) {
        String line = scanner.nextLine();
        System.out.println(line);
    }
} catch (FileNotFoundException e) {
    e.printStackTrace();
}

// Reading different data types
try (Scanner scanner = new Scanner(new File("student_data.txt"))) {
    String name = scanner.nextLine();
    int grade = scanner.nextInt();
    double gpa = scanner.nextDouble();
    scanner.nextLine(); // Consume remaining newline
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
```

#### Using BufferedReader for Large Files
```java
try (BufferedReader br = new BufferedReader(new FileReader("large_file.txt"))) {
    String line;
    int lineCount = 0;
    
    while ((line = br.readLine()) != null) {
        System.out.println("Line " + lineCount + ": " + line);
        lineCount++;
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

#### Reading Structured Data
```java
// Reading student data from CSV-like format
ArrayList<Student> students = new ArrayList<>();

try (Scanner scanner = new Scanner(new File("students.txt"))) {
    while (scanner.hasNextLine()) {
        String line = scanner.nextLine();
        String[] parts = line.split(",");
        
        if (parts.length == 3) {
            String name = parts[0].trim();
            int age = Integer.parseInt(parts[1].trim());
            double gpa = Double.parseDouble(parts[2].trim());
            students.add(new Student(name, age, gpa));
        }
    }
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
```

### 4. Updating Sequential Files

#### Updating by Reading and Rewriting
```java
// Method 1: Read all data, modify, and rewrite entire file
ArrayList<String> lines = new ArrayList<>();

// Read existing data
try (Scanner scanner = new Scanner(new File("data.txt"))) {
    while (scanner.hasNextLine()) {
        lines.add(scanner.nextLine());
    }
} catch (FileNotFoundException e) {
    e.printStackTrace();
}

// Modify specific line
if (lines.size() > 2) {
    lines.set(2, "Updated line 3");
}

// Write back to file
try (PrintWriter writer = new PrintWriter(new FileWriter("data.txt"))) {
    for (String line : lines) {
        writer.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

#### Appending to Files
```java
// Appending to existing file
try (PrintWriter writer = new PrintWriter(new FileWriter("data.txt", true))) {
    writer.println("This is a new line appended to the file");
} catch (IOException e) {
    e.printStackTrace();
}

// Appending with BufferedWriter
try (BufferedWriter bw = new BufferedWriter(new FileWriter("log.txt", true))) {
    bw.newLine();
    bw.write(new Date() + ": Application started");
} catch (IOException e) {
    e.printStackTrace();
}
```

#### Updating Student Records Example
```java
// Update student GPA by name
public static void updateStudentGPA(String filename, String studentName, double newGPA) {
    ArrayList<Student> students = new ArrayList<>();
    
    // Read all students
    try (Scanner scanner = new Scanner(new File(filename))) {
        while (scanner.hasNextLine()) {
            String line = scanner.nextLine();
            String[] parts = line.split(",");
            if (parts.length == 3) {
                Student student = new Student(parts[0].trim(), 
                                            Integer.parseInt(parts[1].trim()),
                                            Double.parseDouble(parts[2].trim()));
                students.add(student);
            }
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
        return;
    }
    
    // Update specific student
    for (Student student : students) {
        if (student.getName().equals(studentName)) {
            student.setGPA(newGPA);
            break;
        }
    }
    
    // Write all students back
    try (PrintWriter writer = new PrintWriter(new FileWriter(filename))) {
        for (Student student : students) {
            writer.println(student.toFileFormat());
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

## Advanced Extensions

### 1. File Processing with Error Handling
```java
public class FileProcessor {
    private String filename;
    private ArrayList<String> data;
    
    public FileProcessor(String filename) {
        this.filename = filename;
        this.data = new ArrayList<>();
    }
    
    public boolean loadFromFile() {
        data.clear();
        try (Scanner scanner = new Scanner(new File(filename))) {
            while (scanner.hasNextLine()) {
                data.add(scanner.nextLine());
            }
            return true;
        } catch (FileNotFoundException e) {
            System.err.println("File not found: " + filename);
            return false;
        } catch (Exception e) {
            System.err.println("Error reading file: " + e.getMessage());
            return false;
        }
    }
    
    public boolean saveToFile() {
        // Create backup first
        backupFile();
        
        try (PrintWriter writer = new PrintWriter(new FileWriter(filename))) {
            for (String line : data) {
                writer.println(line);
            }
            return true;
        } catch (IOException e) {
            System.err.println("Error writing file: " + e.getMessage());
            restoreFromBackup();
            return false;
        }
    }
    
    private void backupFile() {
        String backupName = filename + ".backup";
        try {
            Files.copy(Paths.get(filename), Paths.get(backupName), 
                      StandardCopyOption.REPLACE_EXISTING);
        } catch (IOException e) {
            System.err.println("Warning: Could not create backup");
        }
    }
    
    private void restoreFromBackup() {
        String backupName = filename + ".backup";
        try {
            Files.copy(Paths.get(backupName), Paths.get(filename), 
                      StandardCopyOption.REPLACE_EXISTING);
        } catch (IOException e) {
            System.err.println("Error: Could not restore from backup");
        }
    }
}
```

### 2. Large File Processing with Streaming
```java
import java.nio.file.*;

// Processing large files efficiently
public class LargeFileProcessor {
    public static void processLargeFile(String inputFile, String outputFile) {
        try (Stream<String> lines = Files.lines(Paths.get(inputFile));
             PrintWriter writer = new PrintWriter(new FileWriter(outputFile))) {
            
            lines
                .filter(line -> !line.trim().isEmpty())
                .map(String::toUpperCase)
                .forEach(writer::println);
                
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    // Count words in large file without loading entire file into memory
    public static long countWords(String filename) {
        try (Stream<String> lines = Files.lines(Paths.get(filename))) {
            return lines
                .flatMap(line -> Arrays.stream(line.split("\\s+")))
                .filter(word -> !word.isEmpty())
                .count();
        } catch (IOException e) {
            e.printStackTrace();
            return -1;
        }
    }
}
```

### 3. CSV File Processing
```java
public class CSVProcessor {
    public static void writeCSV(String filename, String[][] data) {
        try (PrintWriter writer = new PrintWriter(new FileWriter(filename))) {
            for (String[] row : data) {
                StringBuilder sb = new StringBuilder();
                for (int i = 0; i < row.length; i++) {
                    if (i > 0) sb.append(",");
                    
                    // Handle commas and quotes in data
                    String field = row[i];
                    if (field.contains(",") || field.contains("\"")) {
                        field = "\"" + field.replace("\"", "\"\"") + "\"";
                    }
                    sb.append(field);
                }
                writer.println(sb.toString());
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    public static ArrayList<String[]> readCSV(String filename) {
        ArrayList<String[]> data = new ArrayList<>();
        
        try (Scanner scanner = new Scanner(new File(filename))) {
            while (scanner.hasNextLine()) {
                String line = scanner.nextLine();
                ArrayList<String> fields = parseCSVLine(line);
                data.add(fields.toArray(new String[0]));
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        
        return data;
    }
    
    private static ArrayList<String> parseCSVLine(String line) {
        ArrayList<String> fields = new ArrayList<>();
        StringBuilder currentField = new StringBuilder();
        boolean inQuotes = false;
        
        for (int i = 0; i < line.length(); i++) {
            char c = line.charAt(i);
            
            if (c == '"') {
                if (inQuotes && i + 1 < line.length() && line.charAt(i + 1) == '"') {
                    currentField.append('"');
                    i++; // Skip next quote
                } else {
                    inQuotes = !inQuotes;
                }
            } else if (c == ',' && !inQuotes) {
                fields.add(currentField.toString());
                currentField = new StringBuilder();
            } else {
                currentField.append(c);
            }
        }
        
        fields.add(currentField.toString());
        return fields;
    }
}
```

### 4. Configuration File Management
```java
import java.util.Properties;

public class ConfigManager {
    private Properties properties;
    private String configFile;
    
    public ConfigManager(String configFile) {
        this.configFile = configFile;
        this.properties = new Properties();
        loadConfig();
    }
    
    public void loadConfig() {
        try (FileInputStream fis = new FileInputStream(configFile)) {
            properties.load(fis);
        } catch (IOException e) {
            System.err.println("Error loading config: " + e.getMessage());
            // Set default values
            setDefaultValues();
        }
    }
    
    public void saveConfig() {
        try (FileOutputStream fos = new FileOutputStream(configFile)) {
            properties.store(fos, "Application Configuration");
        } catch (IOException e) {
            System.err.println("Error saving config: " + e.getMessage());
        }
    }
    
    public String get(String key, String defaultValue) {
        return properties.getProperty(key, defaultValue);
    }
    
    public void set(String key, String value) {
        properties.setProperty(key, value);
    }
    
    private void setDefaultValues() {
        properties.setProperty("app.name", "My Application");
        properties.setProperty("app.version", "1.0");
        properties.setProperty("database.host", "localhost");
        properties.setProperty("database.port", "5432");
    }
}
```

## Key Exam Tips

1. **Always use try-with-resources** for automatic file closing
2. **Handle exceptions** appropriately - FileNotFoundException, IOException
3. **Know the difference** between text and binary file operations
4. **Understand buffering** - when to use BufferedReader/BufferedWriter
5. **Practice file updating** patterns - reading all data vs. appending
6. **Know how to handle different data types** when reading/writing
7. **Understand file paths** - relative vs. absolute

## Common Pitfalls

1. **Not closing files** properly - use try-with-resources or finally blocks
2. **File not found** - always check if file exists before reading
3. **Data type mismatches** when reading (expecting int but getting string)
4. **Memory issues** with large files - use streaming when possible
5. **Character encoding** problems - specify encoding when needed
6. **Mixing different readers/writers** on same file simultaneously
7. **Not handling the last line** properly when reading

## Important File Operations Checklist

### Reading Files:
- ✓ Check if file exists
- ✓ Handle FileNotFoundException
- ✓ Choose appropriate reader (Scanner vs BufferedReader)
- ✓ Handle different data types correctly
- ✓ Close resources properly

### Writing Files:
- ✓ Handle IOException
- ✓ Choose appropriate writer (PrintWriter vs BufferedWriter)
- ✓ Decide on append vs overwrite mode
- ✓ Flush output when necessary
- ✓ Close resources properly

### Updating Files:
- ✓ Consider backup strategy
- ✓ Read existing data first
- ✓ Modify data structure
- ✓ Write back to file
- ✓ Handle partial update failures

## Practice Exercises

1. Create a program that reads a file and counts the frequency of each word
2. Implement a simple log file system that appends timestamped messages
3. Write a program to merge two sorted files into one sorted file
4. Create a student database using text files with add/update/delete operations
5. Implement a configuration manager that reads/writes application settings
6. Write a program to process a large CSV file and extract specific columns
7. Create a file backup utility that creates timestamped backups
8. Implement a simple text-based inventory management system

## File Format Examples

### Simple Text File:
```
John Doe
85
3.75
Jane Smith
92
3.85
```

### CSV Format:
```
Name,Age,GPA
"Doe, John",20,3.75
"Smith, Jane",19,3.85
```

### Configuration File:
```
# Application Configuration
app.name=My Application
app.version=1.0
database.host=localhost
database.port=5432
```