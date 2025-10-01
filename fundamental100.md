# 100 Fundamental Dart Examples

This file provides 100 fundamental Dart code examples for experienced programmers.

### 1. Hello World

A simple "Hello, World!" program is the entry point for any language.
Dart's `main` function is the top-level function where app execution starts.

```dart
// All Dart programs start with the main() function.
void main() {
  print('Hello, World!');
}
```

### 2. Variables and Types

Dart is a statically-typed language.
Variables can be declared with `var`, or with a specific type like `String` or `int`.

```dart
void main() {
  var name = 'Dart'; // Type inferred as String.
  String language = 'Dart'; // Explicitly typed.
  print('Hello, $name! Welcome to $language.');
}
```

### 3. Final and Const

`final` variables can be set only once.
`const` is used for variables that are compile-time constants.

```dart
void main() {
  final String name = 'Dart'; // Runtime constant.
  const double pi = 3.14; // Compile-time constant.
  print('Name is $name, and PI is $pi.');
}
```

### 4. Basic Data Types

Dart supports common data types like numbers, strings, and booleans.

```dart
void main() {
  int age = 30;
  double price = 19.99;
  String city = 'New York';
  bool isTrue = true;
  print('$city is $age years old. Is it true? $isTrue. Price: $price');
}
```

### 5. String Interpolation

String interpolation is a convenient way to build strings.
Use `${expression}` to embed an expression's value inside a string.

```dart
void main() {
  String s1 = 'Dart is fun';
  String s2 = 'and powerful.';
  print('$s1 $s2');
}
```

### 6. Multi-line Strings

Triple quotes (`'''` or `"""`) create multi-line strings.

```dart
void main() {
  String multiLine = """
This is a
multi-line string
in Dart.
""";
  print(multiLine);
}
```

### 7. Basic Arithmetic Operators

Dart supports standard arithmetic operators.

```dart
void main() {
  int a = 10;
  int b = 3;
  print('a + b = ${a + b}'); // Addition
  print('a - b = ${a - b}'); // Subtraction
  print('a * b = ${a * b}'); // Multiplication
  print('a / b = ${a / b}'); // Division
  print('a % b = ${a % b}'); // Modulo
}
```

### 8. Increment and Decrement Operators

Use `++` and `--` to increment and decrement variables.

```dart
void main() {
  int a = 5;
  print('a++ = ${a++}'); // Prints 5, then a becomes 6.
  print('++a = ${++a}'); // a becomes 7, then prints 7.
}
```

### 9. Equality and Relational Operators

Compare values using `==`, `!=`, `<`, `>`, `<=`, and `>=`.

```dart
void main() {
  int a = 10;
  int b = 20;
  print('a == b: ${a == b}'); // false
  print('a != b: ${a != b}'); // true
  print('a > b: ${a > b}'); // false
}
```

### 10. Type Test Operators

`is` and `is!` check if an object has a certain type.

```dart
void main() {
  Object text = 'hello';
  if (text is String) {
    print('The object is a String.');
  }
}
```

### 11. Logical Operators

Combine boolean expressions using `&&` (AND), `||` (OR), and `!` (NOT).

```dart
void main() {
  bool isRaining = true;
  bool haveUmbrella = false;
  if (isRaining && !haveUmbrella) {
    print('You might get wet!');
  }
}
```

### 12. The `if` Statement

Conditional logic is handled with `if`, `else if`, and `else`.

```dart
void main() {
  int number = 0;
  if (number > 0) {
    print('The number is positive.');
  } else if (number < 0) {
    print('The number is negative.');
  } else {
    print('The number is zero.');
  }
}
```

### 13. The `for` Loop

Iterate a specific number of times with a standard `for` loop.

```dart
void main() {
  for (int i = 1; i <= 5; i++) {
    print('Iteration $i');
  }
}
```

### 14. The `for-in` Loop

Iterate over a collection of items.

