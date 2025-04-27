## 📘 Personal OOP Recap with C++ Language Guide

---

### 1. Object-Oriented Programming (OOP) Recap

**OOP Overview:**
Object-Oriented Programming (OOP) is a paradigm that organizes software design around **objects**—entities that combine data (attributes) and behavior (methods). The goal is to model real-world systems, encapsulating data and operations in a modular, reusable, and maintainable way.

OOP promotes code **reusability**, **scalability**, and **maintainability** by enforcing principles like **encapsulation**, **inheritance**, **polymorphism**, and **abstraction**. Each concept serves a distinct purpose, and together they enable clean, flexible designs.

---

**Core Concepts (Detailed):**

1. **Encapsulation:**
   - *Definition:* Bundling data and the methods that operate on that data within a single unit (class), restricting direct access to some of the object's components.
   - *Why?* Encapsulation ensures data integrity and hides internal implementation details from the outside world, exposing only what is necessary via controlled interfaces (public methods).
   - *When to use?* Always. Every class should encapsulate its state to prevent unintended interference and to maintain a clear API.
   - *Benefits:* Data protection, modularity, and easier debugging.

   ```cpp
   class BankAccount {
   private:
       double balance;  // Private data cannot be accessed directly from outside
   public:
       void deposit(double amount) { balance += amount; }
       double getBalance() const { return balance; }  // Controlled access
   };
   ```

2. **Inheritance:**
   - *Definition:* A mechanism by which a class (child/derived) can acquire properties and behaviors (methods) from another class (parent/base).
   - *Why?* Promotes code reuse and logical hierarchy.
   - *When to use?* When there is an **"is-a"** relationship between entities (e.g., a Car is a Vehicle).
   - *Caution:* Favor **composition over inheritance** for flexibility ("has-a" relationships).
   - *Benefits:* Reduces redundancy, simplifies code maintenance, promotes hierarchy modeling.

   ```cpp
   class Vehicle {
   public:
       void start() { cout << "Vehicle started" << endl; }
   };

   class Car : public Vehicle {
   public:
       void honk() { cout << "Car horn!" << endl; }
   };
   ```

3. **Polymorphism:**
   - *Definition:* The ability for different classes to be treated as instances of the same base class through a common interface.
   - *Why?* Provides flexibility and allows for interchangeable components that can be processed uniformly.
   - *When to use?* When multiple classes share common behavior but implement it differently (e.g., `Shape` subclasses like `Circle`, `Square`).
   - *Types:*
     - **Compile-time (static):** Function overloading, operator overloading.
     - **Run-time (dynamic):** Achieved through virtual functions and inheritance.
   - *Benefits:* Promotes scalability and decouples code.

   ```cpp
   class Shape {
   public:
       virtual void draw() = 0;  // Pure virtual function
   };

   class Circle : public Shape {
   public:
       void draw() override { cout << "Drawing Circle" << endl; }
   };

   class Square : public Shape {
   public:
       void draw() override { cout << "Drawing Square" << endl; }
   };

   void render(Shape* s) { s->draw(); }
   ```

4. **Abstraction:**
   - *Definition:* Hiding the complex implementation details and exposing only the necessary functionality to the user.
   - *Why?* Reduces complexity by focusing on what an object does instead of how it does it.
   - *When to use?* Always aim to abstract internal workings of classes where possible.
   - *Mechanisms:*
     - **Abstract classes** (using pure virtual functions).
     - **Interfaces** (purely abstract classes).
   - *Benefits:* Encourages separation of concerns and clean API design.

   ```cpp
   class DatabaseConnection {
   public:
       virtual void connect() = 0;  // Abstract method
   };

   class MySQLConnection : public DatabaseConnection {
   public:
       void connect() override { cout << "Connecting to MySQL" << endl; }
   };
   ```

**Key Takeaway:**
Each OOP principle serves a specific role:
- **Encapsulation:** Data protection and modularity.
- **Inheritance:** Code reuse and hierarchy modeling.
- **Polymorphism:** Flexibility and decoupling.
- **Abstraction:** Complexity management and separation of concerns.

These principles, when combined, form the foundation of robust, scalable, and maintainable software design.

---

### 2. Essential Questions Before Modeling

