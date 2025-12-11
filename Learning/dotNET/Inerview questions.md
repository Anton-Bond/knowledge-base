#### .NET Framework 4.*  VS .NET Core 3.* VS .NET 5
NET Framework 4.* (wind only, slower) + .NET Core 3.* (crossplatform, faster) => .NET 5+ (unified platform)

---
#### IL 
(Intermideate Language) - is a partiali copliled code. To view it -> IL DASM

---
#### JIT
Just in Time compiler - it compiled IL code to Machine language. Optimize code depends on environment (win, linux, macos)

---
#### CLR
Common Language Runtime - 1) invokes JIT to compile to IL code, 2) cleans any unused objects by using GC

---
#### CTS
Common types system - ensures that data types defined in two different languages gets compiled to a common data type

---
#### CLS
Common Language Specification - is a specification or set of rules or guidelines. When any .NET programming language follows to these sets of rules it can be consumed by any language following .NET specifications

---
#### GC
Garbage Collector - is a background process which cleans unused Manage resources

---
#### Stack
- is memory type in app. It store data types like int, double, boolean and variable(pointer) for objects in Heap

---
#### Heap
- is memory type in app. It store data types like string and objects

---
#### Value types
= actual values (int, bool,..)

---
#### Reference types
= refer to values in Heap (objects)

---
#### Boxing and Unboxing
is moved value type to Reference type. Unboxing is viceversa

---
#### Casting
- a mechanism convert one data type to other type

---
#### Implicit casting
- move from lower to higher data type

---
#### Explicit casting
- move from higher to lower data type (data loss)

---
#### Threads
- run code in parallel

---
#### Delegate
- is a pointer to a func and very useful as callbacks to communicate between threads

---
#### Events
- are encapsulation over delegates

---
#### Abstract class
- a partially defined parent class, it is inherited

---
#### Interface
- is a contract, it is implemented. By having contract we have better control on change management and breaking changes.

---
#### Assempbly
- is the fundamental unit of deployment and versioning. It is a compiled code library used for deployment, versioning, and security. There are two main types: Process Assemblies (.exe) and Library Assemblies (.dll).

---
#### Middleware
- is software components that are assembled into an application pipeline to handle requests and responses. Each component:
 -- chooses whether to pass the request to the next component in the pipeline
 -- can perform work before and after the next component in the pipeline

---
#### LINQ
Language Integrated Query - is a set of features in .NET that adds native data querying capabilities to .NET languages using a syntax similar to SQL. (Query and Mehtod Syntax, Deferred and Immediate Execution, )

---
#### DI
- is a design pattern that implements Inversion of Control (IoC) for resolving dependencies. Instead of classes creating their own dependencies, they're provided ("injected") from the outside.
  Service Lifetimes:
  -- Transient: New instance created every time
  -- Scoped: One instance per HTTP request (web) or scope
  -- Singleton: Single instance for the application lifetime

---
#### Why OOP
- it's about the real world

---
#### Abstraction
- Show only what's necessary (it's Design phase)

---
#### Polymorphism
- Objects acts differently under different conditions

---
#### Inheritance
- Parent Child relationship

---
#### Encapsulation
- Hide complexity (it's about execution, implementation of Abstraction)

---
#### Class
- is type, is blueprint

---
#### Object
- is instance of the class

---
#### Virtual and Override keywords
- parent method can be overridden in child class

---
#### Overloading
- methods with the same names but different signature

---
#### Static Polimorphism
- implemented by Method overloading

---
#### Dynamic Polimorphism
- implemented by Method overriding

---
#### Myltiple Interface
- helps to add new methods without affecting the old interfaces

---
#### S
Single Responsibility Principle - one class should only do one thing at a time

---
#### O
Open Closed Principle - class should be open for extension and closed for modification (Rather than modifying the existing class inherit and create a new class)

---
#### L
Liskov Substitution Principle - Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program (the child class should be able to implement all the methods of the parent class without side effects)

---
#### I
Interface Segregation Principle - client should not be forced to use Methods which it does not need

---
#### D
Dependency Inverstion - High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.

---
#### Architecture Style
defines the quidelines and principles that shape the overall structure (REST, SOA, ...)

---
#### Architecture Pattern
provides general structure/layers for designed software system (MVC, Layered architecture, MVVM...)

---
#### Design Pattern
has a very specific context and also demonstate code (Singleton, Factory, ...)

---
#### Design Patterns
are time tested solutions for recurring arhitecture problems
1) Creational category (problems and solutions around object creation issues):
 - Singletone - a class of which only a single instance can exist
 - Prototype - a fully initialized class to be copied or cloned
 - Factory - most use one. Creates an instance of several derived classes. Used when third party components are used.
