# Flutter Basic Widgets

Flutter provides a comprehensive set of basic widgets that are essential for  
building any Flutter application. These widgets form the foundation of the  
Flutter framework and understanding them is crucial before creating your first  
Flutter app. This guide covers the 11 fundamental widgets with two practical  
examples for each, progressing from simple to more complex implementations.  

Each widget example demonstrates common use cases and best practices,  
helping you understand how to effectively use these building blocks in your  
Flutter applications. The examples use dark theme and follow current Flutter  
conventions with null safety and modern widget constructors.  

## Container

Container is a convenience widget that combines common painting, positioning,  
and sizing widgets. It's one of the most versatile widgets in Flutter.  

### Basic Container

A simple container with background color, padding, and margin.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Basic Container'),
        ),
        body: Center(
          child: Container(
            width: 200,
            height: 100,
            padding: const EdgeInsets.all(16),
            margin: const EdgeInsets.all(20),
            color: Colors.blue,
            child: const Text(
              'Hello Container!',
              style: TextStyle(color: Colors.white),
              textAlign: TextAlign.center,
            ),
          ),
        ),
      ),
    );
  }
}
```

This example creates a basic container with fixed dimensions, padding, margin,  
and background color. The container centers its child text widget and applies  
white text color for contrast against the blue background.  

### Decorated Container

A container with border, border radius, and gradient background.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Decorated Container'),
        ),
        body: Center(
          child: Container(
            width: 250,
            height: 120,
            padding: const EdgeInsets.all(20),
            decoration: BoxDecoration(
              gradient: const LinearGradient(
                begin: Alignment.topLeft,
                end: Alignment.bottomRight,
                colors: [Colors.purple, Colors.blue],
              ),
              borderRadius: BorderRadius.circular(15),
              border: Border.all(color: Colors.white, width: 2),
              boxShadow: const [
                BoxShadow(
                  color: Colors.black26,
                  blurRadius: 10,
                  offset: Offset(0, 4),
                ),
              ],
            ),
            child: const Text(
              'Styled Container',
              style: TextStyle(
                fontSize: 18,
                fontWeight: FontWeight.bold,
                color: Colors.white,
              ),
              textAlign: TextAlign.center,
            ),
          ),
        ),
      ),
    );
  }
}
```

This example demonstrates advanced container styling with gradient background,  
rounded corners, border, and shadow. The BoxDecoration provides rich styling  
options including gradients, shadows, and custom borders.  

## Row

Row widget arranges its children horizontally. It's essential for creating  
horizontal layouts and distributing space between widgets.  

### Simple Row

