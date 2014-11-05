--- 
title: "Fun with Swift Extensions"
date: 2014-11-04 22:04:55 -0800
layout: post
---

As promised, a post on extensions in Swift.

This isn't going to be an in-depth tutorial. Extensions have been covered better elsewhere, most notably in [Apple's excellent documentation](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Extensions.html). I just want to show a couple extensions I've written: `Array#find`, and comparison operators for `NSDate`.

### Array#find

If you want to find a value in a Swift Array, you can use the global generic `find` function:

```swift
let arr = [4,8,15,16,23,42]
let idx = find(arr, 15)  // Some(2)
```

However, `find` only lets you search for a static value, and what I really wanted was the ability to find the first value that satisfied a test function. For example, say I want to find the first odd number in an array. Let's extend Array so we can do that.

```swift
extension Array {
    func find(test: (T) -> Bool) -> Int? {
        for i in 0..<self.count {
            if test(self[i]) {
                return i
            }
        }
        return nil
    }
}

let arr = [4,8,15,16,23,42]
let idx = arr.find { $0 % 2 == 1 }  // Some(2)
```

The extension defines a method, `find`, on Array, that takes as an argument a function mapping an element of type T to a Bool, and returns an optional Int. In other words, we pass in a test function, and it returns the index of the first value in the array for which that function is true. If no such values are found, it returns nil.

But wait! you say. Why write an extension when you can just add a new signature to the global `find` function so that it will take a test function rather than a value? FINE. Be that way.

You can, um, find the method signature of `find` in [Apple's documentation](https://developer.apple.com/library/ios/documentation/General/Reference/SwiftStandardLibraryReference/Algorithms.html#//apple_ref/doc/uid/TP40014608-CH15-DontLinkElementID_14):

```swift
func find<C: CollectionType where C.Generator.Element: Equatable>(domain: C, value: C.Generator.Element) -> C.Index?
```

That looks a bit complicated, and CollectionTypes and Generators are a topic for another day, but all you really need to know is that in order to conform to the CollectionType protocol, you need to define a `startIndex` and an `endIndex`, and be subscriptable. C.Generator.Element is the type of items in the collection. So instead of passing in a value of type C.Generator.Element to find, we want to pass in a test function that takes a C.Generator.Element and returns a Bool. The rest looks very much like our Array extension:

```swift
func find<C: CollectionType where C.Generator.Element: Equatable>(domain: C, test: (C.Generator.Element) -> Bool) -> C.Index? {
    for i in domain.startIndex..<domain.endIndex {
        if test(domain[i]) {
            return i
        }
    }
    return nil
}

let arr = [4,8,15,16,23,42]
let idx = find(arr, { $0 % 2 == 1 })  // Some(2)
```

### Comparison Operators for NSDate

NSDate annoyingly does not implement the Comparable protocol, meaning that instead of:

```swift
if someDate <= anotherDate { ...
```

You have to do:

```swift
if someDate.compare(anotherDate) == NSComparisonResult.OrderedAscending ||
        someDate.isEqualToDate(anotherDate) { ...
```

Let's fix that. According to [Apple's docs](https://developer.apple.com/library/ios/documentation/General/Reference/SwiftStandardLibraryReference/Comparable.html#//apple_ref/doc/uid/TP40014608-CH16-SW1), we only need to implement `<` and `==` to conform to the `Comparable` protocol. Pretty clever, since with just those two, you can derive `<=`, `>`, `>=`, and `!=`.

```swift
extension NSDate : Comparable {}

public func < (lhs: NSDate, rhs: NSDate) -> Bool {
    return lhs.compare(rhs) == NSComparisonResult.OrderedAscending &&
        !lhs.isEqualToDate(rhs)
}

public func == (lhs: NSDate, rhs: NSDate) -> Bool {
    return lhs.isEqualToDate(rhs)
}
```

The first line just says that we are extending `NSDate` to conform to the `Comparable` protocol. You can leave this out, but then you'll have to implement all the other comparison operators.

The rest is pretty straight-forward. The only curious bit is why the operators are implemented in the global scope, rather than within the extension. I.e. you'd think you could do this:

```swift
// This does not work!
extension NSDdate : Comparable {
  func < (other:NSDate) -> Bool {
    return self.compare(other) == ...
```

So why not? Operators aren't methods in Swift, but global functions. They have precedence over methods, and have varying rules around associativity. You can even make up your own operators if you want. Be sure to read NSHipster's [great article on Swift operators](http://nshipster.com/swift-operators/).