```dart
void main() {
  var numbers = [1, 2, 3, 4, 5];
  for (var n in numbers) {
    print(n);
  }
}
```

### 15. The `while` Loop

A `while` loop executes as long as its condition is true.

```dart
void main() {
  int i = 1;
  while (i <= 5) {
    print('While loop iteration $i');
    i++;
  }
}
```

### 16. The `do-while` Loop

A `do-while` loop executes at least once.

```dart
void main() {
  int i = 1;
  do {
    print('Do-while loop iteration $i');
    i++;
  } while (i <= 5);
}
```

### 17. The `break` Statement

`break` is used to exit a loop or switch statement prematurely.

```dart
void main() {
  for (int i = 1; i <= 10; i++) {
    if (i == 5) {
      break; // Exit the loop when i is 5.
    }
    print(i);
  }
}
```

### 18. The `continue` Statement

`continue` skips the current iteration and proceeds to the next one.

```dart
void main() {
  for (int i = 1; i <= 5; i++) {
    if (i == 3) {
      continue; // Skip iteration when i is 3.
    }
    print(i);
  }
}
```

### 19. The `switch` Statement

A `switch` statement compares an integer, string, or compile-time constant.
Non-empty `case` clauses must end with `break`.

```dart
void main() {
  String command = 'OPEN';
  switch (command) {
    case 'OPEN':
      print('Opening...');
      break;
    case 'CLOSED':
      print('Closing...');
      break;
    default:
      print('Unknown command.');
  }
}
```

### 20. The `assert` Statement

`assert` disrupts normal execution if a boolean condition is false.
It's ignored in production code.

```dart
void main() {
  String? text; // Nullable type
  assert(text != null, 'Text must not be null');
}
```

### 21. Nullable Variables

Declare variables that can be `null` by adding a `?` to their type.

```dart
void main() {
  String? name; // Can be null or a string.
  name = 'Dart';
  print(name.length);
}
```

### 22. Null-aware Operator `?.`

The `?.` operator prevents a `null` error.
If the object is `null`, the expression evaluates to `null`.

```dart
void main() {
  String? name;
  // If name is null, the expression returns null.
  print(name?.length);
}
```

### 23. Null Coalescing Operator `??`

The `??` operator returns the expression on its left unless it's `null`.
If it is `null`, it evaluates and returns the expression on its right.

```dart
void main() {
  String? name;
  // If name is null, use 'Guest'.
  print('Hello, ${name ?? 'Guest'}');
}
```

### 24. Null Coalescing Assignment `??=`

The `??=` operator assigns a value to a variable only if that variable is `null`.

```dart
void main() {
  String? name;
  name ??= 'Default Name'; // Assigns if name is null.
  print(name);
}
```

### 25. Conditional Expressions

Dart has a concise way to write conditional expressions.
`condition ? expr1 : expr2`

```dart
void main() {
  int a = 10;
  String result = a > 5 ? 'Greater than 5' : 'Less than or equal to 5';
  print(result);
}
```

### 26. Basic Function

Functions are defined with a return type and a list of parameters.

```dart
void greet(String name) {
  print('Hello, $name!');
}

void main() {
  greet('Dart');
}
```

### 27. Function with Return Value

Functions can return a value of any type.

```dart
int add(int a, int b) {
  return a + b;
}

void main() {
  print('Sum: ${add(5, 3)}');
}
```

### 28. Arrow Syntax for Functions

For functions that contain a single expression, you can use arrow syntax.

```dart
int multiply(int a, int b) => a * b;

void main() {
  print('Product: ${multiply(5, 3)}');
}
```

### 29. Positional Parameters

Parameters are positional by default. The order of arguments matters.

```dart
void printPerson(String name, int age) {
  print('$name is $age years old.');
}

void main() {
  printPerson('Alice', 30);
}
```

### 30. Named Parameters

Wrap parameters in `{}` to specify them by name when calling the function.

