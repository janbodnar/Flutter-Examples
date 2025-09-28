# Flutter Object-Oriented Programming - 30 Essential Examples

This comprehensive guide covers Object-Oriented Programming concepts in  
Flutter/Dart with 30 practical examples, progressing from basic classes  
to advanced inheritance patterns, interfaces, and sophisticated OOP  
design patterns. Each example demonstrates real-world usage scenarios  
and follows Flutter best practices.

## In-Depth Description of Object-Oriented Programming in Dart

Object-Oriented Programming (OOP) is a fundamental programming paradigm  
that organizes code into reusable, maintainable structures called classes  
and objects. Dart provides comprehensive OOP support with classes,  
inheritance, interfaces, mixins, and advanced features that enable  
elegant and efficient Flutter application development.

**Classes and Objects**: The foundation of OOP where classes serve as  
blueprints defining properties and behaviors, while objects are instances  
that represent specific data with associated functionality. Dart classes  
support constructors, private members, static members, and comprehensive  
property management.

**Inheritance**: A mechanism enabling classes to inherit properties and  
methods from parent classes, promoting code reuse and establishing  
hierarchical relationships. Dart supports single inheritance with method  
overriding, super calls, and abstract classes for defining contracts.

**Interfaces**: Contracts that define method signatures without  
implementation, ensuring consistent behavior across different classes.  
Dart uses implicit interfaces and abstract classes to achieve interface-  
like functionality with flexible implementation strategies.

**Polymorphism**: The ability to treat objects of different types  
uniformly through common interfaces or base classes. This enables  
flexible, extensible code that can work with multiple implementations  
transparently.

**Encapsulation**: The principle of hiding internal implementation  
details while exposing controlled interfaces. Dart achieves this through  
private members (prefixed with _), getters, setters, and carefully  
designed public APIs.

**Mixins**: A powerful Dart feature enabling multiple inheritance-like  
behavior by composing functionality from multiple sources. Mixins  
provide code reuse without the complexity of multiple inheritance.

## Basic Class Definition

Creating a simple class with properties, constructor, and methods.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Basic Class Definition',
      theme: ThemeData.dark(),
      home: const BasicClassDemo(),
    );
  }
}

// Basic class demonstrating core OOP concepts
class Person {
  // Properties (fields)
  String name;
  int age;
  String email;
  
  // Constructor
  Person(this.name, this.age, this.email);
  
  // Named constructor
  Person.withDefaults(this.name) : age = 0, email = '';
  
  // Method
  String getInfo() {
    return 'Name: $name, Age: $age, Email: $email';
  }
  
  void celebrateBirthday() {
    age++;
  }
  
  // Override toString for better string representation
  @override
  String toString() {
    return 'Person(name: $name, age: $age, email: $email)';
  }
}

class BasicClassDemo extends StatefulWidget {
  const BasicClassDemo({super.key});

  @override
  State<BasicClassDemo> createState() => _BasicClassDemoState();
}

class _BasicClassDemoState extends State<BasicClassDemo> {
  late Person person;
  
