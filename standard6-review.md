# Standard 6: Team Programming - Study Guide

## Overview
Standard 6 covers team programming skills including project planning, collaboration, and applying programming knowledge effectively in team environments. This section represents **7% of the exam**.

## Part A: Programming Project Application

### Core Concepts

#### 1. Formalizing Specifications
```java
/**
 * Project: Student Grade Management System
 * 
 * FUNCTIONAL REQUIREMENTS:
 * - Store student information (name, ID, courses)
 * - Calculate course grades based on assignments, exams, and final
 * - Generate grade reports and transcripts
 * - Export data to various formats (CSV, PDF)
 * 
 * NON-FUNCTIONAL REQUIREMENTS:
 * - System must handle 500+ students
 * - Grade calculations must complete within 2 seconds
 * - Data must persist between application sessions
 * - User interface must be intuitive for teachers
 * 
 * CONSTRAINTS:
 * - Java 8 or higher
 * - No external libraries except for PDF generation
 * - Must work offline
 * - Maximum memory usage: 512MB
 */

// Example of formal method specification
public class GradeCalculator {
    /**
     * Calculates final grade based on weighted components
     * @param assignments Array of assignment scores (0-100)
     * @param exams Array of exam scores (0-100)
     * @param finalExam Final exam score (0-100)
     * @param weights Weights for each component [assignments%, exams%, final%]
     * @precondition assignments.length > 0, exams.length > 0
     * @precondition finalExam >= 0 && finalExam <= 100
     * @precondition weights[0] + weights[1] + weights[2] = 100
     * @postcondition return value between 0 and 100
     * @return Final grade as percentage
     */
    public static double calculateFinalGrade(double[] assignments, 
                                           double[] exams, 
                                           double finalExam,
                                           double[] weights) {
        // Implementation details
    }
}
```

#### 2. Choosing Proper Input Parameters
```java
// Bad: Too many parameters, unclear purpose
public static void createStudent(String n, int a, double g, 
                               String c1, String c2, String c3,
                               double s1, double s2, double s3) {
    // Hard to understand and maintain
}

// Good: Use objects and meaningful parameter names
public class Student {
    private String name;
    private int studentId;
    private List<Course> courses;
    
    public Student(String name, int studentId) {
        this.name = name;
        this.studentId = studentId;
        this.courses = new ArrayList<>();
    }
}

public class Course {
    private String courseName;
    private String courseCode;
    private double grade;
    
    public Course(String courseCode, String courseName) {
        this.courseCode = courseCode;
        this.courseName = courseName;
        this.grade = 0.0;
    }
}

// Better method signature
public static Student createStudent(String name, int studentId, 
                                  List<Course> initialCourses) {
    Student student = new Student(name, studentId);
    student.setCourses(initialCourses);
    return student;
}

// Even better: Use Builder pattern for complex objects
public class StudentBuilder {
    private String name;
    private int studentId;
    private List<Course> courses = new ArrayList<>();
    
    public StudentBuilder setName(String name) {
        this.name = name;
        return this;
    }
    
    public StudentBuilder setStudentId(int studentId) {
        this.studentId = studentId;
        return this;
    }
    
    public StudentBuilder addCourse(Course course) {
        this.courses.add(course);
        return this;
    }
    
    public Student build() {
        return new Student(name, studentId, courses);
    }
}
```

#### 3. Choosing Appropriate Data Structures
```java
// Analyzing requirements to choose data structures
public class GradeBookSystem {
    // HashMap for O(1) student lookup by ID
    private Map<Integer, Student> studentsByID;
    
    // TreeMap for sorted course list
    private TreeMap<String, Course> coursesByCodes;
    
    // ArrayList for maintaining grade history (insertion order matters)
    private List<GradeEntry> gradeHistory;
    
    // Set for unique prerequisites (no duplicates needed)
    private Set<String> coursePrerequisites;
    
    // Priority queue for processing grades by priority
    private PriorityQueue<GradeTask> gradingQueue;
    
    public GradeBookSystem() {
        this.studentsByID = new HashMap<>();
        this.coursesByCodes = new TreeMap<>();  // Automatically sorted
        this.gradeHistory = new ArrayList<>();
        this.coursePrerequisites = new HashSet<>();
        this.gradingQueue = new PriorityQueue<>(new GradeTaskComparator());
    }
    
    // Choose data structure based on access patterns
    // Frequent lookups by ID -> HashMap
    // Need sorted order -> TreeMap/TreeSet
    // Order matters -> ArrayList/LinkedList
    // No duplicates -> Set
    // Priority-based processing -> PriorityQueue
}

// Example of data structure analysis
public class DataStructureChoice {
    /**
     * Scenario 1: Need to store unique email addresses
     * Choice: HashSet<String> - O(1) contains(), no duplicates
     */
    private Set<String> emailAddresses = new HashSet<>();
    
    /**
     * Scenario 2: Need to maintain list of recent actions (newest first)
     * Choice: LinkedList - O(1) insertion at head
     */
    private Deque<UserAction> recentActions = new LinkedList<>();
    
    /**
     * Scenario 3: Need fast access to student records by ID
     * Choice: HashMap - O(1) average lookup time
     */
    private Map<Integer, Student> studentRecords = new HashMap<>();
    
    /**
     * Scenario 4: Need to process tasks in priority order
     * Choice: PriorityQueue - O(log n) insertion, O(1) peek
     */
    private PriorityQueue<Task> taskQueue = new PriorityQueue<>();
}
```