```dart
void printInfo({String? name, int? age}) {
  print('Name: $name, Age: $age');
}

void main() {
  printInfo(age: 30, name: 'Bob');
}
```

### 31. Required Named Parameters

Use the `required` keyword to indicate that a named parameter must be provided.

```dart
void printDetails({required String name, int? age}) {
  print('Name: $name, Age: ${age ?? 'N/A'}');
}

void main() {
  printDetails(name: 'Charlie');
}
```

### 32. Optional Positional Parameters

Wrap parameters in `[]` to make them optional. They must come after any required positional parameters.

```dart
void printAddress(String street, [String? city]) {
  print('Street: $street, City: ${city ?? 'Unknown'}');
}

void main() {
  printAddress('123 Main St');
  printAddress('456 Oak Ave', 'New York');
}
```

### 33. Default Parameter Values

You can provide default values for named and positional parameters.

```dart
void sayHello({String name = 'Guest'}) {
  print('Hello, $name!');
}

void main() {
  sayHello();
  sayHello(name: 'Dart');
}
```

### 34. Anonymous Functions (Lambdas)

Functions can be created without a name. They are often used as arguments.

```dart
void main() {
  var fruits = ['Apple', 'Banana', 'Orange'];
  fruits.forEach((fruit) {
    print('I love $fruit!');
  });
}
```

### 35. Higher-Order Functions

A function that takes another function as a parameter or returns a function.

```dart
void operate(int a, int b, int Function(int, int) operation) {
  print('Result: ${operation(a, b)}');
}

int add(int a, int b) => a + b;
int subtract(int a, int b) => a - b;

void main() {
  operate(10, 5, add);
  operate(10, 5, subtract);
}
```

### 36. Lexical Scope

Dart is a lexically scoped language. Variables are accessible based on the location of the code.

```dart
String topLevel = 'Top';

void main() {
  String insideMain = 'Main';

  void myFunction() {
    String insideFunction = 'Function';
    print(topLevel);
    print(insideMain);
    print(insideFunction);
  }

  myFunction();
}
```

### 37. Lexical Closures

A function object "closes over" variables from its lexical scope.

```dart
Function makeAdder(int addBy) {
  return (int i) => addBy + i;
}

void main() {
  var add2 = makeAdder(2);
  var add4 = makeAdder(4);

  print(add2(3)); // 5
  print(add4(3)); // 7
}
```

### 38. First-Class Functions

Functions are first-class citizens in Dart. They can be assigned to variables and passed as arguments.

```dart
void sayHi(String name) {
  print('Hi, $name!');
}

void main() {
  var greeting = sayHi;
  greeting('Dart');
}
```

### 39. Function Type Aliases (`typedef`)

A `typedef` can be used to define a function type alias.

```dart
typedef IntBinaryOperation = int Function(int, int);

int add(int a, int b) => a + b;
int subtract(int a, int b) => a - b;

void main() {
  IntBinaryOperation myOperation = add;
  print(myOperation(10, 2)); // 12
  myOperation = subtract;
  print(myOperation(10, 2)); // 8
}
```

### 40. The `main` Function Arguments

The `main` function can optionally take a list of strings as arguments.

```dart
// Run from the command line: dart your_script.dart arg1 arg2
void main(List<String> arguments) {
  print('Arguments passed: $arguments');
}
```

### 41. Basic Class and Instance Creation

Define a class with the `class` keyword. Create an instance with `ClassName()`.

```dart
class Greeter {
  String name = 'World';

  void greet() {
    print('Hello, $name!');
  }
}

void main() {
  var greeter = Greeter();
  greeter.greet();
}
```

### 42. Constructors

A constructor is a special function that creates an instance of a class.

```dart
class Person {
  String name;
  int age;

  // Constructor
  Person(String name, int age) {
    this.name = name;
    this.age = age;
  }

  void introduce() {
    print('Hi, I am $name, $age years old.');
  }
}

void main() {
  var person = Person('Alice', 25);
  person.introduce();
}
```

