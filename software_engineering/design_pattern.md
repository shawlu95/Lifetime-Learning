# Design Patterns

### Factory Method [[link](https://www.geeksforgeeks.org/design-patterns-set-2-factory-method/?ref=lbp)]
* an interface is defined
* multiple subclass can implement the interface, and object instantiation is deferred to subclass
* static factory method decides which subclass to instantiate, based on input params
* client passes some info/params to the factory method, and get the object

### Prototype [[Link](https://www.geeksforgeeks.org/prototype-design-pattern/?ref=lbp)]
* a prototype **registry** saves a string -> object mapping of objects (**prototypes**).
* client can get a new object by calling the registry, which clones a copy of the object.
* useful when objects are expensive and slow to create. Clone is faster.

#### Pros
* reduce number of classes

#### Cons
* each prototype (and subclass) most implement `clone` which may be difficult

___
### Builder [[link](https://www.geeksforgeeks.org/builder-design-pattern/)]
* **builder** defines an interface
* **concrete builder** implement the interface
* **director** accepts an instance of the concrete builder, build the object, and offers method returning the object

#### Pros
* build complex object step by step
* flexible construction params, no need to pass nulls

#### Cons
* a separate concrete builder for each type of product

___
### Singleton [[link](https://www.geeksforgeeks.org/singleton-class-java/?ref=lbp)]
* private constructor
* static method returning the only instance in existence
  - static property instantiate
  - static block, can handle exception
  - lazy instantiation (not thread safe)
- synchronize `getInstance`
  - **Bill Pugh** implementation: inner static class, instantiated only when `getInstance` is called

#### Dangers [[link](https://www.geeksforgeeks.org/prevent-singleton-pattern-reflection-serialization-cloning/?ref=lbp)]
* Reflection: use enum which doesn't give constructor definition
* Serialization: new instance is created when de-serialize. Implement `readResolve` in signleton class
* Cloning: override `clone` method: `throw new CloneNotSupportedException();`