#### 4. Designing Appropriate Output
```java
// Different output formats for different audiences
public class ReportGenerator {
    // Teacher-focused output: Detailed breakdown
    public void generateTeacherReport(Course course) {
        System.out.println("DETAILED COURSE REPORT");
        System.out.println("======================");
        System.out.printf("Course: %s (%s)%n", course.getName(), course.getCode());
        System.out.printf("Average Grade: %.2f%n", course.getAverageGrade());
        System.out.printf("Highest Grade: %.2f%n", course.getHighestGrade());
        System.out.printf("Lowest Grade: %.2f%n", course.getLowestGrade());
        System.out.printf("Standard Deviation: %.2f%n", course.getStandardDeviation());
        
        // Grade distribution
        System.out.println("\nGrade Distribution:");
        Map<String, Integer> distribution = course.getGradeDistribution();
        distribution.forEach((grade, count) -> 
            System.out.printf("%s: %d students%n", grade, count));
    }
    
    // Student-focused output: Personal summary
    public void generateStudentReport(Student student, Course course) {
        System.out.println("STUDENT GRADE REPORT");
        System.out.println("===================");
        System.out.printf("Student: %s (ID: %d)%n", student.getName(), student.getId());
        System.out.printf("Course: %s%n", course.getName());
        System.out.printf("Current Grade: %.2f%% (%s)%n", 
                         student.getGrade(course), 
                         student.getLetterGrade(course));
        
        // Detailed breakdown
        System.out.println("\nGrade Breakdown:");
        System.out.printf("Assignments: %.2f%% (%.1f%% weight)%n", 
                         student.getAssignmentAverage(course), 
                         course.getAssignmentWeight());
        System.out.printf("Exams: %.2f%% (%.1f%% weight)%n", 
                         student.getExamAverage(course), 
                         course.getExamWeight());
        System.out.printf("Final Exam: %.2f%% (%.1f%% weight)%n", 
                         student.getFinalExamGrade(course), 
                         course.getFinalWeight());
    }
    
    // Administrative output: Summary statistics
    public void generateAdminSummary(List<Course> courses) {
        System.out.println("ADMINISTRATIVE SUMMARY");
        System.out.println("=====================");
        System.out.printf("Total Courses: %d%n", courses.size());
        System.out.printf("Total Students: %d%n", getTotalStudents(courses));
        
        for (Course course : courses) {
            System.out.printf("\n%s: %.2f%% average, %d students%n", 
                             course.getCode(), 
                             course.getAverageGrade(),
                             course.getStudentCount());
        }
    }
    
    // Exportable formats
    public void exportToCSV(String filename, Course course) {
        try (PrintWriter writer = new PrintWriter(new FileWriter(filename))) {
            writer.println("Student ID,Name,Assignment Avg,Exam Avg,Final,Overall Grade,Letter Grade");
            
            for (Student student : course.getStudents()) {
                writer.printf("%d,%s,%.2f,%.2f,%.2f,%.2f,%s%n",
                             student.getId(),
                             student.getName(),
                             student.getAssignmentAverage(course),
                             student.getExamAverage(course),
                             student.getFinalExamGrade(course),
                             student.getGrade(course),
                             student.getLetterGrade(course));
            }
        } catch (IOException e) {
            System.err.println("Error writing to CSV: " + e.getMessage());
        }
    }
    
    // JSON format for API integration
    public String exportToJSON(Course course) {
        StringBuilder json = new StringBuilder();
        json.append("{\n");
        json.append("  \"course\": {\n");
        json.append("    \"code\": \"").append(course.getCode()).append("\",\n");
        json.append("    \"name\": \"").append(course.getName()).append("\",\n");
        json.append("    \"students\": [\n");
        
        List<Student> students = course.getStudents();
        for (int i = 0; i < students.size(); i++) {
            Student student = students.get(i);
            json.append("      {\n");
            json.append("        \"id\": ").append(student.getId()).append(",\n");
            json.append("        \"name\": \"").append(student.getName()).append("\",\n");
            json.append("        \"grade\": ").append(student.getGrade(course)).append("\n");
            json.append("      }");
            if (i < students.size() - 1) json.append(",");
            json.append("\n");
        }
        
        json.append("    ]\n");
        json.append("  }\n");
        json.append("}");
        return json.toString();
    }
}
```

