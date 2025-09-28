# Flutter State Management - 25 Essential Examples

This comprehensive guide covers Flutter state management with 25 practical  
examples, from basic setState() to advanced state management solutions like  
Provider, Riverpod, and BLoC. Each example demonstrates different approaches  
to managing application state effectively in Flutter applications.

## Basic Counter with setState

A simple counter demonstrating the fundamental setState() approach.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Basic Counter',
      home: const CounterPage(),
    );
  }
}

class CounterPage extends StatefulWidget {
  const CounterPage({super.key});

  @override
  State<CounterPage> createState() => _CounterPageState();
}

class _CounterPageState extends State<CounterPage> {
  int _counter = 0;

  void _increment() {
    setState(() {
      _counter++;
    });
  }

  void _decrement() {
    setState(() {
      _counter--;
    });
  }

  void _reset() {
    setState(() {
      _counter = 0;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic Counter'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Counter Value:',
              style: TextStyle(fontSize: 18),
            ),
            const SizedBox(height: 10),
            Text(
              '$_counter',
              style: const TextStyle(
                fontSize: 48,
                fontWeight: FontWeight.bold,
                color: Colors.blueAccent,
              ),
            ),
            const SizedBox(height: 30),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: _decrement,
                  child: const Text('- Decrement'),
                ),
                const SizedBox(width: 20),
                ElevatedButton(
                  onPressed: _reset,
                  child: const Text('Reset'),
                ),
                const SizedBox(width: 20),
                ElevatedButton(
                  onPressed: _increment,
                  child: const Text('+ Increment'),
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

This basic example demonstrates Flutter's built-in state management using  
setState(). When setState() is called, it triggers a rebuild of the widget  
subtree, updating the UI to reflect the new state. This approach works well  
for simple, local state that doesn't need to be shared between widgets.  

## Form State Management

Managing form input state with validation and error handling.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Form State',
      home: const FormPage(),
    );
  }
}

class FormPage extends StatefulWidget {
  const FormPage({super.key});

  @override
  State<FormPage> createState() => _FormPageState();
}

class _FormPageState extends State<FormPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  bool _isSubmitting = false;
  String? _submissionResult;

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    super.dispose();
  }

  String? _validateName(String? value) {
    if (value == null || value.isEmpty) {
      return 'Name is required';
    }
    if (value.length < 2) {
      return 'Name must be at least 2 characters';
    }
    return null;
  }

  String? _validateEmail(String? value) {
    if (value == null || value.isEmpty) {
      return 'Email is required';
    }
    if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value)) {
      return 'Please enter a valid email';
    }
    return null;
  }

  Future<void> _submitForm() async {
    if (_formKey.currentState!.validate()) {
      setState(() {
        _isSubmitting = true;
        _submissionResult = null;
      });

      // Simulate network request
      await Future.delayed(const Duration(seconds: 2));

      setState(() {
        _isSubmitting = false;
        _submissionResult = 'Form submitted successfully!';
      });

      // Clear form
      _nameController.clear();
      _emailController.clear();
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Form State Management'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              TextFormField(
                controller: _nameController,
                decoration: const InputDecoration(
                  labelText: 'Name',
                  border: OutlineInputBorder(),
                ),
                validator: _validateName,
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _emailController,
                decoration: const InputDecoration(
                  labelText: 'Email',
                  border: OutlineInputBorder(),
                ),
                keyboardType: TextInputType.emailAddress,
                validator: _validateEmail,
              ),
              const SizedBox(height: 20),
              ElevatedButton(
                onPressed: _isSubmitting ? null : _submitForm,
                child: _isSubmitting
                    ? const CircularProgressIndicator()
                    : const Text('Submit'),
              ),
              if (_submissionResult != null) ...[
                const SizedBox(height: 20),
                Container(
                  padding: const EdgeInsets.all(12),
                  decoration: BoxDecoration(
                    color: Colors.green.withOpacity(0.1),
                    border: Border.all(color: Colors.green),
                    borderRadius: BorderRadius.circular(4),
                  ),
                  child: Text(
                    _submissionResult!,
                    style: const TextStyle(color: Colors.green),
                  ),
                ),
              ],
            ],
          ),
        ),
      ),
    );
  }
}
```

Form state management involves handling user input, validation, submission  
states, and feedback. This example demonstrates managing multiple form fields  
with validation, loading states during submission, and displaying results.  
TextEditingControllers manage input state while Form validation provides  
user feedback.  

## Todo List with State

A todo list demonstrating list state management and item operations.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Todo List',
      home: const TodoPage(),
    );
  }
}

class Todo {
  final String id;
  String title;
  bool isCompleted;
  DateTime createdAt;

  Todo({
    required this.id,
    required this.title,
    this.isCompleted = false,
    DateTime? createdAt,
  }) : createdAt = createdAt ?? DateTime.now();

  Todo copyWith({
    String? title,
    bool? isCompleted,
  }) {
    return Todo(
      id: id,
      title: title ?? this.title,
      isCompleted: isCompleted ?? this.isCompleted,
      createdAt: createdAt,
    );
  }
}

class TodoPage extends StatefulWidget {
  const TodoPage({super.key});

  @override
  State<TodoPage> createState() => _TodoPageState();
}

class _TodoPageState extends State<TodoPage> {
  final List<Todo> _todos = [];
  final _textController = TextEditingController();

  void _addTodo() {
    if (_textController.text.trim().isNotEmpty) {
      setState(() {
        _todos.add(Todo(
          id: DateTime.now().millisecondsSinceEpoch.toString(),
          title: _textController.text.trim(),
        ));
      });
      _textController.clear();
    }
  }

  void _toggleTodo(String id) {
    setState(() {
      final index = _todos.indexWhere((todo) => todo.id == id);
      if (index != -1) {
        _todos[index] = _todos[index].copyWith(
          isCompleted: !_todos[index].isCompleted,
        );
      }
    });
  }

  void _deleteTodo(String id) {
    setState(() {
      _todos.removeWhere((todo) => todo.id == id);
    });
  }

  void _editTodo(String id, String newTitle) {
    setState(() {
      final index = _todos.indexWhere((todo) => todo.id == id);
      if (index != -1 && newTitle.trim().isNotEmpty) {
        _todos[index] = _todos[index].copyWith(title: newTitle.trim());
      }
    });
  }

  int get _completedCount => _todos.where((todo) => todo.isCompleted).length;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Todo List'),
        actions: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Center(
              child: Text('${_completedCount}/${_todos.length}'),
            ),
          ),
        ],
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Row(
              children: [
                Expanded(
                  child: TextField(
                    controller: _textController,
                    decoration: const InputDecoration(
                      hintText: 'Enter todo item...',
                      border: OutlineInputBorder(),
                    ),
                    onSubmitted: (_) => _addTodo(),
                  ),
                ),
                const SizedBox(width: 16),
                ElevatedButton(
                  onPressed: _addTodo,
                  child: const Text('Add'),
                ),
              ],
            ),
          ),
          Expanded(
            child: _todos.isEmpty
                ? const Center(
                    child: Text(
                      'No todos yet. Add one above!',
                      style: TextStyle(fontSize: 16),
                    ),
                  )
                : ListView.builder(
                    itemCount: _todos.length,
                    itemBuilder: (context, index) {
                      final todo = _todos[index];
                      return TodoItem(
                        todo: todo,
                        onToggle: () => _toggleTodo(todo.id),
                        onDelete: () => _deleteTodo(todo.id),
                        onEdit: (newTitle) => _editTodo(todo.id, newTitle),
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }

  @override
  void dispose() {
    _textController.dispose();
    super.dispose();
  }
}

class TodoItem extends StatelessWidget {
  final Todo todo;
  final VoidCallback onToggle;
  final VoidCallback onDelete;
  final Function(String) onEdit;

  const TodoItem({
    super.key,
    required this.todo,
    required this.onToggle,
    required this.onDelete,
    required this.onEdit,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 4),
      child: ListTile(
        leading: Checkbox(
          value: todo.isCompleted,
          onChanged: (_) => onToggle(),
        ),
        title: Text(
          todo.title,
          style: TextStyle(
            decoration: todo.isCompleted
                ? TextDecoration.lineThrough
                : TextDecoration.none,
            color: todo.isCompleted
                ? Colors.grey
                : null,
          ),
        ),
        subtitle: Text(
          'Created: ${todo.createdAt.hour}:${todo.createdAt.minute.toString().padLeft(2, '0')}',
          style: const TextStyle(fontSize: 12),
        ),
        trailing: Row(
          mainAxisSize: MainAxisSize.min,
          children: [
            IconButton(
              icon: const Icon(Icons.edit),
              onPressed: () => _showEditDialog(context),
            ),
            IconButton(
              icon: const Icon(Icons.delete),
              onPressed: onDelete,
            ),
          ],
        ),
      ),
    );
  }

  void _showEditDialog(BuildContext context) {
    final controller = TextEditingController(text: todo.title);
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Edit Todo'),
        content: TextField(
          controller: controller,
          autofocus: true,
          decoration: const InputDecoration(
            border: OutlineInputBorder(),
          ),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.of(context).pop(),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () {
              onEdit(controller.text);
              Navigator.of(context).pop();
            },
            child: const Text('Save'),
          ),
        ],
      ),
    );
  }
}
```

This todo list example demonstrates managing a collection of items with  
CRUD operations. It shows how to handle adding, editing, toggling, and  
deleting items while maintaining state consistency. The example uses  
immutable data patterns with copyWith methods and separates presentation  
logic into reusable components.  

## ValueNotifier Counter

Using ValueNotifier for reactive state management without setState.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'ValueNotifier Counter',
      home: const ValueNotifierCounterPage(),
    );
  }
}

class ValueNotifierCounterPage extends StatefulWidget {
  const ValueNotifierCounterPage({super.key});

  @override
  State<ValueNotifierCounterPage> createState() => _ValueNotifierCounterPageState();
}

class _ValueNotifierCounterPageState extends State<ValueNotifierCounterPage> {
  final ValueNotifier<int> _counter = ValueNotifier<int>(0);

  @override
  void dispose() {
    _counter.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('ValueNotifier Counter'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Counter Value:',
              style: TextStyle(fontSize: 18),
            ),
            const SizedBox(height: 10),
            ValueListenableBuilder<int>(
              valueListenable: _counter,
              builder: (context, value, child) {
                return Text(
                  '$value',
                  style: const TextStyle(
                    fontSize: 48,
                    fontWeight: FontWeight.bold,
                    color: Colors.greenAccent,
                  ),
                );
              },
            ),
            const SizedBox(height: 20),
            ValueListenableBuilder<int>(
              valueListenable: _counter,
              builder: (context, value, child) {
                return Text(
                  value.isEven ? 'Even Number' : 'Odd Number',
                  style: TextStyle(
                    fontSize: 16,
                    color: value.isEven ? Colors.blueAccent : Colors.orangeAccent,
                  ),
                );
              },
            ),
            const SizedBox(height: 30),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () => _counter.value--,
                  child: const Text('- Decrease'),
                ),
                const SizedBox(width: 20),
                ElevatedButton(
                  onPressed: () => _counter.value = 0,
                  child: const Text('Reset'),
                ),
                const SizedBox(width: 20),
                ElevatedButton(
                  onPressed: () => _counter.value++,
                  child: const Text('+ Increase'),
                ),
              ],
            ),
            const SizedBox(height: 20),
            ValueListenableBuilder<int>(
              valueListenable: _counter,
              builder: (context, value, child) {
                return LinearProgressIndicator(
                  value: (value % 10) / 10,
                  backgroundColor: Colors.grey[700],
                  valueColor: AlwaysStoppedAnimation<Color>(
                    value.isEven ? Colors.blueAccent : Colors.orangeAccent,
                  ),
                );
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

ValueNotifier provides a lightweight way to manage reactive state without  
the overhead of setState(). ValueListenableBuilder automatically rebuilds  
when the ValueNotifier changes, making it efficient for simple state that  
needs to be observed by multiple widgets. This pattern is ideal for  
standalone values that don't require complex state logic.  

## Shopping Cart State

Managing complex state with multiple related values and operations.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Shopping Cart',
      home: const ShoppingCartPage(),
    );
  }
}

class Product {
  final String id;
  final String name;
  final double price;
  final String category;

  const Product({
    required this.id,
    required this.name,
    required this.price,
    required this.category,
  });
}

class CartItem {
  final Product product;
  int quantity;

  CartItem({
    required this.product,
    this.quantity = 1,
  });

  double get totalPrice => product.price * quantity;
}

class ShoppingCartPage extends StatefulWidget {
  const ShoppingCartPage({super.key});

  @override
  State<ShoppingCartPage> createState() => _ShoppingCartPageState();
}

class _ShoppingCartPageState extends State<ShoppingCartPage> {
  final List<CartItem> _cartItems = [];
  final List<Product> _products = const [
    Product(id: '1', name: 'Coffee', price: 4.50, category: 'Beverages'),
    Product(id: '2', name: 'Sandwich', price: 8.99, category: 'Food'),
    Product(id: '3', name: 'Salad', price: 6.75, category: 'Food'),
    Product(id: '4', name: 'Juice', price: 3.25, category: 'Beverages'),
    Product(id: '5', name: 'Cookie', price: 2.50, category: 'Desserts'),
    Product(id: '6', name: 'Cake', price: 12.99, category: 'Desserts'),
  ];

  void _addToCart(Product product) {
    setState(() {
      final existingIndex = _cartItems.indexWhere(
        (item) => item.product.id == product.id,
      );

      if (existingIndex >= 0) {
        _cartItems[existingIndex].quantity++;
      } else {
        _cartItems.add(CartItem(product: product));
      }
    });

    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text('${product.name} added to cart'),
        duration: const Duration(seconds: 1),
      ),
    );
  }

  void _removeFromCart(String productId) {
    setState(() {
      _cartItems.removeWhere((item) => item.product.id == productId);
    });
  }

  void _updateQuantity(String productId, int newQuantity) {
    setState(() {
      final index = _cartItems.indexWhere(
        (item) => item.product.id == productId,
      );
      
      if (index >= 0) {
        if (newQuantity <= 0) {
          _cartItems.removeAt(index);
        } else {
          _cartItems[index].quantity = newQuantity;
        }
      }
    });
  }

  void _clearCart() {
    setState(() {
      _cartItems.clear();
    });
  }

  double get _totalPrice {
    return _cartItems.fold(0.0, (sum, item) => sum + item.totalPrice);
  }

  int get _totalItems {
    return _cartItems.fold(0, (sum, item) => sum + item.quantity);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Shopping Cart'),
        actions: [
          Stack(
            children: [
              IconButton(
                icon: const Icon(Icons.shopping_cart),
                onPressed: () => _showCartDialog(),
              ),
              if (_totalItems > 0)
                Positioned(
                  right: 8,
                  top: 8,
                  child: Container(
                    padding: const EdgeInsets.all(2),
                    decoration: BoxDecoration(
                      color: Colors.redAccent,
                      borderRadius: BorderRadius.circular(10),
                    ),
                    constraints: const BoxConstraints(
                      minWidth: 16,
                      minHeight: 16,
                    ),
                    child: Text(
                      '$_totalItems',
                      style: const TextStyle(
                        color: Colors.white,
                        fontSize: 12,
                      ),
                      textAlign: TextAlign.center,
                    ),
                  ),
                ),
            ],
          ),
        ],
      ),
      body: Column(
        children: [
          Container(
            padding: const EdgeInsets.all(16),
            color: Colors.grey[900],
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text(
                  'Total: \$${_totalPrice.toStringAsFixed(2)}',
                  style: const TextStyle(
                    fontSize: 18,
                    fontWeight: FontWeight.bold,
                    color: Colors.greenAccent,
                  ),
                ),
                Text(
                  '${_totalItems} items',
                  style: const TextStyle(fontSize: 16),
                ),
              ],
            ),
          ),
          Expanded(
            child: GridView.builder(
              padding: const EdgeInsets.all(16),
              gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
                crossAxisCount: 2,
                crossAxisSpacing: 16,
                mainAxisSpacing: 16,
                childAspectRatio: 0.8,
              ),
              itemCount: _products.length,
              itemBuilder: (context, index) {
                final product = _products[index];
                return Card(
                  child: Padding(
                    padding: const EdgeInsets.all(12),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          product.name,
                          style: const TextStyle(
                            fontSize: 16,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        const SizedBox(height: 4),
                        Text(
                          product.category,
                          style: TextStyle(
                            fontSize: 12,
                            color: Colors.grey[400],
                          ),
                        ),
                        const Spacer(),
                        Text(
                          '\$${product.price.toStringAsFixed(2)}',
                          style: const TextStyle(
                            fontSize: 18,
                            fontWeight: FontWeight.bold,
                            color: Colors.greenAccent,
                          ),
                        ),
                        const SizedBox(height: 8),
                        SizedBox(
                          width: double.infinity,
                          child: ElevatedButton(
                            onPressed: () => _addToCart(product),
                            child: const Text('Add to Cart'),
                          ),
                        ),
                      ],
                    ),
                  ),
                );
              },
            ),
          ),
        ],
      ),
    );
  }

  void _showCartDialog() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Shopping Cart'),
        content: SizedBox(
          width: double.maxFinite,
          child: _cartItems.isEmpty
              ? const Text('Your cart is empty')
              : Column(
                  mainAxisSize: MainAxisSize.min,
                  children: [
                    Flexible(
                      child: ListView.builder(
                        shrinkWrap: true,
                        itemCount: _cartItems.length,
                        itemBuilder: (context, index) {
                          final item = _cartItems[index];
                          return ListTile(
                            title: Text(item.product.name),
                            subtitle: Text(
                              '\$${item.product.price.toStringAsFixed(2)} x ${item.quantity}',
                            ),
                            trailing: Row(
                              mainAxisSize: MainAxisSize.min,
                              children: [
                                IconButton(
                                  icon: const Icon(Icons.remove),
                                  onPressed: () => _updateQuantity(
                                    item.product.id,
                                    item.quantity - 1,
                                  ),
                                ),
                                Text('${item.quantity}'),
                                IconButton(
                                  icon: const Icon(Icons.add),
                                  onPressed: () => _updateQuantity(
                                    item.product.id,
                                    item.quantity + 1,
                                  ),
                                ),
                              ],
                            ),
                          );
                        },
                      ),
                    ),
                    const Divider(),
                    Text(
                      'Total: \$${_totalPrice.toStringAsFixed(2)}',
                      style: const TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  ],
                ),
        ),
        actions: [
          if (_cartItems.isNotEmpty)
            TextButton(
              onPressed: () {
                _clearCart();
                Navigator.of(context).pop();
              },
              child: const Text('Clear Cart'),
            ),
          TextButton(
            onPressed: () => Navigator.of(context).pop(),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }
}
```

This shopping cart example demonstrates managing complex state with multiple  
related entities. It shows how to handle collections, calculations, and user  
interactions while maintaining data consistency. The state includes products,  
cart items, quantities, and totals, all managed through coordinated setState  
calls that preserve referential integrity.  

## ChangeNotifier Model

Using ChangeNotifier for more structured state management.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'ChangeNotifier Model',
      home: const CounterModelPage(),
    );
  }
}

class CounterModel extends ChangeNotifier {
  int _count = 0;
  bool _isLoading = false;
  String? _error;

  int get count => _count;
  bool get isLoading => _isLoading;
  String? get error => _error;
  bool get canIncrement => !_isLoading && _count < 100;
  bool get canDecrement => !_isLoading && _count > 0;

  void increment() {
    if (!canIncrement) return;
    
    _count++;
    _error = null;
    notifyListeners();
  }

  void decrement() {
    if (!canDecrement) return;
    
    _count--;
    _error = null;
    notifyListeners();
  }

  void reset() {
    _count = 0;
    _error = null;
    notifyListeners();
  }

  Future<void> incrementAsync() async {
    if (!canIncrement) return;
    
    _isLoading = true;
    _error = null;
    notifyListeners();

    try {
      // Simulate network delay
      await Future.delayed(const Duration(seconds: 1));
      
      // Simulate potential error
      if (_count > 95) {
        throw Exception('Counter limit reached!');
      }
      
      _count++;
    } catch (e) {
      _error = e.toString();
    } finally {
      _isLoading = false;
      notifyListeners();
    }
  }

  void multiplyBy(int factor) {
    if (_isLoading) return;
    
    final newValue = _count * factor;
    if (newValue > 100) {
      _error = 'Result would exceed maximum value of 100';
    } else {
      _count = newValue;
      _error = null;
    }
    notifyListeners();
  }
}

class CounterModelPage extends StatefulWidget {
  const CounterModelPage({super.key});

  @override
  State<CounterModelPage> createState() => _CounterModelPageState();
}

class _CounterModelPageState extends State<CounterModelPage> {
  late CounterModel _counter;

  @override
  void initState() {
    super.initState();
    _counter = CounterModel();
    _counter.addListener(_onCounterChanged);
  }

  @override
  void dispose() {
    _counter.removeListener(_onCounterChanged);
    _counter.dispose();
    super.dispose();
  }

  void _onCounterChanged() {
    // This method is called whenever the CounterModel notifies listeners
    setState(() {
      // Trigger rebuild when counter changes
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('ChangeNotifier Model'),
        actions: [
          IconButton(
            icon: const Icon(Icons.info_outline),
            onPressed: () => _showInfoDialog(),
          ),
        ],
      ),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(20.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              const Text(
                'Counter Value:',
                style: TextStyle(fontSize: 18),
              ),
              const SizedBox(height: 10),
              Text(
                '${_counter.count}',
                style: TextStyle(
                  fontSize: 48,
                  fontWeight: FontWeight.bold,
                  color: _counter.count > 50 
                      ? Colors.redAccent 
                      : Colors.blueAccent,
                ),
              ),
              const SizedBox(height: 10),
              LinearProgressIndicator(
                value: _counter.count / 100,
                backgroundColor: Colors.grey[700],
                valueColor: AlwaysStoppedAnimation<Color>(
                  _counter.count > 75 
                      ? Colors.redAccent 
                      : _counter.count > 50 
                          ? Colors.orangeAccent 
                          : Colors.greenAccent,
                ),
              ),
              const SizedBox(height: 20),
              if (_counter.error != null) ...[
                Container(
                  padding: const EdgeInsets.all(12),
                  decoration: BoxDecoration(
                    color: Colors.red.withOpacity(0.1),
                    border: Border.all(color: Colors.red),
                    borderRadius: BorderRadius.circular(4),
                  ),
                  child: Row(
                    children: [
                      const Icon(Icons.error, color: Colors.red),
                      const SizedBox(width: 8),
                      Expanded(
                        child: Text(
                          _counter.error!,
                          style: const TextStyle(color: Colors.red),
                        ),
                      ),
                    ],
                  ),
                ),
                const SizedBox(height: 20),
              ],
              if (_counter.isLoading) ...[
                const CircularProgressIndicator(),
                const SizedBox(height: 20),
                const Text('Processing...'),
                const SizedBox(height: 20),
              ],
              Wrap(
                spacing: 10,
                runSpacing: 10,
                alignment: WrapAlignment.center,
                children: [
                  ElevatedButton(
                    onPressed: _counter.canDecrement ? _counter.decrement : null,
                    child: const Text('- 1'),
                  ),
                  ElevatedButton(
                    onPressed: _counter.canIncrement ? _counter.increment : null,
                    child: const Text('+ 1'),
                  ),
                  ElevatedButton(
                    onPressed: _counter.reset,
                    child: const Text('Reset'),
                  ),
                ],
              ),
              const SizedBox(height: 10),
              Wrap(
                spacing: 10,
                runSpacing: 10,
                alignment: WrapAlignment.center,
                children: [
                  ElevatedButton(
                    onPressed: !_counter.isLoading && _counter.count * 2 <= 100
                        ? () => _counter.multiplyBy(2)
                        : null,
                    child: const Text('× 2'),
                  ),
                  ElevatedButton(
                    onPressed: !_counter.isLoading && _counter.count * 5 <= 100
                        ? () => _counter.multiplyBy(5)
                        : null,
                    child: const Text('× 5'),
                  ),
                  ElevatedButton(
                    onPressed: _counter.canIncrement ? _counter.incrementAsync : null,
                    child: const Text('+ 1 Async'),
                  ),
                ],
              ),
            ],
          ),
        ),
      ),
    );
  }

  void _showInfoDialog() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Counter Info'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Current Value: ${_counter.count}'),
            Text('Can Increment: ${_counter.canIncrement}'),
            Text('Can Decrement: ${_counter.canDecrement}'),
            Text('Is Loading: ${_counter.isLoading}'),
            Text('Has Error: ${_counter.error != null}'),
            const SizedBox(height: 10),
            const Text(
              'This example demonstrates ChangeNotifier for structured '
              'state management with business logic separation.',
            ),
          ],
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.of(context).pop(),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }
}
```

ChangeNotifier provides a structured approach to state management by  
encapsulating state logic within a dedicated class. It separates business  
logic from UI code, supports computed properties, handles async operations,  
and provides error handling. The notifyListeners() method efficiently  
updates only the widgets that are listening to changes.  

## InheritedWidget Pattern

Using InheritedWidget to share state down the widget tree.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'InheritedWidget Pattern',
      home: const AppStateProvider(
        child: HomePage(),
      ),
    );
  }
}

class AppState {
  final String userName;
  final int userScore;
  final bool isDarkMode;
  final String selectedLanguage;

  const AppState({
    required this.userName,
    required this.userScore,
    required this.isDarkMode,
    required this.selectedLanguage,
  });

  AppState copyWith({
    String? userName,
    int? userScore,
    bool? isDarkMode,
    String? selectedLanguage,
  }) {
    return AppState(
      userName: userName ?? this.userName,
      userScore: userScore ?? this.userScore,
      isDarkMode: isDarkMode ?? this.isDarkMode,
      selectedLanguage: selectedLanguage ?? this.selectedLanguage,
    );
  }
}

class AppStateInherited extends InheritedWidget {
  const AppStateInherited({
    super.key,
    required this.appState,
    required this.updateState,
    required super.child,
  });

  final AppState appState;
  final Function(AppState) updateState;

  static AppStateInherited? of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<AppStateInherited>();
  }

  @override
  bool updateShouldNotify(AppStateInherited oldWidget) {
    return appState != oldWidget.appState;
  }
}

class AppStateProvider extends StatefulWidget {
  const AppStateProvider({super.key, required this.child});

  final Widget child;

  @override
  State<AppStateProvider> createState() => _AppStateProviderState();
}

class _AppStateProviderState extends State<AppStateProvider> {
  AppState _appState = const AppState(
    userName: 'Guest User',
    userScore: 0,
    isDarkMode: true,
    selectedLanguage: 'English',
  );

  void _updateState(AppState newState) {
    setState(() {
      _appState = newState;
    });
  }

  @override
  Widget build(BuildContext context) {
    return AppStateInherited(
      appState: _appState,
      updateState: _updateState,
      child: widget.child,
    );
  }
}

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    final appState = AppStateInherited.of(context)!.appState;
    
    return Scaffold(
      appBar: AppBar(
        title: Text('Welcome ${appState.userName}'),
        actions: [
          IconButton(
            icon: const Icon(Icons.settings),
            onPressed: () => Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => const SettingsPage()),
            ),
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'User Profile',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 10),
                    Text('Name: ${appState.userName}'),
                    Text('Score: ${appState.userScore}'),
                    Text('Language: ${appState.selectedLanguage}'),
                    Text('Theme: ${appState.isDarkMode ? 'Dark' : 'Light'}'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 20),
            const ScoreWidget(),
            const SizedBox(height: 20),
            const GameControlsWidget(),
          ],
        ),
      ),
    );
  }
}

class ScoreWidget extends StatelessWidget {
  const ScoreWidget({super.key});

