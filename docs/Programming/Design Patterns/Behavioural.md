# Behavioural Design Patterns

Behavioral patterns focus on the interaction between objects, their communication, and the responsibilities of objects. These patterns help make complex communication between objects more flexible and efficient.

## Memento Pattern

!!! abstract "Purpose"
    Primary Purpose of Memento Pattern is to save and store previous state of an object. The Caretaker is responsible for storing and restoring the Memento without modifying it. <br>
    In this blog, we will explore how the Memento Pattern is implemented in Java through an example of a simple Graphic Editor that can save and undo shape modifications.

<h4>Key Components of the Memento Pattern:</h4> 
`Originator`: This is the object whose state is being saved or restored. In our case, it’s the GraphicEditor class.<br>
`Memento`: This object stores the state of the Originator. In our example, it is the EditorMemento class.<br>
`Caretaker`: This class keeps the history of mementos and is responsible for saving and restoring states. In our example, the Caretaker class handles the history of shapes.

<h4>The GraphicEditor Class</h4>
The GraphicEditor class represents the originator. It stores information about the shape being drawn, such as its type, position, color, and size. It also has methods to save and restore its state using the EditorMemento class.

=== "Java"
    ```java title="GraphicEditor.java" linenums="1"
    // The GraphicEditor class manages the properties of shapes and provides functionality to save and restore their state using the Memento pattern.

    package Memento;

    public class GraphicEditor {
        
        private String shapeType;
        private int x;
        private int y;
        private String color;
        private int size;

        public EditorMemento save() {
            // TODO: Create and return a new memento that captures the current state of the shape attributes.
            return new EditorMemento(shapeType, x, y, color, size);
            
        }

        public void restore(EditorMemento memento) {
            // TODO: Restore the shape attributes from the provided memento, updating the graphic editor's state.
            shapeType = memento.getShapeType();
            x = memento.getX();
            y = memento.getY();
            color = memento.getColor();
            size = memento.getSize();
        }

        public String getShape() {
            return "Shape: " + shapeType + ", Position: (" + x + ", " + y + "), Color: " + color + ", Size: " + size;
        }

        public void setShape(String shapeType, int x, int y, String color, int size) {
            this.shapeType = shapeType;
            this.x = x;
            this.y = y;
            this.color = color;
            this.size = size;
        }
    }
    ```
=== "Python"
    ```python title="GraphicEditor.py" linenums="1"
    from EditorMemento import EditorMemento

    class GraphicEditor:
        def __init__(self):
            self.shape_type = None
            self.x = 0
            self.y = 0
            self.color = None
            self.size = 0

        def save(self):
            # Create and return a new memento capturing the current state
            return EditorMemento(self.shape_type, self.x, self.y, self.color, self.size)

        def restore(self, memento):
            # Restore the state from the provided memento
            self.shape_type = memento.shape_type
            self.x = memento.x
            self.y = memento.y
            self.color = memento.color
            self.size = memento.size

        def get_shape(self):
            return f"Shape: {self.shape_type}, Position: ({self.x}, {self.y}), Color: {self.color}, Size: {self.size}"

        def set_shape(self, shape_type, x, y, color, size):
            self.shape_type = shape_type
            self.x = x
            self.y = y
            self.color = color
            self.size = size
    ```


<h4>The EditorMemento Class</h4>
The EditorMemento class captures the state of a shape. It is an immutable object that holds the shape's properties and provides getter methods to retrieve these values.

=== "Java"
    ``` java title="EditorMemento.Java" linenums="1"
    // The EditorMemento class stores the state of a shape, allowing for the preservation and restoration of its attributes in the Memento pattern.

    package Memento;

    public class EditorMemento {
        
        private final String shapeType;
        private final int x;
        private final int y;
        private final String color;
        private final int size;

        public EditorMemento(String shapeType, int x, int y, String color, int size) {
            // TODO: Initialize the shape's attributes with the provided parameters.
            this.shapeType = shapeType;
            this.x = x ;
            this.y = y ;
            this.color = color;
            this.size = size;
            
            
        }
        
        public String getShapeType() {
            return shapeType;
        }

        public int getX() {
            return x;
        }

        public int getY() {
            return y;
        }

        public String getColor() {
            return color;
        }

        public int getSize() {
            return size;
        }
    }
    ```
=== "Python"
    ```python title="EditorMemento.py" linenums="1"
    class EditorMemento:
    def __init__(self, shape_type, x, y, color, size):
        # Store the shape's attributes
        self.shape_type = shape_type
        self.x = x
        self.y = y
        self.color = color
        self.size = size
    ```

<h4>The Caretaker Class</h4>
The Caretaker class is responsible for managing the history of saved states. It uses a Stack to keep track of the mementos. The caretaker does not modify the state of the memento, it only stores and retrieves it when necessary.

=== "Java"
    ```java title="Caretaker.Java" linenums="1"
    // The Caretaker class manages the history of shape states, allowing for saving and undoing changes in the Memento pattern.

    package Memento;

    import java.util.Stack;

    public class Caretaker {
        
        private final Stack<EditorMemento> history = new Stack<>();

        public void saveState(GraphicEditor graphicEditor) {
            // TODO: Save the current state of the graphic editor by pushing its memento onto the history stack.
            history.push(graphicEditor.save());
            
        }

        public void undo(GraphicEditor graphicEditor){
            // TODO: Restore the last saved state of the graphic editor if history is not empty.
            if (!history.empty()){
                history.pop();
                graphicEditor.restore(history.pop());
            }
            
        }
    }
    ```
