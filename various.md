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

## Reactive patterns

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
      title: 'Flutter Reactivity Demo',
      theme: ThemeData.dark(
        
        useMaterial3: true,
      ),
      home: const MyHomePage(),
    );
  }
}

class CounterNotifier extends ChangeNotifier {
  int _count = 0;
  int get count => _count;

  void increment() {
    _count++;
    notifyListeners();
  }

  void decrement() {
    _count--;
    notifyListeners();
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  // 1. Basic state
  int _basicCounter = 0;

  // 2. Stream
  final StreamController<int> _streamController = StreamController<int>();
  int _streamValue = 0;

  // 3. ValueNotifier
  final ValueNotifier<int> _valueNotifier = ValueNotifier<int>(0);

  // 4. ChangeNotifier
  final CounterNotifier _counterNotifier = CounterNotifier();

  // 5. Timer
  Timer? _timer;
  int _timerCount = 0;

  @override
  void initState() {
    super.initState();
    _timer = Timer.periodic(const Duration(seconds: 1), (timer) {
      setState(() => _timerCount++);
      _streamController.add(_streamValue++);
    });
  }

  @override
  void dispose() {
    _timer?.cancel();
    _streamController.close();
    _valueNotifier.dispose();
    _counterNotifier.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Flutter Reactivity Demo')),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text('Flutter Reactivity Patterns', style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold)),
            const SizedBox(height: 24),

            // 1. setState
            _buildSection('1. setState() - Basic State',
              Text('Counter: $_basicCounter', style: const TextStyle(fontSize: 18)),
              Row(children: [
                ElevatedButton(onPressed: () => setState(() => _basicCounter++), child: const Text('+')),
                ElevatedButton(onPressed: () => setState(() => _basicCounter--), child: const Text('-')),
              ]),
            ),

            // 2. StreamBuilder
            _buildSection('2. StreamBuilder - Stream Reactivity',
              StreamBuilder<int>(
                stream: _streamController.stream,
                initialData: 0,
                builder: (context, snapshot) => Text('Stream: ${snapshot.data}', style: const TextStyle(fontSize: 18)),
              ),
              const Text('Updates automatically from stream'),
            ),

            // 3. ValueListenableBuilder
            _buildSection('3. ValueListenableBuilder - ValueNotifier',
              ValueListenableBuilder<int>(
                valueListenable: _valueNotifier,
                builder: (context, value, child) => Text('Notifier: $value', style: const TextStyle(fontSize: 18)),
              ),
              ElevatedButton(onPressed: () => _valueNotifier.value++, child: const Text('Increment Notifier')),
            ),

            // 4. ChangeNotifier
            _buildSection('4. ChangeNotifier - Complex State',
              ListenableBuilder(
                listenable: _counterNotifier,
                builder: (context, child) => Text('ChangeNotifier: ${_counterNotifier.count}', style: const TextStyle(fontSize: 18)),
              ),
              Row(children: [
                ElevatedButton(onPressed: _counterNotifier.increment, child: const Text('+')),
                ElevatedButton(onPressed: _counterNotifier.decrement, child: const Text('-')),
              ]),
            ),

            // 5. Timer reactivity
            _buildSection('5. Timer - Periodic Updates',
              Text('Timer: $_timerCount (auto-updates)', style: const TextStyle(fontSize: 18)),
              const Text('No user interaction needed!'),
            ),

            const SizedBox(height: 32),
            const Card(
              child: Padding(
                padding: EdgeInsets.all(16.0),
                child: Text(
                  'Flutter\'s reactivity means UI automatically updates when state changes. No manual DOM manipulation!',
                  style: TextStyle(fontStyle: FontStyle.italic),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildSection(String title, Widget content, dynamic controls) {
    return Card(
      margin: const EdgeInsets.only(bottom: 16),
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(title, style: const TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
            const SizedBox(height: 8),
            content,
            const SizedBox(height: 8),
            controls is Widget ? controls : const SizedBox(),
          ],
        ),
      ),
    );
  }
}
```

## State changes

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
      title: 'Widget State Changes Demo',
      theme: ThemeData(
        brightness: Brightness.dark,
        colorScheme: const ColorScheme.dark(
          primary: Color(0xFF81A1C1),        // Nord blue
          onPrimary: Color(0xFF2E3440),      // Nord dark
          secondary: Color(0xFF5E81AC),      // Nord light blue
          onSecondary: Color(0xFF2E3440),    // Nord dark
          surface: Color(0xFF2E3440),        // Darker Nord gray
          onSurface: Color(0xFFECEFF4),     // Nord snow
          surfaceContainerHighest: Color(0xFF1E222A), // Much darker Nord for background
          onSurfaceVariant: Color(0xFFECEFF4),  // Nord snow for onBackground
          error: Color(0xFFBF616A),          // Nord red
          onError: Color(0xFFECEFF4),        // Nord snow
        ),
        useMaterial3: true,
      ),
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
  // State variables that control widget properties
  String _textContent = 'Hello Flutter!';
  double _fontSize = 24.0;
  Color _textColor = Colors.black;
  FontWeight _fontWeight = FontWeight.normal;

  Color _containerColor = Colors.blue;
  double _containerSize = 100.0;
  double _borderRadius = 8.0;

  IconData _icon = Icons.star;
  double _iconSize = 48.0;
  Color _iconColor = Colors.yellow;

  double _progressValue = 0.5;
  bool _switchValue = false;
  bool _checkboxValue = false;
  double _sliderValue = 50.0;

  int _imageIndex = 0;
  final List<String> _emojis = ['😀', '🚀', '🌟', '🎯', '💡'];

  void _randomizeAll() {
    setState(() {
      // Text properties
      _textContent = ['Hello!', 'Flutter Rocks!', 'State Changes!', 'Reactive UI!', 'Amazing!'][DateTime.now().millisecond % 5];
      _fontSize = 16.0 + (DateTime.now().millisecond % 24);
      _textColor = [Colors.black, Colors.red, Colors.blue, Colors.green, Colors.purple][DateTime.now().millisecond % 5];
      _fontWeight = [FontWeight.normal, FontWeight.bold][DateTime.now().millisecond % 2];

      // Container properties
      _containerColor = [Colors.blue, Colors.red, Colors.green, Colors.orange, Colors.purple][DateTime.now().millisecond % 5];
      _containerSize = 80.0 + (DateTime.now().millisecond % 80);
      _borderRadius = (DateTime.now().millisecond % 40).toDouble();

      // Icon properties
      _icon = [Icons.star, Icons.favorite, Icons.thumb_up, Icons.lightbulb, Icons.rocket][DateTime.now().millisecond % 5];
      _iconSize = 32.0 + (DateTime.now().millisecond % 48);
      _iconColor = [Colors.yellow, Colors.red, Colors.blue, Colors.green, Colors.orange][DateTime.now().millisecond % 5];

      // Progress and controls
      _progressValue = (DateTime.now().millisecond % 100) / 100.0;
      _switchValue = !_switchValue;
      _checkboxValue = !_checkboxValue;
      _sliderValue = (DateTime.now().millisecond % 100).toDouble();

      // Image/Emoji
      _imageIndex = (_imageIndex + 1) % _emojis.length;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Widget State Changes Demo'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Direct State Changes in Flutter Widgets',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _randomizeAll,
              child: const Text('Randomize All Properties'),
            ),
            const SizedBox(height: 24),

            // Text widget with changing properties
            _buildDemoSection(
              '1. Text Widget',
              'Content, size, color, and weight change based on state',
              Text(
                _textContent,
                style: TextStyle(
                  fontSize: _fontSize,
                  color: _textColor,
                  fontWeight: _fontWeight,
                ),
              ),
            ),

            // Container with changing properties
            _buildDemoSection(
              '2. Container Widget',
              'Color, size, and border radius change dynamically',
              Container(
                width: _containerSize,
                height: _containerSize,
                decoration: BoxDecoration(
                  color: _containerColor,
                  borderRadius: BorderRadius.circular(_borderRadius),
                  boxShadow: [
                    BoxShadow(
                      color: Colors.black.withValues(alpha: 0.3),
                      blurRadius: 8,
                      offset: const Offset(2, 2),
                    ),
                  ],
                ),
                child: const Center(
                  child: Text(
                    'Box',
                    style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold),
                  ),
                ),
              ),
            ),

            // Icon with changing properties
            _buildDemoSection(
              '3. Icon Widget',
              'Icon type, size, and color change reactively',
              Icon(
                _icon,
                size: _iconSize,
                color: _iconColor,
              ),
            ),

            // Progress indicators
            _buildDemoSection(
              '4. Progress Indicators',
              'Progress value changes dynamically',
              Column(
                children: [
                  LinearProgressIndicator(value: _progressValue),
                  const SizedBox(height: 8),
                  CircularProgressIndicator(value: _progressValue),
                  const SizedBox(height: 8),
                  Text('Progress: ${(_progressValue * 100).round()}%'),
                ],
              ),
            ),

            // Interactive controls that change state
            _buildDemoSection(
              '5. Interactive Controls',
              'Switches, checkboxes, and sliders reflect and modify state',
              Column(
                children: [
                  Row(
                    children: [
                      const Text('Switch: '),
                      Switch(
                        value: _switchValue,
                        onChanged: (value) => setState(() => _switchValue = value),
                      ),
                      const SizedBox(width: 16),
                      Text(_switchValue ? 'ON' : 'OFF'),
                    ],
                  ),
                  Row(
                    children: [
                      const Text('Checkbox: '),
                      Checkbox(
                        value: _checkboxValue,
                        onChanged: (value) => setState(() => _checkboxValue = value ?? false),
                      ),
                      const SizedBox(width: 16),
                      Text(_checkboxValue ? 'Checked' : 'Unchecked'),
                    ],
                  ),
                  Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text('Slider: ${_sliderValue.round()}'),
                      Slider(
                        value: _sliderValue,
                        min: 0,
                        max: 100,
                        onChanged: (value) => setState(() => _sliderValue = value),
                      ),
                    ],
                  ),
                ],
              ),
            ),

            // Emoji "image" that changes
            _buildDemoSection(
              '6. Dynamic Content',
              'Emoji changes based on state index',
              Text(
                _emojis[_imageIndex],
                style: const TextStyle(fontSize: 48),
              ),
            ),

            const SizedBox(height: 32),
            const Card(
              child: Padding(
                padding: EdgeInsets.all(16.0),
                child: Text(
                  'Every widget property that depends on state variables automatically updates when those variables change. This is Flutter\'s reactive programming model in action!',
                  style: TextStyle(fontStyle: FontStyle.italic),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildDemoSection(String title, String description, Widget demoWidget) {
    return Card(
      margin: const EdgeInsets.only(bottom: 16),
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(title, style: const TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
            const SizedBox(height: 4),
            Text(description, style: TextStyle(color: Colors.grey[600], fontSize: 14)),
            const SizedBox(height: 16),
            Center(child: demoWidget),
          ],
        ),
      ),
    );
  }
}
```

## Nord theme gallery

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
      title: 'Flutter Widget Gallery - Nord Theme',
      theme: ThemeData(
        brightness: Brightness.dark,
        colorScheme: const ColorScheme.dark(
          primary: Color(0xFF81A1C1),        // Nord blue
          onPrimary: Color(0xFF2E3440),      // Nord dark
          secondary: Color(0xFF5E81AC),      // Nord light blue
          onSecondary: Color(0xFF2E3440),    // Nord dark
          surface: Color(0xFF2E3440),        // Darker Nord gray
          onSurface: Color(0xFFECEFF4),     // Nord snow
          surfaceContainerHighest: Color(0xFF1E222A), // Much darker Nord for background
          onSurfaceVariant: Color(0xFFECEFF4),  // Nord snow for onBackground
          error: Color(0xFFBF616A),          // Nord red
          onError: Color(0xFFECEFF4),        // Nord snow
        ),
        useMaterial3: true,
      ),
      home: const WidgetGallery(),
    );
  }
}

