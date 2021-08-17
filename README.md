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
public class River {
    public River() {
    }
}
```
3- create the class that will depend on the 2 previous classes  
4- create an instance for the 2 classess and pass it to the constructor of  the first class  
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
