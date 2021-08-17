# Dagger-Component-Inject  
Steps to add Dagger  
1- add Dagger Dependencies  
```
    annotationProcessor 'com.google.dagger:dagger-compiler:2.11'
    implementation 'com.google.dagger:dagger:2.11'
    annotationProcessor "com.google.dagger:dagger-compiler:2.11"
    implementation "com.google.dagger:dagger-android:2.35.1"
    implementation 'com.google.dagger:dagger-android-support:2.11'
    annotationProcessor 'com.google.dagger:dagger-android-processor:2.11'
    implementation 'javax.annotation:javax.annotation-api:1.3.2'
    annotationProcessor("javax.annotation:javax.annotation-api:1.3.2")
```
1- create 2 classes  
2- add an empty constructor for the 2 classes  
```java
public class Farm {
    public Farm() {
    }
}
```
```java
public class River {
    public River() {
    }
}
```
3- create the class that will depend on the 2 previous classes  
4- create an instance for the 2 classess and pass it to the constructor of  the master class  
```java
public class Coffee {
    // depend on Farm and River class
    private Farm farm;
    private River river;

    public Coffee(Farm farm, River river) {
        this.farm = farm;
        this.river = river;
    }
}
```
- To add manual dependency injection  
```java 
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Manual dependency injection
        Farm farm = new Farm();
        River river = new River();

        Coffee coffee = new Coffee(farm,river);
    }
```
- To add Automated dependency injection (Constructor injection)  
1- create component interface  
2- inside the interface create a function that return an object of the master class  
3- mark it with a component annotation  
```java 
@Component
public interface CoffeeComponent {
    Coffee getCoffee();
}
```
4- mark the constructor master class and the 2 classes that he depends on with inject annotation  
```java
    @Inject
    public Coffee(Farm farm, River river) {
        this.farm = farm;
        this.river = river;
    }
```
```java 
public class Farm {
    @Inject
    public Farm() {
    }
}
```
```java
public class River {
    @Inject
    public River() {
    }
}
```
5- add a reference to the interface inside main acitivity and use it to access the method to return object 
```java
        // Autamated dependency injection
        CoffeeComponent component = DaggerCoffeeComponent.create();
        component.getCoffee();
```
- Field injection and method Injection  
  when to use it ?
  when there is a parameter that i need to use inside the class but it is NOT inside the constructor 
- steps of field injection 
  1- inside the master class remove parameters of the constructor and it is fields 
  2- create a method that return data from the 2 classes 
  ```java 
      public String getCoffeeCup(){
        
    }
  ```
  3- create at each class a method that return data to the master class 
  ```java
      public String getBeans(){
        return "beans";
    }
  ```
  ```java
      public String getWater(){
        return "water";
    }
  ```
  4- return the data from the 2 classes 
  ```java 
        public String getCoffeeCup(){
          return farm.getBeans()+ "+" + river.getWater();
    }
  ```
  5- add inject annotation to the 2 fields inside the master class 
  ```java
    @Inject
    Farm farm;
    @Inject
    River river;
  ```
  6- access method inside the master class using method that return object of it 
  ```java
        // Autamated Field dependency injection
        CoffeeComponent component = DaggerCoffeeComponent.create();
        Log.d(TAG, "onCreate: "+ component.getCoffee().getCoffeeCup());
  ```
