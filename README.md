# Swift + VScode

- Installed the two most pop Swift extensions, the preview release install made the editor install a CoreLLDB
- `swift repl` gets you the REPL and `swiftc` is the compiler, use the latter like `swiftc hello.swift`
- code completion works on REPL!!
```swift
  3> numbers.sort
Available completions:
	numbers.sort -- sort() -> Void
	numbers.sort -- sort(by: (Int, Int) throws -> Bool(Int, Int) throws -> Bool) -> Void
	numbers.sort -- sorted() -> [Int]
	numbers.sort -- sorted(by: (Int, Int) throws -> Bool(Int, Int) throws -> Bool) -> [Int]
```
- string formatting is kinda uniquely cool in Swift

## Variables

- **let** declaration makes it a constant & **var** declaration makes it a variable
- type inference works while initialization, not required to specify type explicitly, types are inferred by compiler(kinda like Rust)

```swift
let implicitDouble = 70.0
let explicitDouble: Double = 70
let explicitFloat: Float = 4.0
```

- implicit type conversation doesn't work, kinda like Rust
```swift
let apples = 5;
let oranges = 1;
let iPhones = 1;
let textA = "I have \(apples + iPhones) apples and \(oranges) oranges."
// but this kinda works ^^^
// use """ to add strings over multiple lines
```
## Arrays

- arrays of elements and maps are initialised and used kind of like lists in Python

```swift
apples = ["iPhone 5", "iPhone 5C", "iPhone 12 mini"]
apples.append("iPhone 14 Plus")
print(apples)
fruits = [] // fruits is an empty list
```

### Dictionaries

```swift
var occupations = [
    "Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
occupations = [:] // occupations is an empty map
let emptyDictionary: [String: Float] = [:]
// iterate over dictionary
var largest = 0
for (_, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
print(largest)
```

## Control Flow


### Conditionals

- if and switch

```swift
let scores = [75, 43, 100, 98, 100]
var accumulated = 0
for score in scores {
	if score > 50 {
	// statement in an if should evaluate to boolean expression
	// errors are not implicitly zero
		score += 1
	} else {
		score -= 1
	}
}
print(accumulated)
```

### Other Constructs

- `if let` expression together work with values that can be missing, i.e. Optional variables
TODO: In an if statement, the conditional must be a Boolean expression—this means that code such as if score { ... } is an error, not an implicit comparison to zero.

```swift
var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
if let name = optionalName {
    // like if-let of Rust!
    greeting = "Hello, \(name)"
}
```
- There are some fancy way of specifying default values using `??` expression such as
```swift
let informalGreeting = "Hi \(nickname ?? fullName)"
```

### Switch-Case

- not limited to integers, kinda like Golang
- TODO: Check how they are w.r.t Rust
- no fall through

```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
    // TODO: Try removing the default case, does it behave like Rust??
}
// Prints "Is it a spicy red pepper?"
```
### Tuple

- tuples can make a compound value; and help you return more args
- one can access the tuples by their names
```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
	// logic ...
	return (0, 1, 2)
}
let s = calculateStatistics(scores: [1,2,3,4])
print(s.sum)
```

### Loops

- loops       : for-in, while, repeat-while
- no brackets required like Golang for conditionals and loops
- while loop is just like C
- there's a repeat-while, just like do-while of C, as it'll run at least once
```swift
while i < 10 {
	// code
}
repeat {
	i += 10
} while i <= 100
print(i)
```
- numeric range based look like Rust
```swift
var total = 0
for i in 0..<4 {
    total += i
}
print(total) // 6
```
- use `...` for an equivalent loop of `..=` in Rust, i.e. inclusive range, E.g. 1...4 gives 1, 2, 3, 4

### Functions

- funcs are first-class-types, like Python, Golang, i.e. they can be passed around into other funcs, stored in variables, lists, returned from other funcs,
- func init is like Rust;
```swift
func hello(name: String) -> String {
	return "Hello \(name)"
}
```
hello(name: "Harsh")
- syntactic sugar: you can use fn-parameters or add a label to the parameters
```swift
func greet(_ person: String, on day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet("John", on: "Wednesday")
```
- are kind of special types of closures, blocks of code which can be called later
    - isn't this true for all languages, all funcs/methods accept params which are outside the scope of the called func
    - TODO: code in a closure has access to things like variables and functions that were available in the scope where the closure was created
    - coz the function(a special closure) has access to things which were created outside the scope of itself???
