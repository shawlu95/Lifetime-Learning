# Design Patterns

### Factory Method [[link](https://www.geeksforgeeks.org/design-patterns-set-2-factory-method/?ref=lbp)]

- an interface is defined
- multiple subclass can implement the interface, and object instantiation is deferred to subclass
- static factory method decides which subclass to instantiate, based on input params
- client passes some info/params to the factory method, and get the object

### Prototype [[Link](https://www.geeksforgeeks.org/prototype-design-pattern/?ref=lbp)]

- a prototype **registry** saves a string -> object mapping of objects (**prototypes**).
- client can get a new object by calling the registry, which clones a copy of the object.
- useful when objects are expensive and slow to create. Clone is faster.

#### Pros

- reduce number of classes

#### Cons

- each prototype (and subclass) most implement `clone` which may be difficult

---

### Builder [[link](https://www.geeksforgeeks.org/builder-design-pattern/)]

- **builder** defines an interface
- **concrete builder** implement the interface
- **director** accepts an instance of the concrete builder, build the object, and offers method returning the object

#### Pros

- build complex object step by step
- flexible construction params, no need to pass nulls

#### Cons

- a separate concrete builder for each type of product

---

### Singleton [[link](https://www.geeksforgeeks.org/singleton-class-java/?ref=lbp)]

- private constructor
- static method returning the only instance in existence
  - static property instantiate
  - static block, can handle exception
  - lazy instantiation (not thread safe)

* synchronize `getInstance`
  - **Bill Pugh** implementation: inner static class, instantiated only when `getInstance` is called

#### Dangers [[link](https://www.geeksforgeeks.org/prevent-singleton-pattern-reflection-serialization-cloning/?ref=lbp)]

- Reflection: use enum which doesn't give constructor definition
- Serialization: new instance is created when de-serialize. Implement `readResolve` in signleton class
- Cloning: override `clone` method: `throw new CloneNotSupportedException();`

---

### Categories

- creation: abstract factory, builder, factory, prototype, singleton
- structural: adaptor, bridge, composite, decorator, facade, flyweight, proxy
- behavioral: chain of responsibility, command, interpreter, iterator, mediator, memento, observer, state, strategy, template method, visitor

### Bridge Pattern

- have a bridge class to handle request from client and delegate requests to implementation class
- decouple client code from implementation (i.e. change a database)
- migrating implementations is easy
- usually implementations for the same functionality

### Command Pattern

- implements different comnmands (functionality)
- commands are stored in a stack
- support redo, undo, revsersible, execute, get result
- each command has its own implementation class, reducing class size

### Proxy Pattern

- proxy object has the exact same interface as the substituted object (which is hidden)
- example: a remote impl class lives on server; a proxy class lives on the client side, with same interface as the impl, and handle network communication

### Template Method Pattern

- hide complex conditional, by defining control flow in superclass, called "template method"
- specific conditional blocks should be implemented by subclasses aka "hook methods"
- factory method pattern is a special case of template method, applied to object creation
- Strategy uses delegation, Template uses inheritance

### Factory Method Pattern

- an abstract class declares a template method
- each hook method returns an object, aka factory method

### Prototype Pattern

- a large number of similar classes, differing only in attributes, behavior and relationships, can be combined to a single class
- instantiate always from the prototype classes, and modify its attributes
- client calls `clone` method for all kinds of concrete prototypes
- cloning is more efficient than instantiating new object