#### 5. Using Appropriate Test Data
```java
public class TestDataManager {
    // Create representative test data sets
    public static class TestDataSet {
        private List<Student> students;
        private List<Course> courses;
        private String description;
        
        public TestDataSet(String description) {
            this.description = description;
            this.students = new ArrayList<>();
            this.courses = new ArrayList<>();
        }
        
        // Getters and setters
    }
    
    // Small dataset for unit testing
    public static TestDataSet createSmallDataSet() {
        TestDataSet dataset = new TestDataSet("Small dataset for basic testing");
        
        // 3 students, 2 courses
        dataset.addStudent(new Student("Alice Johnson", 1001));
        dataset.addStudent(new Student("Bob Smith", 1002));
        dataset.addStudent(new Student("Carol Davis", 1003));
        
        Course cs101 = new Course("CS101", "Introduction to Programming");
        Course cs102 = new Course("CS102", "Data Structures");
        dataset.addCourse(cs101);
        dataset.addCourse(cs102);
        
        return dataset;
    }
    
    // Medium dataset for integration testing
    public static TestDataSet createMediumDataSet() {
        TestDataSet dataset = new TestDataSet("Medium dataset for integration testing");
        
        // 50 students, 5 courses
        for (int i = 1; i <= 50; i++) {
            dataset.addStudent(new Student("Student " + i, 1000 + i));
        }
        
        String[] courseCodes = {"CS101", "CS102", "CS201", "CS202", "CS301"};
        String[] courseNames = {
            "Introduction to Programming",
            "Data Structures",
            "Algorithms",
            "Database Systems",
            "Software Engineering"
        };
        
        for (int i = 0; i < courseCodes.length; i++) {
            dataset.addCourse(new Course(courseCodes[i], courseNames[i]));
        }
        
        return dataset;
    }
    
    // Edge case data for boundary testing
    public static TestDataSet createEdgeCaseDataSet() {
        TestDataSet dataset = new TestDataSet("Edge cases for boundary testing");
        
        // Empty student name
        dataset.addStudent(new Student("", 0));
        
        // Very long name
        dataset.addStudent(new Student("A".repeat(100), Integer.MAX_VALUE));
        
        // Special characters in name
        dataset.addStudent(new Student("José Müller-O'Connor", 1001));
        
        // Course with no students
        dataset.addCourse(new Course("EMPTY", "Empty Course"));
        
        // Course with maximum students
        Course largeCourse = new Course("LARGE", "Large Course");
        for (int i = 0; i < 1000; i++) {
            largeCourse.addStudent(new Student("Student " + i, i));
        }
        dataset.addCourse(largeCourse);
        
        return dataset;
    }
    
    // Performance test data
    public static TestDataSet createPerformanceDataSet() {
        TestDataSet dataset = new TestDataSet("Large dataset for performance testing");
        
        // 10,000 students, 100 courses
        for (int i = 1; i <= 10000; i++) {
            dataset.addStudent(new Student("Student " + i, i));
        }
        
        for (int i = 1; i <= 100; i++) {
            Course course = new Course("COURSE" + i, "Course " + i);
            // Randomly assign students to courses
            Random rand = new Random();
            for (Student student : dataset.getStudents()) {
                if (rand.nextDouble() < 0.3) {  // 30% chance
                    course.addStudent(student);
                }
            }
            dataset.addCourse(course);
        }
        
        return dataset;
    }
    
    // Invalid data for error testing
    public static void testInvalidInputs() {
        try {
            new Student(null, 1001);
            assert false : "Should fail for null name";
        } catch (IllegalArgumentException e) {
            System.out.println("Correctly caught null name error");
        }
        
        try {
            new Student("Valid Name", -1);
            assert false : "Should fail for negative ID";
        } catch (IllegalArgumentException e) {
            System.out.println("Correctly caught negative ID error");
        }
        
        try {
            new Course("", "Valid Name");
            assert false : "Should fail for empty course code";
        } catch (IllegalArgumentException e) {
            System.out.println("Correctly caught empty course code error");
        }
    }
}
```

