# Various


## Widget alignments

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
      title: 'Container Example',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Container Widget'),
        ),
        body: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            // Top row
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                _buildAlignedContainer(Alignment.topLeft, 'Top Left'),
                const SizedBox(width: 10),
                _buildAlignedContainer(Alignment.topCenter, 'Top Center'),
                const SizedBox(width: 10),
                _buildAlignedContainer(Alignment.topRight, 'Top Right'),
              ],
            ),
            const SizedBox(height: 20),
            // Center row
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                _buildAlignedContainer(Alignment.centerLeft, 'Center Left'),
                const SizedBox(width: 10),
                _buildAlignedContainer(Alignment.center, 'Center'),
                const SizedBox(width: 10),
                _buildAlignedContainer(Alignment.centerRight, 'Center Right'),
              ],
            ),
            const SizedBox(height: 20),
            // Bottom row
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                _buildAlignedContainer(Alignment.bottomLeft, 'Bottom Left'),
                const SizedBox(width: 10),
                _buildAlignedContainer(Alignment.bottomCenter, 'Bottom Center'),
                const SizedBox(width: 10),
                _buildAlignedContainer(Alignment.bottomRight, 'Bottom Right'),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildAlignedContainer(Alignment alignment, String text) {
    return Container(
      width: 100,
      height: 100,
      padding: const EdgeInsets.all(8),
      margin: const EdgeInsets.all(5),
      decoration: BoxDecoration(
        color: Colors.blueGrey,
        borderRadius: BorderRadius.circular(8),
        boxShadow: [
          BoxShadow(
            color: Colors.black.withOpacity(0.2),
            blurRadius: 4,
            offset: const Offset(2, 2),
          ),
        ],
      ),
      alignment: alignment,
      child: Text(
        text,
        style: const TextStyle(
          color: Colors.white,
          fontSize: 10,
        ),
      ),
    );
  }
}
```

## Intent and actions

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
      title: 'Flutter Actions Binding Demo',
      theme: ThemeData.dark(
        useMaterial3: true,
      ),
      home: const MyHomePage(),
    );
  }
}

class IncrementIntent extends Intent {
  const IncrementIntent();
}

class DecrementIntent extends Intent {
  const DecrementIntent();
}

class ResetIntent extends Intent {
  const ResetIntent();
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;
  String _lastAction = 'None';

  void _increment() {
    setState(() {
      _counter++;
      _lastAction = 'Incremented';
    });
  }

  void _decrement() {
    setState(() {
      _counter--;
      _lastAction = 'Decremented';
    });
  }

  void _reset() {
    setState(() {
      _counter = 0;
      _lastAction = 'Reset';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Shortcuts(
      shortcuts: <LogicalKeySet, Intent>{
        LogicalKeySet(LogicalKeyboardKey.arrowUp): const IncrementIntent(),
        LogicalKeySet(LogicalKeyboardKey.arrowDown): const DecrementIntent(),
        LogicalKeySet(LogicalKeyboardKey.keyR): const ResetIntent(),
      },
      child: Actions(
        actions: <Type, Action<Intent>>{
          IncrementIntent: CallbackAction<IncrementIntent>(
            onInvoke: (IncrementIntent intent) => _increment(),
          ),
          DecrementIntent: CallbackAction<DecrementIntent>(
            onInvoke: (DecrementIntent intent) => _decrement(),
          ),
          ResetIntent: CallbackAction<ResetIntent>(
            onInvoke: (ResetIntent intent) => _reset(),
          ),
        },
        child: Focus(
          autofocus: true,
          child: Scaffold(
            appBar: AppBar(
              title: const Text('Actions Binding Demo'),
            ),
            body: Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Counter: $_counter',
                    style: Theme.of(context).textTheme.headlineMedium,
                  ),
                  const SizedBox(height: 8),
                  Text(
                    'Last Action: $_lastAction',
                    style: Theme.of(context).textTheme.bodyLarge,
                  ),
                  const SizedBox(height: 32),
                  const Text(
                    '1. Button Binding (onPressed):',
                    style: TextStyle(fontWeight: FontWeight.bold),
                  ),
                  const SizedBox(height: 8),
                  Row(
                    children: [
                      ElevatedButton(
                        onPressed: _increment,
                        child: const Text('+'),
                      ),
                      const SizedBox(width: 8),
                      ElevatedButton(
                        onPressed: _decrement,
                        child: const Text('-'),
                      ),
                      const SizedBox(width: 8),
                      ElevatedButton(
                        onPressed: _reset,
                        child: const Text('Reset'),
                      ),
                    ],
                  ),
                  const SizedBox(height: 32),
                  const Text(
                    '2. Gesture Binding (onTap):',
                    style: TextStyle(fontWeight: FontWeight.bold),
                  ),
                  const SizedBox(height: 8),
                  Row(
                    children: [
                      GestureDetector(
                        onTap: _increment,
                        child: Container(
                          padding: const EdgeInsets.all(12),
                          decoration: BoxDecoration(
                            color: Colors.green,
                            borderRadius: BorderRadius.circular(8),
                          ),
                          child: const Text(
                            'Tap to +',
                            style: TextStyle(color: Colors.white),
                          ),
                        ),
                      ),
                      const SizedBox(width: 8),
                      GestureDetector(
                        onDoubleTap: _decrement,
                        child: Container(
                          padding: const EdgeInsets.all(12),
                          decoration: BoxDecoration(
                            color: Colors.red,
                            borderRadius: BorderRadius.circular(8),
                          ),
                          child: const Text(
                            'Double Tap to -',
                            style: TextStyle(color: Colors.white),
                          ),
                        ),
                      ),
                    ],
                  ),
                  const SizedBox(height: 32),
                  const Text(
                    '3. Keyboard Shortcuts (Actions/Shortcuts):',
                    style: TextStyle(fontWeight: FontWeight.bold),
                  ),
                  const SizedBox(height: 8),
                  const Text(
                    '• Press ↑ (Up Arrow) to increment\n'
                    '• Press ↓ (Down Arrow) to decrement\n'
                    '• Press R to reset',
                    style: TextStyle(fontSize: 14),
                  ),
                  const Spacer(),
                  const Text(
                    'Try all three binding methods!',
                    style: TextStyle(
                      fontStyle: FontStyle.italic,
                      color: Colors.grey,
                    ),
                  ),
                ],
              ),
            ),
            floatingActionButton: FloatingActionButton(
              onPressed: _increment,
              tooltip: 'Increment',
              child: const Icon(Icons.add),
            ),
          ),
        ),
      ),
    );
  }
}
```