  @override
  Widget build(BuildContext context) {
    final inherited = AppStateInherited.of(context)!;
    final score = inherited.appState.userScore;
    
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            Text(
              'Current Score',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 10),
            Text(
              '$score',
              style: TextStyle(
                fontSize: 36,
                fontWeight: FontWeight.bold,
                color: score >= 100 
                    ? Colors.goldAccent 
                    : score >= 50 
                        ? Colors.greenAccent 
                        : Colors.blueAccent,
              ),
            ),
            const SizedBox(height: 10),
            LinearProgressIndicator(
              value: (score % 100) / 100,
              backgroundColor: Colors.grey[700],
              valueColor: AlwaysStoppedAnimation<Color>(
                score >= 100 ? Colors.goldAccent : Colors.blueAccent,
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class GameControlsWidget extends StatelessWidget {
  const GameControlsWidget({super.key});

  @override
  Widget build(BuildContext context) {
    final inherited = AppStateInherited.of(context)!;
    
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            Text(
              'Game Controls',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 10),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: () {
                      inherited.updateState(
                        inherited.appState.copyWith(
                          userScore: inherited.appState.userScore + 10,
                        ),
                      );
                    },
                    child: const Text('+10 Points'),
                  ),
                ),
                const SizedBox(width: 10),
                Expanded(
                  child: ElevatedButton(
                    onPressed: () {
                      inherited.updateState(
                        inherited.appState.copyWith(
                          userScore: inherited.appState.userScore + 25,
                        ),
                      );
                    },
                    child: const Text('+25 Points'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              onPressed: () {
                inherited.updateState(
                  inherited.appState.copyWith(userScore: 0),
                );
              },
              style: ElevatedButton.styleFrom(
                backgroundColor: Colors.redAccent,
              ),
              child: const Text('Reset Score'),
            ),
          ],
        ),
      ),
    );
  }
}

class SettingsPage extends StatelessWidget {
  const SettingsPage({super.key});

  @override
  Widget build(BuildContext context) {
    final inherited = AppStateInherited.of(context)!;
    final appState = inherited.appState;
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Settings'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'User Name',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 10),
                    TextField(
                      decoration: const InputDecoration(
                        border: OutlineInputBorder(),
                        hintText: 'Enter your name',
                      ),
                      onSubmitted: (value) {
                        if (value.trim().isNotEmpty) {
                          inherited.updateState(
                            appState.copyWith(userName: value.trim()),
                          );
                        }
                      },
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
                    Text(
                      'Language',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 10),
                    DropdownButtonFormField<String>(
                      value: appState.selectedLanguage,
                      decoration: const InputDecoration(
                        border: OutlineInputBorder(),
                      ),
                      items: const [
                        DropdownMenuItem(
                          value: 'English',
                          child: Text('English'),
                        ),
                        DropdownMenuItem(
                          value: 'Spanish',
                          child: Text('Spanish'),
                        ),
                        DropdownMenuItem(
                          value: 'French',
                          child: Text('French'),
                        ),
                        DropdownMenuItem(
                          value: 'German',
                          child: Text('German'),
                        ),
                      ],
                      onChanged: (value) {
                        if (value != null) {
                          inherited.updateState(
                            appState.copyWith(selectedLanguage: value),
                          );
                        }
                      },
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: [
                    Text(
                      'Dark Mode',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    Switch(
                      value: appState.isDarkMode,
                      onChanged: (value) {
                        inherited.updateState(
                          appState.copyWith(isDarkMode: value),
                        );
                      },
                    ),
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

InheritedWidget enables efficient state sharing across the widget tree  
without prop drilling. Widgets can access shared state using the static  
of() method, and Flutter automatically rebuilds dependent widgets when  
the inherited data changes. This pattern works well for app-wide settings,  
themes, and user context that needs to be accessible throughout the app.  

## Stream-Based State

Managing state with StreamController for reactive updates.  

```dart
import 'dart:async';
import 'dart:math';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Stream-Based State',
      home: const StreamStatePage(),
    );
  }
}

class DataPoint {
  final DateTime timestamp;
  final double value;
  final String label;

  DataPoint({
    required this.timestamp,
    required this.value,
    required this.label,
  });
}

class DataStreamController {
  final StreamController<DataPoint> _controller = StreamController<DataPoint>.broadcast();
  final StreamController<List<DataPoint>> _historyController = StreamController<List<DataPoint>>.broadcast();
  final StreamController<String> _statusController = StreamController<String>.broadcast();
  
  Timer? _timer;
  final List<DataPoint> _history = [];
  bool _isActive = false;
  final Random _random = Random();

  Stream<DataPoint> get dataStream => _controller.stream;
  Stream<List<DataPoint>> get historyStream => _historyController.stream;
  Stream<String> get statusStream => _statusController.stream;

  bool get isActive => _isActive;

  void startDataGeneration() {
    if (_isActive) return;
    
    _isActive = true;
    _statusController.add('Active - Generating data...');
    
    _timer = Timer.periodic(const Duration(seconds: 1), (timer) {
      final dataPoint = DataPoint(
        timestamp: DateTime.now(),
        value: _random.nextDouble() * 100,
        label: 'Data ${_history.length + 1}',
      );
      
      _controller.add(dataPoint);
      _history.add(dataPoint);
      
      // Keep only last 20 points
      if (_history.length > 20) {
        _history.removeAt(0);
      }
      
      _historyController.add(List.from(_history));
    });
  }

  void stopDataGeneration() {
    _timer?.cancel();
    _isActive = false;
    _statusController.add('Stopped - No data generation');
  }

  void clearHistory() {
    _history.clear();
    _historyController.add(List.from(_history));
    _statusController.add('History cleared');
  }

  void dispose() {
    _timer?.cancel();
    _controller.close();
    _historyController.close();
    _statusController.close();
  }
}

class StreamStatePage extends StatefulWidget {
  const StreamStatePage({super.key});

  @override
  State<StreamStatePage> createState() => _StreamStatePageState();
}

class _StreamStatePageState extends State<StreamStatePage> {
  late DataStreamController _dataController;

  @override
  void initState() {
    super.initState();
    _dataController = DataStreamController();
  }

  @override
  void dispose() {
    _dataController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Stream-Based State'),
        actions: [
          StreamBuilder<String>(
            stream: _dataController.statusStream,
            initialData: 'Stopped - No data generation',
            builder: (context, snapshot) {
              return Padding(
                padding: const EdgeInsets.all(8.0),
                child: Center(
                  child: Text(
                    _dataController.isActive ? 'LIVE' : 'STOPPED',
                    style: TextStyle(
                      color: _dataController.isActive 
                          ? Colors.greenAccent 
                          : Colors.redAccent,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              );
            },
          ),
        ],
      ),
      body: Column(
        children: [
          // Controls
          Container(
            padding: const EdgeInsets.all(16),
            child: Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: _dataController.isActive 
                        ? null 
                        : _dataController.startDataGeneration,
                    child: const Text('Start Data Stream'),
                  ),
                ),
                const SizedBox(width: 10),
                Expanded(
                  child: ElevatedButton(
                    onPressed: _dataController.isActive 
                        ? _dataController.stopDataGeneration 
                        : null,
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.redAccent,
                    ),
                    child: const Text('Stop Stream'),
                  ),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: _dataController.clearHistory,
                  child: const Text('Clear'),
                ),
              ],
            ),
          ),
          
          // Current Value Display
          StreamBuilder<DataPoint>(
            stream: _dataController.dataStream,
            builder: (context, snapshot) {
              if (!snapshot.hasData) {
                return const Card(
                  margin: EdgeInsets.all(16),
                  child: Padding(
                    padding: EdgeInsets.all(24),
                    child: Center(
                      child: Text(
                        'No data yet\nStart the stream to see live updates',
                        textAlign: TextAlign.center,
                        style: TextStyle(fontSize: 16),
                      ),
                    ),
                  ),
                );
              }
              
              final data = snapshot.data!;
              return Card(
                margin: const EdgeInsets.all(16),
                child: Padding(
                  padding: const EdgeInsets.all(24),
                  child: Column(
                    children: [
                      const Text(
                        'Current Value',
                        style: TextStyle(fontSize: 18),
                      ),
                      const SizedBox(height: 10),
                      Text(
                        data.value.toStringAsFixed(2),
                        style: TextStyle(
                          fontSize: 36,
                          fontWeight: FontWeight.bold,
                          color: data.value > 50 
                              ? Colors.greenAccent 
                              : Colors.orangeAccent,
                        ),
                      ),
                      const SizedBox(height: 10),
                      Text(
                        'Updated: ${data.timestamp.hour.toString().padLeft(2, '0')}:'
                        '${data.timestamp.minute.toString().padLeft(2, '0')}:'
                        '${data.timestamp.second.toString().padLeft(2, '0')}',
                        style: const TextStyle(fontSize: 14),
                      ),
                    ],
                  ),
                ),
              );
            },
          ),
          
          // Status Display
          StreamBuilder<String>(
            stream: _dataController.statusStream,
            initialData: 'Stopped - No data generation',
            builder: (context, snapshot) {
              return Container(
                margin: const EdgeInsets.symmetric(horizontal: 16),
                padding: const EdgeInsets.all(12),
                decoration: BoxDecoration(
                  color: Colors.blueGrey.withOpacity(0.1),
                  border: Border.all(color: Colors.blueGrey),
                  borderRadius: BorderRadius.circular(4),
                ),
                child: Row(
                  children: [
                    const Icon(Icons.info, size: 16),
                    const SizedBox(width: 8),
                    Text(snapshot.data ?? 'Unknown status'),
                  ],
                ),
              );
            },
          ),
          
          const SizedBox(height: 16),
          
          // History Display
          Expanded(
            child: StreamBuilder<List<DataPoint>>(
              stream: _dataController.historyStream,
              initialData: const [],
              builder: (context, snapshot) {
                final history = snapshot.data ?? [];
                
                if (history.isEmpty) {
                  return const Center(
                    child: Text(
                      'No history data\nStart the stream to build history',
                      textAlign: TextAlign.center,
                      style: TextStyle(fontSize: 16),
                    ),
                  );
                }
                
                return Column(
                  children: [
                    Padding(
                      padding: const EdgeInsets.symmetric(horizontal: 16),
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text(
                            'History (${history.length} points)',
                            style: Theme.of(context).textTheme.titleMedium,
                          ),
                          Text(
                            'Avg: ${_calculateAverage(history).toStringAsFixed(1)}',
                            style: const TextStyle(fontSize: 14),
                          ),
                        ],
                      ),
                    ),
                    Expanded(
                      child: ListView.builder(
                        itemCount: history.length,
                        reverse: true,
                        itemBuilder: (context, index) {
                          final item = history[history.length - 1 - index];
                          return ListTile(
                            leading: CircleAvatar(
                              backgroundColor: item.value > 50 
                                  ? Colors.greenAccent 
                                  : Colors.orangeAccent,
                              child: Text(
                                '${index + 1}',
                                style: const TextStyle(
                                  color: Colors.black,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                            ),
                            title: Text(item.value.toStringAsFixed(2)),
                            subtitle: Text(
                              '${item.timestamp.hour.toString().padLeft(2, '0')}:'
                              '${item.timestamp.minute.toString().padLeft(2, '0')}:'
                              '${item.timestamp.second.toString().padLeft(2, '0')}',
                            ),
                            trailing: Container(
                              width: 60,
                              height: 8,
                              decoration: BoxDecoration(
                                color: Colors.grey[800],
                                borderRadius: BorderRadius.circular(4),
                              ),
                              child: FractionallySizedBox(
                                alignment: Alignment.centerLeft,
                                widthFactor: item.value / 100,
                                child: Container(
                                  decoration: BoxDecoration(
                                    color: item.value > 50 
                                        ? Colors.greenAccent 
                                        : Colors.orangeAccent,
                                    borderRadius: BorderRadius.circular(4),
                                  ),
                                ),
                              ),
                            ),
                          );
                        },
                      ),
                    ),
                  ],
                );
              },
            ),
          ),
        ],
      ),
    );
  }

  double _calculateAverage(List<DataPoint> data) {
    if (data.isEmpty) return 0.0;
    final sum = data.fold(0.0, (sum, point) => sum + point.value);
    return sum / data.length;
  }
}
```

Stream-based state management provides reactive programming capabilities  
for real-time data updates. Streams enable decoupled communication between  
data sources and UI components. Multiple widgets can listen to the same  
stream, automatically updating when new data arrives. This pattern excels  
for live data feeds, WebSocket connections, and real-time applications.  

## Multiple Controllers

Managing multiple state controllers for complex application logic.  

```dart
import 'package:flutter/material.dart';
import 'dart:async';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Multiple Controllers',
      home: const MultipleControllersPage(),
    );
  }
}

class UserController extends ChangeNotifier {
  String _name = '';
  String _email = '';
  bool _isLoggedIn = false;
  bool _isLoading = false;

  String get name => _name;
  String get email => _email;
  bool get isLoggedIn => _isLoggedIn;
  bool get isLoading => _isLoading;

  Future<void> login(String name, String email) async {
    _isLoading = true;
    notifyListeners();

    // Simulate network request
    await Future.delayed(const Duration(seconds: 2));

    _name = name;
    _email = email;
    _isLoggedIn = true;
    _isLoading = false;
    notifyListeners();
  }

  void logout() {
    _name = '';
    _email = '';
    _isLoggedIn = false;
    notifyListeners();
  }
}

class ThemeController extends ChangeNotifier {
  bool _isDarkMode = true;
  String _primaryColor = 'blue';
  double _fontSize = 14.0;

  bool get isDarkMode => _isDarkMode;
  String get primaryColor => _primaryColor;
  double get fontSize => _fontSize;

  void toggleTheme() {
    _isDarkMode = !_isDarkMode;
    notifyListeners();
  }

  void setPrimaryColor(String color) {
    _primaryColor = color;
    notifyListeners();
  }

  void setFontSize(double size) {
    _fontSize = size;
    notifyListeners();
  }
}

class NotificationController extends ChangeNotifier {
  final List<String> _notifications = [];
  int _unreadCount = 0;

  List<String> get notifications => List.unmodifiable(_notifications);
  int get unreadCount => _unreadCount;

  void addNotification(String message) {
    _notifications.insert(0, message);
    _unreadCount++;
    notifyListeners();
  }

  void markAllAsRead() {
    _unreadCount = 0;
    notifyListeners();
  }

  void clearNotifications() {
    _notifications.clear();
    _unreadCount = 0;
    notifyListeners();
  }
}

class CounterController extends ChangeNotifier {
  int _count = 0;
  Timer? _autoIncrement;

  int get count => _count;
  bool get isAutoIncrementing => _autoIncrement?.isActive ?? false;

  void increment() {
    _count++;
    notifyListeners();
  }

  void decrement() {
    _count--;
    notifyListeners();
  }

  void reset() {
    _count = 0;
    notifyListeners();
  }

  void startAutoIncrement() {
    _autoIncrement = Timer.periodic(const Duration(seconds: 1), (_) {
      increment();
    });
    notifyListeners();
  }

  void stopAutoIncrement() {
    _autoIncrement?.cancel();
    _autoIncrement = null;
    notifyListeners();
  }

  @override
  void dispose() {
    _autoIncrement?.cancel();
    super.dispose();
  }
}

class MultipleControllersPage extends StatefulWidget {
  const MultipleControllersPage({super.key});

  @override
  State<MultipleControllersPage> createState() => _MultipleControllersPageState();
}

class _MultipleControllersPageState extends State<MultipleControllersPage> {
  late UserController _userController;
  late ThemeController _themeController;
  late NotificationController _notificationController;
  late CounterController _counterController;

  @override
  void initState() {
    super.initState();
    _userController = UserController();
    _themeController = ThemeController();
    _notificationController = NotificationController();
    _counterController = CounterController();

    // Add listeners
    _userController.addListener(_onUserStateChanged);
    _counterController.addListener(_onCounterChanged);
  }

  @override
  void dispose() {
    _userController.removeListener(_onUserStateChanged);
    _counterController.removeListener(_onCounterChanged);
    _userController.dispose();
    _themeController.dispose();
    _notificationController.dispose();
    _counterController.dispose();
    super.dispose();
  }

  void _onUserStateChanged() {
    if (_userController.isLoggedIn) {
      _notificationController.addNotification(
        'Welcome back, ${_userController.name}!'
      );
    }
    setState(() {}); // Trigger rebuild
  }

  void _onCounterChanged() {
    if (_counterController.count % 10 == 0 && _counterController.count > 0) {
      _notificationController.addNotification(
        'Counter reached ${_counterController.count}!'
      );
    }
    setState(() {}); // Trigger rebuild
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Multiple Controllers'),
        actions: [
          Stack(
            children: [
              IconButton(
                icon: const Icon(Icons.notifications),
                onPressed: _showNotifications,
              ),
              if (_notificationController.unreadCount > 0)
                Positioned(
                  right: 8,
                  top: 8,
                  child: Container(
                    padding: const EdgeInsets.all(2),
                    decoration: BoxDecoration(
                      color: Colors.redAccent,
                      borderRadius: BorderRadius.circular(10),
                    ),
                    constraints: const BoxConstraints(
                      minWidth: 16,
                      minHeight: 16,
                    ),
                    child: Text(
                      '${_notificationController.unreadCount}',
                      style: const TextStyle(
                        color: Colors.white,
                        fontSize: 12,
                      ),
                      textAlign: TextAlign.center,
                    ),
                  ),
                ),
            ],
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            // User Section
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'User Controller',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 10),
                    if (!_userController.isLoggedIn) ...[
                      const Text('Not logged in'),
                      const SizedBox(height: 10),
                      ElevatedButton(
                        onPressed: _userController.isLoading
                            ? null
                            : () => _userController.login('John Doe', 'john@example.com'),
                        child: _userController.isLoading
                            ? const CircularProgressIndicator()
                            : const Text('Login'),
                      ),
                    ] else ...[
                      Text('Welcome, ${_userController.name}!'),
                      Text('Email: ${_userController.email}'),
                      const SizedBox(height: 10),
                      ElevatedButton(
                        onPressed: _userController.logout,
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.redAccent,
                        ),
                        child: const Text('Logout'),
                      ),
                    ],
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Theme Section
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Theme Controller',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 10),
                    Row(
                      children: [
                        const Text('Dark Mode'),
                        const Spacer(),
                        Switch(
                          value: _themeController.isDarkMode,
                          onChanged: (_) => _themeController.toggleTheme(),
                        ),
                      ],
                    ),
                    const SizedBox(height: 10),
                    Text('Primary Color: ${_themeController.primaryColor}'),
                    Row(
                      children: [
                        _buildColorButton('blue', Colors.blue),
                        _buildColorButton('green', Colors.green),
                        _buildColorButton('red', Colors.red),
                        _buildColorButton('purple', Colors.purple),
                      ],
                    ),
                    const SizedBox(height: 10),
                    Text('Font Size: ${_themeController.fontSize.toInt()}'),
                    Slider(
                      value: _themeController.fontSize,
                      min: 12,
                      max: 20,
                      divisions: 8,
                      onChanged: _themeController.setFontSize,
                    ),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Counter Section
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    Text(
                      'Counter Controller',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 10),
                    Text(
                      '${_counterController.count}',
                      style: const TextStyle(
                        fontSize: 36,
                        fontWeight: FontWeight.bold,
                        color: Colors.blueAccent,
                      ),
                    ),
                    const SizedBox(height: 10),
                    Row(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        ElevatedButton(
                          onPressed: _counterController.decrement,
                          child: const Text('-'),
                        ),
                        const SizedBox(width: 10),
                        ElevatedButton(
                          onPressed: _counterController.reset,
                          child: const Text('Reset'),
                        ),
                        const SizedBox(width: 10),
                        ElevatedButton(
                          onPressed: _counterController.increment,
                          child: const Text('+'),
                        ),
                      ],
                    ),
                    const SizedBox(height: 10),
                    ElevatedButton(
                      onPressed: _counterController.isAutoIncrementing
                          ? _counterController.stopAutoIncrement
                          : _counterController.startAutoIncrement,
                      style: ElevatedButton.styleFrom(
                        backgroundColor: _counterController.isAutoIncrementing
                            ? Colors.redAccent
                            : Colors.greenAccent,
                      ),
                      child: Text(
                        _counterController.isAutoIncrementing
                            ? 'Stop Auto'
                            : 'Start Auto',
                      ),
                    ),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildColorButton(String colorName, Color color) {
    return Padding(
      padding: const EdgeInsets.only(right: 8),
      child: GestureDetector(
        onTap: () => _themeController.setPrimaryColor(colorName),
        child: Container(
          width: 30,
          height: 30,
          decoration: BoxDecoration(
            color: color,
            shape: BoxShape.circle,
            border: _themeController.primaryColor == colorName
                ? Border.all(color: Colors.white, width: 3)
                : null,
          ),
        ),
      ),
    );
  }

  void _showNotifications() {
    _notificationController.markAllAsRead();
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Notifications'),
        content: SizedBox(
          width: double.maxFinite,
          height: 300,
          child: _notificationController.notifications.isEmpty
              ? const Center(child: Text('No notifications'))
              : ListView.builder(
                  itemCount: _notificationController.notifications.length,
                  itemBuilder: (context, index) {
                    return ListTile(
                      leading: const Icon(Icons.info),
                      title: Text(_notificationController.notifications[index]),
                    );
                  },
                ),
        ),
        actions: [
          TextButton(
            onPressed: () {
              _notificationController.clearNotifications();
              Navigator.of(context).pop();
            },
            child: const Text('Clear All'),
          ),
          TextButton(
            onPressed: () => Navigator.of(context).pop(),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }
}
```

Managing multiple controllers demonstrates separation of concerns in state  
management. Each controller handles a specific domain (user, theme,  
notifications, counter) with its own responsibilities and state. Controllers  
can communicate through listeners and callbacks, enabling coordinated  
behavior while maintaining modularity and testability.  

## Global State with Singleton

Using the singleton pattern for global state management.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Global State Singleton',
      home: const HomePage(),
    );
  }
}

class AppGlobalState extends ChangeNotifier {
  static final AppGlobalState _instance = AppGlobalState._internal();
  factory AppGlobalState() => _instance;
  AppGlobalState._internal();

  // App settings
  String _appLanguage = 'English';
  bool _notificationsEnabled = true;
  double _volume = 0.5;
  
  // User preferences
  Map<String, dynamic> _userPreferences = {
    'username': 'Guest',
    'favoriteColor': 'blue',
    'theme': 'dark',
    'autoSave': true,
  };
  
  // Application data
  List<String> _recentSearches = [];
  Map<String, int> _usageStats = {};

  // Getters
  String get appLanguage => _appLanguage;
  bool get notificationsEnabled => _notificationsEnabled;
  double get volume => _volume;
  Map<String, dynamic> get userPreferences => Map.from(_userPreferences);
  List<String> get recentSearches => List.from(_recentSearches);
  Map<String, int> get usageStats => Map.from(_usageStats);

  // App settings methods
  void setLanguage(String language) {
    _appLanguage = language;
    _incrementUsage('language_change');
    notifyListeners();
  }

  void setNotifications(bool enabled) {
    _notificationsEnabled = enabled;
    _incrementUsage('notifications_toggle');
    notifyListeners();
  }

  void setVolume(double volume) {
    _volume = volume.clamp(0.0, 1.0);
    notifyListeners();
  }

  // User preferences methods
  void setUserPreference(String key, dynamic value) {
    _userPreferences[key] = value;
    _incrementUsage('preference_change');
    notifyListeners();
  }

  T? getUserPreference<T>(String key) {
    return _userPreferences[key] as T?;
  }

  // Search methods
  void addRecentSearch(String query) {
    query = query.trim();
    if (query.isEmpty) return;
    
    _recentSearches.remove(query); // Remove if exists
    _recentSearches.insert(0, query); // Add to beginning
    
    // Keep only last 10 searches
    if (_recentSearches.length > 10) {
      _recentSearches.removeLast();
    }
    
    _incrementUsage('search');
    notifyListeners();
  }

  void clearRecentSearches() {
    _recentSearches.clear();
    notifyListeners();
  }

  // Usage statistics
  void _incrementUsage(String action) {
    _usageStats[action] = (_usageStats[action] ?? 0) + 1;
  }

  int getUsageCount(String action) {
    return _usageStats[action] ?? 0;
  }

  void resetUsageStats() {
    _usageStats.clear();
    notifyListeners();
  }

  // Utility methods
  void resetAllData() {
    _appLanguage = 'English';
    _notificationsEnabled = true;
    _volume = 0.5;
    _userPreferences.clear();
    _recentSearches.clear();
    _usageStats.clear();
    notifyListeners();
  }
}

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  final AppGlobalState _globalState = AppGlobalState();
  final TextEditingController _searchController = TextEditingController();

  @override
  void initState() {
    super.initState();
    _globalState.addListener(_onGlobalStateChanged);
  }

  @override
  void dispose() {
    _globalState.removeListener(_onGlobalStateChanged);
    _searchController.dispose();
    super.dispose();
  }

  void _onGlobalStateChanged() {
    setState(() {
      // Rebuild when global state changes
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Hello ${_globalState.getUserPreference('username') ?? 'Guest'}'),
        actions: [
          IconButton(
            icon: const Icon(Icons.settings),
            onPressed: () => Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => const SettingsPage()),
            ),
          ),
          IconButton(
            icon: const Icon(Icons.bar_chart),
            onPressed: () => Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => const StatsPage()),
            ),
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // User Info Card
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'User Information',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 8),
                    Text('Language: ${_globalState.appLanguage}'),
                    Text('Notifications: ${_globalState.notificationsEnabled ? 'On' : 'Off'}'),
                    Text('Volume: ${(_globalState.volume * 100).toInt()}%'),
                    Text('Favorite Color: ${_globalState.getUserPreference('favoriteColor')}'),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Search Section
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Search',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 8),
                    Row(
                      children: [
                        Expanded(
                          child: TextField(
                            controller: _searchController,
                            decoration: const InputDecoration(
                              hintText: 'Enter search term...',
                              border: OutlineInputBorder(),
                            ),
                            onSubmitted: (query) {
                              if (query.trim().isNotEmpty) {
                                _globalState.addRecentSearch(query);
                                _searchController.clear();
                              }
                            },
                          ),
                        ),
                        const SizedBox(width: 8),
                        ElevatedButton(
                          onPressed: () {
                            final query = _searchController.text;
                            if (query.trim().isNotEmpty) {
                              _globalState.addRecentSearch(query);
                              _searchController.clear();
                            }
                          },
                          child: const Text('Search'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Recent Searches
            if (_globalState.recentSearches.isNotEmpty) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text(
                            'Recent Searches',
                            style: Theme.of(context).textTheme.titleMedium,
                          ),
                          TextButton(
                            onPressed: _globalState.clearRecentSearches,
                            child: const Text('Clear'),
                          ),
                        ],
                      ),
                      const SizedBox(height: 8),
                      Wrap(
                        spacing: 8,
                        runSpacing: 8,
                        children: _globalState.recentSearches.map((search) {
                          return Chip(
                            label: Text(search),
                            onDeleted: () {
                              setState(() {
                                _globalState.recentSearches.remove(search);
                              });
                            },
                          );
                        }).toList(),
                      ),
                    ],
                  ),
                ),
              ),
            ],

            const Spacer(),

            // Global Actions
            Row(
              children: [
                Expanded(
                  child: ElevatedButton(
                    onPressed: () => Navigator.push(
                      context,
                      MaterialPageRoute(builder: (context) => const SecondPage()),
                    ),
                    child: const Text('Go to Second Page'),
                  ),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: () {
                    showDialog(
                      context: context,
                      builder: (context) => AlertDialog(
                        title: const Text('Reset Data'),
                        content: const Text('Are you sure you want to reset all data?'),
                        actions: [
                          TextButton(
                            onPressed: () => Navigator.pop(context),
                            child: const Text('Cancel'),
                          ),
                          ElevatedButton(
                            onPressed: () {
                              _globalState.resetAllData();
                              Navigator.pop(context);
                            },
                            style: ElevatedButton.styleFrom(
                              backgroundColor: Colors.redAccent,
                            ),
                            child: const Text('Reset'),
                          ),
                        ],
                      ),
                    );
                  },
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.redAccent,
                  ),
                  child: const Text('Reset'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}

class SecondPage extends StatefulWidget {
  const SecondPage({super.key});

  @override
  State<SecondPage> createState() => _SecondPageState();
}

class _SecondPageState extends State<SecondPage> {
  final AppGlobalState _globalState = AppGlobalState();

  @override
  void initState() {
    super.initState();
    _globalState.addListener(_onGlobalStateChanged);
  }

  @override
  void dispose() {
    _globalState.removeListener(_onGlobalStateChanged);
    super.dispose();
  }

  void _onGlobalStateChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Second Page'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Global State Access',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 8),
                    Text('Current language: ${_globalState.appLanguage}'),
                    Text('Total searches: ${_globalState.getUsageCount('search')}'),
                    Text('Recent searches: ${_globalState.recentSearches.length}'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: () {
                _globalState.setUserPreference(
                  'lastVisited',
                  DateTime.now().toString(),
                );
              },
              child: const Text('Update Last Visited'),
            ),
          ],
        ),
      ),
    );
  }
}

class SettingsPage extends StatefulWidget {
  const SettingsPage({super.key});

  @override
  State<SettingsPage> createState() => _SettingsPageState();
}

class _SettingsPageState extends State<SettingsPage> {
  final AppGlobalState _globalState = AppGlobalState();

