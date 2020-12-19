--- 
title: "Advent of Code 2020: Swift"
date: 2020-12-28 08:00:55 -0800
layout: post
tags: swift aoc
---

[Advent of Code](https://adventofcode.com) was a fun diversion this year. It seemed a bit easier than some previous years. Perhaps it helped that I picked a language that I had some experience with, [Swift](https://developer.apple.com/swift/). Here are some quick thoughts on Swift as an AoC language, some general AoC tips, and notes on some of the more interesting puzzles.

You can find all my 2020 code [here](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources).

## Swift for AoC

First, the complaints:

**Swift is too pedantic about strings.** AoC usually involves _loads_ of string manipulation and parsing, so being able to easily index into strings is super helpful. Swift does not make this easy out-of-the-box. I understand why. [Strings are complicated](https://oleb.net/blog/2017/11/swift-4-strings/) in a language like Swift that supports Unicode well. Indexing into an array is expected to be a constant-time operation, but that's not possible when you have to look at the preceding bytes in order to figure out what the _i_th character is. Still. You can at least make it easier and less ugly than `myString[myString.index(myString.startIndex, offsetBy: i)]`. Thankfully Swift is easy to monkey-patch, so I added some [useful extensions](https://github.com/bgreenlee/AdventOfCode/blob/main/2020/Sources/Shared/StringExtensions.swift) to make it tolerable.

**Regexes are similarly annoying** in Swift. Here it just feels like they never properly ported them over from Objective-C. Here's an example:

```swift
// print the animals found in a string
let myString = "The quick brown fox jumped over the lazy dogs."
let animalsRegex = try NSRegularExpression(pattern: #"(fox|dog)"#, options: [])
let matches = animalsRegex.matches(in: myString, options: [], range: NSRange(myString.startIndex..<myString.endIndex, in: myString))
for match in matches {
    for i in 1..<match.numberOfRanges {
        print(NSString(string: myString).substring(with: match.range(at: i)))
    }
}
```

My colleague [Mike Simons](https://github.com/waltflanagan) gave me a [RegEx](https://github.com/bgreenlee/AdventOfCode/blob/main/2020/Sources/Shared/RegEx.swift) helper he uses, which reduces the above to:

```swift
let animalsRegex = try RegEx(pattern: #"(fox|dog)"#)
let matches = animalsRegex.matchGroups(in: myString)
for match in matches {
    for group in match {
        print(group)
    }
}
```

**Tuples not being hashable** or even extendable is a pain for a lot of AoC problems. I resorted to using a custom struct for a bunch of problems, but at some point discovered [SIMD](https://developer.apple.com/documentation/swift/simd) vectors, which have the added benefit of having arithmetic operators defined for them.

**What I loved** about Swift:

Working with a **statically-typed language** in a proper IDE (**Xcode**) is such a huge time-saver. Everything is checked as you type (which can be a little annoying when Xcode starts scolding you for not using variables that you've defined before you get a chance to write the code that uses them; but you learn to ignore it). You have a full-featured debugger.

[Playgrounds](https://www.apple.com/swift/playgrounds/) are really useful for trying out quick snippets of code.

Swift is generally a **pleasant, straight-forward language** to write. There's not all the overhead of thinking about memory and ownership like there is in Rust, for example.

It's actually pretty **performant**—at least when not running the debug build (I usually got a 10x speed-up when building for release).

## General AoC Tips

* **Hash maps**, hash maps, hash maps. Even if the problem seems array-shaped, there's a good chance a hash map will be more efficient.
* Many of the problems are grounded in some **basic CS principle** (day 5 this year being a classic example). Think about what that might be before diving in.
* Oftentimes part 2 will be part 1 but **MORE**. Days [15](https://adventofcode.com/2020/day/15) (Rambunctious Recitation) and [23](https://adventofcode.com/2020/day/23) (Crab Cups) this year were good examples of that. When coding part 1, think about how your solution might scale if you were doing a million times more, or with a million times more data.

## The Puzzles

Ok, on to specific puzzles. I'm just going to talk about some of the more interesting ones. Spoilers follow:

### [Day 5: Binary Boarding](https://adventofcode.com/2020/day/5) ([code](https://github.com/bgreenlee/AdventOfCode/blob/main/2020/Sources/05-BinaryBoarding/main.swift))

This is a classic AoC problem in that it disguises basic CS principles. If you were to solve this naively, it might be kind of annoying. but when you realize that the seating strings (e.g. `FBFBBFFRLR`) are just binary numbers, with (F,L) -\> 0 and (B,R) -\> 1, it becomes trivial. Particularly helpful here was Swift's ability to convert a binary string to decimal with `Int(binary, radix: 2)`.

Part 1 just required sorting the list of converted seat numbers and taking the last (highest) one. Part 2, finding the first non-consecutive id, is probably faster just iterating through the sorted list and stopping on the first id that is not one more than the last id (which is what I did originally), but it was more fun and concise to use [Gauss' formula](https://nrich.maths.org/2478) to calculate what the total should be if all the consecutive numbers were present and subtract the actual sum of all numbers.

For kicks, I did a "no code" version in [a spreadsheet](https://docs.google.com/spreadsheets/d/1pgE15V-n5zG6IPb921SJAGYnNPQ7u4Ql_QYS_wNM2V4/edit#gid=0).

### [Day 7: Handy Haversacks](https://adventofcode.com/2020/day/7) ([code](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources/07-HandyHaversacks))

This was the first day that required some real thought. I started down the path of having a bidirectional graph with each bag having parents and children, but I got bogged down. I think sometimes creating objects complicates things vs. just slinging around string hash maps.

### [Day 9: Encoding Error](https://adventofcode.com/2020/day/9) ([code](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources/09-EncodingError))

This is totally an interview question I would have failed. It's easy enough doing it the "dumb" (O(n^2)) way that I did it, constructing a lookup table of all sums in the preamble, or, for part 2, [iterating through every possible sequence](https://github.com/bgreenlee/AdventOfCode/blob/46162105ed4f803151b9db18c87215e15511118c/2020/Sources/09-EncodingError/Part2.swift) of two consecutive numbers, then three, etc.—also O(n^2). After I finished, a friend shared the ["inchworm" technique](https://github.com/bgreenlee/AdventOfCode/blob/main/2020/Sources/09-EncodingError/Part2.swift), which is O(n). Neat! I totally failed that interview.

### [Day 10: Adapter Array](https://adventofcode.com/2020/day/10) ([code](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources/10-AdapterArray))

Part 2 was fun to figure out. There were very different ways to approach this. I went with recognizing that we were dealing with [power sets](https://en.wikipedia.org/wiki/Power_set). I [wrote up my thinking in my code](https://github.com/bgreenlee/AdventOfCode/blob/main/2020/Sources/10-AdapterArray/Part2.swift), so I won't duplicate it here. Some friends solved this using [dynamic programming](https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/m2G1pAq0OO0), which I definitely need to level up on.

### [Day 11: Seating System](https://adventofcode.com/2020/day/11) ([code](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources/11-SeatingSystem))

Nothing particularly interesting here—the first of three [Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)-type problems. I just had to share [this](https://www.reddit.com/r/adventofcode/comments/kcpdbi/2020_day_11_part_2luaroblox_waiting_room/), though.

### [Day 12: Rain Risk](https://adventofcode.com/2020/day/12) ([code](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources/12-RainRisk))

Pretty straightforward. Part 2 was fun because I [re-]learned how to [rotate a vector](https://matthew-brett.github.io/teaching/rotation_2d.html).

### [Day 13: Shuttle Search](https://adventofcode.com/2020/day/13) ([code](https://github.com/bgreenlee/AdventOfCode/blob/main/2020/Sources/13-ShuttleSearch/main.swift))

This was a fun one. I solved part 1 in bed on my phone with a calculator, and then spent the entire next day working on part 2. I learned about the [Extended Euclidean algorithm](https://en.wikipedia.org/wiki/Extended_Euclidean_algorithm) and the [Chinese remainder theorem](https://en.wikipedia.org/wiki/Chinese_remainder_theorem). I used the [existence construction](https://en.wikipedia.org/wiki/Chinese_remainder_theorem#Using_the_existence_construction) to solve it, but the simpler solution that many used was [sieving](https://en.wikipedia.org/wiki/Chinese_remainder_theorem#Search_by_sieving).

### [Day 17: Conway Cubes](https://adventofcode.com/2020/day/17) ([code](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources/17-ConwayCubes))

Game of Life again! Doing it in 3D and then 4D was a fun twist. Nothing particularly tricky. I just have a [soft spot for GoL](https://apps.apple.com/us/app/qr-life/id1061418370).

### [Day 18: Operation Order](https://adventofcode.com/2020/day/18) ([code](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources/18-OperationOrder))

This was a fun one. I spent a lot of time scratching my head and drawing diagrams, until I remembered it's a lot easier to parse expressions in [postfix](https://en.wikipedia.org/wiki/Reverse_Polish_notation), and you can use the [Shunting yard algorithm](https://en.wikipedia.org/wiki/Shunting-yard_algorithm) to convert infix to postfix.

### [Day 19: Monster Messages](https://adventofcode.com/2020/day/19) ([code](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources/19-MonsterMessages))

Woo, boy! This was a good one. I'd never written a parser before, so this was a learning experience. My [part 1 solution](https://github.com/bgreenlee/AdventOfCode/tree/ac4f6e50be0ba2dcca8e69bfbb77e1f83a034b92/2020/Sources/19-MonsterMessages) involved creating a matcher from the grammar, and it was fast and elegant. Part 2 threw in the wrench of recursive rules, which absolutely do not work when building a matcher. I tried a whole bunch of things like adding recursion limits, but I could not get it to work. I ended up moving on and came back to it only after I'd finished all the other days. By then I had heard the term [CYK algorithm](https://en.wikipedia.org/wiki/CYK_algorithm) bandied about, and so I did a complete rewrite using that. I spent a bunch of time reading about the algorithm and about [Chomsky normal form](https://en.wikipedia.org/wiki/Chomsky_normal_form). I "cheated" a bit and didn't write code to convert the provided rules to Chomsky normal form, as there were only a few rules that needed to be tweaked and it was easy enough to do it by hand:

| Original               | Chomsky normal form |
| ---------------------- | ------------------- |
| `107: 18 | 47`         | `107: "b" | "a"` |
| `11: 42 31 | 42 11 31` | `11: 42 31 | 42 133` <br> `133: 11 31` |
| `8: 42 | 42 8`         | `8: 47 50 | 18 4 | 42 8` |

### [Day 20: Jurassic Jigsaw](https://adventofcode.com/2020/day/20) ([code](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources/20-JurassicJigsaw))

This was a favorite. Part 1 was easy once I figured out a way to generate a hash code that uniquely identified a side, irrespective of its orientation:

	static func sideToInt(_ side:String) -> Int {
	    let binary = side.replacingOccurrences(of: ".", with: "0")
	                     .replacingOccurrences(of: "#", with: "1")
	    let num = Int(binary, radix: 2)!
	    let numReversed = Int(String(binary.reversed()), radix: 2)!
	    return num * numReversed * (num ^ numReversed)
	}

Once you have that, your corners are the tiles that have two "singleton" sides (i.e. unique to that tile). No assembling necessary...for part 1. Part 2 did require assembling, and that was a bit tedious. It was a cool problem, though.

### [Day 23: Crab Cups](https://adventofcode.com/2020/day/23) ([code](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources/23-CrabCups))

Classic AoC problem where the simple solution for part 1 utterly fails in part 2. I actually got the answer to part 2 after letting my dumb-but-optimized part 1 solution run overnight, but while it was running, mulling it over in bed, I realized that this was a linked list problem. I fixed it in the morning and the runtime went from ~ 7 hours to 0.2 sec.

### [Day 24: Lobby Layout](https://adventofcode.com/2020/day/24) ([code](https://github.com/bgreenlee/AdventOfCode/tree/main/2020/Sources/24-LobbyLayout))

This was surprisingly easy for this late in the calendar, although my first naive attempt trying to map it to a 2D coordinate system failed. But I googled "hexagonal grid coordinate system" and found [this cool page](https://www.redblobgames.com/grids/hexagons/). I used the cube coordinate system and it was easy-peasy.

Part 2: Yay, more Game of Life! I'm sorely tempted to write a visualization for this.
