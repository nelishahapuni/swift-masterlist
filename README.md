# Swift Masterlist

This documents contains a collection of Swift, OOP, SOLID, etc. concepts.

- [Closures](#closures)
- [Design Patterns](#design-patterns)
    1. [Factory](#1-factory-pattern)
    2. [Singleton](#2-singleton-pattern)

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