  @override
  void initState() {
    super.initState();
    _globalState.addListener(_onGlobalStateChanged);
  }

  @override
  void dispose() {
    _globalState.removeListener(_onGlobalStateChanged);
    super.dispose();
  }

  void _onGlobalStateChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Settings'),
      ),
      body: ListView(
        padding: const EdgeInsets.all(16),
        children: [
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Language',
                    style: Theme.of(context).textTheme.titleMedium,
                  ),
                  DropdownButton<String>(
                    value: _globalState.appLanguage,
                    isExpanded: true,
                    items: ['English', 'Spanish', 'French', 'German']
                        .map((lang) => DropdownMenuItem(
                              value: lang,
                              child: Text(lang),
                            ))
                        .toList(),
                    onChanged: (lang) {
                      if (lang != null) _globalState.setLanguage(lang);
                    },
                  ),
                ],
              ),
            ),
          ),
          const SizedBox(height: 8),
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  Text(
                    'Notifications',
                    style: Theme.of(context).textTheme.titleMedium,
                  ),
                  Switch(
                    value: _globalState.notificationsEnabled,
                    onChanged: _globalState.setNotifications,
                  ),
                ],
              ),
            ),
          ),
          const SizedBox(height: 8),
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Volume: ${(_globalState.volume * 100).toInt()}%',
                    style: Theme.of(context).textTheme.titleMedium,
                  ),
                  Slider(
                    value: _globalState.volume,
                    onChanged: _globalState.setVolume,
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }
}

class StatsPage extends StatefulWidget {
  const StatsPage({super.key});

  @override
  State<StatsPage> createState() => _StatsPageState();
}

class _StatsPageState extends State<StatsPage> {
  final AppGlobalState _globalState = AppGlobalState();

  @override
  void initState() {
    super.initState();
    _globalState.addListener(_onGlobalStateChanged);
  }

  @override
  void dispose() {
    _globalState.removeListener(_onGlobalStateChanged);
    super.dispose();
  }

  void _onGlobalStateChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Usage Statistics'),
        actions: [
          TextButton(
            onPressed: _globalState.resetUsageStats,
            child: const Text('Reset'),
          ),
        ],
      ),
      body: ListView(
        padding: const EdgeInsets.all(16),
        children: [
          if (_globalState.usageStats.isEmpty)
            const Card(
              child: Padding(
                padding: EdgeInsets.all(16),
                child: Text('No usage statistics available yet.'),
              ),
            )
          else
            ..._globalState.usageStats.entries.map((entry) {
              return Card(
                child: ListTile(
                  title: Text(entry.key.replaceAll('_', ' ').toUpperCase()),
                  trailing: Text(
                    '${entry.value}',
                    style: const TextStyle(
                      fontSize: 18,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              );
            }).toList(),
        ],
      ),
    );
  }
}
```

Singleton pattern provides global state access throughout the entire  
application. The singleton instance ensures data consistency and enables  
cross-page state sharing. This pattern works well for app settings, user  
preferences, and global data that needs to persist across navigation.  
However, use it judiciously as it can create tight coupling if overused.  

## State Persistence

Saving and loading state to maintain data across app sessions.  

```dart
import 'package:flutter/material.dart';
import 'dart:convert';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'State Persistence',
      home: const PersistentStatePage(),
    );
  }
}

class AppData {
  String userName;
  int score;
  List<String> achievements;
  DateTime lastLoginDate;
  Map<String, dynamic> settings;

  AppData({
    required this.userName,
    required this.score,
    required this.achievements,
    required this.lastLoginDate,
    required this.settings,
  });

  Map<String, dynamic> toJson() {
    return {
      'userName': userName,
      'score': score,
      'achievements': achievements,
      'lastLoginDate': lastLoginDate.toIso8601String(),
      'settings': settings,
    };
  }

  factory AppData.fromJson(Map<String, dynamic> json) {
    return AppData(
      userName: json['userName'] ?? 'Guest',
      score: json['score'] ?? 0,
      achievements: List<String>.from(json['achievements'] ?? []),
      lastLoginDate: json['lastLoginDate'] != null
          ? DateTime.parse(json['lastLoginDate'])
          : DateTime.now(),
      settings: Map<String, dynamic>.from(json['settings'] ?? {}),
    );
  }

  AppData copyWith({
    String? userName,
    int? score,
    List<String>? achievements,
    DateTime? lastLoginDate,
    Map<String, dynamic>? settings,
  }) {
    return AppData(
      userName: userName ?? this.userName,
      score: score ?? this.score,
      achievements: achievements ?? List.from(this.achievements),
      lastLoginDate: lastLoginDate ?? this.lastLoginDate,
      settings: settings ?? Map.from(this.settings),
    );
  }
}

// Mock storage implementation (in real app, use SharedPreferences or similar)
class MockStorage {
  static final Map<String, String> _storage = {};

  static Future<void> setString(String key, String value) async {
    await Future.delayed(const Duration(milliseconds: 100)); // Simulate async
    _storage[key] = value;
  }

  static Future<String?> getString(String key) async {
    await Future.delayed(const Duration(milliseconds: 100)); // Simulate async
    return _storage[key];
  }

  static Future<void> remove(String key) async {
    await Future.delayed(const Duration(milliseconds: 50)); // Simulate async
    _storage.remove(key);
  }

  static Future<void> clear() async {
    await Future.delayed(const Duration(milliseconds: 50)); // Simulate async
    _storage.clear();
  }
}

class PersistentStateController extends ChangeNotifier {
  static const String _storageKey = 'app_data';
  
  AppData _appData = AppData(
    userName: 'Guest',
    score: 0,
    achievements: [],
    lastLoginDate: DateTime.now(),
    settings: {
      'theme': 'dark',
      'notifications': true,
      'autoSave': true,
    },
  );

  bool _isLoading = false;
  bool _isSaving = false;
  String? _error;

  AppData get appData => _appData;
  bool get isLoading => _isLoading;
  bool get isSaving => _isSaving;
  String? get error => _error;

  Future<void> loadData() async {
    _isLoading = true;
    _error = null;
    notifyListeners();

    try {
      final jsonString = await MockStorage.getString(_storageKey);
      
      if (jsonString != null) {
        final jsonData = jsonDecode(jsonString) as Map<String, dynamic>;
        _appData = AppData.fromJson(jsonData);
      }
    } catch (e) {
      _error = 'Failed to load data: $e';
    } finally {
      _isLoading = false;
      notifyListeners();
    }
  }

  Future<void> saveData() async {
    _isSaving = true;
    _error = null;
    notifyListeners();

    try {
      final jsonString = jsonEncode(_appData.toJson());
      await MockStorage.setString(_storageKey, jsonString);
    } catch (e) {
      _error = 'Failed to save data: $e';
    } finally {
      _isSaving = false;
      notifyListeners();
    }
  }

  void updateUserName(String name) {
    _appData = _appData.copyWith(userName: name);
    notifyListeners();
    if (_appData.settings['autoSave'] == true) {
      saveData();
    }
  }

  void addScore(int points) {
    final newScore = _appData.score + points;
    _appData = _appData.copyWith(score: newScore);
    
    // Check for achievements
    _checkAchievements(newScore);
    
    notifyListeners();
    if (_appData.settings['autoSave'] == true) {
      saveData();
    }
  }

  void _checkAchievements(int score) {
    final achievements = List<String>.from(_appData.achievements);
    
    if (score >= 100 && !achievements.contains('Century Club')) {
      achievements.add('Century Club');
    }
    if (score >= 500 && !achievements.contains('High Achiever')) {
      achievements.add('High Achiever');
    }
    if (score >= 1000 && !achievements.contains('Score Master')) {
      achievements.add('Score Master');
    }
    
    if (achievements.length != _appData.achievements.length) {
      _appData = _appData.copyWith(achievements: achievements);
    }
  }

  void updateSetting(String key, dynamic value) {
    final settings = Map<String, dynamic>.from(_appData.settings);
    settings[key] = value;
    _appData = _appData.copyWith(settings: settings);
    notifyListeners();
    
    if (key != 'autoSave' && settings['autoSave'] == true) {
      saveData();
    }
  }

  void resetScore() {
    _appData = _appData.copyWith(score: 0, achievements: []);
    notifyListeners();
    if (_appData.settings['autoSave'] == true) {
      saveData();
    }
  }

  Future<void> clearAllData() async {
    try {
      await MockStorage.remove(_storageKey);
      _appData = AppData(
        userName: 'Guest',
        score: 0,
        achievements: [],
        lastLoginDate: DateTime.now(),
        settings: {
          'theme': 'dark',
          'notifications': true,
          'autoSave': true,
        },
      );
      notifyListeners();
    } catch (e) {
      _error = 'Failed to clear data: $e';
      notifyListeners();
    }
  }

  void recordLogin() {
    _appData = _appData.copyWith(lastLoginDate: DateTime.now());
    notifyListeners();
    if (_appData.settings['autoSave'] == true) {
      saveData();
    }
  }
}

class PersistentStatePage extends StatefulWidget {
  const PersistentStatePage({super.key});

  @override
  State<PersistentStatePage> createState() => _PersistentStatePageState();
}

class _PersistentStatePageState extends State<PersistentStatePage> {
  late PersistentStateController _controller;
  final TextEditingController _nameController = TextEditingController();

  @override
  void initState() {
    super.initState();
    _controller = PersistentStateController();
    _controller.addListener(_onStateChanged);
    _loadInitialData();
  }

  @override
  void dispose() {
    _controller.removeListener(_onStateChanged);
    _controller.dispose();
    _nameController.dispose();
    super.dispose();
  }

  void _onStateChanged() {
    setState(() {});
  }

  Future<void> _loadInitialData() async {
    await _controller.loadData();
    _nameController.text = _controller.appData.userName;
    _controller.recordLogin();
  }

  @override
  Widget build(BuildContext context) {
    final appData = _controller.appData;
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Persistent State'),
        actions: [
          if (_controller.isSaving)
            const Padding(
              padding: EdgeInsets.all(16),
              child: SizedBox(
                width: 16,
                height: 16,
                child: CircularProgressIndicator(strokeWidth: 2),
              ),
            )
          else
            IconButton(
              icon: const Icon(Icons.save),
              onPressed: _controller.saveData,
            ),
        ],
      ),
      body: _controller.isLoading
          ? const Center(child: CircularProgressIndicator())
          : SingleChildScrollView(
              padding: const EdgeInsets.all(16),
              child: Column(
                children: [
                  if (_controller.error != null) ...[
                    Card(
                      color: Colors.red.withOpacity(0.1),
                      child: Padding(
                        padding: const EdgeInsets.all(16),
                        child: Row(
                          children: [
                            const Icon(Icons.error, color: Colors.red),
                            const SizedBox(width: 8),
                            Expanded(
                              child: Text(
                                _controller.error!,
                                style: const TextStyle(color: Colors.red),
                              ),
                            ),
                          ],
                        ),
                      ),
                    ),
                    const SizedBox(height: 16),
                  ],
                  
                  // User Info Card
                  Card(
                    child: Padding(
                      padding: const EdgeInsets.all(16),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            'User Profile',
                            style: Theme.of(context).textTheme.titleMedium,
                          ),
                          const SizedBox(height: 10),
                          TextField(
                            controller: _nameController,
                            decoration: const InputDecoration(
                              labelText: 'User Name',
                              border: OutlineInputBorder(),
                            ),
                            onSubmitted: (value) {
                              if (value.trim().isNotEmpty) {
                                _controller.updateUserName(value.trim());
                              }
                            },
                          ),
                          const SizedBox(height: 10),
                          Text(
                            'Last Login: ${_formatDate(appData.lastLoginDate)}',
                          ),
                        ],
                      ),
                    ),
                  ),

                  const SizedBox(height: 16),

                  // Score Card
                  Card(
                    child: Padding(
                      padding: const EdgeInsets.all(16),
                      child: Column(
                        children: [
                          Text(
                            'Score',
                            style: Theme.of(context).textTheme.titleMedium,
                          ),
                          const SizedBox(height: 10),
                          Text(
                            '${appData.score}',
                            style: const TextStyle(
                              fontSize: 36,
                              fontWeight: FontWeight.bold,
                              color: Colors.greenAccent,
                            ),
                          ),
                          const SizedBox(height: 10),
                          Row(
                            mainAxisAlignment: MainAxisAlignment.center,
                            children: [
                              ElevatedButton(
                                onPressed: () => _controller.addScore(10),
                                child: const Text('+10'),
                              ),
                              const SizedBox(width: 10),
                              ElevatedButton(
                                onPressed: () => _controller.addScore(50),
                                child: const Text('+50'),
                              ),
                              const SizedBox(width: 10),
                              ElevatedButton(
                                onPressed: () => _controller.addScore(100),
                                child: const Text('+100'),
                              ),
                            ],
                          ),
                          const SizedBox(height: 10),
                          ElevatedButton(
                            onPressed: _controller.resetScore,
                            style: ElevatedButton.styleFrom(
                              backgroundColor: Colors.redAccent,
                            ),
                            child: const Text('Reset Score'),
                          ),
                        ],
                      ),
                    ),
                  ),

                  const SizedBox(height: 16),

                  // Achievements Card
                  Card(
                    child: Padding(
                      padding: const EdgeInsets.all(16),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            'Achievements (${appData.achievements.length})',
                            style: Theme.of(context).textTheme.titleMedium,
                          ),
                          const SizedBox(height: 10),
                          if (appData.achievements.isEmpty)
                            const Text('No achievements yet. Keep scoring!')
                          else
                            Wrap(
                              spacing: 8,
                              runSpacing: 8,
                              children: appData.achievements.map((achievement) {
                                return Chip(
                                  avatar: const Icon(Icons.star, size: 16),
                                  label: Text(achievement),
                                  backgroundColor: Colors.goldAccent.withOpacity(0.2),
                                );
                              }).toList(),
                            ),
                        ],
                      ),
                    ),
                  ),

                  const SizedBox(height: 16),

                  // Settings Card
                  Card(
                    child: Padding(
                      padding: const EdgeInsets.all(16),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            'Settings',
                            style: Theme.of(context).textTheme.titleMedium,
                          ),
                          const SizedBox(height: 10),
                          SwitchListTile(
                            title: const Text('Notifications'),
                            value: appData.settings['notifications'] ?? false,
                            onChanged: (value) {
                              _controller.updateSetting('notifications', value);
                            },
                          ),
                          SwitchListTile(
                            title: const Text('Auto Save'),
                            subtitle: const Text('Automatically save changes'),
                            value: appData.settings['autoSave'] ?? false,
                            onChanged: (value) {
                              _controller.updateSetting('autoSave', value);
                            },
                          ),
                        ],
                      ),
                    ),
                  ),

                  const SizedBox(height: 16),

                  // Action Buttons
                  Row(
                    children: [
                      Expanded(
                        child: ElevatedButton(
                          onPressed: _controller.saveData,
                          child: const Text('Manual Save'),
                        ),
                      ),
                      const SizedBox(width: 10),
                      Expanded(
                        child: ElevatedButton(
                          onPressed: () {
                            showDialog(
                              context: context,
                              builder: (context) => AlertDialog(
                                title: const Text('Clear All Data'),
                                content: const Text(
                                  'This will permanently delete all saved data. Are you sure?',
                                ),
                                actions: [
                                  TextButton(
                                    onPressed: () => Navigator.pop(context),
                                    child: const Text('Cancel'),
                                  ),
                                  ElevatedButton(
                                    onPressed: () {
                                      _controller.clearAllData();
                                      _nameController.text = 'Guest';
                                      Navigator.pop(context);
                                    },
                                    style: ElevatedButton.styleFrom(
                                      backgroundColor: Colors.redAccent,
                                    ),
                                    child: const Text('Clear'),
                                  ),
                                ],
                              ),
                            );
                          },
                          style: ElevatedButton.styleFrom(
                            backgroundColor: Colors.redAccent,
                          ),
                          child: const Text('Clear All'),
                        ),
                      ),
                    ],
                  ),
                ],
              ),
            ),
    );
  }

  String _formatDate(DateTime date) {
    return '${date.day}/${date.month}/${date.year} '
           '${date.hour.toString().padLeft(2, '0')}:'
           '${date.minute.toString().padLeft(2, '0')}';
  }
}
```

State persistence enables data to survive app restarts and device reboots.  
This example demonstrates serializing complex state to JSON, handling  
loading/saving operations with proper error handling, and implementing  
auto-save functionality. Real applications would use SharedPreferences,  
SQLite, or cloud storage for persistence. This approach ensures user data  
and preferences are maintained across app sessions.

## Async State Management

Handling asynchronous operations and loading states effectively.  

```dart
import 'dart:async';
import 'dart:math';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Async State Management',
      home: const AsyncStatePage(),
    );
  }
}

class ApiService {
  static final Random _random = Random();
  
  static Future<List<String>> fetchUsers() async {
    await Future.delayed(Duration(seconds: 1 + _random.nextInt(3)));
    
    if (_random.nextDouble() < 0.2) {
      throw Exception('Failed to fetch users');
    }
    
    return [
      'Alice Johnson',
      'Bob Smith',
      'Charlie Brown',
      'Diana Prince',
      'Edward Norton',
      'Fiona Apple',
      'George Lucas',
      'Hannah Montana',
    ];
  }
  
  static Future<Map<String, dynamic>> fetchUserDetails(String name) async {
    await Future.delayed(Duration(milliseconds: 500 + _random.nextInt(1000)));
    
    if (_random.nextDouble() < 0.15) {
      throw Exception('User not found');
    }
    
    return {
      'name': name,
      'email': '${name.toLowerCase().replaceAll(' ', '.')}@example.com',
      'score': _random.nextInt(1000),
      'level': _random.nextInt(10) + 1,
      'joinDate': DateTime.now().subtract(Duration(days: _random.nextInt(365))),
    };
  }
  
  static Future<bool> updateUserScore(String name, int newScore) async {
    await Future.delayed(Duration(milliseconds: 800 + _random.nextInt(500)));
    
    if (_random.nextDouble() < 0.1) {
      throw Exception('Failed to update score');
    }
    
    return true;
  }
}

enum AsyncState { idle, loading, success, error }

class UserData {
  final String name;
  final String email;
  final int score;
  final int level;
  final DateTime joinDate;
  
  UserData({
    required this.name,
    required this.email,
    required this.score,
    required this.level,
    required this.joinDate,
  });
  
  factory UserData.fromJson(Map<String, dynamic> json) {
    return UserData(
      name: json['name'],
      email: json['email'],
      score: json['score'],
      level: json['level'],
      joinDate: json['joinDate'],
    );
  }
  
  UserData copyWith({
    String? name,
    String? email,
    int? score,
    int? level,
    DateTime? joinDate,
  }) {
    return UserData(
      name: name ?? this.name,
      email: email ?? this.email,
      score: score ?? this.score,
      level: level ?? this.level,
      joinDate: joinDate ?? this.joinDate,
    );
  }
}

class AsyncStateController extends ChangeNotifier {
  AsyncState _usersState = AsyncState.idle;
  AsyncState _userDetailsState = AsyncState.idle;
  AsyncState _updateState = AsyncState.idle;
  
  List<String> _users = [];
  UserData? _selectedUser;
  String? _usersError;
  String? _detailsError;
  String? _updateError;
  
  // Getters
  AsyncState get usersState => _usersState;
  AsyncState get userDetailsState => _userDetailsState;
  AsyncState get updateState => _updateState;
  List<String> get users => _users;
  UserData? get selectedUser => _selectedUser;
  String? get usersError => _usersError;
  String? get detailsError => _detailsError;
  String? get updateError => _updateError;
  
  bool get isLoadingUsers => _usersState == AsyncState.loading;
  bool get isLoadingDetails => _userDetailsState == AsyncState.loading;
  bool get isUpdating => _updateState == AsyncState.loading;
  
  Future<void> fetchUsers() async {
    _usersState = AsyncState.loading;
    _usersError = null;
    notifyListeners();
    
    try {
      _users = await ApiService.fetchUsers();
      _usersState = AsyncState.success;
    } catch (e) {
      _usersError = e.toString();
      _usersState = AsyncState.error;
    }
    
    notifyListeners();
  }
  
  Future<void> fetchUserDetails(String userName) async {
    _userDetailsState = AsyncState.loading;
    _detailsError = null;
    _selectedUser = null;
    notifyListeners();
    
    try {
      final details = await ApiService.fetchUserDetails(userName);
      _selectedUser = UserData.fromJson(details);
      _userDetailsState = AsyncState.success;
    } catch (e) {
      _detailsError = e.toString();
      _userDetailsState = AsyncState.error;
    }
    
    notifyListeners();
  }
  
  Future<void> updateUserScore(int newScore) async {
    if (_selectedUser == null) return;
    
    _updateState = AsyncState.loading;
    _updateError = null;
    notifyListeners();
    
    try {
      await ApiService.updateUserScore(_selectedUser!.name, newScore);
      _selectedUser = _selectedUser!.copyWith(score: newScore);
      _updateState = AsyncState.success;
      
      // Reset update state after a delay
      Timer(const Duration(seconds: 2), () {
        _updateState = AsyncState.idle;
        notifyListeners();
      });
    } catch (e) {
      _updateError = e.toString();
      _updateState = AsyncState.error;
    }
    
    notifyListeners();
  }
  
  void clearErrors() {
    _usersError = null;
    _detailsError = null;
    _updateError = null;
    notifyListeners();
  }
  
  void resetStates() {
    _usersState = AsyncState.idle;
    _userDetailsState = AsyncState.idle;
    _updateState = AsyncState.idle;
    _users.clear();
    _selectedUser = null;
    clearErrors();
    notifyListeners();
  }
}

class AsyncStatePage extends StatefulWidget {
  const AsyncStatePage({super.key});

  @override
  State<AsyncStatePage> createState() => _AsyncStatePageState();
}

class _AsyncStatePageState extends State<AsyncStatePage> {
  late AsyncStateController _controller;
  final TextEditingController _scoreController = TextEditingController();

  @override
  void initState() {
    super.initState();
    _controller = AsyncStateController();
    _controller.addListener(_onStateChanged);
  }

  @override
  void dispose() {
    _controller.removeListener(_onStateChanged);
    _controller.dispose();
    _scoreController.dispose();
    super.dispose();
  }