## Star field

```dart
import 'package:flutter/material.dart';
import 'dart:math';
import 'dart:async';
import 'dart:ui';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Starfield Example',
      theme: ThemeData.dark(),
      home: const MyHomePage(title: 'Fading Stars'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class Star {
  Offset position;
  double opacity;

  Star(this.position, this.opacity);
}

class _MyHomePageState extends State<MyHomePage> {
  List<Star> stars = [];
  Timer? timer;

  @override
  void initState() {
    super.initState();
    timer = Timer.periodic(const Duration(milliseconds: 100), (timer) {
      setState(() {
        // Add a new star at random position
        final random = Random();
        final x = random.nextDouble() * MediaQuery.of(context).size.width;
        final y = random.nextDouble() * MediaQuery.of(context).size.height;
        stars.add(Star(Offset(x, y), 1.0));

        // Decrease opacity of all stars and remove faded ones
        stars.removeWhere((star) {
          star.opacity -= 0.05;
          return star.opacity <= 0;
        });
      });
    });
  }

  @override
  void dispose() {
    timer?.cancel();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // appBar: AppBar(
      //   title: Text(widget.title),
      // ),
      backgroundColor: Colors.black,
      body: CustomPaint(
        painter: StarPainter(stars),
        child: Container(),
      ),
    );
  }
}

class StarPainter extends CustomPainter {
  final List<Star> stars;

  StarPainter(this.stars);

  @override
  void paint(Canvas canvas, Size size) {
    for (final star in stars) {
      final paint = Paint()
        ..color = Colors.white.withValues(alpha: star.opacity)
        ..style = PaintingStyle.stroke
        ..strokeWidth = 1.0;

      double centerX = star.position.dx;
      double centerY = star.position.dy;
      // Draw a small cross
      canvas.drawLine(Offset(centerX - 2, centerY), Offset(centerX + 2, centerY), paint);
      canvas.drawLine(Offset(centerX, centerY - 2), Offset(centerX, centerY + 2), paint);
    }
  }

  @override
  bool shouldRepaint(StarPainter oldDelegate) => true;
}
```


## Water reflection


