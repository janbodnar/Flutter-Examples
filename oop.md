# Flutter Object-Oriented Programming

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

## Generic Classes and Type Safety

Implementing type-safe generic classes with constraints and bounds.

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
      title: 'Generic Classes and Type Safety',
      theme: ThemeData.dark(),
      home: const GenericClassesDemo(),
    );
  }
}

// Generic repository interface for data operations
abstract class Repository<T, ID> {
  Future<T?> findById(ID id);
  Future<List<T>> findAll();
  Future<T> save(T entity);
  Future<void> deleteById(ID id);
  Future<bool> existsById(ID id);
}

// Generic entity base class
abstract class Entity<ID> {
  ID get id;
  DateTime get createdAt;
  DateTime get updatedAt;
}

// Concrete entity implementations
class User extends Entity<String> {
  @override
  final String id;
  final String name;
  final String email;
  final int age;
  @override
  final DateTime createdAt;
  @override
  final DateTime updatedAt;

  User({
    required this.id,
    required this.name,
    required this.email,
    required this.age,
    DateTime? createdAt,
    DateTime? updatedAt,
  }) : createdAt = createdAt ?? DateTime.now(),
       updatedAt = updatedAt ?? DateTime.now();

  User copyWith({
    String? name,
    String? email,
    int? age,
  }) {
    return User(
      id: id,
      name: name ?? this.name,
      email: email ?? this.email,
      age: age ?? this.age,
      createdAt: createdAt,
      updatedAt: DateTime.now(),
    );
  }

  @override
  String toString() => 'User(id: $id, name: $name, email: $email, age: $age)';
}

class Product extends Entity<int> {
  @override
  final int id;
  final String name;
  final double price;
  final String category;
  @override
  final DateTime createdAt;
  @override
  final DateTime updatedAt;

  Product({
    required this.id,
    required this.name,
    required this.price,
    required this.category,
    DateTime? createdAt,
    DateTime? updatedAt,
  }) : createdAt = createdAt ?? DateTime.now(),
       updatedAt = updatedAt ?? DateTime.now();

  Product copyWith({
    String? name,
    double? price,
    String? category,
  }) {
    return Product(
      id: id,
      name: name ?? this.name,
      price: price ?? this.price,
      category: category ?? this.category,
      createdAt: createdAt,
      updatedAt: DateTime.now(),
    );
  }

  @override
  String toString() => 'Product(id: $id, name: $name, price: \$${price.toStringAsFixed(2)}, category: $category)';
}

// Generic in-memory repository implementation
class InMemoryRepository<T extends Entity<ID>, ID> implements Repository<T, ID> {
  final Map<ID, T> _storage = {};
  final String entityType;

  InMemoryRepository(this.entityType);

  @override
  Future<T?> findById(ID id) async {
    await Future.delayed(const Duration(milliseconds: 100)); // Simulate DB access
    return _storage[id];
  }

  @override
  Future<List<T>> findAll() async {
    await Future.delayed(const Duration(milliseconds: 150));
    return _storage.values.toList();
  }

  @override
  Future<T> save(T entity) async {
    await Future.delayed(const Duration(milliseconds: 200));
    _storage[entity.id] = entity;
    return entity;
  }

  @override
  Future<void> deleteById(ID id) async {
    await Future.delayed(const Duration(milliseconds: 100));
    _storage.remove(id);
  }

  @override
  Future<bool> existsById(ID id) async {
    await Future.delayed(const Duration(milliseconds: 50));
    return _storage.containsKey(id);
  }

  // Additional methods specific to in-memory implementation
  int get size => _storage.length;
  List<ID> get allIds => _storage.keys.toList();
  void clear() => _storage.clear();

  // Generic search method with predicate
  Future<List<T>> findWhere(bool Function(T) predicate) async {
    await Future.delayed(const Duration(milliseconds: 100));
    return _storage.values.where(predicate).toList();
  }
}

// Generic service layer with business logic
class EntityService<T extends Entity<ID>, ID> {
  final Repository<T, ID> repository;
  final String entityName;

  EntityService(this.repository, this.entityName);

  Future<T?> getById(ID id) async {
    return await repository.findById(id);
  }

  Future<List<T>> getAll() async {
    return await repository.findAll();
  }

  Future<T> create(T entity) async {
    final exists = await repository.existsById(entity.id);
    if (exists) {
      throw StateError('$entityName with ID ${entity.id} already exists');
    }
    return await repository.save(entity);
  }

  Future<T> update(T entity) async {
    final exists = await repository.existsById(entity.id);
    if (!exists) {
      throw StateError('$entityName with ID ${entity.id} does not exist');
    }
    return await repository.save(entity);
  }

  Future<void> delete(ID id) async {
    final exists = await repository.existsById(entity.id);
    if (!exists) {
      throw StateError('$entityName with ID $id does not exist');
    }
    await repository.deleteById(id);
  }

  Future<bool> exists(ID id) async {
    return await repository.existsById(id);
  }
}

// Generic collection wrapper with type safety
class TypedCollection<T> {
  final List<T> _items = [];
  final String typeName;

  TypedCollection(this.typeName);

  void add(T item) {
    _items.add(item);
  }

  void addAll(Iterable<T> items) {
    _items.addAll(items);
  }

  T? findFirst(bool Function(T) predicate) {
    try {
      return _items.firstWhere(predicate);
    } catch (e) {
      return null;
    }
  }

  List<T> findAll(bool Function(T) predicate) {
    return _items.where(predicate).toList();
  }

  bool remove(T item) {
    return _items.remove(item);
  }

  T removeAt(int index) {
    return _items.removeAt(index);
  }

  void clear() {
    _items.clear();
  }

  // Generic transformation methods
  TypedCollection<R> map<R>(R Function(T) transform, String newTypeName) {
    final newCollection = TypedCollection<R>(newTypeName);
    newCollection.addAll(_items.map(transform));
    return newCollection;
  }

  TypedCollection<T> filter(bool Function(T) predicate) {
    final filtered = TypedCollection<T>(typeName);
    filtered.addAll(_items.where(predicate));
    return filtered;
  }

  // Aggregate operations
  R fold<R>(R initialValue, R Function(R previous, T element) combine) {
    return _items.fold(initialValue, combine);
  }

  T? reduce(T Function(T value, T element) combine) {
    return _items.isEmpty ? null : _items.reduce(combine);
  }

  // Properties
  int get length => _items.length;
  bool get isEmpty => _items.isEmpty;
  bool get isNotEmpty => _items.isNotEmpty;
  List<T> get items => List.unmodifiable(_items);
  T operator [](int index) => _items[index];
}

class GenericClassesDemo extends StatefulWidget {
  const GenericClassesDemo({super.key});

  @override
  State<GenericClassesDemo> createState() => _GenericClassesDemoState();
}

class _GenericClassesDemoState extends State<GenericClassesDemo> {
  late InMemoryRepository<User, String> userRepository;
  late InMemoryRepository<Product, int> productRepository;
  late EntityService<User, String> userService;
  late EntityService<Product, int> productService;
  
  final TypedCollection<String> stringCollection = TypedCollection<String>('String');
  final TypedCollection<int> intCollection = TypedCollection<int>('Integer');
  
  String _operationResult = '';
  List<String> _operations = [];

  @override
  void initState() {
    super.initState();
    
    // Initialize repositories and services
    userRepository = InMemoryRepository<User, String>('User');
    productRepository = InMemoryRepository<Product, int>('Product');
    userService = EntityService<User, String>(userRepository, 'User');
    productService = EntityService<Product, int>(productRepository, 'Product');

    // Initialize with sample data
    _initializeData();
  }

  void _initializeData() async {
    // Add sample users
    await userService.create(User(
      id: 'user1',
      name: 'Alice Johnson',
      email: 'alice@example.com',
      age: 28,
    ));
    
    await userService.create(User(
      id: 'user2',
      name: 'Bob Smith',
      email: 'bob@example.com',
      age: 35,
    ));

    // Add sample products
    await productService.create(Product(
      id: 1,
      name: 'Gaming Laptop',
      price: 1299.99,
      category: 'Electronics',
    ));
    
    await productService.create(Product(
      id: 2,
      name: 'Office Chair',
      price: 299.99,
      category: 'Furniture',
    ));

    // Add items to typed collections
    stringCollection.addAll(['Alpha', 'Beta', 'Gamma', 'Delta']);
    intCollection.addAll([10, 20, 30, 40, 50]);

    setState(() {
      _addOperation('Sample data initialized');
    });
  }

  void _addOperation(String operation) {
    _operations.insert(0, '${DateTime.now().toString().substring(11, 19)}: $operation');
    if (_operations.length > 8) {
      _operations.removeLast();
    }
  }

  Future<void> _performUserOperation(String operation) async {
    try {
      switch (operation) {
        case 'find_all':
          final users = await userService.getAll();
          setState(() {
            _operationResult = 'Found ${users.length} users:\n${users.join('\n')}';
          });
          _addOperation('Found ${users.length} users');
          break;
        case 'find_adults':
          final adults = await userRepository.findWhere((user) => user.age >= 18);
          setState(() {
            _operationResult = 'Found ${adults.length} adults:\n${adults.join('\n')}';
          });
          _addOperation('Found ${adults.length} adult users');
          break;
        case 'add_user':
          final newUser = User(
            id: 'user${DateTime.now().millisecondsSinceEpoch}',
            name: 'New User',
            email: 'new@example.com',
            age: 25,
          );
          await userService.create(newUser);
          setState(() {
            _operationResult = 'Created user: $newUser';
          });
          _addOperation('Created new user');
          break;
      }
    } catch (e) {
      setState(() {
        _operationResult = 'Error: $e';
      });
      _addOperation('Error in user operation: $e');
    }
  }

  Future<void> _performProductOperation(String operation) async {
    try {
      switch (operation) {
        case 'find_all':
          final products = await productService.getAll();
          setState(() {
            _operationResult = 'Found ${products.length} products:\n${products.join('\n')}';
          });
          _addOperation('Found ${products.length} products');
          break;
        case 'find_expensive':
          final expensive = await productRepository.findWhere((product) => product.price > 500);
          setState(() {
            _operationResult = 'Found ${expensive.length} expensive products:\n${expensive.join('\n')}';
          });
          _addOperation('Found ${expensive.length} expensive products');
          break;
        case 'add_product':
          final newProduct = Product(
            id: DateTime.now().millisecondsSinceEpoch,
            name: 'New Product',
            price: 99.99,
            category: 'General',
          );
          await productService.create(newProduct);
          setState(() {
            _operationResult = 'Created product: $newProduct';
          });
          _addOperation('Created new product');
          break;
      }
    } catch (e) {
      setState(() {
        _operationResult = 'Error: $e';
      });
      _addOperation('Error in product operation: $e');
    }
  }