  void _onStateChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Async State Management'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: _controller.isLoadingUsers ? null : _controller.fetchUsers,
          ),
          IconButton(
            icon: const Icon(Icons.clear),
            onPressed: _controller.resetStates,
          ),
        ],
      ),
      body: Column(
        children: [
          // Users Section
          Card(
            margin: const EdgeInsets.all(16),
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Row(
                    children: [
                      Text(
                        'Users',
                        style: Theme.of(context).textTheme.titleMedium,
                      ),
                      const Spacer(),
                      if (_controller.isLoadingUsers)
                        const SizedBox(
                          width: 16,
                          height: 16,
                          child: CircularProgressIndicator(strokeWidth: 2),
                        ),
                    ],
                  ),
                  const SizedBox(height: 10),
                  
                  if (_controller.usersError != null) ...[
                    Container(
                      padding: const EdgeInsets.all(8),
                      decoration: BoxDecoration(
                        color: Colors.red.withOpacity(0.1),
                        border: Border.all(color: Colors.red),
                        borderRadius: BorderRadius.circular(4),
                      ),
                      child: Row(
                        children: [
                          const Icon(Icons.error, color: Colors.red),
                          const SizedBox(width: 8),
                          Expanded(child: Text(_controller.usersError!)),
                          TextButton(
                            onPressed: _controller.fetchUsers,
                            child: const Text('Retry'),
                          ),
                        ],
                      ),
                    ),
                  ] else if (_controller.users.isEmpty && !_controller.isLoadingUsers) ...[
                    ElevatedButton(
                      onPressed: _controller.fetchUsers,
                      child: const Text('Load Users'),
                    ),
                  ] else if (_controller.users.isNotEmpty) ...[
                    SizedBox(
                      height: 100,
                      child: ListView.builder(
                        scrollDirection: Axis.horizontal,
                        itemCount: _controller.users.length,
                        itemBuilder: (context, index) {
                          final user = _controller.users[index];
                          return Padding(
                            padding: const EdgeInsets.only(right: 8),
                            child: ActionChip(
                              label: Text(user),
                              onPressed: _controller.isLoadingDetails
                                  ? null
                                  : () => _controller.fetchUserDetails(user),
                              backgroundColor: _controller.selectedUser?.name == user
                                  ? Colors.blueAccent.withOpacity(0.3)
                                  : null,
                            ),
                          );
                        },
                      ),
                    ),
                  ],
                ],
              ),
            ),
          ),

          // User Details Section
          Expanded(
            child: Card(
              margin: const EdgeInsets.all(16),
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        Text(
                          'User Details',
                          style: Theme.of(context).textTheme.titleMedium,
                        ),
                        const Spacer(),
                        if (_controller.isLoadingDetails)
                          const SizedBox(
                            width: 16,
                            height: 16,
                            child: CircularProgressIndicator(strokeWidth: 2),
                          ),
                      ],
                    ),
                    const SizedBox(height: 10),
                    
                    if (_controller.detailsError != null) ...[
                      Container(
                        padding: const EdgeInsets.all(8),
                        decoration: BoxDecoration(
                          color: Colors.red.withOpacity(0.1),
                          border: Border.all(color: Colors.red),
                          borderRadius: BorderRadius.circular(4),
                        ),
                        child: Row(
                          children: [
                            const Icon(Icons.error, color: Colors.red),
                            const SizedBox(width: 8),
                            Expanded(child: Text(_controller.detailsError!)),
                          ],
                        ),
                      ),
                    ] else if (_controller.selectedUser != null) ...[
                      Expanded(
                        child: SingleChildScrollView(
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              _buildDetailRow('Name', _controller.selectedUser!.name),
                              _buildDetailRow('Email', _controller.selectedUser!.email),
                              _buildDetailRow('Score', '${_controller.selectedUser!.score}'),
                              _buildDetailRow('Level', '${_controller.selectedUser!.level}'),
                              _buildDetailRow(
                                'Join Date',
                                '${_controller.selectedUser!.joinDate.day}/'
                                '${_controller.selectedUser!.joinDate.month}/'
                                '${_controller.selectedUser!.joinDate.year}',
                              ),
                              
                              const SizedBox(height: 20),
                              
                              // Score Update Section
                              const Text(
                                'Update Score',
                                style: TextStyle(
                                  fontSize: 16,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                              const SizedBox(height: 10),
                              
                              Row(
                                children: [
                                  Expanded(
                                    child: TextField(
                                      controller: _scoreController,
                                      decoration: const InputDecoration(
                                        labelText: 'New Score',
                                        border: OutlineInputBorder(),
                                      ),
                                      keyboardType: TextInputType.number,
                                    ),
                                  ),
                                  const SizedBox(width: 10),
                                  ElevatedButton(
                                    onPressed: _controller.isUpdating
                                        ? null
                                        : () {
                                            final score = int.tryParse(_scoreController.text);
                                            if (score != null) {
                                              _controller.updateUserScore(score);
                                              _scoreController.clear();
                                            }
                                          },
                                    child: _controller.isUpdating
                                        ? const SizedBox(
                                            width: 16,
                                            height: 16,
                                            child: CircularProgressIndicator(strokeWidth: 2),
                                          )
                                        : const Text('Update'),
                                  ),
                                ],
                              ),
                              
                              if (_controller.updateError != null) ...[
                                const SizedBox(height: 10),
                                Container(
                                  padding: const EdgeInsets.all(8),
                                  decoration: BoxDecoration(
                                    color: Colors.red.withOpacity(0.1),
                                    border: Border.all(color: Colors.red),
                                    borderRadius: BorderRadius.circular(4),
                                  ),
                                  child: Row(
                                    children: [
                                      const Icon(Icons.error, color: Colors.red, size: 16),
                                      const SizedBox(width: 8),
                                      Expanded(
                                        child: Text(
                                          _controller.updateError!,
                                          style: const TextStyle(fontSize: 12),
                                        ),
                                      ),
                                    ],
                                  ),
                                ),
                              ],
                              
                              if (_controller.updateState == AsyncState.success) ...[
                                const SizedBox(height: 10),
                                Container(
                                  padding: const EdgeInsets.all(8),
                                  decoration: BoxDecoration(
                                    color: Colors.green.withOpacity(0.1),
                                    border: Border.all(color: Colors.green),
                                    borderRadius: BorderRadius.circular(4),
                                  ),
                                  child: const Row(
                                    children: [
                                      Icon(Icons.check, color: Colors.green, size: 16),
                                      SizedBox(width: 8),
                                      Text(
                                        'Score updated successfully!',
                                        style: TextStyle(fontSize: 12),
                                      ),
                                    ],
                                  ),
                                ),
                              ],
                            ],
                          ),
                        ),
                      ),
                    ] else ...[
                      const Expanded(
                        child: Center(
                          child: Text(
                            'Select a user to view details',
                            style: TextStyle(fontSize: 16),
                          ),
                        ),
                      ),
                    ],
                  ],
                ),
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildDetailRow(String label, String value) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        children: [
          SizedBox(
            width: 80,
            child: Text(
              '$label:',
              style: const TextStyle(fontWeight: FontWeight.bold),
            ),
          ),
          Expanded(child: Text(value)),
        ],
      ),
    );
  }
}
```

Async state management handles the complexity of asynchronous operations  
with proper loading states, error handling, and user feedback. This example  
demonstrates managing multiple concurrent async operations, each with their  
own state tracking. The pattern includes retry mechanisms, success feedback,  
and graceful error recovery essential for robust Flutter applications.  

## State with Validation

Implementing comprehensive validation logic within state management.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'State with Validation',
      home: const ValidationStatePage(),
    );
  }
}

class ValidationRule<T> {
  final String message;
  final bool Function(T value) validator;

  ValidationRule(this.message, this.validator);
}

class ValidatedField<T> {
  T _value;
  final List<ValidationRule<T>> _rules;
  final List<String> _errors = [];

  ValidatedField(this._value, this._rules);

  T get value => _value;
  List<String> get errors => List.unmodifiable(_errors);
  bool get isValid => _errors.isEmpty;
  bool get hasErrors => _errors.isNotEmpty;
  String? get firstError => _errors.isNotEmpty ? _errors.first : null;

  void setValue(T newValue) {
    _value = newValue;
    _validate();
  }

  void _validate() {
    _errors.clear();
    for (final rule in _rules) {
      if (!rule.validator(_value)) {
        _errors.add(rule.message);
      }
    }
  }

  void forceValidation() {
    _validate();
  }
}

class UserProfileValidator extends ChangeNotifier {
  late ValidatedField<String> _firstName;
  late ValidatedField<String> _lastName;
  late ValidatedField<String> _email;
  late ValidatedField<String> _phone;
  late ValidatedField<int> _age;
  late ValidatedField<String> _password;
  late ValidatedField<String> _confirmPassword;

  bool _isSubmitting = false;
  bool _hasBeenSubmitted = false;

  UserProfileValidator() {
    _initializeFields();
  }

  void _initializeFields() {
    _firstName = ValidatedField<String>('', [
      ValidationRule('First name is required', (value) => value.isNotEmpty),
      ValidationRule('First name must be at least 2 characters', (value) => value.length >= 2),
      ValidationRule('First name can only contain letters', (value) => RegExp(r'^[a-zA-Z]+$').hasMatch(value)),
    ]);

    _lastName = ValidatedField<String>('', [
      ValidationRule('Last name is required', (value) => value.isNotEmpty),
      ValidationRule('Last name must be at least 2 characters', (value) => value.length >= 2),
      ValidationRule('Last name can only contain letters', (value) => RegExp(r'^[a-zA-Z]+$').hasMatch(value)),
    ]);

    _email = ValidatedField<String>('', [
      ValidationRule('Email is required', (value) => value.isNotEmpty),
      ValidationRule('Please enter a valid email address', (value) => 
          RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value)),
      ValidationRule('Email cannot be from temporary domains', (value) => 
          !value.toLowerCase().contains('tempmail')),
    ]);

    _phone = ValidatedField<String>('', [
      ValidationRule('Phone number is required', (value) => value.isNotEmpty),
      ValidationRule('Phone number must be 10 digits', (value) => 
          RegExp(r'^\d{10}$').hasMatch(value.replaceAll(RegExp(r'[^\d]'), ''))),
    ]);

    _age = ValidatedField<int>(0, [
      ValidationRule('Age is required', (value) => value > 0),
      ValidationRule('You must be at least 13 years old', (value) => value >= 13),
      ValidationRule('Age cannot exceed 120', (value) => value <= 120),
    ]);

    _password = ValidatedField<String>('', [
      ValidationRule('Password is required', (value) => value.isNotEmpty),
      ValidationRule('Password must be at least 8 characters', (value) => value.length >= 8),
      ValidationRule('Password must contain at least one uppercase letter', 
          (value) => RegExp(r'[A-Z]').hasMatch(value)),
      ValidationRule('Password must contain at least one lowercase letter', 
          (value) => RegExp(r'[a-z]').hasMatch(value)),
      ValidationRule('Password must contain at least one digit', 
          (value) => RegExp(r'\d').hasMatch(value)),
      ValidationRule('Password must contain at least one special character', 
          (value) => RegExp(r'[!@#$%^&*(),.?":{}|<>]').hasMatch(value)),
    ]);

    _confirmPassword = ValidatedField<String>('', [
      ValidationRule('Please confirm your password', (value) => value.isNotEmpty),
      ValidationRule('Passwords do not match', (value) => value == _password.value),
    ]);
  }

  // Getters
  ValidatedField<String> get firstName => _firstName;
  ValidatedField<String> get lastName => _lastName;
  ValidatedField<String> get email => _email;
  ValidatedField<String> get phone => _phone;
  ValidatedField<int> get age => _age;
  ValidatedField<String> get password => _password;
  ValidatedField<String> get confirmPassword => _confirmPassword;
  
  bool get isSubmitting => _isSubmitting;
  bool get hasBeenSubmitted => _hasBeenSubmitted;
  
  bool get isValid => 
      _firstName.isValid &&
      _lastName.isValid &&
      _email.isValid &&
      _phone.isValid &&
      _age.isValid &&
      _password.isValid &&
      _confirmPassword.isValid;

  int get totalErrors => 
      _firstName.errors.length +
      _lastName.errors.length +
      _email.errors.length +
      _phone.errors.length +
      _age.errors.length +
      _password.errors.length +
      _confirmPassword.errors.length;

  List<String> get allErrors {
    final errors = <String>[];
    errors.addAll(_firstName.errors);
    errors.addAll(_lastName.errors);
    errors.addAll(_email.errors);
    errors.addAll(_phone.errors);
    errors.addAll(_age.errors);
    errors.addAll(_password.errors);
    errors.addAll(_confirmPassword.errors);
    return errors;
  }

  // Field update methods
  void updateFirstName(String value) {
    _firstName.setValue(value);
    notifyListeners();
  }

  void updateLastName(String value) {
    _lastName.setValue(value);
    notifyListeners();
  }

  void updateEmail(String value) {
    _email.setValue(value);
    notifyListeners();
  }

  void updatePhone(String value) {
    _phone.setValue(value);
    notifyListeners();
  }

  void updateAge(int value) {
    _age.setValue(value);
    notifyListeners();
  }

  void updatePassword(String value) {
    _password.setValue(value);
    // Re-validate confirm password when password changes
    if (_confirmPassword.value.isNotEmpty) {
      _confirmPassword.forceValidation();
    }
    notifyListeners();
  }

  void updateConfirmPassword(String value) {
    _confirmPassword.setValue(value);
    notifyListeners();
  }

  void validateAllFields() {
    _firstName.forceValidation();
    _lastName.forceValidation();
    _email.forceValidation();
    _phone.forceValidation();
    _age.forceValidation();
    _password.forceValidation();
    _confirmPassword.forceValidation();
    notifyListeners();
  }

  Future<bool> submitForm() async {
    _hasBeenSubmitted = true;
    validateAllFields();
    
    if (!isValid) {
      notifyListeners();
      return false;
    }

    _isSubmitting = true;
    notifyListeners();

    // Simulate network request
    await Future.delayed(const Duration(seconds: 2));

    _isSubmitting = false;
    notifyListeners();
    
    return true;
  }

  void resetForm() {
    _initializeFields();
    _isSubmitting = false;
    _hasBeenSubmitted = false;
    notifyListeners();
  }

  Map<String, dynamic> toJson() {
    return {
      'firstName': _firstName.value,
      'lastName': _lastName.value,
      'email': _email.value,
      'phone': _phone.value,
      'age': _age.value,
    };
  }
}

class ValidationStatePage extends StatefulWidget {
  const ValidationStatePage({super.key});

  @override
  State<ValidationStatePage> createState() => _ValidationStatePageState();
}

class _ValidationStatePageState extends State<ValidationStatePage> {
  late UserProfileValidator _validator;
  final _formKey = GlobalKey<FormState>();

  @override
  void initState() {
    super.initState();
    _validator = UserProfileValidator();
    _validator.addListener(_onValidationChanged);
  }

  @override
  void dispose() {
    _validator.removeListener(_onValidationChanged);
    _validator.dispose();
    super.dispose();
  }

  void _onValidationChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('State with Validation'),
        actions: [
          IconButton(
            icon: const Icon(Icons.info_outline),
            onPressed: _showValidationSummary,
          ),
        ],
      ),
      body: Form(
        key: _formKey,
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(16),
          child: Column(
            children: [
              // Validation Summary Card
              if (_validator.hasBeenSubmitted && _validator.totalErrors > 0)
                Card(
                  color: Colors.red.withOpacity(0.1),
                  child: Padding(
                    padding: const EdgeInsets.all(16),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Row(
                          children: [
                            const Icon(Icons.error, color: Colors.red),
                            const SizedBox(width: 8),
                            Text(
                              'Form has ${_validator.totalErrors} errors',
                              style: const TextStyle(
                                color: Colors.red,
                                fontWeight: FontWeight.bold,
                              ),
                            ),
                          ],
                        ),
                        const SizedBox(height: 8),
                        ...(_validator.allErrors.take(3).map((error) => Padding(
                          padding: const EdgeInsets.only(left: 32, top: 2),
                          child: Text('• $error', style: const TextStyle(color: Colors.red)),
                        ))),
                        if (_validator.allErrors.length > 3)
                          Padding(
                            padding: const EdgeInsets.only(left: 32, top: 2),
                            child: Text(
                              '... and ${_validator.allErrors.length - 3} more',
                              style: const TextStyle(color: Colors.red),
                            ),
                          ),
                      ],
                    ),
                  ),
                ),

              const SizedBox(height: 16),

              // Form Fields
              Row(
                children: [
                  Expanded(
                    child: _buildValidatedTextField(
                      label: 'First Name',
                      field: _validator.firstName,
                      onChanged: _validator.updateFirstName,
                    ),
                  ),
                  const SizedBox(width: 16),
                  Expanded(
                    child: _buildValidatedTextField(
                      label: 'Last Name',
                      field: _validator.lastName,
                      onChanged: _validator.updateLastName,
                    ),
                  ),
                ],
              ),

              const SizedBox(height: 16),

              _buildValidatedTextField(
                label: 'Email',
                field: _validator.email,
                onChanged: _validator.updateEmail,
                keyboardType: TextInputType.emailAddress,
              ),

              const SizedBox(height: 16),

              _buildValidatedTextField(
                label: 'Phone Number',
                field: _validator.phone,
                onChanged: _validator.updatePhone,
                keyboardType: TextInputType.phone,
              ),

              const SizedBox(height: 16),

              _buildValidatedTextField(
                label: 'Age',
                field: _validator.age,
                onChanged: (value) => _validator.updateAge(int.tryParse(value) ?? 0),
                keyboardType: TextInputType.number,
              ),

              const SizedBox(height: 16),

              _buildValidatedTextField(
                label: 'Password',
                field: _validator.password,
                onChanged: _validator.updatePassword,
                obscureText: true,
              ),

              const SizedBox(height: 16),

              _buildValidatedTextField(
                label: 'Confirm Password',
                field: _validator.confirmPassword,
                onChanged: _validator.updateConfirmPassword,
                obscureText: true,
              ),

              const SizedBox(height: 32),

              // Submit Button
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: _validator.isSubmitting ? null : _submitForm,
                  style: ElevatedButton.styleFrom(
                    padding: const EdgeInsets.symmetric(vertical: 16),
                    backgroundColor: _validator.isValid && _validator.hasBeenSubmitted
                        ? Colors.greenAccent
                        : null,
                  ),
                  child: _validator.isSubmitting
                      ? const CircularProgressIndicator()
                      : Text(
                          _validator.hasBeenSubmitted 
                              ? (_validator.isValid ? 'Submit Valid Form' : 'Fix Errors and Submit')
                              : 'Submit Form',
                        ),
                ),
              ),

              const SizedBox(height: 16),

              // Reset Button
              SizedBox(
                width: double.infinity,
                child: OutlinedButton(
                  onPressed: _validator.resetForm,
                  child: const Text('Reset Form'),
                ),
              ),

              const SizedBox(height: 16),

              // Validation Stats
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Validation Status',
                        style: Theme.of(context).textTheme.titleMedium,
                      ),
                      const SizedBox(height: 8),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text('Form Valid: ${_validator.isValid ? "Yes" : "No"}'),
                          Text('Total Errors: ${_validator.totalErrors}'),
                        ],
                      ),
                      const SizedBox(height: 4),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text('Submitted: ${_validator.hasBeenSubmitted ? "Yes" : "No"}'),
                          Text('Submitting: ${_validator.isSubmitting ? "Yes" : "No"}'),
                        ],
                      ),
                    ],
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildValidatedTextField<T>({
    required String label,
    required ValidatedField<T> field,
    required Function(String) onChanged,
    TextInputType? keyboardType,
    bool obscureText = false,
  }) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        TextField(
          decoration: InputDecoration(
            labelText: label,
            border: OutlineInputBorder(
              borderSide: BorderSide(
                color: field.hasErrors ? Colors.red : Colors.grey,
              ),
            ),
            errorBorder: const OutlineInputBorder(
              borderSide: BorderSide(color: Colors.red),
            ),
            suffixIcon: field.hasErrors 
                ? const Icon(Icons.error, color: Colors.red)
                : field.isValid && field.value.toString().isNotEmpty
                    ? const Icon(Icons.check, color: Colors.green)
                    : null,
          ),
          keyboardType: keyboardType,
          obscureText: obscureText,
          onChanged: onChanged,
        ),
        if (field.hasErrors) ...[
          const SizedBox(height: 4),
          ...field.errors.map((error) => Padding(
            padding: const EdgeInsets.only(top: 2),
            child: Text(
              error,
              style: const TextStyle(
                color: Colors.red,
                fontSize: 12,
              ),
            ),
          )),
        ],
      ],
    );
  }

  Future<void> _submitForm() async {
    final success = await _validator.submitForm();
    
    if (success && mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: const Text('Form submitted successfully!'),
          backgroundColor: Colors.green,
          action: SnackBarAction(
            label: 'View Data',
            textColor: Colors.white,
            onPressed: _showSubmittedData,
          ),
        ),
      );
    }
  }

  void _showValidationSummary() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Validation Summary'),
        content: SizedBox(
          width: double.maxFinite,
          child: Column(
            mainAxisSize: MainAxisSize.min,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text('Form Valid: ${_validator.isValid}'),
              Text('Total Errors: ${_validator.totalErrors}'),
              Text('Has Been Submitted: ${_validator.hasBeenSubmitted}'),
              const SizedBox(height: 16),
              if (_validator.allErrors.isNotEmpty) ...[
                const Text(
                  'All Errors:',
                  style: TextStyle(fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                SizedBox(
                  height: 200,
                  child: ListView.builder(
                    itemCount: _validator.allErrors.length,
                    itemBuilder: (context, index) {
                      return Text('• ${_validator.allErrors[index]}');
                    },
                  ),
                ),
              ] else ...[
                const Text('No validation errors!'),
              ],
            ],
          ),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }

  void _showSubmittedData() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Submitted Data'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: _validator.toJson().entries.map((entry) {
            return Padding(
              padding: const EdgeInsets.symmetric(vertical: 2),
              child: Text('${entry.key}: ${entry.value}'),
            );
          }).toList(),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }
}
```

State with validation demonstrates comprehensive input validation within  
state management architecture. This approach encapsulates validation rules,  
error tracking, and form state in a structured way. The ValidatedField  
class provides reusable validation logic with multiple rules per field,  
while the controller coordinates overall form validation and submission  
states. This pattern ensures data integrity and provides rich user feedback.

## State Undo/Redo

Implementing undo and redo functionality in state management.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'State Undo/Redo',
      home: const UndoRedoPage(),
    );
  }
}

class DrawingPoint {
  final Offset point;
  final Color color;
  final double size;

  const DrawingPoint({
    required this.point,
    required this.color,
    this.size = 3.0,
  });

  DrawingPoint copyWith({
    Offset? point,
    Color? color,
    double? size,
  }) {
    return DrawingPoint(
      point: point ?? this.point,
      color: color ?? this.color,
      size: size ?? this.size,
    );
  }
}

class DrawingState {
  final List<List<DrawingPoint>> strokes;
  final Color currentColor;
  final double currentSize;

  const DrawingState({
    required this.strokes,
    required this.currentColor,
    this.currentSize = 3.0,
  });

  DrawingState copyWith({
    List<List<DrawingPoint>>? strokes,
    Color? currentColor,
    double? currentSize,
  }) {
    return DrawingState(
      strokes: strokes ?? this.strokes,
      currentColor: currentColor ?? this.currentColor,
      currentSize: currentSize ?? this.currentSize,
    );
  }

  DrawingState addStroke(List<DrawingPoint> stroke) {
    return copyWith(strokes: [...strokes, stroke]);
  }

  DrawingState updateCurrentStroke(List<DrawingPoint> stroke) {
    if (strokes.isEmpty) return addStroke(stroke);
    
    final newStrokes = [...strokes];
    newStrokes[newStrokes.length - 1] = stroke;
    return copyWith(strokes: newStrokes);
  }

  Map<String, dynamic> toJson() {
    return {
      'strokesCount': strokes.length,
      'currentColor': currentColor.value,
      'currentSize': currentSize,
    };
  }
}

class UndoRedoController extends ChangeNotifier {
  static const int maxHistorySize = 50;
  
  final List<DrawingState> _history = [];
  int _currentIndex = -1;
  
  DrawingState _currentState = const DrawingState(
    strokes: [],
    currentColor: Colors.blueAccent,
    currentSize: 3.0,
  );

  List<DrawingPoint> _currentStroke = [];
  bool _isDrawing = false;

  // Getters
  DrawingState get currentState => _currentState;
  List<DrawingPoint> get currentStroke => _currentStroke;
  bool get isDrawing => _isDrawing;
  bool get canUndo => _currentIndex > 0;
  bool get canRedo => _currentIndex < _history.length - 1;
  int get historyLength => _history.length;
  int get currentHistoryIndex => _currentIndex;

  void _saveState(DrawingState state) {
    // Remove any future history if we're not at the end
    if (_currentIndex < _history.length - 1) {
      _history.removeRange(_currentIndex + 1, _history.length);
    }

    // Add new state
    _history.add(state);
    _currentIndex = _history.length - 1;

    // Limit history size
    if (_history.length > maxHistorySize) {
      _history.removeAt(0);
      _currentIndex--;
    }

    _currentState = state;
  }

  void startDrawing(Offset point) {
    _isDrawing = true;
    _currentStroke = [
      DrawingPoint(
        point: point,
        color: _currentState.currentColor,
        size: _currentState.currentSize,
      )
    ];
    notifyListeners();
  }

  void continueDrawing(Offset point) {
    if (!_isDrawing) return;
    
    _currentStroke.add(
      DrawingPoint(
        point: point,
        color: _currentState.currentColor,
        size: _currentState.currentSize,
      ),
    );
    notifyListeners();
  }

  void endDrawing() {
    if (!_isDrawing || _currentStroke.isEmpty) return;
    
    final newState = _currentState.addStroke(List.from(_currentStroke));
    _saveState(newState);
    
    _currentStroke.clear();
    _isDrawing = false;
    notifyListeners();
  }

  void setColor(Color color) {
    final newState = _currentState.copyWith(currentColor: color);
    _currentState = newState;
    notifyListeners();
  }

  void setBrushSize(double size) {
    final newState = _currentState.copyWith(currentSize: size);
    _currentState = newState;
    notifyListeners();
  }

  void undo() {
    if (!canUndo) return;
    
    _currentIndex--;
    _currentState = _history[_currentIndex];
    notifyListeners();
  }

  void redo() {
    if (!canRedo) return;
    
    _currentIndex++;
    _currentState = _history[_currentIndex];
    notifyListeners();
  }

  void clearCanvas() {
    const newState = DrawingState(
      strokes: [],
      currentColor: Colors.blueAccent,
      currentSize: 3.0,
    );
    _saveState(newState);
    notifyListeners();
  }

  void resetHistory() {
    _history.clear();
    _currentIndex = -1;
    _currentState = const DrawingState(
      strokes: [],
      currentColor: Colors.blueAccent,
      currentSize: 3.0,
    );
    _currentStroke.clear();
    _isDrawing = false;
    notifyListeners();
  }

  List<String> getHistoryInfo() {
    return _history.asMap().entries.map((entry) {
      final index = entry.key;
      final state = entry.value;
      final isCurrent = index == _currentIndex;
      return '${isCurrent ? "→ " : "  "}Step ${index + 1}: '
             '${state.strokes.length} strokes';
    }).toList();
  }
}

class UndoRedoPage extends StatefulWidget {
  const UndoRedoPage({super.key});

  @override
  State<UndoRedoPage> createState() => _UndoRedoPageState();
}

class _UndoRedoPageState extends State<UndoRedoPage> {
  late UndoRedoController _controller;

  @override
  void initState() {
    super.initState();
    _controller = UndoRedoController();
    _controller.addListener(_onStateChanged);
  }

  @override
  void dispose() {
    _controller.removeListener(_onStateChanged);
    _controller.dispose();
    super.dispose();
  }

