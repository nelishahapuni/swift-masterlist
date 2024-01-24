# Swift Masterlist

This documents contains a collection of Swift, OOP, SOLID, etc. concepts.

- [Optional](#optional)
- [Combine](#combine)
- [SwiftUI Property Wrappers](#swiftui-property-wrappers)
- [Struct VS Class](#struct-vs-class)
- [Auto Layout](#auto-layout)
- [MVVM](#mvvm)
- [Clean Architecture](#clean-architecture)
- [Navigation](#navigation)
- [Life Cycle Methods](#life-cycle-methods)
- [Generics](#generics)
- [Closures](#closures)
- [Automatic Reference Counting](#automatic-reference-counting)
- [Weak VS Unowned](#weak-vs-unowned)
- [Memory Leaks](#memory-leaks)
- [Codable](#codable)
- [Grand Central Dispatch](#grand-central-dispatch)
- [Async Await](#async-await)
- [Performance Optimization](#performance-optimization)
- [Store & Persist Data](#store--persist-data)
- [CI/CD Pipeline](#cdcd-pipeline)
- [App Signing](#app-signing)
- [Design Patterns](#design-patterns)
    1. [Factory](#1-factory-pattern)
    2. [Singleton](#2-singleton-pattern)
- [OOP](#oop)
- [Data Structures](#data-structures)
- [Algorithms](#algorithms)
    1. [Binary Search](#1-binary-search)
    2. [Merge Search](#2-merge-search)
    3. [Quick Sort](#3-quick-sort)
    4. [Depth First Search](#4-depth-first-search)
    5. [Breadth First Search](#5-breadth-first-search)
    6. [Greedy Algorithm](#6-greedy-algorithm)
- [SOLID Principles](#solid-principles)
    1. [Single Responsibility Principle](#1-single-responsibility-principle)
    2. [Open Closed Principle](#2-open-closed-principle)
    3. [Liskov Substitution](#3-liskov-substitution)
    4. [Interface Segregation](#4-interface-segregation)
    5. [Dependency Inversion](#5-dependency-inversion)

# Optional
- safe way of dealing with/potential nil (Swift is SAFE lang)
- If var nullable -> Optional (enum: none, some)

- Safe: optional chaining, conditional binding, etc.
- Unsafe: forced unwrapping 

# Combine
Publisher Subscriber Model
- Reactive framework 
- Asynchronous & event-driven code

- Sink - connects P&S & receives the value in the closure
- Cancellables - breaks connection between P&S
- Catch - handle errors from publisher
- Receive (on main thread)

@Published wrapper (SwiftUI) 
- declarative programming
- observable properties 
- automatic UI updates

# SwiftUI Property Wrappers
- @State, @Binding, and @ObservedObject to manage the state of views

```swift
class MyViewModel: ObservableObject {
    @Published var //...
}

struct ContentView: View {
    @ObservedObject var viewModel = MyViewModel()
    // ...
}

@Environment(\.colorScheme) var colorScheme (dark/light)
```
- managing shared data and configuration by injecting values into views

# Struct VS Class
Struct (Value Type)
- Best to start with, if class not yet needed (SwiftUI)
- Mutating func - change value of properties & write it back to the original structure
- properties of instance aren’t mutable (can’t be modified)

Class
- Inheritance, init/deinit, ARC
- properties of instance are mutable (can be modified)

BOTH
- Conform to protocols
- Encapsulation & abstraction

# Auto Layout
Importance:
- Adaptability to different screens: adjusts content dynamically for multiple devices

- Contains - width, height, size, distance, etc.
- Priority - satisfy higher priority first then lower

- Layout Anchors (top, bottom, leading, trailing)
- UIStackView - arrange in horizontal/vertical stacks

# MVVM
- We want: separation of concerns, testability and reusability.

- Model: Data (retrieval, manipulation, storage) & business logic 
- View: Displaying data with UI. Observes changes in the ViewModel & updates. Captures user input (UIKit - ViewController)
- ViewModel: Intermediary - presentation logic & exposes properties/methods that the View binds to. Fetches & transforms data from the Model. Initialised in the View Controller.

Two Way Binding: V + VM 
- automatically reflect changes on each other
- Ex. When ViewModel changes, view updates.
- Observability: Combine Framework

# Clean Architecture
- architectural pattern to create *modular, scalable, and maintainable* software by separating concerns into layers

1. Presentation Layer (UI)
    - SwiftUI View + View Model = Presentation Logic
    - Views: UI elements & user interactions
    - View Models: presentation logic & communicate with use case layer

2. Use Case Layer (Business Logic)
    - use cases encapsulate business logic and rules
    - independent of the UI and data source implementations
    - interacts with the entities and calls the data source interfaces for data retrieval or persistence

3. Entities:
    - domain-specific business objects or models
    - encapsulate the core data structures and business rules of the app

4. Data Source Layer (Gateway):
    - interfaces for data access, such as repositories or API clients
    - implemented by concrete classes or services that interact with external data sources
    - data source layer abstracts the details of data storage and retrieval

- Dependency Rule:
    - The direction of dependencies is strictly controlled, with the inner layers not depending on the outer layers
    - Dependencies flow from the outer layers (UI) toward the inner layers (use cases, entities)

# Navigation
(UIKit)
- UINavigationController manages a stack of view controllers, pushing or popping views
- Storyboard VS Programmatic Segues

Coordinator Pattern: 
- Solves “Massive VC Problem” & is reusable
- Coordinator Protocol - methods for starting, presenting, and finishing the flow
- Coordinators create and present VCs & communicate between each other, pass data, trigger navigation, etc.
- Benefits from Dependency Injection

# Life Cycle Methods 
(UIViewController)
- init - method for instance 
- viewDidLoad() - loaded in memory (one time setup, fetching data, init UI components)

(Pull up Sheet)
- viewWillAppear() - before view is about to appear on screen
- viewDidAppear() - after the view has appeared

- viewWillDisappear() - before about to disappear
- viewDidDisappear() - after view has disappeared (save data)
- deinit - when instance is deallocated

SwiftUI: onAppear & onDisappear

# Generics
- Parametrised Types for creating reusable funcs & structs that work with any Type
- Example: func swapValues<T>(_ a: inout T, _ b: inout T) { … }
- Often used w/collections: arrays, dictionaries, optionals, etc.
- Type safety because type-related errors are caught at compile time
- Use 'where' clause to add constraints, such as conforming to specific protocols
- TODO: - Add "some" keyword

# Closures
@escaping vs Non-escaping
- Closure: code that's defined in one place, but executed later
- Values they interact with should be in memory at that time
- Ability to capture values could lead to memory leaks
- After Swift 5.3, closures are non-escaping by default & you need to mark it with @escaping

- Non-escaping: lifetime doesn't exceed func - no mem leaks (.map() or .filter())

- Escaping: exec at later time, after func has returned
- Used for network calls
- Strong refs can cause retain cycles
- Use weak or unknown 

# Automatic Reference Counting
- automatically tracks memory used by instances of classes, to prevent memory leaks
- each instance has retain count, indicating # of refs that point to it
- Add new ref, remove when ref isn't needed
- Count is zero - instance is deallocated
- Supports weak/unowned to prevent retain cycles

# Weak VS Unowned
- refer to an instance without keeping a strong ref
- prevent retain cycles in closures or delegates

WEAK
-  used when the other instance has shorter lifespan
- the instance MIGHT dealloc before WEAK ref is used
- safely stored as OPTIONAL 

UNOWNED
- when other instance has equal or longer lifespan
- instance shouldn’t dealloc
- implicitly unwrapped - if wrong CRASH

- Top cause of leaks

# Memory Leaks
- If used memory exceeds allocated app memory - crash
- Retain cycles: closures or delegates
- Xcode Instruments > Leaks

# Codable 
- Protocol used to encode/decode data into external formats (eg. JSON)
- JSONEncoder (eg. into String)
- JSONDecoder (eg. Into JSON)
- Use CodingkKeys to customise (eg. The name of the property)

# Grand Central Dispatch
- Apple concurrency framework for executing multiple tasks simultaneously 

Use cases:
- Background Tasks - network calls, data processing, not blocking the main thread 
- Parallel Processing - multiple independent tasks to improve performance

- Main thread - Async UI updates to prevent freezing
- Dispatch Groups - used to monitor multiple asynchronous tasks & wait until they finish

# Async Await
(iOS 15)
- async return a value wrapped in a Task (SwiftUI)
- await used inside async func - waits for the completion of async task (ex. Fetch data) 

- Improved readability
- Avoids “Callback Hell” (multiple nested asynchronous callbacks) 
- Async funds can throws errors, can use do/catch to handle them

# Performance Optimization
- Lazy Loading - don’t init properties until they’re accessed (for resource-intensive operations)
- Async Programming (network calls, file operations, etc.)

- Be mindful of Memory Usage, avoid strong reference cycles
- Implement Caching
- Reuse Resources whenever possible to avoid init/deinit
- Only Load Data that’s currently needed

# Store & Persist Data
- User Defaults - small data (user preferences, settings, etc.) that persists across launches
- Keychain - encrypted - most secure (passwords)
- Read/write data from disk
- Third Party (Realm, Firebase, etc.)
- SQL Lite for complex data

# CD/CD Pipeline
(JENKINS)
Continuous Integration 
- Changes from multiple developers in shared repository (version control)
- Automatically build & run tests on new changes

Continuous Deployment
- Automatically deploy tested code changes to PROD environment 
- Faster, more efficient, less error-prone release
- Deployable Artefacts (.ipa build)

- Environment Promotion (dev > staging > prod)
- Rollbacks (if issues detected)

# APP Signing
- digital ID of app
- certificate issued by APPLE
- only executable code is signed

# Design Patterns

## 1. Factory Pattern
- Pattern for creating multiple different instances of a class & allowing the subclasses to alter the type of instances that will be created

```swift
protocol CoffeeFactory { 
    func makeCoffee(name: String) -> Coffee 
}
struct LatteFactory: CoffeeFactory {
    func makeCoffee(name: String) -> Coffee {
        return Coffee(name: name)
    }
}
struct AmericanoFactory: CoffeeFactory {
    func makeCoffee(name: String) -> Coffee {
        return Coffee(name: name)
    }
} 
```
When you want to create a new coffee object:

```swift
func makeAndServeCoffee(factory: CoffeeFactory, name: String) {
    let coffee = factory.makeCoffee(name: name) 
    coffee.serveCoffee()
}

let latteFactory = LatteFactory() 
makeAndServeCoffee(factory: latteFactory, name: "Latte")
```

## 2. Singleton Pattern

Class with a single shared instance, implemented with a static constant/method.
- Child class can't override static var or func.
- Uses: Network Manager, User Defaults, Location Sharing, Notification Manager, etc.
- Pros: Global Access, Lazy Initialisation, Centralised Configuration (settings), Resource Sharing 
- Cons: Anti-Pattern, Hard to test global & tightly coupled code, Race Conditions if there's thread conflicts

# OOP
Encapsulation
- Access controls (private, file private, public, etc.)
- Getter and Setter
- Property Observers (willSet, didSet)
- internal (default) - available within module
- final - prevent a class/method from being subclassed or overridden

Inheritance
- use methods & properties parent another class (using .super)
- Initialiser inheritance to customise init the call super.init()
- Overriding - subclass has own implementation of method
- Designated (must!) & Convenience initialisers (additional in subclass)

Abstraction (interface & protocols)
- Abstract Classes (protocols/classes with default implementation)
- Abstract Props & Methods
- Enums used to mimic abstract data types

Polymorphism (animal: cat, dog) 
- Method overriding (run) vs overloading (compile)
- Dynamic Dispatch - implementation of a method to call at runtime
- Protocol-Based Polymorphism

# Data Structures
- Arrays - constant-time access
- Linked Lists - elem stored in nodes in sequence (singly/doubly)
- Stacks - Last In, First Out - push, pop, peek (top elem) - call stack
- Queues - First In, First Out - Enqueue, Dequeue, peek - task scheduling
- Trees - hierarchical structure of nodes, each with/value & set of child nodes - Binary Trees
- Graphs - Nodes connected by edges - shortest path
- Hash Tables - maps keys to values w/functions - passwords, digital signature, caching, URL shortening, SHA-256 (cryptographic func)

# Algorithms 
// TODO: - Add code segments & more details

## 1. Binary Search
- Sorted list by dividing in half 
- O(log n)
## 2. Merge Sort 
- divide in two halves, recursively sort & merge sorted halves 
- O(n log n)
## 3. Quick Sort 
- select pivot, partition around it & recursively sort partitions 
- O(n^2)
## 4. Depth First Search 
- search entire branch & then backtrack - traversing trees & graphs 
- O(V + E)
## 5. Breadth First Search
- at current depth before moving to next
- O(V + E)
## 6. Greedy Algorithm 
- travelling salesman 
- shortest path

# SOLID Principles
// TODO: - Add code segments & more details

## 1. Single Responsibility Principle
- Avoid Class Monoliths - clear class responsibilities - break down functionality into smaller classes
- Decoupled code - easier to test
- Maintainable - changes to one part is less likely to affect others

## 2. Open Closed Principle 
- Open for extension, closed for modification
    1. Extensions to add functionality without modifying existing code
    2. Dependency Injection - substituting dependencies without modifying class
    3. Abstraction & Inheritance - New functionality can be added by inheriting from parent

## 3. Liskov Substitution 
- objs of superclass should be replaceable w/ objs subclass without affecting the program 
    1. Subtype Conformance - subclass inherits from base class & conforms to its protocols
    2. Overriding Methods - overriden method in subclass should have at least same behaviour as base
    3. Maintains the same method signature when subclassing or conforming to protocols

## 4. Interface Segregation 
- should conform only to necessary protocols 
    1. Smaller & Focused Protocols (Flyable - takeoff(), land(), fly()) Avoid Fat protocols
    2. Conform to multiple relevant protocols (Duck - Flyable, Swimmable)
    3. Provide Default Implementation when needed
    4. Protocol Composition (Protocol Walker, Talker) -> typealias Worker = Walker & Talker

## 5. Dependency Inversion
- Use Protocols as Abstractions: high-level modules rely on abstractions rather than concrete implementation (Protocol MessageSender: sendMessage() - class EmailSender, SMSSender) 
- Dependency Inject into high-level modules instead of creating them internally (constructor, property, or method injection)
- Inversion of Control Containers - Swinject 

