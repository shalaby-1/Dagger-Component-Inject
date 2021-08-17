# Dagger-Component-Inject  
Steps to add Dagger  
1- add Dagger Dependencies  
```
    implementation 'com.google.dagger:dagger:2.11'
    implementation 'com.google.dagger:dagger-android-support:2.11'
    implementation 'javax.annotation:javax.annotation-api:1.3.2'

    annotationProcessor("javax.annotation:javax.annotation-api:1.3.2")
    annotationProcessor 'com.google.dagger:dagger-android-processor:2.11'
    annotationProcessor "com.google.dagger:dagger-compiler:2.11"
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
  ```java
    @Inject
    public Coffee() {
    }
  ```
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
  7- to acess master class directly without interface create a field for the class 
```java
    Coffee coffee;
```
  8- replace the method that return object with the object itself 
```java 
    CoffeeComponent component = DaggerCoffeeComponent.create();
    Log.d(TAG, "onCreate: " + coffee.getCoffeeName());
```
  9- add inject annotation to the object 
```java
    @Inject
    Coffee coffee;
```
  10- to make dagger know that we want to inject an activity inside the component interface create a method named inject receving the desired activity 
  ```java
      void inject(MainActivity mainActivity);
  ```
  11- inside the main activity pass instance of it to the component interface 
  ```java
         // Autamated dependency injection
        CoffeeComponent component = DaggerCoffeeComponent.create();
        // pass instance of the activity to the component interface
        component.inject(this);
        Log.d(TAG, "onCreate: " + coffee.getCoffeeName());
  ```
- Method Injection  
  used when there is a function we want to exceute every time we make an instance of a certain object  
  1- create a method inside the master class 
  ```java
      public void connectElectricity(){
        Log.d(TAG, "aly connectElectricity: connecting ...");
    }
  ```
  2- just add inject annotation to the method and you will not need to acess it using object of the master class 
  ```java
    @Inject
    public void connectElectricity(){
        Log.d(TAG, "aly connectElectricity: connecting ...");
    }
  ```
- Dependency cycle using inject keyword  
![image](https://user-images.githubusercontent.com/88387388/129785673-df12e01e-93d7-49d6-9b9e-c837cb6e93ca.png)
- Dependency cycle using provides and module 
![image](https://user-images.githubusercontent.com/88387388/129785934-ebb5b912-bf60-4513-b2ea-ae97050ea8f9.png)
- Using Dagger for a class from external library where we can not add inject keyword  
1- create a module class for the master class  
```java
public class CoffeeModule {
}
```
2- in the module class create a method that return object from the class from external package (dependency class)
```java
    River provideRiver(){
        return new River();
    }
```
3- To connect component with the module add module annotation for the class 
```java
@Module
public class CoffeeModule {

    River provideRiver(){
        return new River();
    }

}
```
4- add provies annotation for the method 
```java
    @Provides
    River provideRiver(){
        return new River();
    }
```
5- tell component to use the available modules 
```java
@Component(modules = CoffeeModule.class)
public interface CoffeeComponent {
    Coffee getCoffee();

    void inject(MainActivity mainActivity);


}
```