  void _onStateChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Undo/Redo Drawing'),
        actions: [
          IconButton(
            icon: Icon(
              Icons.undo,
              color: _controller.canUndo ? null : Colors.grey,
            ),
            onPressed: _controller.canUndo ? _controller.undo : null,
          ),
          IconButton(
            icon: Icon(
              Icons.redo,
              color: _controller.canRedo ? null : Colors.grey,
            ),
            onPressed: _controller.canRedo ? _controller.redo : null,
          ),
          IconButton(
            icon: const Icon(Icons.clear),
            onPressed: _controller.clearCanvas,
          ),
          IconButton(
            icon: const Icon(Icons.history),
            onPressed: _showHistoryDialog,
          ),
        ],
      ),
      body: Column(
        children: [
          // Drawing Canvas
          Expanded(
            child: Container(
              width: double.infinity,
              color: Colors.grey[900],
              child: GestureDetector(
                onPanStart: (details) {
                  _controller.startDrawing(details.localPosition);
                },
                onPanUpdate: (details) {
                  _controller.continueDrawing(details.localPosition);
                },
                onPanEnd: (details) {
                  _controller.endDrawing();
                },
                child: CustomPaint(
                  painter: DrawingPainter(
                    strokes: _controller.currentState.strokes,
                    currentStroke: _controller.currentStroke,
                  ),
                  child: Container(),
                ),
              ),
            ),
          ),

          // Controls Panel
          Container(
            padding: const EdgeInsets.all(16),
            child: Column(
              children: [
                // Status Row
                Row(
                  children: [
                    Text('Strokes: ${_controller.currentState.strokes.length}'),
                    const Spacer(),
                    Text('History: ${_controller.currentHistoryIndex + 1}/'
                         '${_controller.historyLength}'),
                  ],
                ),

                const SizedBox(height: 16),

                // Color Picker
                Row(
                  children: [
                    const Text('Color: '),
                    const SizedBox(width: 8),
                    ...([
                      Colors.blueAccent,
                      Colors.redAccent,
                      Colors.greenAccent,
                      Colors.orangeAccent,
                      Colors.purpleAccent,
                      Colors.yellowAccent,
                      Colors.white,
                      Colors.black,
                    ].map((color) => Padding(
                      padding: const EdgeInsets.only(right: 8),
                      child: GestureDetector(
                        onTap: () => _controller.setColor(color),
                        child: Container(
                          width: 30,
                          height: 30,
                          decoration: BoxDecoration(
                            color: color,
                            shape: BoxShape.circle,
                            border: _controller.currentState.currentColor == color
                                ? Border.all(color: Colors.white, width: 3)
                                : Border.all(color: Colors.grey, width: 1),
                          ),
                        ),
                      ),
                    ))),
                  ],
                ),

                const SizedBox(height: 16),

                // Brush Size Slider
                Row(
                  children: [
                    const Text('Size: '),
                    Expanded(
                      child: Slider(
                        value: _controller.currentState.currentSize,
                        min: 1.0,
                        max: 10.0,
                        divisions: 9,
                        label: _controller.currentState.currentSize.toInt().toString(),
                        onChanged: _controller.setBrushSize,
                      ),
                    ),
                    Container(
                      width: 30,
                      height: 30,
                      decoration: BoxDecoration(
                        color: _controller.currentState.currentColor,
                        shape: BoxShape.circle,
                      ),
                      child: Center(
                        child: Container(
                          width: _controller.currentState.currentSize * 2,
                          height: _controller.currentState.currentSize * 2,
                          decoration: BoxDecoration(
                            color: _controller.currentState.currentColor,
                            shape: BoxShape.circle,
                          ),
                        ),
                      ),
                    ),
                  ],
                ),

                const SizedBox(height: 16),

                // Action Buttons
                Row(
                  children: [
                    Expanded(
                      child: ElevatedButton.icon(
                        onPressed: _controller.canUndo ? _controller.undo : null,
                        icon: const Icon(Icons.undo),
                        label: const Text('Undo'),
                      ),
                    ),
                    const SizedBox(width: 8),
                    Expanded(
                      child: ElevatedButton.icon(
                        onPressed: _controller.canRedo ? _controller.redo : null,
                        icon: const Icon(Icons.redo),
                        label: const Text('Redo'),
                      ),
                    ),
                    const SizedBox(width: 8),
                    Expanded(
                      child: ElevatedButton.icon(
                        onPressed: _controller.clearCanvas,
                        icon: const Icon(Icons.clear),
                        label: const Text('Clear'),
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.redAccent,
                        ),
                      ),
                    ),
                  ],
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }

  void _showHistoryDialog() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Drawing History'),
        content: SizedBox(
          width: double.maxFinite,
          height: 300,
          child: _controller.historyLength == 0
              ? const Center(child: Text('No history available'))
              : ListView.builder(
                  itemCount: _controller.historyLength,
                  itemBuilder: (context, index) {
                    final isCurrent = index == _controller.currentHistoryIndex;
                    final state = _controller.getHistoryInfo()[index];
                    
                    return ListTile(
                      leading: Icon(
                        isCurrent ? Icons.arrow_right : Icons.circle,
                        color: isCurrent ? Colors.blueAccent : Colors.grey,
                      ),
                      title: Text(
                        state,
                        style: TextStyle(
                          fontWeight: isCurrent ? FontWeight.bold : null,
                          color: isCurrent ? Colors.blueAccent : null,
                        ),
                      ),
                      onTap: () {
                        // Jump to specific history point
                        while (_controller.currentHistoryIndex != index) {
                          if (_controller.currentHistoryIndex < index) {
                            if (_controller.canRedo) {
                              _controller.redo();
                            } else {
                              break;
                            }
                          } else {
                            if (_controller.canUndo) {
                              _controller.undo();
                            } else {
                              break;
                            }
                          }
                        }
                        Navigator.pop(context);
                      },
                    );
                  },
                ),
        ),
        actions: [
          TextButton(
            onPressed: () {
              _controller.resetHistory();
              Navigator.pop(context);
            },
            child: const Text('Clear History'),
          ),
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }
}

class DrawingPainter extends CustomPainter {
  final List<List<DrawingPoint>> strokes;
  final List<DrawingPoint> currentStroke;

  DrawingPainter({
    required this.strokes,
    required this.currentStroke,
  });

  @override
  void paint(Canvas canvas, Size size) {
    // Draw completed strokes
    for (final stroke in strokes) {
      _drawStroke(canvas, stroke);
    }

    // Draw current stroke
    if (currentStroke.isNotEmpty) {
      _drawStroke(canvas, currentStroke);
    }
  }

  void _drawStroke(Canvas canvas, List<DrawingPoint> stroke) {
    if (stroke.isEmpty) return;

    final paint = Paint()
      ..color = stroke.first.color
      ..strokeWidth = stroke.first.size
      ..strokeCap = StrokeCap.round
      ..style = PaintingStyle.stroke;

    if (stroke.length == 1) {
      // Single point - draw a dot
      canvas.drawCircle(stroke.first.point, stroke.first.size / 2, paint..style = PaintingStyle.fill);
    } else {
      // Multiple points - draw lines
      final path = Path();
      path.moveTo(stroke.first.point.dx, stroke.first.point.dy);
      
      for (int i = 1; i < stroke.length; i++) {
        path.lineTo(stroke[i].point.dx, stroke[i].point.dy);
      }
      
      canvas.drawPath(path, paint);
    }
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}
```

Undo/Redo functionality provides users with the ability to reverse and  
restore actions, essential for creative applications. This implementation  
uses the Command pattern with state snapshots to maintain a history stack.  
The controller manages a bounded history buffer, tracks the current position,  
and provides navigation through previous states. This pattern enhances user  
confidence by allowing experimentation without fear of losing work.  

## State with Caching

Implementing caching strategies within state management for performance.  

```dart
import 'dart:async';
import 'dart:math';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'State with Caching',
      home: const CachingStatePage(),
    );
  }
}

class CacheEntry<T> {
  final T data;
  final DateTime createdAt;
  final Duration? ttl;
  int accessCount;
  DateTime lastAccessed;

  CacheEntry({
    required this.data,
    required this.createdAt,
    this.ttl,
    this.accessCount = 1,
    required this.lastAccessed,
  });

  bool get isExpired {
    if (ttl == null) return false;
    return DateTime.now().difference(createdAt) > ttl!;
  }

  void markAccessed() {
    accessCount++;
    lastAccessed = DateTime.now();
  }
}

class CacheManager<T> {
  final Map<String, CacheEntry<T>> _cache = {};
  final int maxSize;
  final Duration defaultTtl;

  CacheManager({
    this.maxSize = 100,
    this.defaultTtl = const Duration(minutes: 5),
  });

  T? get(String key) {
    final entry = _cache[key];
    if (entry == null) return null;
    
    if (entry.isExpired) {
      _cache.remove(key);
      return null;
    }
    
    entry.markAccessed();
    return entry.data;
  }

  void put(String key, T data, {Duration? ttl}) {
    // Remove expired entries
    _cleanupExpired();
    
    // Remove least recently used if at capacity
    if (_cache.length >= maxSize && !_cache.containsKey(key)) {
      _evictLRU();
    }
    
    _cache[key] = CacheEntry(
      data: data,
      createdAt: DateTime.now(),
      ttl: ttl ?? defaultTtl,
      lastAccessed: DateTime.now(),
    );
  }

  void remove(String key) {
    _cache.remove(key);
  }

  void clear() {
    _cache.clear();
  }

  bool containsKey(String key) {
    final entry = _cache[key];
    if (entry == null) return false;
    
    if (entry.isExpired) {
      _cache.remove(key);
      return false;
    }
    
    return true;
  }

  int get size => _cache.length;
  
  List<String> get keys => _cache.keys.toList();

  Map<String, Map<String, dynamic>> getCacheStats() {
    return _cache.map((key, entry) => MapEntry(key, {
      'accessCount': entry.accessCount,
      'lastAccessed': entry.lastAccessed.toIso8601String(),
      'createdAt': entry.createdAt.toIso8601String(),
      'isExpired': entry.isExpired,
      'ttlMinutes': entry.ttl?.inMinutes,
    }));
  }

  void _cleanupExpired() {
    final expiredKeys = _cache.entries
        .where((entry) => entry.value.isExpired)
        .map((entry) => entry.key)
        .toList();
    
    for (final key in expiredKeys) {
      _cache.remove(key);
    }
  }

  void _evictLRU() {
    if (_cache.isEmpty) return;
    
    String? lruKey;
    DateTime? oldestAccess;
    
    for (final entry in _cache.entries) {
      if (oldestAccess == null || entry.value.lastAccessed.isBefore(oldestAccess)) {
        oldestAccess = entry.value.lastAccessed;
        lruKey = entry.key;
      }
    }
    
    if (lruKey != null) {
      _cache.remove(lruKey);
    }
  }
}

class UserProfile {
  final String id;
  final String name;
  final String email;
  final String avatar;
  final int score;
  final List<String> badges;

  UserProfile({
    required this.id,
    required this.name,
    required this.email,
    required this.avatar,
    required this.score,
    required this.badges,
  });

  factory UserProfile.fromJson(Map<String, dynamic> json) {
    return UserProfile(
      id: json['id'],
      name: json['name'],
      email: json['email'],
      avatar: json['avatar'],
      score: json['score'],
      badges: List<String>.from(json['badges'] ?? []),
    );
  }
}

class ApiService {
  static final Random _random = Random();
  
  static Future<UserProfile> fetchUserProfile(String userId) async {
    // Simulate network delay
    await Future.delayed(Duration(seconds: 1 + _random.nextInt(3)));
    
    if (_random.nextDouble() < 0.1) {
      throw Exception('Network error: Failed to fetch user $userId');
    }
    
    return UserProfile(
      id: userId,
      name: 'User ${userId.substring(0, 4)}',
      email: 'user${userId.substring(0, 4)}@example.com',
      avatar: 'https://picsum.photos/100/100?random=$userId',
      score: _random.nextInt(10000),
      badges: _generateRandomBadges(),
    );
  }
  
  static List<String> _generateRandomBadges() {
    final allBadges = [
      'Newcomer', 'Explorer', 'Achiever', 'Expert', 'Master',
      'Veteran', 'Legend', 'Champion', 'Hero', 'Pioneer'
    ];
    
    final count = _random.nextInt(4) + 1;
    final badges = <String>[];
    
    for (int i = 0; i < count; i++) {
      final badge = allBadges[_random.nextInt(allBadges.length)];
      if (!badges.contains(badge)) {
        badges.add(badge);
      }
    }
    
    return badges;
  }
}

enum LoadingState { idle, loading, success, error }

class CachingStateController extends ChangeNotifier {
  final CacheManager<UserProfile> _profileCache = CacheManager<UserProfile>(
    maxSize: 50,
    defaultTtl: const Duration(minutes: 2),
  );
  
  final CacheManager<String> _stringCache = CacheManager<String>(
    maxSize: 100,
    defaultTtl: const Duration(minutes: 1),
  );

  final Map<String, LoadingState> _loadingStates = {};
  final Map<String, String> _errors = {};
  final Set<String> _loadedUsers = {};

  // Getters
  LoadingState getLoadingState(String userId) {
    return _loadingStates[userId] ?? LoadingState.idle;
  }

  String? getError(String userId) {
    return _errors[userId];
  }

  UserProfile? getCachedProfile(String userId) {
    return _profileCache.get(userId);
  }

  bool isLoading(String userId) {
    return getLoadingState(userId) == LoadingState.loading;
  }

  bool hasError(String userId) {
    return _errors.containsKey(userId);
  }

  bool isProfileCached(String userId) {
    return _profileCache.containsKey(userId);
  }

  int get cacheSize => _profileCache.size;
  
  Map<String, Map<String, dynamic>> get cacheStats => _profileCache.getCacheStats();
  
  List<String> get cachedUserIds => _profileCache.keys;

  Future<UserProfile?> loadUserProfile(String userId, {bool forceRefresh = false}) async {
    // Check cache first (unless force refresh)
    if (!forceRefresh) {
      final cached = _profileCache.get(userId);
      if (cached != null) {
        return cached;
      }
    }

    // Set loading state
    _loadingStates[userId] = LoadingState.loading;
    _errors.remove(userId);
    notifyListeners();

    try {
      final profile = await ApiService.fetchUserProfile(userId);
      
      // Cache the result
      _profileCache.put(userId, profile);
      
      // Update state
      _loadingStates[userId] = LoadingState.success;
      _loadedUsers.add(userId);
      
      notifyListeners();
      return profile;
      
    } catch (e) {
      _loadingStates[userId] = LoadingState.error;
      _errors[userId] = e.toString();
      notifyListeners();
      return null;
    }
  }

  void preloadUsers(List<String> userIds) {
    for (final userId in userIds) {
      if (!isProfileCached(userId) && !isLoading(userId)) {
        loadUserProfile(userId);
      }
    }
  }

  void removeCachedProfile(String userId) {
    _profileCache.remove(userId);
    _loadingStates.remove(userId);
    _errors.remove(userId);
    _loadedUsers.remove(userId);
    notifyListeners();
  }

  void clearAllCaches() {
    _profileCache.clear();
    _stringCache.clear();
    _loadingStates.clear();
    _errors.clear();
    _loadedUsers.clear();
    notifyListeners();
  }

  void cacheString(String key, String value) {
    _stringCache.put(key, value);
  }

  String? getCachedString(String key) {
    return _stringCache.get(key);
  }

  // Search functionality with caching
  List<UserProfile> searchProfiles(String query) {
    final cacheKey = 'search_$query';
    
    // This would normally search cached profiles
    // For demo purposes, we'll just filter loaded profiles
    final profiles = <UserProfile>[];
    
    for (final userId in _loadedUsers) {
      final profile = _profileCache.get(userId);
      if (profile != null && 
          profile.name.toLowerCase().contains(query.toLowerCase())) {
        profiles.add(profile);
      }
    }
    
    return profiles;
  }

  // Batch operations
  Future<List<UserProfile>> loadMultipleProfiles(List<String> userIds) async {
    final profiles = <UserProfile>[];
    
    // Start all loads concurrently
    final futures = userIds.map((userId) => loadUserProfile(userId));
    final results = await Future.wait(futures);
    
    for (final profile in results) {
      if (profile != null) {
        profiles.add(profile);
      }
    }
    
    return profiles;
  }
}

class CachingStatePage extends StatefulWidget {
  const CachingStatePage({super.key});

  @override
  State<CachingStatePage> createState() => _CachingStatePageState();
}

class _CachingStatePageState extends State<CachingStatePage> {
  late CachingStateController _controller;
  final TextEditingController _userIdController = TextEditingController();
  final TextEditingController _searchController = TextEditingController();

  @override
  void initState() {
    super.initState();
    _controller = CachingStateController();
    _controller.addListener(_onStateChanged);
    
    // Preload some sample users
    _controller.preloadUsers(['user1', 'user2', 'user3']);
  }

  @override
  void dispose() {
    _controller.removeListener(_onStateChanged);
    _controller.dispose();
    _userIdController.dispose();
    _searchController.dispose();
    super.dispose();
  }

  void _onStateChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('State with Caching'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: () {
              for (final userId in _controller.cachedUserIds) {
                _controller.loadUserProfile(userId, forceRefresh: true);
              }
            },
          ),
          IconButton(
            icon: const Icon(Icons.clear_all),
            onPressed: _controller.clearAllCaches,
          ),
        ],
      ),
      body: Column(
        children: [
          // Controls
          Card(
            margin: const EdgeInsets.all(16),
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                children: [
                  Row(
                    children: [
                      Expanded(
                        child: TextField(
                          controller: _userIdController,
                          decoration: const InputDecoration(
                            labelText: 'User ID',
                            hintText: 'Enter user ID (e.g., user123)',
                            border: OutlineInputBorder(),
                          ),
                        ),
                      ),
                      const SizedBox(width: 8),
                      ElevatedButton(
                        onPressed: () {
                          final userId = _userIdController.text.trim();
                          if (userId.isNotEmpty) {
                            _controller.loadUserProfile(userId);
                          }
                        },
                        child: const Text('Load'),
                      ),
                    ],
                  ),
                  const SizedBox(height: 8),
                  Row(
                    children: [
                      Expanded(
                        child: ElevatedButton(
                          onPressed: () {
                            final userIds = List.generate(5, (i) => 'user${i + 10}');
                            _controller.loadMultipleProfiles(userIds);
                          },
                          child: const Text('Load Batch'),
                        ),
                      ),
                      const SizedBox(width: 8),
                      Expanded(
                        child: ElevatedButton(
                          onPressed: () => _showCacheStats(),
                          child: const Text('Cache Stats'),
                        ),
                      ),
                    ],
                  ),
                ],
              ),
            ),
          ),

          // Cache Status
          Container(
            width: double.infinity,
            padding: const EdgeInsets.symmetric(horizontal: 16),
            child: Text(
              'Cached Profiles: ${_controller.cacheSize}',
              style: Theme.of(context).textTheme.titleMedium,
            ),
          ),

          const SizedBox(height: 8),

          // Search
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 16),
            child: TextField(
              controller: _searchController,
              decoration: InputDecoration(
                labelText: 'Search cached profiles',
                border: const OutlineInputBorder(),
                suffixIcon: IconButton(
                  icon: const Icon(Icons.search),
                  onPressed: () {
                    setState(() {}); // Trigger rebuild to show search results
                  },
                ),
              ),
              onChanged: (_) => setState(() {}),
            ),
          ),

          const SizedBox(height: 16),

          // Profile List
          Expanded(
            child: _buildProfileList(),
          ),
        ],
      ),
    );
  }

  Widget _buildProfileList() {
    final searchQuery = _searchController.text.trim();
    List<UserProfile> profiles;
    
    if (searchQuery.isNotEmpty) {
      profiles = _controller.searchProfiles(searchQuery);
    } else {
      profiles = _controller.cachedUserIds
          .map((id) => _controller.getCachedProfile(id))
          .where((profile) => profile != null)
          .cast<UserProfile>()
          .toList();
    }

    if (profiles.isEmpty && searchQuery.isEmpty) {
      return const Center(
        child: Text(
          'No cached profiles.\nLoad some profiles to see them here.',
          textAlign: TextAlign.center,
        ),
      );
    }

    if (profiles.isEmpty && searchQuery.isNotEmpty) {
      return Center(
        child: Text(
          'No profiles found matching "$searchQuery"',
          textAlign: TextAlign.center,
        ),
      );
    }

    return ListView.builder(
      itemCount: profiles.length,
      itemBuilder: (context, index) {
        final profile = profiles[index];
        final isLoading = _controller.isLoading(profile.id);
        final hasError = _controller.hasError(profile.id);
        
        return Card(
          margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 4),
          child: ListTile(
            leading: Stack(
              children: [
                CircleAvatar(
                  backgroundImage: NetworkImage(profile.avatar),
                  onBackgroundImageError: (_, __) {},
                  child: Text(profile.name.substring(0, 2)),
                ),
                if (isLoading)
                  const Positioned.fill(
                    child: CircularProgressIndicator(strokeWidth: 2),
                  ),
              ],
            ),
            title: Row(
              children: [
                Text(profile.name),
                const SizedBox(width: 8),
                if (_controller.isProfileCached(profile.id))
                  const Icon(Icons.cached, size: 16, color: Colors.greenAccent),
              ],
            ),
            subtitle: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text('Score: ${profile.score}'),
                if (profile.badges.isNotEmpty)
                  Wrap(
                    spacing: 4,
                    children: profile.badges.take(3).map((badge) => 
                      Chip(
                        label: Text(badge, style: const TextStyle(fontSize: 10)),
                        materialTapTargetSize: MaterialTapTargetSize.shrinkWrap,
                        visualDensity: VisualDensity.compact,
                      )
                    ).toList(),
                  ),
                if (hasError)
                  Text(
                    _controller.getError(profile.id) ?? 'Unknown error',
                    style: const TextStyle(color: Colors.red, fontSize: 12),
                  ),
              ],
            ),
            trailing: PopupMenuButton(
              itemBuilder: (context) => [
                PopupMenuItem(
                  child: const Text('Refresh'),
                  onTap: () => _controller.loadUserProfile(profile.id, forceRefresh: true),
                ),
                PopupMenuItem(
                  child: const Text('Remove from cache'),
                  onTap: () => _controller.removeCachedProfile(profile.id),
                ),
              ],
            ),
          ),
        );
      },
    );
  }

  void _showCacheStats() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Cache Statistics'),
        content: SizedBox(
          width: double.maxFinite,
          height: 400,
          child: SingleChildScrollView(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text('Total cached profiles: ${_controller.cacheSize}'),
                const SizedBox(height: 16),
                const Text(
                  'Individual Cache Entries:',
                  style: TextStyle(fontWeight: FontWeight.bold),
                ),
                const SizedBox(height: 8),
                ..._controller.cacheStats.entries.map((entry) {
                  final stats = entry.value;
                  return Card(
                    child: Padding(
                      padding: const EdgeInsets.all(8),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            'User: ${entry.key}',
                            style: const TextStyle(fontWeight: FontWeight.bold),
                          ),
                          Text('Access count: ${stats['accessCount']}'),
                          Text('Expired: ${stats['isExpired']}'),
                          Text('TTL: ${stats['ttlMinutes']} minutes'),
                        ],
                      ),
                    ),
                  );
                }).toList(),
              ],
            ),
          ),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }
}
```

State with caching demonstrates performance optimization through strategic  
data storage. The CacheManager implements LRU eviction, TTL expiration,  
and access tracking for efficient memory management. This pattern reduces  
network requests, improves response times, and provides offline-capable  
functionality. The implementation includes cache statistics, batch loading,  
and search capabilities essential for data-intensive applications.

## State Synchronization

Synchronizing state between multiple widgets and data sources.  

```dart
import 'dart:async';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'State Synchronization',
      home: const StateSyncPage(),
    );
  }
}

class SyncEvent {
  final String type;
  final String source;
  final Map<String, dynamic> data;
  final DateTime timestamp;

  SyncEvent({
    required this.type,
    required this.source,
    required this.data,
    DateTime? timestamp,
  }) : timestamp = timestamp ?? DateTime.now();
}

class StateSync extends ChangeNotifier {
  final Map<String, dynamic> _state = {};
  final List<SyncEvent> _eventLog = [];
  final StreamController<SyncEvent> _eventController = 
      StreamController<SyncEvent>.broadcast();
  
  Timer? _syncTimer;
  bool _isSyncing = false;
  
  Stream<SyncEvent> get eventStream => _eventController.stream;
  Map<String, dynamic> get state => Map.unmodifiable(_state);
  List<SyncEvent> get eventLog => List.unmodifiable(_eventLog);
  bool get isSyncing => _isSyncing;

  void setState(String key, dynamic value, {String source = 'unknown'}) {
    final oldValue = _state[key];
    _state[key] = value;
    
    final event = SyncEvent(
      type: 'state_change',
      source: source,
      data: {
        'key': key,
        'oldValue': oldValue,
        'newValue': value,
      },
    );
    
    _addEvent(event);
    notifyListeners();
    _scheduleBatchSync();
  }

  T? getState<T>(String key) {
    return _state[key] as T?;
  }

  void batchUpdate(Map<String, dynamic> updates, {String source = 'batch'}) {
    final event = SyncEvent(
      type: 'batch_update',
      source: source,
      data: {'updates': updates},
    );

    _state.addAll(updates);
    _addEvent(event);
    notifyListeners();
    _scheduleBatchSync();
  }

  void mergeState(Map<String, dynamic> remoteState, {String source = 'remote'}) {
    _isSyncing = true;
    notifyListeners();

    final conflicts = <String, Map<String, dynamic>>{};
    final merged = <String, dynamic>{};

    for (final entry in remoteState.entries) {
      final key = entry.key;
      final remoteValue = entry.value;
      final localValue = _state[key];

      if (localValue != null && localValue != remoteValue) {
        // Conflict detected
        conflicts[key] = {
          'local': localValue,
          'remote': remoteValue,
          'resolved': remoteValue, // Default to remote value
        };
        merged[key] = remoteValue;
      } else {
        merged[key] = remoteValue;
      }
    }

    _state.addAll(merged);

    final event = SyncEvent(
      type: 'merge_complete',
      source: source,
      data: {
        'conflicts': conflicts,
        'mergedKeys': merged.keys.toList(),
      },
    );

    _addEvent(event);
    _isSyncing = false;
    notifyListeners();
  }

  void _addEvent(SyncEvent event) {
    _eventLog.add(event);
    if (_eventLog.length > 100) {
      _eventLog.removeAt(0);
    }
    _eventController.add(event);
  }

  void _scheduleBatchSync() {
    _syncTimer?.cancel();
    _syncTimer = Timer(const Duration(seconds: 2), _performBatchSync);
  }

  Future<void> _performBatchSync() async {
    if (_isSyncing) return;

    _isSyncing = true;
    notifyListeners();

    // Simulate server sync
    await Future.delayed(const Duration(milliseconds: 500));

    final event = SyncEvent(
      type: 'sync_complete',
      source: 'server',
      data: {'timestamp': DateTime.now().toIso8601String()},
    );

    _addEvent(event);
    _isSyncing = false;
    notifyListeners();
  }

  void clearEventLog() {
    _eventLog.clear();
    notifyListeners();
  }

  @override
  void dispose() {
    _syncTimer?.cancel();
    _eventController.close();
    super.dispose();
  }
}

class StateSyncPage extends StatefulWidget {
  const StateSyncPage({super.key});

  @override
  State<StateSyncPage> createState() => _StateSyncPageState();
}

class _StateSyncPageState extends State<StateSyncPage> {
  late StateSync _stateSync;
  late StreamSubscription _eventSubscription;
  
  @override
  void initState() {
    super.initState();
    _stateSync = StateSync();
    _stateSync.addListener(_onStateChanged);
    _eventSubscription = _stateSync.eventStream.listen(_onSyncEvent);

    // Initialize some state
    _stateSync.batchUpdate({
      'counter': 0,
      'userName': 'User',
      'theme': 'dark',
      'notifications': true,
    }, source: 'initial');
  }

  @override
  void dispose() {
    _stateSync.removeListener(_onStateChanged);
    _eventSubscription.cancel();
    _stateSync.dispose();
    super.dispose();
  }

  void _onStateChanged() {
    setState(() {});
  }

  void _onSyncEvent(SyncEvent event) {
    // Handle real-time sync events
    if (event.type == 'sync_complete') {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('State synchronized with server'),
          duration: Duration(seconds: 1),
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('State Synchronization'),
        actions: [
          if (_stateSync.isSyncing)
            const Padding(
              padding: EdgeInsets.all(16),
              child: SizedBox(
                width: 20,
                height: 20,
                child: CircularProgressIndicator(strokeWidth: 2),
              ),
            ),
          IconButton(
            icon: const Icon(Icons.sync),
            onPressed: _simulateRemoteSync,
          ),
          IconButton(
            icon: const Icon(Icons.history),
            onPressed: _showEventLog,
          ),
        ],
      ),
      body: Column(
        children: [
          // State Display
          Card(
            margin: const EdgeInsets.all(16),
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Current State',
                    style: Theme.of(context).textTheme.titleMedium,
                  ),
                  const SizedBox(height: 8),
                  ..._stateSync.state.entries.map((entry) => Padding(
                    padding: const EdgeInsets.symmetric(vertical: 2),
                    child: Row(
                      children: [
                        SizedBox(
                          width: 120,
                          child: Text(
                            '${entry.key}:',
                            style: const TextStyle(fontWeight: FontWeight.bold),
                          ),
                        ),
                        Expanded(child: Text('${entry.value}')),
                      ],
                    ),
                  )),
                ],
              ),
            ),
          ),

          // Control Widgets
          Expanded(
            child: SingleChildScrollView(
              child: Column(
                children: [
                  // Counter Widget
                  _CounterWidget(stateSync: _stateSync),
                  
                  // Settings Widget  
                  _SettingsWidget(stateSync: _stateSync),
                  
                  // Profile Widget
                  _ProfileWidget(stateSync: _stateSync),
                  
                  // Batch Operations
                  Card(
                    margin: const EdgeInsets.all(16),
                    child: Padding(
                      padding: const EdgeInsets.all(16),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            'Batch Operations',
                            style: Theme.of(context).textTheme.titleMedium,
                          ),
                          const SizedBox(height: 8),
                          Row(
                            children: [
                              Expanded(
                                child: ElevatedButton(
                                  onPressed: () {
                                    _stateSync.batchUpdate({
                                      'counter': (_stateSync.getState<int>('counter') ?? 0) + 10,
                                      'lastAction': 'batch_increment',
                                      'timestamp': DateTime.now().millisecondsSinceEpoch,
                                    }, source: 'batch_controls');
                                  },
                                  child: const Text('Batch +10'),
                                ),
                              ),
                              const SizedBox(width: 8),
                              Expanded(
                                child: ElevatedButton(
                                  onPressed: _simulateRemoteSync,
                                  style: ElevatedButton.styleFrom(
                                    backgroundColor: Colors.blueAccent,
                                  ),
                                  child: const Text('Simulate Remote'),
                                ),
                              ),
                            ],
                          ),
                        ],
                      ),
                    ),
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }

  void _simulateRemoteSync() {
    // Simulate receiving state from remote source
    final remoteState = {
      'counter': (_stateSync.getState<int>('counter') ?? 0) + 5,
      'serverMessage': 'Updated from server at ${DateTime.now().millisecondsSinceEpoch}',
      'remoteFlag': true,
    };

    _stateSync.mergeState(remoteState, source: 'remote_server');
  }

  void _showEventLog() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Sync Event Log'),
        content: SizedBox(
          width: double.maxFinite,
          height: 400,
          child: _stateSync.eventLog.isEmpty
              ? const Center(child: Text('No events yet'))
              : ListView.builder(
                  itemCount: _stateSync.eventLog.length,
                  reverse: true,
                  itemBuilder: (context, index) {
                    final event = _stateSync.eventLog[
                        _stateSync.eventLog.length - 1 - index];
                    return Card(
                      child: Padding(
                        padding: const EdgeInsets.all(8),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Row(
                              children: [
                                Text(
                                  event.type,
                                  style: const TextStyle(fontWeight: FontWeight.bold),
                                ),
                                const Spacer(),
                                Text(
                                  '${event.timestamp.hour}:${event.timestamp.minute.toString().padLeft(2, '0')}:${event.timestamp.second.toString().padLeft(2, '0')}',
                                  style: const TextStyle(fontSize: 12),
                                ),
                              ],
                            ),
                            Text(
                              'Source: ${event.source}',
                              style: const TextStyle(fontSize: 12),
                            ),
                            if (event.data.isNotEmpty)
                              Text(
                                'Data: ${event.data.toString()}',
                                style: const TextStyle(fontSize: 10),
                                maxLines: 2,
                                overflow: TextOverflow.ellipsis,
                              ),
                          ],
                        ),
                      ),
                    );
                  },
                ),
        ),
        actions: [
          TextButton(
            onPressed: () {
              _stateSync.clearEventLog();
              Navigator.pop(context);
            },
            child: const Text('Clear'),
          ),
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }
}

class _CounterWidget extends StatelessWidget {
  final StateSync stateSync;

  const _CounterWidget({required this.stateSync});

  @override
  Widget build(BuildContext context) {
    final counter = stateSync.getState<int>('counter') ?? 0;
    
    return Card(
      margin: const EdgeInsets.all(16),
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            Text(
              'Counter Widget',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 8),
            Text(
              '$counter',
              style: const TextStyle(fontSize: 36, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () => stateSync.setState(
                    'counter', counter - 1, source: 'counter_widget'),
                  child: const Text('-'),
                ),
                const SizedBox(width: 16),
                ElevatedButton(
                  onPressed: () => stateSync.setState(
                    'counter', counter + 1, source: 'counter_widget'),
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

class _SettingsWidget extends StatelessWidget {
  final StateSync stateSync;

  const _SettingsWidget({required this.stateSync});

  @override
  Widget build(BuildContext context) {
    final notifications = stateSync.getState<bool>('notifications') ?? false;
    final theme = stateSync.getState<String>('theme') ?? 'dark';
    
    return Card(
      margin: const EdgeInsets.all(16),
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Settings Widget',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 8),
            SwitchListTile(
              title: const Text('Notifications'),
              value: notifications,
              onChanged: (value) => stateSync.setState(
                'notifications', value, source: 'settings_widget'),
            ),
            DropdownButtonFormField<String>(
              value: theme,
              decoration: const InputDecoration(labelText: 'Theme'),
              items: ['light', 'dark', 'auto'].map((theme) =>
                DropdownMenuItem(value: theme, child: Text(theme))).toList(),
              onChanged: (value) => stateSync.setState(
                'theme', value, source: 'settings_widget'),
            ),
          ],
        ),
      ),
    );
  }
}

class _ProfileWidget extends StatelessWidget {
  final StateSync stateSync;

  const _ProfileWidget({required this.stateSync});

  @override
  Widget build(BuildContext context) {
    final userName = stateSync.getState<String>('userName') ?? 'User';
    
    return Card(
      margin: const EdgeInsets.all(16),
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Profile Widget',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 8),
            TextField(
              decoration: const InputDecoration(
                labelText: 'User Name',
                border: OutlineInputBorder(),
              ),
              controller: TextEditingController(text: userName),
              onSubmitted: (value) => stateSync.setState(
                'userName', value, source: 'profile_widget'),
            ),
          ],
        ),
      ),
    );
  }
}
```

State synchronization enables coordinated updates across multiple widgets  
and data sources. This implementation provides event-driven communication,  
conflict resolution for concurrent updates, and batch synchronization for  
performance. The pattern includes event logging, source tracking, and  
merge strategies essential for distributed state management in complex  
applications with real-time collaboration features.

## State Machine Pattern

Implementing finite state machines for complex state logic.  

```dart
import 'package:flutter/material.dart';
import 'dart:async';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'State Machine Pattern',
      home: const StateMachinePage(),
    );
  }
}