- **What problem am I solving?**
- **What are the core entities in this domain?**
- **What behaviors (methods) do these entities exhibit?**
- **What are the relationships (associations, dependencies) between entities?**
- **Are these relationships "is-a" (inheritance) or "has-a" (composition)?**
- **Where are the boundaries?** (Domain, Infrastructure, Application Layers)
- **Are there potential future changes that should be isolated?**
- **What dependencies does each class have? Can I inject them?**

---

### 3. Principles of Design (SOLID)

**Overview:**
The **SOLID** principles are five design guidelines that help make object-oriented systems more maintainable, scalable, and robust. Each principle addresses a specific aspect of software design and encourages good engineering practices.

---

1. **Single Responsibility Principle (SRP):**
   - *Definition:* A class should have only **one reason to change**, meaning it should only have one job or responsibility.
   - *Why?* Simplifies debugging, testing, and maintenance. If a class has multiple responsibilities, changes in one area can unexpectedly affect others.
   - *When to apply?* Always question: "Does this class do more than one thing?" If yes, refactor.
   - *Benefits:* High cohesion, better separation of concerns.
   
   **Bad Example:**
   ```cpp
   class Report {
   public:
       void generateReport();  // Business logic
       void saveToFile();      // Persistence logic (violates SRP)
   };
   ```

   **Good Example:**
   ```cpp
   class ReportGenerator {
   public:
       void generateReport();
   };

   class ReportSaver {
   public:
       void saveToFile(const Report& report);
   };
   ```

---

2. **Open/Closed Principle (OCP):**
   - *Definition:* Software entities (classes, modules, functions) should be **open for extension, but closed for modification**.
   - *Why?* Allows you to add new functionality without changing existing, tested code, reducing the risk of introducing bugs.
   - *When to apply?* When extending functionality (e.g., adding new types, behaviors).
   - *Benefits:* Promotes code stability and scalability.

   **Example:**
   ```cpp
   class Shape {
   public:
       virtual double area() = 0;
   };

   class Circle : public Shape {
   private:
       double radius;
   public:
       Circle(double r) : radius(r) {}
       double area() override { return 3.14 * radius * radius; }
   };

   class Square : public Shape {
   private:
       double side;
   public:
       Square(double s) : side(s) {}
       double area() override { return side * side; }
   };
   ```
   - If a new shape (e.g., Triangle) is needed, create a new class. Existing code remains untouched.

---

3. **Liskov Substitution Principle (LSP):**
   - *Definition:* Objects of a superclass should be **replaceable** with objects of a subclass without affecting the correctness of the program.
   - *Why?* Ensures subclasses honor the expectations of the base class, avoiding surprises.
   - *When to apply?* Always when designing inheritance hierarchies.
   - *Benefits:* Predictability, safe polymorphism.

   **Violation Example:**
   ```cpp
   class Bird {
   public:
       virtual void fly();
   };

   class Ostrich : public Bird {
   public:
       void fly() override { throw logic_error("Ostrich can't fly!"); }  // Violates LSP
   };
   ```
   - Here, substituting `Bird` with `Ostrich` breaks the expectation that all birds can fly.

   **Solution:**
   - Redesign hierarchy: `FlyingBird` and `NonFlyingBird`.

---

4. **Interface Segregation Principle (ISP):**
   - *Definition:* Clients should **not be forced to depend on interfaces they do not use**. Favor smaller, specific interfaces over large, general-purpose ones.
   - *Why?* Prevents unnecessary dependencies.
   - *When to apply?* When designing interfaces for multiple clients with differing needs.
   - *Benefits:* Reduces coupling, improves code clarity.

   **Bad Example:**
   ```cpp
   class IMachine {
   public:
       virtual void print() = 0;
       virtual void scan() = 0;
       virtual void fax() = 0;
   };
   ```
   - A class that only prints (e.g., `SimplePrinter`) still has to implement `scan()` and `fax()`.

   **Good Example:**
   ```cpp
   class IPrinter {
   public:
       virtual void print() = 0;
   };

   class IScanner {
   public:
       virtual void scan() = 0;
   };
   ```
   - Classes implement only what they need.

---

