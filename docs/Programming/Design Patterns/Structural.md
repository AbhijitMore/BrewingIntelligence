# Structural Design Patterns

Structural patterns deal with object composition and how large structures are formed by combining classes and objects. These patterns focus on simplifying the structure by identifying relationships.

## Adapter Pattern

```java title="Exercise.java" linenums="1"
// This class demonstrates the usage of different weather services through the Adapter pattern.

package Adapter;

import java.util.Scanner;

public class Exercise {
    
    // Do not modify the run method. It is designed to demonstrate the usage of legacy and new weather services through the Adapter pattern.
    public void run() {
        
        Scanner sc = new Scanner(System.in);

        String legacyTemperature = sc.nextLine();
        String legacyCondition = sc.nextLine();

        // Using the legacy weather service with user input
        WeatherService legacyService = new LegacyWeatherService(legacyTemperature, legacyCondition);
        System.out.println("Legacy Weather Service Data:");
        
        // TODO: Print the weather data retrieved from the Legacy weather service.
        System.out.println(legacyService.getWeatherData());

        String temperature = sc.nextLine();
        String condition = sc.nextLine();

        NewWeatherService newService = new NewWeatherService(temperature, condition);
        
        // TODO: Create an adapter for the new weather service.
        
        
        WeatherService adaptedService = new NewWeatherServiceAdapter(newService);
        System.out.println("New Weather Service Data:");
        
        // TODO: Print the weather data retrieved from the new weather service.
        System.out.println(adaptedService.getWeatherData());
        
        
        sc.close();
    }
}
```

```java title="LegacyWeatherService.java" linenums="1"
// The Legacy class that provides weather data in XML format.

package Adapter;

public class LegacyWeatherService implements WeatherService {
    
    private String temperature;
    private String condition;

    public LegacyWeatherService(String temperature, String condition) {
        this.temperature = temperature;
        this.condition = condition;
    }

    @Override
    public String getWeatherData() {
        return "<weather><temperature>" + temperature + "</temperature><condition>" + condition + "</condition></weather>";
    }
}
```

```java title="NewWeatherService.java" linenums="1"
// The New class that provides weather data in JSON format.

package Adapter;

public class NewWeatherService {
    
    private String temperature;
    private String condition;

    public NewWeatherService(String temperature, String condition) {
        this.temperature = temperature;
        this.condition = condition;
    }

    public String fetchWeather() {
        return "{\"temperature\": " + temperature + ", \"condition\": \"" + condition + "\"}";
    }
}
```

```java title="NewWeatherServiceAdapter.java" linenums="1"
// The Adapter class that adapts the new weather service to the WeatherService interface.

package Adapter;

public class NewWeatherServiceAdapter implements WeatherService {
    
    private NewWeatherService newWeatherService;

    public NewWeatherServiceAdapter(NewWeatherService newWeatherService) {
        this.newWeatherService = newWeatherService;
    }

    @Override
    public String getWeatherData() {
        // TODO: Fetch weather data from the new weather service and return the formatted data
        String data = newWeatherService.fetchWeather();
        return data;
        
    }
}
```

```java title="WeatherService.java" linenums="1"
// This interface defines a consistent contract for retrieving weather data from various weather services.

package Adapter;

public interface WeatherService {
    String getWeatherData();
}
```

## Decorator Pattern

```java title="Exercise.java" linenums="1"
// The Exercise class facilitates user interaction to create a customized coffee by allowing the selection of various ingredients and then outputs the final coffee description and total cost.

package Decorator;

import java.util.Scanner;

public class Exercise {

    // Do not modify the run method. It is designed to demonstrate the usage of the Decorator pattern for customizing coffee with various ingredients.
    public void run(){
        
        Scanner sc = new Scanner(System.in);

        Coffee coffee = new BasicCoffee();

        boolean addMoreIngredients = true;

        while (addMoreIngredients) {

            String choices = sc.nextLine();
            String[] ingredients = choices.split(" ");

            for (String choice : ingredients) {
                
                switch (choice) {
                    case "1":
                        // TODO: Complete the implementation for adding Milk to the coffee.
                        coffee = new Milk(coffee);
                        
                        break;
                    case "2":
                        // TODO: Complete the implementation for adding Sugar to the coffee.
                        coffee = new Sugar(coffee);
                        
                        break;
                    case "3":
                        // TODO: Complete the implementation for adding Whipped Cream to the coffee.
                        
                        coffee = new WhippedCream(coffee);
                        break;
                    case "4":
                        addMoreIngredients = false;
                        break;
                    default:
                        System.out.println("Invalid choice: " + choice);
                        break;
                }
            }

            if (!addMoreIngredients) {
                break;
            }
        }

        System.out.println("Final Coffee Description: " + coffee.getDescription());
        System.out.println("Total Cost: $" + coffee.getCost());
        
        sc.close();
    }
}

```