#### 6. Writing Good Documentation
```java
/**
 * StudentGradeManager - A comprehensive system for managing student grades
 * 
 * This class provides functionality to:
 * - Add and remove students from courses
 * - Calculate weighted grades based on assignments, exams, and finals
 * - Generate various reports for different stakeholders
 * - Export data in multiple formats
 * 
 * @author Team Name
 * @version 1.0
 * @since 2024-01-15
 */
public class StudentGradeManager {
    
    // Class-level Javadoc for constants
    /** Maximum number of students per course */
    private static final int MAX_STUDENTS_PER_COURSE = 500;
    
    /** Grade weights must sum to this value */
    private static final double TOTAL_WEIGHT = 100.0;
    
    /**
     * Calculates a student's final grade for a course
     * 
     * This method uses a weighted average formula where:
     * Final Grade = (Assignment Avg × Assignment Weight) + 
     *               (Exam Avg × Exam Weight) + 
     *               (Final Exam × Final Weight)
     * 
     * Example usage:
     * <pre>
     * {@code
     * double[] assignments = {85.0, 90.0, 88.0};
     * double[] exams = {87.0, 92.0};
     * double finalExam = 89.0;
     * double[] weights = {40.0, 35.0, 25.0};  // Must sum to 100
     * 
     * double finalGrade = calculateFinalGrade(assignments, exams, finalExam, weights);
     * System.out.println("Final grade: " + finalGrade);  // Output: 88.45
     * }
     * </pre>
     * 
     * @param assignments Array of assignment scores (must be between 0-100)
     * @param exams Array of exam scores (must be between 0-100)
     * @param finalExam Final exam score (must be between 0-100)
     * @param weights Array of weights [assignment%, exam%, final%] (must sum to 100)
     * @return The calculated final grade as a percentage (0-100)
     * @throws IllegalArgumentException if any score is outside 0-100 range
     * @throws IllegalArgumentException if weights don't sum to 100
     * @throws IllegalArgumentException if any array is null or empty
     * @see #calculateLetterGrade(double)
     * @since 1.0
     */
    public double calculateFinalGrade(double[] assignments, 
                                    double[] exams, 
                                    double finalExam, 
                                    double[] weights) {
        validateInputs(assignments, exams, finalExam, weights);
        
        double assignmentAvg = calculateAverage(assignments);
        double examAvg = calculateAverage(exams);
        
        return (assignmentAvg * weights[0] / 100) + 
               (examAvg * weights[1] / 100) + 
               (finalExam * weights[2] / 100);
    }
    
    // Internal helper methods should have docs too
    /**
     * Validates all input parameters for grade calculation
     * @param assignments Assignment scores array
     * @param exams Exam scores array  
     * @param finalExam Final exam score
     * @param weights Weight distribution array
     * @throws IllegalArgumentException if validation fails
     */
    private void validateInputs(double[] assignments, double[] exams, 
                               double finalExam, double[] weights) {
        // Validation implementation...
    }
    
    // README.md style documentation
    /*
     * PROJECT SETUP AND USAGE
     * ======================
     * 
     * Prerequisites:
     * - Java 8 or higher
     * - No external dependencies required
     * 
     * How to compile:
     * javac *.java
     * 
     * How to run:
     * java StudentGradeManager
     * 
     * Project Structure:
     * src/
     * ├── models/
     * │   ├── Student.java
     * │   └── Course.java
     * ├── managers/
     * │   └── StudentGradeManager.java
     * ├── utils/
     * │   └── ReportGenerator.java
     * └── tests/
     *     └── TestSuite.java
     * 
     * API Usage Examples:
     * 
     * 1. Creating a student:
     *    Student student = new Student("John Doe", 12345);
     * 
     * 2. Creating a course:
     *    Course course = new Course("CS101", "Programming Basics");
     * 
     * 3. Adding student to course:
     *    course.addStudent(student);
     * 
     * 4. Calculating grades:
     *    double[] assignments = {85.0, 90.0, 88.0};
     *    double[] exams = {87.0, 92.0};
     *    double finalExam = 89.0;
     *    double[] weights = {40.0, 35.0, 25.0};
     *    
     *    double finalGrade = manager.calculateFinalGrade(assignments, exams, finalExam, weights);
     * 
     * Known Issues:
     * - None currently
     * 
     * Contributing:
     * - Follow Google Java Style Guide
     * - Write tests for new features
     * - Update documentation
     */
}

// Code comments for complex algorithms
public class SortingAlgorithms {
    /**
     * Quick sort implementation using Lomuto partition scheme
     * 
     * Time Complexity: O(n log n) average, O(n²) worst case
     * Space Complexity: O(log n) due to recursion
     */
    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            // partition() returns the index where the pivot element 
            // should be placed so that elements on left are smaller
            // and elements on right are greater
            int pivotIndex = partition(arr, low, high);
            
            // Recursively sort elements before and after partition
            quickSort(arr, low, pivotIndex - 1);
            quickSort(arr, pivotIndex + 1, high);
        }
    }
    
    // TODO: Optimize this for arrays with many duplicate elements
    // FIXME: Handle integer overflow in partition calculation
    // NOTE: This uses Lomuto partition, consider Hoare partition for better performance
}
```

## Part B: Teamwork and Collaboration

### Core Concepts

#### 1. Presenting Work to Group
```java
// Structure for code review presentations
public class CodeReviewPresentation {
    /**
     * PRESENTATION TEMPLATE FOR CODE REVIEWS
     * 
     * 1. CONTEXT (2 minutes)
     *    - What feature/bug were you working on?
     *    - What approach did you take?
     *    - What challenges did you face?
     * 
     * 2. CODE WALKTHROUGH (5-7 minutes)
     *    - Show the main changes
     *    - Explain any complex logic
     *    - Point out design decisions
     * 
     * 3. TESTING (2 minutes)
     *    - What tests did you write?
     *    - How did you verify the functionality?
     *    - Any edge cases considered?
     * 
     * 4. Q&A (3-4 minutes)
     *    - Be open to feedback
     *    - Ask specific questions if needed
     *    - Thank reviewers for their input
     */
    
    // Example presentation script comments
    public class GradeCalculator {
        /**
         * PRESENTATION NOTES:
         * 
         * "I was tasked with implementing the weighted grade calculation feature.
         * The main challenge was handling edge cases like missing assignments
         * or different weight distributions across courses.
         * 
         * Let me walk through the implementation:
         * 1. Input validation [point to validateInputs method]
         * 2. Average calculation [point to calculateAverage method]
         * 3. Weighted calculation [point to main formula]
         * 
         * For testing, I created test cases covering:
         * - Normal scenarios with valid inputs
         * - Edge cases like empty arrays
         * - Invalid inputs like negative weights
         * 
         * Questions I have for the team:
         * - Should we round grades to nearest integer or keep decimals?
         * - How should we handle courses with no assignments yet?"
         */
        
        public double calculateWeightedGrade(double[] assignments, 
                                           double[] exams, 
                                           double[] weights) {
            // Implementation...
        }
    }
}

// Git commit message best practices for team collaboration
/*
GOOD COMMIT MESSAGES:

feat: Add weighted grade calculation
- Implement calculateWeightedGrade method
- Add input validation for weights and scores
- Include comprehensive test suite

fix: Resolve division by zero in average calculation
- Add check for empty arrays before calculation
- Return 0.0 for courses with no grades yet
- Add unit tests for edge cases

docs: Update API documentation for GradeManager
- Fix typos in method descriptions  
- Add usage examples for grade calculation
- Include parameter validation rules

refactor: Extract common validation logic
- Create ValidationUtils class
- Remove duplicate code from grade classes
- Maintain backward compatibility

test: Add integration tests for student enrollment
- Test enrollment limits
- Verify course capacity constraints
- Add performance tests for large datasets

BAD COMMIT MESSAGES:
- "fixed stuff"
- "working on grades"
- "update"
- "final version"
*/
```

