# Flutter-Examples
Flutter code examples



## Exit button

Button inside application bar

```flutter
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