// Order States
enum OrderState {
  draft,
  pending,
  confirmed,
  preparing,
  ready,
  delivering,
  delivered,
  cancelled,
  refunded,
}

// Order Events
enum OrderEvent {
  submit,
  confirm,
  startPreparing,
  finishPreparing,
  startDelivery,
  completeDelivery,
  cancel,
  refund,
}

class StateTransition {
  final OrderState from;
  final OrderState to;
  final OrderEvent event;
  final String? condition;
  final String description;

  const StateTransition({
    required this.from,
    required this.to,
    required this.event,
    this.condition,
    required this.description,
  });
}

class OrderStateMachine extends ChangeNotifier {
  OrderState _currentState = OrderState.draft;
  final List<StateTransition> _history = [];
  Timer? _autoTimer;
  
  // Define valid transitions
  static const List<StateTransition> _transitions = [
    // From Draft
    StateTransition(
      from: OrderState.draft,
      to: OrderState.pending,
      event: OrderEvent.submit,
      description: 'Order submitted for review',
    ),
    StateTransition(
      from: OrderState.draft,
      to: OrderState.cancelled,
      event: OrderEvent.cancel,
      description: 'Order cancelled while in draft',
    ),
    
    // From Pending
    StateTransition(
      from: OrderState.pending,
      to: OrderState.confirmed,
      event: OrderEvent.confirm,
      description: 'Order confirmed and payment processed',
    ),
    StateTransition(
      from: OrderState.pending,
      to: OrderState.cancelled,
      event: OrderEvent.cancel,
      description: 'Order cancelled during review',
    ),
    
    // From Confirmed
    StateTransition(
      from: OrderState.confirmed,
      to: OrderState.preparing,
      event: OrderEvent.startPreparing,
      description: 'Kitchen started preparing order',
    ),
    StateTransition(
      from: OrderState.confirmed,
      to: OrderState.cancelled,
      event: OrderEvent.cancel,
      condition: 'Before preparation starts',
      description: 'Order cancelled after confirmation',
    ),
    
    // From Preparing
    StateTransition(
      from: OrderState.preparing,
      to: OrderState.ready,
      event: OrderEvent.finishPreparing,
      description: 'Order preparation completed',
    ),
    
    // From Ready
    StateTransition(
      from: OrderState.ready,
      to: OrderState.delivering,
      event: OrderEvent.startDelivery,
      description: 'Order picked up for delivery',
    ),
    
    // From Delivering
    StateTransition(
      from: OrderState.delivering,
      to: OrderState.delivered,
      event: OrderEvent.completeDelivery,
      description: 'Order successfully delivered',
    ),
    
    // From Delivered/Cancelled
    StateTransition(
      from: OrderState.delivered,
      to: OrderState.refunded,
      event: OrderEvent.refund,
      condition: 'Within refund period',
      description: 'Order refunded',
    ),
    StateTransition(
      from: OrderState.cancelled,
      to: OrderState.refunded,
      event: OrderEvent.refund,
      condition: 'Payment was processed',
      description: 'Cancelled order refunded',
    ),
  ];

  // Getters
  OrderState get currentState => _currentState;
  List<StateTransition> get history => List.unmodifiable(_history);
  
  List<OrderEvent> get availableEvents {
    return _transitions
        .where((t) => t.from == _currentState)
        .map((t) => t.event)
        .toList();
  }

  String get stateDescription {
    switch (_currentState) {
      case OrderState.draft:
        return 'Order being created';
      case OrderState.pending:
        return 'Waiting for confirmation';
      case OrderState.confirmed:
        return 'Order confirmed, payment processed';
      case OrderState.preparing:
        return 'Kitchen preparing your order';
      case OrderState.ready:
        return 'Order ready for pickup/delivery';
      case OrderState.delivering:
        return 'Order on the way';
      case OrderState.delivered:
        return 'Order successfully delivered';
      case OrderState.cancelled:
        return 'Order was cancelled';
      case OrderState.refunded:
        return 'Order refunded';
    }
  }

  Color get stateColor {
    switch (_currentState) {
      case OrderState.draft:
        return Colors.grey;
      case OrderState.pending:
        return Colors.orange;
      case OrderState.confirmed:
        return Colors.blue;
      case OrderState.preparing:
        return Colors.purple;
      case OrderState.ready:
        return Colors.green;
      case OrderState.delivering:
        return Colors.teal;
      case OrderState.delivered:
        return Colors.lightGreen;
      case OrderState.cancelled:
        return Colors.red;
      case OrderState.refunded:
        return Colors.pink;
    }
  }

  bool get canTransition => availableEvents.isNotEmpty;
  bool get isTerminal => _currentState == OrderState.delivered || 
                         _currentState == OrderState.refunded;

  bool canTriggerEvent(OrderEvent event) {
    return availableEvents.contains(event);
  }

  String? getEventDescription(OrderEvent event) {
    final transition = _transitions.firstWhere(
      (t) => t.from == _currentState && t.event == event,
      orElse: () => throw StateError('Invalid transition'),
    );
    return transition.description;
  }

  bool triggerEvent(OrderEvent event) {
    if (!canTriggerEvent(event)) {
      return false;
    }

    final transition = _transitions.firstWhere(
      (t) => t.from == _currentState && t.event == event,
    );

    final oldState = _currentState;
    _currentState = transition.to;

    _history.add(StateTransition(
      from: oldState,
      to: _currentState,
      event: event,
      description: transition.description,
    ));

    notifyListeners();
    _scheduleAutoTransition();
    
    return true;
  }

  void _scheduleAutoTransition() {
    _autoTimer?.cancel();

    // Auto-transition for certain states
    switch (_currentState) {
      case OrderState.preparing:
        // Simulate preparation time
        _autoTimer = Timer(const Duration(seconds: 5), () {
          if (_currentState == OrderState.preparing) {
            triggerEvent(OrderEvent.finishPreparing);
          }
        });
        break;
      case OrderState.delivering:
        // Simulate delivery time
        _autoTimer = Timer(const Duration(seconds: 8), () {
          if (_currentState == OrderState.delivering) {
            triggerEvent(OrderEvent.completeDelivery);
          }
        });
        break;
      default:
        break;
    }
  }

  void reset() {
    _autoTimer?.cancel();
    _currentState = OrderState.draft;
    _history.clear();
    notifyListeners();
  }

  List<OrderState> getStateFlow() {
    // Return the typical happy path flow
    return [
      OrderState.draft,
      OrderState.pending,
      OrderState.confirmed,
      OrderState.preparing,
      OrderState.ready,
      OrderState.delivering,
      OrderState.delivered,
    ];
  }

  Map<String, dynamic> getStateInfo() {
    return {
      'currentState': _currentState.toString(),
      'description': stateDescription,
      'availableEvents': availableEvents.map((e) => e.toString()).toList(),
      'isTerminal': isTerminal,
      'historyCount': _history.length,
    };
  }

  @override
  void dispose() {
    _autoTimer?.cancel();
    super.dispose();
  }
}

class StateMachinePage extends StatefulWidget {
  const StateMachinePage({super.key});

  @override
  State<StateMachinePage> createState() => _StateMachinePageState();
}

class _StateMachinePageState extends State<StateMachinePage> {
  late OrderStateMachine _stateMachine;

  @override
  void initState() {
    super.initState();
    _stateMachine = OrderStateMachine();
    _stateMachine.addListener(_onStateChanged);
  }

  @override
  void dispose() {
    _stateMachine.removeListener(_onStateChanged);
    _stateMachine.dispose();
    super.dispose();
  }

  void _onStateChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('State Machine Pattern'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: _stateMachine.reset,
          ),
          IconButton(
            icon: const Icon(Icons.info_outline),
            onPressed: _showStateInfo,
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            // Current State Display
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    Row(
                      children: [
                        Container(
                          width: 20,
                          height: 20,
                          decoration: BoxDecoration(
                            color: _stateMachine.stateColor,
                            shape: BoxShape.circle,
                          ),
                        ),
                        const SizedBox(width: 12),
                        Expanded(
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Text(
                                _stateMachine.currentState.toString().split('.').last.toUpperCase(),
                                style: const TextStyle(
                                  fontSize: 20,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                              Text(
                                _stateMachine.stateDescription,
                                style: const TextStyle(fontSize: 14),
                              ),
                            ],
                          ),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // State Flow Visualization
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Order Flow',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 12),
                    _buildStateFlowVisualization(),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Available Actions
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Available Actions',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 12),
                    if (_stateMachine.availableEvents.isEmpty) ...[
                      Container(
                        padding: const EdgeInsets.all(16),
                        decoration: BoxDecoration(
                          color: _stateMachine.isTerminal 
                              ? Colors.green.withOpacity(0.1)
                              : Colors.grey.withOpacity(0.1),
                          borderRadius: BorderRadius.circular(8),
                        ),
                        child: Row(
                          children: [
                            Icon(
                              _stateMachine.isTerminal 
                                  ? Icons.check_circle 
                                  : Icons.block,
                              color: _stateMachine.isTerminal 
                                  ? Colors.green 
                                  : Colors.grey,
                            ),
                            const SizedBox(width: 8),
                            Text(
                              _stateMachine.isTerminal 
                                  ? 'Order completed!' 
                                  : 'No actions available',
                            ),
                          ],
                        ),
                      ),
                    ] else ...[
                      Wrap(
                        spacing: 8,
                        runSpacing: 8,
                        children: _stateMachine.availableEvents.map((event) {
                          return ElevatedButton.icon(
                            onPressed: () => _stateMachine.triggerEvent(event),
                            icon: _getEventIcon(event),
                            label: Text(_getEventLabel(event)),
                            style: ElevatedButton.styleFrom(
                              backgroundColor: _getEventColor(event),
                            ),
                          );
                        }).toList(),
                      ),
                    ],
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // History
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        Text(
                          'Transition History',
                          style: Theme.of(context).textTheme.titleMedium,
                        ),
                        const Spacer(),
                        if (_stateMachine.history.isNotEmpty)
                          TextButton(
                            onPressed: _showFullHistory,
                            child: const Text('View All'),
                          ),
                      ],
                    ),
                    const SizedBox(height: 8),
                    if (_stateMachine.history.isEmpty) ...[
                      const Text('No transitions yet'),
                    ] else ...[
                      ..._stateMachine.history.take(3).map((transition) {
                        return Padding(
                          padding: const EdgeInsets.symmetric(vertical: 4),
                          child: Row(
                            children: [
                              Container(
                                width: 8,
                                height: 8,
                                decoration: const BoxDecoration(
                                  color: Colors.blueAccent,
                                  shape: BoxShape.circle,
                                ),
                              ),
                              const SizedBox(width: 12),
                              Expanded(
                                child: Text(
                                  '${transition.from.toString().split('.').last} → '
                                  '${transition.to.toString().split('.').last}',
                                  style: const TextStyle(fontWeight: FontWeight.w500),
                                ),
                              ),
                            ],
                          ),
                        );
                      }),
                      if (_stateMachine.history.length > 3) ...[
                        const SizedBox(height: 4),
                        Text(
                          '... and ${_stateMachine.history.length - 3} more transitions',
                          style: const TextStyle(
                            fontSize: 12,
                            color: Colors.grey,
                          ),
                        ),
                      ],
                    ],
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildStateFlowVisualization() {
    final flow = _stateMachine.getStateFlow();
    final currentState = _stateMachine.currentState;
    
    return Wrap(
      alignment: WrapAlignment.center,
      children: flow.asMap().entries.map((entry) {
        final index = entry.key;
        final state = entry.value;
        final isCurrent = state == currentState;
        final isPassed = flow.indexOf(currentState) > index;
        
        return Row(
          mainAxisSize: MainAxisSize.min,
          children: [
            Container(
              padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
              decoration: BoxDecoration(
                color: isCurrent 
                    ? _stateMachine.stateColor.withOpacity(0.3)
                    : isPassed 
                        ? Colors.green.withOpacity(0.1)
                        : Colors.grey.withOpacity(0.1),
                border: Border.all(
                  color: isCurrent 
                      ? _stateMachine.stateColor
                      : isPassed 
                          ? Colors.green
                          : Colors.grey,
                ),
                borderRadius: BorderRadius.circular(4),
              ),
              child: Text(
                state.toString().split('.').last,
                style: TextStyle(
                  fontSize: 10,
                  fontWeight: isCurrent ? FontWeight.bold : null,
                  color: isCurrent 
                      ? _stateMachine.stateColor
                      : isPassed 
                          ? Colors.green
                          : Colors.grey,
                ),
              ),
            ),
            if (index < flow.length - 1) ...[
              Icon(
                Icons.arrow_forward,
                size: 16,
                color: isPassed ? Colors.green : Colors.grey,
              ),
            ],
          ],
        );
      }).toList(),
    );
  }

  Icon _getEventIcon(OrderEvent event) {
    switch (event) {
      case OrderEvent.submit:
        return const Icon(Icons.send);
      case OrderEvent.confirm:
        return const Icon(Icons.check);
      case OrderEvent.startPreparing:
        return const Icon(Icons.kitchen);
      case OrderEvent.finishPreparing:
        return const Icon(Icons.done);
      case OrderEvent.startDelivery:
        return const Icon(Icons.local_shipping);
      case OrderEvent.completeDelivery:
        return const Icon(Icons.home);
      case OrderEvent.cancel:
        return const Icon(Icons.cancel);
      case OrderEvent.refund:
        return const Icon(Icons.money_off);
    }
  }

  String _getEventLabel(OrderEvent event) {
    switch (event) {
      case OrderEvent.submit:
        return 'Submit Order';
      case OrderEvent.confirm:
        return 'Confirm';
      case OrderEvent.startPreparing:
        return 'Start Preparing';
      case OrderEvent.finishPreparing:
        return 'Ready';
      case OrderEvent.startDelivery:
        return 'Start Delivery';
      case OrderEvent.completeDelivery:
        return 'Delivered';
      case OrderEvent.cancel:
        return 'Cancel';
      case OrderEvent.refund:
        return 'Refund';
    }
  }

  Color _getEventColor(OrderEvent event) {
    switch (event) {
      case OrderEvent.cancel:
        return Colors.redAccent;
      case OrderEvent.refund:
        return Colors.orangeAccent;
      case OrderEvent.confirm:
      case OrderEvent.completeDelivery:
        return Colors.greenAccent;
      default:
        return Colors.blueAccent;
    }
  }

  void _showStateInfo() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('State Machine Info'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: _stateMachine.getStateInfo().entries.map((entry) {
            return Padding(
              padding: const EdgeInsets.symmetric(vertical: 2),
              child: Text('${entry.key}: ${entry.value}'),
            );
          }).toList(),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }

  void _showFullHistory() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Full Transition History'),
        content: SizedBox(
          width: double.maxFinite,
          height: 300,
          child: ListView.builder(
            itemCount: _stateMachine.history.length,
            reverse: true,
            itemBuilder: (context, index) {
              final transition = _stateMachine.history[
                  _stateMachine.history.length - 1 - index];
              return ListTile(
                leading: Text('${index + 1}.'),
                title: Text(
                  '${transition.from.toString().split('.').last} → '
                  '${transition.to.toString().split('.').last}',
                ),
                subtitle: Text(transition.description),
              );
            },
          ),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }
}
```

State machine pattern provides structured management of complex state  
transitions with explicit rules and validation. This implementation defines  
valid state transitions, prevents invalid operations, and maintains  
transition history. The pattern excels for workflows, game states, UI  
navigation, and business processes where state changes must follow strict  
rules. Auto-transitions and visual flow representation enhance user  
understanding and system reliability.

## Timer-Based State

Managing time-dependent state changes and scheduled operations.  

```dart
import 'dart:async';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Timer-Based State',
      home: const TimerStatePage(),
    );
  }
}

class TimerController extends ChangeNotifier {
  Timer? _timer;
  DateTime? _startTime;
  Duration _elapsed = Duration.zero;
  Duration _duration = const Duration(minutes: 5);
  bool _isRunning = false;
  bool _isPaused = false;
  
  String _label = 'Timer';
  TimerMode _mode = TimerMode.countdown;
  
  // Getters
  Duration get elapsed => _elapsed;
  Duration get duration => _duration;
  Duration get remaining => _mode == TimerMode.countdown 
      ? _duration - _elapsed 
      : Duration.zero;
  bool get isRunning => _isRunning;
  bool get isPaused => _isPaused;
  String get label => _label;
  TimerMode get mode => _mode;
  
  bool get isExpired => _mode == TimerMode.countdown && remaining <= Duration.zero;
  bool get canStart => !_isRunning && !isExpired;
  bool get canPause => _isRunning && !isPaused;
  bool get canResume => _isRunning && _isPaused;
  bool get canStop => _isRunning || _isPaused;
  
  double get progress {
    if (_mode == TimerMode.stopwatch) {
      return _elapsed.inMilliseconds / _duration.inMilliseconds;
    } else {
      return 1.0 - (remaining.inMilliseconds / _duration.inMilliseconds);
    }
  }

  String get formattedTime {
    final time = _mode == TimerMode.countdown ? remaining : elapsed;
    final hours = time.inHours;
    final minutes = time.inMinutes.remainder(60);
    final seconds = time.inSeconds.remainder(60);
    
    if (hours > 0) {
      return '${hours.toString().padLeft(2, '0')}:'
             '${minutes.toString().padLeft(2, '0')}:'
             '${seconds.toString().padLeft(2, '0')}';
    } else {
      return '${minutes.toString().padLeft(2, '0')}:'
             '${seconds.toString().padLeft(2, '0')}';
    }
  }

  void setDuration(Duration newDuration) {
    if (_isRunning) return;
    _duration = newDuration;
    _elapsed = Duration.zero;
    notifyListeners();
  }

  void setLabel(String newLabel) {
    _label = newLabel;
    notifyListeners();
  }

  void setMode(TimerMode newMode) {
    if (_isRunning) return;
    _mode = newMode;
    _elapsed = Duration.zero;
    notifyListeners();
  }

  void start() {
    if (!canStart) return;
    
    _isRunning = true;
    _isPaused = false;
    _startTime = DateTime.now().subtract(_elapsed);
    
    _timer = Timer.periodic(const Duration(milliseconds: 100), _tick);
    notifyListeners();
  }

  void pause() {
    if (!canPause) return;
    
    _isPaused = true;
    _timer?.cancel();
    notifyListeners();
  }

  void resume() {
    if (!canResume) return;
    
    _isPaused = false;
    _startTime = DateTime.now().subtract(_elapsed);
    
    _timer = Timer.periodic(const Duration(milliseconds: 100), _tick);
    notifyListeners();
  }

  void stop() {
    _timer?.cancel();
    _isRunning = false;
    _isPaused = false;
    _elapsed = Duration.zero;
    _startTime = null;
    notifyListeners();
  }

  void reset() {
    _timer?.cancel();
    _isRunning = false;
    _isPaused = false;
    _elapsed = Duration.zero;
    _startTime = null;
    notifyListeners();
  }

  void _tick(Timer timer) {
    if (_startTime == null) return;
    
    _elapsed = DateTime.now().difference(_startTime!);
    
    // Check if countdown timer expired
    if (_mode == TimerMode.countdown && isExpired) {
      _timer?.cancel();
      _isRunning = false;
      _isPaused = false;
      _elapsed = _duration;
    }
    
    notifyListeners();
  }

  @override
  void dispose() {
    _timer?.cancel();
    super.dispose();
  }
}

enum TimerMode { countdown, stopwatch }

class PomodoroController extends ChangeNotifier {
  static const Duration _workDuration = Duration(minutes: 25);
  static const Duration _shortBreakDuration = Duration(minutes: 5);
  static const Duration _longBreakDuration = Duration(minutes: 15);
  
  final TimerController _timer = TimerController();
  PomodoroState _state = PomodoroState.work;
  int _completedPomodoros = 0;
  int _totalPomodoros = 0;
  DateTime? _sessionStart;
  
  PomodoroController() {
    _timer.addListener(notifyListeners);
    _setupWorkTimer();
  }

  // Getters
  TimerController get timer => _timer;
  PomodoroState get state => _state;
  int get completedPomodoros => _completedPomodoros;
  int get totalPomodoros => _totalPomodoros;
  DateTime? get sessionStart => _sessionStart;
  
  Duration get sessionDuration => _sessionStart != null 
      ? DateTime.now().difference(_sessionStart!)
      : Duration.zero;

  String get stateLabel {
    switch (_state) {
      case PomodoroState.work:
        return 'Work Time';
      case PomodoroState.shortBreak:
        return 'Short Break';
      case PomodoroState.longBreak:
        return 'Long Break';
    }
  }

  Color get stateColor {
    switch (_state) {
      case PomodoroState.work:
        return Colors.redAccent;
      case PomodoroState.shortBreak:
        return Colors.greenAccent;
      case PomodoroState.longBreak:
        return Colors.blueAccent;
    }
  }

  void startSession() {
    if (_sessionStart == null) {
      _sessionStart = DateTime.now();
    }
    _timer.start();
  }

  void completePomodoro() {
    if (_state == PomodoroState.work) {
      _completedPomodoros++;
      _totalPomodoros++;
      
      // Determine next state
      if (_completedPomodoros % 4 == 0) {
        _state = PomodoroState.longBreak;
        _setupLongBreakTimer();
      } else {
        _state = PomodoroState.shortBreak;
        _setupShortBreakTimer();
      }
    } else {
      // Break completed, back to work
      _state = PomodoroState.work;
      _setupWorkTimer();
    }
    
    notifyListeners();
  }

  void skipToNextState() {
    _timer.stop();
    
    switch (_state) {
      case PomodoroState.work:
        _state = PomodoroState.shortBreak;
        _setupShortBreakTimer();
        break;
      case PomodoroState.shortBreak:
        _state = PomodoroState.work;
        _setupWorkTimer();
        break;
      case PomodoroState.longBreak:
        _state = PomodoroState.work;
        _setupWorkTimer();
        break;
    }
    
    notifyListeners();
  }

  void resetSession() {
    _timer.stop();
    _completedPomodoros = 0;
    _totalPomodoros = 0;
    _sessionStart = null;
    _state = PomodoroState.work;
    _setupWorkTimer();
    notifyListeners();
  }

  void _setupWorkTimer() {
    _timer.setMode(TimerMode.countdown);
    _timer.setDuration(_workDuration);
    _timer.setLabel('Work');
  }

  void _setupShortBreakTimer() {
    _timer.setMode(TimerMode.countdown);
    _timer.setDuration(_shortBreakDuration);
    _timer.setLabel('Short Break');
  }

  void _setupLongBreakTimer() {
    _timer.setMode(TimerMode.countdown);
    _timer.setDuration(_longBreakDuration);
    _timer.setLabel('Long Break');
  }

  @override
  void dispose() {
    _timer.dispose();
    super.dispose();
  }
}

enum PomodoroState { work, shortBreak, longBreak }

class TimerStatePage extends StatefulWidget {
  const TimerStatePage({super.key});

  @override
  State<TimerStatePage> createState() => _TimerStatePageState();
}

class _TimerStatePageState extends State<TimerStatePage>
    with TickerProviderStateMixin {
  late TabController _tabController;
  late TimerController _simpleTimer;
  late PomodoroController _pomodoroController;

  @override
  void initState() {
    super.initState();
    _tabController = TabController(length: 2, vsync: this);
    
    _simpleTimer = TimerController();
    _simpleTimer.addListener(_onSimpleTimerChanged);
    
    _pomodoroController = PomodoroController();
    _pomodoroController.addListener(_onPomodoroChanged);
  }

  @override
  void dispose() {
    _tabController.dispose();
    _simpleTimer.removeListener(_onSimpleTimerChanged);
    _simpleTimer.dispose();
    _pomodoroController.removeListener(_onPomodoroChanged);
    _pomodoroController.dispose();
    super.dispose();
  }

  void _onSimpleTimerChanged() {
    setState(() {});
  }

  void _onPomodoroChanged() {
    setState(() {});
    
    // Auto-advance pomodoro when timer expires
    if (_pomodoroController.timer.isExpired) {
      _pomodoroController.completePomodoro();
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Timer-Based State'),
        bottom: TabBar(
          controller: _tabController,
          tabs: const [
            Tab(text: 'Simple Timer'),
            Tab(text: 'Pomodoro'),
          ],
        ),
      ),
      body: TabBarView(
        controller: _tabController,
        children: [
          _buildSimpleTimerTab(),
          _buildPomodoroTab(),
        ],
      ),
    );
  }

  Widget _buildSimpleTimerTab() {
    return SingleChildScrollView(
      padding: const EdgeInsets.all(16),
      child: Column(
        children: [
          // Timer Display
          Card(
            child: Padding(
              padding: const EdgeInsets.all(24),
              child: Column(
                children: [
                  Text(
                    _simpleTimer.formattedTime,
                    style: const TextStyle(
                      fontSize: 48,
                      fontWeight: FontWeight.bold,
                      fontFamily: 'monospace',
                    ),
                  ),
                  const SizedBox(height: 16),
                  LinearProgressIndicator(
                    value: _simpleTimer.progress.clamp(0.0, 1.0),
                    backgroundColor: Colors.grey[700],
                    valueColor: AlwaysStoppedAnimation<Color>(
                      _simpleTimer.isExpired ? Colors.redAccent : Colors.blueAccent,
                    ),
                  ),
                  const SizedBox(height: 8),
                  Text(
                    '${_simpleTimer.mode.toString().split('.').last.toUpperCase()} - '
                    '${_simpleTimer.label}',
                    style: const TextStyle(fontSize: 16),
                  ),
                ],
              ),
            ),
          ),

          const SizedBox(height: 16),

          // Timer Controls
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                children: [
                  Row(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      ElevatedButton.icon(
                        onPressed: _simpleTimer.canStart ? _simpleTimer.start : null,
                        icon: const Icon(Icons.play_arrow),
                        label: const Text('Start'),
                      ),
                      const SizedBox(width: 8),
                      ElevatedButton.icon(
                        onPressed: _simpleTimer.canPause ? _simpleTimer.pause : null,
                        icon: const Icon(Icons.pause),
                        label: const Text('Pause'),
                      ),
                      const SizedBox(width: 8),
                      ElevatedButton.icon(
                        onPressed: _simpleTimer.canResume ? _simpleTimer.resume : null,
                        icon: const Icon(Icons.play_arrow),
                        label: const Text('Resume'),
                      ),
                    ],
                  ),
                  const SizedBox(height: 8),
                  Row(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      ElevatedButton.icon(
                        onPressed: _simpleTimer.canStop ? _simpleTimer.stop : null,
                        icon: const Icon(Icons.stop),
                        label: const Text('Stop'),
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.redAccent,
                        ),
                      ),
                      const SizedBox(width: 8),
                      ElevatedButton.icon(
                        onPressed: _simpleTimer.reset,
                        icon: const Icon(Icons.refresh),
                        label: const Text('Reset'),
                      ),
                    ],
                  ),
                ],
              ),
            ),
          ),

          const SizedBox(height: 16),

          // Settings
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Settings',
                    style: Theme.of(context).textTheme.titleMedium,
                  ),
                  const SizedBox(height: 8),
                  DropdownButtonFormField<TimerMode>(
                    value: _simpleTimer.mode,
                    decoration: const InputDecoration(labelText: 'Mode'),
                    items: TimerMode.values.map((mode) => DropdownMenuItem(
                      value: mode,
                      child: Text(mode.toString().split('.').last),
                    )).toList(),
                    onChanged: _simpleTimer.isRunning ? null : (mode) {
                      if (mode != null) _simpleTimer.setMode(mode);
                    },
                  ),
                  const SizedBox(height: 8),
                  Row(
                    children: [
                      const Text('Duration: '),
                      Expanded(
                        child: Slider(
                          value: _simpleTimer.duration.inMinutes.toDouble(),
                          min: 1,
                          max: 60,
                          divisions: 59,
                          label: '${_simpleTimer.duration.inMinutes} min',
                          onChanged: _simpleTimer.isRunning ? null : (value) {
                            _simpleTimer.setDuration(Duration(minutes: value.toInt()));
                          },
                        ),
                      ),
                    ],
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildPomodoroTab() {
    return SingleChildScrollView(
      padding: const EdgeInsets.all(16),
      child: Column(
        children: [
          // Pomodoro Display
          Card(
            color: _pomodoroController.stateColor.withOpacity(0.1),
            child: Padding(
              padding: const EdgeInsets.all(24),
              child: Column(
                children: [
                  Text(
                    _pomodoroController.stateLabel,
                    style: TextStyle(
                      fontSize: 24,
                      fontWeight: FontWeight.bold,
                      color: _pomodoroController.stateColor,
                    ),
                  ),
                  const SizedBox(height: 8),
                  Text(
                    _pomodoroController.timer.formattedTime,
                    style: const TextStyle(
                      fontSize: 48,
                      fontWeight: FontWeight.bold,
                      fontFamily: 'monospace',
                    ),
                  ),
                  const SizedBox(height: 16),
                  LinearProgressIndicator(
                    value: _pomodoroController.timer.progress.clamp(0.0, 1.0),
                    backgroundColor: Colors.grey[700],
                    valueColor: AlwaysStoppedAnimation<Color>(
                      _pomodoroController.stateColor,
                    ),
                  ),
                ],
              ),
            ),
          ),

          const SizedBox(height: 16),

          // Pomodoro Stats
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceAround,
                children: [
                  Column(
                    children: [
                      Text(
                        '${_pomodoroController.completedPomodoros}',
                        style: const TextStyle(
                          fontSize: 24,
                          fontWeight: FontWeight.bold,
                          color: Colors.greenAccent,
                        ),
                      ),
                      const Text('Completed'),
                    ],
                  ),
                  Column(
                    children: [
                      Text(
                        '${_pomodoroController.totalPomodoros}',
                        style: const TextStyle(
                          fontSize: 24,
                          fontWeight: FontWeight.bold,
                          color: Colors.blueAccent,
                        ),
                      ),
                      const Text('Total'),
                    ],
                  ),
                  if (_pomodoroController.sessionStart != null)
                    Column(
                      children: [
                        Text(
                          '${_pomodoroController.sessionDuration.inMinutes}m',
                          style: const TextStyle(
                            fontSize: 24,
                            fontWeight: FontWeight.bold,
                            color: Colors.orangeAccent,
                          ),
                        ),
                        const Text('Session'),
                      ],
                    ),
                ],
              ),
            ),
          ),

          const SizedBox(height: 16),

          // Pomodoro Controls
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                children: [
                  Row(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      ElevatedButton.icon(
                        onPressed: _pomodoroController.timer.canStart 
                            ? _pomodoroController.startSession 
                            : null,
                        icon: const Icon(Icons.play_arrow),
                        label: const Text('Start'),
                      ),
                      const SizedBox(width: 8),
                      ElevatedButton.icon(
                        onPressed: _pomodoroController.timer.canPause 
                            ? _pomodoroController.timer.pause 
                            : null,
                        icon: const Icon(Icons.pause),
                        label: const Text('Pause'),
                      ),
                      const SizedBox(width: 8),
                      ElevatedButton.icon(
                        onPressed: _pomodoroController.timer.canResume 
                            ? _pomodoroController.timer.resume 
                            : null,
                        icon: const Icon(Icons.play_arrow),
                        label: const Text('Resume'),
                      ),
                    ],
                  ),
                  const SizedBox(height: 8),
                  Row(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      ElevatedButton.icon(
                        onPressed: _pomodoroController.skipToNextState,
                        icon: const Icon(Icons.skip_next),
                        label: const Text('Skip'),
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.orangeAccent,
                        ),
                      ),
                      const SizedBox(width: 8),
                      ElevatedButton.icon(
                        onPressed: _pomodoroController.resetSession,
                        icon: const Icon(Icons.refresh),
                        label: const Text('Reset Session'),
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.redAccent,
                        ),
                      ),
                    ],
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }
}
```

Timer-based state management handles time-dependent operations and  
scheduled state changes. This implementation demonstrates countdown timers,  
stopwatches, and complex time-based workflows like the Pomodoro Technique.  
The pattern includes precise timing, pause/resume functionality, progress  
tracking, and automatic state transitions. Essential for productivity apps,  
games, and any application requiring time-based behavior.

## Observable State Pattern

Implementing the observer pattern for reactive state management.  

```dart
import 'package:flutter/material.dart';
import 'dart:async';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Observable State Pattern',
      home: const ObservableStatePage(),
    );
  }
}

