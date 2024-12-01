# Creational Design Patterns

Creational patterns deal with object creation mechanisms, trying to create objects in a manner suitable to the situation. These patterns help abstract the instantiation process, making it more flexible and dynamic.

## Singleton Pattern

```java title="Exercise.java" linenums="1"
// Exercise.java
// The Exercise class demonstrates how to use the Logger class to log messages of different severity levels. 

package Singleton;

import java.util.Scanner;

public class Exercise {

    // Do not modify the run method. It prompts the user for info, warning, and error messages and logs them accordingly. 
    public void run() {
        
        Logger logger = Logger.getInstance();
        Scanner sc = new Scanner(System.in);

        // Get an info message from the user
        System.out.print("Enter an info message: ");
        String infoMessage = sc.nextLine();
        
        // TODO: Log the info message using the appropriate logging method.
        logger.info(infoMessage);
        

        // Get a warning message from the user
        System.out.print("Enter a warning message: ");
        String warnMessage = sc.nextLine();
        
        // TODO: Log the warn message using the appropriate logging method.
        logger.warn(warnMessage);
        

        // Get an error message from the user
        System.out.print("Enter an error message: ");
        String errorMessage = sc.nextLine();
        
        // TODO: Log the error message using the appropriate logging method.
        logger.error(errorMessage);
        
        
        
        sc.close();
    }
}
```

```java title="Logger.java" linenums="1"
// Logger.java
// The Logger class implements the Singleton Pattern to provide a single point of access for logging messages throughout the application.

package Singleton;

import java.text.SimpleDateFormat;
import java.util.Date;

public class Logger {

    private static Logger instance;

    // Private constructor to prevent instantiation
    private Logger() { 
        
    }
    
    public static synchronized Logger getInstance() {
        if (instance == null) {
            instance = new Logger();
            
        }
        return instance;
    }

    public void info(String message) {
        log("INFO", message);
    }

    public void warn(String message) {
        log("WARN", message);
    }

    public void error(String message) {
        log("ERROR", message);
    }

    private void log(String level, String message) {
        String timestamp = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date());
        System.out.println(String.format("%s [%s]: %s", timestamp, level, message));
    }
}
```

## Builder Pattern

```java title="Exercise.java" linenums="1"
// Exercise.java
// This file gathers meal components from user input and constructs full and simple meals using the Builder design pattern.

package Builder;

import java.util.Scanner;

public class Exercise {

    // Do not modify the run method. It handles the meal construction process using user input and the Builder design pattern.
    public void run() {
        
        Scanner sc = new Scanner(System.in);
        
        // Get full meal components from user
        String fullMainDish = sc.nextLine();
        
        String fullSideDish = sc.nextLine();
        
        String fullDrink = sc.nextLine();
        
        String fullDessert = sc.nextLine();
        
        String fullAppetizer = sc.nextLine();
        
        // TODO: Construct a full meal using MealBuilder with the provided components.
        Meal meal = new MealBuilder(fullMainDish, fullSideDish, fullDrink)
                        .setDessert(fullDessert)
                        .setAppetizer(fullAppetizer)
                        .build();
        
                            
        System.out.println("Full Meal Summary:");
        
        // TODO: Print the summary of the constructed full meal.
        meal.printMealSummary();
    

        // Get simple meal components from user
        String simpleMainDish = sc.nextLine();

        String simpleSideDish = sc.nextLine();
        
        String simpleDrink = sc.nextLine();

        // TODO: Construct a simple meal using MealBuilder with the provided components.
        Meal basic_meal = new MealBuilder(fullMainDish, fullSideDish, fullDrink)
                        .build();
                            
        System.out.println("Simple Meal Summary:");
        
        // TODO: Print the summary of the constructed simple meal.
        basic_meal.printMealSummary();
        
        
        sc.close();
    }
}
```

```java title="MealBuilder.java" linenums="1"
// MealBuilder.java
// This file constructs a Meal object by setting mandatory components (main dish, side dish, drink) and optional components (dessert, appetizer) using the Builder design pattern.

package Builder;

public class MealBuilder {
    
    public String mainDish;
    public String sideDish;
    public String drink;
    public String dessert = "Default Dessert";
    public String appetizer = "Default Appetizer";

    public MealBuilder(String mainDish, String sideDish, String drink) {
        // TODO: Initialize MealBuilder components using the provided parameters.
        this.mainDish = mainDish;
        this.sideDish = sideDish;
        this.drink = drink;

    }

    public MealBuilder setDessert(String dessert) {
        // TODO: Initialize the MealBuilder dessert field with the provided dessert parameter.
        this.dessert = dessert;
        
        return this;
    }

    public MealBuilder setAppetizer(String appetizer) {
        // TODO: Initialize the MealBuilder appetizer field with the provided dessert parameter.
        
        this.appetizer = appetizer;
        return this;
    }

    public Meal build() {
        // TODO: Write the return statement to complete the object construction process.
        return Meal.getInstance(this);
    }
}

```