- anonymous function closures: think Java Lambdas!! and sorta-Rust syntax

```swift
numbers.map({(n: Int) -> Int in
    // Use in to separate the arguments and return type from the body.
    let result = 3 * n
    return result
})
// a more clean way to write the same
// reason: the types are known

// exact-text: When a closure’s type is already known, such as the callback 
// for a delegate, you can omit the type of its parameters, its return type, or both
let mappedNumbers = numbers.map({ number in 3 * number })

// Exercise: return all zeros for odd numbers
numbers.map({(n: Int) -> Int in 
    if n % 2 == 1 {
        return 0
    }
    return 1
})

#### Closures

```swift
// hasAnyMatches accepts an Array and a func with signature accepting an int
// and returning a boolean and the hasAnyMatches function returns a Bool
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```

- sorting example closure
TODO: DOUBT! Try to jump inside this definition
```swift
You can refer to parameters by number instead of by name—this approach is especially useful in very short closures. A closure passed as the last argument to a function can appear immediately after the parentheses. When a closure is the only argument to a function, you can omit the parentheses entirely.
```

## Classes and Objects

```swift
class Shape {
    // declare variables and functions like you would normally
    let name = "Shapes are geometry"
    var numberOfSides = 0
    // init is an initializer, think of it like a constructor
    // the args to this are passed just like how it's done in most languages; think constructor!
    init(name: String) {
        // like the this keyword of java & the customary self of Python
        // helps distinguish the name property from the name param
        self.name = name
    }
    deinit() {
        // can perform some object cleanup
        // E.g. cleanup of some external resources like DB connection(s), open FDs, open sockets 
    }
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
    func randomNumber(n: Int) -> Int {
        return n + numberOfSides
    }
}
 1> s.randomNumber(n: 4)
$R0: Int = 4
 2> s.numberOfSides = 5
 3> s.randomNumber(n: 4)
$R1: Int = 9
 4> var shapeDescription = s.simpleDescription()
shapeDescription: String = "A shape with 5 sides."
```


### Sub-class

```swift
class Square: Shape {
    var sideLength: Double

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        // TODO: Why can it access numberOfSides directly, just coz I called super?
        // Shouldn't it be super.numberOfSides to explicitly indicate the base-class's value?
        numberOfSides = 4
    }

    func area() -> Double {
        return sideLength * sideLength
    }

    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}
 40. let test = Square(sideLength: 5.2, name: "my test square")
 41. test.area()
 42. test.simpleDescription()
$R0: String = "A square with sides of length 5.2."
test: Square = {
  __lldb_expr_3.Shape = {
    name = "my test square"
    numberOfSides = 4
  }
  sideLength = 5.2000000000000002
}
```
// exercise: a circle Shape
```swift
class Circle: Shape {
    var radius = 0.0
    let PI = 3.141
    init(radius: Double, name: String) {
        super.init(name: name)
        self.radius = radius
    }
    func area(r: Double) -> Double {
        return PI * r * r
    }
    override func simpleDescription() -> String {
        return "A circle with radius \(radius)"
    }
    // besides stored properties(aka data members);
    // properties can have getters and setters
    var perimeter: Double {
        get {
            return 2 * PI * radius
        }
        set {
            // assigning the value for radius
            // newValue is the implicit name for the setter, an explicit one can be specified
            radius = newValue / (2*PI)
        }
    }
}
// triangle.perimeter will get the value & if we try to set 
// the property of triangle.perimeter = <something>, it'll go ahead and update the radius
// TODO: Run this and set the perimeter
// TODO(Advanced): write a unit test to check the setter * getter, wait 
// is that sort of a Behaviour or Integration test or some test with scenarios?
```
DOUBT: Changing the value of properties defined by the superclass. Any additional setup work that uses methods, getters, or setters can also be done at this point.
^^ What does this mean? Example??