=== "Python"
    ```python title="Caretaker.py" linenums="1"
    class Caretaker:
    def __init__(self):
        # Stack to maintain history of states
        self.history = []

    def save_state(self, graphic_editor):
        # Save the current state by appending its memento to the history
        self.history.append(graphic_editor.save())

    def undo(self, graphic_editor):
        # Restore the last saved state if history is not empty
        if self.history:
            self.history.pop()  # Discard the latest state
            if self.history:  # Check if there’s another state to revert to
                graphic_editor.restore(self.history[-1])
    ```

<h4>The Exercise Class (Main Application)</h4>
The Exercise class is responsible for interacting with the user. It collects the shape's attributes and uses the GraphicEditor and Caretaker to manage and save the state.

=== "Java"
    ```java title="Exercise.java" linenums="1"
    // The Exercise class allows users to input shape attributes and provides functionality to manage these shapes using the Memento pattern.

    package Memento;
    import java.util.Scanner;
    public class Exercise {
        
        // Do not modify the run method. It is designed to gather user input and manage shape states.
        public void run() {
            Scanner sc = new Scanner(System.in);
            GraphicEditor graphicEditor = new GraphicEditor();
            Caretaker caretaker = new Caretaker();

            for (int i = 0; i < 3; i++) {
                String shape = sc.next(); 
                int x = sc.nextInt();     
                int y = sc.nextInt();     
                String color = sc.next(); 
                int size = sc.nextInt(); 

                // TODO: Update the graphic editor with the new shape attributes from user input.
                graphicEditor.setShape(shape, x, y, color, size);

                // TODO: Save the current state of the graphic editor to the history
                caretaker.saveState(graphicEditor);
            }
            sc.close();

            // TODO: Implement the undo operation to revert to the previous shape state
            caretaker.undo(graphicEditor);
            // TODO: Output the current shape attributes after the undo operation to verify the restored state.
            System.out.println(graphicEditor.getShape());
            
        }
    }
    ```

=== "Python"
    ```python title="Exercise.py" linenums="1"
    from GraphicEditor import GraphicEditor
    from Caretaker import Caretaker

    class Exercise:
        def run(self):
            graphic_editor = GraphicEditor()
            caretaker = Caretaker()

            for _ in range(3):
                # Take input from the user
                shape = input("Enter shape type: ")  
                x = int(input("Enter x-coordinate: "))  
                y = int(input("Enter y-coordinate: "))  
                color = input("Enter color: ")  
                size = int(input("Enter size: "))  

                # Update the graphic editor with new shape attributes
                graphic_editor.set_shape(shape, x, y, color, size)

                # Save the current state to the history
                caretaker.save_state(graphic_editor)

            # Undo the last operation
            caretaker.undo(graphic_editor)

            # Print the current state after undo
            print(graphic_editor.get_shape())

    if __name__ == "__main__":
        Exercise().run()
    ```

<h4> How the Memento Pattern Works in This Example </h4>
`Setting the Shape:` The user inputs the shape's attributes (type, position, color, and size). These attributes are set in the GraphicEditor object.<br>
`Saving State:` Each time the user changes the shape, the current state is saved by calling the saveState method of the Caretaker class. This stores the state in a Stack.<br>
`Undo Operation:` The user can undo the changes by calling the undo method of the Caretaker class. This restores the previous shape state from the stack, effectively rolling back to an earlier state.<br>

<h4> Benefits of Using the Memento Pattern </h4>
`Encapsulation Preservation:` The state of an object is captured and stored outside of the object, allowing for state restoration without exposing the internal structure.<br>
`Undo/Redo Operations:` The Memento Pattern provides an easy way to implement undo and redo functionality.<br>
`Separation of Concerns:` The Caretaker class is responsible for managing the state history, allowing the GraphicEditor class to focus solely on the editing functionality.


## Observer Pattern

```java title="Exercise.java" linenums="1"
// The Exercise class simulates stock price updates, registers investors, and removes an observer after the 4th update.

package Observer;

import java.util.Scanner;

public class Exercise {
    
    // Do not modify the run method. It is designed to handle user input, manage stock price updates, and control the observer notification process.
    public void run() {
        
        Scanner sc = new Scanner(System.in);
        
        double priceChangeThreshold = sc.nextDouble();
        StockMarket stockMarket = new StockMarket(priceChangeThreshold);

        InvestorA investorA = new InvestorA();
        InvestorB investorB = new InvestorB();
        
        // TODO: Register Investor A as an observer to receive stock updates.
        stockMarket.registerObserver(investorA);
        
        
        // TODO: Register Investor B as an observer to receive stock updates.
        stockMarket.registerObserver(investorB);

        int updates = sc.nextInt();
        
        for (int i = 1; i <= updates; i++) {
            
            if(i == 5) {
                // TODO: Remove Investor B from receiving notifications after the 4th update.
                stockMarket.removeObserver(investorB);
                
            }
            
            String stockSymbol = sc.next();
            double newPrice = sc.nextDouble();
            double oldPrice = sc.nextDouble();
            
            // TODO: Update the stock price and notify observers.
            stockMarket.setStockPrice(stockSymbol, newPrice, oldPrice);

            
        }
        sc.close();
    }
}

```