### 43. Syntactic Sugar for Constructors

Dart provides a handy syntax for assigning constructor arguments to instance variables.

```dart
class Person {
  String name;
  int age;

  // Syntactic sugar for the constructor
  Person(this.name, this.age);

  void introduce() {
    print('Hi, I am $name, $age years old.');
  }
}

void main() {
  var person = Person('Bob', 30);
  person.introduce();
}
```

### 44. Named Constructors

A class can have multiple constructors by giving them names.

```dart
class Point {
  double x, y;

  Point(this.x, this.y);

  // Named constructor
  Point.origin() {
    x = 0;
    y = 0;
  }

  void display() {
    print('($x, $y)');
  }
}

void main() {
  var p1 = Point(2, 3);
  var p2 = Point.origin();
  p1.display();
  p2.display();
}
```

### 45. Initializer Lists

An initializer list allows you to initialize instance variables before the constructor body runs.

```dart
class Rectangle {
  final double left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Initializer list sets 'right' and 'bottom' before the body runs.
  Rectangle.fromPoints(Point p1, Point p2)
      : left = p1.x < p2.x ? p1.x : p2.x,
        top = p1.y < p2.y ? p1.y : p2.y,
        width = (p1.x - p2.x).abs(),
        height = (p1.y - p2.y).abs();
}

class Point {
  double x, y;
  Point(this.x, this.y);
}

void main() {
  var rect = Rectangle.fromPoints(Point(10, 20), Point(30, 5));
  print(rect.width);
}
```

### 46. Redirecting Constructors

A redirecting constructor's body is empty, with the constructor call appearing after a colon.

```dart
class Car {
  String make;
  String model;

  Car(this.make, this.model);

  // Redirecting constructor
  Car.ford(String model) : this('Ford', model);

  void display() {
    print('$make $model');
  }
}

void main() {
  var car = Car.ford('Mustang');
  car.display();
}
```

### 47. Constant Constructors

If your class produces objects that never change, you can make these objects compile-time constants.

```dart
class ImmutablePoint {
  final double x, y;

  const ImmutablePoint(this.x, this.y);
}

void main() {
  var p1 = const ImmutablePoint(1, 1);
  var p2 = const ImmutablePoint(1, 1);

  // p1 and p2 are the same instance.
  print(identical(p1, p2)); // true
}
```

### 48. Factory Constructors

A factory constructor can return an instance from a cache or a sub-type.

```dart
class Logger {
  final String name;
  static final Map<String, Logger> _cache = {};

  factory Logger(String name) {
    return _cache.putIfAbsent(name, () => Logger._internal(name));
  }

  Logger._internal(this.name);

  void log(String msg) {
    print('$name: $msg');
  }
}

void main() {
  var logger1 = Logger('UI');
  var logger2 = Logger('UI');
  logger1.log('Button clicked');
  print(identical(logger1, logger2)); // true
}
```

### 49. Getters and Setters

Getters and setters are special methods that provide read and write access to an object's properties.

```dart
class Rectangle {
  double left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Getter
  double get right => left + width;
  // Setter
  set right(double value) => left = value - width;
}

void main() {
  var rect = Rectangle(0, 0, 10, 10);
  print(rect.right); // 10
  rect.right = 20;
  print(rect.left); // 10
}
```

### 50. Static Variables

Static variables are common to all instances of a class.

```dart
class Circle {
  static const double pi = 3.14159;
  double radius;

  Circle(this.radius);
}

void main() {
  print(Circle.pi);
}
```

### 51. Static Methods

Static methods are not tied to an instance of a class.

```dart
class MathUtils {
  static int sum(int a, int b) {
    return a + b;
  }
}

void main() {
  print(MathUtils.sum(10, 5));
}
```

### 52. Inheritance

A class can inherit from another class using the `extends` keyword.

```dart
class Animal {
  void eat() {
    print('Animal is eating.');
  }
}

class Dog extends Animal {
  void bark() {
    print('Dog is barking.');
  }
}

void main() {
  var dog = Dog();
  dog.eat();
  dog.bark();
}
```