abstract class Observer<T> {
  void onChanged(T oldValue, T newValue);
}

class Observable<T> {
  T _value;
  final List<Observer<T>> _observers = [];
  final List<void Function(T, T)> _callbacks = [];
  String? _name;

  Observable(this._value, {String? name}) : _name = name;

  T get value => _value;
  String? get name => _name;
  int get observerCount => _observers.length + _callbacks.length;

  set value(T newValue) {
    if (_value != newValue) {
      final oldValue = _value;
      _value = newValue;
      _notifyObservers(oldValue, newValue);
    }
  }

  void addObserver(Observer<T> observer) {
    if (!_observers.contains(observer)) {
      _observers.add(observer);
    }
  }

  void removeObserver(Observer<T> observer) {
    _observers.remove(observer);
  }

  void addCallback(void Function(T oldValue, T newValue) callback) {
    _callbacks.add(callback);
  }

  void removeCallback(void Function(T oldValue, T newValue) callback) {
    _callbacks.remove(callback);
  }

  void _notifyObservers(T oldValue, T newValue) {
    // Notify observer objects
    for (final observer in _observers) {
      observer.onChanged(oldValue, newValue);
    }

    // Notify callback functions
    for (final callback in _callbacks) {
      callback(oldValue, newValue);
    }
  }

  void dispose() {
    _observers.clear();
    _callbacks.clear();
  }

  @override
  String toString() {
    return '${_name ?? 'Observable'}($_value)';
  }
}

class ComputedObservable<T> extends Observable<T> {
  final List<Observable> _dependencies = [];
  final T Function() _computeFunction;
  bool _isComputing = false;

  ComputedObservable(this._computeFunction, {String? name}) 
      : super(_computeFunction(), name: name);

  void dependsOn(Observable dependency) {
    if (!_dependencies.contains(dependency)) {
      _dependencies.add(dependency);
      dependency.addCallback(_onDependencyChanged);
    }
  }

  void _onDependencyChanged(dynamic oldValue, dynamic newValue) {
    if (!_isComputing) {
      _recompute();
    }
  }

  void _recompute() {
    _isComputing = true;
    final newValue = _computeFunction();
    _isComputing = false;
    
    if (_value != newValue) {
      final oldValue = _value;
      _value = newValue;
      _notifyObservers(oldValue, newValue);
    }
  }

  @override
  void dispose() {
    for (final dependency in _dependencies) {
      dependency.removeCallback(_onDependencyChanged);
    }
    _dependencies.clear();
    super.dispose();
  }
}

class ObservableList<T> extends Observable<List<T>> {
  ObservableList(List<T> initialList, {String? name}) 
      : super(List<T>.from(initialList), name: name);

  void add(T item) {
    final newList = List<T>.from(_value);
    newList.add(item);
    value = newList;
  }

  void remove(T item) {
    final newList = List<T>.from(_value);
    newList.remove(item);
    value = newList;
  }

  void removeAt(int index) {
    if (index >= 0 && index < _value.length) {
      final newList = List<T>.from(_value);
      newList.removeAt(index);
      value = newList;
    }
  }

  void clear() {
    if (_value.isNotEmpty) {
      value = [];
    }
  }

  int get length => _value.length;
  bool get isEmpty => _value.isEmpty;
  T operator [](int index) => _value[index];
}

class TaskModel {
  final String id;
  final String title;
  final bool isCompleted;
  final DateTime createdAt;

  const TaskModel({
    required this.id,
    required this.title,
    this.isCompleted = false,
    DateTime? createdAt,
  }) : createdAt = createdAt ?? const DateTime.fromMillisecondsSinceEpoch(0);

  TaskModel copyWith({
    String? id,
    String? title,
    bool? isCompleted,
    DateTime? createdAt,
  }) {
    return TaskModel(
      id: id ?? this.id,
      title: title ?? this.title,
      isCompleted: isCompleted ?? this.isCompleted,
      createdAt: createdAt ?? this.createdAt,
    );
  }

  @override
  bool operator ==(Object other) =>
      identical(this, other) ||
      other is TaskModel &&
          runtimeType == other.runtimeType &&
          id == other.id &&
          title == other.title &&
          isCompleted == other.isCompleted;

  @override
  int get hashCode => id.hashCode ^ title.hashCode ^ isCompleted.hashCode;
}

class ObservableStore extends ChangeNotifier {
  final ObservableList<TaskModel> _tasks = ObservableList<TaskModel>([], name: 'tasks');
  final Observable<String> _filter = Observable<String>('all', name: 'filter');
  final Observable<bool> _isLoading = Observable<bool>(false, name: 'isLoading');
  
  late final ComputedObservable<List<TaskModel>> _filteredTasks;
  late final ComputedObservable<int> _completedCount;
  late final ComputedObservable<int> _pendingCount;
  late final ComputedObservable<bool> _allCompleted;

  final StreamController<String> _actionController = StreamController<String>.broadcast();
  final List<String> _actionHistory = [];

  ObservableStore() {
    _setupComputedObservables();
    _setupObservers();
  }

  void _setupComputedObservables() {
    _filteredTasks = ComputedObservable<List<TaskModel>>(
      () {
        switch (_filter.value) {
          case 'completed':
            return _tasks.value.where((task) => task.isCompleted).toList();
          case 'pending':
            return _tasks.value.where((task) => !task.isCompleted).toList();
          default:
            return _tasks.value;
        }
      },
      name: 'filteredTasks',
    );
    _filteredTasks.dependsOn(_tasks);
    _filteredTasks.dependsOn(_filter);

    _completedCount = ComputedObservable<int>(
      () => _tasks.value.where((task) => task.isCompleted).length,
      name: 'completedCount',
    );
    _completedCount.dependsOn(_tasks);

    _pendingCount = ComputedObservable<int>(
      () => _tasks.value.where((task) => !task.isCompleted).length,
      name: 'pendingCount',
    );
    _pendingCount.dependsOn(_tasks);

    _allCompleted = ComputedObservable<bool>(
      () => _tasks.value.isNotEmpty && _pendingCount.value == 0,
      name: 'allCompleted',
    );
    _allCompleted.dependsOn(_tasks);
  }

  void _setupObservers() {
    _tasks.addCallback((oldTasks, newTasks) {
      _logAction('Tasks changed: ${oldTasks.length} -> ${newTasks.length}');
      notifyListeners();
    });

    _filter.addCallback((oldFilter, newFilter) {
      _logAction('Filter changed: $oldFilter -> $newFilter');
      notifyListeners();
    });

    _isLoading.addCallback((oldLoading, newLoading) {
      _logAction('Loading state: $oldLoading -> $newLoading');
      notifyListeners();
    });

    _filteredTasks.addCallback((oldTasks, newTasks) {
      _logAction('Filtered tasks: ${oldTasks.length} -> ${newTasks.length}');
    });

    _completedCount.addCallback((oldCount, newCount) {
      _logAction('Completed count: $oldCount -> $newCount');
    });

    _allCompleted.addCallback((oldValue, newValue) {
      if (newValue && !oldValue && _tasks.value.isNotEmpty) {
        _logAction('🎉 All tasks completed!');
      }
    });
  }

  void _logAction(String action) {
    _actionHistory.add('${DateTime.now().millisecondsSinceEpoch}: $action');
    if (_actionHistory.length > 50) {
      _actionHistory.removeAt(0);
    }
    _actionController.add(action);
  }

  // Getters
  ObservableList<TaskModel> get tasks => _tasks;
  Observable<String> get filter => _filter;
  Observable<bool> get isLoading => _isLoading;
  ComputedObservable<List<TaskModel>> get filteredTasks => _filteredTasks;
  ComputedObservable<int> get completedCount => _completedCount;
  ComputedObservable<int> get pendingCount => _pendingCount;
  ComputedObservable<bool> get allCompleted => _allCompleted;
  Stream<String> get actionStream => _actionController.stream;
  List<String> get actionHistory => List.unmodifiable(_actionHistory);

  // Actions
  void addTask(String title) {
    if (title.trim().isEmpty) return;
    
    final task = TaskModel(
      id: DateTime.now().millisecondsSinceEpoch.toString(),
      title: title.trim(),
      createdAt: DateTime.now(),
    );
    
    _tasks.add(task);
  }

  void toggleTask(String taskId) {
    final currentTasks = _tasks.value;
    final taskIndex = currentTasks.indexWhere((task) => task.id == taskId);
    
    if (taskIndex != -1) {
      final updatedTask = currentTasks[taskIndex].copyWith(
        isCompleted: !currentTasks[taskIndex].isCompleted,
      );
      
      final newTasks = List<TaskModel>.from(currentTasks);
      newTasks[taskIndex] = updatedTask;
      _tasks.value = newTasks;
    }
  }

  void removeTask(String taskId) {
    final currentTasks = _tasks.value;
    final newTasks = currentTasks.where((task) => task.id != taskId).toList();
    _tasks.value = newTasks;
  }

  void setFilter(String newFilter) {
    _filter.value = newFilter;
  }

  Future<void> loadTasks() async {
    _isLoading.value = true;
    
    // Simulate loading
    await Future.delayed(const Duration(seconds: 2));
    
    final sampleTasks = [
      TaskModel(id: '1', title: 'Learn Flutter state management'),
      TaskModel(id: '2', title: 'Build awesome app', isCompleted: true),
      TaskModel(id: '3', title: 'Write documentation'),
    ];
    
    _tasks.value = sampleTasks;
    _isLoading.value = false;
  }

  void clearCompleted() {
    final newTasks = _tasks.value.where((task) => !task.isCompleted).toList();
    _tasks.value = newTasks;
  }

  void toggleAll() {
    final shouldMarkAllCompleted = _pendingCount.value > 0;
    final newTasks = _tasks.value.map((task) {
      return task.copyWith(isCompleted: shouldMarkAllCompleted);
    }).toList();
    _tasks.value = newTasks;
  }

  Map<String, dynamic> getObservableStats() {
    return {
      'total_observables': 7,
      'tasks_observers': _tasks.observerCount,
      'filter_observers': _filter.observerCount,
      'loading_observers': _isLoading.observerCount,
      'filtered_tasks_observers': _filteredTasks.observerCount,
      'completed_count_observers': _completedCount.observerCount,
      'all_completed_observers': _allCompleted.observerCount,
    };
  }

  @override
  void dispose() {
    _tasks.dispose();
    _filter.dispose();
    _isLoading.dispose();
    _filteredTasks.dispose();
    _completedCount.dispose();
    _pendingCount.dispose();
    _allCompleted.dispose();
    _actionController.close();
    super.dispose();
  }
}

class ObservableStatePage extends StatefulWidget {
  const ObservableStatePage({super.key});

  @override
  State<ObservableStatePage> createState() => _ObservableStatePageState();
}

class _ObservableStatePageState extends State<ObservableStatePage> {
  late ObservableStore _store;
  final TextEditingController _taskController = TextEditingController();
  late StreamSubscription _actionSubscription;

  @override
  void initState() {
    super.initState();
    _store = ObservableStore();
    _store.addListener(_onStoreChanged);
    _actionSubscription = _store.actionStream.listen(_onAction);
    _store.loadTasks();
  }

  @override
  void dispose() {
    _store.removeListener(_onStoreChanged);
    _store.dispose();
    _taskController.dispose();
    _actionSubscription.cancel();
    super.dispose();
  }

  void _onStoreChanged() {
    setState(() {});
  }

  void _onAction(String action) {
    // Handle real-time action notifications
    if (action.contains('🎉')) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text(action),
          backgroundColor: Colors.green,
          duration: const Duration(seconds: 2),
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Observable State Pattern'),
        actions: [
          IconButton(
            icon: const Icon(Icons.analytics),
            onPressed: _showObservableStats,
          ),
          IconButton(
            icon: const Icon(Icons.history),
            onPressed: _showActionHistory,
          ),
        ],
      ),
      body: Column(
        children: [
          // Loading indicator
          if (_store.isLoading.value)
            const LinearProgressIndicator(),

          // Stats bar
          Container(
            padding: const EdgeInsets.all(16),
            color: Colors.grey[900],
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: [
                _buildStatItem('Total', _store.tasks.length.toString()),
                _buildStatItem('Completed', _store.completedCount.value.toString()),
                _buildStatItem('Pending', _store.pendingCount.value.toString()),
                if (_store.allCompleted.value)
                  const Icon(Icons.celebration, color: Colors.goldAccent),
              ],
            ),
          ),

          // Task input
          Padding(
            padding: const EdgeInsets.all(16),
            child: Row(
              children: [
                Expanded(
                  child: TextField(
                    controller: _taskController,
                    decoration: const InputDecoration(
                      hintText: 'Add a new task...',
                      border: OutlineInputBorder(),
                    ),
                    onSubmitted: (value) {
                      _store.addTask(value);
                      _taskController.clear();
                    },
                  ),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: () {
                    _store.addTask(_taskController.text);
                    _taskController.clear();
                  },
                  child: const Text('Add'),
                ),
              ],
            ),
          ),

          // Filter buttons
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 16),
            child: Row(
              children: [
                _buildFilterButton('all', 'All'),
                _buildFilterButton('pending', 'Pending'),
                _buildFilterButton('completed', 'Completed'),
                const Spacer(),
                if (_store.completedCount.value > 0)
                  TextButton(
                    onPressed: _store.clearCompleted,
                    child: const Text('Clear Completed'),
                  ),
                IconButton(
                  icon: Icon(
                    _store.allCompleted.value 
                        ? Icons.check_box 
                        : Icons.check_box_outline_blank,
                  ),
                  onPressed: _store.toggleAll,
                  tooltip: 'Toggle All',
                ),
              ],
            ),
          ),

          // Task list
          Expanded(
            child: _store.filteredTasks.value.isEmpty
                ? Center(
                    child: Text(
                      _store.isLoading.value 
                          ? 'Loading tasks...'
                          : 'No tasks found',
                      style: const TextStyle(fontSize: 16),
                    ),
                  )
                : ListView.builder(
                    itemCount: _store.filteredTasks.value.length,
                    itemBuilder: (context, index) {
                      final task = _store.filteredTasks.value[index];
                      return Card(
                        margin: const EdgeInsets.symmetric(
                          horizontal: 16,
                          vertical: 4,
                        ),
                        child: ListTile(
                          leading: Checkbox(
                            value: task.isCompleted,
                            onChanged: (_) => _store.toggleTask(task.id),
                          ),
                          title: Text(
                            task.title,
                            style: TextStyle(
                              decoration: task.isCompleted
                                  ? TextDecoration.lineThrough
                                  : null,
                            ),
                          ),
                          trailing: IconButton(
                            icon: const Icon(Icons.delete),
                            onPressed: () => _store.removeTask(task.id),
                          ),
                        ),
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }

  Widget _buildStatItem(String label, String value) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      children: [
        Text(
          value,
          style: const TextStyle(
            fontSize: 18,
            fontWeight: FontWeight.bold,
            color: Colors.blueAccent,
          ),
        ),
        Text(label, style: const TextStyle(fontSize: 12)),
      ],
    );
  }

  Widget _buildFilterButton(String filterValue, String label) {
    final isSelected = _store.filter.value == filterValue;
    return Padding(
      padding: const EdgeInsets.only(right: 8),
      child: FilterChip(
        label: Text(label),
        selected: isSelected,
        onSelected: (_) => _store.setFilter(filterValue),
      ),
    );
  }

  void _showObservableStats() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Observable Statistics'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: _store.getObservableStats().entries.map((entry) {
            return Text('${entry.key}: ${entry.value}');
          }).toList(),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }

  void _showActionHistory() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Action History'),
        content: SizedBox(
          width: double.maxFinite,
          height: 400,
          child: ListView.builder(
            reverse: true,
            itemCount: _store.actionHistory.length,
            itemBuilder: (context, index) {
              final action = _store.actionHistory[
                  _store.actionHistory.length - 1 - index];
              return Text(
                action,
                style: const TextStyle(fontSize: 12),
              );
            },
          ),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }
}
```

The Observable State Pattern implements reactive programming principles  
with automatic dependency tracking and computed values. This pattern  
provides fine-grained reactivity where changes to observables automatically  
trigger updates in dependent computed values and UI components. The  
implementation includes action logging, statistics tracking, and stream-based  
communication for real-time updates essential for complex reactive applications.

## Provider Pattern

Using the Provider package for dependency injection and state management.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Provider Pattern',
      home: const ProviderPage(),
    );
  }
}

class UserModel extends ChangeNotifier {
  String _name = '';
  String _email = '';
  int _points = 0;
  bool _isLoading = false;

  String get name => _name;
  String get email => _email;
  int get points => _points;
  bool get isLoading => _isLoading;

  Future<void> updateProfile(String name, String email) async {
    _isLoading = true;
    notifyListeners();

    await Future.delayed(const Duration(seconds: 2));

    _name = name;
    _email = email;
    _isLoading = false;
    notifyListeners();
  }

  void addPoints(int points) {
    _points += points;
    notifyListeners();
  }
}

class ProviderPage extends StatelessWidget {
  const ProviderPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Provider Pattern')),
      body: const Center(
        child: Text('Provider example would go here\nRequires provider package'),
      ),
    );
  }
}
```

Provider pattern simplifies dependency injection and state management  
through InheritedWidget under the hood. It provides type-safe access to  
shared state, automatic disposal, and rebuild optimization. While we  
demonstrate the concept here, real usage requires the provider package  
for complete functionality with Consumer and Selector widgets.

## Cubit Pattern

Implementing the Cubit pattern for predictable state management.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Cubit Pattern',
      home: const CubitPage(),
    );
  }
}

abstract class CounterState {
  const CounterState();
}

class CounterInitial extends CounterState {
  const CounterInitial();
}

class CounterValue extends CounterState {
  final int count;
  const CounterValue(this.count);
}

class CounterLoading extends CounterState {
  const CounterLoading();
}

class CounterError extends CounterState {
  final String message;
  const CounterError(this.message);
}

class CounterCubit extends ChangeNotifier {
  CounterState _state = const CounterInitial();
  
  CounterState get state => _state;

  void emit(CounterState newState) {
    _state = newState;
    notifyListeners();
  }

  void increment() {
    if (_state is CounterValue) {
      final currentValue = (_state as CounterValue).count;
      emit(CounterValue(currentValue + 1));
    } else {
      emit(const CounterValue(1));
    }
  }

  void decrement() {
    if (_state is CounterValue) {
      final currentValue = (_state as CounterValue).count;
      emit(CounterValue(currentValue - 1));
    } else {
      emit(const CounterValue(-1));
    }
  }

  Future<void> incrementAsync() async {
    emit(const CounterLoading());
    
    try {
      await Future.delayed(const Duration(seconds: 1));
      increment();
    } catch (e) {
      emit(CounterError(e.toString()));
    }
  }

  void reset() {
    emit(const CounterValue(0));
  }
}

class CubitPage extends StatefulWidget {
  const CubitPage({super.key});

  @override
  State<CubitPage> createState() => _CubitPageState();
}

class _CubitPageState extends State<CubitPage> {
  late CounterCubit _cubit;

  @override
  void initState() {
    super.initState();
    _cubit = CounterCubit();
    _cubit.addListener(_onCubitChanged);
  }

  @override
  void dispose() {
    _cubit.removeListener(_onCubitChanged);
    _cubit.dispose();
    super.dispose();
  }

  void _onCubitChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Cubit Pattern')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            _buildStateWidget(),
            const SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: _cubit.decrement,
                  child: const Text('-'),
                ),
                const SizedBox(width: 20),
                ElevatedButton(
                  onPressed: _cubit.increment,
                  child: const Text('+'),
                ),
              ],
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              onPressed: _cubit.incrementAsync,
              child: const Text('Async +1'),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildStateWidget() {
    final state = _cubit.state;
    
    if (state is CounterInitial) {
      return const Text('Initial State');
    } else if (state is CounterLoading) {
      return const CircularProgressIndicator();
    } else if (state is CounterValue) {
      return Text(
        '${state.count}',
        style: const TextStyle(fontSize: 48, fontWeight: FontWeight.bold),
      );
    } else if (state is CounterError) {
      return Text(
        'Error: ${state.message}',
        style: const TextStyle(color: Colors.red),
      );
    }
    
    return const Text('Unknown State');
  }
}
```

Cubit pattern provides predictable state management through explicit state  
classes and emission. Each state change is tracked and predictable, making  
debugging easier. The pattern separates business logic from UI concerns  
and provides clear state transitions with type safety.

## Event-Driven State

Managing state through event dispatching and handling.  

```dart
import 'dart:async';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Event-Driven State',
      home: const EventDrivenPage(),
    );
  }
}

abstract class AppEvent {
  const AppEvent();
}

class UserLoginEvent extends AppEvent {
  final String username;
  const UserLoginEvent(this.username);
}

class UserLogoutEvent extends AppEvent {
  const UserLogoutEvent();
}

class AddItemEvent extends AppEvent {
  final String item;
  const AddItemEvent(this.item);
}

class RemoveItemEvent extends AppEvent {
  final String item;
  const RemoveItemEvent(this.item);
}

class AppState {
  final bool isLoggedIn;
  final String username;
  final List<String> items;
  final DateTime lastUpdated;

  const AppState({
    this.isLoggedIn = false,
    this.username = '',
    this.items = const [],
    DateTime? lastUpdated,
  }) : lastUpdated = lastUpdated ?? const DateTime.fromMillisecondsSinceEpoch(0);

  AppState copyWith({
    bool? isLoggedIn,
    String? username,
    List<String>? items,
    DateTime? lastUpdated,
  }) {
    return AppState(
      isLoggedIn: isLoggedIn ?? this.isLoggedIn,
      username: username ?? this.username,
      items: items ?? this.items,
      lastUpdated: lastUpdated ?? DateTime.now(),
    );
  }
}

class EventBus extends ChangeNotifier {
  AppState _state = const AppState();
  final StreamController<AppEvent> _eventController = StreamController<AppEvent>.broadcast();
  final List<AppEvent> _eventHistory = [];

  AppState get state => _state;
  Stream<AppEvent> get eventStream => _eventController.stream;
  List<AppEvent> get eventHistory => List.unmodifiable(_eventHistory);

  void dispatch(AppEvent event) {
    _eventHistory.add(event);
    if (_eventHistory.length > 50) {
      _eventHistory.removeAt(0);
    }
    
    _eventController.add(event);
    _handleEvent(event);
  }

  void _handleEvent(AppEvent event) {
    if (event is UserLoginEvent) {
      _state = _state.copyWith(
        isLoggedIn: true,
        username: event.username,
      );
    } else if (event is UserLogoutEvent) {
      _state = _state.copyWith(
        isLoggedIn: false,
        username: '',
        items: [],
      );
    } else if (event is AddItemEvent) {
      final newItems = List<String>.from(_state.items);
      if (!newItems.contains(event.item)) {
        newItems.add(event.item);
        _state = _state.copyWith(items: newItems);
      }
    } else if (event is RemoveItemEvent) {
      final newItems = List<String>.from(_state.items);
      newItems.remove(event.item);
      _state = _state.copyWith(items: newItems);
    }

    notifyListeners();
  }

  @override
  void dispose() {
    _eventController.close();
    super.dispose();
  }
}

class EventDrivenPage extends StatefulWidget {
  const EventDrivenPage({super.key});

  @override
  State<EventDrivenPage> createState() => _EventDrivenPageState();
}

class _EventDrivenPageState extends State<EventDrivenPage> {
  late EventBus _eventBus;
  late StreamSubscription _eventSubscription;
  final TextEditingController _itemController = TextEditingController();

  @override
  void initState() {
    super.initState();
    _eventBus = EventBus();
    _eventBus.addListener(_onStateChanged);
    _eventSubscription = _eventBus.eventStream.listen(_onEvent);
  }

  @override
  void dispose() {
    _eventBus.removeListener(_onStateChanged);
    _eventSubscription.cancel();
    _eventBus.dispose();
    _itemController.dispose();
    super.dispose();
  }

  void _onStateChanged() {
    setState(() {});
  }

