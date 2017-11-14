Interface
=========

class A 
--------

```java
public class A implements B.MyInterface {

// Do something Here when call from B class
@Override
public void onSample(String S){
 // Do Something With S
}
// Do Extra Here When Call from B class
@Override
public void onExtra(String X){
// Do Domthing With x 
}
}
```

class B
-------

```java
public class B {

public MyInterface myInterface;

//Initialize Interface Here
public interface MyInterface{

void onSomething(String S);

void onExtra(String X);
}

// Do Something on A call by calling Here
myInterface.onSomething("Hello");
// Do Extra on A class by calling Here
myInterface.onExtra("Meo");

}