### 53. Overriding Members

A subclass can provide its own implementation of an instance method, getter, or setter.

```dart
class Animal {
  void makeSound() {
    print('Some generic animal sound.');
  }
}

class Cat extends Animal {
  @override
  void makeSound() {
    print('Meow!');
  }
}

void main() {
  var cat = Cat();
  cat.makeSound();
}
```

### 54. The `super` Keyword

The `super` keyword is used to refer to the parent class.

```dart
class Vehicle {
  String name;
  Vehicle(this.name);
}

class Car extends Vehicle {
  Car(String name) : super(name) {
    print('Car constructor called for $name');
  }
}

void main() {
  var car = Car('Tesla');
}
```

### 55. Abstract Classes

Abstract classes cannot be instantiated. They often have abstract methods.

```dart
abstract class Shape {
  void draw(); // Abstract method
}

class Circle extends Shape {
  @override
  void draw() {
    print('Drawing a circle.');
  }
}

void main() {
  // var shape = Shape(); // Error: Abstract classes can't be instantiated.
  var circle = Circle();
  circle.draw();
}
```

### 56. Interfaces

Every class implicitly defines an interface. You can implement one or more interfaces.

```dart
class Speaker {
  void speak() {}
}

class Person implements Speaker {
  @override
  void speak() {
    print('Hello!');
  }
}

void main() {
  var person = Person();
  person.speak();
}
```

### 57. Mixins

Mixins are a way of reusing a class's code in multiple class hierarchies.

```dart
mixin Flyer {
  void fly() {
    print('I am flying.');
  }
}

class Bird with Flyer {}

class Plane with Flyer {}

void main() {
  var bird = Bird();
  bird.fly();
  var plane = Plane();
  plane.fly();
}
```

### 58. `on` Keyword for Mixins

The `on` keyword restricts the use of a mixin to classes that extend or implement a certain type.

```dart
class Animal {}
class Mammal extends Animal {}

mixin Walker on Animal {
  void walk() {
    print('Walking...');
  }
}

class Human extends Mammal with Walker {}
// class Fish with Walker {} // Error: Fish does not extend Animal.

void main() {
  var human = Human();
  human.walk();
}
```

### 59. Extension Methods

Extension methods allow you to add new functionality to existing libraries.

```dart
extension on String {
  int toInt() {
    return int.parse(this);
  }
}

void main() {
  String numberString = '123';
  print(numberString.toInt());
}
```

### 60. `toString()` Method

Override `toString()` to provide a custom string representation of your object.

```dart
class Point {
  int x, y;
  Point(this.x, this.y);

  @override
  String toString() => 'Point($x, $y)';
}

void main() {
  var p = Point(2, 3);
  print(p.toString());
}
```

### 61. `hashCode` Getter

If you override `==`, you must also override the `hashCode` getter.

```dart
class Person {
  final String name;
  Person(this.name);

  @override
  bool operator ==(Object other) =>
      other is Person && other.name == name;

  @override
  int get hashCode => name.hashCode;
}

void main() {
  var p1 = Person('A');
  var p2 = Person('A');
  print(p1.hashCode == p2.hashCode);
}
```

### 62. `noSuchMethod()`

If you try to use a non-existent method or property, `noSuchMethod()` is called.

```dart
class MockProxy {
  @override
  dynamic noSuchMethod(Invocation invocation) {
    print('Called: ${invocation.memberName}');
    return null;
  }
}

void main() {
  dynamic mock = MockProxy();
  mock.someMethod();
}
```

### 63. Enum Types

Enums are used to define a fixed number of constant values.

```dart
enum Color { red, green, blue }

void main() {
  print(Color.red.index); // 0
  print(Color.values); // [Color.red, Color.green, Color.blue]
}
```

### 64. Enhanced Enums

Enums can have fields, methods, and constructors.

