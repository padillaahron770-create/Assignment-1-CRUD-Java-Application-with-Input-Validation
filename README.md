# Assignment-1-CRUD-Java-Application-with-Input-Validation

import java.io.Serializable;

public class Item implements Serializable {
    private String name;
    private String category;
    private double price;

    public Item(String name, String category, double price) {
        this.name = name;
        this.category = category;
        this.price = price;
    }

    public void update(String name, String category, double price) {
        this.name = name;
        this.category = category;
        this.price = price;
    }

    @Override
    public String toString() {
        return name + " | Category: " + category + " | Price: PHP " + price;
    }
}

import java.io.*;
import java.util.*;

public class InventoryApp {

    static final String FILE_NAME = "items.dat";
    static Scanner scanner = new Scanner(System.in);
    static ArrayList<Item> items = new ArrayList<>();

    public static void main(String[] args) {
        loadFromFile();

        while (true) {
            showMenu();
            int choice = getIntInput();

            switch (choice) {
                case 1 -> addItem();
                case 2 -> viewItems();
                case 3 -> updateItem();
                case 4 -> deleteItem();
                case 5 -> {
                    saveToFile();
                    System.out.println("Program terminated.");
                    System.exit(0);
                }
                default -> System.out.println("Invalid choice.");
            }
        }
    }

    static void showMenu() {
        System.out.println("\n=== ITEM INVENTORY SYSTEM ===");
        System.out.println("1. Add Item");
        System.out.println("2. View Items");
        System.out.println("3. Update Item");
        System.out.println("4. Delete Item");
        System.out.println("5. Exit");
        System.out.print("Select option: ");
    }
    
    static void addItem() {
        System.out.print("Item name: ");
        String name = scanner.nextLine().trim();

        System.out.print("Category: ");
        String category = scanner.nextLine().trim();

        System.out.print("Price: ");
        double price = getDoubleInput();

        items.add(new Item(name, category, price));
        saveToFile();
        System.out.println("Item added successfully.");
    }

    static void viewItems() {
        if (items.isEmpty()) {
            System.out.println("No items found.");
            return;
        }

        System.out.println("\n--- ITEM LIST ---");
        for (int i = 0; i < items.size(); i++) {
            System.out.println((i + 1) + ". " + items.get(i));
        }
    }

    static void updateItem() {
        viewItems();
        if (items.isEmpty()) return;

        System.out.print("Select item number to update: ");
        int index = getIntInput() - 1;

        if (index < 0 || index >= items.size()) {
            System.out.println("Invalid selection.");
            return;
        }

        System.out.print("New name: ");
        String name = scanner.nextLine().trim();

        System.out.print("New category: ");
        String category = scanner.nextLine().trim();

        System.out.print("New price: ");
        double price = getDoubleInput();

        items.get(index).update(name, category, price);
        saveToFile();
        System.out.println("Item updated successfully.");
    }

    static void deleteItem() {
        viewItems();
        if (items.isEmpty()) return;

        System.out.print("Select item number to delete: ");
        int index = getIntInput() - 1;

        if (index < 0 || index >= items.size()) {
            System.out.println("Invalid selection.");
            return;
        }

        items.remove(index);
        saveToFile();
        System.out.println("Item deleted successfully.");
    }

    static void saveToFile() {
        try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream(FILE_NAME))) {
            out.writeObject(items);
        } catch (IOException e) {
            System.out.println("Error saving file.");
        }
    }

    static void loadFromFile() {
        File file = new File(FILE_NAME);
        if (!file.exists()) return;

        try (ObjectInputStream in = new ObjectInputStream(new FileInputStream(FILE_NAME))) {
            items = (ArrayList<Item>) in.readObject();
        } catch (IOException | ClassNotFoundException e) {
            System.out.println("Error loading file.");
        }
    }

    static int getIntInput() {
        while (!scanner.hasNextInt()) {
            System.out.print("Enter a valid number: ");
            scanner.next();
        }
        int value = scanner.nextInt();
        scanner.nextLine();
        return value;
    }

    static double getDoubleInput() {
        while (!scanner.hasNextDouble()) {
            System.out.print("Enter a valid amount: ");
            scanner.next();
        }
        double value = scanner.nextDouble();
        scanner.nextLine();
        return value;
    }
}

