# Factory Pattern
![image](../images/factory.jpg)
The factory pattern's primary purpose is to separate object consumption from object construction. By utilizing a factory, a consumer of your object need only utilize the factory andn associated methods to create an instance of your specific class to use in whatever other objects they wish.

# Motivation
The factory pattern is an extremely common design pattern used throughout software.  It began to get popular as objected-oriented software projectes grew in size and complexity, and the classes that they contained grew as well.  Consider this example class.
```Java
public class MyClass {
  private final DependencyA depA;
  
  public MyClass(DependencyA depA) {
    this.depA = depA;
  }
  
  ...
}
```
Constructing this class is relatively straight forward.  You need an instance of DependencyA (whether you create one or find one through some other means is up to you) and then it's a simple `MyClass myClassInstance = new MyClass(dependencyA)` and you're done.  However, imagine that `MyClass` is substantially more complex, and therefore to prevent the class from being many thousands of lines of code, delegates much of its functionality to other classes, and as such, looks more like this:
```Java
public class MyClass {
  private final DependencyA depA;
  private final DependencyB depB;
  private final DependencyC depC;
  private final DependencyD depD;
  private final DependencyE depE;
  
  public MyClass(DependencyA depA,
                 DependencyB depB,
                 DependencyC depC,
                 DependencyD depD,
                 DependencyE depE) {
    this.depA = depA;
    this.depB = debB;
    this.depC = depC;
    this.depD = depD;
    this.depE = depE;
  }
  ...
}
```
This class has substantially more dependencies that our first iteration of it and finding or creating all those dependencies could be somewhat difficult, and that assumes that none of those dependencies themselves have dependencies. This can very quickly become quite the mess. What's worse, every time that we need to construct this object we have to go through the same song and dance to figure all of this stuff out.  To solve this, we create another class that encapsulates this logic of actually constructing the class, so that other classes can instead reuse this new class instead of implementing it themselves.  Our factory would look something like this:
```Java
public class MyClassFactory {
  ...
  public MyClass getNewInstance() {
    // get or construct our dependencies.
    DependencyA depA = new DependencyA();
    DependencyB depB = ...
    
    return new MyClass(depA, depB...);
  }
  ...
}
```
Now a consumer of MyClass can simply get an instance of MyClassFactory (which ideally only gets constructed once and then passed around as necessary) and call `myClassFactoryInstance.getNewInstance()` whenever then need an instance of `MyClass`