```dart
enum Vehicle implements Comparable<Vehicle> {
  car(tires: 4, passengers: 5, carbonPerKilometer: 400),
  bus(tires: 6, passengers: 50, carbonPerKilometer: 800),
  bicycle(tires: 2, passengers: 1, carbonPerKilometer: 0);

  const Vehicle({
    required this.tires,
    required this.passengers,
    required this.carbonPerKilometer,
  });

  final int tires;
  final int passengers;
  final int carbonPerKilometer;

  int get carbonFootprint => carbonPerKilometer * 5;

  @override
  int compareTo(Vehicle other) => carbonFootprint - other.carbonFootprint;
}

void main() {
  print(Vehicle.car.carbonFootprint);
}
```

### 65. Callable Classes

A class can be made callable like a function by implementing the `call()` method.

```dart
class WannabeFunction {
  String call(String a, String b, String c) => '$a $b $c!';
}

void main() {
  var wf = WannabeFunction();
  var out = wf('Hi', 'there,', 'gang');
  print(out);
}
```

### 66. Isolates (Conceptual)

Dart is single-threaded, but it can run code in parallel using Isolates.
Each isolate has its own memory and event loop.

```dart
import 'dart:isolate';

void myIsolate(SendPort sendPort) {
  sendPort.send('Hello from the isolate!');
}

void main() async {
  final receivePort = ReceivePort();
  await Isolate.spawn(myIsolate, receivePort.sendPort);
  receivePort.listen((message) {
    print(message);
  });
}
```

### 67. Class Modifiers: `abstract`

An `abstract` class cannot be instantiated but can be subclassed.

```dart
abstract class Doer {
  void doSomething(); // Abstract method
}

class EffectiveDoer extends Doer {
  void doSomething() {
    print('Doing something');
  }
}

void main() {
  var d = EffectiveDoer();
  d.doSomething();
}
```

### 68. Class Modifiers: `interface`

An `interface` class can be implemented but not extended outside its own library.

```dart
// In a library file `shape.dart`
// library shape;
// interface class Shape {
//   void draw();
// }

// In another file
// import 'shape.dart';
// class Circle implements Shape { ... } // OK
// class MyShape extends Shape { ... } // Error
```

### 69. Class Modifiers: `final`

A `final` class cannot be extended or implemented outside its own library.

```dart
// In a library file `point.dart`
// library point;
// final class Point { ... }

// In another file
// import 'point.dart';
// class MyPoint extends Point { ... } // Error
// class MyPoint implements Point { ... } // Error
```

### 70. Class Modifiers: `mixin`

A `mixin` class can be used as a mixin, but cannot be instantiated.

```dart
mixin class Musical {
  bool canPlayPiano = false;
  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    }
  }
}

class Musician with Musical {
  // ...
}
```

### 71. Lists

A `List` is an ordered collection of objects.

```dart
void main() {
  var list = [1, 2, 3];
  print(list.length); // 3
  print(list[1]); // 2
}
```

### 72. List with a specific type

You can create a list that holds only a specific type of object.

```dart
void main() {
  List<String> names = ['Alice', 'Bob'];
  // names.add(123); // Error: A value of type 'int' can't be assigned to a variable of type 'String'.
  print(names);
}
```

### 73. `const` Lists

A `const` list cannot be changed.

```dart
void main() {
  final list = const [1, 2, 3];
  // list.add(4); // Error: Cannot add to an unmodifiable list
}
```

### 74. Spread Operator (`...`)

The spread operator (`...`) provides a concise way to insert multiple values into a collection.

```dart
void main() {
  var list1 = [1, 2, 3];
  var list2 = [0, ...list1, 4];
  print(list2); // [0, 1, 2, 3, 4]
}
```

### 75. Null-aware Spread Operator (`...?`)

The null-aware spread operator (`...?`) avoids an exception when the expression to the right of the operator is null.