A basic row with evenly spaced text widgets.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Simple Row'),
        ),
        body: const Center(
          child: Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: [
              Text('First', style: TextStyle(fontSize: 18)),
              Text('Second', style: TextStyle(fontSize: 18)),
              Text('Third', style: TextStyle(fontSize: 18)),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example shows a basic row with three text widgets evenly distributed  
using spaceEvenly alignment. The Row widget automatically arranges children  
horizontally and the mainAxisAlignment property controls spacing.  

### Row with Mixed Widgets

A row containing different types of widgets with flexible spacing.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Mixed Row Widgets'),
        ),
        body: Padding(
          padding: const EdgeInsets.all(20),
          child: Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            crossAxisAlignment: CrossAxisAlignment.center,
            children: [
              const Icon(Icons.star, color: Colors.yellow, size: 30),
              Expanded(
                child: Container(
                  padding: const EdgeInsets.symmetric(horizontal: 16),
                  child: const Text(
                    'Flexible Text',
                    textAlign: TextAlign.center,
                    style: TextStyle(fontSize: 16),
                  ),
                ),
              ),
              ElevatedButton(
                onPressed: () {},
                child: const Text('Action'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example combines an icon, expanded text, and button in a row. The Expanded  
widget allows the text to take available space, while other widgets maintain  
their intrinsic size. Cross-axis alignment centers all widgets vertically.  

## Column

Column widget arranges its children vertically. It's the vertical counterpart  
to Row and essential for creating vertical layouts.  

### Basic Column

A simple column with center-aligned text widgets.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Basic Column'),
        ),
        body: const Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.center,
            children: [
              Text('Top Text', style: TextStyle(fontSize: 20)),
              SizedBox(height: 20),
              Text('Middle Text', style: TextStyle(fontSize: 18)),
              SizedBox(height: 20),
              Text('Bottom Text', style: TextStyle(fontSize: 16)),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example demonstrates a basic column with three text widgets vertically  
centered. SizedBox widgets provide consistent spacing between elements.  
The mainAxisAlignment centers the column content vertically on the screen.  

### Column with Card Layout

A column containing cards with different content types.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Column with Cards'),
        ),
        body: Padding(
          padding: const EdgeInsets.all(16),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Row(
                    children: [
                      const Icon(Icons.person, size: 30),
                      const SizedBox(width: 16),
                      const Expanded(
                        child: Text(
                          'User Profile',
                          style: TextStyle(fontSize: 18),
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
                  child: Row(
                    children: [
                      const Icon(Icons.settings, size: 30),
                      const SizedBox(width: 16),
                      const Expanded(
                        child: Text(
                          'Settings',
                          style: TextStyle(fontSize: 18),
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
                  child: Row(
                    children: [
                      const Icon(Icons.help, size: 30),
                      const SizedBox(width: 16),
                      const Expanded(
                        child: Text(
                          'Help & Support',
                          style: TextStyle(fontSize: 18),
                        ),
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
}
```

This example shows a column with multiple cards, each containing an icon and  
text. The crossAxisAlignment.stretch makes cards fill the available width.  
Cards provide elevation and material design styling for content grouping.  

## Stack

Stack widget allows you to overlay widgets on top of each other. It's perfect  
for creating layered interfaces and positioning widgets absolutely.  

### Basic Stack

A simple stack with overlapping colored containers.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Basic Stack'),
        ),
        body: Center(
          child: Stack(
            children: [
              Container(
                width: 200,
                height: 200,
                color: Colors.red,
              ),
              Container(
                width: 150,
                height: 150,
                color: Colors.green,
              ),
              Container(
                width: 100,
                height: 100,
                color: Colors.blue,
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example creates a basic stack with three overlapping containers of  
different sizes and colors. Stack children are positioned from bottom to top  
in the order they appear in the children list.  

### Positioned Stack

A stack with precisely positioned widgets using Positioned widget.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Positioned Stack'),
        ),
        body: Stack(
          children: [
            Container(
              width: double.infinity,
              height: double.infinity,
              decoration: const BoxDecoration(
                gradient: LinearGradient(
                  begin: Alignment.topLeft,
                  end: Alignment.bottomRight,
                  colors: [Colors.purple, Colors.blue],
                ),
              ),
            ),
            const Positioned(
              top: 50,
              left: 20,
              child: Icon(Icons.star, color: Colors.yellow, size: 40),
            ),
            const Positioned(
              top: 50,
              right: 20,
              child: Icon(Icons.favorite, color: Colors.red, size: 40),
            ),
            Positioned(
              bottom: 50,
              left: 20,
              right: 20,
              child: Container(
                padding: const EdgeInsets.all(16),
                decoration: BoxDecoration(
                  color: Colors.white.withOpacity(0.9),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: const Text(
                  'Overlay Content',
                  style: TextStyle(
                    color: Colors.black,
                    fontSize: 18,
                    fontWeight: FontWeight.bold,
                  ),
                  textAlign: TextAlign.center,
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

This example demonstrates precise positioning with a gradient background,  
icons in corners, and a bottom overlay container. The Positioned widget  
allows exact placement using top, bottom, left, and right properties.  

## Text

Text widget displays a string of text with a single style. It's the most  
fundamental widget for displaying textual information in Flutter apps.  

### Styled Text

A text widget with custom styling and formatting options.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Styled Text'),
        ),
        body: const Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Text(
                'Large Bold Text',
                style: TextStyle(
                  fontSize: 24,
                  fontWeight: FontWeight.bold,
                  color: Colors.blue,
                ),
              ),
              SizedBox(height: 20),
              Text(
                'Italic Text with Shadow',
                style: TextStyle(
                  fontSize: 18,
                  fontStyle: FontStyle.italic,
                  shadows: [
                    Shadow(
                      blurRadius: 3.0,
                      color: Colors.grey,
                      offset: Offset(2.0, 2.0),
                    ),
                  ],
                ),
              ),
              SizedBox(height: 20),
              Text(
                'Underlined Text',
                style: TextStyle(
                  fontSize: 16,
                  decoration: TextDecoration.underline,
                  decorationColor: Colors.red,
                  decorationThickness: 2.0,
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example shows various text styling options including font size, weight,  
color, shadows, and text decorations. Each Text widget demonstrates different  
styling properties available for customizing text appearance.  

### Multiline Text

A text widget displaying multiple lines with text alignment and overflow.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Multiline Text'),
        ),
        body: const Padding(
          padding: EdgeInsets.all(20),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              Text(
                'This is a long text that will wrap to multiple lines when the available width is not sufficient to display it in a single line. Flutter automatically handles text wrapping.',
                style: TextStyle(fontSize: 16, height: 1.5),
                textAlign: TextAlign.justify,
              ),
              SizedBox(height: 30),
              Text(
                'This text has a maximum of 2 lines and will be truncated with ellipsis if it exceeds the limit. This demonstrates text overflow handling.',
                style: TextStyle(fontSize: 16),
                maxLines: 2,
                overflow: TextOverflow.ellipsis,
              ),
              SizedBox(height: 30),
              Text(
                'CENTER ALIGNED TEXT',
                style: TextStyle(
                  fontSize: 18,
                  fontWeight: FontWeight.bold,
                  letterSpacing: 2.0,
                ),
                textAlign: TextAlign.center,
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example demonstrates text wrapping, line limits, overflow handling, and  
text alignment. The first text shows natural wrapping, the second shows  
ellipsis overflow, and the third shows center alignment with letter spacing.  

## RichText

RichText widget allows you to display text with multiple styles within a  
single text widget. It's perfect for complex text formatting needs.  

### Basic RichText

A RichText widget with different styles within the same text block.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Basic RichText'),
        ),
        body: Center(
          child: Padding(
            padding: const EdgeInsets.all(20),
            child: RichText(
              text: const TextSpan(
                style: TextStyle(fontSize: 16, color: Colors.white),
                children: [
                  TextSpan(text: 'This is '),
                  TextSpan(
                    text: 'bold',
                    style: TextStyle(fontWeight: FontWeight.bold),
                  ),
                  TextSpan(text: ' and this is '),
                  TextSpan(
                    text: 'italic',
                    style: TextStyle(fontStyle: FontStyle.italic),
                  ),
                  TextSpan(text: ' and this is '),
                  TextSpan(
                    text: 'colored',
                    style: TextStyle(color: Colors.blue),
                  ),
                  TextSpan(text: ' text.'),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```

This example shows basic RichText usage with multiple TextSpan children,  
each having different styles. The base style is applied to the entire text,  
while individual spans override specific properties like weight and color.  

### Advanced RichText

A complex RichText with interactive spans and various formatting options.  

```dart
import 'package:flutter/gestures.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData.dark(),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  String _clickedText = 'None';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Advanced RichText'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            RichText(
              text: TextSpan(
                style: const TextStyle(fontSize: 16, color: Colors.white),
                children: [
                  const TextSpan(
                    text: 'Welcome to our ',
                  ),
                  TextSpan(
                    text: 'Flutter',
                    style: const TextStyle(
                      color: Colors.blue,
                      fontWeight: FontWeight.bold,
                      fontSize: 18,
                    ),
                  ),
                  const TextSpan(
                    text: ' application. Please read our ',
                  ),
                  TextSpan(
                    text: 'Terms of Service',
                    style: const TextStyle(
                      color: Colors.orange,
                      decoration: TextDecoration.underline,
                    ),
                    recognizer: TapGestureRecognizer()
                      ..onTap = () {
                        setState(() {
                          _clickedText = 'Terms of Service';
                        });
                      },
                  ),
                  const TextSpan(
                    text: ' and ',
                  ),
                  TextSpan(
                    text: 'Privacy Policy',
                    style: const TextStyle(
                      color: Colors.green,
                      decoration: TextDecoration.underline,
                    ),
                    recognizer: TapGestureRecognizer()
                      ..onTap = () {
                        setState(() {
                          _clickedText = 'Privacy Policy';
                        });
                      },
                  ),
                  const TextSpan(
                    text: ' carefully.',
                  ),
                ],
              ),
            ),
            const SizedBox(height: 30),
            Text(
              'Last clicked: $_clickedText',
              style: const TextStyle(
                fontSize: 16,
                fontWeight: FontWeight.bold,
                color: Colors.yellow,
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example shows advanced RichText with clickable spans using  
TapGestureRecognizer. It demonstrates how to create interactive text with  
different colors, styles, and tap handlers for individual text segments.  

## TextField

TextField widget allows users to enter text. It's essential for forms,  
search functionality, and any user input scenarios.  

### Basic TextField

A simple text field with label and basic styling.  

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
      theme: ThemeData.dark(),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  final TextEditingController _controller = TextEditingController();
  String _enteredText = '';

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic TextField'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(
          children: [
            TextField(
              controller: _controller,
              decoration: const InputDecoration(
                labelText: 'Enter your name',
                border: OutlineInputBorder(),
                prefixIcon: Icon(Icons.person),
              ),
              onChanged: (value) {
                setState(() {
                  _enteredText = value;
                });
              },
            ),
            const SizedBox(height: 20),
            Text(
              'You entered: $_enteredText',
              style: const TextStyle(fontSize: 16),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example shows a basic TextField with label, border, prefix icon, and  
real-time text tracking. The TextEditingController manages the field's text  
value, and onChanged callback updates the display text immediately.  

### Advanced TextField

A text field with validation, custom styling, and multiple input types.  

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
      theme: ThemeData.dark(),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  final _formKey = GlobalKey<FormState>();
  final TextEditingController _emailController = TextEditingController();
  final TextEditingController _passwordController = TextEditingController();
  bool _obscurePassword = true;

  @override
  void dispose() {
    _emailController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  String? _validateEmail(String? value) {
    if (value == null || value.isEmpty) {
      return 'Please enter your email';
    }
    if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value)) {
      return 'Please enter a valid email';
    }
    return null;
  }

  String? _validatePassword(String? value) {
    if (value == null || value.isEmpty) {
      return 'Please enter your password';
    }
    if (value.length < 6) {
      return 'Password must be at least 6 characters';
    }
    return null;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Advanced TextField'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Form(
          key: _formKey,
          child: Column(
            children: [
              TextFormField(
                controller: _emailController,
                keyboardType: TextInputType.emailAddress,
                decoration: const InputDecoration(
                  labelText: 'Email Address',
                  hintText: 'Enter your email',
                  border: OutlineInputBorder(),
                  prefixIcon: Icon(Icons.email),
                ),
                validator: _validateEmail,
              ),
              const SizedBox(height: 20),
              TextFormField(
                controller: _passwordController,
                obscureText: _obscurePassword,
                decoration: InputDecoration(
                  labelText: 'Password',
                  hintText: 'Enter your password',
                  border: const OutlineInputBorder(),
                  prefixIcon: const Icon(Icons.lock),
                  suffixIcon: IconButton(
                    icon: Icon(
                      _obscurePassword ? Icons.visibility : Icons.visibility_off,
                    ),
                    onPressed: () {
                      setState(() {
                        _obscurePassword = !_obscurePassword;
                      });
                    },
                  ),
                ),
                validator: _validatePassword,
              ),
              const SizedBox(height: 30),
              ElevatedButton(
                onPressed: () {
                  if (_formKey.currentState!.validate()) {
                    ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('Form is valid!')),
                    );
                  }
                },
                child: const Text('Submit'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example demonstrates advanced TextField usage with form validation,  
different keyboard types, password visibility toggle, and custom validators.  
It shows a complete login form with email and password validation.  

## Image

Image widget displays images from various sources including assets, network,  
files, and memory. It's essential for showing visual content in apps.  

### Network Image

Loading and displaying an image from a network URL.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Network Image'),
        ),
        body: const Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Text(
                'Loading image from network:',
                style: TextStyle(fontSize: 16),
              ),
              SizedBox(height: 20),
              Image(
                image: NetworkImage(
                  'https://picsum.photos/300/200',
                ),
                width: 300,
                height: 200,
                fit: BoxFit.cover,
                loadingBuilder: (context, child, loadingProgress) {
                  if (loadingProgress == null) return child;
                  return SizedBox(
                    width: 300,
                    height: 200,
                    child: Center(
                      child: CircularProgressIndicator(
                        value: loadingProgress.expectedTotalBytes != null
                            ? loadingProgress.cumulativeBytesLoaded /
                                loadingProgress.expectedTotalBytes!
                            : null,
                      ),
                    ),
                  );
                },
                errorBuilder: (context, error, stackTrace) {
                  return Container(
                    width: 300,
                    height: 200,
                    color: Colors.grey[800],
                    child: const Center(
                      child: Column(
                        mainAxisAlignment: MainAxisAlignment.center,
                        children: [
                          Icon(Icons.error, color: Colors.red, size: 40),
                          SizedBox(height: 8),
                          Text('Failed to load image'),
                        ],
                      ),
                    ),
                  );
                },
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example shows how to load a network image with loading progress indicator  
and error handling. The loadingBuilder shows a circular progress indicator  
while loading, and errorBuilder displays an error message if loading fails.  

### Asset Image with Effects

Displaying local asset images with various effects and transformations.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Asset Image Effects'),
        ),
        body: const Padding(
          padding: EdgeInsets.all(20),
          child: Column(
            children: [
              Text(
                'Image with various effects:',
                style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
              ),
              SizedBox(height: 30),
              
              // Circular image with border
              CircleAvatar(
                radius: 60,
                backgroundColor: Colors.blue,
                child: CircleAvatar(
                  radius: 55,
                  backgroundColor: Colors.grey,
                  child: Icon(
                    Icons.person,
                    size: 60,
                    color: Colors.white,
                  ),
                ),
              ),
              
              SizedBox(height: 30),
              
              // Rounded rectangle image
              ClipRRect(
                borderRadius: BorderRadius.circular(15),
                child: Container(
                  width: 200,
                  height: 120,
                  decoration: BoxDecoration(
                    gradient: LinearGradient(
                      begin: Alignment.topLeft,
                      end: Alignment.bottomRight,
                      colors: [Colors.purple, Colors.blue],
                    ),
                  ),
                  child: Center(
                    child: Icon(
                      Icons.image,
                      size: 60,
                      color: Colors.white,
                    ),
                  ),
                ),
              ),
              
              SizedBox(height: 30),
              
              // Image with shadow and effects
              Container(
                decoration: BoxDecoration(
                  borderRadius: BorderRadius.circular(10),
                  boxShadow: [
                    BoxShadow(
                      color: Colors.black26,
                      blurRadius: 10,
                      offset: Offset(0, 5),
                    ),
                  ],
                ),
                child: ClipRRect(
                  borderRadius: BorderRadius.circular(10),
                  child: Container(
                    width: 150,
                    height: 100,
                    color: Colors.orange,
                    child: Center(
                      child: Icon(
                        Icons.photo,
                        size: 40,
                        color: Colors.white,
                      ),
                    ),
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example demonstrates various image effects including circular avatars,  
rounded corners, shadows, and gradient backgrounds. It shows how to style  
images using ClipRRect, Container, and CircleAvatar widgets.  

## Icon

Icon widget displays material design icons. It's perfect for buttons,  
navigation, and visual cues throughout your application.  

### Basic Icons

Displaying various icons with different sizes and colors.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Basic Icons'),
        ),
        body: const Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  Icon(Icons.home, size: 40, color: Colors.blue),
                  Icon(Icons.favorite, size: 40, color: Colors.red),
                  Icon(Icons.star, size: 40, color: Colors.yellow),
                ],
              ),
              SizedBox(height: 40),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  Icon(Icons.settings, size: 50, color: Colors.green),
                  Icon(Icons.person, size: 50, color: Colors.orange),
                  Icon(Icons.notifications, size: 50, color: Colors.purple),
                ],
              ),
              SizedBox(height: 40),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  Icon(Icons.email, size: 35),
                  Icon(Icons.phone, size: 35),
                  Icon(Icons.message, size: 35),
                ],
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example showcases basic icon usage with different sizes and colors.  
Icons are arranged in rows to demonstrate various commonly used material  
design icons with consistent spacing and visual hierarchy.  

### Interactive Icon Buttons

Icons used as interactive buttons with state changes and animations.  

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
      theme: ThemeData.dark(),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  bool _isFavorited = false;
  bool _isBookmarked = false;
  int _rating = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Interactive Icons'),
        actions: [
          IconButton(
            icon: Icon(_isBookmarked ? Icons.bookmark : Icons.bookmark_border),
            onPressed: () {
              setState(() {
                _isBookmarked = !_isBookmarked;
              });
            },
          ),
        ],
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            // Favorite button
            GestureDetector(
              onTap: () {
                setState(() {
                  _isFavorited = !_isFavorited;
                });
              },
              child: AnimatedContainer(
                duration: const Duration(milliseconds: 200),
                padding: const EdgeInsets.all(16),
                decoration: BoxDecoration(
                  color: _isFavorited ? Colors.red[100] : Colors.grey[800],
                  borderRadius: BorderRadius.circular(50),
                ),
                child: Icon(
                  _isFavorited ? Icons.favorite : Icons.favorite_border,
                  size: 40,
                  color: _isFavorited ? Colors.red : Colors.grey,
                ),
              ),
            ),
            
            const SizedBox(height: 40),
            
            // Star rating
            const Text(
              'Rate this app:',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: List.generate(5, (index) {
                return GestureDetector(
                  onTap: () {
                    setState(() {
                      _rating = index + 1;
                    });
                  },
                  child: Icon(
                    index < _rating ? Icons.star : Icons.star_border,
                    size: 40,
                    color: Colors.amber,
                  ),
                );
              }),
            ),
            
            const SizedBox(height: 20),
            Text(
              'Rating: $_rating/5',
              style: const TextStyle(fontSize: 16),
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates interactive icons with state changes, animations,  
and user feedback. It includes a favorite toggle, bookmark in app bar,  
and star rating system with visual feedback and smooth transitions.  

## ElevatedButton

ElevatedButton is the primary button widget in Flutter, providing material  
design styling with elevation and various customization options.  

### Basic ElevatedButton

Simple elevated buttons with different styles and states.  

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
      theme: ThemeData.dark(),
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Basic ElevatedButton'),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              ElevatedButton(
                onPressed: () {
                  debugPrint('Basic button pressed');
                },
                child: const Text('Basic Button'),
              ),
              
              const SizedBox(height: 20),
              
              ElevatedButton.icon(
                onPressed: () {
                  debugPrint('Icon button pressed');
                },
                icon: const Icon(Icons.thumb_up),
                label: const Text('Like'),
              ),
              
              const SizedBox(height: 20),
              
              const ElevatedButton(
                onPressed: null, // Disabled button
                child: Text('Disabled Button'),
              ),
              
              const SizedBox(height: 20),
              
              SizedBox(
                width: 200,
                height: 50,
                child: ElevatedButton(
                  onPressed: () {
                    debugPrint('Wide button pressed');
                  },
                  child: const Text(
                    'Wide Button',
                    style: TextStyle(fontSize: 16),
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example shows basic ElevatedButton usage including standard button,  
icon button, disabled state, and custom sizing. Each button demonstrates  
different configurations and styling options available.  

### Styled ElevatedButton

Custom styled buttons with colors, shapes, and advanced theming.  

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
      theme: ThemeData.dark(),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  bool _isLoading = false;

  Future<void> _simulateAction() async {
    setState(() {
      _isLoading = true;
    });
    
    await Future.delayed(const Duration(seconds: 2));
    
    setState(() {
      _isLoading = false;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Styled ElevatedButton'),
      ),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(20),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              // Custom colored button
              ElevatedButton(
                onPressed: () {},
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.purple,
                  foregroundColor: Colors.white,
                  elevation: 8,
                  shadowColor: Colors.purpleAccent,
                  padding: const EdgeInsets.symmetric(
                    horizontal: 32, 
                    vertical: 16,
                  ),
                ),
                child: const Text('Purple Button'),
              ),
              
              const SizedBox(height: 20),
              
              // Rounded button
              ElevatedButton(
                onPressed: () {},
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.orange,
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(25),
                  ),
                  padding: const EdgeInsets.symmetric(
                    horizontal: 40, 
                    vertical: 15,
                  ),
                ),
                child: const Text('Rounded Button'),
              ),
              
              const SizedBox(height: 20),
              
              // Gradient button
              Container(
                decoration: BoxDecoration(
                  gradient: const LinearGradient(
                    colors: [Colors.blue, Colors.green],
                  ),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: ElevatedButton(
                  onPressed: () {},
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.transparent,
                    shadowColor: Colors.transparent,
                    padding: const EdgeInsets.symmetric(
                      horizontal: 30, 
                      vertical: 15,
                    ),
                  ),
                  child: const Text(
                    'Gradient Button',
                    style: TextStyle(color: Colors.white),
                  ),
                ),
              ),
              
              const SizedBox(height: 20),
              
              // Loading button
              SizedBox(
                width: 150,
                height: 45,
                child: ElevatedButton(
                  onPressed: _isLoading ? null : _simulateAction,
                  child: _isLoading
                      ? const SizedBox(
                          width: 20,
                          height: 20,
                          child: CircularProgressIndicator(
                            strokeWidth: 2,
                            valueColor: AlwaysStoppedAnimation<Color>(
                              Colors.white,
                            ),
                          ),
                        )
                      : const Text('Load Data'),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

This example demonstrates advanced button styling including custom colors,  
shapes, gradients, and loading states. It shows how to create visually  
appealing buttons with different themes and interactive feedback.  

## Scaffold

Scaffold provides the basic material design layout structure for a screen.  
It's the foundation widget that implements the basic material layout.  

### Basic Scaffold

A simple scaffold with app bar, body, and floating action button.  

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
      theme: ThemeData.dark(),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic Scaffold'),
        backgroundColor: Colors.blue,
        elevation: 4,
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'You have pushed the button this many times:',
              style: TextStyle(fontSize: 16),
            ),
            const SizedBox(height: 20),
            Text(
              '$_counter',
              style: const TextStyle(
                fontSize: 48,
                fontWeight: FontWeight.bold,
                color: Colors.blue,
              ),
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

This example shows a basic scaffold structure with app bar, centered body  
content, and floating action button. It implements a simple counter app  
demonstrating the fundamental scaffold layout components.  

### Advanced Scaffold

A scaffold with drawer, bottom navigation, and snack bar functionality.  

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
      theme: ThemeData.dark(),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _selectedIndex = 0;
  final List<String> _pages = ['Home', 'Search', 'Profile'];

  void _onItemTapped(int index) {
    setState(() {
      _selectedIndex = index;
    });
  }

  void _showSnackBar() {
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('This is a snack bar!'),
        duration: Duration(seconds: 2),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(_pages[_selectedIndex]),
        actions: [
          IconButton(
            icon: const Icon(Icons.notifications),
            onPressed: _showSnackBar,
          ),
          IconButton(
            icon: const Icon(Icons.more_vert),
            onPressed: () {},
          ),
        ],
      ),
      drawer: Drawer(
        child: ListView(
          padding: EdgeInsets.zero,
          children: [
            const DrawerHeader(
              decoration: BoxDecoration(
                color: Colors.blue,
              ),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                mainAxisAlignment: MainAxisAlignment.end,
                children: [
                  CircleAvatar(
                    radius: 30,
                    backgroundColor: Colors.white,
                    child: Icon(Icons.person, size: 30, color: Colors.blue),
                  ),
                  SizedBox(height: 10),
                  Text(
                    'John Doe',
                    style: TextStyle(color: Colors.white, fontSize: 18),
                  ),
                  Text(
                    'john.doe@example.com',
                    style: TextStyle(color: Colors.white70, fontSize: 14),
                  ),
                ],
              ),
            ),
            ListTile(
              leading: const Icon(Icons.home),
              title: const Text('Home'),
              onTap: () {
                Navigator.pop(context);
              },
            ),
            ListTile(
              leading: const Icon(Icons.settings),
              title: const Text('Settings'),
              onTap: () {
                Navigator.pop(context);
              },
            ),
            ListTile(
              leading: const Icon(Icons.help),
              title: const Text('Help'),
              onTap: () {
                Navigator.pop(context);
              },
            ),
            const Divider(),
            ListTile(
              leading: const Icon(Icons.logout),
              title: const Text('Logout'),
              onTap: () {
                Navigator.pop(context);
              },
            ),
          ],
        ),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(
              _selectedIndex == 0
                  ? Icons.home
                  : _selectedIndex == 1
                      ? Icons.search
                      : Icons.person,
              size: 100,
              color: Colors.blue,
            ),
            const SizedBox(height: 20),
            Text(
              'Current Page: ${_pages[_selectedIndex]}',
              style: const TextStyle(
                fontSize: 24,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 40),
            ElevatedButton(
              onPressed: _showSnackBar,
              child: const Text('Show Snack Bar'),
            ),
          ],
        ),
      ),
      bottomNavigationBar: BottomNavigationBar(
        items: const [
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.search),
            label: 'Search',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.person),
            label: 'Profile',
          ),
        ],
        currentIndex: _selectedIndex,
        onTap: _onItemTapped,
      ),
    );
  }
}
```

This example demonstrates advanced scaffold features including navigation  
drawer with user profile, bottom navigation bar, app bar actions, and  
snack bar integration. It shows a complete app structure with multiple  
navigation options and interactive elements.  