```java title="CoffeeDecorator.java" linenums="1"
// Abstract class for decorating Coffee objects, allowing additional features to be added while preserving core functionality.

package Decorator;

public abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public String getDescription() {
        // TODO: Complete this method to return the description of the decorated coffee.
        return coffee.getDescription();
    }

    @Override
    public double getCost() {
        // TODO: Complete this method to return the cost of the decorated coffee.
        return coffee.getCost();
    }
}
```

```java title="Milk.java" linenums="1"
// Concrete decorator class that adds Milk to a Coffee object, modifying its description and cost accordingly.

package Decorator;

public class Milk extends CoffeeDecorator {

    public Milk(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.50; 
    }
}
```

```java title="Sugar.java" linenums="1"
// Concrete decorator class that adds Sugar to a Coffee object, enhancing its description and adjusting its cost.

package Decorator;

public class Sugar extends CoffeeDecorator {

    public Sugar(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Sugar";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.30; 
    }
}
```

```java title="WhippedCream.java" linenums="1"
// Concrete decorator class that adds Whipped Cream to a Coffee object, enhancing its description and adjusting its cost.

package Decorator;

public class WhippedCream extends CoffeeDecorator {

    public WhippedCream(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Whipped Cream";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.70; 
    }
}
```

```java title="BasicCoffee.java" linenums="1"
// This class represents implementation of the Coffee interface representing a basic coffee with a fixed description and cost.

package Decorator;

public class BasicCoffee implements Coffee {

    @Override
    public String getDescription() {
        return "Basic Coffee";
    }

    @Override
    public double getCost() {
        return 3.00; 
    }
}
```

```java title="Coffee.java" linenums="1"
// This interface defines the contract for Coffee objects, requiring methods for obtaining the description and cost.

package Decorator;

public interface Coffee {
    String getDescription();
    double getCost();
}
```

## Proxy Pattern

```java title="Exercise.java" linenums="1"
// This class demonstrates the use of the Proxy Design Pattern to manage access to a network service and cache responses.

package Proxy;

import java.util.Scanner;

public class Exercise {

    // Do not modify the run method. It demonstrates the usage of the Proxy Design Pattern to manage access to a network service.
    public void run() {
        
        NetworkService networkService = new NetworkServiceProxy();
        Scanner sc = new Scanner(System.in);

        String userInput = sc.nextLine();
        
        // TODO: Fetch data using the networkService and print the result
        String fetch = networkService.fetchData(userInput);
        System.out.println(fetch);
        
        
        // TODO: Fetch data again using the networkService (should retrieve from cache) and print the result
        String Refetch = networkService.fetchData(userInput);
        System.out.println(Refetch);
        
        sc.close();
    }
}
```

```java title="NetworkServiceProxy.java" linenums="1"
// This class implements the Proxy Design Pattern to manage access to the RealNetworkService and cache responses.

package Proxy;

import java.util.HashMap;
import java.util.Map;

public class NetworkServiceProxy implements NetworkService {
    
    private RealNetworkService realNetworkService;
    private Map<String, String> cache;

    public NetworkServiceProxy() {
        // TODO: Initialize the cache to store fetched data.
        cache = new HashMap<>();
    }

    @Override
    public String fetchData(String input) {
        
        if (cache.containsKey(input)) {
            System.out.println("Fetching data from cache");
            
            // TODO: Return the cached data for the given input.
            return cache.get(input);
            
        }

        if (realNetworkService == null) {
            // TODO: Initialize the RealNetworkService if it has not been created yet.
            realNetworkService = new RealNetworkService();
            
        }
        
        // TODO: Fetch data from the real network service using the provided input.
        String data = realNetworkService.fetchData(input);
        
        // TODO: Cache the fetched data with the input as the key for future access.
        cache.put(input, data);
        
        // TODO: Return the fetched data to the client.
        return data;

    }
}
```

```java title="RealNetworkService.java" linenums="1"
// This class represents the actual network service that fetches data from a remote server.

package Proxy;

public class RealNetworkService implements NetworkService {
    
    private String data;
    
    @Override
    public String fetchData(String input) {
        data = "Data fetched from remote server for input: " + input;
        return data;
    }
}
```

```java title="NetworkService.java" linenums="1"
// This interface defines the contract for network services that fetch data based on user input.

package Proxy;

public interface NetworkService {
    String fetchData(String input);
}
```
## Composite Pattern

## Facade Pattern

## Flyweight Pattern