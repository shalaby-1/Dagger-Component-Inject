# Dagger-Component-Inject
```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

//        // Manual dependency injection
//        Farm farm = new Farm();
//        River river = new River();
//
//        Coffee coffee = new Coffee(farm,river);

        // Autamated dependency injection
        CoffeeComponent component = DaggerCoffeeComponent.create();
        component.getCoffee();

    }
```