2) Behavioral category (problems and solutions around communication between objects):
 - Template - difine a skeleton of the algorithm in parent class and let subclass override specific steps
 - Chain of responsibility - a way of passing a request between chain of objects
 - Command - encapsulate a command request as an object
 - Mediator - defines simplified communication between classes
 - Memento - capture and restore an object's internal state
 - Observer - where an object (the **subject**) maintains a list of its dependents (**observers**) and notifies them automatically of any state changes, usually by calling one of their methods.
1) Structural cat (solving concerns around structure and object composition): 
 - Adapter helps to make incompitable interfaces compitable
 - Composite - a tree structure of simple and composite objects
 - Decorator - add responsibilities to object dynamically
#### ! Repository pattern
- acts like an abstract layer between Models and Data access technologies like EF, ADO.NET and so on. Data access logic is centralized making code maintainable, testable and modular.
#### ! UOW (Unit of work)
- this pattern helps to manage transactions and changes made to objects. It goes with Repository pattern well.

---
#### MVC
- arhitecture where we divide the project into three primary layers:
 -- Controller - takes user inputs, load models and passes the view
 -- View - handles the look
 -- Model - has business logic

---
#### MVP
- arhitecture where we divide the project into three primary layers:
 -- Presenter - takes input from View, loads the Model and updates the View accordingly
 -- View - takes user control, handles the look. It forwards all user interaction to the Presenter
 -- Model - has business logic

---
#### MVVM
- arhitecture where we divide the project into three primary layers:
 -- ViewModel - is mediator between View and Model and does all heavy lifting. It talk with Model and updates the View. ViewModel and View are binded through automated bindings
 -- View - takes user control, handles the look. All actions and updates to view happens through automated bindings
 -- Model - has business logic

---
#### REST
Representational State Transfer - is an architectural style for designing networked applications. REST APIs use HTTP protocols to perform operations on resources.
	Key REST Principles:
	- Stateless: Each request contains all information needed to process it
	- Client-Server: Separation of concerns
	- Cacheable: Responses can be cached
	- Uniform Interface: Consistent API design
	- Layered System: Architecture can have multiple layers
	- HTTP Methods
	- Coresponded Code statuses
	- versioning

---
#### DRY
Don't Repeat Yourself - Principle: Write it once, use it everywhere. (If you're copying and pasting the same code in multiple places, you're doing it wrong)

---
#### KISS
Keep It Simple, Stupid - Principle: Simplicity should be a key goal in design, and unnecessary complexity should be avoided.

---
#### YAGNI
You Aren't Gonna Need It
- Principle: Don't implement functionality until you actually need it.

---
#### SoC
Separation of Concerns - Principle: Split your program into distinct sections that each address a separate concern.

---

#### Important Security Concepts:
#### CORS
Cross-Origin Resource Sharing - A mechanism that allows or restricts web pages from making requests to a different domain than the one that served the web page.

---
#### Anti-Forgery Tokens
CSRF Protection - Prevents Cross-Site Request Forgery attacks where a malicious site tricks users into submitting requests to your site.
	How it works: The server generates a unique token that must be included in form submissions.

---
#### XSS
Cross-Site Scripting Protection - is a security vulnerability that allows attackers to inject malicious scripts into web pages viewed by other users.
	How It Works:
		Attackers find a way to insert malicious code into a website
		Victims visit the website and the malicious code runs in their browser
		The code can steal cookies, session tokens, or redirect users to fake websites

---
#### CSP
Content Security Policy - is a security standard that tells browsers which sources of content are allowed to load on your website. (set HTTP Header: Content-Security-Policy: default-src 'self'; script-src 'self')
		How It Works: You create a "whitelist" of trusted sources, and the browser blocks anything that's not on the list.

---
#### HTTPS/SSL
- Forces secure encrypted connections

---
#### Input Validation
- prevent malicious data input

---
#### Authentication
= Who are you? (Login)

---
#### Authorization
= What are you allowed to do? (Permissions)

---
#### SQL Injection Protection

#### "Start?"
"I'm available to start immediately"; "I can start at any time."

---
#### "Vacation?"
"No, I don't have any vacations planned for the immediate future."

---
#### "Overtime?"
"I understand that sometimes overtime is necessary to meet important deadlines or goals. I'm absolutely fine with that and willing to put in the extra effort when the situation requires it. I'm genuinely passionate about my work."

---