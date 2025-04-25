# Swift Masterlist

This documents contains a collection of Swift, OOP, SOLID, etc. concepts.

- [Swift](#swift)
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
    - [Caching](#caching)
    - [Method Dispatch](#method-dispatch)
    - [Inheritance vs Interfaces](#inheritance-vs-interfaces)
    - [Animations](#animations)
    - [Typealias](#typealias)
- [UIKit](#uikit)
    - [Auto Layout](#auto-layout)
    - [Navigation](#navigation)
    - [Life Cycle Methods](#life-cycle-methods)
    - [UIGestureRecognizer](#uigesturerecognizer)
    - [Responder Chain](#responder-chain)
- [SwiftUI](#swiftui)
    - [Property Wrappers](#property-wrappers)
    - [View Preferences](#view-preferences)
- [Architectural Patterns](#architectural-patterns)
    - [MVC](#mvc)
    - [MVP](#mvp)
    - [MVVM](#mvvm)
    - [VIPER](#viper)
    - [Clean Architecture](#clean-architecture)
- [Concurrency](#concurrency)
    - [Grand Central Dispatch](#grand-central-dispatch)
    - [Asynchronous vs Synchronous](#asynchronous-vs-synchronous)
    - [Serial vs Concurrent Queue](#serial-vs-concurrent-queue)
    - [Dispatch Queues](#dispatch-queues)
        - [Quality of Service](#quality-of-service)
        - [Attributes of Queues](#attributes-of-queues)
        - [Target Queues](#target-queues)
        - [Auto Release Frequency](#auto-release-frequency)
        - [Dispatch Group](#dispatch-group)
        - [Dispatch Work Item](#dispatch-work-item)
    - [Deadlock](#deadlock)
    - [Race Conditions](#race-conditions)
    - [Async Await](#async-await)
- [Performance Optimization](#performance-optimization)
- [Store & Persist Data](#store--persist-data)
- [CI/CD Pipeline](#cdcd-pipeline)
    - [Continuous Integration](#continuous-integration)
    - [Continuous Deployment](#continuous-deployment)
- [App Signing](#app-signing)
- [Identity Pinning](#identity-pinning)
- [Frameworks](#frameworks)
    - [Combine](#combine)
    - [RxSwift](#rxswift)
        - [Observable](#observable)
        - [Subject](#subject)
        - [Ignoring Operator](#ignoring-operator)
        - [Skipping Operator](#skipping-operator)
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
    - [Facade](#facade)
    - [Mediator](#mediator)
    - [Coordinator](#coordinator)
    - [Singleton](#singleton)
- [Object Oriented Programming](#object-oriented-programming)
    - [Encapsulation](#encapsulation)
    - [Inheritance](#inheritance)
    - [Abstraction](#abstraction)
    - [Polymorphism](#polymorphism)
- [Protocol Oriented Programming](#protocol-oriented-programming)
- [Functional Programming](#functional-programming)
    - [Map](#map)
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
- [Best Practices](#best-practices)
- [Databases](#databases)
    - [Relational](#relational)
    - [Non-Relational](#non-relational)
- [Automation Testing](#automation-testing)

# Swift

Concepts related to the core swift language principles.

## Structs
- value type, mutating keyword
- can extend, not inherit
- Copy on write: copy only if contents are modified, otherwise ref
- properties of instance aren't mutable (can't be modified)
- static (direct) method dispatch
- has default (memberwise) initializer - if no private properties
- init in extension calls default init (acts like convenience)
- mock inheritance = protocol + ext + default implem of protocol methods

Alloc on Stack:
- Thread Safe: each thread has its own stack
- memory req calc at compile time
- faster alloc/dealloc: only move stack pointer (single instruction)

## Classes
- reference type
- can .deinit()
- inheritance
- properties of instance are mutable (can be modified)
- dynamic method dispatch
- required & convenience init
- use === to compare if ref/obj/address identical

Alloc on Heap:
- Thread Unsafe: global mem space needed, not exclusive to any thread
- mem size calc at run-time (mem leaks)
- lifetime can't be predicted
- slower alloc/dealloc: entire heap is recalc

Use cases:
- need to override methods
- Obj-C interoperability
- need shared mutable state (ex: App Delegate)
- control identity/address
- inheritance needed (alt. use Protocol Orient-Prog w/structs)

## Actor
(Swift 5.5)
- reference type, no inheritance
- Thread Safe: no data races, all mutations are performed serially
- best for mult-threaded envr

Ref Types: Class, Actors, Closures

## Optional
- safe way of dealing with/potential nil (Swift is SAFE lang)
- If var nullable -> Optional (enum: none, some)

- Safe: optional chaining, conditional binding, etc.
- Unsafe: forced unwrapping 

## Automatic Reference Counting
- memory management feature of the Clang compiler for Obj-C & Swift
- inserts messages 'retain' and 'release' into the object code (product of comppiler) at compile time

How it works: 
- tracks the # of strong references to each object in your code
- when an obj has no more strong references, it is deallocated
- Supports weak/unowned refs to prevent retain cycles

## Generics
Parametrised Types for creating reusable funcs & structs that work with any Type
    
    func swapValues(_ a: inout T, _ b: inout T) { ... }

- Often used w/collections: arrays, dictionaries, optionals, etc.
- **Type safe** - type-related errors caught at compile time

- Use **where** clause to add constraints, such as conforming to specific protocols
- Use **some** for more readable parameters in func declarations, instead of using <T: ...>

## Closures
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

- anonymous funcs: Closures are implemented as anon funcs - they don't have a name & defined inline
- lexical scoping: capture the vars from the surrounding lexical context - they can access and manipulate vars from the scope in which they are defined.
- lifetime: lifetime extends beyond the scope in which they are defined - allows them to be passed around and executed at a later time, while still retaining access to the variables they captured.
- Stateful Behavior: can maintain state between mult invocations bc they capture & retain the state of variables from their enclosing scope.

## Weak VS Unowned
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

## Memory Leaks
- If used memory exceeds allocated app memory - crash
- Retain cycles: closures or delegates
- Xcode Instruments > Leaks

## Codable 
- Protocol used to encode/decode data into external formats (eg. JSON)
- JSONEncoder (eg. into String)
- JSONDecoder (eg. Into JSON)
- Use CodingkKeys to customise (eg. The name of the property)

## Caching
- disk vs mem caching
- NSCache (mem cache) used in Kingfisher

## Method Dispatch
- tells app where to find method in mem before it's exec in CPU

Direct/Static Dispatch:
- faster - knows location 
- when no overridden methods (no inheritance)

Table/Dynamic Dispatch:
- slower - search first 
- witness table - pointer to func (keep track of location of funcs) 
- subclass: copy witness table & new subclass methods are added

Message Dispatch:
- slowest but most dynamic
- allows dispatch behavior to be changed at runtime
- no func offset - entire class hierarchy crawled to determine exec func

Value Type: Static
Protocol & Ref Type: Table 
Extension: Static

If class 'final' - static dispatch
If method 'dynamic' - message dispatch - moved to Obj-C runtime

## Inheritance vs Interfaces
- swift doesn't support mult inheritance (diamond problem)
- solution: interfaces (protocols) - implement instead of overriding

Protocols:
- Properties:
    - specifies name, type, gettable/settable
    - doesn't specify stored/comp property
    
## Animations
-  use **UIView.animate** API
- animation(withDuration: animations: completion)

## Typealias
-  Assign an alias to existing types
```swift
struct Receipt {
    let totalCosts: Double
}
```
which then becomes:
```swift
typealias Dollar = Double
typealias Euro = Double

struct Receipt {
    let totalCosts: Dollar
}
```
### Use with Generics
```swift
typealias Currency = Double
typealias ExchangeResult<Currency> = Result<Currency, Error>

enum ExchangeError: Error {
    case invalidInput
}
```
You can use the **Currency** typealias, to pass either **Euro** or **Dollar**:
```swift
extension Dollar {
    func toEuro() -> ExchangeResult<Euro> {
        guard self > 0 else {
            return ExchangeResult.failure(ExchangeError.invalidInput)
        }
        return Result.success(self * 0.896)
    }
}

let receipt = Receipt(totalCosts: 10)
receipt.totalCosts.toEuro() // .success(8.96)

let doubleNumber: Double = 10
doubleNumber.toEuro() // .success(8.96)
```
### Reusing callbacks
typealias Completion = () -> Void

### Conforming to Multiple Ptocols
typealias multProtocols = FirstProtocol & SecondProtocol
```swift
typealias TransitionDelegate = UIViewController & UIViewControllerTransitioningDelegate
```

### Reusable Dictionary
```swift
public typealias TrackingProperties = [String: Any]
```

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

## Life Cycle Methods 
(UIViewController)
- init - method for instance
- loadView() creates and instantiates the UIView obj
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
- add target & action - updates gesture start, change, or end
- add gesture rec. to view (one and only one)
- handlePinch(_:) and handleRotate(_:)
- UIPanGestureRecognizer, UIPinchGestureRecognizer, UIRotationGestureRecognizer

## Responder Chain

# SwiftUI 

## Property Wrappers
- @State, @Binding, and @ObservedObject to manage the state of views

- @Published wrapper (SwiftUI) 
- declarative programming
- observable properties 
- automatic UI updates

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

## View Preferences

# Architectural Patterns

## MVC
- Model: data & business logic
- View: UI & sends interactions to controller
- Controller: intermediary b/ween view & model

## MVP
- Model: data & business logic
- View: more isolated, interacts primarily w/presenter
- Presenter: 
    - receives input from view, interacts w/ model to perform business logic, and updates the view with the results.
    - interacts w/model independently of view

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
- View: present UI and gather user input - send user actions to Presenter
- Interactor: business logic - fetches data from the model (Entity) & processes it (ex: network requests) - then sends the data to the Presenter for display
- Presenter: middleman - takes input from Interactor, prepares data for display, then passes it to the View. Decides what happens when a user interacts with the View.
- Entity: data structures (structs) - business objects.
- Router: navigation logic for the application - next screen based on user actions & the app's state

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

### Grand Central Dispatch
- queue API - start tasks in FIFO order
- decides which thread should exec task 
- chooses appropriate dispatch queue
- app can put task in queue SYNC or ASYNC

Manual Creation:
- more control: start, delay, cancel, etc. 
- init Thread obj 
- responsibility: dealloc, resources access, mem leaks, etc.

Order vs Manner of Exec
- Order: decides if task will be picked up serially or concurrently
- Manner: how the job will be exec (will it block current exec?)

## Asynchronous vs Synchronous
- Async: don't block current exec, new task will be completed later
- Sync: block current exec until new task is completed
- Sync/Async affects current thread FROM which you are dispatching

## Serial vs Concurrent Queue
- Serial: 1 task at a time
- Concurrent: exec mult tasks simultaneously, but tasks will be dequeued serially FIFO
- Serial/Concurrent affects destination queue TO which you are dispatching.

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
- tells the system how to utilize the resources
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

## Deadlock
- two or more threads are blocked, waiting for each other to release resources - system freeze or crash

```swift
let queue1 = DispatchQueue(label: "com.example.queue1")
let queue2 = DispatchQueue(label: "com.example.queue2")

queue1.async {
    print("Task 1a")
    queue2.sync {
        print("Task 1b")
    }
    print("Task 1c")
}

queue2.async {
    print("Task 2a")
    queue1.sync {
        print("Task 2b")
    }
    print("Task 2c")
}
```

## Race Conditions
- two or more threads access and modify shared data simultaneously, resulting in unpredictable behavior
- to prevent use locks, semaphores, or barriers

Problem: since we might access the same currentCount var from mult threads, some values may not be written

```swift
var count = 0

func incrementCount() {
    let currentCount = count
    count = currentCount + 1
}

DispatchQueue.concurrentPerform(iterations: 1000) { _ in
    incrementCount()
}

print("Final count: \(count)")
```

Fix: force block to be exec synchronously:

```swift
func incrementCount() {
    queue.sync {
        let currentCount = count
        count = currentCount + 1
    }
}
```

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

# App Signing
- digital ID of app
- certificate issued by APPLE
- only executable code is signed

# Identity Pinning

# Frameworks

## Combine
- Publisher Subscriber Model
- Reactive framework 
- Asynchronous & event-driven code

- Sink - connects P&S & receives the value in the closure
- Cancellables - breaks connection between P&S
- Catch - handle errors from publisher
- Receive (on main thread)

### Transforming Operators
- collect() - convert a stream of values into an array - collect(n) - collect into arrays with n number of items
- map() - operate on values emmitted from a publisher
- tryMap(_:) - takes a closure that can throw an error, which is then emitted downstream
```swift
...
Just("Directory name that does not exist")
    .tryMap { try FileManager.default.contentsOfDirectory(atPath: $0) }
    .sink(receiveCompletion: { print($0) },
          receiveValue: { print($0) })
...
```
#### flatMap(maxPublishers:_:)
- flattens multiple emissions from upstream publishers into a single downstream publisher
- the publisher returned by flatMap often isn't of the same type as the upstream publishers it receives
- You can specify a **n** number of max publishers ( .flatMap(maxPublishers: .max(n)) { $0.message }) (default is unlimited)

#### Replacing Upstream Output
- replaceNil(with:) - set a default value to replace nil values with - passes an optional downstream
- replaceEmpty(with:) - replaces (inserts) a value if a publisher completes without emitting a value

#### Incrementally transforming output
- scan(_:_:) - provides the current value emitted by an upstream publisher to a closure, along with the last value returned by that closure.
- tryScan() - same but throws an error

```swift
...
august2019
    .scan(50) { latest, current in
      max(0, latest + current)
    }
...
```
### Filtering Operators
NOTE: Most operators have a throwing equivalent (e.g. _filter_ vs _tryFilter_)

- filter() - filter incoming value by a condition - e.g. filter{ $0.isMultiple(of: 3) }
- removeDuplicates() - removing incoming values with are dublicates - works on any Type that conforms to Equatable
- compactMap() - operate on optional values, discard nils (e.g. convert Strings to Ints, discard nil results)
- ignoreOutput() - doesn’t matter which or how many values are emitted - they’re all ignored - you only push the completion event through to the consumer
- first(where:) - LAZY - only takes values until it finds one matching the condition you provided - when match is found - cancels subscription and completes
- last(where:) - GREEDY - wait for all values to be emitted before finding a match - upstream must be a publisher which completes

#### Dropping Values
- dropFirst(n) - ignores the first _n_ values emitted by the publisher - only values emitted after _n_ values have been emitted will be allowed (default is 1)
- drop(while:) - ignores any values emitted by the publisher until the _first time_ that condition is met
- drop(untilOutputFrom: _isReady_) - skips any values emitted by a publisher until a 2nd publisher starts emitting values, creating a relationship between them - useful when you have a user tapping a button, but you want to ignore all taps until your _isReady_ publisher emits some result

#### Limiting values (Prefix family)
- prefix(_:), prefix(while:), prefix(untilOutputFrom:) - the prefix operators _take_ values until that condition is met, then completes

### Combining Operators
#### Prepending
- prepend(Output...) - it can take any # of values, as long as they’re of the same _Output_ type as the original publisher
- prepend(Sequence) - it takes any _Sequence_ object as an input (e.g. Array or Set) - use .prepend(stride(from: 6, to: 11, by: 2)) to _stride_ through values 6-11 in steps of 2
- prepend(Publisher) - 
```swift
...
let publisher = [3, 4].publisher
publisher
    .prepend(1, 2)
    .prepend(-1, 0)
    .sink(receiveValue: { print($0) })
...
// prints
-1
0
1
2
3
4
```
#### Appending
-  works similarly to prepend: It also takes a variadic list of type _Output_ but then appends its items after the original publisher has completed with a .finished event

## RxSwift
- library for async & event-based code
- using observable sequences & functional operators
- parametrized exec w/schedulers

### Observable
- when there's a subscriber, it sends events via emitting
- emits a 'next' event w/element & continues until success/error
- call dispose() to stop emitting - auto prevents mem leak

### Subject
- can be Observable or Observer
- PublishSubject - start empty, only emit new element to subscribers
- BehaviorSubject - start w/init value, replay it or latest element to new subscribers
- ReplaySubject - iniy w/buffer size, will maintain a buffer of elements up to that size & reply it to next subscribers

### Ignoring Operator
- ignores all .next event elements
- use if only need .completed/.error
- .ignoreElements() on subj - subject.ignoreElements().subscribe { ... }.disposed(by: disposeBag)
- .elementAt(n) - returns elem at n

### Skipping Operator
- .skip(n) - ignore elem at index n
- .skipWhile(condition) - skip if true
- .skipUntil(trigger) - skip until trigger is activated

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

## Facade
- facade is a higher-level interface that simplifies interactions b/w subsystems
- subsystems have zero knowledge of the facade
- often used w/API calls/fetching
- minimizes dependencies between subsystems
- let's suppose we have 2 subsystems AccountClient & MovieClient (which depends on AccountClient) - premium & regular accounts
- Create MovieListFetcher to init both subsystems & use func getMovies to call the funcs .getAccountType * then .getUpcomingMovies
```swift
protocol MovieClient {
    func getUpcomingMovies(accountType: AccountType,
                           completion: @escaping (Result<[Movie], Error>) -> Void)
}

protocol AccountClient {
    func getAccountType(completion: @escaping (Result<AccountType, Error>) -> Void)
}
```

```swift
class MovieListFetcher {
    let movieClient: MovieClient
    let accountClient: AccountClient

    init(movieClient: MovieClient, accountClient: AccountClient) {
        self.movieClient = movieClient
        self.accountClient = accountClient
    }

    func getMovies(completion: @escaping (Result<[Movie], Error>) -> Void) {
        accountClient.getAccountType { [weak self] result in
            guard let self = self else { return }
            switch result {
            case .success(let accountType):
                self.movieClient.getUpcomingMovies(accountType: accountType, completion: completion)
            case .failure(let error):
                completion(.failure(error))
            }
        }
    }
}
```

## Mediator
- used in chat context between mult users
- Protocol ChatUser: var name, func send() & receive()
- class User: implements ChatUser
- ChatMediator class has list of Users & func register(user)
- init chatMediator and 3 users(Ani, Bob, Chad)
- user1.send("helllo")
- user2.send("hello there")

Source: https://ruslandzhafarov.medium.com/design-patterns-in-swift-mediator-2e1be4c7ffc3

## Coordinator
- Coordinator Protocol - methods for starting, presenting, and finishing the flow
- Coordinators create and present VCs & communicate between each other, pass data, trigger navigation, etc.
- Benefits from Dependency Injection

## Singleton

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

# Protocol Oriented Programming
- composition & protocol comformance over inheritance
- flexible, allows to mix-and-match protocols, reusable
- protocol extensions allow for default implementations & adding functionality

# Functional Programming
- declarative, pure functions
- given same inputs - returns the same output
- no side effects (unpredicable behavior): input/output, throwing errors, network calls, etc
- higher Order Functions: .map(), .filter(), .reduce()

## Map
Map - transform from one thing to another
```swift
let numbers = [1, 2, 3, 4, 5]
let doubled = numbers.map { $0 * 2 }
```
- Will take each value in an array & run it through a closure, where $0 refers to the element in question.
```swift
let imageViews = view.subviews.compactMap { $0 as? UIImageView }
let urls = urlStrings.compactMap { URL(string: $0) }
```
- compactMap() does the same thing, but if your transformation returns an optional it will be unwrapped and have any nil values discarded.
```swift
let number: String? = getUser(id: 97)
let result = number.flatMap { Int($0) }
```
- flatMap() also performs a transformation, then flattens what comes back so that “optional optional” just becomes “optional”.

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

# Data Structures

## Arrays 
    - list of ordered items
    - access: constant-time access w/index O(1)
## Sets
    - unordered list of unique elements
## Tuples
    - compound value w/diffent type of values 
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
![]() <img src="https://i.imgur.com/YJaMK71.jpeg)" width=30%>

    - hierarchical structure of nodes, each with/value & set of child nodes 
    - Binary Trees
## Graphs 
    - Nodes connected by edges 
## Heap
    - Complete binary tree
    - Max-Heap: root node value is greatest among children
    - Min-Heap: root node value is least among children
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

## Topological Sort
![]() <img src="https://i.imgur.com/b9g6btz.jpeg" width=50%>

## Dijkstra's algorithm
![]() <img src="https://i.imgur.com/9dq4uux.jpeg" width=50%>

![]() <img src="https://i.imgur.com/ee3twZ6.jpeg" width=50%>

![]() <img src="https://i.imgur.com/tZayBSf.jpeg" width=50%>

## Bellman-Ford

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
- Dijkstra's (dykestra's) alg - shortest path to next node
- Fractional Knapsack alg - find profit/weight ratio, sort by ratio, add until full

# Best Practices
- KISS (keep it stupid simple), DRY (dont repeat yourself)
- Prefer Structs over Classes
- prefer immutable values (let)
- use Enums for States & Error Handling
- use weak, unowned to prevent mem leaks
- use protocols for abstract interfaces
- documentation & comments

# Databases

## Relational
- structured tables (rows/columns)
- each table is an entity - relatoinship b/w entities w/foreign keys
- ACID transactions (Atomicity, Consistency, Isolation, Durability) - data integrity & reliability
- use SQL (Structured Query Language)
- used for transactional systems, financial & enterprise applications

## Non-Relational
- flexible data models (key: value, document, graph, etc.)
- easy horizontal scalability - distributing data across nodes
- used for real-time analytics, social networks, content management, distributed systems

# Automation Testing
- Automation testing w/APPIUM (java) using XCUItest iOS driver
- inspect element class, attributes, etc.
