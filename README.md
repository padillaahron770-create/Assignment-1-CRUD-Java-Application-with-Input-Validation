# Assignment-1-CRUD-Java-Application-with-Input-Validation

import java.io.Serializable;

public class Student implements Serializable {
    int id;
    String name;

    public Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public String toString() {
        return "ID: " + id + " | Name: " + name;
    }
}

import java.io.*;
import java.util.ArrayList;

public class StudentFile {

    static String filename = "students.dat";

    public static ArrayList<Student> readFile() {
        try {
            ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filename));
            return (ArrayList<Student>) ois.readObject();
        } catch (Exception e) {
            return new ArrayList<>();
        }
    }

    public static void writeFile(ArrayList<Student> students) {
        try {
            ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filename));
            oos.writeObject(students);
        } catch (Exception e) {
            System.out.println("Error saving file.");
        }
    }
}

import java.util.ArrayList;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        int choice;

        do {
            System.out.println("\n=== SIMPLE CRUD MENU ===");
            System.out.println("1. Add");
            System.out.println("2. View");
            System.out.println("3. Update");
            System.out.println("4. Delete");
            System.out.println("5. Exit");
            System.out.print("Choice: ");

            while (!sc.hasNextInt()) {
                System.out.print("Enter number only: ");
                sc.next();
            }
            choice = sc.nextInt();

            switch (choice) {
                case 1 -> add(sc);
                case 2 -> view();
                case 3 -> update(sc);
                case 4 -> delete(sc);
                case 5 -> System.out.println("Goodbye!");
                default -> System.out.println("Invalid choice!");
            }
        } while (choice != 5);
    }

    static void add(Scanner sc) {
        ArrayList<Student> students = StudentFile.readFile();

        System.out.print("Enter ID: ");
        int id = sc.nextInt();
        sc.nextLine();

        System.out.print("Enter Name: ");
        String name = sc.nextLine();

        students.add(new Student(id, name));
        StudentFile.writeFile(students);
        System.out.println("Added successfully!");
    }

    static void view() {
        ArrayList<Student> students = StudentFile.readFile();

        if (students.isEmpty()) {
            System.out.println("No records found.");
        } else {
            for (Student s : students) {
                System.out.println(s);
            }
        }
    }

    static void update(Scanner sc) {
        ArrayList<Student> students = StudentFile.readFile();

        System.out.print("Enter ID to update: ");
        int id = sc.nextInt();
        sc.nextLine();

        boolean found = false;

        for (Student s : students) {
            if (s.id == id) {
                System.out.print("New Name: ");
                s.name = sc.nextLine();
                found = true;
                break;
            }
        }

        if (found) {
            StudentFile.writeFile(students);
            System.out.println("Updated successfully!");
        } else {
            System.out.println("ID not found.");
        }
    }

    static void delete(Scanner sc) {
        ArrayList<Student> students = StudentFile.readFile();

        System.out.print("Enter ID to delete: ");
        int id = sc.nextInt();

        boolean removed = students.removeIf(s -> s.id == id);

        if (removed) {
            StudentFile.writeFile(students);
            System.out.println("Deleted successfully!");
        } else {
            System.out.println("ID not found.");
        }
    }
}