```java title="Meal.java" linenums="1"
// This file defines the Meal class, representing a meal's components and providing a method to print the summary.

package Builder;

public class Meal {
    
    private String mainDish;
    private String sideDish;
    private String drink;
    private String dessert;
    private String appetizer;

    private Meal(MealBuilder builder) {
        // TODO: Implement the Meal constructor to initialize Meal components from the MealBuilder.
            this.mainDish = builder.mainDish;
            this.sideDish = builder.sideDish;
            this.drink = builder.drink;
            this.dessert = builder.dessert;
            this.appetizer = builder.appetizer;
    }
    
    public static synchronized Meal getInstance(MealBuilder builder) {
        //TODO: Return a new instance of Meal using the provided MealBuilder
        return new Meal(builder);

    }

    public void printMealSummary() {
        System.out.println("Main Dish: " + mainDish);
        System.out.println("Side Dish: " + sideDish);
        System.out.println("Drink: " + drink);
        System.out.println("Dessert: " + dessert);
        System.out.println("Appetizer: " + appetizer);
    }
}

```

## Factory Pattern

```java title="Exercise.java" linenums="1"
// This class handles the creation of document instances based on user input.

package Factory;

import java.util.Scanner;

public class Exercise {
    
    // Do not modify the run method. It is designed to handle user input for creating document instances based on user commands.
    public void run() {
        
        Scanner sc = new Scanner(System.in);

        String documentType = sc.nextLine();

        try {
            // TODO: Create a document instance using the DocumentFactory based on user input
            Document document = DocumentFactory.createDocument(documentType);
            
            // TODO: Display the type of the created document instance
            document.displayType();
            
            
        } catch (IllegalArgumentException e) {
            System.out.println(e.getMessage());
        }
        
        sc.close();
    }
}
```

```java title="DocumentFactory.java" linenums="1"
// This factory class is responsible for creating instances of different document types.

package Factory;

public class DocumentFactory {
    
    public static Document createDocument(String type) {
        
        switch (type.toLowerCase()) {
            case "pdf":
                // TODO: Return a new instance of PDFDocument
                
                return new PDFDocument();
            case "word":
                // TODO: Return a new instance of WordDocument
                
                return new WordDocument();
            case "html":
                // TODO: Return a new instance of HTMLDocument
                
                return new HTMLDocument();
            default:
                throw new IllegalArgumentException("Unknown document type: " + type);
        }
    }
}
```

```java title="PDFDocument.java" linenums="1"
// This class represents a PDF document type.

package Factory;

public class PDFDocument extends Document {
    
    @Override
    public void displayType() {
        System.out.println("Creating a PDF Document");
    }
}
```

```java title="WordDocument.java" linenums="1"
// This class represents a Word document type.

package Factory;

public class WordDocument extends Document {
    
    @Override
    public void displayType() {
        System.out.println("Creating a Word Document");
    }
}
```

```java title="HTMLDocument.java" linenums="1"
// This class represents a HTML document type.

package Factory;

public class HTMLDocument extends Document {
    
    @Override
    public void displayType() {
        System.out.println("Creating an HTML Document");
    }
}
```

```java title="Document.java" linenums="1"
// This abstract class represents the structure for various document types.

package Factory;

public abstract class Document {
    public abstract void displayType();
}
```

## Abstract Factory Pattern

## Prototype Pattern

```java title="Exercise.java" linenums="1"
// This class demonstrates the use of the Prototype Design Pattern for cloning characters.

package Prototype;

import java.util.Scanner;

public class Exercise {
    
    // Do not modify the run method. It is designed to gather user input and manage character states.
    public void run() {
        
        Scanner sc = new Scanner(System.in);

        String warriorName = sc.nextLine();

        int health = sc.nextInt();

        int attackPower = sc.nextInt();

        int defense = sc.nextInt();

        Warrior warrior = new Warrior(warriorName, health, attackPower, defense);

        // TODO: Clone the original warrior character to create a new instance.
        Warrior clonedWarrior = warrior.clone();
        
        int clonedHealth = sc.nextInt();

        int clonedAttackPower = sc.nextInt();

        int clonedDefense = sc.nextInt();
        
        clonedWarrior.setHealth(clonedHealth);
        clonedWarrior.setAttackPower(clonedAttackPower);
        clonedWarrior.setDefense(clonedDefense);
        
        System.out.println("Original Character:");
        
        // TODO: Display the original warrior's attributes
       warrior.displayAttributes();
        
        

        System.out.println("\nCloned Character:");
        
        // TODO: Display the cloned warrior's attributes
        clonedWarrior.displayAttributes();
        

        sc.close();
    }
}

```

