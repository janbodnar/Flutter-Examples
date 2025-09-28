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