  void _onEvent(AppEvent event) {
    String message = '';
    if (event is UserLoginEvent) {
      message = 'User ${event.username} logged in';
    } else if (event is UserLogoutEvent) {
      message = 'User logged out';
    } else if (event is AddItemEvent) {
      message = 'Item "${event.item}" added';
    } else if (event is RemoveItemEvent) {
      message = 'Item "${event.item}" removed';
    }

    if (message.isNotEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text(message), duration: const Duration(seconds: 1)),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    final state = _eventBus.state;

    return Scaffold(
      appBar: AppBar(
        title: const Text('Event-Driven State'),
        actions: [
          IconButton(
            icon: const Icon(Icons.history),
            onPressed: _showEventHistory,
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            // Login Section
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    if (!state.isLoggedIn) ...[
                      const Text('Please log in'),
                      const SizedBox(height: 10),
                      ElevatedButton(
                        onPressed: () {
                          _eventBus.dispatch(const UserLoginEvent('John Doe'));
                        },
                        child: const Text('Login'),
                      ),
                    ] else ...[
                      Text('Welcome, ${state.username}!'),
                      const SizedBox(height: 10),
                      ElevatedButton(
                        onPressed: () {
                          _eventBus.dispatch(const UserLogoutEvent());
                        },
                        child: const Text('Logout'),
                      ),
                    ],
                  ],
                ),
              ),
            ),

            const SizedBox(height: 16),

            // Items Section
            if (state.isLoggedIn) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    children: [
                      Row(
                        children: [
                          Expanded(
                            child: TextField(
                              controller: _itemController,
                              decoration: const InputDecoration(
                                labelText: 'Add item',
                                border: OutlineInputBorder(),
                              ),
                            ),
                          ),
                          const SizedBox(width: 8),
                          ElevatedButton(
                            onPressed: () {
                              if (_itemController.text.isNotEmpty) {
                                _eventBus.dispatch(AddItemEvent(_itemController.text));
                                _itemController.clear();
                              }
                            },
                            child: const Text('Add'),
                          ),
                        ],
                      ),
                    ],
                  ),
                ),
              ),

              const SizedBox(height: 16),

              // Items List
              Expanded(
                child: Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          'Items (${state.items.length})',
                          style: Theme.of(context).textTheme.titleMedium,
                        ),
                        const SizedBox(height: 8),
                        if (state.items.isEmpty)
                          const Text('No items yet')
                        else
                          Expanded(
                            child: ListView.builder(
                              itemCount: state.items.length,
                              itemBuilder: (context, index) {
                                final item = state.items[index];
                                return ListTile(
                                  title: Text(item),
                                  trailing: IconButton(
                                    icon: const Icon(Icons.delete),
                                    onPressed: () {
                                      _eventBus.dispatch(RemoveItemEvent(item));
                                    },
                                  ),
                                );
                              },
                            ),
                          ),
                      ],
                    ),
                  ),
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }

  void _showEventHistory() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Event History'),
        content: SizedBox(
          width: double.maxFinite,
          height: 400,
          child: ListView.builder(
            reverse: true,
            itemCount: _eventBus.eventHistory.length,
            itemBuilder: (context, index) {
              final event = _eventBus.eventHistory[
                  _eventBus.eventHistory.length - 1 - index];
              return ListTile(
                leading: Text('${index + 1}.'),
                title: Text(event.runtimeType.toString()),
                subtitle: _getEventDescription(event),
              );
            },
          ),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }

  Widget? _getEventDescription(AppEvent event) {
    if (event is UserLoginEvent) {
      return Text('Username: ${event.username}');
    } else if (event is AddItemEvent) {
      return Text('Item: ${event.item}');
    } else if (event is RemoveItemEvent) {
      return Text('Item: ${event.item}');
    }
    return null;
  }
}
```

Event-driven state management decouples state changes from UI components  
through event dispatching. Events represent user actions or system changes,  
while handlers modify state in response. This pattern provides excellent  
testability, clear audit trails, and supports complex state interactions  
through event composition and middleware.

## MVC Pattern

Implementing Model-View-Controller architecture for state separation.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'MVC Pattern',
      home: const MVCPage(),
    );
  }
}

// Model
class CalculatorModel extends ChangeNotifier {
  double _currentValue = 0.0;
  double _previousValue = 0.0;
  String _operation = '';
  bool _isNewEntry = true;
  List<String> _history = [];

  double get currentValue => _currentValue;
  double get previousValue => _previousValue;
  String get operation => _operation;
  bool get isNewEntry => _isNewEntry;
  List<String> get history => List.unmodifiable(_history);

  void setCurrentValue(double value) {
    _currentValue = value;
    _isNewEntry = false;
    notifyListeners();
  }

  void setPreviousValue(double value) {
    _previousValue = value;
    notifyListeners();
  }

  void setOperation(String op) {
    _operation = op;
    notifyListeners();
  }

  void setNewEntry(bool isNew) {
    _isNewEntry = isNew;
    notifyListeners();
  }

  void addToHistory(String entry) {
    _history.add(entry);
    if (_history.length > 20) {
      _history.removeAt(0);
    }
    notifyListeners();
  }

  void clearHistory() {
    _history.clear();
    notifyListeners();
  }

  void reset() {
    _currentValue = 0.0;
    _previousValue = 0.0;
    _operation = '';
    _isNewEntry = true;
    notifyListeners();
  }
}

// Controller
class CalculatorController {
  final CalculatorModel _model;

  CalculatorController(this._model);

  void inputNumber(String number) {
    if (_model.isNewEntry) {
      _model.setCurrentValue(double.tryParse(number) ?? 0.0);
    } else {
      final currentStr = _model.currentValue.toString();
      final newStr = currentStr == '0.0' ? number : currentStr + number;
      _model.setCurrentValue(double.tryParse(newStr) ?? 0.0);
    }
  }

  void inputDecimal() {
    if (_model.isNewEntry) {
      _model.setCurrentValue(0.0);
    }
    
    final currentStr = _model.currentValue.toString();
    if (!currentStr.contains('.')) {
      _model.setCurrentValue(double.parse('$currentStr.'));
    }
  }

  void inputOperation(String operation) {
    if (_model.operation.isNotEmpty && !_model.isNewEntry) {
      calculate();
    }
    
    _model.setPreviousValue(_model.currentValue);
    _model.setOperation(operation);
    _model.setNewEntry(true);
  }

  void calculate() {
    if (_model.operation.isEmpty) return;

    double result = 0.0;
    final prev = _model.previousValue;
    final current = _model.currentValue;

    switch (_model.operation) {
      case '+':
        result = prev + current;
        break;
      case '-':
        result = prev - current;
        break;
      case '×':
        result = prev * current;
        break;
      case '÷':
        if (current != 0) {
          result = prev / current;
        } else {
          return; // Division by zero
        }
        break;
    }

    // Add to history
    final historyEntry = '$prev ${_model.operation} $current = $result';
    _model.addToHistory(historyEntry);

    _model.setCurrentValue(result);
    _model.setOperation('');
    _model.setNewEntry(true);
  }

  void clear() {
    _model.reset();
  }

  void clearEntry() {
    _model.setCurrentValue(0.0);
    _model.setNewEntry(true);
  }

  void backspace() {
    if (_model.isNewEntry) return;

    final currentStr = _model.currentValue.toString();
    if (currentStr.length > 1) {
      final newStr = currentStr.substring(0, currentStr.length - 1);
      _model.setCurrentValue(double.tryParse(newStr) ?? 0.0);
    } else {
      _model.setCurrentValue(0.0);
      _model.setNewEntry(true);
    }
  }

  void clearHistory() {
    _model.clearHistory();
  }
}

// View
class MVCPage extends StatefulWidget {
  const MVCPage({super.key});

  @override
  State<MVCPage> createState() => _MVCPageState();
}

class _MVCPageState extends State<MVCPage> {
  late CalculatorModel _model;
  late CalculatorController _controller;

  @override
  void initState() {
    super.initState();
    _model = CalculatorModel();
    _controller = CalculatorController(_model);
    _model.addListener(_onModelChanged);
  }

  @override
  void dispose() {
    _model.removeListener(_onModelChanged);
    _model.dispose();
    super.dispose();
  }

  void _onModelChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('MVC Pattern - Calculator'),
        actions: [
          IconButton(
            icon: const Icon(Icons.history),
            onPressed: _showHistory,
          ),
        ],
      ),
      body: Column(
        children: [
          // Display
          Container(
            width: double.infinity,
            padding: const EdgeInsets.all(24),
            color: Colors.grey[900],
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.end,
              children: [
                if (_model.operation.isNotEmpty)
                  Text(
                    '${_model.previousValue} ${_model.operation}',
                    style: const TextStyle(fontSize: 18, color: Colors.grey),
                  ),
                Text(
                  _formatNumber(_model.currentValue),
                  style: const TextStyle(
                    fontSize: 36,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ],
            ),
          ),

          // Buttons
          Expanded(
            child: GridView.count(
              crossAxisCount: 4,
              padding: const EdgeInsets.all(8),
              crossAxisSpacing: 8,
              mainAxisSpacing: 8,
              children: [
                _buildButton('C', () => _controller.clear(), Colors.redAccent),
                _buildButton('CE', () => _controller.clearEntry(), Colors.orangeAccent),
                _buildButton('⌫', () => _controller.backspace(), Colors.orangeAccent),
                _buildButton('÷', () => _controller.inputOperation('÷'), Colors.blueAccent),

                _buildButton('7', () => _controller.inputNumber('7')),
                _buildButton('8', () => _controller.inputNumber('8')),
                _buildButton('9', () => _controller.inputNumber('9')),
                _buildButton('×', () => _controller.inputOperation('×'), Colors.blueAccent),

                _buildButton('4', () => _controller.inputNumber('4')),
                _buildButton('5', () => _controller.inputNumber('5')),
                _buildButton('6', () => _controller.inputNumber('6')),
                _buildButton('-', () => _controller.inputOperation('-'), Colors.blueAccent),

                _buildButton('1', () => _controller.inputNumber('1')),
                _buildButton('2', () => _controller.inputNumber('2')),
                _buildButton('3', () => _controller.inputNumber('3')),
                _buildButton('+', () => _controller.inputOperation('+'), Colors.blueAccent),

                _buildButton('0', () => _controller.inputNumber('0')),
                _buildButton('.', () => _controller.inputDecimal()),
                const SizedBox(),
                _buildButton('=', () => _controller.calculate(), Colors.greenAccent),
              ],
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildButton(String text, [VoidCallback? onPressed, Color? color]) {
    return ElevatedButton(
      onPressed: onPressed,
      style: ElevatedButton.styleFrom(
        backgroundColor: color,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(8),
        ),
      ),
      child: Text(
        text,
        style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
      ),
    );
  }

  String _formatNumber(double number) {
    if (number == number.toInt()) {
      return number.toInt().toString();
    }
    return number.toString();
  }

  void _showHistory() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Calculation History'),
        content: SizedBox(
          width: double.maxFinite,
          height: 400,
          child: _model.history.isEmpty
              ? const Center(child: Text('No calculations yet'))
              : ListView.builder(
                  reverse: true,
                  itemCount: _model.history.length,
                  itemBuilder: (context, index) {
                    final entry = _model.history[
                        _model.history.length - 1 - index];
                    return ListTile(
                      title: Text(
                        entry,
                        style: const TextStyle(fontFamily: 'monospace'),
                      ),
                    );
                  },
                ),
        ),
        actions: [
          TextButton(
            onPressed: () {
              _controller.clearHistory();
              Navigator.pop(context);
            },
            child: const Text('Clear History'),
          ),
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }
}
```

MVC pattern separates concerns into Model (data), View (UI), and Controller  
(business logic). The Model manages state and notifies observers of changes.  
The Controller handles user input and updates the Model. The View displays  
data and delegates user interactions to the Controller. This separation  
improves testability, maintainability, and code organization.

## Redux Pattern

Implementing Redux-style unidirectional data flow.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Redux Pattern',
      home: const ReduxPage(),
    );
  }
}

// Actions
abstract class Action {
  const Action();
}

class IncrementAction extends Action {
  const IncrementAction();
}

class DecrementAction extends Action {
  const DecrementAction();
}

class ResetAction extends Action {
  const ResetAction();
}

class SetCounterAction extends Action {
  final int value;
  const SetCounterAction(this.value);
}

// State
class AppState {
  final int counter;
  final DateTime lastUpdated;
  final List<String> actionHistory;

  const AppState({
    this.counter = 0,
    DateTime? lastUpdated,
    this.actionHistory = const [],
  }) : lastUpdated = lastUpdated ?? const DateTime.fromMillisecondsSinceEpoch(0);

  AppState copyWith({
    int? counter,
    DateTime? lastUpdated,
    List<String>? actionHistory,
  }) {
    return AppState(
      counter: counter ?? this.counter,
      lastUpdated: lastUpdated ?? DateTime.now(),
      actionHistory: actionHistory ?? this.actionHistory,
    );
  }
}

// Reducer
AppState appReducer(AppState state, Action action) {
  final newHistory = List<String>.from(state.actionHistory);
  newHistory.add('${DateTime.now().millisecondsSinceEpoch}: ${action.runtimeType}');
  if (newHistory.length > 20) {
    newHistory.removeAt(0);
  }

  switch (action.runtimeType) {
    case IncrementAction:
      return state.copyWith(
        counter: state.counter + 1,
        actionHistory: newHistory,
      );
    case DecrementAction:
      return state.copyWith(
        counter: state.counter - 1,
        actionHistory: newHistory,
      );
    case ResetAction:
      return state.copyWith(
        counter: 0,
        actionHistory: newHistory,
      );
    case SetCounterAction:
      final setValue = action as SetCounterAction;
      return state.copyWith(
        counter: setValue.value,
        actionHistory: newHistory,
      );
    default:
      return state;
  }
}

// Store
class Store extends ChangeNotifier {
  AppState _state = const AppState();
  
  AppState get state => _state;

  void dispatch(Action action) {
    final newState = appReducer(_state, action);
    _state = newState;
    notifyListeners();
  }
}

class ReduxPage extends StatefulWidget {
  const ReduxPage({super.key});

  @override
  State<ReduxPage> createState() => _ReduxPageState();
}

class _ReduxPageState extends State<ReduxPage> {
  late Store _store;
  final TextEditingController _valueController = TextEditingController();

  @override
  void initState() {
    super.initState();
    _store = Store();
    _store.addListener(_onStoreChanged);
  }

  @override
  void dispose() {
    _store.removeListener(_onStoreChanged);
    _store.dispose();
    _valueController.dispose();
    super.dispose();
  }

  void _onStoreChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    final state = _store.state;

    return Scaffold(
      appBar: AppBar(
        title: const Text('Redux Pattern'),
        actions: [
          IconButton(
            icon: const Icon(Icons.history),
            onPressed: _showActionHistory,
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(24),
                child: Column(
                  children: [
                    const Text(
                      'Counter Value',
                      style: TextStyle(fontSize: 18),
                    ),
                    const SizedBox(height: 10),
                    Text(
                      '${state.counter}',
                      style: const TextStyle(
                        fontSize: 48,
                        fontWeight: FontWeight.bold,
                        color: Colors.blueAccent,
                      ),
                    ),
                    const SizedBox(height: 10),
                    Text(
                      'Last updated: ${_formatTime(state.lastUpdated)}',
                      style: const TextStyle(fontSize: 12, color: Colors.grey),
                    ),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 20),

            // Action Buttons
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () => _store.dispatch(const DecrementAction()),
                  child: const Text('- Decrement'),
                ),
                const SizedBox(width: 20),
                ElevatedButton(
                  onPressed: () => _store.dispatch(const IncrementAction()),
                  child: const Text('+ Increment'),
                ),
              ],
            ),

            const SizedBox(height: 10),

            ElevatedButton(
              onPressed: () => _store.dispatch(const ResetAction()),
              style: ElevatedButton.styleFrom(
                backgroundColor: Colors.redAccent,
              ),
              child: const Text('Reset'),
            ),

            const SizedBox(height: 20),

            // Custom Value Input
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    const Text('Set Custom Value'),
                    const SizedBox(height: 10),
                    Row(
                      children: [
                        Expanded(
                          child: TextField(
                            controller: _valueController,
                            decoration: const InputDecoration(
                              labelText: 'Enter value',
                              border: OutlineInputBorder(),
                            ),
                            keyboardType: TextInputType.number,
                          ),
                        ),
                        const SizedBox(width: 10),
                        ElevatedButton(
                          onPressed: () {
                            final value = int.tryParse(_valueController.text);
                            if (value != null) {
                              _store.dispatch(SetCounterAction(value));
                              _valueController.clear();
                            }
                          },
                          child: const Text('Set'),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),

            const SizedBox(height: 20),

            // Stats
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceAround,
                  children: [
                    Column(
                      children: [
                        Text(
                          '${state.actionHistory.length}',
                          style: const TextStyle(
                            fontSize: 24,
                            fontWeight: FontWeight.bold,
                            color: Colors.greenAccent,
                          ),
                        ),
                        const Text('Actions'),
                      ],
                    ),
                    Column(
                      children: [
                        Text(
                          state.counter.isNegative ? 'Negative' : 
                          state.counter == 0 ? 'Zero' : 'Positive',
                          style: TextStyle(
                            fontSize: 16,
                            fontWeight: FontWeight.bold,
                            color: state.counter.isNegative 
                                ? Colors.redAccent 
                                : state.counter == 0 
                                    ? Colors.grey 
                                    : Colors.greenAccent,
                          ),
                        ),
                        const Text('Sign'),
                      ],
                    ),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  String _formatTime(DateTime time) {
    return '${time.hour.toString().padLeft(2, '0')}:'
           '${time.minute.toString().padLeft(2, '0')}:'
           '${time.second.toString().padLeft(2, '0')}';
  }

  void _showActionHistory() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Action History'),
        content: SizedBox(
          width: double.maxFinite,
          height: 400,
          child: _store.state.actionHistory.isEmpty
              ? const Center(child: Text('No actions yet'))
              : ListView.builder(
                  reverse: true,
                  itemCount: _store.state.actionHistory.length,
                  itemBuilder: (context, index) {
                    final action = _store.state.actionHistory[
                        _store.state.actionHistory.length - 1 - index];
                    return ListTile(
                      leading: Text('${index + 1}.'),
                      title: Text(
                        action,
                        style: const TextStyle(fontFamily: 'monospace', fontSize: 12),
                      ),
                    );
                  },
                ),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Close'),
          ),
        ],
      ),
    );
  }
}
```

Redux pattern implements unidirectional data flow with Actions, Reducers,  
and Store. Actions describe what happened, Reducers specify how state  
changes, and Store holds application state. This architecture provides  
predictable state updates, excellent debugging capabilities, and supports  
time-travel debugging through action replay.

## MVVM Pattern

Implementing Model-View-ViewModel architecture with data binding.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'MVVM Pattern',
      home: const MVVMPage(),
    );
  }
}

// Model
class User {
  final String id;
  final String name;
  final String email;
  final int age;

  const User({
    required this.id,
    required this.name,
    required this.email,
    required this.age,
  });

  User copyWith({
    String? id,
    String? name,
    String? email,
    int? age,
  }) {
    return User(
      id: id ?? this.id,
      name: name ?? this.name,
      email: email ?? this.email,
      age: age ?? this.age,
    );
  }
}

class UserRepository {
  final List<User> _users = [
    const User(id: '1', name: 'Alice Johnson', email: 'alice@example.com', age: 28),
    const User(id: '2', name: 'Bob Smith', email: 'bob@example.com', age: 35),
    const User(id: '3', name: 'Charlie Brown', email: 'charlie@example.com', age: 22),
  ];

  Future<List<User>> getUsers() async {
    await Future.delayed(const Duration(seconds: 1));
    return List.from(_users);
  }

  Future<User> createUser(User user) async {
    await Future.delayed(const Duration(seconds: 1));
    _users.add(user);
    return user;
  }

  Future<User> updateUser(User user) async {
    await Future.delayed(const Duration(seconds: 1));
    final index = _users.indexWhere((u) => u.id == user.id);
    if (index != -1) {
      _users[index] = user;
    }
    return user;
  }

  Future<void> deleteUser(String id) async {
    await Future.delayed(const Duration(seconds: 1));
    _users.removeWhere((u) => u.id == id);
  }
}

// ViewModel
class UserViewModel extends ChangeNotifier {
  final UserRepository _repository = UserRepository();
  
  List<User> _users = [];
  bool _isLoading = false;
  String? _error;
  String _searchQuery = '';

  // Getters
  List<User> get users => _searchQuery.isEmpty
      ? _users
      : _users.where((user) => 
          user.name.toLowerCase().contains(_searchQuery.toLowerCase()) ||
          user.email.toLowerCase().contains(_searchQuery.toLowerCase())
        ).toList();
  
  bool get isLoading => _isLoading;
  String? get error => _error;
  String get searchQuery => _searchQuery;
  int get totalUsers => _users.length;
  int get filteredUsersCount => users.length;
  double get averageAge => _users.isEmpty 
      ? 0.0 
      : _users.fold(0, (sum, user) => sum + user.age) / _users.length;

  // Commands
  Future<void> loadUsers() async {
    _setLoading(true);
    _setError(null);
    
    try {
      _users = await _repository.getUsers();
    } catch (e) {
      _setError('Failed to load users: $e');
    } finally {
      _setLoading(false);
    }
  }

  Future<void> createUser({
    required String name,
    required String email,
    required int age,
  }) async {
    if (name.isEmpty || email.isEmpty || age <= 0) {
      _setError('Please provide valid user information');
      return;
    }

    _setLoading(true);
    _setError(null);

    try {
      final user = User(
        id: DateTime.now().millisecondsSinceEpoch.toString(),
        name: name,
        email: email,
        age: age,
      );
      
      await _repository.createUser(user);
      await loadUsers(); // Refresh list
    } catch (e) {
      _setError('Failed to create user: $e');
    } finally {
      _setLoading(false);
    }
  }

  Future<void> updateUser(User user) async {
    _setLoading(true);
    _setError(null);

    try {
      await _repository.updateUser(user);
      await loadUsers(); // Refresh list
    } catch (e) {
      _setError('Failed to update user: $e');
    } finally {
      _setLoading(false);
    }
  }

  Future<void> deleteUser(String id) async {
    _setLoading(true);
    _setError(null);

    try {
      await _repository.deleteUser(id);
      await loadUsers(); // Refresh list
    } catch (e) {
      _setError('Failed to delete user: $e');
    } finally {
      _setLoading(false);
    }
  }

  void setSearchQuery(String query) {
    _searchQuery = query;
    notifyListeners();
  }

  void clearError() {
    _setError(null);
  }

  // Private methods
  void _setLoading(bool loading) {
    _isLoading = loading;
    notifyListeners();
  }

  void _setError(String? error) {
    _error = error;
    notifyListeners();
  }
}

// View
class MVVMPage extends StatefulWidget {
  const MVVMPage({super.key});

  @override
  State<MVVMPage> createState() => _MVVMPageState();
}

class _MVVMPageState extends State<MVVMPage> {
  late UserViewModel _viewModel;
  final TextEditingController _searchController = TextEditingController();

  @override
  void initState() {
    super.initState();
    _viewModel = UserViewModel();
    _viewModel.addListener(_onViewModelChanged);
    _viewModel.loadUsers();
  }

  @override
  void dispose() {
    _viewModel.removeListener(_onViewModelChanged);
    _viewModel.dispose();
    _searchController.dispose();
    super.dispose();
  }

  void _onViewModelChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('MVVM Pattern - Users'),
        actions: [
          IconButton(
            icon: const Icon(Icons.add),
            onPressed: _showCreateUserDialog,
          ),
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: _viewModel.loadUsers,
          ),
        ],
      ),
      body: Column(
        children: [
          // Error Display
          if (_viewModel.error != null)
            Container(
              width: double.infinity,
              padding: const EdgeInsets.all(12),
              margin: const EdgeInsets.all(16),
              decoration: BoxDecoration(
                color: Colors.red.withOpacity(0.1),
                border: Border.all(color: Colors.red),
                borderRadius: BorderRadius.circular(4),
              ),
              child: Row(
                children: [
                  const Icon(Icons.error, color: Colors.red),
                  const SizedBox(width: 8),
                  Expanded(child: Text(_viewModel.error!)),
                  TextButton(
                    onPressed: _viewModel.clearError,
                    child: const Text('Dismiss'),
                  ),
                ],
              ),
            ),

          // Search Bar
          Padding(
            padding: const EdgeInsets.all(16),
            child: TextField(
              controller: _searchController,
              decoration: const InputDecoration(
                labelText: 'Search users...',
                prefixIcon: Icon(Icons.search),
                border: OutlineInputBorder(),
              ),
              onChanged: _viewModel.setSearchQuery,
            ),
          ),

          // Stats Bar
          Container(
            width: double.infinity,
            padding: const EdgeInsets.all(16),
            color: Colors.grey[900],
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: [
                _buildStatItem('Total', _viewModel.totalUsers.toString()),
                _buildStatItem('Filtered', _viewModel.filteredUsersCount.toString()),
                _buildStatItem('Avg Age', _viewModel.averageAge.toStringAsFixed(1)),
              ],
            ),
          ),

          // User List
          Expanded(
            child: _viewModel.isLoading
                ? const Center(child: CircularProgressIndicator())
                : _viewModel.users.isEmpty
                    ? const Center(
                        child: Text(
                          'No users found',
                          style: TextStyle(fontSize: 16),
                        ),
                      )
                    : ListView.builder(
                        itemCount: _viewModel.users.length,
                        itemBuilder: (context, index) {
                          final user = _viewModel.users[index];
                          return Card(
                            margin: const EdgeInsets.symmetric(
                              horizontal: 16,
                              vertical: 4,
                            ),
                            child: ListTile(
                              leading: CircleAvatar(
                                child: Text(user.name.substring(0, 2)),
                              ),
                              title: Text(user.name),
                              subtitle: Text('${user.email} • Age: ${user.age}'),
                              trailing: PopupMenuButton(
                                itemBuilder: (context) => [
                                  PopupMenuItem(
                                    child: const Text('Edit'),
                                    onTap: () => _showEditUserDialog(user),
                                  ),
                                  PopupMenuItem(
                                    child: const Text('Delete'),
                                    onTap: () => _confirmDeleteUser(user),
                                  ),
                                ],
                              ),
                            ),
                          );
                        },
                      ),
          ),
        ],
      ),
    );
  }

  Widget _buildStatItem(String label, String value) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      children: [
        Text(
          value,
          style: const TextStyle(
            fontSize: 18,
            fontWeight: FontWeight.bold,
            color: Colors.blueAccent,
          ),
        ),
        Text(label, style: const TextStyle(fontSize: 12)),
      ],
    );
  }

  void _showCreateUserDialog() {
    final nameController = TextEditingController();
    final emailController = TextEditingController();
    final ageController = TextEditingController();

    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Create User'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            TextField(
              controller: nameController,
              decoration: const InputDecoration(labelText: 'Name'),
            ),
            TextField(
              controller: emailController,
              decoration: const InputDecoration(labelText: 'Email'),
            ),
            TextField(
              controller: ageController,
              decoration: const InputDecoration(labelText: 'Age'),
              keyboardType: TextInputType.number,
            ),
          ],
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () {
              final age = int.tryParse(ageController.text) ?? 0;
              _viewModel.createUser(
                name: nameController.text,
                email: emailController.text,
                age: age,
              );
              Navigator.pop(context);
            },
            child: const Text('Create'),
          ),
        ],
      ),
    );
  }

  void _showEditUserDialog(User user) {
    final nameController = TextEditingController(text: user.name);
    final emailController = TextEditingController(text: user.email);
    final ageController = TextEditingController(text: user.age.toString());

    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Edit User'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            TextField(
              controller: nameController,
              decoration: const InputDecoration(labelText: 'Name'),
            ),
            TextField(
              controller: emailController,
              decoration: const InputDecoration(labelText: 'Email'),
            ),
            TextField(
              controller: ageController,
              decoration: const InputDecoration(labelText: 'Age'),
              keyboardType: TextInputType.number,
            ),
          ],
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () {
              final age = int.tryParse(ageController.text) ?? user.age;
              final updatedUser = user.copyWith(
                name: nameController.text,
                email: emailController.text,
                age: age,
              );
              _viewModel.updateUser(updatedUser);
              Navigator.pop(context);
            },
            child: const Text('Update'),
          ),
        ],
      ),
    );
  }

  void _confirmDeleteUser(User user) {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Delete User'),
        content: Text('Are you sure you want to delete ${user.name}?'),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () {
              _viewModel.deleteUser(user.id);
              Navigator.pop(context);
            },
            style: ElevatedButton.styleFrom(
              backgroundColor: Colors.redAccent,
            ),
            child: const Text('Delete'),
          ),
        ],
      ),
    );
  }
}
```

MVVM pattern separates presentation logic into ViewModels that expose  
data and commands to Views. The ViewModel acts as a binding layer between  
Model and View, handling business logic, data transformation, and state  
management. This architecture provides excellent testability, clear  
separation of concerns, and supports data binding for reactive UIs.    
