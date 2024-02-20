# Swift Masterlist

This documents contains a collection of Swift, OOP, SOLID, etc. concepts.

- [Swift](#swift)
    - [Structs Classes Actors](#structs-classes-actors)
        - [Structs](#structs)
        - [Classes](#classes)
        - [Actors](#actors)
    - [Automatic Reference Counting](#automatic-reference-counting)
    - [Optional](#optional)
    - [Generics](#generics)
    - [Closures](#closures)
    - [Weak VS Unowned](#weak-vs-unowned)
    - [Memory Leaks](#memory-leaks)
    - [Codable](#codable)
- [UIKit](#uikit)
    - [Auto Layout](#auto-layout)
    - [Navigation](#navigation)
    - [Life Cycle Methods](#life-cycle-methods)
    - [UIGestureRecognizer](#uigesturerecognizer)
- [SwiftUI](#swiftui)
    - [Property Wrappers](#property-wrappers)
- [Architectural Patterns](#architectural-patterns)
    - [MVC](#mvc)
    - [MVP](#mvp)
    - [MVVM](#mvvm)
    - [VIPER](#viper)
    - [Clean Architecture](#clean-architecture)
- [Concurrency](#concurrency)
    - [Grand Central Dispatch vs Manual Thread Creation](#grand-central-dispatch-vs-manual-thread-creation)
    - [Asynchronous vs Synchronous](#asynchronous-vs-synchronous)
    - [Serial vs Concurrent Queue](#serial-vs-concurrent-queue)
    - [Dispatch Queues](#dispatch-queues)
        - [Quality of Service](#quality-of-service)
        - [Attributes of Queues](#attributes-of-queues)
        - [Target Queues](#target-queues)
        - [Auto Release Frequency](#auto-release-frequency)
        - [Dispatch Group](#dispatch-group)
        - [Dispatch Work Item](#dispatch-work-item)
    - [Async Await](#async-await)
- [Performance Optimization](#performance-optimization)
- [Store & Persist Data](#store--persist-data)
- [CI/CD Pipeline](#cdcd-pipeline)
    - [Continuous Integration](#continuous-integration)
    - [Continuous Deployment](#continuous-deployment)
- [App Signing](#app-signing)
- [Frameworks](#frameworks)
    - [Combine](#combine)
    - [RxSwift](#rxswift)
    - [AVFoundation](#avfoundation)
    - [Core Location](#core-location)
    - [Charts](#charts)
- [Extensions](#extensions)
    - [Action](#action)
    - [Share](#share)
    - [Notification Service](#notification-service)
- [Design Patterns](#design-patterns)
    - [Design Pattern Classes](#design-pattern-classes)
    - [Observer](#observer)
    - [Command](#command)
    - [Builder](#builder)
    - [Factory](#factory)
    - [Singleton](#singleton)
- [Object Oriented Programming](#object-oriented-programming)
    - [Encapsulation](#encapsulation)
    - [Inheritance](#inheritance)
    - [Abstraction](#abstraction)
    - [Polymorphism](#polymorphism)
- [Functional Programming](#functional-programming)
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
    - [Greedy Algorithms](#6-greedy-algorithms)
- [SOLID Principles](#solid-principles)
    - [Single Responsibility Principle](#1-single-responsibility-principle)
    - [Open Closed Principle](#2-open-closed-principle)
    - [Liskov Substitution](#3-liskov-substitution)
    - [Interface Segregation](#4-interface-segregation)
    - [Dependency Inversion](#5-dependency-inversion)

# Swift

Concepts related to the core swift language principles.

## Structs Classes Actors

### Structs
- value type, thread safe
- best to start with, if class not yet needed (SwiftUI)
- **mutating func** - change value of properties & write it back to the original structure
- properties of instance aren't mutable (can't be modified)
- alloc on Stack

### Classes
- reference type
- init/deinit, ARC
- supports inheritance
- properties of instance are mutable (can be modified)
- alloc on Heap

### Both
- conform to protocols
- encapsulation & Abstraction

### Actor
(Swift 5.5)
- reference type, thread safe
- prevents data races, since all mutations will be performed serially, one after the other
- no subclassing

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
Parametrised Types for creating reusable funcs & structs that work with any Type
    
    func swapValues(_ a: inout T, _ b: inout T) { ... }

- Often used w/collections: arrays, dictionaries, optionals, etc.
- **Type safe** - type-related errors caught at compile time

- Use **where** clause to add constraints, such as conforming to specific protocols
- Use **some** for more readable parameters in func declarations, instead of using <T: ...>

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

# UIKit

## Auto Layout
Adaptability to different screens: adjusts content dynamically for multiple devices

- Constrains - width, height, size, distance, etc.
- Priority - satisfy higher priority first then lower
- Layout Anchors (top, bottom, leading, trailing)
- Bounds (size) vs Frame (rotated/expanded)

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

## UIGestureRecognizer
- Add custom recognizer of pinches, drags, taps, etc. to regular views (other than buttons)
- add target & action - updates when gesture starts, changes, or ends
- add gesture rec. to view (one and only one)
- handlePinch(_:) and handleRotate(_:)
- UIPanGestureRecognizer, UIPinchGestureRecognizer, UIRotationGestureRecognizer

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
Modular, scalable, and maintainable software by separating into *layers*

- Presentation Layer (UI)
    - SwiftUI View + View Model = Presentation Logic
    - Views: UI elements & user interactions
    - View Model: presentation logic & communicate with use case layer

- Use Case Layer (Business Logic)
    - encapsulates business logic & rules, independent of the UI & data source implementation
    - interacts with the entities and calls the data source interfaces for data retrieval or persistence

- Entities
    - domain-specific business objects or models
    - encapsulate the core data structures and business rules of the app

- Data Source Layer (Gateway)
    - Interfaces for data access: repositories, API clients
    - Implemented by classes or services that interact w/external data sources
    - data source layer abstracts the details of data storage and retrieval

- Dependency Rule
    - Dependencies flow: from outer layers (UI) towards the inner layers (use cases, entities)
    - direction of dependencies is strictly controlled, inner layers not dependent on the outer layers

# Concurrency
- Exec mult instructions simultaneously
- 2 queues -> 1 counter, 
- achieved by Time Slicing & Context Switching 

Parallelism:
- using multiple resources to solve problems by breaking them down
- 2 queues -> 2 counters (more resources)


### Grand Central Dispatch vs Manual Thread Creation
Manually:
- more control: start, delay, cancel, etc. 
- init Thread obj 
- responsibility: dealloc, resources access, mem leaks, etc.

Grand Central Dispatch:
- queue API - start tasks in FIFO order
- decides which thread should exec task 
- chooses appropriate dispatch queue
- app can put task in queue SYNC or ASYNC

Order vs Manner of Exec
- Order: decides if task will be picked up serially or concurrently
- Manner: how the job will be exec (will it block current exec?)

## Asynchronous vs Synchronous
- Async: don't block current exec, new task will be completed later
- Sync: block current exec until new task is completed

## Serial vs Concurrent Queue
- Serial: 1 task at a time
- Concurrent: exec mult tasks simultaneously, but tasks will be dequeued serially FIFO

Differences:
- Serial/Concurrent affects destination queue TO which you are dispatching.
- Sync/Async affects the current thread FROM which you are dispatching

## Dispatch Queues
- 3 Types of Queues: 
    - Main Queues (system)
        - Serial
        - Uses main thread (only one that has access) 
        - UIKit tied to main thread, waits for user interactions
        - introduce additional BG threads to handle service calls to not freeze the UI
    - Global Concurrent Queues (system)
        - Concurrent
        - Do not use main thread
        - Priorities decided through QoS
    - Custom Queues Queues

### Quality of Service 
- tells the system how to utilize the resources in system
- User Interactive (Highest Priority - finish first) - involved in updating UI (ex. Animations)
- User Initiated - data required for immediate results (ex. scrolling table view)
- Utility - Long running task, low priority (downloading)
- Background (Lowest Priority - finish last) - not visible to user, creating backups, restoring from server, etc.

Additional Queues:
- QoS is between user initiated & utility
- No QoS info 

### Attributes of Queues
- .concurrent (queue is serial by default)
- .initiallyInactive - exec of task dispatched to queue should start later time (call .activate() when you wanna start it)

### Target Queues
- queue that your custom queue will use BTS
- priority is inherited from its target queue
- if target priority queue not specified, then it's "default priority global queue"
- How to use: u have 2 separate serial queues w/many tasks - create a target queue to make tasks serial together (serial behaviour is inherited)

### Auto Release Frequency
- requency at with which the dispatch queue will auto-release the resources being used
- .inherit - inherit from target queue (default behaviour)
- .workitem - individual auto-release pool
- .never - never setup an individual auto release pool

### Dispatch Group
- exec mult tasks together (ex.: API calls, download mult images) 
- wait for all tasks to be completed

Functions: 
- .enter() - task added to DG
- .leave() - called when exec completes
- .wait() - wait until task complete & dont proceed until done (NEVER use on main)
- .notify() -finish when num of .enter() equals num .leave()

- Ex: Splash screen 
.enter() before API call block
.leave() when task completes (.sink)
.notify() will be called - stop animation & nav to next screen

### Dispatch Work Item 
- flexibility to cancel task if exec hasnt started
- can dispatch on both DispatchQueue & DispatchGroup

## Async Await
(iOS 15)
- 'async' keyword on funcs for async operations
- 'await' inside an async func suspends exec until result of another async operation is available (more sequential & readable)
- don't need callbacks & completion handlers, no nesting & "callback hell"
- try-catch for error handling
- used within Task 

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

# Frameworks

## Combine
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

## RxSwift
 TODO
## AVFoundation

## Core Location

## Charts

# Extensions

## Action

## Share

## Notification Service

# Design Patterns

## Design Pattern Classes

- Behavioral - efficient communication patterns between obj & entities 
- Creational - create new objects easily & safely with flexibility
- Structural - streamlined ways to correlate relationships between obj & entities 

## Observer 
- obj with dependents (observers)
- one to many relationship (combine, swiftUI, notification)

## Command 
- sender-receiver (make an order) 
- queue tasks, tracking operation history, etc.

## Builder 
- add methods (burgers - tomatoes, mayo, etc) 
- similar to SwiftUI

## Factory
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

## Singleton Pattern

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

# Functional Programming
- declarative, pure functions
- given same inputs - returns the same output
- no side effects (unpredicable behavior): input/output, throwing errors, network calls, etc
- higher Order Functions: .map(), .filter(), .reduce()

Principles:
- Immutability - declared values are unchanged
- Disciplined state - no shared state & mutable data
- Referential transparency - if you replace a func call with its return value, behavior should be same 

```swift
func two() {
    return 2
}

let four = two() + two() // 4
// or
let four = two() + 2 // 4
// or
let four = 2 + two() // 4
// or
let four = 2 + 2 // 4
```

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
## Heap
    TODO
## Hash Tables 
    - maps keys to values w/functions 
    - passwords, digital signature, caching, URL shortening, SHA-256 (cryptographic func)

# Algorithms 

## Binary Search
- D&C, Sorted list then divide in half until target reached
- O(log n)
## Merge Sort 
- D&C, recursively divide in halves, sort halves independently & merge sorted halves 
- O(n log n)
## Quick Sort 
- D&C, select pivot, partition around it, recursively sort partitions & combine
- O(n^2)

## Introsort
(swift)
- hybrid alg 
- begins w/recursive Quicksort
- falls back of Heapsort if certain depth level is reached (max = 2 * log(n))
- optimize more w/Insertion Sort if n small
- comparison alg

## Depth First Search 
- search entire branch & then backtrack - traversing trees & graphs 
- O(V + E)
## Breadth First Search
- at current depth before moving to next
- O(V + E)
## Greedy Algorithms
- Dijkstra's (dykestra's) alg - shortest path to netx node
- Fractional Knapsack alg - find profit/weight ratio, sort by ratio, add until full

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