```java title="StockMarket.java" linenums="1"
// The StockMarket class tracks stock price changes and notifies observers if the change exceeds a threshold.

package Observer;

import java.util.ArrayList;
import java.util.List;

public class StockMarket implements Subject {
    
    private final List<Observer> observers;
    private final double priceChangeThreshold;

    public StockMarket(double priceChangeThreshold) {
        // TODO: Initialize the list of observers to keep track of registered observers.
        observers = new ArrayList<>();
        
    
        this.priceChangeThreshold = priceChangeThreshold;
    }

    @Override
    public void registerObserver(Observer o) {
        // TODO: Add observer to the list of observers
        observers.add(o);
        
    }

    @Override
    public void removeObserver(Observer o) {
        // TODO: Remove observer from the list of observers
        observers.remove(o);
        
    }

    @Override
    public void notifyObservers(String stockSymbol, double newPrice) {
        for (Observer observer : observers) {
            // TODO: Inform each observer about the updated stock price.
            observer.update(stockSymbol, newPrice);
            
        }
    }

    public void setStockPrice(String stockSymbol, double newPrice, double oldPrice) {
        double priceChange = Math.abs(newPrice - oldPrice) / oldPrice * 100;
        if (priceChange >= priceChangeThreshold) {
            // TODO: Notify observers if the price change exceeds the threshold
            notifyObservers(stockSymbol, newPrice);
            
        }
    }
}
```

```java title="InvestorA.java" linenums="1"
// The InvestorA class implements the Observer interface and receives stock price updates.

package Observer;

public class InvestorA implements Observer {
    
    @Override
    public void update(String stockSymbol, double newPrice) {
        System.out.println("Investor A notified: Stock " + stockSymbol + " has a new price: $" + newPrice);
    }
}
```

```java title="InvestorB.java" linenums="1"
// The InvestorB class implements the Observer interface and receives stock price updates.

package Observer;

public class InvestorB implements Observer {
    
    @Override
    public void update(String stockSymbol, double newPrice) {
        System.out.println("Investor B notified: Stock " + stockSymbol + " has a new price: $" + newPrice);
    }
}
```

```java title="Subject.java" linenums="1"
// The Subject interface defines methods for registering, removing, and notifying observers about stock price changes.

package Observer;

public interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers(String stockSymbol, double newPrice);
}
```

```java title="observer.java" linenums="1"
// The Observer interface defines the update method for receiving stock price change notifications.

package Observer;

public interface Observer {
    void update(String stockSymbol, double newPrice);
}
```

## Strategy Pattern

```java title="Exercise.java" linenums="1"
// Exercise.java
// This class manages the formatting process of a document using various text formatting strategies.

package Strategy;

import java.util.Scanner;

public class Exercise {
    
    // Do not modify the run method. It facilitates the formatting process of a document using different formatting strategies.
    public void run() {
        
        Scanner sc = new Scanner(System.in);
        Document document = new Document();
        
        String userInput = sc.nextLine();
        document.setContent(userInput);

        // Using PlainTextFormatter
        // TODO: Set the formatter for the document to PlainTextFormatter.
        PlainTextFormatter plainText = new PlainTextFormatter();
        document.setFormatter(plainText);
        
        System.out.println("Plain Text:");
        document.display();

        // Using HTMLFormatter
        // TODO: Set the formatter for the document to HTMLFormatter.
        HTMLFormatter htmlText = new HTMLFormatter();
        document.setFormatter(htmlText);
        
        
        System.out.println("HTML Format:");
        document.display();

        // Using MarkdownFormatter
        // TODO: Set the formatter for the document to MarkdownFormatter.
        MarkdownFormatter markdownText = new MarkdownFormatter();
        document.setFormatter(markdownText);
        
        
        System.out.println("Markdown Format:");
        document.display(); 
        
        sc.close();
    }
}
```

```java title="Document.java" linenums="1"
// Document.java
// This class represents a document that can have its content formatted using different strategies.

package Strategy;

public class Document {
    
    private String content;
    private TextFormatter formatter;
    
    public void setContent(String content) {
        this.content = content;
    }

    public void setFormatter(TextFormatter formatter) {
        this.formatter = formatter;
    }

    public void display() {
        // TODO: Print the formatted content using the chosen formatter.
        System.out.println(formatter.format(this.content));
        
    }
}
```

```java title="PlainTextFormatter.java" linenums="1"
// PlainTextFormatter.java
// This class implements the TextFormatter interface to format text as plain text.

package Strategy;

public class PlainTextFormatter implements TextFormatter {
    
    @Override
    public String format(String text) {
        // TODO: Return the input text without any formatting.
        return text;
        
    }
}
```

