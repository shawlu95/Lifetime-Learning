## Effective Java

### Chapter 2 Objects

1. prefer stafic factory methods to public constructors
   - do not make duplicate constructors with different params; pick descriptive factory methods instead
   - **instance-controlled**: factory methods control what instances exist
2. builder pattern
   - Similar patterns are: telescoping, Java Beans (may have inconsistent state)
   - builder steps: 1) pass required params to instantiate a builder object. 2) set optional params. 3) call **build()**
3. enforce singleton with private constructor or enum
   - the lack of a public or protected constructor guarantees a single instance
   - single-element enum type is the best way to implement singleton
4. use private constructors tp enforce non-instantiability
   - throw an **AssertionError** in private constructor to prevent being called from within the class
5. pass in underlying resource to public constructor
   - static utility class and singletons can't have dependent resources
6. avoid unncessaty instantiation
   - use static factory methods to avoid creating objects
   - prefer primitives to boxed primitives
7. eliminate obselete references
   - when a program manages its own memory
   - when caches are out-of-date
   - when registering listeners and callbacks
8. avoid finalizers and cleaners
   - can arbitrarily delay reclamation of resources
   - cannot be relied on to update persistant state
9. prefer _try-with-resource_ to _try-finally_
   - resource must implement the `AutoCloseable` interface which has a `close` method
   - multiple resources can be tried in a single block without additional nesting
   - can use `catch` to handle exception

### Chapter 3 Common Methods

10. Cautions: Overriding Equal

    - logical test is not identity test (reference)
    - value class usually needs logical test
    - for `EnumType` logical test is equal to identity test
    - general contracts:
      - reflexive: equal to itself
      - symmetric
      - transitive
      - consistent
      - Non-null
    - There's no way to extend an instantiable class and add a value component while preserving equals contract
    - Equal function must use deterministic computation

    - use Google's `AutoValue` framework

11. Override `hashcode` when overridding `equal`

    - needed for hash set and map
    - default `Objects.hash` is good for non-critical performance

---

### Chapter 4 Classes and Interfaces

15. minimize accessibility
    - Private: only accessible from top-level class in which it's declared
    - Package-private: accessible from any class in the package (default if not specified)
    - protected: accessible from subclasses
    - public: discouraged
    - _Public classes should have no public fields_
    - _ensure objects referenced by public static final fields are immutable_
    -
16. use accessor to access and modify fields
17. minimize mutability
    - **functional approach**: methods return a new object, without modifying the params
    - **procedural/imperative approach**: modify the input params
    - immutable objects make great map keys
    - static factory method encourages sharing instance (instance controlled)
    - provide _public mutable companion_ if needed (e.g. `StringBuilder`)
18. facor composition:
    - inheritance vioaltes encapsulation
    - new class has a **private** field that refereces an instance of the existing class
    - new class' methods are called **forwarding methods**
    - also known as **decorator** or **delegation**
19. design for inheritance or forbid it
    - inheritance requires huge maintenance commitment
    - use `@implSpec` tag to describe how overridable methods are implemented
    - test by writing subclasses
    - constructor must not invoke overridable methods: super class constructor is called before subclass constructor, before subclass is instantiated
    - if a class is designed to be inherited, do not implement `Cloneable` or `Serializable`
    - forbid inheritance: declare class as `final`; make constructor private; use static factory methods
20. Prefer interface to abstract class
21. Design interface
    - default method implementation allows new method to be added to interface without invoking compile error
    - not possible to maintain all invariants of every conceivable implementation
    - Avoid adding new method to existing interface unless absolutely needed
22. use interface to define type, not constant
    - declare constant in interface
    - or enum type
    - or non-instantiable utility class
23. avoid tagged class, use hirachical class

    - more flexible, allow new subclass to be added
    - minimize boilerplate

24. favor static member classes
    - four types of member class
      - static: no reference to the containing class, free from memory leak
      - non-static: cannot be insantiated without an isntance of the containing class
      - anonymous: to be used locally, non-static context only, e.g. function (now lambda is preferred)
      - local: only in non-static context
25. each source file should contain only one top-level class

---

### Chapter 5: Generics

---

### Chapter 6: Enums and Annotations

34. Use enums instead of int constant

- `int enum` and `string enum` are bad patterns
- enums are classes that export one instance for each enumeration constant via a public static final field
- eums types are instance-controlled, generalization of singletons which are enums with only one element
- enums are immutable, all fields must be final

35. Use instance fields instead of originals

- Example use case: `SOLO(1), DUET(2)`
- Designed for enum-based data structures such as `EnumSet` and `EnumMap`

36. Use `EnumSet` instead of bit field

- Each `EnumSet` is represented as a bit vector, super light weight
- EnumSet is bounded by integer/long size (32/64 bit)

37. Use `EnumMap` instead of ordinal indexing

- array is not compatible with generics
- combine the type safety of map with the speed of array
- never use orginal to index into arrays
