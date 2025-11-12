import java.io.FileWriter;
import java.io.IOException;
import java.util.Scanner;

class Student {
    String name;
    int[] marks;
    double average;
    char grade;

    Student(String name, int subjectCount) {
        this.name = name;
        this.marks = new int[subjectCount];
    }

    void calculateAverage() {
        int total = 0;
        for (int mark : marks) total += mark;
        this.average = (double) total / marks.length;
        assignGrade();
    }

    void assignGrade() {
        if (average >= 85) grade = 'A';
        else if (average >= 70) grade = 'B';
        else if (average >= 55) grade = 'C';
        else if (average >= 40) grade = 'D';
        else grade = 'F';
    }
}

public class StudentPerformanceAnalyzer {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of students: ");
        int n = sc.nextInt();
        System.out.print("Enter number of subjects: ");
        int m = sc.nextInt();
        sc.nextLine(); // consume newline

        Student[] students = new Student[n];

        for (int i = 0; i < n; i++) {
            System.out.print("\nEnter student name: ");
            String name = sc.nextLine();
            students[i] = new Student(name, m);
            System.out.println("Enter marks for " + name + ":");
            for (int j = 0; j < m; j++) {
                System.out.print("Subject " + (j + 1) + ": ");
                students[i].marks[j] = sc.nextInt();
            }
            sc.nextLine();
            students[i].calculateAverage();
        }

        generateReport(students);
    }

    static void generateReport(Student[] students) {
        double totalAverage = 0;
        double highest = 0, lowest = 100;
        String topStudent = "", lowStudent = "";

        for (Student s : students) {
            totalAverage += s.average;
            if (s.average > highest) { highest = s.average; topStudent = s.name; }
            if (s.average < lowest) { lowest = s.average; lowStudent = s.name; }
        }

        double classAverage = totalAverage / students.length;

        System.out.println("\n========= STUDENT PERFORMANCE REPORT =========");
        for (Student s : students) {
            System.out.printf("%-15s | Avg: %.2f | Grade: %c%n", s.name, s.average, s.grade);
        }
        System.out.println("==============================================");
        System.out.printf("Class Average: %.2f%n", classAverage);
        System.out.println("Top Performer : " + topStudent + " (" + highest + ")");
        System.out.println("Lowest Performer: " + lowStudent + " (" + lowest + ")");
        System.out.println("Overall Class Performance: " + classifyPerformance(classAverage));

        saveToFile(students, classAverage, topStudent, lowStudent);
    }

    static String classifyPerformance(double avg) {
        if (avg >= 85) return "Excellent";
        else if (avg >= 70) return "Good";
        else if (avg >= 55) return "Average";
        else return "Needs Improvement";
    }

    static void saveToFile(Student[] students, double avg, String top, String low) {
        try (FileWriter writer = new FileWriter("report.txt")) {
            writer.write("=== STUDENT PERFORMANCE REPORT ===\n");
            for (Student s : students) {
                writer.write(String.format("%s | Avg: %.2f | Grade: %c%n", s.name, s.average, s.grade));
            }
            writer.write("\nClass Average: " + avg + "\n");
            writer.write("Top Performer: " + top + "\n");
            writer.write("Lowest Performer: " + low + "\n");
            writer.write("Overall Class Performance: " + classifyPerformance(avg) + "\n");
            System.out.println("\nReport saved successfully to report.txt");
        } catch (IOException e) {
            System.out.println("Error writing file: " + e.getMessage());
        }
    }
}
