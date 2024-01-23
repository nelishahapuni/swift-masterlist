# Swift Masterlist

This documents contains a collection of Swift, OOP, SOLID, etc. concepts.

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

# Algorithms // TODO: - Add code segments & more details

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