#### 2. Coordinating Work with Others
```java
// Project coordination strategies
public class TeamCoordination {
    
    /**
     * FEATURE BRANCH WORKFLOW
     * 
     * 1. Create feature branch from main:
     *    git checkout -b feature/grade-calculation
     * 
     * 2. Work on feature, making small commits:
     *    git add .
     *    git commit -m "Add basic grade calculation logic"
     * 
     * 3. Keep in sync with main:
     *    git fetch origin
     *    git rebase origin/main
     * 
     * 4. Push feature branch:
     *    git push origin feature/grade-calculation
     * 
     * 5. Create pull request for review
     * 
     * 6. After approval, merge to main
     */
    
    // Code ownership and responsibility matrix
    /*
     * TEAM RESPONSIBILITY MATRIX
     * ========================
     * 
     * Alice (Team Lead):
     * - Overall architecture
     * - Code review coordination
     * - Release planning
     * 
     * Bob (Backend Developer):
     * - Grade calculation algorithms
     * - Data persistence layer
     * - Performance optimization
     * 
     * Carol (Frontend Developer):
     * - User interface design
     * - Report generation views
     * - User experience
     * 
     * David (QA Engineer):
     * - Test plan creation
     * - Automated testing framework
     * - Bug tracking and verification
     * 
     * SHARED RESPONSIBILITIES:
     * - Code reviews (everyone reviews everyone)
     * - Documentation updates
     * - Daily standups
     * - Sprint retrospectives
     */
    
    // Interface definitions for coordinated development
    public interface GradeCalculationService {
        /**
         * Team contract: All implementations must follow this interface
         * This allows team members to work on different implementations
         * without breaking the existing codebase
         */
        double calculateFinalGrade(Student student, Course course);
        String getLetterGrade(double numericGrade);
        Map<String, Double> getGradeBreakdown(Student student, Course course);
    }
    
    // Coordination comments for parallel development
    public class StudentEnrollment {
        // TODO: @Bob - Implement database persistence after interface review
        // TODO: @Carol - Add UI validation once business rules are confirmed
        // TODO: @David - Create test scenarios for enrollment limits
        
        // NOTE: This method is used by both grade calculation and reporting
        // Any changes must be coordinated with both teams
        public void enrollStudent(Student student, Course course) {
            // Current implementation...
            // FIXME: @Alice - Review enrollment validation logic
        }
    }
}

// Conflict resolution in code
public class ConflictResolution {
    // Handling merge conflicts
    /*
     * MERGE CONFLICT RESOLUTION STEPS:
     * 
     * 1. Identify conflicting files:
     *    git status
     * 
     * 2. Open conflicting files, look for markers:
     *    <<<<<<< HEAD
     *    // Your code
     *    =======
     *    // Their code
     *    >>>>>>> feature-branch
     * 
     * 3. Discuss with team member who made the other change
     * 
     * 4. Choose best solution (may be combination)
     * 
     * 5. Test the merged code
     * 
     * 6. Commit the resolution:
     *    git add .
     *    git commit -m "Resolve merge conflict in GradeCalculator"
     */
    
    // Design disagreement resolution
    /*
     * DESIGN DISAGREEMENT: Grade Storage Format
     * 
     * Option A (Alice): Store as percentage (0-100)
     * - Pros: Easy to understand, direct display
     * - Cons: Loss of precision, harder to change scale
     * 
     * Option B (Bob): Store as fraction (0.0-1.0)
     * - Pros: Better precision, easier calculations
     * - Cons: Need conversion for display
     * 
     * RESOLUTION APPROACH:
     * 1. List pros/cons for each option
     * 2. Consider future requirements
     * 3. Create small prototype for each
     * 4. Team votes after discussion
     * 5. Document decision and rationale
     * 
     * DECISION: Use percentage (Option A)
     * RATIONALE: Stakeholders prefer percentage format,
     * and precision loss is acceptable for grade display
     */
}
```

