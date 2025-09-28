# Flutter-Examples

Flutter code examples demonstrating various concepts and features  
for building mobile, web, and desktop applications.  

## Getting Started

If you're new to Flutter, start with the [Introduction](introduction.md)  
to learn about Flutter's history, installation, and basic concepts.  

## Documentation

- [Introduction](introduction.md) - Flutter overview, history, and installation  
- [Basics](basics.md) - 30 essential Flutter examples for beginners  
- [Layout](layout.md) - Comprehensive layout widgets and techniques  
- [Animation](anim.md) - Animation examples and techniques  

## Examples

### Exit button

Button inside application bar

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Exit Button App',
      theme: ThemeData.dark(),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        leading: IconButton(
          icon: const Icon(Icons.exit_to_app),
          tooltip: 'Exit',
          onPressed: () {
            SystemNavigator.pop();
          },
        ),
        title: const Text('Exit Button App'),
      ),
      body: const Center(
        child: Text('Hello there!'),
      ),
    );
  }
}
```

Exit button positioned absolutely

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Exit Button App',
      theme: ThemeData.dark(),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Exit Button App'),
      ),
      body: Stack(
        children: [
          Positioned(
            top: 10,
            left: 10,
            child: ElevatedButton(
              onPressed: () {
                SystemNavigator.pop();
              },
              child: const Text('Exit App'),
            ),
          ),
        ],
      ),
    );
  }
}
```
