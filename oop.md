# Object Oriented Programming Design and Principles

## Goals of OOP

- Flexible
- Extensible
- Reusable

---

### Quick Definitions

- **Client Code**: Any code that has access to that object. Only knows object by its public interface.
- **Interface**: The set of all functions and operators a class defines _publicly_.
- **Implementation**: The definition of an objects interface. The client does not know about this. State and the definitions of member functions and the operators.

## 4 Principles

- Encapsulation
- Abstraction
- Inheritance
- Polymorphism

### 1. Encapsulation

Data and behaviors are encapsulated together behind an interface. - Member functions have direct access to the member variables of the object via _this_. This simplifies function calls.

**Proper _encapsulation_**:

1. Data of a class remains internal.
2. Client can only interact with the data via the interface.

### 2. Abstraction

An object presents only the necessary interface to client code.

- Hides unnecessary implementation details from the client.
  - Member functions that client code does not need should be private or protected.

### 3. Inheritance

- **Implementation Inheritance**
  - Class inherits interface and implementation of its base class.
- **Interface Inheritance** (abstract base class in C++, pure virtual functions)
  - Abstract classes in C++, known as _interfaces_ in Java.
  - **Benefits**
    - Reduce dependencies between base/derived class
    - (Flexible, Extensible, Reusable) Code the client to depend on an interface rather than a specific implementation.

#### **Multiple Inheritance**

- Class should inherit **implementation** from one class only (_extends_ in Java, not forced in C++)
- Class can inherit as many interfaces it would like. (_implements_ in Java)

### 4. Polymorphism

A single interface may have many different implementations (virtual functions and function overriding in C++)

**Benefits:**

- Avoid nasty switch statements (function calls resolve dynamically.)
- (Flexible) Allows the implementation of an interface to change at **run time**.

---

### Programming interfaces in C++

- Abstract base class using pure virtual functions to declare the interface.
- Implement the interface in subclasses via public inheritance.
- Client maintains reference or pointer to the base class.
- Calls through the reference or pointer is polymorphic.

## Adapter Pattern

Can be implemented using:

- Inheritance (creates dependencies)
- Composition (uses composed object's interface)

# Design Patterns

### Creational Patterns

- Describes how objects are created and initialized.
  - Builder, abstract factory, factory singleton, prototype.

## Singleton Pattern

Enforce only single object of given type exists. - Instantiates the object on demand. - Easy to obtain an alias from anywhere in program

```cpp
// Portfolio.h
class Portfolio {
private:
    static Portfolio * instance_; // Static pointer to the only object of this class. Not a member variable.
    Portfolio (); // IMPORTANT, constructor MUST be PRIVATE.
    virtual ~Portfolio (); // IMPORTANT, destructor MUST be PRIVATE.
public:
    static Portfolio * instance(); // Function that allows access to the singleton object.
}
    Portfolio * Portfolio::instance_ = 0; // Init singleton to 0.
    Portfolio * Portfolio::instance() { // returns access to the obj. Inits if == 0.
    if (instance_ == 0){
        instance_ = new Portfolio;
    }
    return instance_;
    }
    void Portfolio::fini() { // Destructs the obj.
    delete instance_;
    instance_ = 0;`

    }
```

```cpp
// main.cpp
int main (int, char * []) {
    try {
        Stock *s = new Stock ("Alice's Restaurant", 20, 7, 11, 13);
        Bond *b = new Bond ("City Infrastructure", 10, 2, 3, 5);
        Portfolio::instance()->add (s);
        Portfolio::instance()->add (b);
        Portfolio::instance()->print ();
        Portfolio::fini();
    }
    catch (Portfolio::error_condition &e) {
        cout << "Portfolio error: " << e << endl;
        return -1;
    }
    catch (...) {
        cout << "unknown error" << endl;
        return -2;
    }
    return 0;
}
```

## Prototype Pattern

- Wraps copy construction within a common virtual method
- Each subclass overrides that method: uses its specific copy constructor with dynamic allocation to “clone” itself.

```cpp
//Security.h
struct Security {
  public:
    virtual Security * clone () = 0; // Pure virtual method.
};

Security * Stock::clone () {
return new Stock(*this);
}

Security * Bond::clone () {
    return new Bond(*this);
}

Security * Agent::sell (Security *s){
    Security * current = Portfolio::instance(this)->find(s);
    if (current == 0) {
        throw cannot_provide;
    }
    Security * copy = current->clone(); // Dynamically bind to the clone function of the class.
    Portfolio::instance(this)->remove(current);
    reserve_ += copy->shares_ * copy->current_value_;
    return copy;
}
```

## Factory Method Pattern

Creating a related type polymorphically using a factory class.

_See Studio 18 assignment for implementation._

# Behavioral Design Patterns

Concerned with algorithms and how responsibilities are assigned between objects.

- Patterns of communiciation between objects at run time.

## Strategy Pattern

Define a family of algorithms that are interchangeable at run-time. Decouples client from the specific algorithms it uses.

- Share abstract base class defining family for share via SMS, email, etc.

### Object Modeling Technique (OMT)

![Strategy Pattern OMT](strategy-pattern-omt.png)

- Italicised are **abstract base class**. (eg. _Strategy_)
  - _AlgorithmInterface()_ is a pure virtual member function.