# Swift Masterlist

This documents contains a collection of Swift, OOP, SOLID, etc. concepts.

- [Swift](#swift)
    - [Structs Classes ](#structs--classes)
    - [Automatic Reference Counting](#automatic-reference-counting)
    - [Optional](#optional)
    - [Generics](#generics)
- [UIKit](#uikit)
    - [Auto Layout](#auto-layout)
    - [Navigation](#navigation)
    - [Life Cycle Methods](#life-cycle-methods)
- [Combine](#combine)
- [SwiftUI](#swiftui)
    - [Property Wrappers](#property-wrappers)
- [Architectural Patterns](#architectural-patterns)
    - [MVC](#mvc)
    - [MVP](#mvp)
    - [MVVM](#mvvm)
    - [VIPER](#viper)
    - [Clean Architecture](#clean-architecture)
- [Closures](#closures)
- [Weak VS Unowned](#weak-vs-unowned)
- [Memory Leaks](#memory-leaks)
- [Codable](#codable)
- [Grand Central Dispatch](#grand-central-dispatch)
- [Async Await](#async-await)
- [Performance Optimization](#performance-optimization)
- [Store & Persist Data](#store--persist-data)
- [CI/CD Pipeline](#cdcd-pipeline)
    - [Continuous Integration](#continuous-integration)
    - [Continuous Deployment](#continuous-deployment)
- [App Signing](#app-signing)
- [Asynchronous VS Synchronous](#asynchronous-vs-synchronous)
- [Design Patterns](#design-patterns)
    - [Factory](#1-factory-pattern)
    - [Singleton](#2-singleton-pattern)
- [Object Oriented Programming](#object-oriented-programming)
    - [Encapsulation](#encapsulation)
    - [Inheritance](#inheritance)
    - [Abstraction](#abstraction)
    - [Polymorphism](#polymorphism)
- [Data Structures](#data-structures)
    - [Arrays](#arrays)
    - [Linked Lists](#linked-lists)
    - [Stacks](#stacks)
    - [Queues](#queues)
    - [Trees](#trees)
    - [Graphs](#graphs)
    - [Hash Tables](#hash-tables)
- [Algorithms](#algorithms)
    - [Binary Search](#1-binary-search)
    - [Merge Search](#2-merge-search)
    - [Quick Sort](#3-quick-sort)
    - [Depth First Search](#4-depth-first-search)
    - [Breadth First Search](#5-breadth-first-search)
    - [Greedy Algorithm](#6-greedy-algorithm)
- [SOLID Principles](#solid-principles)
    - [Single Responsibility Principle](#1-single-responsibility-principle)
    - [Open Closed Principle](#2-open-closed-principle)
    - [Liskov Substitution](#3-liskov-substitution)
    - [Interface Segregation](#4-interface-segregation)
    - [Dependency Inversion](#5-dependency-inversion)

# Swift

Concepts related to the core swift language principles.

## Structs & Classes

### Struct
- value type
- best to start with, if class not yet needed (SwiftUI)
- **mutating func** - change value of properties & write it back to the original structure
- properties of instance aren't mutable (can't be modified)
- alloc on Stack

### Class
- reference type
- init/deinit, ARC
- supports inheritance
- properties of instance are mutable (can be modified)
- alloc on Heap

### Both
- conform to protocols
- encapsulation & Abstraction

## Optional
- safe way of dealing with/potential nil (Swift is SAFE lang)
- If var nullable -> Optional (enum: none, some)

- Safe: optional chaining, conditional binding, etc.
- Unsafe: forced unwrapping 

## Automatic Reference Counting
- automatically tracks memory used by instances of classes, to prevent memory leaks
- each instance has retain count, indicating # of refs that point to it
- Add new ref, remove when ref isn't needed
- Count is zero - instance is deallocated
- Supports weak/unowned to prevent retain cycles

## Generics
- Parametrised Types for creating reusable funcs & structs that work with any Type
- func swapValues<T>(_ a: inout T, _ b: inout T) { ... }
- Often used w/collections: arrays, dictionaries, optionals, etc.
- Type safety because type-related errors are caught at compile time
- Use 'where' clause to add constraints, such as conforming to specific protocols
- TODO: - Add "some" keyword

# UIKit

## Auto Layout
- Adaptability to different screens: adjusts content dynamically for multiple devices

- Contains - width, height, size, distance, etc.
- Priority - satisfy higher priority first then lower

- Layout Anchors (top, bottom, leading, trailing)
- UIStackView - arrange in horizontal/vertical stacks

## Navigation
- UINavigationController manages a stack of view controllers, pushing or popping views
- Storyboard VS Programmatic Segues

### Coordinator Pattern
- Coordinator Protocol - methods for starting, presenting, and finishing the flow
- Coordinators create and present VCs & communicate between each other, pass data, trigger navigation, etc.
- Benefits from Dependency Injection

## Life Cycle Methods 
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

# Combine
- Publisher Subscriber Model
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

# SwiftUI 

## Property Wrappers
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

# Architectural Patterns

## MVC
// TODO: - add info

## MVP
// TODO: - add info

## MVVM
- We want: separation of concerns, testability and reusability.

- Model: Data (retrieval, manipulation, storage) & business logic 
- View: Displaying data with UI. Observes changes in the ViewModel & updates. Captures user input (UIKit - ViewController)
- ViewModel: Intermediary - presentation logic & exposes properties/methods that the View binds to. Fetches & transforms data from the Model. Initialised in the View Controller.

Two Way Binding: V + VM 
- automatically reflect changes on each other
- Ex. When ViewModel changes, view updates.
- Observability: Combine Framework

## VIPER
// TODO: - add info

## Clean Architecture
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

# Weak VS Unowned
- refer to an instance without keeping a strong ref
- prevent retain cycles in closures or delegates

WEAK
-  used when the other instance has shorter lifespan
- the instance MIGHT dealloc before WEAK ref is used
- safely stored as OPTIONAL 

UNOWNED
- when other instance has equal or longer lifespan
- instance shouldn't dealloc
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
- Avoids "Callback Hell" (multiple nested asynchronous callbacks) 
- Async funds can throws errors, can use do/catch to handle them

# Performance Optimization
- Lazy Loading - don't init properties until they're accessed (for resource-intensive operations)
- Async Programming (network calls, file operations, etc.)

- Be mindful of Memory Usage, avoid strong reference cycles
- Implement Caching
- Reuse Resources whenever possible to avoid init/deinit
- Only Load Data that's currently needed

# Store & Persist Data
- User Defaults - small data (user preferences, settings, etc.) that persists across launches
- Keychain - encrypted - most secure (passwords)
- Read/write data from disk
- Third Party (Realm, Firebase, etc.)
- SQL Lite for complex data

# CD/CD Pipeline

## Continuous Integration 
- jenkins
- Changes from multiple developers in shared repository (version control)
- Automatically build & run tests on new changes

## Continuous Deployment
- Automatically deploy tested code changes to PROD environment 
- Faster, more efficient, less error-prone release
- Deployable Artefacts (.ipa build)

- Environment Promotion (dev > staging > prod)
- Rollbacks (if issues detected)

# APP Signing
- digital ID of app
- certificate issued by APPLE
- only executable code is signed

# Asynchronous VS Synchronous
- Async is non-blocking, which means it will send multiple requests to a server. 
- Sync is blocking it will only send the server one request at a time and wait for that request to be answered by the server.

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

# Object Oriented Programming

## Encapsulation
- Access controls (private, file private, public, etc.)
- Getter and Setter
- Property Observers (willSet, didSet)
- internal (default) - available within module
- final - prevent a class/method from being subclassed or overridden

## Inheritance
- use methods & properties parent another class (using .super)
- Initialiser inheritance to customise init the call super.init()
- Overriding - subclass has own implementation of method
- Designated (must!) & Convenience initialisers (additional in subclass)

## Abstraction
- interface & protocols
- Abstract Classes (protocols/classes with default implementation)
- Abstract Props & Methods
- Enums used to mimic abstract data types

## Polymorphism 
- Method overriding (run) vs overloading (compile)
- Dynamic Dispatch - implementation of a method to call at runtime
- Protocol-Based Polymorphism

# Data Structures

## Arrays 
    - constant-time access
## Linked Lists
    - elem stored in nodes in sequence (singly/doubly)
## Stacks 
    - Last In, First Out 
    - push, pop, peek (top elem) 
    - call stack
## Queues 
    - First In, First Out 
    - enqueue, dequeue, peek 
    - task scheduling
## Trees 
    - hierarchical structure of nodes, each with/value & set of child nodes 
    - Binary Trees
## Graphs 
    - Nodes connected by edges 
## Hash Tables 
    - maps keys to values w/functions 
    - passwords, digital signature, caching, URL shortening, SHA-256 (cryptographic func)

# Algorithms 
// TODO: - Add code segments & more details

## Binary Search
- Sorted list by dividing in half 
- O(log n)
## Merge Sort 
- divide in two halves, recursively sort & merge sorted halves 
- O(n log n)
## Quick Sort 
- select pivot, partition around it & recursively sort partitions 
- O(n^2)
## Depth First Search 
- search entire branch & then backtrack - traversing trees & graphs 
- O(V + E)
## Breadth First Search
- at current depth before moving to next
- O(V + E)
## Greedy Algorithm 
- travelling salesman 
- graphs, shortest path

# SOLID Principles

## Single Responsibility Principle
- Avoid Class Monoliths - clear class responsibilities - break down functionality into smaller classes
- Decoupled code - easier to test
- Maintainable - changes to one part is less likely to affect others

## Open Closed Principle 
- Open for extension, closed for modification
- Extensions to add functionality without modifying existing code
    2. Dependency Injection - substituting dependencies without modifying class
    3. Abstraction & Inheritance - New functionality can be added by inheriting from parent

## Liskov Substitution 
- objs of superclass should be replaceable w/ objs subclass without affecting the program 
    1. Subtype Conformance - subclass inherits from base class & conforms to its protocols
    2. Overriding Methods - overriden method in subclass should have at least same behaviour as base
    3. Maintains the same method signature when subclassing or conforming to protocols

## Interface Segregation 
- should conform only to necessary protocols 
    1. Smaller & Focused Protocols (Flyable - takeoff(), land(), fly()) Avoid Fat protocols
    2. Conform to multiple relevant protocols (Duck - Flyable, Swimmable)
    3. Provide Default Implementation when needed
    4. Protocol Composition (Protocol Walker, Talker) -> typealias Worker = Walker & Talker

## Dependency Inversion
    - Use Protocols as Abstractions: high-level modules rely on abstractions rather than concrete implementation (Protocol MessageSender: sendMessage() - class EmailSender, SMSSender) 
    - Dependency Inject into high-level modules instead of creating them internally (constructor, property, or method injection)
    - Inversion of Control Containers - Swinject 
