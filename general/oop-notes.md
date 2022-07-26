# OOP Notes
- Association is stronger than dependency
  - `Course` is a dependency (it's required in `teach`, and changing how `getKnowledge()` works could break `Professor`)
    - UML: dotted arrow line
  - `Student` is an association (same thing as before, changing `remember()` could break professor - but furthermore `Student` is available _everywhere_ inside `Professor`)
    - UML: solid arrow line
```
class Professor is
  field Student student
  // ...
  method teach(Course c) is
    // ...
    this.student.remember(c.getKnowledge())
```
- Aggregation (UML: empty diamond arrow line)
  - represents "one-to-many", "many-to-many" or "whole-part" relations
  - An object "as" a set of other objects and serves as a container/collection
- Composition (UML: filled diamond arrow line)
  - Similar to Aggregation
  - BUT the component can _only_ exist as part of the container
  
## Definitions
- **Dependency**: Class А can be affected by changes in class B.
- **Association**: Object А knows about object B. Class A depends on B.
- **Aggregation**: : Object А knows about object B, and consists of B. Class A depends on B.
- **Composition**: Object А knows about object B, consists of B, and manages B’s life cycle. Class A depends on B.
- **Implementation**:  Class А defines methods declared in interface B. Objects A can be treated as B. Class A depends on B.
- **Inheritance**: Class А inherits interface and implementation of class B but can extend it. Objects A can be treated as B. Class A depends on B.

![Relations between objects and classes: from weakest to strongest.](https://user-images.githubusercontent.com/8353666/180961378-df424219-cd52-4465-bd6f-abb8daf0889e.png)