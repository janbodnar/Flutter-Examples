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