  @override
  void initState() {
    super.initState();
    person = Person('Alice Johnson', 28, 'alice@example.com');
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic Class Definition'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Person Information',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Name: ${person.name}'),
                    Text('Age: ${person.age}'),
                    Text('Email: ${person.email}'),
                    const SizedBox(height: 12),
                    Text('Full Info: ${person.getInfo()}'),
                    const SizedBox(height: 12),
                    Text('String Representation: ${person.toString()}'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Center(
              child: ElevatedButton(
                onPressed: () {
                  setState(() {
                    person.celebrateBirthday();
                  });
                },
                child: const Text('Celebrate Birthday'),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Class Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Properties: name, age, email'),
                    const Text('• Default constructor: Person(name, age, email)'),
                    const Text('• Named constructor: Person.withDefaults(name)'),
                    const Text('• Instance methods: getInfo(), celebrateBirthday()'),
                    const Text('• Method override: toString()'),
                    const Text('• State management with object mutation'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates fundamental class concepts including property  
definition, constructors, methods, and object state management. The  
Person class shows how to encapsulate data and behavior while providing  
controlled access through methods. The Flutter UI demonstrates real-time  
object state changes and method invocation.

## Private Members and Encapsulation

Implementing data hiding with private members and controlled access.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Private Members and Encapsulation',
      theme: ThemeData.dark(),
      home: const EncapsulationDemo(),
    );
  }
}

// Class demonstrating encapsulation principles
class BankAccount {
  // Private properties (encapsulated data)
  String _accountNumber;
  String _holderName;
  double _balance;
  List<String> _transactionHistory;
  
  // Constructor
  BankAccount(this._accountNumber, this._holderName, [double initialBalance = 0.0])
      : _balance = initialBalance,
        _transactionHistory = ['Account opened with balance: \$${initialBalance.toStringAsFixed(2)}'];
  
  // Getters (controlled read access)
  String get accountNumber => _accountNumber.replaceRange(4, 8, '****');
  String get holderName => _holderName;
  double get balance => _balance;
  List<String> get recentTransactions => _transactionHistory.take(5).toList();
  
  // Setter with validation (controlled write access)
  set holderName(String name) {
    if (name.isNotEmpty && name.length >= 2) {
      _holderName = name;
      _addTransaction('Name updated to: $name');
    } else {
      throw ArgumentError('Name must be at least 2 characters long');
    }
  }
  
  // Public methods with business logic
  bool deposit(double amount) {
    if (amount > 0) {
      _balance += amount;
      _addTransaction('Deposited: \$${amount.toStringAsFixed(2)}');
      return true;
    }
    return false;
  }
  
  bool withdraw(double amount) {
    if (amount > 0 && amount <= _balance) {
      _balance -= amount;
      _addTransaction('Withdrew: \$${amount.toStringAsFixed(2)}');
      return true;
    }
    return false;
  }
  
  // Private helper method
  void _addTransaction(String transaction) {
    _transactionHistory.add('${DateTime.now().toString().substring(0, 19)}: $transaction');
    if (_transactionHistory.length > 10) {
      _transactionHistory.removeAt(0);
    }
  }
  
  // Method to check if account can perform transaction
  bool canWithdraw(double amount) {
    return amount > 0 && amount <= _balance;
  }
}

class EncapsulationDemo extends StatefulWidget {
  const EncapsulationDemo({super.key});

  @override
  State<EncapsulationDemo> createState() => _EncapsulationDemoState();
}

class _EncapsulationDemoState extends State<EncapsulationDemo> {
  late BankAccount account;
  final TextEditingController _amountController = TextEditingController();
  String _message = '';

  @override
  void initState() {
    super.initState();
    account = BankAccount('1234-5678-9012', 'John Doe', 1000.0);
  }

  @override
  void dispose() {
    _amountController.dispose();
    super.dispose();
  }

  void _performTransaction(bool isDeposit) {
    final amount = double.tryParse(_amountController.text);
    if (amount == null) {
      setState(() {
        _message = 'Please enter a valid amount';
      });
      return;
    }

    bool success;
    if (isDeposit) {
      success = account.deposit(amount);
      setState(() {
        _message = success ? 'Deposit successful!' : 'Deposit failed!';
      });
    } else {
      success = account.withdraw(amount);
      setState(() {
        _message = success ? 'Withdrawal successful!' : 'Insufficient funds or invalid amount!';
      });
    }
    
    _amountController.clear();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Private Members and Encapsulation'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Account Information',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Account: ${account.accountNumber}'),
                    Text('Holder: ${account.holderName}'),
                    Text(
                      'Balance: \$${account.balance.toStringAsFixed(2)}',
                      style: const TextStyle(
                        fontSize: 20,
                        fontWeight: FontWeight.bold,
                        color: Colors.greenAccent,
                      ),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Transaction Panel',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    TextField(
                      controller: _amountController,
                      decoration: const InputDecoration(
                        labelText: 'Amount',
                        prefixText: '\$',
                        border: OutlineInputBorder(),
                      ),
                      keyboardType: TextInputType.number,
                    ),
                    const SizedBox(height: 16),
                    Row(
                      children: [
                        Expanded(
                          child: ElevatedButton(
                            onPressed: () => _performTransaction(true),
                            child: const Text('Deposit'),
                          ),
                        ),
                        const SizedBox(width: 16),
                        Expanded(
                          child: ElevatedButton(
                            onPressed: () => _performTransaction(false),
                            style: ElevatedButton.styleFrom(
                              backgroundColor: Colors.orange,
                            ),
                            child: const Text('Withdraw'),
                          ),
                        ),
                      ],
                    ),
                    if (_message.isNotEmpty) ...[
                      const SizedBox(height: 12),
                      Text(
                        _message,
                        style: TextStyle(
                          color: _message.contains('successful') 
                              ? Colors.green 
                              : Colors.red,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ],
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Recent Transactions',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    ...account.recentTransactions.map((transaction) => Padding(
                      padding: const EdgeInsets.symmetric(vertical: 2),
                      child: Text(
                        transaction,
                        style: const TextStyle(fontSize: 12),
                      ),
                    )),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Encapsulation Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Private fields: _balance, _accountNumber, _transactionHistory'),
                    const Text('• Getters: controlled read access to internal state'),
                    const Text('• Setters: validated write access with business rules'),
                    const Text('• Private methods: _addTransaction() for internal logic'),
                    const Text('• Public interface: deposit(), withdraw(), canWithdraw()'),
                    const Text('• Data hiding: account number masked in getter'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates encapsulation principles through private members  
and controlled access patterns. The BankAccount class hides internal  
implementation details while providing a clean public interface. Getters  
and setters control data access, and private methods handle internal  
business logic safely.

## Static Members and Class-Level Functionality

Implementing static properties and methods for class-level operations.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Static Members',
      theme: ThemeData.dark(),
      home: const StaticMembersDemo(),
    );
  }
}

// Utility class with static members
class MathUtils {
  // Static constants
  static const double pi = 3.14159;
  static const double e = 2.71828;
  
  // Static properties with getters
  static int _operationCount = 0;
  static int get operationCount => _operationCount;
  
  // Static methods
  static double add(double a, double b) {
    _operationCount++;
    return a + b;
  }
  
  static double multiply(double a, double b) {
    _operationCount++;
    return a * b;
  }
  
  static double power(double base, double exponent) {
    _operationCount++;
    double result = 1;
    for (int i = 0; i < exponent; i++) {
      result *= base;
    }
    return result;
  }
  
  static double circleArea(double radius) {
    _operationCount++;
    return pi * radius * radius;
  }
  
  static double circleCircumference(double radius) {
    _operationCount++;
    return 2 * pi * radius;
  }
  
  static void resetOperationCount() {
    _operationCount = 0;
  }
  
  static String getStatistics() {
    return 'Total operations performed: $_operationCount';
  }
}

// Class with both static and instance members
class Counter {
  // Static member to track all counters
  static int _totalCounters = 0;
  static List<Counter> _allCounters = [];
  
  // Instance members
  int _value = 0;
  final String name;
  
  // Constructor
  Counter(this.name) {
    _totalCounters++;
    _allCounters.add(this);
  }
  
  // Instance methods
  void increment() => _value++;
  void decrement() => _value > 0 ? _value-- : 0;
  void reset() => _value = 0;
  
  // Instance getter
  int get value => _value;
  
  // Static getters
  static int get totalCounters => _totalCounters;
  static int get totalValue => _allCounters.fold(0, (sum, counter) => sum + counter.value);
  static List<String> get allCounterNames => _allCounters.map((c) => c.name).toList();
  
  // Static methods
  static void resetAllCounters() {
    for (Counter counter in _allCounters) {
      counter.reset();
    }
  }
  
  static Counter? findCounterByName(String name) {
    try {
      return _allCounters.firstWhere((counter) => counter.name == name);
    } catch (e) {
      return null;
    }
  }
  
  @override
  String toString() => 'Counter($name: $_value)';
}

class StaticMembersDemo extends StatefulWidget {
  const StaticMembersDemo({super.key});

  @override
  State<StaticMembersDemo> createState() => _StaticMembersDemoState();
}

class _StaticMembersDemoState extends State<StaticMembersDemo> {
  final List<Counter> _counters = [];
  final TextEditingController _nameController = TextEditingController();
  final TextEditingController _num1Controller = TextEditingController();
  final TextEditingController _num2Controller = TextEditingController();
  double _mathResult = 0.0;

  @override
  void initState() {
    super.initState();
    // Create some default counters
    _counters.add(Counter('Main Counter'));
    _counters.add(Counter('Secondary Counter'));
  }

  @override
  void dispose() {
    _nameController.dispose();
    _num1Controller.dispose();
    _num2Controller.dispose();
    super.dispose();
  }

  void _createCounter() {
    if (_nameController.text.isNotEmpty) {
      setState(() {
        _counters.add(Counter(_nameController.text));
        _nameController.clear();
      });
    }
  }

  void _performMathOperation(String operation) {
    final num1 = double.tryParse(_num1Controller.text) ?? 0;
    final num2 = double.tryParse(_num2Controller.text) ?? 0;
    
    setState(() {
      switch (operation) {
        case 'add':
          _mathResult = MathUtils.add(num1, num2);
          break;
        case 'multiply':
          _mathResult = MathUtils.multiply(num1, num2);
          break;
        case 'power':
          _mathResult = MathUtils.power(num1, num2);
          break;
        case 'circle_area':
          _mathResult = MathUtils.circleArea(num1);
          break;
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Static Members'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Static Constants Demo
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Static Constants & Utilities',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('π (Pi): ${MathUtils.pi}'),
                    Text('e: ${MathUtils.e}'),
                    Text(MathUtils.getStatistics()),
                    const SizedBox(height: 16),
                    Row(
                      children: [
                        Expanded(
                          child: TextField(
                            controller: _num1Controller,
                            decoration: const InputDecoration(
                              labelText: 'Number 1',
                              border: OutlineInputBorder(),
                            ),
                            keyboardType: TextInputType.number,
                          ),
                        ),
                        const SizedBox(width: 8),
                        Expanded(
                          child: TextField(
                            controller: _num2Controller,
                            decoration: const InputDecoration(
                              labelText: 'Number 2',
                              border: OutlineInputBorder(),
                            ),
                            keyboardType: TextInputType.number,
                          ),
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performMathOperation('add'),
                          child: const Text('Add'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performMathOperation('multiply'),
                          child: const Text('Multiply'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performMathOperation('power'),
                          child: const Text('Power'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performMathOperation('circle_area'),
                          child: const Text('Circle Area'),
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Text(
                      'Result: ${_mathResult.toStringAsFixed(2)}',
                      style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Counter Management Demo
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Static vs Instance Members',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Total Counters: ${Counter.totalCounters}'),
                    Text('Total Value: ${Counter.totalValue}'),
                    const SizedBox(height: 16),
                    Row(
                      children: [
                        Expanded(
                          child: TextField(
                            controller: _nameController,
                            decoration: const InputDecoration(
                              labelText: 'Counter Name',
                              border: OutlineInputBorder(),
                            ),
                          ),
                        ),
                        const SizedBox(width: 8),
                        ElevatedButton(
                          onPressed: _createCounter,
                          child: const Text('Create'),
                        ),
                      ],
                    ),
                    const SizedBox(height: 16),
                    ElevatedButton(
                      onPressed: () {
                        setState(() {
                          Counter.resetAllCounters();
                        });
                      },
                      style: ElevatedButton.styleFrom(backgroundColor: Colors.orange),
                      child: const Text('Reset All Counters'),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Individual Counters
            ..._counters.map((counter) => Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: [
                    Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(counter.name, style: const TextStyle(fontWeight: FontWeight.bold)),
                        Text('Value: ${counter.value}'),
                      ],
                    ),
                    Row(
                      children: [
                        IconButton(
                          onPressed: () {
                            setState(() {
                              counter.decrement();
                            });
                          },
                          icon: const Icon(Icons.remove),
                        ),
                        IconButton(
                          onPressed: () {
                            setState(() {
                              counter.increment();
                            });
                          },
                          icon: const Icon(Icons.add),
                        ),
                        IconButton(
                          onPressed: () {
                            setState(() {
                              counter.reset();
                            });
                          },
                          icon: const Icon(Icons.refresh),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            )),
            
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Static Members Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Static constants: pi, e for mathematical operations'),
                    const Text('• Static variables: _operationCount, _totalCounters'),
                    const Text('• Static methods: MathUtils utility functions'),
                    const Text('• Static getters: totalCounters, totalValue'),
                    const Text('• Class-level state: tracking all instances'),
                    const Text('• Mixed static/instance design patterns'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates static members and class-level functionality  
through utility classes and instance tracking. Static members belong to  
the class rather than instances, enabling shared state and utility  
functions. The MathUtils class provides stateless utilities, while  
Counter shows mixed static/instance patterns for managing collections.

## Basic Inheritance

Implementing inheritance with parent-child class relationships.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Basic Inheritance',
      theme: ThemeData.dark(),
      home: const InheritanceDemo(),
    );
  }
}

// Base class (parent)
class Animal {
  String name;
  int age;
  String species;
  
  Animal(this.name, this.age, this.species);
  
  // Method that can be inherited
  String makeSound() {
    return '$name makes a generic animal sound';
  }
  
  String getInfo() {
    return 'Name: $name, Age: $age, Species: $species';
  }
  
  void sleep() {
    print('$name is sleeping');
  }
  
  // Virtual method (can be overridden)
  String getDescription() {
    return 'This is $name, a $age-year-old $species';
  }
}

// Derived class (child) - inherits from Animal
class Dog extends Animal {
  String breed;
  bool isGoodBoy;
  
  // Constructor that calls parent constructor
  Dog(String name, int age, this.breed, [this.isGoodBoy = true]) 
      : super(name, age, 'Dog');
  
  // Override parent method
  @override
  String makeSound() {
    return '$name barks: Woof! Woof!';
  }
  
  // Override parent method with additional functionality
  @override
  String getDescription() {
    String baseDescription = super.getDescription();
    return '$baseDescription and is a $breed breed. Good boy: $isGoodBoy';
  }
  
  // New method specific to Dog
  void fetch() {
    print('$name is fetching the ball!');
  }
  
  void wagTail() {
    if (isGoodBoy) {
      print('$name is wagging tail happily!');
    }
  }
}

// Another derived class - inherits from Animal
class Cat extends Animal {
  bool isIndoor;
  int livesRemaining;
  
  Cat(String name, int age, [this.isIndoor = true, this.livesRemaining = 9]) 
      : super(name, age, 'Cat');
  
  // Override parent method
  @override
  String makeSound() {
    return '$name meows: Meow... Purr...';
  }
  
  // Override parent method
  @override
  String getDescription() {
    String baseDescription = super.getDescription();
    return '$baseDescription, lives ${isIndoor ? "indoors" : "outdoors"} and has $livesRemaining lives remaining';
  }
  
  // New methods specific to Cat
  void climb() {
    print('$name is climbing up high!');
  }
  
  void hunt() {
    if (!isIndoor) {
      print('$name is hunting for prey!');
    } else {
      print('$name is hunting dust bunnies indoors!');
    }
  }
  
  void useLive() {
    if (livesRemaining > 0) {
      livesRemaining--;
      print('$name used a life! $livesRemaining lives remaining.');
    }
  }
}

// Derived class from Dog (multi-level inheritance)
class ServiceDog extends Dog {
  String serviceType;
  bool isWorking;
  
  ServiceDog(String name, int age, String breed, this.serviceType) 
      : isWorking = false,
        super(name, age, breed, true);
  
  @override
  String getDescription() {
    String baseDescription = super.getDescription();
    return '$baseDescription. Service type: $serviceType. Currently ${isWorking ? "working" : "resting"}';
  }
  
  void startWork() {
    isWorking = true;
    print('$name is now working as a $serviceType service dog');
  }
  
  void stopWork() {
    isWorking = false;
    print('$name is now off duty and can play');
  }
  
  // Override with service dog specific behavior
  @override
  void fetch() {
    if (isWorking) {
      print('$name is performing service duties, not playing fetch');
    } else {
      super.fetch();
    }
  }
}

class InheritanceDemo extends StatefulWidget {
  const InheritanceDemo({super.key});

  @override
  State<InheritanceDemo> createState() => _InheritanceDemoState();
}

class _InheritanceDemoState extends State<InheritanceDemo> {
  late List<Animal> animals;
  late ServiceDog serviceDog;
  String _actionResult = '';

  @override
  void initState() {
    super.initState();
    animals = [
      Dog('Buddy', 3, 'Golden Retriever', true),
      Cat('Whiskers', 5, true, 7),
      Dog('Max', 2, 'German Shepherd', true),
      Cat('Shadow', 4, false, 9),
    ];
    serviceDog = ServiceDog('Guide', 4, 'Labrador', 'Guide Dog');
  }

  void _performAction(Animal animal, String action) {
    setState(() {
      switch (action) {
        case 'sound':
          _actionResult = animal.makeSound();
          break;
        case 'sleep':
          animal.sleep();
          _actionResult = '${animal.name} is sleeping...';
          break;
        case 'special':
          if (animal is Dog) {
            if (animal is ServiceDog) {
              animal.isWorking ? animal.stopWork() : animal.startWork();
              _actionResult = '${animal.name} work status changed';
            } else {
              animal.fetch();
              _actionResult = '${animal.name} fetched the ball!';
            }
          } else if (animal is Cat) {
            animal.hunt();
            _actionResult = '${animal.name} went hunting!';
          }
          break;
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic Inheritance'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            if (_actionResult.isNotEmpty) ...[
              Card(
                color: Colors.blue.shade800,
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Last Action Result',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Text(_actionResult),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],
            
            // Animal list
            ...animals.map((animal) => Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      animal.name,
                      style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Text(animal.getDescription()),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performAction(animal, 'sound'),
                          child: const Text('Make Sound'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performAction(animal, 'sleep'),
                          child: const Text('Sleep'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performAction(animal, 'special'),
                          child: Text(animal is Dog ? 'Fetch' : 'Hunt'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            )),
            
            const SizedBox(height: 16),
            
            // Service Dog (multi-level inheritance)
            Card(
              color: Colors.green.shade800,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      '${serviceDog.name} (Service Dog)',
                      style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Text(serviceDog.getDescription()),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performAction(serviceDog, 'sound'),
                          child: const Text('Make Sound'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performAction(serviceDog, 'special'),
                          child: Text(serviceDog.isWorking ? 'Stop Work' : 'Start Work'),
                        ),
                        ElevatedButton(
                          onPressed: () {
                            setState(() {
                              serviceDog.fetch();
                              _actionResult = serviceDog.isWorking 
                                  ? '${serviceDog.name} is working, no fetch!'
                                  : '${serviceDog.name} fetched while off duty!';
                            });
                          },
                          child: const Text('Fetch'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Inheritance Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Base class: Animal with common properties and methods'),
                    const Text('• Derived classes: Dog, Cat inherit from Animal'),
                    const Text('• Constructor chaining: super() calls parent constructor'),
                    const Text('• Method overriding: makeSound(), getDescription()'),
                    const Text('• Super calls: accessing parent implementation'),
                    const Text('• Multi-level inheritance: ServiceDog extends Dog'),
                    const Text('• Polymorphism: Animal list containing different types'),
                    const Text('• Type checking: is operator for runtime type identification'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates basic inheritance concepts through an animal  
hierarchy. The Animal base class provides common functionality, while  
Dog and Cat derive specialized behavior. ServiceDog shows multi-level  
inheritance, and the UI demonstrates polymorphism by treating different  
types uniformly through their common base class.

## Method Overriding and Super Calls

Advanced inheritance with method overriding and parent method access.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Method Overriding',
      theme: ThemeData.dark(),
      home: const MethodOverridingDemo(),
    );
  }
}

// Base class for different shapes
abstract class Shape {
  String name;
  String color;
  
  Shape(this.name, this.color);
  
  // Virtual method that should be overridden
  double calculateArea();
  
  // Virtual method that can be extended
  String getDescription() {
    return '$name with $color color';
  }
  
  // Method that uses template method pattern
  String getFullInfo() {
    return '${getDescription()}, Area: ${calculateArea().toStringAsFixed(2)}';
  }
  
  // Method that can be overridden for custom drawing
  Widget draw(BuildContext context) {
    return Container(
      width: 100,
      height: 100,
      decoration: BoxDecoration(
        color: _getColor(),
        border: Border.all(color: Colors.white, width: 2),
      ),
      child: Center(
        child: Text(
          name,
          style: const TextStyle(fontWeight: FontWeight.bold),
        ),
      ),
    );
  }
  
  Color _getColor() {
    switch (color.toLowerCase()) {
      case 'red': return Colors.red;
      case 'blue': return Colors.blue;
      case 'green': return Colors.green;
      case 'yellow': return Colors.yellow;
      case 'purple': return Colors.purple;
      default: return Colors.grey;
    }
  }
}

// Rectangle implementation
class Rectangle extends Shape {
  double width;
  double height;
  
  Rectangle(this.width, this.height, String color) 
      : super('Rectangle', color);
  
  @override
  double calculateArea() {
    return width * height;
  }
  
  @override
  String getDescription() {
    // Call parent method and extend it
    String baseDescription = super.getDescription();
    return '$baseDescription (${width}x${height})';
  }
  
  @override
  Widget draw(BuildContext context) {
    return Container(
      width: width * 10,
      height: height * 10,
      decoration: BoxDecoration(
        color: _getColor().withOpacity(0.7),
        border: Border.all(color: Colors.white, width: 2),
      ),
      child: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(name, style: const TextStyle(fontWeight: FontWeight.bold)),
            Text('${width}x${height}', style: const TextStyle(fontSize: 12)),
          ],
        ),
      ),
    );
  }
  
  Color _getColor() {
    switch (color.toLowerCase()) {
      case 'red': return Colors.red;
      case 'blue': return Colors.blue;
      case 'green': return Colors.green;
      case 'yellow': return Colors.yellow;
      case 'purple': return Colors.purple;
      default: return Colors.grey;
    }
  }
}

// Circle implementation
class Circle extends Shape {
  double radius;
  
  Circle(this.radius, String color) : super('Circle', color);
  
  @override
  double calculateArea() {
    return 3.14159 * radius * radius;
  }
  
  @override
  String getDescription() {
    String baseDescription = super.getDescription();
    return '$baseDescription (radius: $radius)';
  }
  
  @override
  Widget draw(BuildContext context) {
    return Container(
      width: radius * 20,
      height: radius * 20,
      decoration: BoxDecoration(
        color: _getColor().withOpacity(0.7),
        shape: BoxShape.circle,
        border: Border.all(color: Colors.white, width: 2),
      ),
      child: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(name, style: const TextStyle(fontWeight: FontWeight.bold)),
            Text('r=$radius', style: const TextStyle(fontSize: 12)),
          ],
        ),
      ),
    );
  }
  
  Color _getColor() {
    switch (color.toLowerCase()) {
      case 'red': return Colors.red;
      case 'blue': return Colors.blue;
      case 'green': return Colors.green;
      case 'yellow': return Colors.yellow;
      case 'purple': return Colors.purple;
      default: return Colors.grey;
    }
  }
}

// Triangle implementation
class Triangle extends Shape {
  double base;
  double height;
  
  Triangle(this.base, this.height, String color) 
      : super('Triangle', color);
  
  @override
  double calculateArea() {
    return 0.5 * base * height;
  }
  
  @override
  String getDescription() {
    String baseDescription = super.getDescription();
    return '$baseDescription (base: $base, height: $height)';
  }
  
  @override
  Widget draw(BuildContext context) {
    return CustomPaint(
      size: Size(base * 10, height * 10),
      painter: TrianglePainter(color: _getColor()),
      child: Container(
        width: base * 10,
        height: height * 10,
        alignment: Alignment.center,
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(name, style: const TextStyle(fontWeight: FontWeight.bold)),
            Text('${base}x${height}', style: const TextStyle(fontSize: 12)),
          ],
        ),
      ),
    );
  }
  
  Color _getColor() {
    switch (color.toLowerCase()) {
      case 'red': return Colors.red;
      case 'blue': return Colors.blue;
      case 'green': return Colors.green;
      case 'yellow': return Colors.yellow;
      case 'purple': return Colors.purple;
      default: return Colors.grey;
    }
  }
}

// Custom painter for triangle
class TrianglePainter extends CustomPainter {
  final Color color;
  
  TrianglePainter({required this.color});
  
  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()
      ..color = color.withOpacity(0.7)
      ..style = PaintingStyle.fill;
    
    final path = Path();
    path.moveTo(size.width / 2, 0);
    path.lineTo(0, size.height);
    path.lineTo(size.width, size.height);
    path.close();
    
    canvas.drawPath(path, paint);
    
    // Border
    final borderPaint = Paint()
      ..color = Colors.white
      ..style = PaintingStyle.stroke
      ..strokeWidth = 2;
    
    canvas.drawPath(path, borderPaint);
  }
  
  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => false;
}

class MethodOverridingDemo extends StatefulWidget {
  const MethodOverridingDemo({super.key});

  @override
  State<MethodOverridingDemo> createState() => _MethodOverridingDemoState();
}

class _MethodOverridingDemoState extends State<MethodOverridingDemo> {
  List<Shape> shapes = [];
  String _selectedColor = 'blue';
  final List<String> _colors = ['red', 'blue', 'green', 'yellow', 'purple'];

  @override
  void initState() {
    super.initState();
    shapes = [
      Rectangle(5, 3, 'red'),
      Circle(4, 'blue'),
      Triangle(6, 4, 'green'),
      Rectangle(3, 3, 'yellow'),
      Circle(2.5, 'purple'),
    ];
  }

  void _addShape(String shapeType) {
    setState(() {
      switch (shapeType) {
        case 'Rectangle':
          shapes.add(Rectangle(4, 3, _selectedColor));
          break;
        case 'Circle':
          shapes.add(Circle(3, _selectedColor));
          break;
        case 'Triangle':
          shapes.add(Triangle(5, 3, _selectedColor));
          break;
      }
    });
  }

  double _getTotalArea() {
    return shapes.fold(0.0, (sum, shape) => sum + shape.calculateArea());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Method Overriding'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Shape creation controls
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Create New Shape',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        const Text('Color: '),
                        DropdownButton<String>(
                          value: _selectedColor,
                          items: _colors.map((color) => DropdownMenuItem(
                            value: color,
                            child: Text(color.toUpperCase()),
                          )).toList(),
                          onChanged: (value) {
                            setState(() {
                              _selectedColor = value!;
                            });
                          },
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _addShape('Rectangle'),
                          child: const Text('Add Rectangle'),
                        ),
                        ElevatedButton(
                          onPressed: () => _addShape('Circle'),
                          child: const Text('Add Circle'),
                        ),
                        ElevatedButton(
                          onPressed: () => _addShape('Triangle'),
                          child: const Text('Add Triangle'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Summary information
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Shape Summary',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Total shapes: ${shapes.length}'),
                    Text('Total area: ${_getTotalArea().toStringAsFixed(2)}'),
                    Text('Rectangles: ${shapes.whereType<Rectangle>().length}'),
                    Text('Circles: ${shapes.whereType<Circle>().length}'),
                    Text('Triangles: ${shapes.whereType<Triangle>().length}'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Shape list with details
            const Text(
              'Shape Details',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            ...shapes.asMap().entries.map((entry) {
              int index = entry.key;
              Shape shape = entry.value;
              
              return Card(
                margin: const EdgeInsets.only(bottom: 12),
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Row(
                    children: [
                      // Visual representation
                      Container(
                        width: 80,
                        height: 80,
                        padding: const EdgeInsets.all(8),
                        child: shape.draw(context),
                      ),
                      const SizedBox(width: 16),
                      // Shape information
                      Expanded(
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(
                              'Shape #${index + 1}',
                              style: const TextStyle(fontWeight: FontWeight.bold),
                            ),
                            Text(shape.getDescription()),
                            Text('Area: ${shape.calculateArea().toStringAsFixed(2)}'),
                            Text(shape.getFullInfo()),
                          ],
                        ),
                      ),
                      // Remove button
                      IconButton(
                        onPressed: () {
                          setState(() {
                            shapes.removeAt(index);
                          });
                        },
                        icon: const Icon(Icons.delete, color: Colors.red),
                      ),
                    ],
                  ),
                ),
              );
            }),
            
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Method Overriding Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Abstract methods: calculateArea() must be implemented'),
                    const Text('• Virtual methods: getDescription() can be overridden'),
                    const Text('• Super calls: extending parent method functionality'),
                    const Text('• Template method pattern: getFullInfo() uses overridden methods'),
                    const Text('• Custom widget drawing: draw() method overriding'),
                    const Text('• Polymorphic behavior: same interface, different implementations'),
                    const Text('• Runtime type checking: whereType<T>() for filtering'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates method overriding and super calls through a  
shape hierarchy. Each shape implements calculateArea() differently while  
extending getDescription() using super calls. The template method pattern  
in getFullInfo() shows how base classes can orchestrate overridden  
methods for consistent behavior.

## Abstract Classes and Methods

Implementing abstract classes that define contracts for derived classes.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Abstract Classes',
      theme: ThemeData.dark(),
      home: const AbstractClassesDemo(),
    );
  }
}

// Abstract base class for payment processing
abstract class PaymentProcessor {
  String processorName;
  double transactionFee;
  
  PaymentProcessor(this.processorName, this.transactionFee);
  
  // Abstract methods that must be implemented by concrete classes
  Future<bool> processPayment(double amount, String currency);
  String getPaymentStatus(String transactionId);
  Map<String, dynamic> getTransactionDetails(String transactionId);
  
  // Concrete method that can be inherited or overridden
  double calculateTotalAmount(double amount) {
    return amount + (amount * transactionFee);
  }
  
  // Template method using abstract methods
  Future<Map<String, dynamic>> executePayment(double amount, String currency) async {
    final timestamp = DateTime.now();
    final transactionId = _generateTransactionId();
    
    try {
      final success = await processPayment(amount, currency);
      final totalAmount = calculateTotalAmount(amount);
      
      return {
        'transactionId': transactionId,
        'processor': processorName,
        'amount': amount,
        'totalAmount': totalAmount,
        'currency': currency,
        'success': success,
        'timestamp': timestamp,
        'status': getPaymentStatus(transactionId),
        'details': success ? getTransactionDetails(transactionId) : null,
      };
    } catch (e) {
      return {
        'transactionId': transactionId,
        'processor': processorName,
        'success': false,
        'error': e.toString(),
        'timestamp': timestamp,
      };
    }
  }
  
  String _generateTransactionId() {
    return 'TXN_${DateTime.now().millisecondsSinceEpoch}';
  }
  
  // Virtual method that can be overridden
  String getProcessorInfo() {
    return 'Processor: $processorName, Fee: ${(transactionFee * 100).toStringAsFixed(1)}%';
  }
}

// Concrete implementation for credit card processing
class CreditCardProcessor extends PaymentProcessor {
  String cardType;
  bool requiresCVV;
  
  CreditCardProcessor(this.cardType, [this.requiresCVV = true]) 
      : super('Credit Card ($cardType)', 0.029); // 2.9% fee
  
  @override
  Future<bool> processPayment(double amount, String currency) async {
    // Simulate payment processing delay
    await Future.delayed(const Duration(seconds: 2));
    
    // Simulate payment success/failure (90% success rate)
    return DateTime.now().millisecond % 10 != 0;
  }
  
  @override
  String getPaymentStatus(String transactionId) {
    // Simulate random status
    final statuses = ['completed', 'pending', 'authorized'];
    return statuses[DateTime.now().millisecond % statuses.length];
  }
  
  @override
  Map<String, dynamic> getTransactionDetails(String transactionId) {
    return {
      'cardType': cardType,
      'maskedCardNumber': '**** **** **** 1234',
      'authCode': 'AUTH${DateTime.now().millisecond}',
      'cvvRequired': requiresCVV,
      'processor': 'Visa/MasterCard Network',
    };
  }
  
  @override
  String getProcessorInfo() {
    String baseInfo = super.getProcessorInfo();
    return '$baseInfo, Card Type: $cardType, CVV Required: $requiresCVV';
  }
}

// Concrete implementation for PayPal processing
class PayPalProcessor extends PaymentProcessor {
  String accountEmail;
  bool isSandbox;
  
  PayPalProcessor(this.accountEmail, [this.isSandbox = false]) 
      : super('PayPal', 0.034); // 3.4% fee
  
  @override
  Future<bool> processPayment(double amount, String currency) async {
    // Simulate PayPal API call delay
    await Future.delayed(const Duration(seconds: 1));
    
    // PayPal has slightly higher success rate
    return DateTime.now().millisecond % 20 != 0;
  }
  
  @override
  String getPaymentStatus(String transactionId) {
    final statuses = ['completed', 'pending_review', 'cleared'];
    return statuses[DateTime.now().millisecond % statuses.length];
  }
  
  @override
  Map<String, dynamic> getTransactionDetails(String transactionId) {
    return {
      'paypalAccount': accountEmail,
      'paypalTransactionId': 'PP${DateTime.now().millisecondsSinceEpoch}',
      'environment': isSandbox ? 'sandbox' : 'production',
      'paymentMethod': 'PayPal Balance',
    };
  }
  
  @override
  String getProcessorInfo() {
    String baseInfo = super.getProcessorInfo();
    return '$baseInfo, Account: $accountEmail, Sandbox: $isSandbox';
  }
}

// Concrete implementation for cryptocurrency processing
class CryptoProcessor extends PaymentProcessor {
  String cryptoType;
  String walletAddress;
  
  CryptoProcessor(this.cryptoType, this.walletAddress) 
      : super('Crypto ($cryptoType)', 0.015); // 1.5% fee
  
  @override
  Future<bool> processPayment(double amount, String currency) async {
    // Simulate blockchain confirmation delay
    await Future.delayed(const Duration(seconds: 3));
    
    // Crypto has lower success rate due to network issues
    return DateTime.now().millisecond % 15 != 0;
  }
  
  @override
  String getPaymentStatus(String transactionId) {
    final statuses = ['confirmed', 'pending_confirmation', 'broadcasted'];
    return statuses[DateTime.now().millisecond % statuses.length];
  }
  
  @override
  Map<String, dynamic> getTransactionDetails(String transactionId) {
    return {
      'cryptoType': cryptoType,
      'walletAddress': walletAddress,
      'blockHash': '0x${DateTime.now().millisecondsSinceEpoch.toRadixString(16)}',
      'confirmations': DateTime.now().millisecond % 6 + 1,
      'gasUsed': '${21000 + DateTime.now().millisecond % 50000}',
    };
  }
  
  @override
  double calculateTotalAmount(double amount) {
    // Crypto has fixed fee structure
    double baseFee = super.calculateTotalAmount(amount);
    double networkFee = 0.001; // Additional network fee
    return baseFee + networkFee;
  }
  
  @override
  String getProcessorInfo() {
    String baseInfo = super.getProcessorInfo();
    return '$baseInfo, Crypto: $cryptoType, Wallet: ${walletAddress.substring(0, 8)}...';
  }
}

class AbstractClassesDemo extends StatefulWidget {
  const AbstractClassesDemo({super.key});

  @override
  State<AbstractClassesDemo> createState() => _AbstractClassesDemoState();
}

class _AbstractClassesDemoState extends State<AbstractClassesDemo> {
  List<PaymentProcessor> processors = [];
  List<Map<String, dynamic>> transactions = [];
  bool _isProcessing = false;
  final TextEditingController _amountController = TextEditingController(text: '100.00');

  @override
  void initState() {
    super.initState();
    processors = [
      CreditCardProcessor('Visa', true),
      PayPalProcessor('user@example.com', false),
      CryptoProcessor('Bitcoin', '1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa'),
      CreditCardProcessor('MasterCard', false),
      PayPalProcessor('business@company.com', true),
    ];
  }

  @override
  void dispose() {
    _amountController.dispose();
    super.dispose();
  }

  Future<void> _processPayment(PaymentProcessor processor) async {
    final amount = double.tryParse(_amountController.text) ?? 0.0;
    if (amount <= 0) return;

    setState(() {
      _isProcessing = true;
    });

    try {
      final result = await processor.executePayment(amount, 'USD');
      setState(() {
        transactions.insert(0, result);
        _isProcessing = false;
      });
    } catch (e) {
      setState(() {
        _isProcessing = false;
      });
      
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Payment failed: $e')),
        );
      }
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Abstract Classes'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Payment amount input
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Payment Amount',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    TextField(
                      controller: _amountController,
                      decoration: const InputDecoration(
                        labelText: 'Amount',
                        prefixText: '\$',
                        suffixText: 'USD',
                        border: OutlineInputBorder(),
                      ),
                      keyboardType: TextInputType.number,
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Payment processors
            const Text(
              'Available Payment Processors',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            ...processors.map((processor) => Card(
              margin: const EdgeInsets.only(bottom: 8),
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      processor.processorName,
                      style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Text(processor.getProcessorInfo()),
                    const SizedBox(height: 8),
                    Text(
                      'Total Amount: \$${processor.calculateTotalAmount(double.tryParse(_amountController.text) ?? 0.0).toStringAsFixed(2)}',
                      style: const TextStyle(fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    ElevatedButton(
                      onPressed: _isProcessing ? null : () => _processPayment(processor),
                      child: _isProcessing 
                          ? const Text('Processing...')
                          : const Text('Process Payment'),
                    ),
                  ],
                ),
              ),
            )),
            
            const SizedBox(height: 16),
            
            // Transaction history
            if (transactions.isNotEmpty) ...[
              const Text(
                'Transaction History',
                style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
              ),
              const SizedBox(height: 12),
              ...transactions.take(5).map((transaction) => Card(
                margin: const EdgeInsets.only(bottom: 8),
                color: transaction['success'] ? Colors.green.shade900 : Colors.red.shade900,
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text(
                            transaction['transactionId'],
                            style: const TextStyle(fontWeight: FontWeight.bold),
                          ),
                          Icon(
                            transaction['success'] ? Icons.check_circle : Icons.error,
                            color: transaction['success'] ? Colors.green : Colors.red,
                          ),
                        ],
                      ),
                      const SizedBox(height: 8),
                      Text('Processor: ${transaction['processor']}'),
                      Text('Amount: \$${transaction['amount']?.toStringAsFixed(2) ?? 'N/A'}'),
                      if (transaction.containsKey('totalAmount'))
                        Text('Total: \$${transaction['totalAmount']?.toStringAsFixed(2)}'),
                      if (transaction.containsKey('status'))
                        Text('Status: ${transaction['status']}'),
                      if (transaction['details'] != null) ...[
                        const SizedBox(height: 8),
                        const Text('Details:', style: TextStyle(fontWeight: FontWeight.bold)),
                        ...transaction['details'].entries.map<Widget>((entry) =>
                          Text('  ${entry.key}: ${entry.value}')
                        ),
                      ],
                      if (transaction.containsKey('error'))
                        Text('Error: ${transaction['error']}', 
                             style: const TextStyle(color: Colors.red)),
                    ],
                  ),
                ),
              )),
            ],
            
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Abstract Classes Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Abstract class: PaymentProcessor defines contract'),
                    const Text('• Abstract methods: processPayment(), getPaymentStatus()'),
                    const Text('• Concrete methods: calculateTotalAmount() with default implementation'),
                    const Text('• Template method: executePayment() orchestrates abstract methods'),
                    const Text('• Polymorphism: List<PaymentProcessor> with different implementations'),
                    const Text('• Method overriding: calculateTotalAmount() in CryptoProcessor'),
                    const Text('• Interface segregation: each processor implements specific behavior'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates abstract classes through a payment processing  
system. The abstract PaymentProcessor class defines contracts that  
concrete implementations must fulfill, while providing shared  
functionality through template methods. Each processor implements  
abstract methods differently while inheriting common behavior.

## Interface Implementation

Implementing implicit interfaces and abstract classes as contracts.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Interface Implementation',
      theme: ThemeData.dark(),
      home: const InterfaceDemo(),
    );
  }
}

// Abstract interface for data serialization
abstract class Serializable {
  Map<String, dynamic> toJson();
  void fromJson(Map<String, dynamic> json);
  String serialize();
}

// Abstract interface for data validation
abstract class Validatable {
  bool isValid();
  List<String> getValidationErrors();
}

// Abstract interface for auditing
abstract class Auditable {
  DateTime get createdAt;
  DateTime get updatedAt;
  String get createdBy;
  void updateAuditInfo(String user);
}

// Concrete class implementing multiple interfaces
class User implements Serializable, Validatable, Auditable {
  String id;
  String name;
  String email;
  int age;
  DateTime _createdAt;
  DateTime _updatedAt;
  String _createdBy;

  User({
    required this.id,
    required this.name,
    required this.email,
    required this.age,
    String? createdBy,
  }) : _createdAt = DateTime.now(),
       _updatedAt = DateTime.now(),
       _createdBy = createdBy ?? 'system';

  // Implementing Serializable interface
  @override
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'email': email,
      'age': age,
      'createdAt': _createdAt.toIso8601String(),
      'updatedAt': _updatedAt.toIso8601String(),
      'createdBy': _createdBy,
    };
  }

  @override
  void fromJson(Map<String, dynamic> json) {
    id = json['id'] ?? '';
    name = json['name'] ?? '';
    email = json['email'] ?? '';
    age = json['age'] ?? 0;
    _createdAt = DateTime.tryParse(json['createdAt'] ?? '') ?? DateTime.now();
    _updatedAt = DateTime.tryParse(json['updatedAt'] ?? '') ?? DateTime.now();
    _createdBy = json['createdBy'] ?? 'system';
  }

  @override
  String serialize() {
    return '${toJson()}'.replaceAll('{', '').replaceAll('}', '');
  }

  // Implementing Validatable interface
  @override
  bool isValid() {
    return getValidationErrors().isEmpty;
  }

  @override
  List<String> getValidationErrors() {
    List<String> errors = [];
    
    if (id.isEmpty) errors.add('ID is required');
    if (name.isEmpty || name.length < 2) errors.add('Name must be at least 2 characters');
    if (!email.contains('@') || !email.contains('.')) errors.add('Invalid email format');
    if (age < 0 || age > 150) errors.add('Age must be between 0 and 150');
    
    return errors;
  }

  // Implementing Auditable interface
  @override
  DateTime get createdAt => _createdAt;

  @override
  DateTime get updatedAt => _updatedAt;

  @override
  String get createdBy => _createdBy;

  @override
  void updateAuditInfo(String user) {
    _updatedAt = DateTime.now();
    // Note: We don't change _createdBy, only track who made the update
  }

  // Additional User-specific methods
  bool get isAdult => age >= 18;
  
  String getDisplayName() {
    return '$name (${email})';
  }
}

// Another class implementing the same interfaces
class Product implements Serializable, Validatable, Auditable {
  String sku;
  String name;
  double price;
  int quantity;
  String category;
  DateTime _createdAt;
  DateTime _updatedAt;
  String _createdBy;

  Product({
    required this.sku,
    required this.name,
    required this.price,
    required this.quantity,
    required this.category,
    String? createdBy,
  }) : _createdAt = DateTime.now(),
       _updatedAt = DateTime.now(),
       _createdBy = createdBy ?? 'system';

  // Implementing Serializable interface
  @override
  Map<String, dynamic> toJson() {
    return {
      'sku': sku,
      'name': name,
      'price': price,
      'quantity': quantity,
      'category': category,
      'createdAt': _createdAt.toIso8601String(),
      'updatedAt': _updatedAt.toIso8601String(),
      'createdBy': _createdBy,
    };
  }

  @override
  void fromJson(Map<String, dynamic> json) {
    sku = json['sku'] ?? '';
    name = json['name'] ?? '';
    price = (json['price'] ?? 0.0).toDouble();
    quantity = json['quantity'] ?? 0;
    category = json['category'] ?? '';
    _createdAt = DateTime.tryParse(json['createdAt'] ?? '') ?? DateTime.now();
    _updatedAt = DateTime.tryParse(json['updatedAt'] ?? '') ?? DateTime.now();
    _createdBy = json['createdBy'] ?? 'system';
  }

  @override
  String serialize() {
    return 'Product[$sku]: $name, \$${price.toStringAsFixed(2)}, Qty: $quantity';
  }

  // Implementing Validatable interface
  @override
  bool isValid() {
    return getValidationErrors().isEmpty;
  }

  @override
  List<String> getValidationErrors() {
    List<String> errors = [];
    
    if (sku.isEmpty) errors.add('SKU is required');
    if (name.isEmpty) errors.add('Product name is required');
    if (price < 0) errors.add('Price cannot be negative');
    if (quantity < 0) errors.add('Quantity cannot be negative');
    if (category.isEmpty) errors.add('Category is required');
    
    return errors;
  }

  // Implementing Auditable interface
  @override
  DateTime get createdAt => _createdAt;

  @override
  DateTime get updatedAt => _updatedAt;

  @override
  String get createdBy => _createdBy;

  @override
  void updateAuditInfo(String user) {
    _updatedAt = DateTime.now();
  }

  // Product-specific methods
  double get totalValue => price * quantity;
  bool get isInStock => quantity > 0;
}

// Service class that works with any object implementing these interfaces
class DataService {
  // Method that works with any Serializable object
  static String exportData<T extends Serializable>(List<T> items) {
    return items.map((item) => item.serialize()).join('\n');
  }

  // Method that works with any Validatable object
  static Map<String, List<String>> validateData<T extends Validatable>(List<T> items) {
    Map<String, List<String>> validationResults = {};
    
    for (int i = 0; i < items.length; i++) {
      List<String> errors = items[i].getValidationErrors();
      if (errors.isNotEmpty) {
        validationResults['Item $i'] = errors;
      }
    }
    
    return validationResults;
  }

  // Method that works with any Auditable object
  static List<Map<String, dynamic>> getAuditInfo<T extends Auditable>(List<T> items) {
    return items.map((item) => {
      'createdAt': item.createdAt.toIso8601String(),
      'updatedAt': item.updatedAt.toIso8601String(),
      'createdBy': item.createdBy,
      'type': item.runtimeType.toString(),
    }).toList();
  }

  // Generic method working with objects that implement multiple interfaces
  static List<T> getValidItems<T extends Validatable>(List<T> items) {
    return items.where((item) => item.isValid()).toList();
  }
}

class InterfaceDemo extends StatefulWidget {
  const InterfaceDemo({super.key});

  @override
  State<InterfaceDemo> createState() => _InterfaceDemoState();
}

class _InterfaceDemoState extends State<InterfaceDemo> {
  List<User> users = [];
  List<Product> products = [];
  String _exportData = '';
  Map<String, List<String>> _validationErrors = {};

  @override
  void initState() {
    super.initState();
    
    // Initialize with sample data
    users = [
      User(id: '1', name: 'Alice Johnson', email: 'alice@example.com', age: 28),
      User(id: '2', name: 'Bob Smith', email: 'bob@example.com', age: 35),
      User(id: '3', name: '', email: 'invalid-email', age: -5), // Invalid user
    ];
    
    products = [
      Product(sku: 'LAPTOP001', name: 'Gaming Laptop', price: 1299.99, quantity: 15, category: 'Electronics'),
      Product(sku: 'BOOK001', name: 'Flutter Guide', price: 29.99, quantity: 50, category: 'Books'),
      Product(sku: '', name: '', price: -10, quantity: -5, category: ''), // Invalid product
    ];
  }

  void _exportAllData() {
    // Polymorphic usage - treating different types uniformly
    List<Serializable> allSerializable = [...users, ...products];
    
    setState(() {
      _exportData = DataService.exportData(allSerializable);
    });
  }

  void _validateAllData() {
    List<Validatable> allValidatable = [...users, ...products];
    
    setState(() {
      _validationErrors = DataService.validateData(allValidatable);
    });
  }

  void _updateAuditInfo() {
    List<Auditable> allAuditable = [...users, ...products];
    
    for (var item in allAuditable) {
      item.updateAuditInfo('admin_user');
    }
    
    setState(() {
      // Force rebuild to show updated times
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Interface Implementation'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Action buttons
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Interface Operations',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: _exportAllData,
                          child: const Text('Export All Data'),
                        ),
                        ElevatedButton(
                          onPressed: _validateAllData,
                          child: const Text('Validate All Data'),
                        ),
                        ElevatedButton(
                          onPressed: _updateAuditInfo,
                          child: const Text('Update Audit Info'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Users section
            const Text(
              'Users (Implementing All Interfaces)',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            ...users.map((user) => Card(
              color: user.isValid() ? Colors.green.shade900 : Colors.red.shade900,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      user.name.isEmpty ? 'Unnamed User' : user.name,
                      style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    Text('ID: ${user.id}, Email: ${user.email}, Age: ${user.age}'),
                    Text('Valid: ${user.isValid() ? "Yes" : "No"}'),
                    Text('Created: ${user.createdAt.toString().substring(0, 19)}'),
                    Text('Updated: ${user.updatedAt.toString().substring(0, 19)}'),
                    Text('Created by: ${user.createdBy}'),
                    if (!user.isValid()) ...[
                      const SizedBox(height: 8),
                      const Text('Validation Errors:', style: TextStyle(color: Colors.red)),
                      ...user.getValidationErrors().map((error) => Text('  • $error')),
                    ],
                  ],
                ),
              ),
            )),
            
            const SizedBox(height: 16),
            
            // Products section
            const Text(
              'Products (Implementing All Interfaces)',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            ...products.map((product) => Card(
              color: product.isValid() ? Colors.blue.shade900 : Colors.red.shade900,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      product.name.isEmpty ? 'Unnamed Product' : product.name,
                      style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    Text('SKU: ${product.sku}, Category: ${product.category}'),
                    Text('Price: \$${product.price.toStringAsFixed(2)}, Quantity: ${product.quantity}'),
                    Text('Total Value: \$${product.totalValue.toStringAsFixed(2)}'),
                    Text('Valid: ${product.isValid() ? "Yes" : "No"}'),
                    Text('Created: ${product.createdAt.toString().substring(0, 19)}'),
                    Text('Updated: ${product.updatedAt.toString().substring(0, 19)}'),
                    if (!product.isValid()) ...[
                      const SizedBox(height: 8),
                      const Text('Validation Errors:', style: TextStyle(color: Colors.red)),
                      ...product.getValidationErrors().map((error) => Text('  • $error')),
                    ],
                  ],
                ),
              ),
            )),
            
            const SizedBox(height: 16),
            
            // Export data results
            if (_exportData.isNotEmpty) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Exported Data (Serializable Interface)',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Container(
                        width: double.infinity,
                        padding: const EdgeInsets.all(12),
                        decoration: BoxDecoration(
                          color: Colors.grey.shade900,
                          borderRadius: BorderRadius.circular(8),
                        ),
                        child: Text(
                          _exportData,
                          style: const TextStyle(fontFamily: 'monospace', fontSize: 12),
                        ),
                      ),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],
            
            // Validation errors
            if (_validationErrors.isNotEmpty) ...[
              Card(
                color: Colors.red.shade900,
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Validation Errors (Validatable Interface)',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      ..._validationErrors.entries.map((entry) => Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text('${entry.key}:', style: const TextStyle(fontWeight: FontWeight.bold)),
                          ...entry.value.map((error) => Text('  • $error')),
                          const SizedBox(height: 8),
                        ],
                      )),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],
            
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Interface Implementation Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Multiple interface implementation: User and Product implement 3 interfaces'),
                    const Text('• Interface segregation: Separate interfaces for different concerns'),
                    const Text('• Polymorphism: DataService works with any object implementing interfaces'),
                    const Text('• Generic constraints: <T extends Interface> for type safety'),
                    const Text('• Duck typing: Same interface, different implementations'),
                    const Text('• Contract enforcement: Abstract methods must be implemented'),
                    const Text('• Code reusability: Generic service methods work with any conforming type'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates interface implementation through multiple  
abstract contracts. User and Product classes implement Serializable,  
Validatable, and Auditable interfaces, showing how different types can  
conform to common contracts. The DataService uses generic constraints  
to work polymorphically with any implementing type.

## Polymorphism and Dynamic Dispatch

Demonstrating polymorphic behavior and runtime method resolution.

```dart
import 'package:flutter/material.dart';
import 'dart:math' as math;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Polymorphism and Dynamic Dispatch',
      theme: ThemeData.dark(),
      home: const PolymorphismDemo(),
    );
  }
}

// Base class for different media types
abstract class Media {
  String title;
  String creator;
  DateTime releaseDate;
  double rating;
  
  Media(this.title, this.creator, this.releaseDate, this.rating);
  
  // Abstract methods for polymorphic behavior
  String getMediaType();
  String getDescription();
  Duration getDuration();
  Widget getIcon();
  
  // Virtual method that can be overridden
  String getDisplayTitle() {
    return title;
  }
  
  // Concrete method using template method pattern
  String getFullInfo() {
    return '${getMediaType()}: ${getDisplayTitle()} by $creator (${rating}/5)';
  }
  
  // Polymorphic method for different play behaviors
  String play() {
    return 'Playing ${getMediaType()}: $title';
  }
  
  bool get isHighRated => rating >= 4.0;
  
  String get formattedReleaseDate => 
      '${releaseDate.day}/${releaseDate.month}/${releaseDate.year}';
}

// Movie implementation
class Movie extends Media {
  String director;
  String genre;
  List<String> actors;
  
  Movie({
    required String title,
    required this.director,
    required DateTime releaseDate,
    required double rating,
    required this.genre,
    required this.actors,
  }) : super(title, director, releaseDate, rating);
  
  @override
  String getMediaType() => 'Movie';
  
  @override
  String getDescription() {
    return 'A $genre film directed by $director, starring ${actors.join(", ")}';
  }
  
  @override
  Duration getDuration() {
    // Movies typically 90-180 minutes
    return Duration(minutes: 90 + (title.length * 2));
  }
  
  @override
  Widget getIcon() {
    return const Icon(Icons.movie, color: Colors.red, size: 30);
  }
  
  @override
  String play() {
    return '🎬 ${super.play()} - Now showing in theater mode!';
  }
  
  @override
  String getDisplayTitle() {
    return isHighRated ? '⭐ ${super.getDisplayTitle()}' : super.getDisplayTitle();
  }
}

// Book implementation
class Book extends Media {
  String author;
  String publisher;
  int pages;
  
  Book({
    required String title,
    required this.author,
    required DateTime releaseDate,
    required double rating,
    required this.publisher,
    required this.pages,
  }) : super(title, author, releaseDate, rating);
  
  @override
  String getMediaType() => 'Book';
  
  @override
  String getDescription() {
    return 'A $pages-page book by $author, published by $publisher';
  }
  
  @override
  Duration getDuration() {
    // Estimate reading time: ~250 words per minute, ~250 words per page
    return Duration(minutes: pages);
  }
  
  @override
  Widget getIcon() {
    return const Icon(Icons.menu_book, color: Colors.green, size: 30);
  }
  
  @override
  String play() {
    return '📖 ${super.play()} - Opening e-reader mode!';
  }
}

// Music implementation
class Song extends Media {
  String artist;
  String album;
  String genre;
  
  Song({
    required String title,
    required this.artist,
    required DateTime releaseDate,
    required double rating,
    required this.album,
    required this.genre,
  }) : super(title, artist, releaseDate, rating);
  
  @override
  String getMediaType() => 'Song';
  
  @override
  String getDescription() {
    return 'A $genre track by $artist from the album "$album"';
  }
  
  @override
  Duration getDuration() {
    // Songs typically 3-5 minutes
    return Duration(minutes: 3, seconds: (title.length * 5) % 60);
  }
  
  @override
  Widget getIcon() {
    return const Icon(Icons.music_note, color: Colors.blue, size: 30);
  }
  
  @override
  String play() {
    return '🎵 ${super.play()} - Now playing with enhanced audio!';
  }
  
  @override
  String getDisplayTitle() {
    return '$title ${isHighRated ? "🎵" : "♪"}';
  }
}

// Podcast implementation
class Podcast extends Media {
  String host;
  int episodeNumber;
  String category;
  
  Podcast({
    required String title,
    required this.host,
    required DateTime releaseDate,
    required double rating,
    required this.episodeNumber,
    required this.category,
  }) : super(title, host, releaseDate, rating);
  
  @override
  String getMediaType() => 'Podcast';
  
  @override
  String getDescription() {
    return 'Episode $episodeNumber: $title, a $category podcast hosted by $host';
  }
  
  @override
  Duration getDuration() {
    // Podcasts vary widely, 20-90 minutes
    return Duration(minutes: 30 + (episodeNumber * 2) % 60);
  }
  
  @override
  Widget getIcon() {
    return const Icon(Icons.mic, color: Colors.orange, size: 30);
  }
  
  @override
  String play() {
    return '🎙️ ${super.play()} - Streaming episode $episodeNumber!';
  }
  
  @override
  String getDisplayTitle() {
    return 'Ep. $episodeNumber: ${super.getDisplayTitle()}';
  }
}

// MediaPlayer class demonstrating polymorphic usage
class MediaPlayer {
  List<Media> playlist = [];
  int currentIndex = 0;
  
  void addMedia(Media media) {
    playlist.add(media);
  }
  
  void removeMedia(int index) {
    if (index >= 0 && index < playlist.length) {
      playlist.removeAt(index);
      if (currentIndex >= playlist.length && playlist.isNotEmpty) {
        currentIndex = playlist.length - 1;
      }
    }
  }
  
  String playCurrentMedia() {
    if (playlist.isEmpty) return 'No media in playlist';
    return playlist[currentIndex].play();
  }
  
  Media? getCurrentMedia() {
    return playlist.isEmpty ? null : playlist[currentIndex];
  }
  
  void next() {
    if (playlist.isNotEmpty) {
      currentIndex = (currentIndex + 1) % playlist.length;
    }
  }
  
  void previous() {
    if (playlist.isNotEmpty) {
      currentIndex = currentIndex > 0 ? currentIndex - 1 : playlist.length - 1;
    }
  }
  
  // Polymorphic operations
  Duration getTotalDuration() {
    Duration total = Duration.zero;
    for (Media media in playlist) {
      total += media.getDuration();
    }
    return total;
  }
  
  List<Media> getHighRatedMedia() {
    return playlist.where((media) => media.isHighRated).toList();
  }
  
  List<Media> getMediaByType(Type type) {
    return playlist.where((media) => media.runtimeType == type).toList();
  }
  
  // Demonstrate late binding / dynamic dispatch
  List<String> getAllDescriptions() {
    return playlist.map((media) => media.getDescription()).toList();
  }
}

class PolymorphismDemo extends StatefulWidget {
  const PolymorphismDemo({super.key});

  @override
  State<PolymorphismDemo> createState() => _PolymorphismDemoState();
}

class _PolymorphismDemoState extends State<PolymorphismDemo> {
  MediaPlayer player = MediaPlayer();
  String _playMessage = '';

  @override
  void initState() {
    super.initState();
    
    // Initialize with diverse media types
    player.addMedia(Movie(
      title: 'The Matrix',
      director: 'Wachowski Sisters',
      releaseDate: DateTime(1999, 3, 31),
      rating: 4.8,
      genre: 'Sci-Fi',
      actors: ['Keanu Reeves', 'Laurence Fishburne', 'Carrie-Anne Moss'],
    ));
    
    player.addMedia(Book(
      title: 'Clean Code',
      author: 'Robert Martin',
      releaseDate: DateTime(2008, 8, 1),
      rating: 4.5,
      publisher: 'Prentice Hall',
      pages: 464,
    ));
    
    player.addMedia(Song(
      title: 'Bohemian Rhapsody',
      artist: 'Queen',
      releaseDate: DateTime(1975, 10, 31),
      rating: 4.9,
      album: 'A Night at the Opera',
      genre: 'Rock',
    ));
    
    player.addMedia(Podcast(
      title: 'The Future of AI',
      host: 'Tech Insights',
      releaseDate: DateTime(2024, 1, 15),
      rating: 4.2,
      episodeNumber: 42,
      category: 'Technology',
    ));
    
    player.addMedia(Movie(
      title: 'Inception',
      director: 'Christopher Nolan',
      releaseDate: DateTime(2010, 7, 16),
      rating: 4.7,
      genre: 'Thriller',
      actors: ['Leonardo DiCaprio', 'Marion Cotillard'],
    ));
  }

  void _playCurrentMedia() {
    setState(() {
      _playMessage = player.playCurrentMedia();
    });
  }

  String _formatDuration(Duration duration) {
    int hours = duration.inHours;
    int minutes = duration.inMinutes % 60;
    if (hours > 0) {
      return '${hours}h ${minutes}m';
    }
    return '${minutes}m';
  }

  @override
  Widget build(BuildContext context) {
    final currentMedia = player.getCurrentMedia();
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Polymorphism and Dynamic Dispatch'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Media Player Controls
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Media Player (Polymorphic Behavior)',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    if (currentMedia != null) ...[
                      Row(
                        children: [
                          currentMedia.getIcon(),
                          const SizedBox(width: 12),
                          Expanded(
                            child: Column(
                              crossAxisAlignment: CrossAxisAlignment.start,
                              children: [
                                Text(
                                  currentMedia.getDisplayTitle(),
                                  style: const TextStyle(
                                    fontSize: 16,
                                    fontWeight: FontWeight.bold,
                                  ),
                                ),
                                Text(currentMedia.creator),
                                Text(currentMedia.getDescription()),
                                Text('Duration: ${_formatDuration(currentMedia.getDuration())}'),
                                Text('Rating: ${currentMedia.rating}/5 ⭐'),
                              ],
                            ),
                          ),
                        ],
                      ),
                      const SizedBox(height: 16),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                        children: [
                          IconButton(
                            onPressed: () {
                              setState(() {
                                player.previous();
                              });
                            },
                            icon: const Icon(Icons.skip_previous),
                            iconSize: 32,
                          ),
                          ElevatedButton.icon(
                            onPressed: _playCurrentMedia,
                            icon: const Icon(Icons.play_arrow),
                            label: const Text('Play'),
                          ),
                          IconButton(
                            onPressed: () {
                              setState(() {
                                player.next();
                              });
                            },
                            icon: const Icon(Icons.skip_next),
                            iconSize: 32,
                          ),
                        ],
                      ),
                      if (_playMessage.isNotEmpty) ...[
                        const SizedBox(height: 12),
                        Container(
                          width: double.infinity,
                          padding: const EdgeInsets.all(12),
                          decoration: BoxDecoration(
                            color: Colors.green.shade800,
                            borderRadius: BorderRadius.circular(8),
                          ),
                          child: Text(_playMessage),
                        ),
                      ],
                    ] else ...[
                      const Text('No media in playlist'),
                    ],
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Playlist Statistics
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Playlist Statistics (Polymorphic Operations)',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Total Items: ${player.playlist.length}'),
                    Text('Total Duration: ${_formatDuration(player.getTotalDuration())}'),
                    Text('High Rated Items: ${player.getHighRatedMedia().length}'),
                    Text('Movies: ${player.getMediaByType(Movie).length}'),
                    Text('Books: ${player.getMediaByType(Book).length}'),
                    Text('Songs: ${player.getMediaByType(Song).length}'),
                    Text('Podcasts: ${player.getMediaByType(Podcast).length}'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Complete Playlist
            const Text(
              'Complete Playlist (Different Types, Same Interface)',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            ...player.playlist.asMap().entries.map((entry) {
              int index = entry.key;
              Media media = entry.value;
              bool isCurrent = index == player.currentIndex;
              
              return Card(
                color: isCurrent ? Colors.blue.shade800 : null,
                margin: const EdgeInsets.only(bottom: 8),
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Row(
                    children: [
                      media.getIcon(),
                      const SizedBox(width: 12),
                      Expanded(
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(
                              media.getFullInfo(),
                              style: TextStyle(
                                fontWeight: isCurrent ? FontWeight.bold : FontWeight.normal,
                              ),
                            ),
                            Text(
                              media.getDescription(),
                              style: const TextStyle(fontSize: 12, color: Colors.grey),
                            ),
                            Text(
                              'Duration: ${_formatDuration(media.getDuration())} • Released: ${media.formattedReleaseDate}',
                              style: const TextStyle(fontSize: 11, color: Colors.grey),
                            ),
                          ],
                        ),
                      ),
                      if (isCurrent)
                        const Icon(Icons.volume_up, color: Colors.blue),
                      IconButton(
                        onPressed: () {
                          setState(() {
                            player.removeMedia(index);
                          });
                        },
                        icon: const Icon(Icons.remove_circle, color: Colors.red),
                      ),
                    ],
                  ),
                ),
              );
            }),
            
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Polymorphism Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Runtime polymorphism: Same method calls, different implementations'),
                    const Text('• Dynamic dispatch: Method resolution at runtime based on actual type'),
                    const Text('• Interface uniformity: List<Media> contains different concrete types'),
                    const Text('• Method overriding: Each type provides specialized behavior'),
                    const Text('• Late binding: Actual method called determined at runtime'),
                    const Text('• Template method pattern: Base class orchestrates polymorphic calls'),
                    const Text('• Type checking: runtimeType and whereType<T>() for filtering'),
                    const Text('• Liskov substitution: Derived types substitutable for base type'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates polymorphism and dynamic dispatch through a  
media player system. Different media types (Movie, Book, Song, Podcast)  
share a common interface while providing specialized implementations.  
The MediaPlayer treats all types uniformly, with method resolution  
occurring at runtime based on the actual object type.

## Mixins and Multiple Inheritance

Implementing code reuse through mixins and composition patterns.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Mixins and Multiple Inheritance',
      theme: ThemeData.dark(),
      home: const MixinsDemo(),
    );
  }
}

// Mixin for logging capabilities
mixin Loggable {
  final List<String> _logs = [];
  
  void log(String message) {
    final timestamp = DateTime.now().toString().substring(0, 19);
    _logs.add('[$timestamp] $message');
    print('LOG: $message');
  }
  
  List<String> getLogs() => List.unmodifiable(_logs);
  
  void clearLogs() => _logs.clear();
  
  String getLastLog() => _logs.isEmpty ? 'No logs' : _logs.last;
}

// Mixin for validation capabilities
mixin Validatable {
  List<String> _validationRules = [];
  
  void addValidationRule(String rule) {
    _validationRules.add(rule);
  }
  
  List<String> getValidationRules() => List.unmodifiable(_validationRules);
  
  // Abstract-like method that must be implemented
  bool validate();
  
  List<String> getValidationErrors() {
    List<String> errors = [];
    if (!validate()) {
      errors.add('Validation failed');
    }
    return errors;
  }
}

// Mixin for caching capabilities
mixin Cacheable<T> {
  final Map<String, T> _cache = {};
  int _maxCacheSize = 10;
  
  void setMaxCacheSize(int size) {
    _maxCacheSize = size;
    if (_cache.length > _maxCacheSize) {
      // Remove oldest entries
      final entries = _cache.entries.take(_cache.length - _maxCacheSize);
      for (var entry in entries) {
        _cache.remove(entry.key);
      }
    }
  }
  
  void cacheValue(String key, T value) {
    if (_cache.length >= _maxCacheSize) {
      // Remove oldest entry
      final firstKey = _cache.keys.first;
      _cache.remove(firstKey);
    }
    _cache[key] = value;
  }
  
  T? getCachedValue(String key) {
    return _cache[key];
  }
  
  bool isCached(String key) => _cache.containsKey(key);
  
  void clearCache() => _cache.clear();
  
  int getCacheSize() => _cache.length;
  
  List<String> getCacheKeys() => List.unmodifiable(_cache.keys);
}

// Mixin for serialization
mixin Serializable {
  Map<String, dynamic> toJson();
  void fromJson(Map<String, dynamic> json);
  
  String serialize() {
    try {
      return toJson().toString();
    } catch (e) {
      return 'Serialization failed: $e';
    }
  }
}

// Class using multiple mixins
class SmartUser with Loggable, Validatable, Cacheable<String>, Serializable {
  String id;
  String name;
  String email;
  int age;
  Map<String, dynamic> preferences;
  
  SmartUser({
    required this.id,
    required this.name,
    required this.email,
    required this.age,
    Map<String, dynamic>? preferences,
  }) : preferences = preferences ?? {} {
    log('SmartUser created: $name');
    addValidationRule('Name must not be empty');
    addValidationRule('Email must contain @');
    addValidationRule('Age must be positive');
    setMaxCacheSize(5);
  }
  
  // Implementation for Validatable mixin
  @override
  bool validate() {
    if (name.isEmpty) {
      log('Validation failed: Name is empty');
      return false;
    }
    if (!email.contains('@')) {
      log('Validation failed: Invalid email format');
      return false;
    }
    if (age <= 0) {
      log('Validation failed: Invalid age');
      return false;
    }
    log('Validation passed for user: $name');
    return true;
  }
  
  // Implementation for Serializable mixin
  @override
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'email': email,
      'age': age,
      'preferences': preferences,
    };
  }
  
  @override
  void fromJson(Map<String, dynamic> json) {
    id = json['id'] ?? '';
    name = json['name'] ?? '';
    email = json['email'] ?? '';
    age = json['age'] ?? 0;
    preferences = json['preferences'] ?? {};
    log('User data loaded from JSON');
  }
  
  // Using cache for expensive operations
  String getDisplayName() {
    const key = 'display_name';
    
    if (isCached(key)) {
      log('Retrieved display name from cache');
      return getCachedValue(key)!;
    }
    
    // Simulate expensive operation
    final displayName = '$name (${email.split('@')[0]})';
    cacheValue(key, displayName);
    log('Computed and cached display name');
    
    return displayName;
  }
  
  void updateProfile({String? newName, String? newEmail, int? newAge}) {
    if (newName != null) {
      name = newName;
      log('Name updated to: $newName');
    }
    if (newEmail != null) {
      email = newEmail;
      log('Email updated to: $newEmail');
    }
    if (newAge != null) {
      age = newAge;
      log('Age updated to: $newAge');
    }
    
    // Clear cache when profile changes
    clearCache();
    log('Cache cleared due to profile update');
  }
  
  void setPreference(String key, dynamic value) {
    preferences[key] = value;
    log('Preference set: $key = $value');
  }
  
  T? getPreference<T>(String key) {
    return preferences[key] as T?;
  }
}

// Another class demonstrating different mixin combinations
class SmartProduct with Loggable, Cacheable<double> {
  String sku;
  String name;
  double basePrice;
  Map<String, double> regionPrices;
  
  SmartProduct({
    required this.sku,
    required this.name,
    required this.basePrice,
    Map<String, double>? regionPrices,
  }) : regionPrices = regionPrices ?? {} {
    log('SmartProduct created: $name ($sku)');
    setMaxCacheSize(3);
  }
  
  double getPriceForRegion(String region) {
    final key = 'price_$region';
    
    if (isCached(key)) {
      log('Retrieved $region price from cache');
      return getCachedValue(key)!;
    }
    
    // Calculate price with region-specific adjustments
    double price = regionPrices[region] ?? basePrice;
    
    // Simulate complex pricing calculation
    if (region == 'premium') {
      price *= 1.2;
    } else if (region == 'discount') {
      price *= 0.8;
    }
    
    cacheValue(key, price);
    log('Calculated and cached $region price: \$${price.toStringAsFixed(2)}');
    
    return price;
  }
  
  void updateRegionPrice(String region, double price) {
    regionPrices[region] = price;
    clearCache(); // Invalidate cache
    log('Updated $region price to \$${price.toStringAsFixed(2)}');
  }
}

// Class showing mixin ordering and method resolution
class AdvancedLogger with Loggable {
  String name;
  
  AdvancedLogger(this.name) {
    log('AdvancedLogger initialized: $name');
  }
  
  // Override mixin method to add custom behavior
  @override
  void log(String message) {
    super.log('[$name] $message');
    // Could add additional logging behavior here
  }
  
  void performOperation(String operation) {
    log('Starting operation: $operation');
    
    // Simulate some work
    for (int i = 0; i < 3; i++) {
      log('Step ${i + 1} of $operation');
    }
    
    log('Completed operation: $operation');
  }
}

class MixinsDemo extends StatefulWidget {
  const MixinsDemo({super.key});

  @override
  State<MixinsDemo> createState() => _MixinsDemoState();
}

class _MixinsDemoState extends State<MixinsDemo> {
  late SmartUser user;
  late SmartProduct product;
  late AdvancedLogger logger;
  String _operationResult = '';

  @override
  void initState() {
    super.initState();
    
    user = SmartUser(
      id: '1',
      name: 'Alice Johnson',
      email: 'alice@example.com',
      age: 28,
      preferences: {'theme': 'dark', 'language': 'en'},
    );
    
    product = SmartProduct(
      sku: 'LAPTOP001',
      name: 'Gaming Laptop',
      basePrice: 1299.99,
      regionPrices: {'us': 1299.99, 'eu': 1199.99, 'asia': 999.99},
    );
    
    logger = AdvancedLogger('SystemLogger');
  }

  void _performUserOperation(String operation) {
    setState(() {
      switch (operation) {
        case 'validate':
          bool isValid = user.validate();
          _operationResult = 'User validation: ${isValid ? "Passed" : "Failed"}';
          break;
        case 'serialize':
          String serialized = user.serialize();
          _operationResult = 'Serialized: $serialized';
          break;
        case 'cache_display_name':
          String displayName = user.getDisplayName();
          _operationResult = 'Display name: $displayName';
          break;
        case 'update_profile':
          user.updateProfile(newName: 'Alice Smith', newAge: 29);
          _operationResult = 'Profile updated successfully';
          break;
      }
    });
  }

  void _performProductOperation(String region) {
    setState(() {
      double price = product.getPriceForRegion(region);
      _operationResult = 'Price for $region: \$${price.toStringAsFixed(2)}';
    });
  }

  void _performLoggerOperation() {
    setState(() {
      logger.performOperation('Data Processing');
      _operationResult = 'Logger operation completed - check logs';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Mixins and Multiple Inheritance'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Operation Result
            if (_operationResult.isNotEmpty) ...[
              Card(
                color: Colors.blue.shade800,
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Operation Result',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Text(_operationResult),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],
            
            // SmartUser Demo
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'SmartUser (Multiple Mixins)',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Name: ${user.name}'),
                    Text('Email: ${user.email}'),
                    Text('Age: ${user.age}'),
                    Text('Cache Size: ${user.getCacheSize()}/${5}'),
                    Text('Validation Rules: ${user.getValidationRules().length}'),
                    Text('Logs: ${user.getLogs().length}'),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performUserOperation('validate'),
                          child: const Text('Validate'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performUserOperation('serialize'),
                          child: const Text('Serialize'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performUserOperation('cache_display_name'),
                          child: const Text('Get Display Name'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performUserOperation('update_profile'),
                          child: const Text('Update Profile'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // SmartProduct Demo
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'SmartProduct (Selective Mixins)',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('SKU: ${product.sku}'),
                    Text('Name: ${product.name}'),
                    Text('Base Price: \$${product.basePrice.toStringAsFixed(2)}'),
                    Text('Cache Size: ${product.getCacheSize()}/${3}'),
                    Text('Logs: ${product.getLogs().length}'),
                    const SizedBox(height: 12),
                    const Text('Get Regional Pricing:'),
                    const SizedBox(height: 8),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performProductOperation('us'),
                          child: const Text('US Price'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performProductOperation('eu'),
                          child: const Text('EU Price'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performProductOperation('premium'),
                          child: const Text('Premium Price'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performProductOperation('discount'),
                          child: const Text('Discount Price'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // AdvancedLogger Demo
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'AdvancedLogger (Mixin Override)',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Logger Name: ${logger.name}'),
                    Text('Logs: ${logger.getLogs().length}'),
                    const SizedBox(height: 12),
                    ElevatedButton(
                      onPressed: _performLoggerOperation,
                      child: const Text('Perform Logged Operation'),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Recent Logs Display
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Recent Logs (From All Objects)',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    ExpansionTile(
                      title: Text('User Logs (${user.getLogs().length})'),
                      children: user.getLogs().take(5).map((log) => 
                        Padding(
                          padding: const EdgeInsets.symmetric(vertical: 2, horizontal: 16),
                          child: Text(log, style: const TextStyle(fontSize: 12)),
                        ),
                      ).toList(),
                    ),
                    ExpansionTile(
                      title: Text('Product Logs (${product.getLogs().length})'),
                      children: product.getLogs().take(5).map((log) => 
                        Padding(
                          padding: const EdgeInsets.symmetric(vertical: 2, horizontal: 16),
                          child: Text(log, style: const TextStyle(fontSize: 12)),
                        ),
                      ).toList(),
                    ),
                    ExpansionTile(
                      title: Text('Logger Logs (${logger.getLogs().length})'),
                      children: logger.getLogs().take(5).map((log) => 
                        Padding(
                          padding: const EdgeInsets.symmetric(vertical: 2, horizontal: 16),
                          child: Text(log, style: const TextStyle(fontSize: 12)),
                        ),
                      ).toList(),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Mixins Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Mixin declaration: mixin Loggable, mixin Validatable'),
                    const Text('• Multiple mixin usage: class SmartUser with Loggable, Validatable'),
                    const Text('• Mixin constraints: Cacheable<T> with generic type parameter'),
                    const Text('• Method override in mixins: AdvancedLogger overrides log()'),
                    const Text('• Shared functionality: Logging, validation, caching behavior'),
                    const Text('• Selective mixin application: Different classes use different combinations'),
                    const Text('• Linear composition: Dart resolves method conflicts using linearization'),
                    const Text('• Code reuse: Same mixin functionality across multiple classes'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates mixins and multiple inheritance patterns in  
Dart. Mixins provide code reuse without traditional inheritance  
limitations, enabling composition of functionality. SmartUser combines  
logging, validation, caching, and serialization capabilities through  
multiple mixins, while other classes use selective combinations.

## Factory Constructors and Design Patterns

Implementing factory patterns and advanced constructor techniques.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Factory Constructors and Design Patterns',
      theme: ThemeData.dark(),
      home: const FactoryDemo(),
    );
  }
}

// Abstract base class for database connections
abstract class DatabaseConnection {
  String host;
  int port;
  String database;
  bool isConnected;
  
  DatabaseConnection(this.host, this.port, this.database) : isConnected = false;
  
  // Factory constructor that creates appropriate implementation
  factory DatabaseConnection.create(String type, String host, int port, String database) {
    switch (type.toLowerCase()) {
      case 'mysql':
        return MySQLConnection(host, port, database);
      case 'postgresql':
        return PostgreSQLConnection(host, port, database);
      case 'mongodb':
        return MongoDBConnection(host, port, database);
      case 'redis':
        return RedisConnection(host, port, database);
      default:
        throw ArgumentError('Unsupported database type: $type');
    }
  }
  
  // Factory constructor with predefined configurations
  factory DatabaseConnection.development(String type) {
    return DatabaseConnection.create(type, 'localhost', _getDefaultPort(type), 'dev_db');
  }
  
  factory DatabaseConnection.production(String type, String host) {
    return DatabaseConnection.create(type, host, _getDefaultPort(type), 'prod_db');
  }
  
  static int _getDefaultPort(String type) {
    switch (type.toLowerCase()) {
      case 'mysql': return 3306;
      case 'postgresql': return 5432;
      case 'mongodb': return 27017;
      case 'redis': return 6379;
      default: return 8080;
    }
  }
  
  // Abstract methods that must be implemented
  Future<bool> connect();
  Future<void> disconnect();
  Future<Map<String, dynamic>> query(String sql);
  String getConnectionString();
  String getDatabaseType();
}

// Concrete MySQL implementation
class MySQLConnection extends DatabaseConnection {
  String username;
  String password;
  
  MySQLConnection(String host, int port, String database, [this.username = 'root', this.password = '']) 
      : super(host, port, database);
  
  @override
  Future<bool> connect() async {
    await Future.delayed(const Duration(milliseconds: 500));
    isConnected = true;
    return true;
  }
  
  @override
  Future<void> disconnect() async {
    await Future.delayed(const Duration(milliseconds: 200));
    isConnected = false;
  }
  
  @override
  Future<Map<String, dynamic>> query(String sql) async {
    if (!isConnected) throw StateError('Not connected to database');
    
    await Future.delayed(const Duration(milliseconds: 300));
    return {
      'type': 'mysql',
      'query': sql,
      'rows_affected': 5,
      'execution_time_ms': 150,
    };
  }
  
  @override
  String getConnectionString() {
    return 'mysql://$username:***@$host:$port/$database';
  }
  
  @override
  String getDatabaseType() => 'MySQL';
}

// Concrete PostgreSQL implementation
class PostgreSQLConnection extends DatabaseConnection {
  String schema;
  
  PostgreSQLConnection(String host, int port, String database, [this.schema = 'public']) 
      : super(host, port, database);
  
  @override
  Future<bool> connect() async {
    await Future.delayed(const Duration(milliseconds: 600));
    isConnected = true;
    return true;
  }
  
  @override
  Future<void> disconnect() async {
    await Future.delayed(const Duration(milliseconds: 300));
    isConnected = false;
  }
  
  @override
  Future<Map<String, dynamic>> query(String sql) async {
    if (!isConnected) throw StateError('Not connected to database');
    
    await Future.delayed(const Duration(milliseconds: 250));
    return {
      'type': 'postgresql',
      'query': sql,
      'rows_affected': 3,
      'schema': schema,
      'execution_time_ms': 120,
    };
  }
  
  @override
  String getConnectionString() {
    return 'postgresql://user:***@$host:$port/$database?schema=$schema';
  }
  
  @override
  String getDatabaseType() => 'PostgreSQL';
}

// Concrete MongoDB implementation
class MongoDBConnection extends DatabaseConnection {
  String collection;
  
  MongoDBConnection(String host, int port, String database, [this.collection = 'default']) 
      : super(host, port, database);
  
  @override
  Future<bool> connect() async {
    await Future.delayed(const Duration(milliseconds: 400));
    isConnected = true;
    return true;
  }
  
  @override
  Future<void> disconnect() async {
    await Future.delayed(const Duration(milliseconds: 250));
    isConnected = false;
  }
  
  @override
  Future<Map<String, dynamic>> query(String sql) async {
    if (!isConnected) throw StateError('Not connected to database');
    
    await Future.delayed(const Duration(milliseconds: 200));
    return {
      'type': 'mongodb',
      'query': sql,
      'documents_found': 12,
      'collection': collection,
      'execution_time_ms': 80,
    };
  }
  
  @override
  String getConnectionString() {
    return 'mongodb://$host:$port/$database/$collection';
  }
  
  @override
  String getDatabaseType() => 'MongoDB';
}

// Concrete Redis implementation
class RedisConnection extends DatabaseConnection {
  int selectedDatabase;
  
  RedisConnection(String host, int port, String database, [this.selectedDatabase = 0]) 
      : super(host, port, database);
  
  @override
  Future<bool> connect() async {
    await Future.delayed(const Duration(milliseconds: 300));
    isConnected = true;
    return true;
  }
  
  @override
  Future<void> disconnect() async {
    await Future.delayed(const Duration(milliseconds: 100));
    isConnected = false;
  }
  
  @override
  Future<Map<String, dynamic>> query(String sql) async {
    if (!isConnected) throw StateError('Not connected to database');
    
    await Future.delayed(const Duration(milliseconds: 100));
    return {
      'type': 'redis',
      'command': sql,
      'keys_affected': 1,
      'database': selectedDatabase,
      'execution_time_ms': 50,
    };
  }
  
  @override
  String getConnectionString() {
    return 'redis://$host:$port/$selectedDatabase';
  }
  
  @override
  String getDatabaseType() => 'Redis';
}

// Singleton pattern for connection pool manager
class ConnectionPoolManager {
  static ConnectionPoolManager? _instance;
  final Map<String, DatabaseConnection> _connections = {};
  int _maxConnections = 5;
  
  // Private constructor for singleton
  ConnectionPoolManager._internal();
  
  // Factory constructor returns singleton instance
  factory ConnectionPoolManager() {
    _instance ??= ConnectionPoolManager._internal();
    return _instance!;
  }
  
  // Factory constructor with configuration
  factory ConnectionPoolManager.withConfig(int maxConnections) {
    final instance = ConnectionPoolManager();
    instance._maxConnections = maxConnections;
    return instance;
  }
  
  Future<DatabaseConnection> getConnection(String type, String environment) async {
    final key = '${type}_$environment';
    
    if (_connections.containsKey(key)) {
      return _connections[key]!;
    }
    
    if (_connections.length >= _maxConnections) {
      throw StateError('Maximum connections reached');
    }
    
    DatabaseConnection connection;
    if (environment == 'development') {
      connection = DatabaseConnection.development(type);
    } else {
      connection = DatabaseConnection.production(type, 'prod-server.example.com');
    }
    
    await connection.connect();
    _connections[key] = connection;
    
    return connection;
  }
  
  Future<void> closeConnection(String type, String environment) async {
    final key = '${type}_$environment';
    final connection = _connections[key];
    
    if (connection != null) {
      await connection.disconnect();
      _connections.remove(key);
    }
  }
  
  Future<void> closeAllConnections() async {
    for (var connection in _connections.values) {
      await connection.disconnect();
    }
    _connections.clear();
  }
  
  List<String> getActiveConnections() {
    return _connections.keys.toList();
  }
  
  int get activeConnectionCount => _connections.length;
  int get maxConnections => _maxConnections;
}

// Builder pattern for complex query construction
class QueryBuilder {
  String _table = '';
  List<String> _selectFields = [];
  List<String> _whereConditions = [];
  List<String> _joins = [];
  String _orderBy = '';
  int? _limit;
  
  // Factory constructors for different query types
  factory QueryBuilder.select(String table) {
    final builder = QueryBuilder();
    builder._table = table;
    return builder;
  }
  
  factory QueryBuilder.insert(String table) {
    final builder = QueryBuilder();
    builder._table = table;
    return builder;
  }
  
  QueryBuilder fields(List<String> fields) {
    _selectFields = fields;
    return this;
  }
  
  QueryBuilder where(String condition) {
    _whereConditions.add(condition);
    return this;
  }
  
  QueryBuilder join(String table, String condition) {
    _joins.add('JOIN $table ON $condition');
    return this;
  }
  
  QueryBuilder orderBy(String field, [String direction = 'ASC']) {
    _orderBy = 'ORDER BY $field $direction';
    return this;
  }
  
  QueryBuilder limit(int count) {
    _limit = count;
    return this;
  }
  
  String build() {
    final query = StringBuffer();
    
    if (_selectFields.isEmpty) {
      query.write('SELECT * FROM $_table');
    } else {
      query.write('SELECT ${_selectFields.join(', ')} FROM $_table');
    }
    
    for (var join in _joins) {
      query.write(' $join');
    }
    
    if (_whereConditions.isNotEmpty) {
      query.write(' WHERE ${_whereConditions.join(' AND ')}');
    }
    
    if (_orderBy.isNotEmpty) {
      query.write(' $_orderBy');
    }
    
    if (_limit != null) {
      query.write(' LIMIT $_limit');
    }
    
    return query.toString();
  }
}

class FactoryDemo extends StatefulWidget {
  const FactoryDemo({super.key});

  @override
  State<FactoryDemo> createState() => _FactoryDemoState();
}

class _FactoryDemoState extends State<FactoryDemo> {
  final ConnectionPoolManager _poolManager = ConnectionPoolManager.withConfig(3);
  List<String> _operations = [];
  String _queryResult = '';
  final List<String> _databases = ['MySQL', 'PostgreSQL', 'MongoDB', 'Redis'];
  String _selectedDatabase = 'MySQL';
  String _selectedEnvironment = 'development';

  void _addOperation(String operation) {
    setState(() {
      _operations.insert(0, '${DateTime.now().toString().substring(11, 19)}: $operation');
      if (_operations.length > 10) {
        _operations.removeLast();
      }
    });
  }

  Future<void> _connectToDatabase() async {
    try {
      final connection = await _poolManager.getConnection(_selectedDatabase, _selectedEnvironment);
      _addOperation('Connected to ${connection.getDatabaseType()} (${connection.getConnectionString()})');
    } catch (e) {
      _addOperation('Connection failed: $e');
    }
  }

  Future<void> _executeQuery() async {
    try {
      final connection = await _poolManager.getConnection(_selectedDatabase, _selectedEnvironment);
      
      // Build a sample query
      final query = QueryBuilder.select('users')
          .fields(['id', 'name', 'email'])
          .where('age > 18')
          .orderBy('name')
          .limit(10)
          .build();
      
      final result = await connection.query(query);
      
      setState(() {
        _queryResult = result.toString();
      });
      
      _addOperation('Executed query on ${connection.getDatabaseType()}');
    } catch (e) {
      _addOperation('Query failed: $e');
      setState(() {
        _queryResult = 'Error: $e';
      });
    }
  }

  Future<void> _closeConnection() async {
    try {
      await _poolManager.closeConnection(_selectedDatabase, _selectedEnvironment);
      _addOperation('Closed connection to $_selectedDatabase ($_selectedEnvironment)');
    } catch (e) {
      _addOperation('Close failed: $e');
    }
  }

  Future<void> _closeAllConnections() async {
    try {
      await _poolManager.closeAllConnections();
      _addOperation('Closed all connections');
    } catch (e) {
      _addOperation('Close all failed: $e');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Factory Constructors and Design Patterns'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Database Selection
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Factory Constructor Demo',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        const Text('Database: '),
                        DropdownButton<String>(
                          value: _selectedDatabase,
                          items: _databases.map((db) => DropdownMenuItem(
                            value: db,
                            child: Text(db),
                          )).toList(),
                          onChanged: (value) {
                            setState(() {
                              _selectedDatabase = value!;
                            });
                          },
                        ),
                        const SizedBox(width: 20),
                        const Text('Environment: '),
                        DropdownButton<String>(
                          value: _selectedEnvironment,
                          items: ['development', 'production'].map((env) => DropdownMenuItem(
                            value: env,
                            child: Text(env),
                          )).toList(),
                          onChanged: (value) {
                            setState(() {
                              _selectedEnvironment = value!;
                            });
                          },
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Connection Operations
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Connection Pool Manager (Singleton)',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Active Connections: ${_poolManager.activeConnectionCount}/${_poolManager.maxConnections}'),
                    if (_poolManager.getActiveConnections().isNotEmpty) ...[
                      const SizedBox(height: 8),
                      const Text('Active:', style: TextStyle(fontWeight: FontWeight.bold)),
                      ..._poolManager.getActiveConnections().map((conn) => Text('  • $conn')),
                    ],
                    const SizedBox(height: 16),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: _connectToDatabase,
                          child: const Text('Connect'),
                        ),
                        ElevatedButton(
                          onPressed: _executeQuery,
                          child: const Text('Execute Query'),
                        ),
                        ElevatedButton(
                          onPressed: _closeConnection,
                          child: const Text('Close Connection'),
                        ),
                        ElevatedButton(
                          onPressed: _closeAllConnections,
                          child: const Text('Close All'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            // Query Result
            if (_queryResult.isNotEmpty) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Query Result (Builder Pattern)',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Container(
                        width: double.infinity,
                        padding: const EdgeInsets.all(12),
                        decoration: BoxDecoration(
                          color: Colors.grey.shade900,
                          borderRadius: BorderRadius.circular(8),
                        ),
                        child: Text(
                          _queryResult,
                          style: const TextStyle(fontFamily: 'monospace', fontSize: 12),
                        ),
                      ),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],
            
            // Operations Log
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Operations Log',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Container(
                      height: 200,
                      width: double.infinity,
                      padding: const EdgeInsets.all(12),
                      decoration: BoxDecoration(
                        color: Colors.grey.shade900,
                        borderRadius: BorderRadius.circular(8),
                        border: Border.all(color: Colors.grey.shade700),
                      ),
                      child: SingleChildScrollView(
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: _operations.map((op) => Padding(
                            padding: const EdgeInsets.only(bottom: 4),
                            child: Text(
                              op,
                              style: const TextStyle(fontSize: 12, fontFamily: 'monospace'),
                            ),
                          )).toList(),
                        ),
                      ),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Design Patterns Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Factory Constructor: DatabaseConnection.create() creates specific types'),
                    const Text('• Named Constructors: .development(), .production() configurations'),
                    const Text('• Singleton Pattern: ConnectionPoolManager single instance'),
                    const Text('• Builder Pattern: QueryBuilder fluent interface for SQL construction'),
                    const Text('• Polymorphism: Different database types through common interface'),
                    const Text('• Abstract Factory: Factory methods return abstract base type'),
                    const Text('• Object Pool: ConnectionPoolManager manages connection reuse'),
                    const Text('• Strategy Pattern: Different database implementations'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates factory constructors and design patterns  
through database connection management. Factory constructors create  
appropriate implementations based on parameters, while the singleton  
ConnectionPoolManager manages instances. The QueryBuilder shows the  
builder pattern for fluent interface construction.