#### 3. Meeting Deadlines and Milestones
```java
public class ProjectMilestones {
    
    // Milestone breakdown structure
    /*
     * PROJECT: Student Grade Management System
     * 
     * MILESTONE 1: Core Data Models (Week 1)
     * - Create Student class
     * - Create Course class  
     * - Create Grade class
     * - Basic validation
     * - Unit tests for models
     * 
     * MILESTONE 2: Basic Operations (Week 2)
     * - Student enrollment/withdrawal
     * - Grade entry and retrieval
     * - Course management
     * - Integration tests
     * 
     * MILESTONE 3: Grade Calculations (Week 3)
     * - Weighted grade formulas
     * - Letter grade conversion
     * - GPA calculation
     * - Edge case handling
     * 
     * MILESTONE 4: Reporting (Week 4)
     * - Student transcripts
     * - Course grade distributions
     * - Export functionality
     * - Report templates
     * 
     * MILESTONE 5: Testing & Polish (Week 5)
     * - Comprehensive testing
     * - Performance optimization
     * - Documentation completion
     * - Code cleanup
     */
    
    // Task estimation and tracking
    public class TaskEstimation {
        /**
         * ESTIMATION TECHNIQUES:
         * 
         * 1. Planning Poker:
         *    - Each team member estimates independently
         *    - Reveal estimates simultaneously
         *    - Discuss differences and re-estimate
         * 
         * 2. Three-Point Estimation:
         *    - Optimistic estimate (O)
         *    - Most likely estimate (M)  
         *    - Pessimistic estimate (P)
         *    - Final estimate = (O + 4M + P) / 6
         * 
         * EXAMPLE TASK: Implement Grade Calculator
         * - Optimistic: 4 hours (everything goes smoothly)
         * - Most likely: 8 hours (expected time)
         * - Pessimistic: 16 hours (many complications)
         * - Estimate: (4 + 4*8 + 16) / 6 = 8.7 hours
         */
        
        // Sprint planning example
        /*
         * SPRINT 1 PLANNING (2 weeks)
         * 
         * Team velocity: 40 story points (based on previous sprint)
         * Sprint goal: Complete core data models and basic operations
         * 
         * SELECTED USER STORIES:
         * - As a teacher, I want to create a course (3 points)
         * - As a teacher, I want to add students to course (5 points)
         * - As a teacher, I want to enter grades (8 points)
         * - As a student, I want to view my grades (5 points)
         * - As an admin, I want to export grade data (13 points)
         * - As a teacher, I want to calculate final grades (8 points)
         * 
         * Total: 42 points (slightly over capacity - need to adjust)
         * 
         * ADJUSTED PLAN:
         * - Move "export grade data" to next sprint (-13 points)
         * - Add "student enrollment validation" (5 points)
         * - Total: 34 points (under capacity, good buffer)
         */
    }
    
    // Daily standup structure
    /*
     * DAILY STANDUP FORMAT (15 minutes max)
     * 
     * Each team member answers:
     * 1. What did I accomplish yesterday?
     * 2. What will I work on today?
     * 3. Any blockers or impediments?
     * 
     * EXAMPLE:
     * Alice: "Yesterday I completed the Student class with validation.
     *         Today I'll work on the Course class.
     *         No blockers."
     * 
     * Bob: "Yesterday I started grade calculation logic.
     *       Today I'll finish the weighted average function.
     *       Blocked on: Need clarification on rounding rules."
     * 
     * Carol: "Yesterday I designed the grade entry UI.
     *        Today I'll implement the form validation.
     *        No blockers."
     * 
     * FOLLOW-UP ACTIONS:
     * - Alice will pair with Bob on rounding rules after standup
     * - Team will review UI designs in afternoon
     */
}
```