  void _performCollectionOperation(String operation) {
    switch (operation) {
      case 'filter_strings':
        final filtered = stringCollection.filter((str) => str.length > 4);
        setState(() {
          _operationResult = 'Filtered strings (length > 4): ${filtered.items.join(', ')}';
        });
        _addOperation('Filtered string collection');
        break;
      case 'map_strings':
        final mapped = stringCollection.map<int>((str) => str.length, 'StringLength');
        setState(() {
          _operationResult = 'Mapped to lengths: ${mapped.items.join(', ')}';
        });
        _addOperation('Mapped strings to lengths');
        break;
      case 'sum_integers':
        final sum = intCollection.fold<int>(0, (prev, element) => prev + element);
        setState(() {
          _operationResult = 'Sum of integers: $sum';
        });
        _addOperation('Calculated sum of integers');
        break;
      case 'add_items':
        stringCollection.add('Echo');
        intCollection.add(60);
        setState(() {
          _operationResult = 'Added items to collections';
        });
        _addOperation('Added items to typed collections');
        break;
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Generic Classes and Type Safety'),
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

            // Generic Repository Operations
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Generic Repository<User, String>',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Repository Size: ${userRepository.size}'),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performUserOperation('find_all'),
                          child: const Text('Find All Users'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performUserOperation('find_adults'),
                          child: const Text('Find Adults'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performUserOperation('add_user'),
                          child: const Text('Add User'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Generic Repository Operations - Products
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Generic Repository<Product, int>',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Repository Size: ${productRepository.size}'),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performProductOperation('find_all'),
                          child: const Text('Find All Products'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performProductOperation('find_expensive'),
                          child: const Text('Find Expensive'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performProductOperation('add_product'),
                          child: const Text('Add Product'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Generic Collections
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Generic Collections',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('TypedCollection<String>: ${stringCollection.length} items'),
                    Text('Items: ${stringCollection.items.join(', ')}'),
                    const SizedBox(height: 8),
                    Text('TypedCollection<int>: ${intCollection.length} items'),
                    Text('Items: ${intCollection.items.join(', ')}'),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performCollectionOperation('filter_strings'),
                          child: const Text('Filter Strings'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performCollectionOperation('map_strings'),
                          child: const Text('Map to Lengths'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performCollectionOperation('sum_integers'),
                          child: const Text('Sum Integers'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performCollectionOperation('add_items'),
                          child: const Text('Add Items'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

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
                      height: 120,
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
                            padding: const EdgeInsets.only(bottom: 2),
                            child: Text(
                              op,
                              style: const TextStyle(fontSize: 11, fontFamily: 'monospace'),
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
                      'Generic Programming Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Generic classes: Repository<T, ID>, TypedCollection<T>'),
                    const Text('• Type constraints: <T extends Entity<ID>>'),
                    const Text('• Generic methods: map<R>(), fold<R>(), findWhere()'),
                    const Text('• Type safety: Compile-time type checking and inference'),
                    const Text('• Generic interfaces: Repository<T, ID> contract'),
                    const Text('• Type parameters: Multiple type variables in single class'),
                    const Text('• Generic inheritance: Entity<ID> base class'),
                    const Text('• Type bounds: Restricting generic parameters to specific types'),
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

This example demonstrates generic programming with type-safe collections,  
repositories, and services. Generic classes provide code reuse while  
maintaining type safety through compile-time checking. The Repository  
pattern shows how generics enable flexible data access layers, while  
TypedCollection demonstrates generic transformations and operations.

## Composition vs Inheritance

Demonstrating composition patterns as alternatives to inheritance.

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
      title: 'Composition vs Inheritance',
      theme: ThemeData.dark(),
      home: const CompositionDemo(),
    );
  }
}

// Components for composition
class Logger {
  final String name;
  final List<String> _logs = [];
  
  Logger(this.name);
  
  void log(String message) {
    final timestamp = DateTime.now().toString().substring(0, 19);
    final logEntry = '[$timestamp] [$name] $message';
    _logs.add(logEntry);
    print(logEntry);
  }
  
  List<String> getLogs() => List.unmodifiable(_logs);
  void clearLogs() => _logs.clear();
}

class Validator {
  final Map<String, List<String Function(dynamic)>> _rules = {};
  
  void addRule(String field, String Function(dynamic) rule) {
    _rules.putIfAbsent(field, () => []).add(rule);
  }
  
  Map<String, List<String>> validate(Map<String, dynamic> data) {
    Map<String, List<String>> errors = {};
    
    _rules.forEach((field, rules) {
      final value = data[field];
      for (var rule in rules) {
        final error = rule(value);
        if (error.isNotEmpty) {
          errors.putIfAbsent(field, () => []).add(error);
        }
      }
    });
    
    return errors;
  }
  
  bool isValid(Map<String, dynamic> data) {
    return validate(data).isEmpty;
  }
}

class Cache<T> {
  final Map<String, T> _cache = {};
  final int maxSize;
  
  Cache({this.maxSize = 100});
  
  void put(String key, T value) {
    if (_cache.length >= maxSize) {
      // Simple LRU: remove first entry
      final firstKey = _cache.keys.first;
      _cache.remove(firstKey);
    }
    _cache[key] = value;
  }
  
  T? get(String key) => _cache[key];
  
  bool containsKey(String key) => _cache.containsKey(key);
  
  void remove(String key) => _cache.remove(key);
  
  void clear() => _cache.clear();
  
  int get size => _cache.length;
  
  List<String> get keys => _cache.keys.toList();
}

class EventEmitter {
  final Map<String, List<Function(dynamic)>> _listeners = {};
  
  void on(String event, Function(dynamic) listener) {
    _listeners.putIfAbsent(event, () => []).add(listener);
  }
  
  void emit(String event, [dynamic data]) {
    final listeners = _listeners[event];
    if (listeners != null) {
      for (var listener in listeners) {
        listener(data);
      }
    }
  }
  
  void off(String event, [Function(dynamic)? listener]) {
    if (listener != null) {
      _listeners[event]?.remove(listener);
    } else {
      _listeners[event]?.clear();
    }
  }
  
  List<String> get events => _listeners.keys.toList();
}

// Inheritance-based approach (traditional OOP)
abstract class BaseEntity {
  String id;
  DateTime createdAt;
  DateTime updatedAt;
  
  BaseEntity(this.id) : 
    createdAt = DateTime.now(),
    updatedAt = DateTime.now();
  
  void log(String message) {
    print('[$id] $message');
  }
  
  bool validate() {
    return id.isNotEmpty;
  }
  
  void touch() {
    updatedAt = DateTime.now();
  }
}

class InheritanceUser extends BaseEntity {
  String name;
  String email;
  
  InheritanceUser(String id, this.name, this.email) : super(id);
  
  @override
  bool validate() {
    return super.validate() && 
           name.isNotEmpty && 
           email.contains('@');
  }
  
  void updateProfile(String newName, String newEmail) {
    log('Updating profile');
    name = newName;
    email = newEmail;
    touch();
  }
  
  @override
  String toString() => 'InheritanceUser(id: $id, name: $name, email: $email)';
}

// Composition-based approach (preferred)
class CompositionUser {
  final String id;
  String name;
  String email;
  final DateTime createdAt;
  DateTime updatedAt;
  
  // Composed components
  final Logger _logger;
  final Validator _validator;
  final Cache<String> _cache;
  final EventEmitter _eventEmitter;
  
  CompositionUser(this.id, this.name, this.email) 
    : createdAt = DateTime.now(),
      updatedAt = DateTime.now(),
      _logger = Logger('User-$id'),
      _validator = Validator(),
      _cache = Cache<String>(maxSize: 10),
      _eventEmitter = EventEmitter() {
    
    // Setup validation rules
    _validator.addRule('name', (value) => 
        value == null || value.isEmpty ? 'Name is required' : '');
    _validator.addRule('email', (value) => 
        value == null || !value.contains('@') ? 'Invalid email format' : '');
    
    _logger.log('User created: $name ($email)');
    _eventEmitter.emit('user_created', this);
  }
  
  // Delegate functionality to components
  void log(String message) => _logger.log(message);
  
  bool validate() {
    final data = {'name': name, 'email': email};
    final isValid = _validator.isValid(data);
    log('Validation result: $isValid');
    return isValid;
  }
  
  Map<String, List<String>> getValidationErrors() {
    return _validator.validate({'name': name, 'email': email});
  }
  
  void cacheData(String key, String value) {
    _cache.put(key, value);
    log('Cached data: $key');
  }
  
  String? getCachedData(String key) {
    final value = _cache.get(key);
    log('Retrieved cached data: $key ${value != null ? '(found)' : '(not found)'}');
    return value;
  }
  
  void addEventListener(String event, Function(dynamic) listener) {
    _eventEmitter.on(event, listener);
  }
  
  void updateProfile(String newName, String newEmail) {
    log('Updating profile from ($name, $email) to ($newName, $newEmail)');
    
    final oldData = {'name': name, 'email': email};
    name = newName;
    email = newEmail;
    updatedAt = DateTime.now();
    
    _eventEmitter.emit('profile_updated', {
      'old': oldData,
      'new': {'name': name, 'email': email},
      'user': this,
    });
    
    log('Profile updated successfully');
  }
  
  void performExpensiveOperation() {
    const cacheKey = 'expensive_result';
    
    // Check cache first
    final cached = getCachedData(cacheKey);
    if (cached != null) {
      log('Returning cached result: $cached');
      _eventEmitter.emit('operation_completed', cached);
      return;
    }
    
    // Perform expensive operation
    log('Performing expensive computation...');
    final result = 'Computed result for $name at ${DateTime.now()}';
    
    // Cache the result
    cacheData(cacheKey, result);
    _eventEmitter.emit('operation_completed', result);
  }
  
  // Expose component functionality
  List<String> get logs => _logger.getLogs();
  int get cacheSize => _cache.size;
  List<String> get cachedKeys => _cache.keys;
  List<String> get events => _eventEmitter.events;
  
  @override
  String toString() => 'CompositionUser(id: $id, name: $name, email: $email)';
}

// Example showing composition with different component combinations
class SmartDevice {
  final String id;
  final String name;
  final String type;
  
  // Flexible composition - only include needed components
  final Logger? logger;
  final Cache<Map<String, dynamic>>? cache;
  final EventEmitter? eventEmitter;
  
  bool _isOnline = false;
  Map<String, dynamic> _settings = {};
  
  SmartDevice({
    required this.id,
    required this.name,
    required this.type,
    bool enableLogging = true,
    bool enableCaching = true,
    bool enableEvents = true,
  }) : logger = enableLogging ? Logger('Device-$id') : null,
       cache = enableCaching ? Cache<Map<String, dynamic>>(maxSize: 5) : null,
       eventEmitter = enableEvents ? EventEmitter() : null {
    
    logger?.log('Device initialized: $name ($type)');
  }
  
  void connect() {
    _isOnline = true;
    logger?.log('Device connected');
    eventEmitter?.emit('device_connected', this);
  }
  
  void disconnect() {
    _isOnline = false;
    logger?.log('Device disconnected');
    eventEmitter?.emit('device_disconnected', this);
  }
  
  void updateSetting(String key, dynamic value) {
    final oldValue = _settings[key];
    _settings[key] = value;
    
    logger?.log('Setting updated: $key = $value (was: $oldValue)');
    
    // Cache settings if caching is enabled
    cache?.put('settings', Map<String, dynamic>.from(_settings));
    
    eventEmitter?.emit('setting_changed', {
      'key': key,
      'oldValue': oldValue,
      'newValue': value,
    });
  }
  
  Map<String, dynamic> getSettings() {
    // Try cache first
    final cached = cache?.get('settings');
    if (cached != null) {
      logger?.log('Retrieved settings from cache');
      return Map<String, dynamic>.from(cached);
    }
    
    logger?.log('Retrieved settings from memory');
    return Map<String, dynamic>.from(_settings);
  }
  
  bool get isOnline => _isOnline;
  List<String> get logs => logger?.getLogs() ?? [];
}

class CompositionDemo extends StatefulWidget {
  const CompositionDemo({super.key});

  @override
  State<CompositionDemo> createState() => _CompositionDemoState();
}

class _CompositionDemoState extends State<CompositionDemo> {
  late InheritanceUser inheritanceUser;
  late CompositionUser compositionUser;
  late SmartDevice smartDevice;
  
  String _operationResult = '';
  List<String> _events = [];

  @override
  void initState() {
    super.initState();
    
    // Initialize inheritance-based user
    inheritanceUser = InheritanceUser('INH001', 'John Doe', 'john@example.com');
    
    // Initialize composition-based user
    compositionUser = CompositionUser('COMP001', 'Jane Smith', 'jane@example.com');
    
    // Setup event listeners for composition user
    compositionUser.addEventListener('profile_updated', (data) {
      _addEvent('Profile updated: ${data['old']} -> ${data['new']}');
    });
    
    compositionUser.addEventListener('operation_completed', (result) {
      _addEvent('Operation completed: $result');
    });
    
    // Initialize smart device
    smartDevice = SmartDevice(
      id: 'DEV001',
      name: 'Smart Thermostat',
      type: 'Climate Control',
    );
    
    smartDevice.eventEmitter?.on('device_connected', (device) {
      _addEvent('Device connected: ${device.name}');
    });
    
    smartDevice.eventEmitter?.on('setting_changed', (data) {
      _addEvent('Device setting changed: ${data['key']} = ${data['newValue']}');
    });
  }

  void _addEvent(String event) {
    setState(() {
      _events.insert(0, '${DateTime.now().toString().substring(11, 19)}: $event');
      if (_events.length > 8) {
        _events.removeLast();
      }
    });
  }

  void _performInheritanceOperation(String operation) {
    setState(() {
      switch (operation) {
        case 'update_profile':
          inheritanceUser.updateProfile('John Updated', 'john.updated@example.com');
          _operationResult = 'Inheritance: Profile updated\n$inheritanceUser';
          break;
        case 'validate':
          bool isValid = inheritanceUser.validate();
          _operationResult = 'Inheritance: Validation result: $isValid\n$inheritanceUser';
          break;
      }
    });
  }

  void _performCompositionOperation(String operation) {
    setState(() {
      switch (operation) {
        case 'update_profile':
          compositionUser.updateProfile('Jane Updated', 'jane.updated@example.com');
          _operationResult = 'Composition: Profile updated\n$compositionUser';
          break;
        case 'validate':
          bool isValid = compositionUser.validate();
          final errors = compositionUser.getValidationErrors();
          _operationResult = 'Composition: Validation result: $isValid\nErrors: $errors\n$compositionUser';
          break;
        case 'cache_operation':
          compositionUser.cacheData('temp_data', 'Temporary cached value');
          final cached = compositionUser.getCachedData('temp_data');
          _operationResult = 'Composition: Cached and retrieved: $cached';
          break;
        case 'expensive_operation':
          compositionUser.performExpensiveOperation();
          _operationResult = 'Composition: Expensive operation completed (check events)';
          break;
      }
    });
  }

  void _performDeviceOperation(String operation) {
    setState(() {
      switch (operation) {
        case 'connect':
          smartDevice.connect();
          _operationResult = 'Device: Connected - ${smartDevice.name}';
          break;
        case 'update_setting':
          smartDevice.updateSetting('temperature', 22.5);
          smartDevice.updateSetting('mode', 'heating');
          final settings = smartDevice.getSettings();
          _operationResult = 'Device: Settings updated - $settings';
          break;
        case 'disconnect':
          smartDevice.disconnect();
          _operationResult = 'Device: Disconnected - ${smartDevice.name}';
          break;
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Composition vs Inheritance'),
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

            // Inheritance Example
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Inheritance Approach',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text(inheritanceUser.toString()),
                    Text('Created: ${inheritanceUser.createdAt.toString().substring(0, 19)}'),
                    Text('Updated: ${inheritanceUser.updatedAt.toString().substring(0, 19)}'),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performInheritanceOperation('update_profile'),
                          child: const Text('Update Profile'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performInheritanceOperation('validate'),
                          child: const Text('Validate'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Composition Example
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Composition Approach',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text(compositionUser.toString()),
                    Text('Created: ${compositionUser.createdAt.toString().substring(0, 19)}'),
                    Text('Updated: ${compositionUser.updatedAt.toString().substring(0, 19)}'),
                    Text('Logs: ${compositionUser.logs.length}'),
                    Text('Cache size: ${compositionUser.cacheSize}'),
                    Text('Events: ${compositionUser.events.join(', ')}'),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performCompositionOperation('update_profile'),
                          child: const Text('Update Profile'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performCompositionOperation('validate'),
                          child: const Text('Validate'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performCompositionOperation('cache_operation'),
                          child: const Text('Cache Data'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performCompositionOperation('expensive_operation'),
                          child: const Text('Expensive Operation'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Smart Device Example (Flexible Composition)
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Flexible Composition (Smart Device)',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Device: ${smartDevice.name} (${smartDevice.type})'),
                    Text('Online: ${smartDevice.isOnline}'),
                    Text('Logs: ${smartDevice.logs.length}'),
                    Text('Settings: ${smartDevice.getSettings()}'),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performDeviceOperation('connect'),
                          child: const Text('Connect'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performDeviceOperation('update_setting'),
                          child: const Text('Update Settings'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performDeviceOperation('disconnect'),
                          child: const Text('Disconnect'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Events Log
            if (_events.isNotEmpty) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Events Log (Composition Components)',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Container(
                        height: 150,
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
                            children: _events.map((event) => Padding(
                              padding: const EdgeInsets.only(bottom: 2),
                              child: Text(
                                event,
                                style: const TextStyle(fontSize: 11, fontFamily: 'monospace'),
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
            ],

            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Composition vs Inheritance Concepts',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Inheritance: "is-a" relationship, tight coupling, single parent'),
                    const Text('• Composition: "has-a" relationship, loose coupling, multiple components'),
                    const Text('• Flexibility: Composition allows runtime component selection'),
                    const Text('• Reusability: Components can be shared across different classes'),
                    const Text('• Testing: Easier to mock individual components in composition'),
                    const Text('• Single Responsibility: Each component handles one concern'),
                    const Text('• Dependency Injection: Components can be injected for flexibility'),
                    const Text('• Strategy Pattern: Different component implementations'),
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

This example demonstrates composition versus inheritance approaches to  
object design. The inheritance approach creates tight coupling through  
class hierarchies, while composition promotes loose coupling by combining  
focused components. Composition provides greater flexibility, easier  
testing, and better separation of concerns.

## Delegation Pattern and Proxy Objects

Implementing delegation and proxy patterns for controlled object access.

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
      title: 'Delegation and Proxy Patterns',
      theme: ThemeData.dark(),
      home: const DelegationDemo(),
    );
  }
}

// Service interface for delegation
abstract class UserService {
  Future<List<Map<String, dynamic>>> getAllUsers();
  Future<Map<String, dynamic>?> getUserById(String id);
  Future<Map<String, dynamic>> createUser(Map<String, dynamic> userData);
  Future<Map<String, dynamic>> updateUser(String id, Map<String, dynamic> userData);
  Future<bool> deleteUser(String id);
}

// Concrete service implementation
class ConcreteUserService implements UserService {
  final Map<String, Map<String, dynamic>> _users = {};

  @override
  Future<List<Map<String, dynamic>>> getAllUsers() async {
    await Future.delayed(const Duration(milliseconds: 200));
    return _users.values.toList();
  }

  @override
  Future<Map<String, dynamic>?> getUserById(String id) async {
    await Future.delayed(const Duration(milliseconds: 100));
    return _users[id];
  }

  @override
  Future<Map<String, dynamic>> createUser(Map<String, dynamic> userData) async {
    await Future.delayed(const Duration(milliseconds: 150));
    final id = DateTime.now().millisecondsSinceEpoch.toString();
    userData['id'] = id;
    userData['createdAt'] = DateTime.now().toIso8601String();
    _users[id] = userData;
    return userData;
  }

  @override
  Future<Map<String, dynamic>> updateUser(String id, Map<String, dynamic> userData) async {
    await Future.delayed(const Duration(milliseconds: 150));
    if (!_users.containsKey(id)) {
      throw Exception('User not found: $id');
    }
    userData['id'] = id;
    userData['updatedAt'] = DateTime.now().toIso8601String();
    _users[id] = {..._users[id]!, ...userData};
    return _users[id]!;
  }

  @override
  Future<bool> deleteUser(String id) async {
    await Future.delayed(const Duration(milliseconds: 100));
    return _users.remove(id) != null;
  }
}

// Caching proxy that delegates to the real service
class CachingUserServiceProxy implements UserService {
  final UserService _delegate;
  final Map<String, dynamic> _cache = {};
  final Map<String, DateTime> _cacheTimestamps = {};
  final Duration _cacheExpiry = const Duration(minutes: 5);

  CachingUserServiceProxy(this._delegate);

  String _getCacheKey(String operation, [String? id]) {
    return id != null ? '${operation}_$id' : operation;
  }

  bool _isCacheValid(String key) {
    final timestamp = _cacheTimestamps[key];
    if (timestamp == null) return false;
    return DateTime.now().difference(timestamp) < _cacheExpiry;
  }

  @override
  Future<List<Map<String, dynamic>>> getAllUsers() async {
    const key = 'all_users';
    
    if (_isCacheValid(key)) {
      return List<Map<String, dynamic>>.from(_cache[key]);
    }
    
    final users = await _delegate.getAllUsers();
    _cache[key] = users;
    _cacheTimestamps[key] = DateTime.now();
    
    return users;
  }

  @override
  Future<Map<String, dynamic>?> getUserById(String id) async {
    final key = _getCacheKey('user', id);
    
    if (_isCacheValid(key)) {
      return Map<String, dynamic>.from(_cache[key]);
    }
    
    final user = await _delegate.getUserById(id);
    if (user != null) {
      _cache[key] = user;
      _cacheTimestamps[key] = DateTime.now();
    }
    
    return user;
  }

  @override
  Future<Map<String, dynamic>> createUser(Map<String, dynamic> userData) async {
    final user = await _delegate.createUser(userData);
    
    // Invalidate cache
    _cache.remove('all_users');
    _cacheTimestamps.remove('all_users');
    
    return user;
  }

  @override
  Future<Map<String, dynamic>> updateUser(String id, Map<String, dynamic> userData) async {
    final user = await _delegate.updateUser(id, userData);
    
    // Update cache
    final userKey = _getCacheKey('user', id);
    _cache[userKey] = user;
    _cacheTimestamps[userKey] = DateTime.now();
    
    // Invalidate all users cache
    _cache.remove('all_users');
    _cacheTimestamps.remove('all_users');
    
    return user;
  }

  @override
  Future<bool> deleteUser(String id) async {
    final success = await _delegate.deleteUser(id);
    
    if (success) {
      // Remove from cache
      _cache.remove(_getCacheKey('user', id));
      _cacheTimestamps.remove(_getCacheKey('user', id));
      
      // Invalidate all users cache
      _cache.remove('all_users');
      _cacheTimestamps.remove('all_users');
    }
    
    return success;
  }

  // Proxy-specific methods
  void clearCache() {
    _cache.clear();
    _cacheTimestamps.clear();
  }
  
  Map<String, DateTime> get cacheInfo => Map.from(_cacheTimestamps);
}

// Logging proxy that delegates and adds logging
class LoggingUserServiceProxy implements UserService {
  final UserService _delegate;
  final List<String> _operationLog = [];

  LoggingUserServiceProxy(this._delegate);

  void _log(String operation, [String? details]) {
    final entry = '${DateTime.now().toIso8601String()}: $operation${details != null ? ' - $details' : ''}';
    _operationLog.add(entry);
  }

  @override
  Future<List<Map<String, dynamic>>> getAllUsers() async {
    _log('getAllUsers', 'Starting operation');
    try {
      final users = await _delegate.getAllUsers();
      _log('getAllUsers', 'Success - ${users.length} users retrieved');
      return users;
    } catch (e) {
      _log('getAllUsers', 'Error - $e');
      rethrow;
    }
  }

  @override
  Future<Map<String, dynamic>?> getUserById(String id) async {
    _log('getUserById', 'ID: $id');
    try {
      final user = await _delegate.getUserById(id);
      _log('getUserById', user != null ? 'Success - User found' : 'User not found');
      return user;
    } catch (e) {
      _log('getUserById', 'Error - $e');
      rethrow;
    }
  }

  @override
  Future<Map<String, dynamic>> createUser(Map<String, dynamic> userData) async {
    _log('createUser', 'Name: ${userData['name']}');
    try {
      final user = await _delegate.createUser(userData);
      _log('createUser', 'Success - User created with ID: ${user['id']}');
      return user;
    } catch (e) {
      _log('createUser', 'Error - $e');
      rethrow;
    }
  }

  @override
  Future<Map<String, dynamic>> updateUser(String id, Map<String, dynamic> userData) async {
    _log('updateUser', 'ID: $id, Changes: ${userData.keys.join(', ')}');
    try {
      final user = await _delegate.updateUser(id, userData);
      _log('updateUser', 'Success - User updated');
      return user;
    } catch (e) {
      _log('updateUser', 'Error - $e');
      rethrow;
    }
  }

  @override
  Future<bool> deleteUser(String id) async {
    _log('deleteUser', 'ID: $id');
    try {
      final success = await _delegate.deleteUser(id);
      _log('deleteUser', success ? 'Success - User deleted' : 'User not found');
      return success;
    } catch (e) {
      _log('deleteUser', 'Error - $e');
      rethrow;
    }
  }

  List<String> get operationLog => List.unmodifiable(_operationLog);
  
  void clearLog() {
    _operationLog.clear();
  }
}

// Access control proxy with role-based permissions
class AccessControlUserServiceProxy implements UserService {
  final UserService _delegate;
  final String _userRole;
  final Map<String, List<String>> _permissions = {
    'admin': ['read', 'write', 'delete'],
    'editor': ['read', 'write'],
    'viewer': ['read'],
  };

  AccessControlUserServiceProxy(this._delegate, this._userRole);

  bool _hasPermission(String permission) {
    final userPermissions = _permissions[_userRole] ?? [];
    return userPermissions.contains(permission);
  }

  void _checkPermission(String permission, String operation) {
    if (!_hasPermission(permission)) {
      throw Exception('Access denied: $operation requires $permission permission');
    }
  }

  @override
  Future<List<Map<String, dynamic>>> getAllUsers() async {
    _checkPermission('read', 'getAllUsers');
    return await _delegate.getAllUsers();
  }

  @override
  Future<Map<String, dynamic>?> getUserById(String id) async {
    _checkPermission('read', 'getUserById');
    return await _delegate.getUserById(id);
  }

  @override
  Future<Map<String, dynamic>> createUser(Map<String, dynamic> userData) async {
    _checkPermission('write', 'createUser');
    return await _delegate.createUser(userData);
  }

  @override
  Future<Map<String, dynamic>> updateUser(String id, Map<String, dynamic> userData) async {
    _checkPermission('write', 'updateUser');
    return await _delegate.updateUser(id, userData);
  }

  @override
  Future<bool> deleteUser(String id) async {
    _checkPermission('delete', 'deleteUser');
    return await _delegate.deleteUser(id);
  }

  String get role => _userRole;
  List<String> get permissions => _permissions[_userRole] ?? [];
}

class DelegationDemo extends StatefulWidget {
  const DelegationDemo({super.key});

  @override
  State<DelegationDemo> createState() => _DelegationDemoState();
}

class _DelegationDemoState extends State<DelegationDemo> {
  late ConcreteUserService concreteService;
  late CachingUserServiceProxy cachingProxy;
  late LoggingUserServiceProxy loggingProxy;
  late AccessControlUserServiceProxy accessProxy;
  
  UserService currentService = ConcreteUserService();
  String currentServiceType = 'Concrete Service';
  String currentRole = 'admin';
  
  String _operationResult = '';
  List<Map<String, dynamic>> _users = [];

  @override
  void initState() {
    super.initState();
    
    concreteService = ConcreteUserService();
    cachingProxy = CachingUserServiceProxy(concreteService);
    loggingProxy = LoggingUserServiceProxy(cachingProxy);
    accessProxy = AccessControlUserServiceProxy(loggingProxy, currentRole);
    
    currentService = accessProxy;
    _initializeData();
  }

  void _initializeData() async {
    try {
      await currentService.createUser({
        'name': 'Alice Johnson',
        'email': 'alice@example.com',
        'role': 'admin'
      });
      
      await currentService.createUser({
        'name': 'Bob Smith', 
        'email': 'bob@example.com',
        'role': 'editor'
      });
      
      _refreshUsersList();
    } catch (e) {
      print('Initialization error: $e');
    }
  }

  void _switchService(String serviceType) {
    setState(() {
      currentServiceType = serviceType;
      switch (serviceType) {
        case 'Concrete Service':
          currentService = concreteService;
          break;
        case 'Caching Proxy':
          currentService = cachingProxy;
          break;
        case 'Logging Proxy':
          currentService = loggingProxy;
          break;
        case 'Access Control Proxy':
          currentService = AccessControlUserServiceProxy(loggingProxy, currentRole);
          break;
        case 'Full Proxy Chain':
          currentService = AccessControlUserServiceProxy(loggingProxy, currentRole);
          break;
      }
    });
  }

  void _changeRole(String role) {
    setState(() {
      currentRole = role;
      if (currentServiceType.contains('Access Control') || currentServiceType.contains('Full')) {
        accessProxy = AccessControlUserServiceProxy(loggingProxy, role);
        currentService = accessProxy;
      }
    });
  }

  Future<void> _refreshUsersList() async {
    try {
      final users = await currentService.getAllUsers();
      setState(() {
        _users = users;
        _operationResult = 'Retrieved ${users.length} users';
      });
    } catch (e) {
      setState(() {
        _operationResult = 'Error: $e';
      });
    }
  }

  Future<void> _createUser() async {
    try {
      final user = await currentService.createUser({
        'name': 'New User ${DateTime.now().millisecond}',
        'email': 'newuser${DateTime.now().millisecond}@example.com',
        'role': 'viewer'
      });
      
      setState(() {
        _operationResult = 'Created user: ${user['name']}';
      });
      
      _refreshUsersList();
    } catch (e) {
      setState(() {
        _operationResult = 'Error creating user: $e';
      });
    }
  }

  Future<void> _deleteLastUser() async {
    if (_users.isNotEmpty) {
      try {
        final lastUser = _users.last;
        final success = await currentService.deleteUser(lastUser['id']);
        
        setState(() {
          _operationResult = success ? 'Deleted user: ${lastUser['name']}' : 'Delete failed';
        });
        
        if (success) {
          _refreshUsersList();
        }
      } catch (e) {
        setState(() {
          _operationResult = 'Error deleting user: $e';
        });
      }
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Delegation and Proxy Patterns'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Service Selection
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Service Selection',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    DropdownButton<String>(
                      value: currentServiceType,
                      isExpanded: true,
                      items: [
                        'Concrete Service',
                        'Caching Proxy',
                        'Logging Proxy',
                        'Access Control Proxy',
                        'Full Proxy Chain'
                      ].map((type) => DropdownMenuItem(
                        value: type,
                        child: Text(type),
                      )).toList(),
                      onChanged: (value) => _switchService(value!),
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        const Text('Role: '),
                        DropdownButton<String>(
                          value: currentRole,
                          items: ['admin', 'editor', 'viewer'].map((role) => DropdownMenuItem(
                            value: role,
                            child: Text(role),
                          )).toList(),
                          onChanged: (value) => _changeRole(value!),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Operations
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Operations',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: _refreshUsersList,
                          child: const Text('Refresh Users'),
                        ),
                        ElevatedButton(
                          onPressed: _createUser,
                          child: const Text('Create User'),
                        ),
                        ElevatedButton(
                          onPressed: _deleteLastUser,
                          child: const Text('Delete Last User'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Result
            if (_operationResult.isNotEmpty) ...[
              Card(
                color: _operationResult.startsWith('Error') ? Colors.red.shade900 : Colors.green.shade900,
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

            // Users List
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Users List',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    ..._users.map((user) => Padding(
                      padding: const EdgeInsets.symmetric(vertical: 4),
                      child: Text('${user['name']} (${user['email']}) - Role: ${user['role']}'),
                    )),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Proxy Information
            if (currentServiceType.contains('Logging') || currentServiceType.contains('Full')) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Operation Log (Logging Proxy)',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Container(
                        height: 150,
                        width: double.infinity,
                        padding: const EdgeInsets.all(8),
                        decoration: BoxDecoration(
                          color: Colors.grey.shade900,
                          borderRadius: BorderRadius.circular(4),
                          border: Border.all(color: Colors.grey.shade700),
                        ),
                        child: SingleChildScrollView(
                          child: Text(
                            loggingProxy.operationLog.take(10).join('\n'),
                            style: const TextStyle(fontSize: 10, fontFamily: 'monospace'),
                          ),
                        ),
                      ),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],

            if (currentServiceType.contains('Access Control') || currentServiceType.contains('Full')) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Access Control Info',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Text('Current Role: $currentRole'),
                      Text('Permissions: ${accessProxy.permissions.join(', ')}'),
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
                      'Delegation and Proxy Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Delegation: Proxy objects delegate calls to wrapped services'),
                    const Text('• Proxy Pattern: Controlled access to underlying objects'),
                    const Text('• Caching Proxy: Transparent performance optimization'),
                    const Text('• Logging Proxy: Cross-cutting concern implementation'),
                    const Text('• Access Control Proxy: Security and authorization layer'),
                    const Text('• Proxy Chaining: Multiple proxies working together'),
                    const Text('• Interface Consistency: All proxies implement same interface'),
                    const Text('• Transparent Enhancement: Adding features without changing clients'),
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

This example demonstrates delegation and proxy patterns for controlled  
object access. Proxy objects wrap the concrete service and add  
functionality like caching, logging, and access control while  
maintaining the same interface. Multiple proxies can be chained  
together for comprehensive cross-cutting concerns.

## Observer Pattern and Event-Driven Architecture

Implementing observer pattern for loose coupling and event notifications.

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
      title: 'Observer Pattern',
      theme: ThemeData.dark(),
      home: const ObserverDemo(),
    );
  }
}

// Observer interface
abstract class Observer<T> {
  void update(T data);
  String get observerName;
}

// Subject interface  
abstract class Subject<T> {
  void attach(Observer<T> observer);
  void detach(Observer<T> observer);
  void notify(T data);
}

// Event data classes
class StockPriceEvent {
  final String symbol;
  final double price;
  final double change;
  final double changePercent;
  final DateTime timestamp;

  StockPriceEvent({
    required this.symbol,
    required this.price,
    required this.change,
    required this.changePercent,
  }) : timestamp = DateTime.now();

  @override
  String toString() => 
    '$symbol: \$${price.toStringAsFixed(2)} (${change >= 0 ? '+' : ''}${change.toStringAsFixed(2)}, ${changePercent.toStringAsFixed(1)}%)';
}

// Concrete Subject - Stock Market
class StockMarket implements Subject<StockPriceEvent> {
  final List<Observer<StockPriceEvent>> _observers = [];
  final Map<String, double> _stockPrices = {
    'AAPL': 150.0,
    'GOOGL': 2500.0,
    'MSFT': 300.0,
    'TSLA': 800.0,
    'AMZN': 3200.0,
  };

  @override
  void attach(Observer<StockPriceEvent> observer) {
    if (!_observers.contains(observer)) {
      _observers.add(observer);
    }
  }

  @override
  void detach(Observer<StockPriceEvent> observer) {
    _observers.remove(observer);
  }

  @override
  void notify(StockPriceEvent event) {
    for (var observer in _observers) {
      observer.update(event);
    }
  }

  void updateStockPrice(String symbol, double newPrice) {
    if (_stockPrices.containsKey(symbol)) {
      final oldPrice = _stockPrices[symbol]!;
      final change = newPrice - oldPrice;
      final changePercent = (change / oldPrice) * 100;

      _stockPrices[symbol] = newPrice;

      final event = StockPriceEvent(
        symbol: symbol,
        price: newPrice,
        change: change,
        changePercent: changePercent,
      );

      notify(event);
    }
  }

  Map<String, double> get stockPrices => Map.unmodifiable(_stockPrices);
  List<Observer<StockPriceEvent>> get observers => List.unmodifiable(_observers);
}

// Concrete Observers
class PortfolioTracker implements Observer<StockPriceEvent> {
  @override
  final String observerName;
  final Map<String, int> _holdings = {};
  final List<String> _transactions = [];
  double _totalValue = 0.0;

  PortfolioTracker(this.observerName);

  void addHolding(String symbol, int shares) {
    _holdings[symbol] = (_holdings[symbol] ?? 0) + shares;
    _transactions.add('${DateTime.now().toString().substring(11, 19)}: Added $shares shares of $symbol');
  }

  @override
  void update(StockPriceEvent data) {
    if (_holdings.containsKey(data.symbol)) {
      final shares = _holdings[data.symbol]!;
      final value = shares * data.price;
      
      _transactions.add(
        '${DateTime.now().toString().substring(11, 19)}: ${data.symbol} updated - '
        '$shares shares worth \$${value.toStringAsFixed(2)} (${data.change >= 0 ? '+' : ''}${(shares * data.change).toStringAsFixed(2)})'
      );

      _updateTotalValue();
    }
  }

  void _updateTotalValue() {
    // This would normally calculate from current prices
    // Simplified for demo
    _totalValue += 100; // Simulate value change
  }

  Map<String, int> get holdings => Map.unmodifiable(_holdings);
  List<String> get transactions => List.unmodifiable(_transactions.reversed.take(5).toList());
  double get totalValue => _totalValue;
}

class AlertSystem implements Observer<StockPriceEvent> {
  @override
  final String observerName;
  final Map<String, double> _priceAlerts = {};
  final List<String> _alerts = [];

  AlertSystem(this.observerName);

  void setPriceAlert(String symbol, double targetPrice) {
    _priceAlerts[symbol] = targetPrice;
    _alerts.add('${DateTime.now().toString().substring(11, 19)}: Alert set for $symbol at \$${targetPrice.toStringAsFixed(2)}');
  }

  @override
  void update(StockPriceEvent data) {
    final alertPrice = _priceAlerts[data.symbol];
    if (alertPrice != null) {
      if ((data.change > 0 && data.price >= alertPrice) || 
          (data.change < 0 && data.price <= alertPrice)) {
        _alerts.add(
          '${DateTime.now().toString().substring(11, 19)}: 🚨 ALERT: ${data.symbol} '
          'reached \$${data.price.toStringAsFixed(2)} (target: \$${alertPrice.toStringAsFixed(2)})'
        );
        _priceAlerts.remove(data.symbol); // Remove triggered alert
      }
    }

    // Price change alerts
    if (data.changePercent.abs() > 5.0) {
      _alerts.add(
        '${DateTime.now().toString().substring(11, 19)}: 📈 ${data.symbol} '
        '${data.change > 0 ? 'SURGE' : 'DROP'}: ${data.changePercent.abs().toStringAsFixed(1)}%'
      );
    }
  }

  Map<String, double> get priceAlerts => Map.unmodifiable(_priceAlerts);
  List<String> get alerts => List.unmodifiable(_alerts.reversed.take(8).toList());
}

class NewsGenerator implements Observer<StockPriceEvent> {
  @override
  final String observerName;
  final List<String> _news = [];

  NewsGenerator(this.observerName);

  @override
  void update(StockPriceEvent data) {
    String headline;
    
    if (data.changePercent > 3.0) {
      headline = '📈 ${data.symbol} surges ${data.changePercent.toStringAsFixed(1)}% to \$${data.price.toStringAsFixed(2)}';
    } else if (data.changePercent < -3.0) {
      headline = '📉 ${data.symbol} drops ${data.changePercent.abs().toStringAsFixed(1)}% to \$${data.price.toStringAsFixed(2)}';
    } else {
      headline = '📊 ${data.symbol} moves to \$${data.price.toStringAsFixed(2)} (${data.changePercent >= 0 ? '+' : ''}${data.changePercent.toStringAsFixed(1)}%)';
    }

    _news.add('${DateTime.now().toString().substring(11, 19)}: $headline');
  }

  List<String> get news => List.unmodifiable(_news.reversed.take(6).toList());
}

class TradingBot implements Observer<StockPriceEvent> {
  @override
  final String observerName;
  final List<String> _trades = [];
  final Map<String, double> _buyThresholds = {};
  final Map<String, double> _sellThresholds = {};

  TradingBot(this.observerName);

  void setBuyThreshold(String symbol, double threshold) {
    _buyThresholds[symbol] = threshold;
  }

  void setSellThreshold(String symbol, double threshold) {
    _sellThresholds[symbol] = threshold;
  }

  @override
  void update(StockPriceEvent data) {
    final buyThreshold = _buyThresholds[data.symbol];
    final sellThreshold = _sellThresholds[data.symbol];

    // Buy signal
    if (buyThreshold != null && data.price <= buyThreshold && data.changePercent < -2.0) {
      _trades.add(
        '${DateTime.now().toString().substring(11, 19)}: 🤖 BUY ${data.symbol} at \$${data.price.toStringAsFixed(2)} '
        '(threshold: \$${buyThreshold.toStringAsFixed(2)})'
      );
    }

    // Sell signal
    if (sellThreshold != null && data.price >= sellThreshold && data.changePercent > 2.0) {
      _trades.add(
        '${DateTime.now().toString().substring(11, 19)}: 🤖 SELL ${data.symbol} at \$${data.price.toStringAsFixed(2)} '
        '(threshold: \$${sellThreshold.toStringAsFixed(2)})'
      );
    }
  }

  List<String> get trades => List.unmodifiable(_trades.reversed.take(5).toList());
  Map<String, double> get buyThresholds => Map.unmodifiable(_buyThresholds);
  Map<String, double> get sellThresholds => Map.unmodifiable(_sellThresholds);
}

class ObserverDemo extends StatefulWidget {
  const ObserverDemo({super.key});

  @override
  State<ObserverDemo> createState() => _ObserverDemoState();
}

class _ObserverDemoState extends State<ObserverDemo> {
  late StockMarket stockMarket;
  late PortfolioTracker portfolio;
  late AlertSystem alertSystem;
  late NewsGenerator newsGenerator;
  late TradingBot tradingBot;

  @override
  void initState() {
    super.initState();

    // Initialize the subject and observers
    stockMarket = StockMarket();
    portfolio = PortfolioTracker('My Portfolio');
    alertSystem = AlertSystem('Alert System');
    newsGenerator = NewsGenerator('Financial News');
    tradingBot = TradingBot('Trading Bot');

    // Attach observers to subject
    stockMarket.attach(portfolio);
    stockMarket.attach(alertSystem);
    stockMarket.attach(newsGenerator);
    stockMarket.attach(tradingBot);

    // Setup initial data
    portfolio.addHolding('AAPL', 100);
    portfolio.addHolding('GOOGL', 10);
    portfolio.addHolding('TSLA', 50);

    alertSystem.setPriceAlert('AAPL', 160.0);
    alertSystem.setPriceAlert('TSLA', 750.0);

    tradingBot.setBuyThreshold('MSFT', 280.0);
    tradingBot.setSellThreshold('AMZN', 3300.0);
  }

  void _simulateRandomPriceChange() {
    final symbols = stockMarket.stockPrices.keys.toList();
    final randomSymbol = symbols[DateTime.now().millisecond % symbols.length];
    final currentPrice = stockMarket.stockPrices[randomSymbol]!;
    
    // Generate random price change between -10% and +10%
    final changePercent = (DateTime.now().millisecond % 200 - 100) / 10.0; 
    final newPrice = currentPrice * (1 + changePercent / 100);
    
    stockMarket.updateStockPrice(randomSymbol, double.parse(newPrice.toStringAsFixed(2)));
  }

  void _simulateSpecificPriceChange(String symbol, double newPrice) {
    stockMarket.updateStockPrice(symbol, newPrice);
  }

  void _detachObserver(Observer<StockPriceEvent> observer) {
    setState(() {
      stockMarket.detach(observer);
    });
  }

  void _attachObserver(Observer<StockPriceEvent> observer) {
    setState(() {
      stockMarket.attach(observer);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Observer Pattern - Stock Market'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Stock Market Control
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Stock Market (Subject)',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Text('Active Observers: ${stockMarket.observers.length}'),
                    const SizedBox(height: 8),
                    ...stockMarket.stockPrices.entries.map((entry) =>
                      Text('${entry.key}: \$${entry.value.toStringAsFixed(2)}')
                    ),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: _simulateRandomPriceChange,
                          child: const Text('Random Price Change'),
                        ),
                        ElevatedButton(
                          onPressed: () => _simulateSpecificPriceChange('AAPL', 165.0),
                          child: const Text('AAPL +10%'),
                        ),
                        ElevatedButton(
                          onPressed: () => _simulateSpecificPriceChange('TSLA', 720.0),
                          child: const Text('TSLA -10%'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Observer Controls
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Observer Controls',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: stockMarket.observers.contains(portfolio) 
                              ? () => _detachObserver(portfolio)
                              : () => _attachObserver(portfolio),
                          child: Text(stockMarket.observers.contains(portfolio) 
                              ? 'Detach Portfolio' 
                              : 'Attach Portfolio'),
                        ),
                        ElevatedButton(
                          onPressed: stockMarket.observers.contains(alertSystem) 
                              ? () => _detachObserver(alertSystem)
                              : () => _attachObserver(alertSystem),
                          child: Text(stockMarket.observers.contains(alertSystem) 
                              ? 'Detach Alerts' 
                              : 'Attach Alerts'),
                        ),
                        ElevatedButton(
                          onPressed: stockMarket.observers.contains(tradingBot) 
                              ? () => _detachObserver(tradingBot)
                              : () => _attachObserver(tradingBot),
                          child: Text(stockMarket.observers.contains(tradingBot) 
                              ? 'Detach Bot' 
                              : 'Attach Bot'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Portfolio Observer
            Card(
              color: stockMarket.observers.contains(portfolio) 
                  ? Colors.green.shade900 
                  : Colors.grey.shade800,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Portfolio Tracker Observer ${stockMarket.observers.contains(portfolio) ? '(Active)' : '(Detached)'}',
                      style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Text('Holdings: ${portfolio.holdings.entries.map((e) => '${e.key}: ${e.value}').join(', ')}'),
                    const SizedBox(height: 8),
                    const Text('Recent Transactions:', style: TextStyle(fontWeight: FontWeight.bold)),
                    ...portfolio.transactions.map((transaction) => 
                      Text(transaction, style: const TextStyle(fontSize: 12))
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Alert System Observer
            Card(
              color: stockMarket.observers.contains(alertSystem) 
                  ? Colors.orange.shade900 
                  : Colors.grey.shade800,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Alert System Observer ${stockMarket.observers.contains(alertSystem) ? '(Active)' : '(Detached)'}',
                      style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Text('Active Alerts: ${alertSystem.priceAlerts.entries.map((e) => '${e.key}: \$${e.value}').join(', ')}'),
                    const SizedBox(height: 8),
                    const Text('Recent Alerts:', style: TextStyle(fontWeight: FontWeight.bold)),
                    ...alertSystem.alerts.map((alert) => 
                      Text(alert, style: const TextStyle(fontSize: 12))
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // News Generator Observer
            Card(
              color: stockMarket.observers.contains(newsGenerator) 
                  ? Colors.blue.shade900 
                  : Colors.grey.shade800,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'News Generator Observer ${stockMarket.observers.contains(newsGenerator) ? '(Active)' : '(Detached)'}',
                      style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('Recent News:', style: TextStyle(fontWeight: FontWeight.bold)),
                    ...newsGenerator.news.map((news) => 
                      Text(news, style: const TextStyle(fontSize: 12))
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Trading Bot Observer
            Card(
              color: stockMarket.observers.contains(tradingBot) 
                  ? Colors.purple.shade900 
                  : Colors.grey.shade800,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Trading Bot Observer ${stockMarket.observers.contains(tradingBot) ? '(Active)' : '(Detached)'}',
                      style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Text('Buy Thresholds: ${tradingBot.buyThresholds.entries.map((e) => '${e.key}: \$${e.value}').join(', ')}'),
                    Text('Sell Thresholds: ${tradingBot.sellThresholds.entries.map((e) => '${e.key}: \$${e.value}').join(', ')}'),
                    const SizedBox(height: 8),
                    const Text('Recent Trades:', style: TextStyle(fontWeight: FontWeight.bold)),
                    ...tradingBot.trades.map((trade) => 
                      Text(trade, style: const TextStyle(fontSize: 12))
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
                      'Observer Pattern Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Subject-Observer relationship: StockMarket notifies multiple observers'),
                    const Text('• Loose coupling: Observers don\'t know about each other'),
                    const Text('• Dynamic subscription: Observers can attach/detach at runtime'),
                    const Text('• Event-driven architecture: Price changes trigger notifications'),
                    const Text('• One-to-many communication: Single event, multiple handlers'),
                    const Text('• Push model: Subject pushes data to all observers'),
                    const Text('• Interface segregation: Observer interface defines contract'),
                    const Text('• Separation of concerns: Each observer handles specific functionality'),
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

This example demonstrates the observer pattern through a stock market  
simulation. The StockMarket subject maintains a list of observers and  
notifies them of price changes. Different observers (Portfolio,  
AlertSystem, NewsGenerator, TradingBot) respond to the same events in  
their own specialized ways, promoting loose coupling and separation  
of concerns.

## Strategy Pattern and Algorithm Selection

Implementing strategy pattern for interchangeable algorithms.

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
      title: 'Strategy Pattern',
      theme: ThemeData.dark(),
      home: const StrategyDemo(),
    );
  }
}

// Strategy interface for sorting algorithms
abstract class SortingStrategy {
  String get name;
  List<int> sort(List<int> data);
  String get description;
  String get timeComplexity;
}

// Concrete strategies
class BubbleSortStrategy implements SortingStrategy {
  @override
  String get name => 'Bubble Sort';
  
  @override
  String get description => 'Simple comparison-based algorithm that repeatedly steps through the list';
  
  @override
  String get timeComplexity => 'O(n²)';

  @override
  List<int> sort(List<int> data) {
    List<int> result = List.from(data);
    int n = result.length;
    
    for (int i = 0; i < n - 1; i++) {
      for (int j = 0; j < n - i - 1; j++) {
        if (result[j] > result[j + 1]) {
          int temp = result[j];
          result[j] = result[j + 1];
          result[j + 1] = temp;
        }
      }
    }
    
    return result;
  }
}

class QuickSortStrategy implements SortingStrategy {
  @override
  String get name => 'Quick Sort';
  
  @override
  String get description => 'Efficient divide-and-conquer algorithm using partitioning';
  
  @override
  String get timeComplexity => 'O(n log n) average, O(n²) worst';

  @override
  List<int> sort(List<int> data) {
    List<int> result = List.from(data);
    _quickSort(result, 0, result.length - 1);
    return result;
  }

  void _quickSort(List<int> arr, int low, int high) {
    if (low < high) {
      int pi = _partition(arr, low, high);
      _quickSort(arr, low, pi - 1);
      _quickSort(arr, pi + 1, high);
    }
  }

  int _partition(List<int> arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
      if (arr[j] < pivot) {
        i++;
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
      }
    }

    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;

    return i + 1;
  }
}

class MergeSortStrategy implements SortingStrategy {
  @override
  String get name => 'Merge Sort';
  
  @override
  String get description => 'Stable divide-and-conquer algorithm with consistent performance';
  
  @override
  String get timeComplexity => 'O(n log n)';

  @override
  List<int> sort(List<int> data) {
    List<int> result = List.from(data);
    _mergeSort(result, 0, result.length - 1);
    return result;
  }

  void _mergeSort(List<int> arr, int left, int right) {
    if (left < right) {
      int middle = (left + right) ~/ 2;
      _mergeSort(arr, left, middle);
      _mergeSort(arr, middle + 1, right);
      _merge(arr, left, middle, right);
    }
  }

  void _merge(List<int> arr, int left, int middle, int right) {
    List<int> leftArray = arr.sublist(left, middle + 1);
    List<int> rightArray = arr.sublist(middle + 1, right + 1);

    int i = 0, j = 0, k = left;

    while (i < leftArray.length && j < rightArray.length) {
      if (leftArray[i] <= rightArray[j]) {
        arr[k] = leftArray[i];
        i++;
      } else {
        arr[k] = rightArray[j];
        j++;
      }
      k++;
    }

    while (i < leftArray.length) {
      arr[k] = leftArray[i];
      i++;
      k++;
    }

    while (j < rightArray.length) {
      arr[k] = rightArray[j];
      j++;
      k++;
    }
  }
}

// Strategy context
class SortingContext {
  SortingStrategy _strategy;
  
  SortingContext(this._strategy);
  
  void setStrategy(SortingStrategy strategy) {
    _strategy = strategy;
  }
  
  List<int> executeSort(List<int> data) {
    return _strategy.sort(data);
  }
  
  String get currentStrategyName => _strategy.name;
  String get currentStrategyDescription => _strategy.description;
  String get currentTimeComplexity => _strategy.timeComplexity;
}

class StrategyDemo extends StatefulWidget {
  const StrategyDemo({super.key});

  @override
  State<StrategyDemo> createState() => _StrategyDemoState();
}

class _StrategyDemoState extends State<StrategyDemo> {
  late SortingContext sortingContext;
  final List<SortingStrategy> strategies = [
    BubbleSortStrategy(),
    QuickSortStrategy(),
    MergeSortStrategy(),
  ];
  
  List<int> originalData = [];
  List<int> sortedData = [];
  String operationResult = '';
  int operationTime = 0;

  @override
  void initState() {
    super.initState();
    sortingContext = SortingContext(strategies[0]);
    _generateRandomData();
  }

  void _generateRandomData() {
    final random = math.Random();
    originalData = List.generate(20, (index) => random.nextInt(100) + 1);
    setState(() {
      sortedData = [];
      operationResult = '';
      operationTime = 0;
    });
  }

  void _performSort() {
    final stopwatch = Stopwatch()..start();
    
    setState(() {
      sortedData = sortingContext.executeSort(originalData);
      operationTime = stopwatch.elapsedMicroseconds;
      operationResult = 'Sorted ${originalData.length} elements using ${sortingContext.currentStrategyName}';
    });
    
    stopwatch.stop();
  }

  void _changeStrategy(SortingStrategy strategy) {
    setState(() {
      sortingContext.setStrategy(strategy);
      sortedData = [];
      operationResult = '';
      operationTime = 0;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Strategy Pattern - Sorting Algorithms'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Strategy Selection
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Select Sorting Strategy',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    ...strategies.map((strategy) => RadioListTile<SortingStrategy>(
                      title: Text(strategy.name),
                      subtitle: Text('${strategy.timeComplexity} - ${strategy.description}'),
                      value: strategy,
                      groupValue: sortingContext._strategy,
                      onChanged: (value) => _changeStrategy(value!),
                    )),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Current Strategy Info
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Current Strategy',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Text('Algorithm: ${sortingContext.currentStrategyName}'),
                    Text('Time Complexity: ${sortingContext.currentTimeComplexity}'),
                    Text('Description: ${sortingContext.currentStrategyDescription}'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Data and Operations
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Data Operations',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        ElevatedButton(
                          onPressed: _generateRandomData,
                          child: const Text('Generate Random Data'),
                        ),
                        const SizedBox(width: 16),
                        ElevatedButton(
                          onPressed: _performSort,
                          child: const Text('Sort Data'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Original Data
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Original Data',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: originalData.map((number) => Container(
                        padding: const EdgeInsets.all(8),
                        decoration: BoxDecoration(
                          color: Colors.blue.shade800,
                          borderRadius: BorderRadius.circular(4),
                        ),
                        child: Text(number.toString()),
                      )).toList(),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Sorted Data
            if (sortedData.isNotEmpty) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Sorted Data',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Wrap(
                        spacing: 8,
                        runSpacing: 8,
                        children: sortedData.map((number) => Container(
                          padding: const EdgeInsets.all(8),
                          decoration: BoxDecoration(
                            color: Colors.green.shade800,
                            borderRadius: BorderRadius.circular(4),
                          ),
                          child: Text(number.toString()),
                        )).toList(),
                      ),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],

            // Operation Result
            if (operationResult.isNotEmpty) ...[
              Card(
                color: Colors.green.shade900,
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
                      Text(operationResult),
                      Text('Execution Time: ${operationTime} microseconds'),
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
                      'Strategy Pattern Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Strategy interface: Common contract for all algorithms'),
                    const Text('• Concrete strategies: Different sorting algorithm implementations'),
                    const Text('• Context class: SortingContext manages strategy selection'),
                    const Text('• Runtime strategy switching: Change algorithms dynamically'),
                    const Text('• Algorithm encapsulation: Each strategy encapsulates specific logic'),
                    const Text('• Open/Closed principle: Easy to add new strategies without modification'),
                    const Text('• Composition over inheritance: Context has-a strategy, not is-a strategy'),
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

This example demonstrates the strategy pattern through interchangeable  
sorting algorithms. The SortingContext can dynamically switch between  
different sorting strategies (Bubble Sort, Quick Sort, Merge Sort)  
without changing its interface. This promotes algorithm flexibility  
and follows the Open/Closed Principle.

## Command Pattern and Undo Operations

Implementing command pattern for encapsulating operations and undo functionality.

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
      title: 'Command Pattern',
      theme: ThemeData.dark(),
      home: const CommandDemo(),
    );
  }
}

// Command interface
abstract class Command {
  void execute();
  void undo();
  String get description;
}

// Receiver - Text Editor
class TextEditor {
  String _content = '';
  int _cursorPosition = 0;
  
  String get content => _content;
  int get cursorPosition => _cursorPosition;
  
  void insertText(String text, int position) {
    if (position >= 0 && position <= _content.length) {
      _content = _content.substring(0, position) + text + _content.substring(position);
      _cursorPosition = position + text.length;
    }
  }
  
  void deleteText(int startPosition, int length) {
    if (startPosition >= 0 && startPosition + length <= _content.length) {
      _content = _content.substring(0, startPosition) + _content.substring(startPosition + length);
      _cursorPosition = startPosition;
    }
  }
  
  void setCursorPosition(int position) {
    if (position >= 0 && position <= _content.length) {
      _cursorPosition = position;
    }
  }
  
  void replaceAll(String oldText, String newText) {
    _content = _content.replaceAll(oldText, newText);
  }
  
  void clear() {
    _content = '';
    _cursorPosition = 0;
  }
  
  void append(String text) {
    _content += text;
    _cursorPosition = _content.length;
  }
}

// Concrete Commands
class InsertTextCommand implements Command {
  final TextEditor _editor;
  final String _text;
  final int _position;
  
  InsertTextCommand(this._editor, this._text, this._position);
  
  @override
  void execute() {
    _editor.insertText(_text, _position);
  }
  
  @override
  void undo() {
    _editor.deleteText(_position, _text.length);
  }
  
  @override
  String get description => 'Insert "$_text" at position $_position';
}

class DeleteTextCommand implements Command {
  final TextEditor _editor;
  final int _startPosition;
  final int _length;
  String _deletedText = '';
  
  DeleteTextCommand(this._editor, this._startPosition, this._length);
  
  @override
  void execute() {
    _deletedText = _editor.content.substring(_startPosition, _startPosition + _length);
    _editor.deleteText(_startPosition, _length);
  }
  
  @override
  void undo() {
    _editor.insertText(_deletedText, _startPosition);
  }
  
  @override
  String get description => 'Delete $_length characters at position $_startPosition';
}

class ReplaceTextCommand implements Command {
  final TextEditor _editor;
  final String _oldText;
  final String _newText;
  String _originalContent = '';
  
  ReplaceTextCommand(this._editor, this._oldText, this._newText);
  
  @override
  void execute() {
    _originalContent = _editor.content;
    _editor.replaceAll(_oldText, _newText);
  }
  
  @override
  void undo() {
    _editor.clear();
    _editor.append(_originalContent);
  }
  
  @override
  String get description => 'Replace "$_oldText" with "$_newText"';
}

class ClearTextCommand implements Command {
  final TextEditor _editor;
  String _originalContent = '';
  int _originalPosition = 0;
  
  ClearTextCommand(this._editor);
  
  @override
  void execute() {
    _originalContent = _editor.content;
    _originalPosition = _editor.cursorPosition;
    _editor.clear();
  }
  
  @override
  void undo() {
    _editor.append(_originalContent);
    _editor.setCursorPosition(_originalPosition);
  }
  
  @override
  String get description => 'Clear all text';
}

// Macro Command - composite command
class MacroCommand implements Command {
  final List<Command> _commands;
  final String _description;
  
  MacroCommand(this._commands, this._description);
  
  @override
  void execute() {
    for (var command in _commands) {
      command.execute();
    }
  }
  
  @override
  void undo() {
    for (var command in _commands.reversed) {
      command.undo();
    }
  }
  
  @override
  String get description => _description;
}

// Command Invoker
class CommandManager {
  final List<Command> _history = [];
  int _currentPosition = -1;
  
  void executeCommand(Command command) {
    // Remove any commands after current position (for redo functionality)
    if (_currentPosition < _history.length - 1) {
      _history.removeRange(_currentPosition + 1, _history.length);
    }
    
    command.execute();
    _history.add(command);
    _currentPosition++;
  }
  
  bool canUndo() {
    return _currentPosition >= 0;
  }
  
  bool canRedo() {
    return _currentPosition < _history.length - 1;
  }
  
  void undo() {
    if (canUndo()) {
      _history[_currentPosition].undo();
      _currentPosition--;
    }
  }
  
  void redo() {
    if (canRedo()) {
      _currentPosition++;
      _history[_currentPosition].execute();
    }
  }
  
  List<String> get commandHistory {
    return _history.take(_currentPosition + 1).map((cmd) => cmd.description).toList();
  }
  
  void clearHistory() {
    _history.clear();
    _currentPosition = -1;
  }
  
  int get historySize => _history.length;
  int get currentPosition => _currentPosition;
}

class CommandDemo extends StatefulWidget {
  const CommandDemo({super.key});

  @override
  State<CommandDemo> createState() => _CommandDemoState();
}

class _CommandDemoState extends State<CommandDemo> {
  final TextEditor editor = TextEditor();
  final CommandManager commandManager = CommandManager();
  final TextEditingController _inputController = TextEditingController();
  final TextEditingController _replaceOldController = TextEditingController();
  final TextEditingController _replaceNewController = TextEditingController();

  void _executeCommand(Command command) {
    setState(() {
      commandManager.executeCommand(command);
    });
  }

  void _undo() {
    setState(() {
      commandManager.undo();
    });
  }

  void _redo() {
    setState(() {
      commandManager.redo();
    });
  }

  void _insertText() {
    if (_inputController.text.isNotEmpty) {
      final command = InsertTextCommand(
        editor, 
        _inputController.text, 
        editor.cursorPosition
      );
      _executeCommand(command);
      _inputController.clear();
    }
  }

  void _deleteText() {
    if (editor.content.isNotEmpty && editor.cursorPosition > 0) {
      final command = DeleteTextCommand(editor, editor.cursorPosition - 1, 1);
      _executeCommand(command);
    }
  }

  void _replaceText() {
    if (_replaceOldController.text.isNotEmpty) {
      final command = ReplaceTextCommand(
        editor,
        _replaceOldController.text,
        _replaceNewController.text,
      );
      _executeCommand(command);
      _replaceOldController.clear();
      _replaceNewController.clear();
    }
  }

  void _clearText() {
    final command = ClearTextCommand(editor);
    _executeCommand(command);
  }

  void _executeMacro() {
    final commands = [
      InsertTextCommand(editor, 'Hello ', editor.cursorPosition),
      InsertTextCommand(editor, 'World! ', editor.cursorPosition + 6),
      InsertTextCommand(editor, 'This is a macro command.', editor.cursorPosition + 13),
    ];
    
    final macroCommand = MacroCommand(commands, 'Insert greeting macro');
    _executeCommand(macroCommand);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Command Pattern - Text Editor'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Text Editor Display
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Text Editor',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Container(
                      width: double.infinity,
                      height: 120,
                      padding: const EdgeInsets.all(12),
                      decoration: BoxDecoration(
                        color: Colors.grey.shade900,
                        borderRadius: BorderRadius.circular(8),
                        border: Border.all(color: Colors.grey.shade700),
                      ),
                      child: Text(
                        editor.content.isEmpty ? '(empty)' : editor.content,
                        style: const TextStyle(fontFamily: 'monospace'),
                      ),
                    ),
                    const SizedBox(height: 8),
                    Text('Cursor Position: ${editor.cursorPosition}'),
                    Text('Content Length: ${editor.content.length}'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Command Controls
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Command Controls',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    
                    // Insert Text
                    Row(
                      children: [
                        Expanded(
                          child: TextField(
                            controller: _inputController,
                            decoration: const InputDecoration(
                              labelText: 'Text to insert',
                              border: OutlineInputBorder(),
                            ),
                          ),
                        ),
                        const SizedBox(width: 8),
                        ElevatedButton(
                          onPressed: _insertText,
                          child: const Text('Insert'),
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),

                    // Replace Text
                    Row(
                      children: [
                        Expanded(
                          child: TextField(
                            controller: _replaceOldController,
                            decoration: const InputDecoration(
                              labelText: 'Find',
                              border: OutlineInputBorder(),
                            ),
                          ),
                        ),
                        const SizedBox(width: 8),
                        Expanded(
                          child: TextField(
                            controller: _replaceNewController,
                            decoration: const InputDecoration(
                              labelText: 'Replace with',
                              border: OutlineInputBorder(),
                            ),
                          ),
                        ),
                        const SizedBox(width: 8),
                        ElevatedButton(
                          onPressed: _replaceText,
                          child: const Text('Replace'),
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),

                    // Other Commands
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: _deleteText,
                          child: const Text('Delete Char'),
                        ),
                        ElevatedButton(
                          onPressed: _clearText,
                          child: const Text('Clear All'),
                        ),
                        ElevatedButton(
                          onPressed: _executeMacro,
                          child: const Text('Execute Macro'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Undo/Redo Controls
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Undo/Redo Controls',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        ElevatedButton(
                          onPressed: commandManager.canUndo() ? _undo : null,
                          child: const Text('Undo'),
                        ),
                        const SizedBox(width: 16),
                        ElevatedButton(
                          onPressed: commandManager.canRedo() ? _redo : null,
                          child: const Text('Redo'),
                        ),
                        const SizedBox(width: 16),
                        ElevatedButton(
                          onPressed: () {
                            setState(() {
                              commandManager.clearHistory();
                            });
                          },
                          child: const Text('Clear History'),
                        ),
                      ],
                    ),
                    const SizedBox(height: 8),
                    Text('Can Undo: ${commandManager.canUndo()}'),
                    Text('Can Redo: ${commandManager.canRedo()}'),
                    Text('History Size: ${commandManager.historySize}'),
                    Text('Current Position: ${commandManager.currentPosition}'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Command History
            if (commandManager.commandHistory.isNotEmpty) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Command History',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Container(
                        height: 150,
                        width: double.infinity,
                        padding: const EdgeInsets.all(8),
                        decoration: BoxDecoration(
                          color: Colors.grey.shade900,
                          borderRadius: BorderRadius.circular(4),
                          border: Border.all(color: Colors.grey.shade700),
                        ),
                        child: SingleChildScrollView(
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: commandManager.commandHistory.asMap().entries.map((entry) {
                              int index = entry.key;
                              String command = entry.value;
                              bool isCurrent = index == commandManager.currentPosition;
                              
                              return Padding(
                                padding: const EdgeInsets.symmetric(vertical: 1),
                                child: Text(
                                  '${index + 1}. $command${isCurrent ? ' ← Current' : ''}',
                                  style: TextStyle(
                                    fontSize: 12,
                                    fontFamily: 'monospace',
                                    color: isCurrent ? Colors.yellow : null,
                                    fontWeight: isCurrent ? FontWeight.bold : FontWeight.normal,
                                  ),
                                ),
                              );
                            }).toList(),
                          ),
                        ),
                      ),
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
                      'Command Pattern Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Command interface: Common contract for all operations'),
                    const Text('• Concrete commands: Specific operations (Insert, Delete, Replace)'),
                    const Text('• Receiver: TextEditor that performs actual operations'),
                    const Text('• Invoker: CommandManager that executes and tracks commands'),
                    const Text('• Undo functionality: Commands can reverse their operations'),
                    const Text('• Command history: Track and navigate through executed commands'),
                    const Text('• Macro commands: Composite commands combining multiple operations'),
                    const Text('• Decoupling: Commands decouple invoker from receiver'),
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

This example demonstrates the command pattern through a text editor  
with undo/redo functionality. Commands encapsulate operations as  
objects, enabling undo operations, command history tracking, and  
macro commands. The pattern decouples the invoker (CommandManager)  
from the receiver (TextEditor) and provides flexible operation  
management.

## State Pattern and State Machines

Implementing state pattern for managing object behavior based on state.

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
      title: 'State Pattern',
      theme: ThemeData.dark(),
      home: const StatePatternDemo(),
    );
  }
}

// State interface
abstract class VendingMachineState {
  String get stateName;
  String insertCoin(VendingMachine machine, double amount);
  String selectProduct(VendingMachine machine, String product);
  String dispenseProduct(VendingMachine machine);
  String cancel(VendingMachine machine);
}

// Context - Vending Machine
class VendingMachine {
  VendingMachineState _state = IdleState();
  double _insertedAmount = 0.0;
  String? _selectedProduct;
  final Map<String, double> _products = {
    'Coke': 1.50,
    'Water': 1.00,
    'Chips': 2.00,
    'Candy': 1.25,
  };
  final Map<String, int> _inventory = {
    'Coke': 5,
    'Water': 3,
    'Chips': 2,
    'Candy': 4,
  };

  void setState(VendingMachineState state) {
    _state = state;
  }

  String insertCoin(double amount) => _state.insertCoin(this, amount);
  String selectProduct(String product) => _state.selectProduct(this, product);
  String dispenseProduct() => _state.dispenseProduct(this);
  String cancel() => _state.cancel(this);

  // Getters and setters
  String get currentState => _state.stateName;
  double get insertedAmount => _insertedAmount;
  String? get selectedProduct => _selectedProduct;
  Map<String, double> get products => Map.unmodifiable(_products);
  Map<String, int> get inventory => Map.unmodifiable(_inventory);

  void addAmount(double amount) => _insertedAmount += amount;
  void resetAmount() => _insertedAmount = 0.0;
  void setSelectedProduct(String? product) => _selectedProduct = product;
  
  bool hasProduct(String product) => _inventory[product] != null && _inventory[product]! > 0;
  double getProductPrice(String product) => _products[product] ?? 0.0;
  
  void decrementInventory(String product) {
    if (_inventory[product] != null && _inventory[product]! > 0) {
      _inventory[product] = _inventory[product]! - 1;
    }
  }
}

// Concrete States
class IdleState implements VendingMachineState {
  @override
  String get stateName => 'Idle';

  @override
  String insertCoin(VendingMachine machine, double amount) {
    machine.addAmount(amount);
    machine.setState(HasMoneyState());
    return 'Inserted \$${amount.toStringAsFixed(2)}. Total: \$${machine.insertedAmount.toStringAsFixed(2)}';
  }

  @override
  String selectProduct(VendingMachine machine, String product) {
    return 'Please insert money first';
  }

  @override
  String dispenseProduct(VendingMachine machine) {
    return 'No product selected';
  }

  @override
  String cancel(VendingMachine machine) {
    return 'Nothing to cancel';
  }
}

class HasMoneyState implements VendingMachineState {
  @override
  String get stateName => 'Has Money';

  @override
  String insertCoin(VendingMachine machine, double amount) {
    machine.addAmount(amount);
    return 'Added \$${amount.toStringAsFixed(2)}. Total: \$${machine.insertedAmount.toStringAsFixed(2)}';
  }

  @override
  String selectProduct(VendingMachine machine, String product) {
    if (!machine.hasProduct(product)) {
      return '$product is out of stock';
    }
    
    double price = machine.getProductPrice(product);
    if (machine.insertedAmount < price) {
      return 'Insufficient funds. Need \$${(price - machine.insertedAmount).toStringAsFixed(2)} more';
    }
    
    machine.setSelectedProduct(product);
    machine.setState(ProductSelectedState());
    return 'Selected $product (\$${price.toStringAsFixed(2)})';
  }

  @override
  String dispenseProduct(VendingMachine machine) {
    return 'Please select a product first';
  }

  @override
  String cancel(VendingMachine machine) {
    double refund = machine.insertedAmount;
    machine.resetAmount();
    machine.setState(IdleState());
    return 'Transaction cancelled. Refunded \$${refund.toStringAsFixed(2)}';
  }
}

class ProductSelectedState implements VendingMachineState {
  @override
  String get stateName => 'Product Selected';

  @override
  String insertCoin(VendingMachine machine, double amount) {
    machine.addAmount(amount);
    return 'Added \$${amount.toStringAsFixed(2)}. Total: \$${machine.insertedAmount.toStringAsFixed(2)}';
  }

  @override
  String selectProduct(VendingMachine machine, String product) {
    if (!machine.hasProduct(product)) {
      return '$product is out of stock';
    }
    
    double price = machine.getProductPrice(product);
    if (machine.insertedAmount < price) {
      return 'Insufficient funds for $product';
    }
    
    machine.setSelectedProduct(product);
    return 'Changed selection to $product (\$${price.toStringAsFixed(2)})';
  }

  @override
  String dispenseProduct(VendingMachine machine) {
    String product = machine.selectedProduct!;
    double price = machine.getProductPrice(product);
    double change = machine.insertedAmount - price;
    
    machine.decrementInventory(product);
    machine.resetAmount();
    machine.setSelectedProduct(null);
    machine.setState(DispensingState());
    
    return 'Dispensing $product. Change: \$${change.toStringAsFixed(2)}';
  }

  @override
  String cancel(VendingMachine machine) {
    double refund = machine.insertedAmount;
    machine.resetAmount();
    machine.setSelectedProduct(null);
    machine.setState(IdleState());
    return 'Selection cancelled. Refunded \$${refund.toStringAsFixed(2)}';
  }
}

class DispensingState implements VendingMachineState {
  @override
  String get stateName => 'Dispensing';

  @override
  String insertCoin(VendingMachine machine, double amount) {
    return 'Please wait, dispensing product...';
  }

  @override
  String selectProduct(VendingMachine machine, String product) {
    return 'Please wait, dispensing product...';
  }

  @override
  String dispenseProduct(VendingMachine machine) {
    // Simulate dispensing complete
    machine.setState(IdleState());
    return 'Product dispensed. Thank you!';
  }

  @override
  String cancel(VendingMachine machine) {
    return 'Cannot cancel during dispensing';
  }
}

class OutOfOrderState implements VendingMachineState {
  @override
  String get stateName => 'Out of Order';

  @override
  String insertCoin(VendingMachine machine, double amount) {
    return 'Machine is out of order';
  }

  @override
  String selectProduct(VendingMachine machine, String product) {
    return 'Machine is out of order';
  }

  @override
  String dispenseProduct(VendingMachine machine) {
    return 'Machine is out of order';
  }

  @override
  String cancel(VendingMachine machine) {
    return 'Machine is out of order';
  }
}

class StatePatternDemo extends StatefulWidget {
  const StatePatternDemo({super.key});

  @override
  State<StatePatternDemo> createState() => _StatePatternDemoState();
}

class _StatePatternDemoState extends State<StatePatternDemo> {
  final VendingMachine machine = VendingMachine();
  String _lastAction = '';
  List<String> _actionLog = [];

  void _performAction(String action, [dynamic param]) {
    String result = '';
    
    switch (action) {
      case 'insert_coin':
        result = machine.insertCoin(param as double);
        break;
      case 'select_product':
        result = machine.selectProduct(param as String);
        break;
      case 'dispense':
        result = machine.dispenseProduct();
        break;
      case 'cancel':
        result = machine.cancel();
        break;
      case 'out_of_order':
        machine.setState(OutOfOrderState());
        result = 'Machine set to out of order';
        break;
      case 'reset':
        machine.setState(IdleState());
        machine.resetAmount();
        machine.setSelectedProduct(null);
        result = 'Machine reset to idle state';
        break;
    }

    setState(() {
      _lastAction = result;
      _actionLog.insert(0, '${DateTime.now().toString().substring(11, 19)}: $result');
      if (_actionLog.length > 8) {
        _actionLog.removeLast();
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('State Pattern - Vending Machine'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Machine Status
            Card(
              color: machine.currentState == 'Out of Order' 
                  ? Colors.red.shade900 
                  : Colors.blue.shade900,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Vending Machine Status',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    Text('State: ${machine.currentState}'),
                    Text('Inserted Amount: \$${machine.insertedAmount.toStringAsFixed(2)}'),
                    Text('Selected Product: ${machine.selectedProduct ?? 'None'}'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Products and Inventory
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Products & Inventory',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    ...machine.products.entries.map((product) => Padding(
                      padding: const EdgeInsets.symmetric(vertical: 4),
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text('${product.key}: \$${product.value.toStringAsFixed(2)}'),
                          Text('Stock: ${machine.inventory[product.key]}'),
                          ElevatedButton(
                            onPressed: () => _performAction('select_product', product.key),
                            child: const Text('Select'),
                          ),
                        ],
                      ),
                    )),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Coin Operations
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Coin Operations',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performAction('insert_coin', 0.25),
                          child: const Text('Insert \$0.25'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performAction('insert_coin', 0.50),
                          child: const Text('Insert \$0.50'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performAction('insert_coin', 1.00),
                          child: const Text('Insert \$1.00'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performAction('cancel'),
                          child: const Text('Cancel/Refund'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Machine Operations
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Machine Operations',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        ElevatedButton(
                          onPressed: () => _performAction('dispense'),
                          child: const Text('Dispense Product'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performAction('out_of_order'),
                          style: ElevatedButton.styleFrom(backgroundColor: Colors.red),
                          child: const Text('Set Out of Order'),
                        ),
                        ElevatedButton(
                          onPressed: () => _performAction('reset'),
                          style: ElevatedButton.styleFrom(backgroundColor: Colors.green),
                          child: const Text('Reset Machine'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Last Action
            if (_lastAction.isNotEmpty) ...[
              Card(
                color: _lastAction.contains('out of order') || _lastAction.contains('Insufficient') 
                    ? Colors.red.shade900 
                    : Colors.green.shade900,
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
                      Text(_lastAction),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],

            // Action Log
            if (_actionLog.isNotEmpty) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Action Log',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Container(
                        height: 120,
                        width: double.infinity,
                        padding: const EdgeInsets.all(8),
                        decoration: BoxDecoration(
                          color: Colors.grey.shade900,
                          borderRadius: BorderRadius.circular(4),
                          border: Border.all(color: Colors.grey.shade700),
                        ),
                        child: SingleChildScrollView(
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: _actionLog.map((log) => Padding(
                              padding: const EdgeInsets.only(bottom: 2),
                              child: Text(
                                log,
                                style: const TextStyle(fontSize: 11, fontFamily: 'monospace'),
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
            ],

            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'State Pattern Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• State interface: Common contract for all states'),
                    const Text('• Concrete states: IdleState, HasMoneyState, ProductSelectedState, etc.'),
                    const Text('• Context object: VendingMachine manages current state'),
                    const Text('• State transitions: Dynamic behavior changes based on state'),
                    const Text('• State-specific behavior: Same action produces different results'),
                    const Text('• State encapsulation: Each state handles its own logic'),
                    const Text('• Finite state machine: Clear state transitions and rules'),
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

This example demonstrates the state pattern through a vending machine  
simulation. Different states (Idle, HasMoney, ProductSelected,  
Dispensing, OutOfOrder) handle the same operations differently,  
enabling complex state-dependent behavior while maintaining clean  
code organization.

## Template Method Pattern

Implementing template method for algorithm structure with customizable steps.

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
      title: 'Template Method Pattern',
      theme: ThemeData.dark(),
      home: const TemplateMethodDemo(),
    );
  }
}

// Abstract template class
abstract class DataProcessor {
  final String processorName;
  final List<String> _processingLog = [];

  DataProcessor(this.processorName);

  // Template method - defines algorithm structure
  Future<Map<String, dynamic>> processData(List<Map<String, dynamic>> data) async {
    _log('Starting data processing');
    
    // Step 1: Validate input
    if (!validateInput(data)) {
      _log('Input validation failed');
      return {'success': false, 'error': 'Invalid input data'};
    }
    _log('Input validation passed');

    // Step 2: Preprocess data
    List<Map<String, dynamic>> preprocessedData = preprocessData(data);
    _log('Data preprocessing completed');

    // Step 3: Transform data (abstract method)
    List<Map<String, dynamic>> transformedData = await transformData(preprocessedData);
    _log('Data transformation completed');

    // Step 4: Validate output
    if (!validateOutput(transformedData)) {
      _log('Output validation failed');
      return {'success': false, 'error': 'Invalid output data'};
    }
    _log('Output validation passed');

    // Step 5: Post-process results
    Map<String, dynamic> result = postProcessResults(transformedData);
    _log('Post-processing completed');

    // Step 6: Generate report (hook method)
    String? report = generateReport(result);
    if (report != null) {
      result['report'] = report;
      _log('Report generated');
    }

    _log('Processing completed successfully');
    return result;
  }

  // Hook method with default implementation (can be overridden)
  bool validateInput(List<Map<String, dynamic>> data) {
    return data.isNotEmpty;
  }

  // Concrete method with default implementation
  List<Map<String, dynamic>> preprocessData(List<Map<String, dynamic>> data) {
    _log('Applying default preprocessing');
    return data.map((item) => Map<String, dynamic>.from(item)).toList();
  }

  // Abstract method - must be implemented by subclasses
  Future<List<Map<String, dynamic>>> transformData(List<Map<String, dynamic>> data);

  // Hook method with default implementation
  bool validateOutput(List<Map<String, dynamic>> data) {
    return data.isNotEmpty;
  }

  // Concrete method with default implementation
  Map<String, dynamic> postProcessResults(List<Map<String, dynamic>> data) {
    _log('Applying default post-processing');
    return {
      'success': true,
      'processedCount': data.length,
      'data': data,
      'processor': processorName,
      'timestamp': DateTime.now().toIso8601String(),
    };
  }

  // Hook method - optional implementation
  String? generateReport(Map<String, dynamic> result) {
    return null; // Default: no report
  }

  // Utility method
  void _log(String message) {
    _processingLog.add('${DateTime.now().toString().substring(11, 19)}: $message');
  }

  List<String> get processingLog => List.unmodifiable(_processingLog);
  void clearLog() => _processingLog.clear();
}

// Concrete implementation for statistical processing
class StatisticalProcessor extends DataProcessor {
  StatisticalProcessor() : super('Statistical Processor');

  @override
  bool validateInput(List<Map<String, dynamic>> data) {
    // More strict validation for statistical data
    return data.isNotEmpty && 
           data.every((item) => item.containsKey('value') && item['value'] is num);
  }

  @override
  List<Map<String, dynamic>> preprocessData(List<Map<String, dynamic>> data) {
    _log('Statistical preprocessing: normalizing values');
    return data.map((item) {
      double value = (item['value'] as num).toDouble();
      return {
        ...item,
        'value': value,
        'normalized': value / 100.0, // Simple normalization
      };
    }).toList();
  }

  @override
  Future<List<Map<String, dynamic>>> transformData(List<Map<String, dynamic>> data) async {
    _log('Performing statistical transformations');
    await Future.delayed(const Duration(milliseconds: 500)); // Simulate processing

    List<double> values = data.map((item) => item['value'] as double).toList();
    double mean = values.reduce((a, b) => a + b) / values.length;
    double sum = values.reduce((a, b) => a + b);
    double min = values.reduce((a, b) => a < b ? a : b);
    double max = values.reduce((a, b) => a > b ? a : b);

    return data.map((item) {
      double value = item['value'] as double;
      return {
        ...item,
        'deviation_from_mean': value - mean,
        'percentage_of_total': (value / sum) * 100,
        'is_outlier': (value - mean).abs() > (max - min) * 0.3,
      };
    }).toList();
  }

  @override
  String? generateReport(Map<String, dynamic> result) {
    List<Map<String, dynamic>> data = result['data'] as List<Map<String, dynamic>>;
    List<double> values = data.map((item) => item['value'] as double).toList();
    
    double mean = values.reduce((a, b) => a + b) / values.length;
    double min = values.reduce((a, b) => a < b ? a : b);
    double max = values.reduce((a, b) => a > b ? a : b);
    int outliers = data.where((item) => item['is_outlier'] == true).length;

    return '''Statistical Analysis Report:
- Total values: ${values.length}
- Mean: ${mean.toStringAsFixed(2)}
- Min: ${min.toStringAsFixed(2)}
- Max: ${max.toStringAsFixed(2)}
- Range: ${(max - min).toStringAsFixed(2)}
- Outliers detected: $outliers''';
  }
}

// Concrete implementation for text processing
class TextProcessor extends DataProcessor {
  TextProcessor() : super('Text Processor');

  @override
  bool validateInput(List<Map<String, dynamic>> data) {
    return data.isNotEmpty && 
           data.every((item) => item.containsKey('text') && item['text'] is String);
  }

  @override
  List<Map<String, dynamic>> preprocessData(List<Map<String, dynamic>> data) {
    _log('Text preprocessing: cleaning and normalizing');
    return data.map((item) {
      String text = item['text'] as String;
      return {
        ...item,
        'text': text,
        'cleaned_text': text.toLowerCase().trim(),
        'original_length': text.length,
      };
    }).toList();
  }

  @override
  Future<List<Map<String, dynamic>>> transformData(List<Map<String, dynamic>> data) async {
    _log('Performing text analysis transformations');
    await Future.delayed(const Duration(milliseconds: 300));

    return data.map((item) {
      String text = item['cleaned_text'] as String;
      List<String> words = text.split(RegExp(r'\s+'));
      
      return {
        ...item,
        'word_count': words.length,
        'character_count': text.length,
        'sentence_count': text.split(RegExp(r'[.!?]+')).where((s) => s.isNotEmpty).length,
        'average_word_length': words.isNotEmpty 
            ? words.map((w) => w.length).reduce((a, b) => a + b) / words.length 
            : 0.0,
        'contains_numbers': RegExp(r'\d').hasMatch(text),
      };
    }).toList();
  }

  @override
  String? generateReport(Map<String, dynamic> result) {
    List<Map<String, dynamic>> data = result['data'] as List<Map<String, dynamic>>;
    
    int totalWords = data.fold(0, (sum, item) => sum + (item['word_count'] as int));
    int totalChars = data.fold(0, (sum, item) => sum + (item['character_count'] as int));
    double avgWordsPerText = data.isNotEmpty ? totalWords / data.length : 0.0;
    int textsWithNumbers = data.where((item) => item['contains_numbers'] == true).length;

    return '''Text Analysis Report:
- Total texts processed: ${data.length}
- Total words: $totalWords
- Total characters: $totalChars
- Average words per text: ${avgWordsPerText.toStringAsFixed(1)}
- Texts containing numbers: $textsWithNumbers
- Average text length: ${data.isNotEmpty ? totalChars / data.length : 0} characters''';
  }
}

// Concrete implementation for image metadata processing
class ImageMetadataProcessor extends DataProcessor {
  ImageMetadataProcessor() : super('Image Metadata Processor');

  @override
  bool validateInput(List<Map<String, dynamic>> data) {
    return data.isNotEmpty && 
           data.every((item) => 
               item.containsKey('filename') && 
               item.containsKey('width') && 
               item.containsKey('height'));
  }

  @override
  List<Map<String, dynamic>> preprocessData(List<Map<String, dynamic>> data) {
    _log('Image metadata preprocessing');
    return data.map((item) {
      int width = item['width'] as int;
      int height = item['height'] as int;
      return {
        ...item,
        'aspect_ratio': width / height,
        'total_pixels': width * height,
        'is_landscape': width > height,
        'is_square': width == height,
      };
    }).toList();
  }

  @override
  Future<List<Map<String, dynamic>>> transformData(List<Map<String, dynamic>> data) async {
    _log('Processing image metadata transformations');
    await Future.delayed(const Duration(milliseconds: 400));

    return data.map((item) {
      int pixels = item['total_pixels'] as int;
      double aspectRatio = item['aspect_ratio'] as double;
      
      String resolution;
      if (pixels > 8000000) resolution = 'Ultra High';
      else if (pixels > 2000000) resolution = 'High';
      else if (pixels > 500000) resolution = 'Medium';
      else resolution = 'Low';
      
      String format;
      if (aspectRatio > 1.7) format = 'Wide';
      else if (aspectRatio > 1.2) format = 'Standard';
      else if (aspectRatio > 0.8) format = 'Square';
      else format = 'Tall';

      return {
        ...item,
        'resolution_category': resolution,
        'format_category': format,
        'size_mb': (pixels * 3) / (1024 * 1024), // Rough estimate for RGB
      };
    }).toList();
  }

  @override
  bool validateOutput(List<Map<String, dynamic>> data) {
    return super.validateOutput(data) && 
           data.every((item) => 
               item.containsKey('resolution_category') && 
               item.containsKey('format_category'));
  }

  @override
  String? generateReport(Map<String, dynamic> result) {
    List<Map<String, dynamic>> data = result['data'] as List<Map<String, dynamic>>;
    
    Map<String, int> resolutions = {};
    Map<String, int> formats = {};
    double totalSizeMb = 0;
    
    for (var item in data) {
      String resolution = item['resolution_category'] as String;
      String format = item['format_category'] as String;
      double sizeMb = item['size_mb'] as double;
      
      resolutions[resolution] = (resolutions[resolution] ?? 0) + 1;
      formats[format] = (formats[format] ?? 0) + 1;
      totalSizeMb += sizeMb;
    }

    return '''Image Metadata Analysis Report:
- Total images: ${data.length}
- Total estimated size: ${totalSizeMb.toStringAsFixed(1)} MB
- Resolution distribution: ${resolutions.entries.map((e) => '${e.key}: ${e.value}').join(', ')}
- Format distribution: ${formats.entries.map((e) => '${e.key}: ${e.value}').join(', ')}
- Average size per image: ${data.isNotEmpty ? (totalSizeMb / data.length).toStringAsFixed(1) : 0} MB''';
  }
}

class TemplateMethodDemo extends StatefulWidget {
  const TemplateMethodDemo({super.key});

  @override
  State<TemplateMethodDemo> createState() => _TemplateMethodDemoState();
}

class _TemplateMethodDemoState extends State<TemplateMethodDemo> {
  DataProcessor? currentProcessor;
  Map<String, dynamic>? processingResult;
  bool isProcessing = false;

  final List<DataProcessor> processors = [
    StatisticalProcessor(),
    TextProcessor(),
    ImageMetadataProcessor(),
  ];

  final Map<String, List<Map<String, dynamic>>> sampleData = {
    'Statistical Processor': [
      {'id': 1, 'value': 85, 'category': 'A'},
      {'id': 2, 'value': 92, 'category': 'B'},
      {'id': 3, 'value': 78, 'category': 'A'},
      {'id': 4, 'value': 95, 'category': 'C'},
      {'id': 5, 'value': 67, 'category': 'B'},
    ],
    'Text Processor': [
      {'id': 1, 'text': 'Hello world! This is a sample text.', 'author': 'Alice'},
      {'id': 2, 'text': 'Flutter development is fun and engaging.', 'author': 'Bob'},
      {'id': 3, 'text': 'Object-oriented programming concepts.', 'author': 'Charlie'},
      {'id': 4, 'text': 'Template method pattern example with text processing.', 'author': 'Diana'},
    ],
    'Image Metadata Processor': [
      {'filename': 'photo1.jpg', 'width': 1920, 'height': 1080, 'format': 'JPEG'},
      {'filename': 'image2.png', 'width': 800, 'height': 600, 'format': 'PNG'},
      {'filename': 'pic3.jpg', 'width': 1080, 'height': 1920, 'format': 'JPEG'},
      {'filename': 'logo.png', 'width': 500, 'height': 500, 'format': 'PNG'},
    ],
  };

  Future<void> _processData(DataProcessor processor) async {
    setState(() {
      currentProcessor = processor;
      isProcessing = true;
      processingResult = null;
    });

    processor.clearLog();
    final data = sampleData[processor.processorName] ?? [];
    final result = await processor.processData(data);

    setState(() {
      processingResult = result;
      isProcessing = false;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Template Method Pattern'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Processor Selection
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Select Data Processor',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 12),
                    ...processors.map((processor) => Padding(
                      padding: const EdgeInsets.symmetric(vertical: 4),
                      child: ElevatedButton(
                        onPressed: isProcessing ? null : () => _processData(processor),
                        child: Text('Process with ${processor.processorName}'),
                      ),
                    )),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Sample Data Display
            if (currentProcessor != null) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Sample Data for ${currentProcessor!.processorName}',
                        style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
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
                          sampleData[currentProcessor!.processorName].toString(),
                          style: const TextStyle(fontSize: 11, fontFamily: 'monospace'),
                        ),
                      ),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],

            // Processing Status
            if (isProcessing) ...[
              const Card(
                color: Colors.orange,
                child: Padding(
                  padding: EdgeInsets.all(16),
                  child: Row(
                    children: [
                      CircularProgressIndicator(),
                      SizedBox(width: 16),
                      Text('Processing data...'),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],

            // Processing Result
            if (processingResult != null) ...[
              Card(
                color: processingResult!['success'] == true 
                    ? Colors.green.shade900 
                    : Colors.red.shade900,
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Processing Result',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Text('Status: ${processingResult!['success'] == true ? 'Success' : 'Failed'}'),
                      if (processingResult!.containsKey('error'))
                        Text('Error: ${processingResult!['error']}'),
                      if (processingResult!.containsKey('processedCount'))
                        Text('Processed Count: ${processingResult!['processedCount']}'),
                      if (processingResult!.containsKey('report')) ...[
                        const SizedBox(height: 12),
                        const Text('Report:', style: TextStyle(fontWeight: FontWeight.bold)),
                        Container(
                          width: double.infinity,
                          padding: const EdgeInsets.all(8),
                          margin: const EdgeInsets.only(top: 4),
                          decoration: BoxDecoration(
                            color: Colors.grey.shade800,
                            borderRadius: BorderRadius.circular(4),
                          ),
                          child: Text(
                            processingResult!['report'],
                            style: const TextStyle(fontSize: 12, fontFamily: 'monospace'),
                          ),
                        ),
                      ],
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],

            // Processing Log
            if (currentProcessor != null && currentProcessor!.processingLog.isNotEmpty) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Processing Log',
                        style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      const SizedBox(height: 8),
                      Container(
                        height: 150,
                        width: double.infinity,
                        padding: const EdgeInsets.all(8),
                        decoration: BoxDecoration(
                          color: Colors.grey.shade900,
                          borderRadius: BorderRadius.circular(4),
                          border: Border.all(color: Colors.grey.shade700),
                        ),
                        child: SingleChildScrollView(
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: currentProcessor!.processingLog.map((log) => 
                              Text(log, style: const TextStyle(fontSize: 11, fontFamily: 'monospace'))
                            ).toList(),
                          ),
                        ),
                      ),
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
                      'Template Method Pattern Concepts Demonstrated',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('• Template method: processData() defines algorithm structure'),
                    const Text('• Abstract methods: transformData() must be implemented'),
                    const Text('• Hook methods: generateReport() with optional override'),
                    const Text('• Concrete methods: preprocessData(), postProcessResults()'),
                    const Text('• Algorithm invariance: Processing steps remain consistent'),
                    const Text('• Step customization: Subclasses customize specific steps'),
                    const Text('• Code reuse: Common processing logic shared across processors'),
                    const Text('• Inversion of control: Template controls algorithm flow'),
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

This example demonstrates the template method pattern through data  
processing workflows. The abstract DataProcessor defines the algorithm  
structure while concrete implementations (StatisticalProcessor,  
TextProcessor, ImageMetadataProcessor) customize specific steps. This  
ensures consistent processing flow while allowing specialized behavior.

## Decorator Pattern and Dynamic Behavior

Adding behavior to objects dynamically through decoration.

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
      title: 'Decorator Pattern',
      theme: ThemeData.dark(),
      home: const DecoratorDemo(),
    );
  }
}

// Component interface
abstract class Coffee {
  String get description;
  double get cost;
}

// Concrete component
class BasicCoffee implements Coffee {
  @override
  String get description => 'Basic Coffee';
  
  @override
  double get cost => 2.00;
}

// Base decorator
abstract class CoffeeDecorator implements Coffee {
  final Coffee coffee;
  
  CoffeeDecorator(this.coffee);
}

// Concrete decorators
class MilkDecorator extends CoffeeDecorator {
  MilkDecorator(Coffee coffee) : super(coffee);
  
  @override
  String get description => '${coffee.description}, Milk';
  
  @override
  double get cost => coffee.cost + 0.50;
}

class SugarDecorator extends CoffeeDecorator {
  SugarDecorator(Coffee coffee) : super(coffee);
  
  @override
  String get description => '${coffee.description}, Sugar';
  
  @override
  double get cost => coffee.cost + 0.25;
}

class VanillaDecorator extends CoffeeDecorator {
  VanillaDecorator(Coffee coffee) : super(coffee);
  
  @override
  String get description => '${coffee.description}, Vanilla';
  
  @override
  double get cost => coffee.cost + 0.75;
}

class WhipCreamDecorator extends CoffeeDecorator {
  WhipCreamDecorator(Coffee coffee) : super(coffee);
  
  @override
  String get description => '${coffee.description}, Whip Cream';
  
  @override
  double get cost => coffee.cost + 1.00;
}

class DecoratorDemo extends StatefulWidget {
  const DecoratorDemo({super.key});

  @override
  State<DecoratorDemo> createState() => _DecoratorDemoState();
}

class _DecoratorDemoState extends State<DecoratorDemo> {
  Coffee currentCoffee = BasicCoffee();
  List<String> decoratorHistory = ['Basic Coffee'];

  void _addDecorator(String decorator) {
    setState(() {
      switch (decorator) {
        case 'Milk':
          currentCoffee = MilkDecorator(currentCoffee);
          break;
        case 'Sugar':
          currentCoffee = SugarDecorator(currentCoffee);
          break;
        case 'Vanilla':
          currentCoffee = VanillaDecorator(currentCoffee);
          break;
        case 'Whip Cream':
          currentCoffee = WhipCreamDecorator(currentCoffee);
          break;
      }
      decoratorHistory.add(decorator);
    });
  }

  void _reset() {
    setState(() {
      currentCoffee = BasicCoffee();
      decoratorHistory = ['Basic Coffee'];
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Decorator Pattern - Coffee Shop')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    Text('Current Order: ${currentCoffee.description}', 
                         style: const TextStyle(fontSize: 18)),
                    Text('Total Cost: \$${currentCoffee.cost.toStringAsFixed(2)}', 
                         style: const TextStyle(fontSize: 16, color: Colors.green)),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Wrap(
              spacing: 8,
              children: ['Milk', 'Sugar', 'Vanilla', 'Whip Cream']
                  .map((decorator) => ElevatedButton(
                        onPressed: () => _addDecorator(decorator),
                        child: Text('Add $decorator'),
                      ))
                  .toList(),
            ),
            const SizedBox(height: 16),
            ElevatedButton(onPressed: _reset, child: const Text('Reset Order')),
          ],
        ),
      ),
    );
  }
}
```

The decorator pattern allows adding behavior to objects dynamically  
without altering their structure. Each decorator wraps the previous  
component and extends its functionality, enabling flexible composition  
of features at runtime.

## Chain of Responsibility Pattern

Implementing chain of responsibility for request processing.

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
      title: 'Chain of Responsibility',
      theme: ThemeData.dark(),
      home: const ChainOfResponsibilityDemo(),
    );
  }
}

// Request class
class SupportRequest {
  final String issue;
  final int priority;
  final String customerType;
  
  SupportRequest(this.issue, this.priority, this.customerType);
}

// Handler interface
abstract class SupportHandler {
  SupportHandler? nextHandler;
  
  void setNext(SupportHandler handler) {
    nextHandler = handler;
  }
  
  String handle(SupportRequest request);
}

// Concrete handlers
class Level1Support extends SupportHandler {
  @override
  String handle(SupportRequest request) {
    if (request.priority <= 2) {
      return 'Level 1 Support: Handled "${request.issue}"';
    }
    return nextHandler?.handle(request) ?? 'No handler available';
  }
}

class Level2Support extends SupportHandler {
  @override
  String handle(SupportRequest request) {
    if (request.priority <= 4) {
      return 'Level 2 Support: Handled "${request.issue}"';
    }
    return nextHandler?.handle(request) ?? 'No handler available';
  }
}

class Level3Support extends SupportHandler {
  @override
  String handle(SupportRequest request) {
    if (request.priority <= 5) {
      return 'Level 3 Support: Handled "${request.issue}"';
    }
    return nextHandler?.handle(request) ?? 'Request escalated to management';
  }
}

class ChainOfResponsibilityDemo extends StatefulWidget {
  const ChainOfResponsibilityDemo({super.key});

  @override
  State<ChainOfResponsibilityDemo> createState() => _ChainOfResponsibilityDemoState();
}

class _ChainOfResponsibilityDemoState extends State<ChainOfResponsibilityDemo> {
  late SupportHandler chain;
  String result = '';

  @override
  void initState() {
    super.initState();
    // Build the chain
    final level1 = Level1Support();
    final level2 = Level2Support();
    final level3 = Level3Support();
    
    level1.setNext(level2);
    level2.setNext(level3);
    
    chain = level1;
  }

  void _handleRequest(String issue, int priority, String customerType) {
    final request = SupportRequest(issue, priority, customerType);
    setState(() {
      result = chain.handle(request);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Chain of Responsibility')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            if (result.isNotEmpty)
              Card(
                color: Colors.green.shade900,
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Text('Result: $result'),
                ),
              ),
            const SizedBox(height: 16),
            const Text('Sample Support Requests:', style: TextStyle(fontSize: 18)),
            const SizedBox(height: 8),
            ...[(('Login Issue', 1)), ('Billing Question', 3)), ('System Crash', 5))]
                .map((request) => ElevatedButton(
                      onPressed: () => _handleRequest(request.$1, request.$2, 'Standard'),
                      child: Text('${request.$1} (Priority ${request.$2})'),
                    ))
                .toList(),
          ],
        ),
      ),
    );
  }
}
```

Chain of responsibility passes requests along a chain of handlers  
until one handles it. This decouples senders from receivers and  
allows multiple objects to handle requests without explicit knowledge  
of which one will process it.

## Builder Pattern for Complex Objects

Constructing complex objects step by step using builder pattern.

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
      title: 'Builder Pattern',
      theme: ThemeData.dark(),
      home: const BuilderDemo(),
    );
  }
}

// Product class
class Computer {
  final String cpu;
  final String ram;
  final String storage;
  final String gpu;
  final String motherboard;
  final bool hasWifi;
  final bool hasBluetooth;
  
  Computer._builder(ComputerBuilder builder)
      : cpu = builder._cpu,
        ram = builder._ram,
        storage = builder._storage,
        gpu = builder._gpu,
        motherboard = builder._motherboard,
        hasWifi = builder._hasWifi,
        hasBluetooth = builder._hasBluetooth;

  @override
  String toString() {
    return 'Computer: CPU=$cpu, RAM=$ram, Storage=$storage, GPU=$gpu, MB=$motherboard, WiFi=$hasWifi, BT=$hasBluetooth';
  }
}

// Builder class
class ComputerBuilder {
  String _cpu = '';
  String _ram = '';
  String _storage = '';
  String _gpu = '';
  String _motherboard = '';
  bool _hasWifi = false;
  bool _hasBluetooth = false;
  
  ComputerBuilder setCpu(String cpu) {
    _cpu = cpu;
    return this;
  }
  
  ComputerBuilder setRam(String ram) {
    _ram = ram;
    return this;
  }
  
  ComputerBuilder setStorage(String storage) {
    _storage = storage;
    return this;
  }
  
  ComputerBuilder setGpu(String gpu) {
    _gpu = gpu;
    return this;
  }
  
  ComputerBuilder setMotherboard(String motherboard) {
    _motherboard = motherboard;
    return this;
  }
  
  ComputerBuilder setWifi(bool hasWifi) {
    _hasWifi = hasWifi;
    return this;
  }
  
  ComputerBuilder setBluetooth(bool hasBluetooth) {
    _hasBluetooth = hasBluetooth;
    return this;
  }
  
  Computer build() {
    return Computer._builder(this);
  }
}

class BuilderDemo extends StatefulWidget {
  const BuilderDemo({super.key});

  @override
  State<BuilderDemo> createState() => _BuilderDemoState();
}

class _BuilderDemoState extends State<BuilderDemo> {
  Computer? builtComputer;
  
  void _buildGamingComputer() {
    final computer = ComputerBuilder()
        .setCpu('Intel i9-12900K')
        .setRam('32GB DDR4')
        .setStorage('1TB NVMe SSD')
        .setGpu('NVIDIA RTX 3080')
        .setMotherboard('ASUS ROG Strix')
        .setWifi(true)
        .setBluetooth(true)
        .build();
    
    setState(() {
      builtComputer = computer;
    });
  }
  
  void _buildOfficeComputer() {
    final computer = ComputerBuilder()
        .setCpu('Intel i5-12400')
        .setRam('16GB DDR4')
        .setStorage('512GB SSD')
        .setGpu('Integrated')
        .setMotherboard('Standard ATX')
        .setWifi(true)
        .setBluetooth(false)
        .build();
    
    setState(() {
      builtComputer = computer;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Builder Pattern')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            ElevatedButton(
              onPressed: _buildGamingComputer,
              child: const Text('Build Gaming Computer'),
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: _buildOfficeComputer,
              child: const Text('Build Office Computer'),
            ),
            const SizedBox(height: 16),
            if (builtComputer != null)
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Text(builtComputer.toString()),
                ),
              ),
          ],
        ),
      ),
    );
  }
}
```

The builder pattern constructs complex objects step by step, providing  
a fluent interface for object creation. It separates construction from  
representation, allowing the same construction process to create  
different representations.

## Adapter Pattern for Interface Compatibility

Adapting incompatible interfaces to work together.

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
      title: 'Adapter Pattern',
      theme: ThemeData.dark(),
      home: const AdapterDemo(),
    );
  }
}

// Target interface (what client expects)
abstract class MediaPlayer {
  void play(String audioType, String fileName);
}

// Legacy media players (adaptees)
class Mp3Player {
  void playMp3(String fileName) {
    print('Playing MP3: $fileName');
  }
}

class Mp4Player {
  void playMp4(String fileName) {
    print('Playing MP4: $fileName');
  }
}

// Adapter
class MediaAdapter implements MediaPlayer {
  final Mp3Player _mp3Player = Mp3Player();
  final Mp4Player _mp4Player = Mp4Player();
  
  @override
  void play(String audioType, String fileName) {
    switch (audioType.toLowerCase()) {
      case 'mp3':
        _mp3Player.playMp3(fileName);
        break;
      case 'mp4':
        _mp4Player.playMp4(fileName);
        break;
      default:
        throw UnsupportedError('$audioType format not supported');
    }
  }
}

class AdapterDemo extends StatefulWidget {
  const AdapterDemo({super.key});

  @override
  State<AdapterDemo> createState() => _AdapterDemoState();
}

class _AdapterDemoState extends State<AdapterDemo> {
  final MediaPlayer player = MediaAdapter();
  String result = '';

  void _playMedia(String type, String fileName) {
    try {
      player.play(type, fileName);
      setState(() {
        result = 'Successfully played $fileName ($type)';
      });
    } catch (e) {
      setState(() {
        result = 'Error: $e';
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Adapter Pattern')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            if (result.isNotEmpty)
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Text(result),
                ),
              ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: () => _playMedia('mp3', 'song.mp3'),
              child: const Text('Play MP3'),
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: () => _playMedia('mp4', 'video.mp4'),
              child: const Text('Play MP4'),
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: () => _playMedia('avi', 'movie.avi'),
              child: const Text('Play AVI (Unsupported)'),
            ),
          ],
        ),
      ),
    );
  }
}
```

The adapter pattern allows incompatible interfaces to work together  
by wrapping existing classes with a new interface. It enables legacy  
code integration without modifying existing implementations.

## Facade Pattern for Simplified Interfaces

Providing a simplified interface to complex subsystems.

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
      title: 'Facade Pattern',
      theme: ThemeData.dark(),
      home: const FacadeDemo(),
    );
  }
}

// Complex subsystem classes
class AudioSystem {
  void turnOn() => print('Audio system on');
  void setVolume(int level) => print('Volume set to $level');
  void turnOff() => print('Audio system off');
}

class VideoSystem {
  void turnOn() => print('Video system on');
  void setResolution(String resolution) => print('Resolution: $resolution');
  void turnOff() => print('Video system off');
}

class LightingSystem {
  void turnOn() => print('Lights on');
  void dim(int level) => print('Lights dimmed to $level%');
  void turnOff() => print('Lights off');
}

// Facade
class HomeTheaterFacade {
  final AudioSystem _audio = AudioSystem();
  final VideoSystem _video = VideoSystem();
  final LightingSystem _lights = LightingSystem();
  
  void watchMovie() {
    print('Getting ready to watch movie...');
    _lights.dim(10);
    _audio.turnOn();
    _audio.setVolume(5);
    _video.turnOn();
    _video.setResolution('1080p');
    print('Movie mode activated!');
  }
  
  void endMovie() {
    print('Shutting down movie mode...');
    _video.turnOff();
    _audio.turnOff();
    _lights.turnOff();
    print('Movie mode deactivated!');
  }
}

class FacadeDemo extends StatefulWidget {
  const FacadeDemo({super.key});

  @override
  State<FacadeDemo> createState() => _FacadeDemoState();
}

class _FacadeDemoState extends State<FacadeDemo> {
  final HomeTheaterFacade facade = HomeTheaterFacade();
  List<String> operations = [];

  void _watchMovie() {
    facade.watchMovie();
    setState(() {
      operations.add('Movie mode started');
    });
  }

  void _endMovie() {
    facade.endMovie();
    setState(() {
      operations.add('Movie mode ended');
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Facade Pattern')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            ElevatedButton(
              onPressed: _watchMovie,
              child: const Text('Start Movie'),
            ),
            const SizedBox(height: 8),
            ElevatedButton(
              onPressed: _endMovie,
              child: const Text('End Movie'),
            ),
            const SizedBox(height: 16),
            if (operations.isNotEmpty)
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    children: operations.map((op) => Text(op)).toList(),
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

The facade pattern provides a simplified interface to complex  
subsystems. It hides complexity from clients and provides a unified  
interface for common operations, making the system easier to use.

## Flyweight Pattern for Memory Efficiency

Sharing objects efficiently to minimize memory usage.

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
      title: 'Flyweight Pattern',
      theme: ThemeData.dark(),
      home: const FlyweightDemo(),
    );
  }
}

// Flyweight interface
abstract class TreeType {
  void render(Canvas canvas, double x, double y, Color color);
}

// Concrete flyweight
class ConcreteTreeType implements TreeType {
  final String name;
  final String sprite;
  
  ConcreteTreeType(this.name, this.sprite);
  
  @override
  void render(Canvas canvas, double x, double y, Color color) {
    // Simulate rendering tree with intrinsic data
    final paint = Paint()..color = color;
    canvas.drawCircle(Offset(x, y), 20, paint);
  }
}

// Flyweight factory
class TreeTypeFactory {
  static final Map<String, TreeType> _treeTypes = {};
  
  static TreeType getTreeType(String name, String sprite) {
    return _treeTypes.putIfAbsent(name, () => ConcreteTreeType(name, sprite));
  }
  
  static int get createdTreeTypes => _treeTypes.length;
}

// Context class
class Tree {
  final double x, y;
  final Color color;
  final TreeType type;
  
  Tree(this.x, this.y, this.color, this.type);
  
  void render(Canvas canvas) {
    type.render(canvas, x, y, color);
  }
}

class FlyweightDemo extends StatefulWidget {
  const FlyweightDemo({super.key});

  @override
  State<FlyweightDemo> createState() => _FlyweightDemoState();
}

class _FlyweightDemoState extends State<FlyweightDemo> {
  final List<Tree> trees = [];
  
  void _addTrees() {
    final random = DateTime.now().millisecond;
    for (int i = 0; i < 10; i++) {
      final type = TreeTypeFactory.getTreeType('Oak', 'oak_sprite');
      trees.add(Tree(
        (random + i * 30) % 300.0,
        (random + i * 20) % 200.0,
        Colors.green,
        type,
      ));
    }
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Flyweight Pattern')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            Text('Trees: ${trees.length}'),
            Text('Tree Types Created: ${TreeTypeFactory.createdTreeTypes}'),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _addTrees,
              child: const Text('Add 10 Trees'),
            ),
            const SizedBox(height: 16),
            Expanded(
              child: Container(
                decoration: BoxDecoration(border: Border.all()),
                child: CustomPaint(
                  painter: ForestPainter(trees),
                  size: Size.infinite,
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class ForestPainter extends CustomPainter {
  final List<Tree> trees;
  
  ForestPainter(this.trees);
  
  @override
  void paint(Canvas canvas, Size size) {
    for (final tree in trees) {
      tree.render(canvas);
    }
  }
  
  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}
```

The flyweight pattern minimizes memory usage by sharing common data  
between multiple objects. Intrinsic state is shared while extrinsic  
state is passed as parameters, enabling efficient object management.

## Model-View-Controller Architecture

Implementing MVC pattern for separation of concerns.

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
      title: 'MVC Pattern',
      theme: ThemeData.dark(),
      home: const MVCDemo(),
    );
  }
}

// Model
class CounterModel {
  int _count = 0;
  final List<Function()> _observers = [];
  
  int get count => _count;
  
  void addObserver(Function() observer) {
    _observers.add(observer);
  }
  
  void removeObserver(Function() observer) {
    _observers.remove(observer);
  }
  
  void increment() {
    _count++;
    _notifyObservers();
  }
  
  void decrement() {
    _count--;
    _notifyObservers();
  }
  
  void reset() {
    _count = 0;
    _notifyObservers();
  }
  
  void _notifyObservers() {
    for (final observer in _observers) {
      observer();
    }
  }
}

// Controller
class CounterController {
  final CounterModel _model;
  
  CounterController(this._model);
  
  void increment() => _model.increment();
  void decrement() => _model.decrement();
  void reset() => _model.reset();
  
  int get count => _model.count;
}

// View
class MVCDemo extends StatefulWidget {
  const MVCDemo({super.key});

  @override
  State<MVCDemo> createState() => _MVCDemoState();
}

class _MVCDemoState extends State<MVCDemo> {
  late CounterModel model;
  late CounterController controller;
  
  @override
  void initState() {
    super.initState();
    model = CounterModel();
    controller = CounterController(model);
    
    // View observes model changes
    model.addObserver(() {
      setState(() {}); // Update view when model changes
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('MVC Pattern')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Count: ${controller.count}', style: const TextStyle(fontSize: 24)),
            const SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                ElevatedButton(
                  onPressed: controller.decrement,
                  child: const Text('-'),
                ),
                ElevatedButton(
                  onPressed: controller.reset,
                  child: const Text('Reset'),
                ),
                ElevatedButton(
                  onPressed: controller.increment,
                  child: const Text('+'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
```

The MVC pattern separates application logic into three interconnected  
components: Model manages data, View handles presentation, and  
Controller coordinates between them, promoting maintainability and  
testability.

## Prototype Pattern for Object Cloning

Creating objects through cloning existing prototypes.

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
      title: 'Prototype Pattern',
      theme: ThemeData.dark(),
      home: const PrototypeDemo(),
    );
  }
}

abstract class Prototype {
  Prototype clone();
}

class Document implements Prototype {
  String title;
  String content;
  List<String> tags;
  DateTime created;
  
  Document(this.title, this.content, this.tags, this.created);
  
  @override
  Document clone() {
    return Document(title, content, List.from(tags), created);
  }
  
  @override
  String toString() => '$title: $content (${tags.join(', ')})';
}

class PrototypeDemo extends StatefulWidget {
  const PrototypeDemo({super.key});

  @override
  State<PrototypeDemo> createState() => _PrototypeDemoState();
}

class _PrototypeDemoState extends State<PrototypeDemo> {
  final List<Document> documents = [];
  late Document prototype;

  @override
  void initState() {
    super.initState();
    prototype = Document('Template', 'Default content', ['template'], DateTime.now());
  }

  void _cloneDocument() {
    final cloned = prototype.clone();
    cloned.title = 'Cloned Doc ${documents.length + 1}';
    setState(() {
      documents.add(cloned);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Prototype Pattern')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            ElevatedButton(
              onPressed: _cloneDocument,
              child: const Text('Clone Document'),
            ),
            const SizedBox(height: 16),
            Expanded(
              child: ListView.builder(
                itemCount: documents.length,
                itemBuilder: (context, index) => Card(
                  child: Padding(
                    padding: const EdgeInsets.all(8),
                    child: Text(documents[index].toString()),
                  ),
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

The prototype pattern creates objects by cloning existing instances  
rather than creating new ones from scratch. This is useful when object  
creation is expensive or when you need objects similar to existing ones.

## Visitor Pattern for Operations on Object Structures

Implementing visitor pattern for adding operations to object hierarchies.

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
      title: 'Visitor Pattern',
      theme: ThemeData.dark(),
      home: const VisitorDemo(),
    );
  }
}

abstract class Visitor {
  void visitCircle(Circle circle);
  void visitRectangle(Rectangle rectangle);
}

abstract class Shape {
  void accept(Visitor visitor);
}

class Circle implements Shape {
  final double radius;
  Circle(this.radius);
  
  @override
  void accept(Visitor visitor) => visitor.visitCircle(this);
}

class Rectangle implements Shape {
  final double width, height;
  Rectangle(this.width, this.height);
  
  @override
  void accept(Visitor visitor) => visitor.visitRectangle(this);
}

class AreaVisitor implements Visitor {
  double totalArea = 0;
  
  @override
  void visitCircle(Circle circle) {
    totalArea += 3.14159 * circle.radius * circle.radius;
  }
  
  @override
  void visitRectangle(Rectangle rectangle) {
    totalArea += rectangle.width * rectangle.height;
  }
}

class VisitorDemo extends StatefulWidget {
  const VisitorDemo({super.key});

  @override
  State<VisitorDemo> createState() => _VisitorDemoState();
}

class _VisitorDemoState extends State<VisitorDemo> {
  final List<Shape> shapes = [
    Circle(5),
    Rectangle(4, 6),
    Circle(3),
  ];
  
  double totalArea = 0;

  void _calculateArea() {
    final visitor = AreaVisitor();
    for (final shape in shapes) {
      shape.accept(visitor);
    }
    setState(() {
      totalArea = visitor.totalArea;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Visitor Pattern')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            Text('Shapes: ${shapes.length}'),
            ElevatedButton(
              onPressed: _calculateArea,
              child: const Text('Calculate Total Area'),
            ),
            if (totalArea > 0)
              Text('Total Area: ${totalArea.toStringAsFixed(2)}'),
          ],
        ),
      ),
    );
  }
}
```

The visitor pattern separates algorithms from object structures,  
allowing new operations to be added without modifying existing classes.  
It's useful for operations across different types in a hierarchy.

## Memento Pattern for State Snapshots

Capturing and restoring object state using memento pattern.

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
      title: 'Memento Pattern',
      theme: ThemeData.dark(),
      home: const MementoDemo(),
    );
  }
}

class TextMemento {
  final String text;
  final int cursorPosition;
  final DateTime timestamp;
  
  TextMemento(this.text, this.cursorPosition) : timestamp = DateTime.now();
}

class TextEditor {
  String _text = '';
  int _cursorPosition = 0;
  
  String get text => _text;
  int get cursorPosition => _cursorPosition;
  
  void write(String text) {
    _text += text;
    _cursorPosition = _text.length;
  }
  
  void setText(String text) {
    _text = text;
    _cursorPosition = text.length;
  }
  
  TextMemento createMemento() {
    return TextMemento(_text, _cursorPosition);
  }
  
  void restoreFromMemento(TextMemento memento) {
    _text = memento.text;
    _cursorPosition = memento.cursorPosition;
  }
}

class MementoDemo extends StatefulWidget {
  const MementoDemo({super.key});

  @override
  State<MementoDemo> createState() => _MementoDemoState();
}

class _MementoDemoState extends State<MementoDemo> {
  final TextEditor editor = TextEditor();
  final List<TextMemento> history = [];
  final TextEditingController controller = TextEditingController();

  void _saveState() {
    setState(() {
      history.add(editor.createMemento());
    });
  }

  void _restoreState(int index) {
    editor.restoreFromMemento(history[index]);
    setState(() {
      controller.text = editor.text;
    });
  }

  void _updateText() {
    editor.setText(controller.text);
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Memento Pattern')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            TextField(
              controller: controller,
              onChanged: (_) => _updateText(),
              decoration: const InputDecoration(labelText: 'Text Editor'),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _saveState,
              child: const Text('Save State'),
            ),
            const SizedBox(height: 16),
            Text('Current: ${editor.text}'),
            const SizedBox(height: 16),
            const Text('History:'),
            Expanded(
              child: ListView.builder(
                itemCount: history.length,
                itemBuilder: (context, index) => ListTile(
                  title: Text(history[index].text),
                  subtitle: Text(history[index].timestamp.toString()),
                  trailing: ElevatedButton(
                    onPressed: () => _restoreState(index),
                    child: const Text('Restore'),
                  ),
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

The memento pattern captures object state without violating  
encapsulation, enabling undo functionality and state restoration.  
It's essential for implementing undo/redo operations in applications.

## Mediator Pattern for Object Communication

Implementing mediator pattern to handle complex object interactions.

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
      title: 'Mediator Pattern',
      theme: ThemeData.dark(),
      home: const MediatorDemo(),
    );
  }
}

abstract class Mediator {
  void notify(Component sender, String event);
}

abstract class Component {
  Mediator? mediator;
  
  Component([this.mediator]);
  
  void setMediator(Mediator mediator) {
    this.mediator = mediator;
  }
}

class Button extends Component {
  final String name;
  bool _enabled = true;
  
  Button(this.name);
  
  bool get enabled => _enabled;
  
  void click() {
    mediator?.notify(this, 'click');
  }
  
  void setEnabled(bool enabled) {
    _enabled = enabled;
  }
}

class CheckBox extends Component {
  final String name;
  bool _checked = false;
  
  CheckBox(this.name);
  
  bool get checked => _checked;
  
  void toggle() {
    _checked = !_checked;
    mediator?.notify(this, 'change');
  }
}

class DialogMediator implements Mediator {
  late Button submitButton;
  late CheckBox termsCheckBox;
  
  void initialize(Button submit, CheckBox terms) {
    submitButton = submit;
    termsCheckBox = terms;
    
    submit.setMediator(this);
    terms.setMediator(this);
    
    // Initial state
    submitButton.setEnabled(false);
  }
  
  @override
  void notify(Component sender, String event) {
    if (sender == termsCheckBox && event == 'change') {
      submitButton.setEnabled(termsCheckBox.checked);
    }
  }
}

class MediatorDemo extends StatefulWidget {
  const MediatorDemo({super.key});

  @override
  State<MediatorDemo> createState() => _MediatorDemoState();
}

class _MediatorDemoState extends State<MediatorDemo> {
  late DialogMediator mediator;
  late Button submitButton;
  late CheckBox termsCheckBox;

  @override
  void initState() {
    super.initState();
    mediator = DialogMediator();
    submitButton = Button('Submit');
    termsCheckBox = CheckBox('Accept Terms');
    
    mediator.initialize(submitButton, termsCheckBox);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Mediator Pattern')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            CheckboxListTile(
              title: Text('Accept Terms and Conditions'),
              value: termsCheckBox.checked,
              onChanged: (_) {
                termsCheckBox.toggle();
                setState(() {});
              },
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: submitButton.enabled ? submitButton.click : null,
              child: const Text('Submit'),
            ),
            const SizedBox(height: 16),
            Text('Submit button enabled: ${submitButton.enabled}'),
          ],
        ),
      ),
    );
  }
}
```

The mediator pattern defines how objects communicate without explicit  
references to each other. It promotes loose coupling by centralizing  
complex communications in a mediator object.

## Abstract Factory Pattern

Creating families of related objects using abstract factories.

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
      title: 'Abstract Factory Pattern',
      theme: ThemeData.dark(),
      home: const AbstractFactoryDemo(),
    );
  }
}

// Abstract products
abstract class Button {
  void render();
  String get style;
}

abstract class Checkbox {
  void render();
  String get style;
}

// Concrete products for Windows
class WindowsButton implements Button {
  @override
  void render() => print('Rendering Windows button');
  
  @override
  String get style => 'Windows Button';
}

class WindowsCheckbox implements Checkbox {
  @override
  void render() => print('Rendering Windows checkbox');
  
  @override
  String get style => 'Windows Checkbox';
}

// Concrete products for Mac
class MacButton implements Button {
  @override
  void render() => print('Rendering Mac button');
  
  @override
  String get style => 'Mac Button';
}

class MacCheckbox implements Checkbox {
  @override
  void render() => print('Rendering Mac checkbox');
  
  @override
  String get style => 'Mac Checkbox';
}

// Abstract factory
abstract class UIFactory {
  Button createButton();
  Checkbox createCheckbox();
}

// Concrete factories
class WindowsFactory implements UIFactory {
  @override
  Button createButton() => WindowsButton();
  
  @override
  Checkbox createCheckbox() => WindowsCheckbox();
}

class MacFactory implements UIFactory {
  @override
  Button createButton() => MacButton();
  
  @override
  Checkbox createCheckbox() => MacCheckbox();
}

class AbstractFactoryDemo extends StatefulWidget {
  const AbstractFactoryDemo({super.key});

  @override
  State<AbstractFactoryDemo> createState() => _AbstractFactoryDemoState();
}

class _AbstractFactoryDemoState extends State<AbstractFactoryDemo> {
  UIFactory? currentFactory;
  Button? button;
  Checkbox? checkbox;

  void _selectFactory(String type) {
    setState(() {
      currentFactory = type == 'Windows' ? WindowsFactory() : MacFactory();
      button = currentFactory!.createButton();
      checkbox = currentFactory!.createCheckbox();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Abstract Factory Pattern')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            Row(
              children: [
                ElevatedButton(
                  onPressed: () => _selectFactory('Windows'),
                  child: const Text('Windows UI'),
                ),
                const SizedBox(width: 16),
                ElevatedButton(
                  onPressed: () => _selectFactory('Mac'),
                  child: const Text('Mac UI'),
                ),
              ],
            ),
            const SizedBox(height: 16),
            if (button != null && checkbox != null) ...[
              Text('Button: ${button!.style}'),
              Text('Checkbox: ${checkbox!.style}'),
            ],
          ],
        ),
      ),
    );
  }
}
```

The abstract factory pattern creates families of related objects  
without specifying concrete classes. It ensures compatibility between  
products and enables easy switching between product families.

## Bridge Pattern for Implementation Independence

Separating abstraction from implementation using bridge pattern.

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
      title: 'Bridge Pattern',
      theme: ThemeData.dark(),
      home: const BridgeDemo(),
    );
  }
}

// Implementation interface
abstract class DrawingAPI {
  void drawCircle(double x, double y, double radius);
  void drawRectangle(double x, double y, double width, double height);
  String get apiName;
}

// Concrete implementations
class Canvas2D implements DrawingAPI {
  @override
  void drawCircle(double x, double y, double radius) {
    print('Canvas2D: Drawing circle at ($x, $y) with radius $radius');
  }
  
  @override
  void drawRectangle(double x, double y, double width, double height) {
    print('Canvas2D: Drawing rectangle at ($x, $y) with size ${width}x$height');
  }
  
  @override
  String get apiName => 'Canvas 2D';
}

class SVG implements DrawingAPI {
  @override
  void drawCircle(double x, double y, double radius) {
    print('SVG: <circle cx="$x" cy="$y" r="$radius"/>');
  }
  
  @override
  void drawRectangle(double x, double y, double width, double height) {
    print('SVG: <rect x="$x" y="$y" width="$width" height="$height"/>');
  }
  
  @override
  String get apiName => 'SVG';
}

// Abstraction
abstract class Shape {
  final DrawingAPI drawingAPI;
  
  Shape(this.drawingAPI);
  
  void draw();
  void resize(double factor);
}

// Refined abstractions
class Circle extends Shape {
  double x, y, radius;
  
  Circle(DrawingAPI drawingAPI, this.x, this.y, this.radius) : super(drawingAPI);
  
  @override
  void draw() {
    drawingAPI.drawCircle(x, y, radius);
  }
  
  @override
  void resize(double factor) {
    radius *= factor;
  }
}

class Rectangle extends Shape {
  double x, y, width, height;
  
  Rectangle(DrawingAPI drawingAPI, this.x, this.y, this.width, this.height) 
      : super(drawingAPI);
  
  @override
  void draw() {
    drawingAPI.drawRectangle(x, y, width, height);
  }
  
  @override
  void resize(double factor) {
    width *= factor;
    height *= factor;
  }
}

class BridgeDemo extends StatefulWidget {
  const BridgeDemo({super.key});

  @override
  State<BridgeDemo> createState() => _BridgeDemoState();
}

class _BridgeDemoState extends State<BridgeDemo> {
  DrawingAPI currentAPI = Canvas2D();
  List<Shape> shapes = [];
  List<String> operations = [];

  void _switchAPI() {
    setState(() {
      currentAPI = currentAPI is Canvas2D ? SVG() : Canvas2D();
      operations.add('Switched to ${currentAPI.apiName}');
    });
  }

  void _addCircle() {
    final circle = Circle(currentAPI, 50, 50, 25);
    shapes.add(circle);
    circle.draw();
    setState(() {
      operations.add('Added circle using ${currentAPI.apiName}');
    });
  }

  void _addRectangle() {
    final rectangle = Rectangle(currentAPI, 100, 100, 50, 30);
    shapes.add(rectangle);
    rectangle.draw();
    setState(() {
      operations.add('Added rectangle using ${currentAPI.apiName}');
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Bridge Pattern')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            Text('Current API: ${currentAPI.apiName}'),
            const SizedBox(height: 16),
            Row(
              children: [
                ElevatedButton(
                  onPressed: _switchAPI,
                  child: const Text('Switch API'),
                ),
                const SizedBox(width: 16),
                ElevatedButton(
                  onPressed: _addCircle,
                  child: const Text('Add Circle'),
                ),
                const SizedBox(width: 16),
                ElevatedButton(
                  onPressed: _addRectangle,
                  child: const Text('Add Rectangle'),
                ),
              ],
            ),
            const SizedBox(height: 16),
            Text('Total shapes: ${shapes.length}'),
            const SizedBox(height: 16),
            Expanded(
              child: ListView.builder(
                itemCount: operations.length,
                itemBuilder: (context, index) => ListTile(
                  title: Text(operations[index]),
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

The bridge pattern separates abstraction from implementation,  
allowing both to vary independently. It's useful when you want to  
avoid permanent binding between abstraction and implementation.