```dart
void main() {
  List<int>? list1;
  var list2 = [0, ...?list1];
  print(list2); // [0]
}
```

### 76. Collection `if`

Use collection `if` to build a collection conditionally.

```dart
void main() {
  bool promoActive = true;
  var nav = [
    'Home',
    'Furniture',
    'Plants',
    if (promoActive) 'Outlet'
  ];
  print(nav);
}
```

### 77. Collection `for`

Use collection `for` to build a collection using a loop.

```dart
void main() {
  var listOfInts = [1, 2, 3];
  var listOfStrings = [
    '#0',
    for (var i in listOfInts) '#$i'
  ];
  print(listOfStrings); // [#0, #1, #2, #3]
}
```

### 78. Sets

A `Set` is an unordered collection of unique items.

```dart
void main() {
  var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};
  halogens.add('fluorine'); // Does nothing, already in the set.
  print(halogens.length); // 5
}
```

### 79. Creating a Set from a List

You can create a set from a list to get the unique items.

```dart
void main() {
  var numbers = [1, 2, 2, 3, 4, 4, 4];
  var uniqueNumbers = Set.from(numbers);
  print(uniqueNumbers); // {1, 2, 3, 4}
}
```

### 80. Maps

A `Map` is a collection of key-value pairs.

```dart
void main() {
  var gifts = {
    // Key:    Value
    'first': 'partridge',
    'second': 'turtledoves',
    'fifth': 'golden rings'
  };

  print(gifts['fifth']); // golden rings
}
```

### 81. Adding to a Map

You can add a new key-value pair to a map.

```dart
void main() {
  var gifts = {'first': 'partridge'};
  gifts['fourth'] = 'calling birds';
  print(gifts);
}
```

### 82. `const` Maps

A `const` map cannot be changed.

```dart
void main() {
  final constantMap = const {
    2: 'helium',
    10: 'neon',
    18: 'argon',
  };
  // constantMap[2] = 'Helium'; // Error
}
```

### 83. Generics

Generics allow you to define types that are parameterized over other types.

```dart
abstract class Cache<T> {
  T getByKey(String key);
  void setByKey(String key, T value);
}

class MemoryCache<T> implements Cache<T> {
  final Map<String, T> _cache = {};

  @override
  T getByKey(String key) => _cache[key]!;

  @override
  void setByKey(String key, T value) => _cache[key] = value;
}
```

### 84. Using Generic Types

Using generic types improves type safety.

```dart
void main() {
  var names = <String>['Seth', 'Kathy', 'Lars'];
  var pages = <String, String>{
    'index.html': 'Homepage',
    'robots.txt': 'Hints for web robots'
  };
  print(names);
  print(pages);
}
```

### 85. Restricting the Parameterized Type

When implementing a generic type, you might want to limit its parameters. You can use `extends`.

```dart
class SomeBaseClass {}
class Foo<T extends SomeBaseClass> {
  // Implementation...
}

class Extender extends SomeBaseClass {}

void main() {
  var someBaseClassFoo = Foo<SomeBaseClass>();
  var extenderFoo = Foo<Extender>();
  // var foo = Foo<Object>(); // Error
}
```

### 86. Asynchronous Programming: `Future`

A `Future` represents a potential value, or error, that will be available at some time in the future.

```dart
Future<String> fetchUserData() {
  return Future.delayed(Duration(seconds: 2), () => 'User Data');
}

void main() {
  print('Fetching user data...');
  fetchUserData().then((data) {
    print(data);
  });
}
```

### 87. `async` and `await`

The `async` and `await` keywords provide a declarative way to define asynchronous functions and use their results.

```dart
Future<String> fetchUserData() {
  return Future.delayed(Duration(seconds: 2), () => 'User Data');
}

void printUserData() async {
  print('Fetching user data...');
  String data = await fetchUserData();
  print(data);
}

void main() {
  printUserData();
}
```

### 88. Handling Errors with `try-catch`

Use a `try-catch` block to handle errors in an `async` function.