#### 4. Peer Performance Evaluation
```java
public class PeerEvaluation {
    
    // Peer review checklist
    /*
     * PEER REVIEW CRITERIA:
     * 
     * CODE QUALITY (1-5 scale):
     * - Readability and organization
     * - Following coding standards
     * - Appropriate comments and documentation
     * - Error handling and edge cases
     * - Performance considerations
     * 
     * COLLABORATION (1-5 scale):
     * - Responsive to feedback
     * - Helps others when needed
     * - Communicates clearly
     * - Meets deadlines and commitments
     * - Participates in team discussions
     * 
     * TECHNICAL SKILLS (1-5 scale):
     * - Problem-solving ability
     * - Knowledge of tools and technologies
     * - Testing and debugging skills
     * - Understanding of requirements
     * - Innovation and creativity
     */
    
    // Constructive feedback examples
    public class FeedbackExamples {
        /*
         * GOOD FEEDBACK (Specific, Actionable, Balanced):
         * 
         * ✓ "Your implementation of the grade calculator is well-structured. 
         *   The use of the Strategy pattern for different grading schemes is 
         *   excellent. For improvement, consider adding more detailed Javadoc 
         *   comments for the public methods, especially the calculateFinalGrade 
         *   method which has complex parameters."
         * 
         * ✓ "I appreciate how you handled the edge cases in the student 
         *   enrollment logic. The null checks and validation are thorough. 
         *   One suggestion: the method is getting long (40+ lines). Consider 
         *   extracting some validation logic into separate methods."
         * 
         * ✓ "Your test coverage is comprehensive and the test names clearly 
         *   describe what they're testing. Nice work! To make tests even 
         *   better, consider using parameterized tests for the multiple 
         *   grade boundary scenarios."
         * 
         * BAD FEEDBACK (Vague, Unhelpful):
         * 
         * ✗ "Code looks good"
         * ✗ "Needs improvement"
         * ✗ "I don't like this approach"
         * ✗ "This is wrong"
         */
        
        // Self-evaluation template
        /*
         * SELF-EVALUATION TEMPLATE:
         * 
         * 1. ACCOMPLISHMENTS THIS SPRINT:
         *    - Implemented grade calculation module
         *    - Created comprehensive test suite (95% coverage)
         *    - Helped Bob debug the enrollment issue
         *    - Updated documentation for new APIs
         * 
         * 2. AREAS FOR IMPROVEMENT:
         *    - Need to improve estimation accuracy (overestimated by 30%)
         *    - Could be more proactive in code reviews
         *    - Should ask for help sooner when stuck
         * 
         * 3. LEARNING GOALS FOR NEXT SPRINT:
         *    - Learn more about performance optimization
         *    - Improve understanding of database design
         *    - Practice pair programming techniques
         * 
         * 4. FEEDBACK FOR TEAM PROCESS:
         *    - Morning standups are helpful
         *    - Need better tooling for code reviews
         *    - Sprint retrospectives lead to good improvements
         */
    }
    
    // Code review best practices
    public class CodeReviewGuidelines {
        /**
         * BEFORE SUBMITTING FOR REVIEW:
         * 
         * 1. Self-review your code
         *    - Read through every line
         *    - Check for formatting consistency
         *    - Ensure tests pass locally
         * 
         * 2. Write clear pull request description
         *    - Explain what changes were made
         *    - Link to relevant tickets/issues
         *    - Include screenshots if UI changes
         * 
         * 3. Keep changes focused
         *    - One feature/fix per PR
         *    - Avoid unrelated formatting changes
         *    - Limit size (< 400 lines when possible)
         */
        
        /**
         * DURING CODE REVIEW:
         * 
         * 1. Focus on the right things
         *    - Logic and design decisions
         *    - Potential bugs or edge cases
         *    - Security implications
         *    - Performance issues
         * 
         * 2. Don't nitpick
         *    - Avoid style comments (use automated tools)
         *    - Focus on significant issues
         *    - Suggest alternatives, don't just criticize
         * 
         * 3. Ask questions
         *    - "Can you explain why you chose this approach?"
         *    - "Have you considered the case where...?"
         *    - "What happens if this fails?"
         */
    }
}
```

#### 5. Professional Behavior and Attitude
```java
public class ProfessionalBehavior {
    
    // Communication best practices
    /*
     * EFFECTIVE TEAM COMMUNICATION:
     * 
     * MEETINGS:
     * ✓ Arrive on time and prepared
     * ✓ Stay focused on agenda topics
     * ✓ Listen actively to others
     * ✓ Take notes and ask clarifying questions
     * ✓ Follow up on action items
     * 
     * EMAIL/CHAT:
     * ✓ Use clear, descriptive subject lines
     * ✓ Keep messages concise but complete
     * ✓ Use appropriate tone (professional but friendly)
     * ✓ Respond promptly to requests
     * ✓ Use @mentions sparingly and appropriately
     * 
     * CODE COMMENTS:
     * ✓ Be respectful and constructive
     * ✓ Explain "why" not just "what"
     * ✓ Use collaborative language ("we", "our")
     * ✓ Acknowledge good work
     */
    
    // Conflict resolution strategies
    public class ConflictResolution {
        /**
         * HANDLING DISAGREEMENTS:
         * 
         * 1. LISTEN FIRST
         *    - Understand the other person's perspective
         *    - Don't interrupt or immediately counter
         *    - Ask questions to clarify their position
         * 
         * 2. FOCUS ON FACTS
         *    - Discuss technical merits
         *    - Avoid personal attacks
         *    - Use data to support arguments
         * 
         * 3. FIND COMMON GROUND
         *    - Identify shared goals
         *    - Look for compromise solutions
         *    - Consider hybrid approaches
         * 
         * 4. ESCALATE WHEN NECESSARY
         *    - Bring in team lead for major decisions
         *    - Document the disagreement and options
         *    - Accept team decisions gracefully
         * 
         * EXAMPLE SCENARIO:
         * Disagreement on whether to use inheritance or composition
         * for the grade calculation hierarchy.
         * 
         * RESOLUTION PROCESS:
         * 1. Each person explains their reasoning (5 min each)
         * 2. Team discusses pros/cons of each approach (10 min)
         * 3. Team lead makes final decision based on discussion
         * 4. Decision is documented in code comments
         * 5. Everyone commits to implementing the chosen approach
         */
    }
    
    // Accountability and reliability
    public class Accountability {
        /*
         * BEING ACCOUNTABLE:
         * 
         * COMMITMENTS:
         * - Only commit to what you can deliver
         * - Communicate early if you'll miss deadlines
         * - Take ownership of your mistakes
         * - Follow through on promises
         * 
         * QUALITY:
         * - Test your code before submitting
         * - Review your work critically
         * - Accept feedback gracefully
         * - Fix bugs you introduce promptly
         * 
         * COMMUNICATION:
         * - Keep status updates honest and accurate
         * - Admit when you don't understand something
         * - Ask for help when needed
         * - Share knowledge with teammates
         * 
         * EXAMPLE ACCOUNTABILITY STATEMENT:
         * "I committed to completing the grade calculation feature
         * by Friday. On Wednesday, I realized I underestimated the
         * complexity of handling weighted grades. I immediately
         * communicated this to the team and worked with Alice to
         * adjust the timeline. We delivered a simplified version
         * on Friday and scheduled the advanced features for the
         * next sprint."
         */
    }
    
    // Continuous improvement mindset
    public class ContinuousImprovement {
        /**
         * GROWTH-ORIENTED BEHAVIORS:
         * 
         * LEARNING:
         * - Seek feedback actively
         * - Learn from mistakes without defensiveness
         * - Share knowledge with team members
         * - Stay updated with best practices
         * 
         * COLLABORATION:
         * - Help teammates when possible
         * - Participate in code reviews constructively
         * - Contribute to team retrospectives
         * - Suggest process improvements
         * 
         * REFLECTION:
         * - Review your work regularly
         * - Identify areas for improvement
         * - Set learning goals
         * - Track your progress
         * 
         * RETROSPECTIVE CONTRIBUTIONS:
         * "What went well: Our pair programming session on the
         * complex algorithm helped us catch bugs early.
         * 
         * What could improve: I should have asked for help sooner
         * when I was stuck on the database integration.
         * 
         * Action item: I'll set up a 'help needed' flag in our
         * task board when I'm stuck for more than 2 hours."
         */
    }
}
```

