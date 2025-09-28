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

### Notification 

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
      title: 'Notification Button Demo',
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
        title: const Text('Notification Button'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Show SnackBar when button is pressed
            ScaffoldMessenger.of(context).showSnackBar(
              const SnackBar(
                content: Text('Button Clicked!'),
                duration: Duration(seconds: 3),
              ),
            );
          },
          child: const Text('Show Notification'),
        ),
      ),
    );
  }
}
```



### Notification positioned at the top

```flutter
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Notification Button Demo',
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
      appBar: AppBar(title: const Text('Notification Button')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Show SnackBar at the top when button is pressed
            ScaffoldMessenger.of(context).showSnackBar(
              SnackBar(
                content: const Text('Button Clicked!'),
                duration: const Duration(seconds: 3),
                behavior: SnackBarBehavior.floating,
                margin: EdgeInsets.only(
                  top: MediaQuery.of(context).padding.top + 10.0,
                  left: 10.0,
                  right: 10.0,
                  bottom: MediaQuery.of(context).size.height - 
                         MediaQuery.of(context).padding.top - 70.0,
                ),
              ),
            );
          },
          child: const Text('Show Notification'),
        ),
      ),
    );
  }
}
```

### Other notifications

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
      title: 'Notification Demo',
      theme: ThemeData.dark(),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key});

  void _showSnackBar(BuildContext context) {
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: const Text('Top SnackBar!'),
        duration: const Duration(seconds: 3),
        behavior: SnackBarBehavior.floating,
        margin: const EdgeInsets.only(top: 10.0, left: 10.0, right: 10.0),
      ),
    );
  }

  void _showAlertDialog(BuildContext context) {
    showDialog(
      context: context,
      builder: (BuildContext context) {
        return AlertDialog(
          title: const Text('Alert'),
          content: const Text('This is an alert dialog.'),
          actions: [
            TextButton(
              child: const Text('OK'),
              onPressed: () => Navigator.of(context).pop(),
            ),
          ],
        );
      },
    );
  }

  void _showBottomSheet(BuildContext context) {
    showModalBottomSheet(
      context: context,
      builder: (BuildContext context) {
        return Container(
          height: 200,
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              const Text('This is a BottomSheet!'),
              ElevatedButton(
                child: const Text('Close'),
                onPressed: () => Navigator.of(context).pop(),
              ),
            ],
          ),
        );
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Notification Types'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () => _showSnackBar(context),
              child: const Text('Show SnackBar'),
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              onPressed: () => _showAlertDialog(context),
              child: const Text('Show AlertDialog'),
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              onPressed: () => _showBottomSheet(context),
              child: const Text('Show BottomSheet'),
            ),
          ],
        ),
      ),
    );
  }
}
```


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