```java title="HTMLFormatter.java" linenums="1"
// HTMLFormatter.java
// This class implements the TextFormatter interface to format the text as HTML.

package Strategy;

public class HTMLFormatter implements TextFormatter {
    
    @Override
    public String format(String text) {
        // TODO: Wrap the input text in HTML tags: "<html><body>" and "</body></html>".
        return "<html><body>" + text + "</body></html>";
    }
}
```

```java title="MarkdownFormatter.java" linenums="1"
// MarkdownFormatter.java
// This class implements the TextFormatter interface to format text using Markdown syntax.

package Strategy;

public class MarkdownFormatter implements TextFormatter {
    
    @Override
    public String format(String text) {
        // TODO: Wrap the input text in Markdown syntax: "**" and "**".
        return "**" + text + "**";
        
    }
}
```

```java title="TextFormatter.java" linenums="1"
// TextFormatter.java
// This Interface defines a contract for text formatting strategies.

package Strategy;

public interface TextFormatter {
    String format(String text);
}
```

## Command Pattern

```java title="Exercise.java" linenums="1"
// This class is responsible for creating devices, commands, and a remote control to demonstrate the command pattern functionality.

package Command;

public class Exercise {
    
    //Do not modify the run method; it is designed to manage command execution and control the devices.
    public void run () {
        
        // Create devices
        Light light = new Light();
        Fan fan = new Fan();

        // Create commands
        Command lightOn = new LightCommands.LightOnCommand(light);
        Command lightOff = new LightCommands.LightOffCommand(light);
        Command fanOn = new FanCommands.FanOnCommand(fan);
        Command fanOff = new FanCommands.FanOffCommand(fan);

        // Create remote control
        // TODO: Instantiate the RemoteControl object to manage commands.
        RemoteControl remoteControl = new RemoteControl();

        
        // TODO: Set the command for turning the light on using the LightOnCommand using remoteControl object.
        remoteControl.setLightOnCommand(lightOn);
        
        
        // TODO: Set the command for turning off the light using LightOffCommand using remoteControl object.
        remoteControl.setLightOffCommand(lightOff);
        
        
        // TODO: Set the command for turning on the fan using FanOnCommand using remoteControl object.
        remoteControl.setFanOnCommand(fanOn);
        
        
        // TODO: Set the command for turning off the fan using FanOffCommand using remoteControl object.
        remoteControl.setFanOffCommand(fanOff);
        

        // Test the functionality
        // TODO: Press the button to turn on the light and verify the output.
        remoteControl.pressLightOnButton();
        
        
        // TODO: Press the button to turn off the light and verify the output.
        remoteControl.pressLightOffButton();
        
        // TODO: Press the button to turn on the fan and verify the output.
        remoteControl.pressFanOnButton();
        
        // TODO: Press the button to turn off the fan and verify the output.
        remoteControl.pressFanOffButton();

    }
}
```

```java title="RemoteControl.java" linenums="1"
// This class acts as a remote control, allowing the execution of commands for controlling devices like lights and fans.

package Command;

public class RemoteControl {
    
    private Command lightOnCommand;
    private Command lightOffCommand;
    private Command fanOnCommand;
    private Command fanOffCommand;

    public void setLightOnCommand(Command command) {
        this.lightOnCommand = command;
    }

    public void setLightOffCommand(Command command) {
        this.lightOffCommand = command;
    }

    public void setFanOnCommand(Command command) {
        this.fanOnCommand = command;
    }

    public void setFanOffCommand(Command command) {
        this.fanOffCommand = command;
    }

    public void pressLightOnButton() {
        if (lightOnCommand != null) {
            lightOnCommand.execute();
        }
    }

    public void pressLightOffButton() {
        if (lightOffCommand != null) {
            lightOffCommand.execute();
        }
    }

    public void pressFanOnButton() {
        if (fanOnCommand != null) {
            fanOnCommand.execute();
        }
    }

    public void pressFanOffButton() {
        if (fanOffCommand != null) {
            fanOffCommand.execute();
        }
    }
}
```

```java title="LightCommands.java" linenums="1"
// This class contains command implementations for controlling the light, including turning it on and off.

package Command;

public class LightCommands {
    
    public static class LightOnCommand implements Command {
        
        private Light light;

        public LightOnCommand(Light light) {
            this.light = light;
        }

        //TODO: Override the execute() method from the Command interface and Implement the logic to turn on the light when this command is executed.
        public void execute(){
            light.turnOn();
        }
        
        
    }

    public static class LightOffCommand implements Command {
        
        private Light light;

        public LightOffCommand(Light light) {
            this.light = light;
        }

        //TODO: Override the execute() method from the Command interface and Implement the logic to turn off the light when this command is executed.
        public void execute(){
            light.turnOff();
        }
    
    }
}
```

```java title="FanCommands.java" linenums="1"
// This class contains command implementations for controlling the fan, including turning it on and off.

package Command;

public class FanCommands {

    public static class FanOnCommand implements Command {
        
        private Fan fan;

        public FanOnCommand(Fan fan) {
            this.fan = fan;
        }

        //TODO: Override the execute() method from the Command interface and Implement the logic to turn on the fan when this command is executed.
        public void execute(){
            fan.turnOn();
        }
        
        
    }

    public static class FanOffCommand implements Command {
        
        private Fan fan;

        public FanOffCommand(Fan fan) {
            this.fan = fan;
        }

        //TODO: Override the execute() method from the Command interface and Implement the logic to turn off the fan when this command is executed.
        
        public void execute(){
            fan.turnOff();
        }
    }
}
```