```dart
import 'package:flutter/material.dart';

class ReflectionExample extends StatelessWidget {
  const ReflectionExample({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Water Reflection Example',
      home: const ReflectionPage(),
    );
  }
}

class ReflectionPage extends StatelessWidget {
  const ReflectionPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Water Reflection'),
      ),
      body: Center(
        child: SizedBox(
          height: 600,
          child: Stack(
            alignment: Alignment.topCenter,
            children: [
              // Original image
              Image.network(
                'https://picsum.photos/200/300?random=1',
                height: 300,
                width: 200,
                fit: BoxFit.cover,
              ),
              // Reflection
              Positioned(
                top: 320,
                child: ShaderMask(
                  shaderCallback: (bounds) => LinearGradient(
                    begin: Alignment.topCenter,
                    end: Alignment.bottomCenter,
                    colors: [Colors.white.withValues(alpha: 0.8), Colors.transparent],
                  ).createShader(bounds),
                  blendMode: BlendMode.dstIn,
                  child: Transform.scale(
                    scaleY: -1,
                    child: Image.network(
                      'https://picsum.photos/200/300?random=1',
                      height: 250,
                      width: 200,
                      fit: BoxFit.cover,
                    ),
                  ),
                ),
              ),
              // Water-like gradient overlay
              Positioned(
                top: 320,
                child: Container(
                  height: 250,
                  width: 200,
                  decoration: BoxDecoration(
                    gradient: LinearGradient(
                      begin: Alignment.topCenter,
                      end: Alignment.bottomCenter,
                      colors: [
                        Colors.blue.withValues(alpha: 0.1),
                        Colors.blue.withValues(alpha: 0.3),
                        Colors.blue.withValues(alpha: 0.5),
                      ],
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

## Painting 

```dart
import 'package:flutter/material.dart';
import 'dart:math';
import 'dart:ui';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Paint Shapes Example',
      theme: ThemeData.dark(),
      home: const MyHomePage(title: 'Painted Shapes'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: CustomPaint(
        painter: ShapePainter(),
        child: Container(),
      ),
    );
  }
}