5. **Dependency Inversion Principle (DIP):**
   - *Definition:* **High-level modules** should not depend on **low-level modules**. Both should depend on **abstractions**.
   - *Why?* Promotes flexibility and decouples system components.
   - *When to apply?* Always, especially in layered architectures.
   - *Benefits:* Easier to swap components, improves maintainability.

   **Without DIP:**
   ```cpp
   class FileLogger {
   public:
       void log(const string& message);
   };

   class Service {
   private:
       FileLogger logger;  // Direct dependency
   };
   ```

   **With DIP (using interfaces):**
   ```cpp
   class ILogger {
   public:
       virtual void log(const string& message) = 0;
   };

   class FileLogger : public ILogger {
   public:
       void log(const string& message) override;
   };

   class Service {
   private:
       ILogger* logger;
   public:
       Service(ILogger* logger) : logger(logger) {}
   };
   ```

---

**Key Takeaway:**
- **SRP:** Keep classes focused.
- **OCP:** Extend behavior without modifying existing code.
- **LSP:** Subtypes should be replaceable.
- **ISP:** Favor specific, minimal interfaces.
- **DIP:** Depend on abstractions, not implementations.

These principles guide you toward **flexible**, **robust**, and **scalable** system design.

### 4. Dependency Injection (DI)

**What is it?**
Dependency Injection (DI) is a design pattern in which an object's dependencies (services, tools, or collaborators that the object needs) are provided externally, instead of the object creating them internally.

**Analogy:** Imagine you are renting a car. Instead of building the car yourself, the rental company provides you with a car that you can use. You depend on a car, but you don't care how it was built, as long as it works.

**Why use DI?**
- **Decoupling:** Classes depend on abstractions (interfaces) rather than concrete implementations.
- **Flexibility:** Easily swap implementations (e.g., ConsoleLogger vs FileLogger).
- **Testability:** Makes unit testing easier by allowing mocks or stubs to be injected.
- **Scalability:** Encourages loosely coupled architecture, which is easier to maintain and extend.

Without DI (tight coupling):
```cpp
class Service {
private:
    ConsoleLogger logger;  // Direct dependency on a concrete type
public:
    void doSomething() {
        logger.log("Doing something");
    }
};
```
- The `Service` is tightly coupled to `ConsoleLogger`.
- You cannot easily replace `ConsoleLogger` with another logger (e.g., FileLogger) or mock it for testing.

With DI (loose coupling):
```cpp
class ILogger {
public:
    virtual void log(const string& message) = 0;
};

class ConsoleLogger : public ILogger {
public:
    void log(const string& message) override {
        cout << message << endl;
    }
};

class Service {
private:
    ILogger* logger;  // Depend on abstraction
public:
    Service(ILogger* logger) : logger(logger) {}
    void doSomething() {
        logger->log("Doing something");
    }
};

int main() {
    ConsoleLogger logger;
    Service service(&logger);  // Inject dependency at runtime
    service.doSomething();
}
```

**Types of Dependency Injection:**

1. **Constructor Injection:**
   - Dependencies are passed through the constructor.
   - Most common and preferred for mandatory dependencies.

   ```cpp
   class Service {
   private:
       ILogger* logger;
   public:
       Service(ILogger* logger) : logger(logger) {}
   };
   ```

2. **Setter Injection:**
   - Dependencies are set through public setter methods.
   - Useful for optional dependencies.

   ```cpp
   class Service {
   private:
       ILogger* logger;
   public:
       void setLogger(ILogger* logger) { this->logger = logger; }
   };
   ```

3. **Interface Injection:**
   - The dependency provides an injector method defined in an interface.

   ```cpp
   class ILoggerInjectable {
   public:
       virtual void injectLogger(ILogger* logger) = 0;
   };

   class Service : public ILoggerInjectable {
   private:
       ILogger* logger;
   public:
       void injectLogger(ILogger* logger) override { this->logger = logger; }
   };
   ```

**Common Pitfalls in DI:**
- **Over-injection:** Injecting too many dependencies into one class. If a class needs too many collaborators, it's likely violating SRP (Single Responsibility Principle).
- **Service Locator anti-pattern:** Avoid global service locators that provide dependencies; this can lead to hidden dependencies and tight coupling.
- **Not using abstractions:** Always depend on interfaces/abstract classes, not concrete implementations.

---

### 5. Common OOP Mistakes

1. **God Objects:** Classes that know too much or do too much.
2. **Excessive inheritance:** Prefer **composition over inheritance**.
3. **Tight coupling:** Makes code hard to change.
4. **Lack of abstraction:** Too many details exposed.
5. **Violating SRP:** Mixing unrelated responsibilities.

---

### 6. Naming Conventions & Class Types

**Classes:** PascalCase (e.g., `BankAccount`, `UserService`).