class WidgetGallery extends StatefulWidget {
  const WidgetGallery({super.key});

  @override
  State<WidgetGallery> createState() => _WidgetGalleryState();
}

class _WidgetGalleryState extends State<WidgetGallery> with SingleTickerProviderStateMixin {
  late TabController _tabController;

  @override
  void initState() {
    super.initState();
    _tabController = TabController(length: 5, vsync: this);
  }

  @override
  void dispose() {
    _tabController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Flutter Widget Gallery'),
        bottom: TabBar(
          controller: _tabController,
          tabs: const [
            Tab(text: 'Basic', icon: Icon(Icons.text_fields)),
            Tab(text: 'Layout', icon: Icon(Icons.grid_view)),
            Tab(text: 'Input', icon: Icon(Icons.input)),
            Tab(text: 'Material', icon: Icon(Icons.palette)),
            Tab(text: 'Advanced', icon: Icon(Icons.extension)),
          ],
        ),
      ),
      body: TabBarView(
        controller: _tabController,
        children: [
          _buildBasicWidgetsTab(),
          _buildLayoutWidgetsTab(),
          _buildInputWidgetsTab(),
          _buildMaterialWidgetsTab(),
          _buildAdvancedWidgetsTab(),
        ],
      ),
    );
  }

  Widget _buildBasicWidgetsTab() {
    return ListView(
      padding: const EdgeInsets.all(16),
      children: [
        _buildWidgetCard(
          'Text',
          'Displays text with various styling options',
          const Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text('Regular Text', style: TextStyle(fontSize: 16)),
              Text('Bold Text', style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold)),
              Text('Italic Text', style: TextStyle(fontSize: 16, fontStyle: FontStyle.italic)),
              Text('Colored Text', style: TextStyle(fontSize: 16, color: Color(0xFF81A1C1))),
            ],
          ),
        ),
        _buildWidgetCard(
          'Icon',
          'Displays material design icons',
          const Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: [
              Icon(Icons.star, size: 32, color: Color(0xFF81A1C1)),
              Icon(Icons.favorite, size: 32, color: Color(0xFFBF616A)),
              Icon(Icons.thumb_up, size: 32, color: Color(0xFFA3BE8C)),
              Icon(Icons.lightbulb, size: 32, color: Color(0xFFEBCA89)),
            ],
          ),
        ),
        _buildWidgetCard(
          'Image',
          'Displays images from various sources',
          Container(
            height: 100,
            decoration: BoxDecoration(
              color: Theme.of(context).colorScheme.surface,
              borderRadius: BorderRadius.circular(8),
            ),
            child: const Center(
              child: Text('Image widgets would go here\n(Network, Asset, Memory, etc.)'),
            ),
          ),
        ),
      ],
    );
  }

  Widget _buildLayoutWidgetsTab() {
    return ListView(
      padding: const EdgeInsets.all(16),
      children: [
        _buildWidgetCard(
          'Container',
          'A convenience widget that combines painting and positioning',
          Container(
            width: 120,
            height: 80,
            decoration: BoxDecoration(
              color: Theme.of(context).colorScheme.primary,
              borderRadius: BorderRadius.circular(12),
              boxShadow: [
                BoxShadow(
                  color: Colors.black.withValues(alpha: 0.3),
                  blurRadius: 8,
                  offset: const Offset(2, 2),
                ),
              ],
            ),
            child: const Center(
              child: Text('Container', style: TextStyle(color: Colors.white)),
            ),
          ),
        ),
        _buildWidgetCard(
          'Row & Column',
          'Layout widgets for horizontal and vertical arrangements',
          Column(
            children: [
              const Text('Row Example:'),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  Container(width: 60, height: 60, color: Theme.of(context).colorScheme.primary),
                  Container(width: 60, height: 60, color: Theme.of(context).colorScheme.secondary),
                  Container(width: 60, height: 60, color: Theme.of(context).colorScheme.surface),
                ],
              ),
              const SizedBox(height: 16),
              const Text('Column Example:'),
              Column(
                children: [
                  Container(width: 60, height: 30, color: Theme.of(context).colorScheme.primary),
                  Container(width: 60, height: 30, color: Theme.of(context).colorScheme.secondary),
                  Container(width: 60, height: 30, color: Theme.of(context).colorScheme.surface),
                ],
              ),
            ],
          ),
        ),
        _buildWidgetCard(
          'Stack',
          'Overlays multiple children on top of each other',
          SizedBox(
            height: 100,
            child: Stack(
              alignment: Alignment.center,
              children: [
                Container(width: 80, height: 80, color: Theme.of(context).colorScheme.primary),
                Container(width: 60, height: 60, color: Theme.of(context).colorScheme.secondary),
                Container(width: 40, height: 40, color: Theme.of(context).colorScheme.surface),
                const Text('Stack', style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold)),
              ],
            ),
          ),
        ),
      ],
    );
  }

  Widget _buildInputWidgetsTab() {
    return ListView(
      padding: const EdgeInsets.all(16),
      children: [
        _buildWidgetCard(
          'TextField',
          'Input field for text entry',
          const SizedBox(
            width: 250,
            child: TextField(
              decoration: InputDecoration(
                labelText: 'Enter text',
                border: OutlineInputBorder(),
              ),
            ),
          ),
        ),
        _buildWidgetCard(
          'Buttons',
          'Various button types for user interaction',
          Column(
            children: [
              ElevatedButton(onPressed: () {}, child: const Text('Elevated')),
              const SizedBox(height: 8),
              OutlinedButton(onPressed: () {}, child: const Text('Outlined')),
              const SizedBox(height: 8),
              TextButton(onPressed: () {}, child: const Text('Text')),
            ],
          ),
        ),
        _buildWidgetCard(
          'Selection Controls',
          'Switches, checkboxes, and radio buttons',
          Column(
            children: [
              Row(
                children: [
                  const Text('Switch:'),
                  const SizedBox(width: 8),
                  Switch(value: true, onChanged: (value) {}),
                ],
              ),
              Row(
                children: [
                  const Text('Checkbox:'),
                  const SizedBox(width: 8),
                  Checkbox(value: true, onChanged: (value) {}),
                ],
              ),
            ],
          ),
        ),
        _buildWidgetCard(
          'Slider',
          'Allows selection from a range of values',
          const SizedBox(
            width: 250,
            child: Slider(value: 0.5, onChanged: null),
          ),
        ),
      ],
    );
  }

  Widget _buildMaterialWidgetsTab() {
    return ListView(
      padding: const EdgeInsets.all(16),
      children: [
        _buildWidgetCard(
          'Card',
          'Material Design card for displaying content',
          const Card(
            child: Padding(
              padding: EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text('Card Title', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
                  SizedBox(height: 8),
                  Text('This is a material design card with some content.'),
                ],
              ),
            ),
          ),
        ),
        _buildWidgetCard(
          'AppBar',
          'Top app bar with title and actions',
          Container(
            height: 60,
            decoration: BoxDecoration(
              color: Theme.of(context).colorScheme.surface,
              borderRadius: BorderRadius.circular(8),
            ),
            child: const Row(
              children: [
                SizedBox(width: 16),
                Text('App Bar', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
                Spacer(),
                Icon(Icons.search),
                SizedBox(width: 8),
                Icon(Icons.more_vert),
                SizedBox(width: 16),
              ],
            ),
          ),
        ),
        _buildWidgetCard(
          'BottomNavigationBar',
          'Navigation bar at the bottom of the screen',
          Container(
            height: 60,
            decoration: BoxDecoration(
              color: Theme.of(context).colorScheme.surface,
              borderRadius: BorderRadius.circular(8),
            ),
            child: const Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                Column(
                  mainAxisSize: MainAxisSize.min,
                  children: [Icon(Icons.home), Text('Home')],
                ),
                Column(
                  mainAxisSize: MainAxisSize.min,
                  children: [Icon(Icons.search), Text('Search')],
                ),
                Column(
                  mainAxisSize: MainAxisSize.min,
                  children: [Icon(Icons.person), Text('Profile')],
                ),
              ],
            ),
          ),
        ),
      ],
    );
  }

  Widget _buildAdvancedWidgetsTab() {
    return ListView(
      padding: const EdgeInsets.all(16),
      children: [
        _buildWidgetCard(
          'ListView',
          'Scrollable list of widgets',
          SizedBox(
            height: 150,
            child: ListView.builder(
              itemCount: 5,
              itemBuilder: (context, index) {
                return ListTile(
                  leading: CircleAvatar(
                    backgroundColor: Theme.of(context).colorScheme.primary,
                    child: Text('${index + 1}'),
                  ),
                  title: Text('List Item ${index + 1}'),
                  subtitle: Text('Subtitle for item ${index + 1}'),
                );
              },
            ),
          ),
        ),
        _buildWidgetCard(
          'GridView',
          'Grid layout for displaying items',
          SizedBox(
            height: 150,
            child: GridView.count(
              crossAxisCount: 3,
              children: List.generate(
                6,
                (index) => Container(
                  margin: const EdgeInsets.all(4),
                  decoration: BoxDecoration(
                    color: Theme.of(context).colorScheme.primary.withValues(alpha: 0.7),
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: Center(
                    child: Text(
                      '${index + 1}',
                      style: const TextStyle(color: Colors.white, fontWeight: FontWeight.bold),
                    ),
                  ),
                ),
              ),
            ),
          ),
        ),
        _buildWidgetCard(
          'TabBar & TabBarView',
          'Tabbed interface for organizing content',
          DefaultTabController(
            length: 3,
            child: Column(
              children: [
                Container(
                  height: 40,
                  decoration: BoxDecoration(
                    color: Theme.of(context).colorScheme.surface,
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: const TabBar(
                    tabs: [
                      Tab(text: 'Tab 1'),
                      Tab(text: 'Tab 2'),
                      Tab(text: 'Tab 3'),
                    ],
                  ),
                ),
                SizedBox(
                  height: 80,
                  child: TabBarView(
                    children: [
                      const Center(child: Text('Content 1')),
                      const Center(child: Text('Content 2')),
                      const Center(child: Text('Content 3')),
                    ],
                  ),
                ),
              ],
            ),
          ),
        ),
      ],
    );
  }

  Widget _buildWidgetCard(String title, String description, Widget demoWidget) {
    return Card(
      margin: const EdgeInsets.only(bottom: 16),
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              title,
              style: const TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 4),
            Text(
              description,
              style: TextStyle(
                color: Theme.of(context).colorScheme.onSurface.withValues(alpha: 0.7),
                fontSize: 14,
              ),
            ),
            const SizedBox(height: 16),
            Center(child: demoWidget),
          ],
        ),
      ),
    );
  }
}
```