class ShapePainter extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    // Fill paint for filled shapes
    final fillPaint = Paint()
      ..style = PaintingStyle.fill;

    // Stroke paint for outlines
    final strokePaint = Paint()
      ..style = PaintingStyle.stroke
      ..strokeWidth = 3.0
      ..color = Colors.white;

    // Draw a white circle (filled)
    fillPaint.color = Colors.white;
    canvas.drawCircle(Offset(size.width / 6, size.height / 6), 40, fillPaint);

    // Draw a cyan rectangle (filled)
    fillPaint.color = Colors.cyan;
    canvas.drawRect(Rect.fromLTWH(size.width / 3, size.height / 6 - 20, 80, 40), fillPaint);

    // Draw a orange rounded rectangle (filled)
    fillPaint.color = Colors.orange;
    canvas.drawRRect(RRect.fromRectAndRadius(Rect.fromLTWH(size.width * 2 / 3, size.height / 6 - 20, 80, 40), const Radius.circular(10)), fillPaint);

    // Draw a pink oval (filled)
    fillPaint.color = Colors.pink;
    canvas.drawOval(Rect.fromLTWH(30, size.height / 3, 120, 60), fillPaint);

    // Draw a lime arc (filled)
    fillPaint.color = Colors.lime;
    canvas.drawArc(
      Rect.fromLTWH(size.width - 150, size.height / 3, 120, 120),
      0,
      pi / 2,
      true,
      fillPaint,
    );

    // Draw a purple triangle using path (filled)
    fillPaint.color = Colors.purple;
    final trianglePath = Path()
      ..moveTo(size.width / 2, size.height / 2)
      ..lineTo(size.width / 2 - 40, size.height / 2 + 40)
      ..lineTo(size.width / 2 + 40, size.height / 2 + 40)
      ..close();
    canvas.drawPath(trianglePath, fillPaint);

    // Draw a yellow star using path (filled)
    fillPaint.color = Colors.yellow;
    final starPath = Path();
    double centerX = size.width / 6;
    double centerY = size.height * 2 / 3;
    double radius = 30;
    for (int i = 0; i < 5; i++) {
      double angle = (i * 72 - 90) * pi / 180;
      double x = centerX + radius * cos(angle);
      double y = centerY + radius * sin(angle);
      if (i == 0) {
        starPath.moveTo(x, y);
      } else {
        starPath.lineTo(x, y);
      }
      angle = ((i + 0.5) * 72 - 90) * pi / 180;
      x = centerX + (radius / 2) * cos(angle);
      y = centerY + (radius / 2) * sin(angle);
      starPath.lineTo(x, y);
    }
    starPath.close();
    canvas.drawPath(starPath, fillPaint);

    // Draw a line (stroke)
    strokePaint.color = Colors.red;
    canvas.drawLine(Offset(size.width / 3, size.height * 2 / 3), Offset(size.width / 3 + 100, size.height * 2 / 3 + 50), strokePaint);

    // Draw points (stroke)
    strokePaint.color = Colors.green;
    canvas.drawPoints(PointMode.points, [Offset(size.width * 2 / 3, size.height * 2 / 3), Offset(size.width * 2 / 3 + 20, size.height * 2 / 3 + 20)], strokePaint);

    // Draw a hexagon using path (stroke)
    strokePaint.color = Colors.blue;
    final hexagonPath = Path();
    double hexCenterX = size.width * 5 / 6;
    double hexCenterY = size.height * 2 / 3;
    double hexRadius = 35;
    for (int i = 0; i < 6; i++) {
      double angle = (i * 60) * pi / 180;
      double x = hexCenterX + hexRadius * cos(angle);
      double y = hexCenterY + hexRadius * sin(angle);
      if (i == 0) {
        hexagonPath.moveTo(x, y);
      } else {
        hexagonPath.lineTo(x, y);
      }
    }
    hexagonPath.close();
    canvas.drawPath(hexagonPath, strokePaint);
  }

  @override
  bool shouldRepaint(CustomPainter oldDelegate) => false;
}
```

## Command palette

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'dart:io';

class ShowCommandPaletteIntent extends Intent {
  const ShowCommandPaletteIntent();
}

class HideCommandPaletteIntent extends Intent {
  const HideCommandPaletteIntent();
}

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Command Palette App',
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

class Command {
  final String name;
  final String description;
  final VoidCallback action;

  Command(this.name, this.description, this.action);
}

class _MyHomePageState extends State<MyHomePage> {
  String statusMessage = 'Ready';
  bool showCommandPalette = false;
  late List<Command> commands;
  final FocusNode _commandFocusNode = FocusNode();
  final FocusNode _mainFocusNode = FocusNode();

  @override
  void initState() {
    super.initState();
    commands = [
      Command('time', 'Show current time', () {
        setState(() {
          statusMessage = 'Current time: ${DateTime.now().toString()}';
        });
      }),
      Command('hello', 'Display hello message', () {
        setState(() {
          statusMessage = 'Hello there!';
        });
      }),
      Command('quit', 'Quit the app', () {
        exit(0);
      }),
    ];
  }

  void _showCommandPalette() {
    setState(() {
      showCommandPalette = true;
    });
    // Request focus after the widget is built
    WidgetsBinding.instance.addPostFrameCallback((_) {
      _commandFocusNode.requestFocus();
    });
  }

  @override
  void dispose() {
    _commandFocusNode.dispose();
    _mainFocusNode.dispose();
    super.dispose();
  }

  void _hideCommandPalette() {
    setState(() {
      showCommandPalette = false;
    });
    // Return focus to main widget after hiding palette
    WidgetsBinding.instance.addPostFrameCallback((_) {
      _mainFocusNode.requestFocus();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Shortcuts(
            shortcuts: <LogicalKeySet, Intent>{
        LogicalKeySet(LogicalKeyboardKey.control, LogicalKeyboardKey.shift, LogicalKeyboardKey.keyP): const ShowCommandPaletteIntent(),
        LogicalKeySet(LogicalKeyboardKey.escape): const HideCommandPaletteIntent(),
      },
      child: Actions(
        actions: <Type, Action<Intent>>{
          ShowCommandPaletteIntent: CallbackAction<ShowCommandPaletteIntent>(
            onInvoke: (ShowCommandPaletteIntent intent) =>
                _showCommandPalette(),
          ),
          HideCommandPaletteIntent: CallbackAction<HideCommandPaletteIntent>(
            onInvoke: (HideCommandPaletteIntent intent) =>
                _hideCommandPalette(),
          ),
        },
        child: Focus(
          focusNode: _mainFocusNode,
          autofocus: true,
          child: Scaffold(
            body: GestureDetector(
              behavior: HitTestBehavior.opaque,
              onTap: () {
                if (showCommandPalette) {
                  _hideCommandPalette();
                }
              },
              child: Column(
                children: [
                  if (showCommandPalette)
                    GestureDetector(
                      onTap: () {}, // Prevent tap from bubbling up
                      child: Container(
                        color: Theme.of(context).colorScheme.surface,
                        padding: const EdgeInsets.all(8.0),
                        child: TextField(
                          focusNode: _commandFocusNode,
                          decoration: const InputDecoration(
                            hintText: 'Type a command...',
                            border: OutlineInputBorder(),
                          ),
                          onSubmitted: (value) {
                            _hideCommandPalette();
                            final cmd = commands.firstWhere(
                              (c) => c.name.toLowerCase() == value.toLowerCase(),
                              orElse: () => Command('', '', () {}),
                            );
                            if (cmd.name.isNotEmpty) {
                              cmd.action();
                            } else {
                              setState(() {
                                statusMessage = 'Unknown command: $value';
                              });
                            }
                          },
                        ),
                      ),
                    ),
                  Expanded(
                    child: const Center(
                      child: Text(
                        'Press Ctrl + Shift + P to open command palette',
                      ),
                    ),
                  ),
                ],
              ),
            ),
            bottomNavigationBar: GestureDetector(
              onTap: () {
                if (showCommandPalette) {
                  _hideCommandPalette();
                }
              },
              child: Container(
                height: 40,
                padding: const EdgeInsets.symmetric(
                  horizontal: 8.0,
                  vertical: 8.0,
                ),
                color: Theme.of(context).bottomAppBarTheme.color,
                child: Align(
                  alignment: Alignment.centerLeft,
                  child: Text(statusMessage),
                ),
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```