```java title="Light.java" linenums="1"
// This class represents a light that can be turned on or off, providing the corresponding output.

package Command;

public class Light {
    
    public void turnOn() {
        System.out.println("The light is on.");
    }

    public void turnOff() {
        System.out.println("The light is off.");
    }
}
```

```java title="Fan.java" linenums="1"
// This class represents a fan that can be turned on or off, providing the corresponding output.

package Command;

public class Fan {
    
    public void turnOn() {
        System.out.println("The fan is on.");
    }

    public void turnOff() {
        System.out.println("The fan is off.");
    }
}
```

```java title="Command.java" linenums="1"
// This interface defines a command that can be executed, requiring an implementation of the execute method.

package Command;

public interface Command {
    void execute();
}
```

## Template Method Pattern


```java title="Exercise.java" linenums="1"
// This class is responsible for executing the report generation process using different report types.

package Template;

import java.util.Scanner;

public class Exercise {
    
    // Do not modify the run method. It manages the report generation process for various report types.
    public void run() {
        
        Scanner sc = new Scanner(System.in);
        
        // Generate Sales Report
        ReportTemplate salesReport = new SalesReport(sc);
        System.out.println("Generating Sales Report:");
        
        // TODO: Generate the Sales Report by calling the generateReport() method.
        salesReport.generateReport();
        
        
        // Generate Employee Report
        ReportTemplate employeeReport = new EmployeeReport(sc);
        System.out.println("Generating Employee Report:");
        
        // TODO: Generate the Employee Report by calling the generateReport() method.
        employeeReport.generateReport();
        
        // Generate Inventory Report
        ReportTemplate inventoryReport = new InventoryReport(sc);
        System.out.println("Generating Inventory Report:");
        
        // TODO: Generate the Inventory Report by calling the generateReport() method.
        inventoryReport.generateReport();
          
          
    }
}
```

```java title="SalesReport.java" linenums="1"
// This class represents a Sales Report, gathering and processing sales data from user input.

package Template;

import java.util.Scanner;

public class SalesReport extends ReportTemplate {
    
    private Scanner sc;

    public SalesReport(Scanner sc) {
        this.sc = sc;
    }
    
    @Override
    protected void gatherData() {
        String gatherData = sc.nextLine();
        System.out.println(gatherData);
    }

    @Override
    protected void processData() {
        String processData = sc.nextLine();
        System.out.println(processData);
    }
}
```

```java title="EmployeeReport.java" linenums="1"
// This class represents a Employee Report, gathering and processing sales data from user input.

package Template;

import java.util.Scanner;

public class EmployeeReport extends ReportTemplate {
    
    private Scanner sc;

    public EmployeeReport(Scanner sc) {
        this.sc = sc;
    }
    
    @Override
    protected void gatherData() {
        String gatherData = sc.nextLine();
        System.out.println(gatherData);
    }

    @Override
    protected void processData() {
        String processData = sc.nextLine();
        System.out.println(processData);
    }
}
```

```java title="InventoryReport.java" linenums="1"
// This class represents a Inventory Report, gathering and processing sales data from user input.

package Template;

import java.util.Scanner;

public class InventoryReport extends ReportTemplate {
    
    private Scanner sc;

    public InventoryReport(Scanner sc) {
        this.sc = sc;
    }
    
    @Override
    protected void gatherData() {
        String gatherData = sc.nextLine();
        System.out.println(gatherData);
    }

    @Override
    protected void processData() {
        String processData = sc.nextLine();
        System.out.println(processData);
    }
}
```

```java title="ReportTemplate.java" linenums="1"
// Abstract class defining the template for report generation, enforcing the structure while allowing specific implementations for each report type.

package Template;

public abstract class ReportTemplate {
    
    // Template method defining the skeleton of the report generation
    public final void generateReport() {
        gatherData(); // Specific to each report
        processData(); // Specific to each report
        formatReport(); // Common across all reports
        printReport(); // Common across all reports
    }
    
    // Steps to be implemented by subclasses
    protected abstract void gatherData();
    protected abstract void processData();
    
    // Default methods that can be common across all reports
    protected void formatReport() {
        System.out.println("Formatting the report with appropriate layout and style.");
    }
    
    protected void printReport() {
        System.out.println("Printing the report for final review and distribution.");
    }
}
```

## Iterator Pattern

```java title="Exercise.java" linenums="1"
// This class allows users to input various types of notifications and then displays them.

package Iterator;

import java.util.Scanner;

public class Exercise {
    
    // Do not modify the run method. It is designed to handle user input and manage the notification workflow.
    public void run() {
        
        Scanner sc = new Scanner(System.in);
        NotificationManager notificationManager = new NotificationManager();

        // Add Notifications
        for(int i=0;i < 2;i++) {
            String emailNotification = sc.nextLine();
            String smsNotification = sc.nextLine();
            String pushNotification = sc.nextLine();
            
            notificationManager.addEmailNotification(emailNotification);
            notificationManager.addSMSNotification(smsNotification);
            notificationManager.addPushNotification(pushNotification);
        }

        // Print all notifications
        // TODO: Use notificationManager to display all the added notifications by invoking the method that prints them.
            notificationManager.printAllNotifications();

        
        sc.close();
    }
}
```