**Interfaces/Abstract Classes:** Prefix with 'I' or suffix with 'able' (e.g., `ILogger`, `Drawable`).

**Methods & Variables:** camelCase (e.g., `calculateTotal`, `userId`).

**Constants/Defines:** ALL_CAPS (e.g., `MAX_USERS`).

---

### 7. Project Organization (Layered Architecture)

- **Domain Layer:** Core business logic (Entities, Aggregates, Value Objects).
- **Application Layer:** Orchestrates tasks (Services, Use Cases).
- **Infrastructure Layer:** External concerns (Database, File System, APIs).
- **DTOs (Data Transfer Objects):** Simple structures for data movement between layers.

**Folder Structure Example:**
```
src/
├── domain/
│   ├── entities/
│   ├── value_objects/
│   └── services/
├── application/
│   └── use_cases/
├── infrastructure/
│   └── persistence/
└── dto/
```

---

### 8. Domain-Driven Design (DDD)

**Overview:**
Domain-Driven Design (DDD) is an approach to software development that emphasizes collaboration between technical experts and domain experts to create a **model** that reflects the core business logic. It promotes designing software around the business domain, ensuring that the system evolves with the business needs.

**Why use DDD?**
- Aligns software design with business goals.
- Encourages deeper understanding of the domain.
- Helps manage complexity in large systems.
- Provides a shared language between developers and business stakeholders (**Ubiquitous Language**).

**When to use DDD?**
- Suitable for **complex business domains** where deep domain knowledge and rules are critical.
- Less useful for CRUD-heavy or simple applications with minimal business logic.

---

**Core Concepts:**

1. **Entity:**
   - Has a distinct identity that runs through time.
   - Example: `Order`, `Customer` (identified by ID).

2. **Value Object:**
   - Describes attributes but has no distinct identity.
   - Immutable.
   - Example: `Money`, `Address`.

3. **Aggregate:**
   - A cluster of associated objects treated as a single unit.
   - One root entity (the **Aggregate Root**) controls access.
   - Example: An `Order` with a collection of `OrderItems`.

4. **Repository:**
   - Provides access to aggregate roots.
   - Handles persistence logic.

5. **Service (Domain Service):**
   - Encapsulates domain logic that doesn't naturally fit within an entity or value object.

6. **Bounded Context:**
   - Defines boundaries within which a particular domain model applies.
   - Different contexts may use the same terms differently.

7. **Ubiquitous Language:**
   - A shared language between developers and domain experts, used consistently across code and documentation.

---

**Layered Architecture in DDD:**

