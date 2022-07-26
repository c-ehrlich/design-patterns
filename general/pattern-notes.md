# Design Patterns

## Code Reuse
- Making code easier to reuse usually takes extra work
- Issues: tight coupling between components, dependencies on concrete classes instead of interfaces, hardcoded operations
- Using Design Patterns can make code easier to reuse. However it also often makes them more complicated.
- Three levels
  - Classes
  - Design Patterns - a description about how a couple of classes can relate to and interact with each other. Let you reuse design ideas and concepts independently of concrete code.
  - Frameworks

## Extensibility
- Adding new features etc
- We want to provide that possibility as broadly as possible

## Design Principles
- Encapsulate what varies
  - Protect the rest of the code from adverse effects
  - Spend less time making changes to existing things, more time implementing new features
- Options:
  - On a method level
    - Scenario: online store, and the way tax is calculated changes
    - If tax is calculated in `getOrderTotal`, that entire method needs to be reworked
    - If tax is calculated in `getTaxRate`, we can call that from within `getOrderTotal`
  - On a class level
    - Common pattern: add more and more responsibilities to a class that was once meant to do a simple thing
    - Solution: Extract some stuff to a new class
    - Extract class `TaxCalculator` from class `Order`

## Program to an Interface, not to an Implementation
Or: "Depend on abstractions, not on concrete classes".
1. Determine what exactly one object needs from the other
2. Describe these methods in a new interface or abstract class
3. Make the class that is a dependency implement this interface
4. Now make the second class dependent on this interface rather than on the concrete class
5. Now you can create other classes that implement this interface and they will also work :)
6. Downside: code is more complicated

![Before and after extracting the interface. ](https://user-images.githubusercontent.com/8353666/180968047-dc70501b-458a-43bb-a56e-242465dff1ba.png)

## Favor Composition Over Inheritance
- Inheritance comes with a lot of issues!
  - A subclass can't reduce the interface of the superclass, even if it doesn't need everything
  - When overriding methods you need to be sure that the new behaviour is compatible with the old one
  - Inheritance breaks encapsulation - because the subclass has access to the internal details of the superclass
  - Changes in the superclass may break functionality of the subclass
- Composition
  - Inheritance means **is a**, composition means **has a**.

## SOLID Principles
- From Agile / Uncle Bob
- They don't always need to be applied - just be pragmatic and strive for them in general

### 1. Single Responsibility Principle
> "A class should have just one reason to change"

- Every class should be responsible for a single part of the functionality of the software, and that responsibility should be encapsulated/hidden inside the class
- The point is to reduce complexity - classes become too big and you can't remember their details
- If a class does too many things, you have to change it everytime one of those things changes
- Example: `Employee` class responsible for both managing employee data and printing timesheets
  - Timesheet reporting might change in the future...
- Solution: Move timesheet stuff into a separate class. Give it a method `printTimesheet(employee)`;
  
### 2. Open/Closed Principle
> Classes should be open for extension but closed for modification

- **Keep existing code from breaking when you implement new features**
- Open & Closed at the same time is kind of a weird analogy but there are reasons for it
  - Open ... can extend it, make a subclass, add new methods, override base behaviour, etc.
  - Closed ... 10The interface is clearly defined and won't be changed in the future
- Changing a class that is already developed, tested, and included in other places is risky - instead create a subclass and override stuff that you want to behave differently

### 3. Liskov Substitution Principle
> When extending a class, remember that you should be able to pass objects of the subclass in place of objects of the parent class without breaking the client code

- Overridden methods should _extend_ the base behavior rather than replacing it with something else entirely
- This is the most clear and absolute principle... there are parameters
#### Rules
1. Parameter types in a method of a subclass should _match_ or be _more abstract_ than parameter types in the medhod of the superclass
   - Example: class has a method `feed(Cat c)`
   - Good subclass: `feed(Animal c)`
   - Bad subclass: `feed(BengalCat c)` - breaks if called with a TabbyCat
2. The return types in a method of the subclass should _match_ or be a _subtype_ of the return type in the method of the superclass
   - Example: superclass has `buyCat(): Cat`
   - Good subclass: `buyCat(): BengalCat`
   - Bad subclass: `buyCat(): Animal` - might break if buyCat returns a Dog
3. A method in a subclass shouldn't throw types of exceptions which the base method isn't expected to throw
   - The types of exceptions thrown should _match_ or _be subclasses_ of the ones the base class can throw
4. A subclass shouldn't strengthen pre-conditions
   - if the superclass has a parameter of type `int`, the subclass should NOT demand a _positive_ `**int**`. 
   - This is because the client code would now break when passing negative numbers to the subclass.
5. A subclass shouldn't weaken post-conditions
   - Example: superclass creates a db connection, does stuff, closes the db connection. Subclass should not override this function and leave the db connection open.
6. Invariants of a superclass must be preserved
   - **Invariants**: conditions in which an object makes sense
     - Easy invariants: explicitly defined in interfaces etc
     - But can also exist in the form of test **assertions**
   - This one isn't super formal but best to stick with it
   - Can be hard to know what all the invariants are
   - Safest way to extend a class is to leave existing stuff the same and introduce new stuff, but that's not always doable
7. A subclass shouldn't change the values of private fields of the superclass
   - Some languages (python, JS) don't have any protections for private members
![Bad example](https://user-images.githubusercontent.com/8353666/181045146-c74b1cc5-919e-43c0-ad31-0f09e44b2040.png)
![Good Example](https://user-images.githubusercontent.com/8353666/181045358-6033155b-c6e5-4a5b-9b23-9c5e7a537039.png)
   - The second version doesn't break `save()` in the subclass
### 4. Interface Segregation Principle
> "Clients shouldn't be forced to depend on methods they do not use"

- Try to make interfaces narrow/specific enough that you're not passing around a million things you don't need
- Break down "fat" interfaces into more granular ones
- Class inheritance lets a class have just one superclass, but it doesn't limit the number if interfaces the class can implement at the same time!
- So don't cram everything into one interface... just implement multiple in the subclass
#### Example
- library that deals with cloud providers
- not all of them have all the same features
![Bad example](https://user-images.githubusercontent.com/8353666/181047864-c8834cd7-bec4-419d-90c9-084b624edc50.png)
![Good example](https://user-images.githubusercontent.com/8353666/181047996-7e429fda-fd5f-4680-9896-b5ab18d402c0.png)
- don't want to put a bunch of stubs
- But it's also possible to go too far on this...

### 5. Dependency Inversion Principle
> "High-level classes shouldn't depend on low-level classes. Both should depend on abstractions. 
> Abstractions shouldn't depend on details. Details should depend on abstractions."

- Low-level: working with disk, network, etc
- High-level: business logic

It's a common pattern to create the low-level stuff first and then build the high-level stuff on top of it. But this can be problematic. Instead:
1. Describe interfaces for low-level stuff. `openReport(file)` instead of `openFile(x), readBytes(n), closeFile(x)`.
2. Make high-level classes dependent on those interfaces. This is a much 'softer' dependency because you can change the stuff that the interface uses and the high-level class doesn't have to care.
3. The low-level classes you build become _dependent_ on the interface
#### Example
- high-level budget reporting class uses a low-level database class... so if the database functions change, the budget reporting class needs to change also
- solution: interface that deals with this