```java title="NotificationManager.java" linenums="1"
// This class manages different types of notifications (email, SMS, push) and provides methods to add notifications and print them using an iterator pattern.

package Iterator;

import java.util.Iterator;

public class NotificationManager {
    
    private NotificationCollection emailNotifications;
    private NotificationCollection smsNotifications;
    private NotificationCollection pushNotifications;

    public NotificationManager() {
        
        // TODO: Initialize Email Notifications using the class responsible for handling Email Notifications.
        this.emailNotifications = new EmailNotification();
        
        
        // TODO: Initialize SMS Notifications using the class responsible for handling SMS Notifications.
        this.smsNotifications = new SMSNotification();
        
        // TODO: Initialize Push Notifications using the class responsible for handling Push Notifications.
        this.pushNotifications = new PushNotification();
        

    }

    public void addEmailNotification(String message) {
        ((EmailNotification) emailNotifications).addNotification(message);
    }

    public void addSMSNotification(String message) {
        ((SMSNotification) smsNotifications).addNotification(message);
    }

    public void addPushNotification(String message) {
        ((PushNotification) pushNotifications).addNotification(message);
    }

    public void printAllNotifications() {
        printNotifications(emailNotifications.createIterator(), "Email");
        printNotifications(smsNotifications.createIterator(), "SMS");
        printNotifications(pushNotifications.createIterator(), "Push");
    }

    private void printNotifications(Iterator<Notification> iterator, String type) {
        System.out.println(type + " Notifications:");
        while (iterator.hasNext()) {
            System.out.println(iterator.next().getMessage());
        }
    }
}
```

```java title="EmailNotification.java" linenums="1"
// This class represents a collection of Email notifications and provides functionality to add notifications and create an iterator for traversing through them.

package Iterator;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class EmailNotification implements NotificationCollection {
    
    private List<Notification> emailNotifications;

    public EmailNotification() {
        emailNotifications = new ArrayList<>();
    }

    public void addNotification(String message) {
        emailNotifications.add(new Notification(message));
    }

    @Override
    public Iterator<Notification> createIterator() {
        // TODO: Return a new iterator for the Email Notifications using the EmailNotificationIterator class.
        return new EmailNotificationIterator(this.emailNotifications);
        
    }

    private class EmailNotificationIterator implements Iterator<Notification> {
        private int position = 0;
        private List<Notification> notifications;

        public EmailNotificationIterator(List<Notification> notifications) {
            this.notifications = notifications;
        }

        @Override
        public boolean hasNext() {
            return position < notifications.size();
        }

        @Override
        public Notification next() {
            return notifications.get(position++);
        }
    }
}
```

```java title="SMSNotification.java" linenums="1"
// This class represents a collection of SMS notifications and provides functionality to add notifications and create an iterator for traversing through them.

package Iterator;

import java.util.Iterator;
import java.util.ArrayDeque;
import java.util.Queue;

public class SMSNotification implements NotificationCollection {
    
    private Queue<Notification> smsNotifications;

    public SMSNotification() {
        smsNotifications = new ArrayDeque<>();
    }

    public void addNotification(String message) {
        smsNotifications.add(new Notification(message));
    }

    @Override
    public Iterator<Notification> createIterator() {
        // TODO: Return a new iterator for the SMS Notifications using the SMSNotificationIterator class.
        return new SMSNotificationIterator(this.smsNotifications);
        
    }

    private class SMSNotificationIterator implements Iterator<Notification> {
        private Queue<Notification> notifications;

        public SMSNotificationIterator(Queue<Notification> notifications) {
            this.notifications = new ArrayDeque<>(notifications);
        }

        @Override
        public boolean hasNext() {
            return !notifications.isEmpty();
        }

        @Override
        public Notification next() {
            return notifications.poll();
        }
    }
}
```

```java title="PushNotification.java" linenums="1"
// This class represents a collection of Push notifications and provides functionality to add notifications and create an iterator for traversing through them.

package Iterator;

import java.util.LinkedHashSet;
import java.util.Iterator;
import java.util.Set;

public class PushNotification implements NotificationCollection {
    
    private Set<Notification> pushNotifications;

    public PushNotification() {
        pushNotifications = new LinkedHashSet<>();
    }

    public void addNotification(String message) {
        pushNotifications.add(new Notification(message));
    }

    @Override
    public Iterator<Notification> createIterator() {
        // TODO: Return a new iterator for the Push Notifications using the PushNotificationIterator class.
        return new PushNotificationIterator(this.pushNotifications);
        
    }

    private class PushNotificationIterator implements Iterator<Notification> {
        private Iterator<Notification> iterator;

        public PushNotificationIterator(Set<Notification> notifications) {
            this.iterator = notifications.iterator();
        }

        @Override
        public boolean hasNext() {
            return iterator.hasNext();
        }

        @Override
        public Notification next() {
            return iterator.next();
        }
    }
}
```