```dart
Future<String> fetchUserData() {
  return Future.delayed(Duration(seconds: 2), () => throw 'Could not fetch data');
}

void printUserData() async {
  try {
    String data = await fetchUserData();
    print(data);
  } catch (e) {
    print('Caught error: $e');
  }
}

void main() {
  printUserData();
}
```

### 89. `Future.catchError`

You can also handle errors using the `catchError` method on a `Future`.

```dart
void main() {
  Future.delayed(Duration(seconds: 1), () => throw 'Error!')
      .then((value) => print('Success'))
      .catchError((e) => print('Caught: $e'));
}
```

### 90. `Future.wait`

`Future.wait` allows you to run multiple futures and wait for them all to complete.

```dart
Future<String> fetchFirst() => Future.delayed(Duration(seconds: 1), () => 'First');
Future<String> fetchSecond() => Future.delayed(Duration(seconds: 2), () => 'Second');

void main() async {
  List<String> results = await Future.wait([fetchFirst(), fetchSecond()]);
  print(results);
}
```

### 91. Streams

A `Stream` is a sequence of asynchronous events. It's like an async `Iterable`.

```dart
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}

void main() {
  countStream(5).listen((data) {
    print(data);
  });
}
```

### 92. `async*` and `yield`

An `async*` function generates a stream. The `yield` statement is used to push values into the stream.

```dart
Stream<String> getNames() async* {
  yield 'Alice';
  await Future.delayed(Duration(milliseconds: 500));
  yield 'Bob';
  await Future.delayed(Duration(milliseconds: 500));
  yield 'Charlie';
}

void main() {
  getNames().listen((name) {
    print(name);
  });
}
```

### 93. `await for`

An `await for` loop is used to iterate over the events of a stream.

```dart
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    yield i;
  }
}

void main() async {
  await for (var i in countStream(3)) {
    print(i);
  }
}
```

### 94. `Stream.fromIterable`

Create a stream from a collection of items.

```dart
void main() {
  Stream.fromIterable([1, 2, 3]).listen((i) {
    print(i);
  });
}
```

### 95. `Stream.periodic`

Creates a stream that repeatedly emits events at a given interval.

```dart
void main() {
  Stream.periodic(Duration(seconds: 1), (i) => i).take(5).listen((i) {
    print(i);
  });
}
```

### 96. `StreamController`

A `StreamController` gives you more control over a stream.

```dart
import 'dart:async';

void main() {
  final controller = StreamController<int>();

  controller.stream.listen((data) {
    print('Data: $data');
  });

  controller.add(1);
  controller.add(2);
  controller.add(3);

  controller.close();
}
```

### 97. Broadcast Streams

A standard stream can only have one listener. A broadcast stream allows any number of listeners.

```dart
import 'dart:async';

void main() {
  final controller = StreamController<int>.broadcast();
  final stream = controller.stream;

  stream.listen((data) => print('Listener 1: $data'));
  stream.listen((data) => print('Listener 2: $data'));

  controller.add(1);
  controller.add(2);
}
```

### 98. Transforming a Stream with `map`

The `map` method transforms each event of a stream into a new event.

```dart
void main() {
  Stream.fromIterable([1, 2, 3])
      .map((i) => i * 10)
      .listen((i) => print(i));
}
```

### 99. Filtering a Stream with `where`

The `where` method filters the events of a stream based on a condition.

```dart
void main() {
  Stream.fromIterable([1, 2, 3, 4, 5])
      .where((i) => i.isEven)
      .listen((i) => print(i));
}
```

### 100. Handling Errors in a Stream

Errors in a stream can be handled by the `onError` callback in `listen`.

```dart
void main() {
  Stream.fromFuture(
          Future.delayed(Duration(seconds: 1)).then((_) => throw 'Oops'))
      .listen(
        (data) => print('Data: $data'),
        onError: (err) => print('Error: $err'),
        onDone: () => print('Done'),
      );
}
```