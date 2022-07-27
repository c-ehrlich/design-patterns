# Factory Method
(aka Virtual Contructor)

- Provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created
- Complexity: 1/3
- Popularity: 3/3
## Problem
- Adding a new class `Ship` to the program isn't that simple if the rest of the code is already coupled to existing classes `Truck`
- Adding `Ship` would require large changes all over the codebase
- Result: Nasty code full of conditionals
## Solution
- Replace direct object construction calls (`new Truck()`) with calls to a spcial _factory_ method.
- The `new` call is made from within that method
![Factory Method](https://user-images.githubusercontent.com/8353666/181175501-87dd0458-03e9-41f0-af33-ecddc7ced67a.png)
- Objects returned by a factory method are often called **products**.
- You can now override the factory method in a subclass to change what is being created, to a subclass of what the parent method creates
## Limitations
- Need a shared base class or interface (same methods etc)
  - example: interface Transport must be implemented by both Truch and Ship
## Structure
![Factory Method Structure](https://user-images.githubusercontent.com/8353666/181175955-963f1e88-3068-495e-9968-6a203c4299ba.png) 
1. The **Product** declares the interface - this interface is common to all objects that can be produced by the creator and its subclasses.
2. **Concrete Products** are different implementations of the product interface.
3. The **Creator** class declares the factory method that returns new product objects.
    - The return type of this method needs to match the product interface!
    - Can choose to declare the factory method as abstract to force all sub-classes to implement their own versions of the method. OR the base factory method can return some default product type
    - ! Despite the pattern name, product creation is **NOT** the primary responsibility of the Creator
      - The point is to decouple the creation logic from from that class
4. **Concrete Creators** override the base factory method so it returns a different type of product.
   - The factory method doesn't have to create **new** instances all the time. It can also return existing objects from a cache, object pool, etc.
## Applicability
- Use the Factory Method when you don't know beforehand the exact types and dependencies of the objects your code should work with.
  - Factory Method makes it easier to extend the product construction code independently from the rest of the code
  - For example, to add a MacButton to the pseudocode we just need to create a new creator subclass
- Use the Factory Method when you want to provide users of your library or framework with a way to extend its internal components.
  - Easier than full on inheritance
  - Let's say a framework provides square buttons but you want roung buttons
    - Extend the `Button` class with a `RoundButton` subclass
    - Extend `UIFramework` with `UIWithRoundButtons` that overrides `createButton()`
    - Base class returns `Button` object, our sublcass returns `RoundButton` objects
    - Now just use `UIWithRoundButtons` instead of `UIFramework`.
- Use the Factory Method when you want to save system resources by reusing existing objects instead of rebuilding them each time.
  - Resource-intensive objects like database connections, file systems, network resources
  - Without factory: 1. create some kind of storage to keep track of all the created objects 2. when someone requests an object, check the storage 3. return from storage or 4. create
    - That's a lot of code! Factory puts it all in one place
  - A constructor must always return new objects by definition. So by wrapping it we can build the above.
## How to Implement
1. Make all products follow the same interface. Declare methods that make sense in every product.
2. Add an empty factory method inside the creator class
3. In the creator's code, replace all existing references to constructors with calls to the factory method
4. Create a set of creator subclasses for each type of product listed in the factory method. Override and extend as necessary
## Pros and Cons
- ++ Avoid tight coupling between the creator and the concrete products.
- ++ single responsibility principle - move the product creation code into one place
- ++ Open/Closed principle - introduce new types of products into the program without breaking existing stuff
- -- The code may become more complicated since you need to implement a lot of new subclasses
## Relations with Other Patterns
- Often start with **Factory Method** (less complcated) and evolve toward **Abstract Factory**, **Prototype**, or **Builder** (more flexible, but more complicated)
- **Abstract Factory** classes are often based on a set of **Factory Methods**, but you can also use **Prototype** to compose the methods on these classes.
- You can use **Factory Method** along with **Iterator** to let collection subclasses return different types of iterators that are compatible with the collections.
- **Prototype** isn't based on inheritance, so it doesn't have its drawbacks. On the other hand, **Prototype** requires a complicated initialization of the cloned object. **Factory Method** is based on inheritance but doesn't require an initialization step.
- **Factory Method** is a specialization of **Template Method**. At the same time, a **Factory Method** may serve as a step in a larger **Template Method**.