```java title="Notification.java" linenums="1"
// This class represents a notification with a message for managing notifications.

package Iterator;

public class Notification {
    private String message;

    public Notification(String message) {
        this.message = message;
    }

    public String getMessage() {
        return message;
    }
}
```

```java title="NotificationCollection.java" linenums="1"
// This defines an interface for collections that provide an iterator for notifications.

package Iterator;

import java.util.Iterator;

public interface NotificationCollection {
    public Iterator<Notification> createIterator();
}
```

## State pattern

```java title="Exercise.java" linenums="1"
// The Exercise class demonstrates the State Design Pattern for a Media Player.

package State;

import java.util.Scanner;

public class Exercise {

    // Do not modify the run method. It is designed to handle user commands (Play, Pause, stop) for the Media Player. 
    public void run() {
        
        MediaPlayer mediaPlayer = new MediaPlayer();
        Scanner sc = new Scanner(System.in);
        
        String choice = sc.next();
        
        switch (choice) {
            case "Play":
                mediaPlayer.play();
                break;
            case "Pause":
                // TODO: Set the Media Player state to PausedState
                mediaPlayer.setState(new PausedState());
                
                mediaPlayer.pause();
                break;
            case "Stop":
                // TODO: Set the Media Player state to StoppedState
                mediaPlayer.setState(new StoppedState());
                
                mediaPlayer.stop();
                break;
            default:
                System.out.println("Invalid choice.");
        }
        
        // TODO: Display the current state of the Media Player
        mediaPlayer.displayState();
    
        sc.close();
    }
}
```

```java title="MediaPlayer.java" linenums="1"
// The MediaPlayer class manages the current state of the Media Player using the State Design Pattern.

package State;

public class MediaPlayer {
    
    private State state;
    
    public MediaPlayer() {
        // TODO: Set the initial state of the Media Player to PlayingState
        state = new PlayingState();
        
    }
    
    public void setState(State state) {
        this.state = state;
    }
    
    public void play() {
        // TODO: Implement the functionality for pressing play
        state.pressPlay();
    }

    public void stop() {
        // TODO: Implement the functionality for pressing stop
        state.pressStop();
    }

    public void pause() {
        // TODO: Implement the functionality for pressing pause
        state.pressPause();
    }

    public void displayState() {
        // TODO: Implement the functionality to display the current state
        state.display();
    }
}
```

```java title="PlayingState.java" linenums="1"
// The PlayingState class represents the playing state of the Media Player. 

package State;

public class PlayingState implements State {

    @Override
    public void pressPlay() {
        System.out.println("Starting playback");
    }

    @Override
    public void pressStop() {
        System.out.println("Stopping playback");
    }

    @Override
    public void pressPause() {
        System.out.println("Pausing playback");
    }

    @Override
    public void display() {
        System.out.println("Current State: Playing");
    }
}
```

```java title="PausedState.java" linenums="1"
// The PausedState class represents the playing state of the Media Player.

package State;

public class PausedState implements State {

    @Override
    public void pressPlay() {
        System.out.println("Resuming playback");
    }

    @Override
    public void pressStop() {
        System.out.println("Stopping playback from pause");
    }

    @Override
    public void pressPause() {
        System.out.println("Pausing playback");
    }

    @Override
    public void display() {
        System.out.println("Current State: Paused");
    }
}
```

```java title="StoppedState.java" linenums="1"
// The StoppedState class represents the playing state of the Media Player. 

package State;

public class StoppedState implements State {

    @Override
    public void pressPlay() {
        System.out.println("Starting playback");
    }

    @Override
    public void pressStop() {
        System.out.println("Stopping playback");
    }

    @Override
    public void pressPause() {
        System.out.println("Can't pause. Media is already stopped");
    }

    @Override
    public void display() {
        System.out.println("Current State: Stopped");
    }
}
```

```java title="State.java" linenums="1"
// The State interface defines the methods for different states of the Media Player. 

package State;

public interface State {
    void pressPlay();
    void pressStop();
    void pressPause();
    void display();
}
```

## Mediator Pattern