![DDD Layers](https://raw.githubusercontent.com/Sairyss/domain-driven-hexagon/main/docs/img/layers.png)

- **Domain Layer:** Core business rules (Entities, Value Objects, Aggregates, Domain Services).
- **Application Layer:** Coordinates tasks (use cases, orchestrates domain layer).
- **Infrastructure Layer:** Handles technical concerns (databases, messaging, APIs).
- **Interface Layer (or Presentation):** User interface or external communication.

---

**Practical Example in C++:**

**Value Object (Money):**
```cpp
class Money {
private:
    double amount;
    string currency;
public:
    Money(double amount, const string& currency) : amount(amount), currency(currency) {}
    double getAmount() const { return amount; }
    string getCurrency() const { return currency; }
};
```

**Entity (Order):**
```cpp
class Order {
private:
    int id;
    Money total;
public:
    Order(int id, const Money& total) : id(id), total(total) {}
    int getId() const { return id; }
    Money getTotal() const { return total; }
};
```

**Aggregate Root (Order):**
- `Order` could be the aggregate root managing its `OrderItems`.

**Repository Interface:**
```cpp
class IOrderRepository {
public:
    virtual void save(const Order& order) = 0;
    virtual Order findById(int id) = 0;
};
```

**Infrastructure Repository Implementation:**
```cpp
class FileOrderRepository : public IOrderRepository {
public:
    void save(const Order& order) override {
        // Persist order (mock implementation)
    }

    Order findById(int id) override {
        // Retrieve order (mock implementation)
        return Order(id, Money(100.0, "USD"));
    }
};
```

**Domain Service:**
```cpp
class OrderService {
private:
    IOrderRepository* repository;
public:
    OrderService(IOrderRepository* repository) : repository(repository) {}
    void processOrder(int id) {
        Order order = repository->findById(id);
        cout << "Processing order with total: " << order.getTotal().getAmount() << " " << order.getTotal().getCurrency() << endl;
    }
};
```

**Application Layer (Main):**
```cpp
int main() {
    FileOrderRepository repository;
    OrderService service(&repository);
    service.processOrder(1);
}
```

---

**Key Takeaways for DDD:**
- Focus on **business domain and logic** first.
- Collaborate closely with **domain experts**.
- Use **entities** for objects with identity.
- Use **value objects** for descriptive, immutable data.
- Control consistency boundaries with **aggregates**.
- Decouple persistence logic with **repositories**.
- Organize the system with **bounded contexts**.
- Maintain a **ubiquitous language** across code and communication.

---

### 9. DDD vs Layered Architecture (Domain, DTO, Infra)

**Clarifying DDD vs Traditional Layered Design:**

DDD provides **strategic and tactical patterns** to align software with business needs. The **layered architecture** (Domain, Application, Infrastructure, DTOs) is a **tactical implementation** approach commonly used within DDD but can also exist independently in non-DDD systems.

**Key Differences & Relationships:**

1. **Domain Layer (Core of DDD):**
   - Contains **Entities**, **Value Objects**, **Aggregates**, **Domain Services**.
   - Focuses on representing the business **rules** and **logic**.
   - Exists **only** when there is meaningful domain complexity.

2. **Application Layer:**
   - Coordinates **use cases** without containing business rules.
   - Delegates to domain objects to perform actions.
   - Present in **DDD** and **non-DDD** architectures.

3. **Infrastructure Layer:**
   - Handles technical details like persistence, messaging, file systems.
   - Exists in all layered systems.

4. **DTOs (Data Transfer Objects):**
   - Simple structures used to transfer data across layers or over network boundaries.
   - **DTOs are not part of the domain**; they serve to decouple internal models from external representations (e.g., APIs).

5. **Bounded Context (DDD only):**
   - Defines clear **boundaries** around a specific domain model and its language.
   - Multiple bounded contexts may exist within a system (e.g., Accounting, Shipping).

---

**Summary Table:**

| Concept               | DDD Specific? | Purpose                                          |
|-----------------------|---------------|--------------------------------------------------|
| **Domain Layer**      | Yes (core)    | Encapsulates business rules, models the domain.  |
| **Application Layer** | No            | Coordinates tasks, orchestrates domain logic.    |
| **Infrastructure**    | No            | Handles technical concerns (DB, APIs).           |
| **DTOs**              | No            | Transfers data between layers/systems.           |
| **Bounded Context**   | Yes (core)    | Defines domain model boundaries.                 |

**Conclusion:**
- **DDD** provides the **philosophy and structure** for modeling complex domains.
- **Layered architecture** (Domain, Application, Infra, DTO) is a **practical implementation pattern** used within or outside of DDD.
- The **Domain Layer** and **Bounded Contexts** are DDD-specific constructs that elevate layered architecture to handle business complexity effectively.

---

### 10. Layered Architecture (Domain, DTO, Infra) - Best Practices & Variations

**Overview:**
Layered Architecture, also known as N-tier architecture, is a design pattern that separates an application into distinct layers, each with specific responsibilities. This promotes separation of concerns, improves maintainability, and allows different teams to work independently on different parts of the system.

This architecture **originated from OOP principles**, as it aligns with **encapsulation** and **abstraction**, isolating business logic from technical concerns. Layers also complement **SOLID** principles by promoting **single responsibility** and **dependency inversion**.

**Core Layers and Common Additions:**

1. **Presentation Layer (Interface Layer):**
   - Handles user interactions (e.g., web interfaces, APIs).
   - Displays data and captures user input.
   - **Includes:** Controllers, Views, API Endpoints.

2. **Application Layer:**
   - Orchestrates **use cases**.
   - Manages transactions and coordination between the domain and infrastructure.
   - **Includes:** Application Services, Commands, Queries.

3. **Domain Layer:**
   - Encapsulates **core business logic**.
   - **Includes:** Entities, Value Objects, Aggregates, Domain Services.

4. **Infrastructure Layer:**
   - Handles **technical concerns** (databases, external APIs, messaging systems).
   - Provides concrete implementations for interfaces defined in other layers.
   - **Includes:** Repositories, Messaging, File Storage.

5. **DTOs (Data Transfer Objects):**
   - Transfer data between layers or across network boundaries.
   - Decouple **internal models** from external representations.

**Common Additional Layers/Modules in Real-World Projects:**

- **Security Layer/Module:** Handles authentication, authorization, and security concerns (e.g., token validation).
- **Exception Handling Layer:** Centralizes error handling and translates domain errors into appropriate responses (e.g., HTTP errors).
- **Business Rules Layer (Optional):** Sometimes separated to clarify domain rules or validations outside core entities.
- **Testing Layer:** Although not a runtime layer, having a clear **tests module** aligned with the architecture ensures each layer is tested appropriately.

**Example Folder Structure (Extended):**
```
src/
├── application/
│   └── use_cases/
├── domain/
│   ├── entities/
│   ├── value_objects/
│   └── services/
├── infrastructure/
│   ├── persistence/
│   └── messaging/
├── interfaces/
│   └── controllers/
├── dto/
├── security/
├── exceptions/
└── tests/
```

---

**Best Practices:**
- **Strict Dependency Rule:** Dependencies should always point inward (Infrastructure → Application → Domain).
- **Keep DTOs out of the Domain Layer:** Prevent pollution of business logic.
- **Centralize Exception Handling:** Ensure consistent error management across layers.
- **Isolate Security Concerns:** Keep security logic (e.g., token validation) separate from business logic.
- **Dependency Injection (DI):** Use interfaces to inject infrastructure services.
- **Test Each Layer Independently:** Focus unit tests on the domain and integration tests on application/infrastructure.

---

**Variations of Layered Architecture:**
1. **Hexagonal Architecture (Ports and Adapters):** Promotes domain independence from external systems.
2. **Onion Architecture:** Visualizes layers like an onion, emphasizing the Domain at the core.
3. **Clean Architecture:** Strong emphasis on dependency direction (inward) and separation of concerns.

---

**Diagram Example (Extended Clean Architecture):**
```
+---------------------------+
|      Presentation Layer   |
+---------------------------+
|  Security & Exception Mgt |
+---------------------------+
|      Application Layer    |
+---------------------------+
|         Domain Layer      |
+---------------------------+
|     Infrastructure Layer  |
+---------------------------+
```

**Key Takeaways:**
- **Layers originated from OOP**, leveraging encapsulation and abstraction.
- **SOLID principles** complement layered design by enforcing responsibilities and decoupling.
- Modularize common concerns (**security, exceptions, tests**) to maintain system clarity.
- Choose variations (**Hexagonal, Onion, Clean Architecture**) based on system complexity and business needs.

---

**Best Practices:**

- **Strict Dependency Rule:**
  - Dependencies should always point inward (Infrastructure → Application → Domain).
  - The Domain Layer should remain independent of external systems.

- **Keep DTOs out of the Domain Layer:**
  - Use DTOs to transfer data across layers, but **never pollute the domain with DTO structures**.

- **Dependency Injection (DI) for infrastructure:**
  - Inject infrastructure services (e.g., repositories) into the Application Layer using abstractions (interfaces).

- **Unit tests focus on the Domain Layer:**
  - Test business logic in isolation, mocking out infrastructure concerns.

- **Separate Read and Write models (CQRS variation):**
  - For complex systems, split **commands** (writes) and **queries** (reads) into separate models to optimize scalability.

---

**Variations of Layered Architecture:**

1. **Hexagonal Architecture (Ports and Adapters):**
   - Emphasizes the domain's independence from external systems.
   - External systems interact with the application via "ports" (interfaces), and adapters implement those ports.
   - Keeps the domain core clean and independent.

2. **Onion Architecture:**
   - Similar to hexagonal but visually layers the system like an onion.
   - The core is the Domain Layer, surrounded by Application, Infrastructure, and UI layers.

3. **Clean Architecture:**
   - Enforces strong separation between domain and external systems.
   - Similar to onion but adds explicit focus on the direction of dependencies (inward).

---

**Diagram Example (Clean Architecture):**
```
+-------------------+
| Presentation Layer|
+-------------------+
| Application Layer |
+-------------------+
|   Domain Layer    |
+-------------------+
| Infrastructure    |
+-------------------+
```

**Key Takeaways:**
- **Separation of concerns** is critical for maintainability.
- **Dependencies should always point inward**, protecting the Domain Layer.
- Use **DTOs** to decouple data representations.
- Consider **CQRS, Hexagonal, Onion, or Clean Architectures** for scalable, robust systems.