```java title="Warrior.java" linenums="1"
// This class represents a Warrior character in the Prototype Design Pattern.

package Prototype;

public class Warrior implements Character {
    
    private String name;
    private int health;
    private int attackPower;
    private int defense;

    public Warrior(String name, int health, int attackPower, int defense) {
        this.name = name;
        this.health = health;
        this.attackPower = attackPower;
        this.defense = defense;
    }

    @Override
    public Warrior clone() {
        // TODO: Return a new Warrior instance with the given set of attributes.
        return new Warrior(name, health, attackPower, defense);
        
    }

    @Override
    public void displayAttributes() {
        System.out.println("Warrior - Name: " + name + ", Health: " + health + ", Attack Power: " + attackPower + ", Defense: " + defense);
    }

    // Getters
    public String getName() {
        return name;
    }

    public int getHealth() {
        return health;
    }

    public int getAttackPower() {
        return attackPower;
    }

    public int getDefense() {
        return defense;
    }

    // Setters
    public void setName(String name) {
        this.name = name;
    }

    public void setHealth(int health) {
        this.health = health;
    }

    public void setAttackPower(int attackPower) {
        this.attackPower = attackPower;
    }

    public void setDefense(int defense) {
        this.defense = defense;
    }
}
```

```java title="Mage.java" linenums="1"
// This class represents a Mage character in the Prototype Design Pattern.

package Prototype;

public class Mage implements Character {
    
    private String name;
    private int health;
    private int attackPower;
    private int defense;

    public Mage(String name, int health, int attackPower, int defense) {
        this.name = name;
        this.health = health;
        this.attackPower = attackPower;
        this.defense = defense;
    }

    @Override
    public Mage clone() {
        // TODO: Return a new Mage instance with the given set of attributes.
        return new Mage(name, health, attackPower, defense);
        
    }

    @Override
    public void displayAttributes() {
        System.out.println("Mage - Name: " + name + ", Health: " + health + ", Attack Power: " + attackPower + ", Defense: " + defense);
    }

    // Getters
    public String getName() {
        return name;
    }

    public int getHealth() {
        return health;
    }

    public int getAttackPower() {
        return attackPower;
    }

    public int getDefense() {
        return defense;
    }

    // Setters
    public void setName(String name) {
        this.name = name;
    }

    public void setHealth(int health) {
        this.health = health;
    }

    public void setAttackPower(int attackPower) {
        this.attackPower = attackPower;
    }

    public void setDefense(int defense) {
        this.defense = defense;
    }
}
```

```java title="Archer.java" linenums="1"
// This class represents a Archer character in the Prototype Design Pattern.

package Prototype;

public class Archer implements Character {
    
    private String name;
    private int health;
    private int attackPower;
    private int defense;

    public Archer(String name, int health, int attackPower, int defense) {
        this.name = name;
        this.health = health;
        this.attackPower = attackPower;
        this.defense = defense;
    }

    @Override
    public Archer clone() {
        // TODO: Return a new Archer instance with the given set of attributes.
        return new Archer(name, health, attackPower, defense);
        
    }

    @Override
    public void displayAttributes() {
        System.out.println("Archer - Name: " + name + ", Health: " + health + ", Attack Power: " + attackPower + ", Defense: " + defense);
    }

    // Getters
    public String getName() {
        return name;
    }

    public int getHealth() {
        return health;
    }

    public int getAttackPower() {
        return attackPower;
    }

    public int getDefense() {
        return defense;
    }

    // Setters
    public void setName(String name) {
        this.name = name;
    }

    public void setHealth(int health) {
        this.health = health;
    }

    public void setAttackPower(int attackPower) {
        this.attackPower = attackPower;
    }

    public void setDefense(int defense) {
        this.defense = defense;
    }
}

```

```java title="Character.java" linenums="1"
// This interface defines the behavior for cloning and displaying attributes of character objects.

package Prototype;

public interface Character {
    Character clone();
    void displayAttributes();
}

```