```java title="Exercise.java" linenums="1"
// This class simulates the flight control system, managing airplane takeoff and landing requests through a control tower mediator.

package Mediator;

import java.util.Scanner;

public class Exercise {

    // Do not modify the run method. It demonstrates the functionality of the Flight Control System using the Mediator design pattern.
    public void run() {
        
        Scanner sc = new Scanner(System.in);
        
        ControlTower controlTower = new ControlTower();
        
        String airplaneId1 = sc.nextLine();
        String airplaneId2 = sc.nextLine();
        String airplaneId3 = sc.nextLine();
        String airplaneId4 = sc.nextLine();
        
        // TODO: Instantiate an airplane with the first provided ID
        Airplane airplane1 = new Airplane(airplaneId1);
        
        // TODO: Instantiate an airplane with the second provided ID
        Airplane airplane2 = new Airplane(airplaneId2);
        
        // TODO: Instantiate an airplane with the third provided ID
        Airplane airplane3 = new Airplane(airplaneId3);
        
        // TODO: Instantiate an airplane with the fourth provided ID
        Airplane airplane4 = new Airplane(airplaneId4);
    
    
        // TODO: Register the first airplane with the control tower
        controlTower.registerAirplane(airplane1);
        
        
        // TODO: Register the second airplane with the control tower
        controlTower.registerAirplane(airplane2);
        
        
        // TODO: Register the third airplane with the control tower
        controlTower.registerAirplane(airplane3);
        
        
        // TODO: Register the fourth airplane with the control tower
        controlTower.registerAirplane(airplane4);

        airplane1.requestTakeoff();
        airplane2.requestTakeoff();
        airplane3.requestTakeoff();
        airplane4.requestTakeoff();
        
        // TODO: Mark the first airplane as having completed takeoff and free a runway
        controlTower.completeTakeoff(airplane1);
        
        
        // TODO: Mark the second airplane as having completed takeoff and free a runway
        controlTower.completeTakeoff(airplane2);
        

        airplane3.requestTakeoff();
        airplane4.requestTakeoff();
        
        // TODO: Mark the third airplane as having completed takeoff and free a runway
        controlTower.completeTakeoff(airplane3);
        
        // TODO: Mark the fourth airplane as having completed takeoff and free a runway
        controlTower.completeTakeoff(airplane4);

        airplane1.requestLanding();
        airplane2.requestLanding();
        
        // TODO: Mark the first airplane as having completed landing and free a runway
        controlTower.completeLanding(airplane1);
        
        
        // TODO: Mark the second airplane as having completed landing and free a runway
        controlTower.completeLanding(airplane2);
        
        
        airplane3.requestLanding();
        airplane4.requestLanding();
        
        // TODO: Mark the third airplane as having completed landing and free a runway
        controlTower.completeLanding(airplane3);
        
        
        // TODO: Mark the fourth airplane as having completed landing and free a runway
        controlTower.completeLanding(airplane4);
        
        

        sc.close();
    }
}
```

```java title="Airplane.java" linenums="1"
// This class represents an airplane that interacts with the Control Tower mediator for takeoff and landing requests.

package Mediator;

public class Airplane {
    private String id;
    private Mediator mediator;

    public Airplane(String id) {
        this.id = id;
    }

    public void setMediator(Mediator mediator) {
        this.mediator = mediator;
    }

    public void requestTakeoff() {
        System.out.println("Airplane " + id + " requesting takeoff");
        
        // TODO: Notify the mediator to handle the takeoff request for this airplane
        mediator.handleTakeoffRequest(this);
        
    }

    public void requestLanding() {
        System.out.println("Airplane " + id + " requesting landing");
        
        // TODO: Notify the mediator to handle the landing request for this airplane
        mediator.handleLandingRequest(this);
        
    }

    public void receiveNotification(String message) {
        System.out.println("Airplane " + id + ": " + message);
    }

    public String getId() {
        return id;
    }
}
```

```java title="ControlTower.java" linenums="1"
// This class serves as the mediator, managing communication and runway allocation between airplanes for takeoff and landing.

package Mediator;

import java.util.ArrayList;
import java.util.List;

public class ControlTower implements Mediator {
    
    private List<Airplane> airplanes;
    private int takeoffRunways;
    private int landingRunways;

    public ControlTower() {
        // TODO: Initialize the list of airplanes to manage communication
        airplanes = new ArrayList<>();
        
        this.takeoffRunways = 2;
        this.landingRunways = 2;
    }

    @Override
    public void registerAirplane(Airplane airplane) {
        // TODO: Add the airplane to the list of registered airplanes
        airplanes.add(airplane);
        
        // TODO: Set the mediator for the airplane to enable communication
        airplane.setMediator(this);
        
        
    }

    @Override
    public void handleTakeoffRequest(Airplane airplane) {
        if (takeoffRunways > 0) {
            takeoffRunways--; 
            notifyAirplane(airplane, "Takeoff approved. Runways available: " + takeoffRunways);
        } else {
            notifyAirplane(airplane, "Takeoff denied. No runways available. Please wait");
        }
    }

    @Override
    public void handleLandingRequest(Airplane airplane) {
        if (landingRunways > 0) {
            landingRunways--;
            notifyAirplane(airplane, "Landing approved. Runways available: " + landingRunways);
        } else {
            notifyAirplane(airplane, "Landing denied. No runways available. Please wait");
        }
    }

    // Simulate the completion of takeoff and free the runway
    public void completeTakeoff(Airplane airplane) {
        System.out.println("Airplane " + airplane.getId() + " has taken off");
        takeoffRunways++;
        System.out.println("Runway freed. Available takeoff runways: " + takeoffRunways);
    }

    // Simulate the completion of landing and free the runway
    public void completeLanding(Airplane airplane) {
        System.out.println("Airplane " + airplane.getId() + " has landed");
        landingRunways++;
        System.out.println("Runway freed. Available landing runways: " + landingRunways);
    }

    private void notifyAirplane(Airplane airplane, String message) {
        // TODO: Notify the airplane of the status message from the control tower
        airplane.receiveNotification(message);
    }
}

```

```java title="Mediator.java" linenums="1"
// This interface defines the contract for the mediator responsible for managing airplane communication and requests for takeoff and landing.

package Mediator;

public interface Mediator {
    void registerAirplane(Airplane airplane);
    void handleTakeoffRequest(Airplane airplane);
    void handleLandingRequest(Airplane airplane);
}
```