## Key Exam Tips

1. **Understand project lifecycle** - from requirements to delivery
2. **Know how to write clear specifications** and documentation
3. **Practice team communication** skills and terminology
4. **Understand different roles** in programming teams
5. **Know version control concepts** for team coordination
6. **Practice peer review** techniques and constructive feedback
7. **Understand agile methodologies** and team practices

## Common Team Programming Scenarios

### 1. Feature Development Workflow
```java
// Typical feature development process
/*
 * FEATURE: Add Grade Export Functionality
 * 
 * 1. PLANNING:
 *    - Team discusses requirements
 *    - Creates user stories
 *    - Estimates effort
 *    - Assigns team members
 * 
 * 2. DESIGN:
 *    - Design interface/API
 *    - Review with team
 *    - Create technical design doc
 *    - Get approval before coding
 * 
 * 3. IMPLEMENTATION:
 *    - Create feature branch
 *    - Write tests first (TDD)
 *    - Implement feature
 *    - Regular commits with clear messages
 * 
 * 4. REVIEW:
 *    - Self-review code
 *    - Create pull request
 *    - Address review comments
 *    - Get approval from reviewers
 * 
 * 5. INTEGRATION:
 *    - Merge to main branch
 *    - Deploy to staging
 *    - Test in staging environment
 *    - Deploy to production
 * 
 * 6. MONITORING:
 *    - Monitor for issues
 *    - Gather user feedback
 *    - Plan improvements
 */
```

### 2. Bug Fix Workflow
```java
// Bug fixing process
/*
 * BUG: Grade calculation incorrect for courses with missing assignments
 * 
 * 1. BUG REPORT:
 *    - Clear description of issue
 *    - Steps to reproduce
 *    - Expected vs actual behavior
 *    - Environment information
 * 
 * 2. INVESTIGATION:
 *    - Reproduce the bug locally
 *    - Identify root cause
 *    - Assess impact and priority
 *    - Estimate fix effort
 * 
 * 3. FIX:
 *    - Create hotfix branch
 *    - Write test that fails with bug
 *    - Implement fix
 *    - Verify test now passes
 * 
 * 4. VERIFICATION:
 *    - Test fix thoroughly
 *    - Check for regression issues
 *    - Get peer review
 *    - Update documentation if needed
 * 
 * 5. DEPLOYMENT:
 *    - Merge to main
 *    - Deploy to production
 *    - Monitor for fix effectiveness
 *    - Close bug report
 */
```

## Practice Scenarios

1. **Team Meeting Simulation**: Practice presenting your work to teammates
2. **Code Review Exercise**: Review peer's code and provide constructive feedback
3. **Merge Conflict Resolution**: Practice resolving conflicts with version control
4. **Sprint Planning**: Estimate tasks and plan sprint goals
5. **Documentation Challenge**: Write clear API documentation for team use
6. **Pair Programming**: Collaborate on solving a coding problem
7. **Retrospective Meeting**: Identify team improvements and action items

## Team Programming Best Practices Summary

### Communication:
- Write clear commit messages and documentation
- Communicate early about blockers or changes
- Be respectful in all interactions
- Ask questions when uncertain

### Collaboration:
- Review others' code constructively
- Help teammates when possible
- Share knowledge and best practices
- Follow agreed-upon coding standards

### Accountability:
- Meet deadlines and commitments
- Take ownership of your work
- Test thoroughly before submitting
- Be reliable in team interactions

### Continuous Improvement:
- Participate in retrospectives
- Learn from mistakes
- Stay updated with team practices
- Contribute to process improvements

Remember: Team programming is not just about writing code together—it's about effective communication, collaboration, and shared responsibility for project success.