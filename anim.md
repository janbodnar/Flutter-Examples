# Flutter Animation Examples

Flutter animations bring your UI to life with smooth transitions and visual  
effects. This guide covers 25 essential animation patterns from basic to  
advanced techniques.  

## Fade animation

Simple fade in/out effect using AnimatedOpacity widget.  

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
      title: 'Fade Animation',
      theme: ThemeData.dark(),
      home: const FadeAnimationPage(),
    );
  }
}

class FadeAnimationPage extends StatefulWidget {
  const FadeAnimationPage({super.key});

  @override
  State<FadeAnimationPage> createState() => _FadeAnimationPageState();
}

class _FadeAnimationPageState extends State<FadeAnimationPage> {
  bool _isVisible = true;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Fade Animation'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedOpacity(
              opacity: _isVisible ? 1.0 : 0.0,
              duration: const Duration(seconds: 1),
              child: const Text(
                'Hello World!',
                style: TextStyle(fontSize: 24),
              ),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                setState(() {
                  _isVisible = !_isVisible;
                });
              },
              child: const Text('Toggle Visibility'),
            ),
          ],
        ),
      ),
    );
  }
}
```

The AnimatedOpacity widget automatically animates opacity changes over the  
specified duration. When _isVisible changes, the text smoothly fades in or  
out. This is perfect for creating subtle entrance and exit effects.  

## Scale animation

Animates widget size using AnimatedScale widget.  

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
      title: 'Scale Animation',
      theme: ThemeData.dark(),
      home: const ScaleAnimationPage(),
    );
  }
}

class ScaleAnimationPage extends StatefulWidget {
  const ScaleAnimationPage({super.key});

  @override
  State<ScaleAnimationPage> createState() => _ScaleAnimationPageState();
}

class _ScaleAnimationPageState extends State<ScaleAnimationPage> {
  double _scale = 1.0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Scale Animation'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedScale(
              scale: _scale,
              duration: const Duration(milliseconds: 500),
              child: Container(
                width: 100,
                height: 100,
                decoration: BoxDecoration(
                  color: Colors.blue,
                  borderRadius: BorderRadius.circular(8),
                ),
              ),
            ),
            const SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _scale = 0.5;
                    });
                  },
                  child: const Text('Scale Down'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _scale = 1.5;
                    });
                  },
                  child: const Text('Scale Up'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _scale = 1.0;
                    });
                  },
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
```

AnimatedScale provides smooth scaling transitions between different sizes.  
The scale property accepts values where 1.0 is normal size, values less than  
1.0 shrink the widget, and values greater than 1.0 enlarge it. This creates  
engaging button press effects and attention-grabbing animations.  

## Rotation animation

Rotates widgets using AnimatedRotation widget.  

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
      title: 'Rotation Animation',
      theme: ThemeData.dark(),
      home: const RotationAnimationPage(),
    );
  }
}

class RotationAnimationPage extends StatefulWidget {
  const RotationAnimationPage({super.key});

  @override
  State<RotationAnimationPage> createState() => _RotationAnimationPageState();
}

class _RotationAnimationPageState extends State<RotationAnimationPage> {
  double _turns = 0.0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Rotation Animation'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedRotation(
              turns: _turns,
              duration: const Duration(seconds: 1),
              child: const Icon(
                Icons.star,
                size: 100,
                color: Colors.amber,
              ),
            ),
            const SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _turns += 0.25; // 90 degrees
                    });
                  },
                  child: const Text('Rotate 90°'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _turns += 1.0; // 360 degrees
                    });
                  },
                  child: const Text('Full Rotation'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _turns = 0.0;
                    });
                  },
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
```

AnimatedRotation spins widgets smoothly around their center point. The turns  
property specifies rotation where 1.0 equals a full 360-degree rotation.  
Fractional values create partial rotations, making it perfect for loading  
indicators, interactive elements, and visual feedback.  

## Slide animation

Slides widgets in and out using AnimatedSlide widget.  

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
      title: 'Slide Animation',
      theme: ThemeData.dark(),
      home: const SlideAnimationPage(),
    );
  }
}

class SlideAnimationPage extends StatefulWidget {
  const SlideAnimationPage({super.key});

  @override
  State<SlideAnimationPage> createState() => _SlideAnimationPageState();
}

class _SlideAnimationPageState extends State<SlideAnimationPage> {
  Offset _offset = const Offset(0, 0);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Slide Animation'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedSlide(
              offset: _offset,
              duration: const Duration(milliseconds: 800),
              child: Container(
                width: 100,
                height: 100,
                decoration: BoxDecoration(
                  color: Colors.green,
                  borderRadius: BorderRadius.circular(8),
                ),
                child: const Center(
                  child: Text(
                    'Box',
                    style: TextStyle(color: Colors.white, fontSize: 16),
                  ),
                ),
              ),
            ),
            const SizedBox(height: 40),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _offset = const Offset(-1, 0);
                    });
                  },
                  child: const Text('Left'),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _offset = const Offset(1, 0);
                    });
                  },
                  child: const Text('Right'),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _offset = const Offset(0, -1);
                    });
                  },
                  child: const Text('Up'),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _offset = const Offset(0, 1);
                    });
                  },
                  child: const Text('Down'),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _offset = const Offset(0, 0);
                    });
                  },
                  child: const Text('Center'),
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

AnimatedSlide moves widgets along X and Y axes relative to their size. Offset  
values are multiplied by the widget's dimensions - (1,0) moves right by the  
widget's width, (-1,0) moves left, (0,1) moves down by height, and (0,-1)  
moves up. This creates smooth directional movement animations.  

## AnimatedContainer

Animates multiple properties simultaneously with AnimatedContainer.  

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
      title: 'AnimatedContainer Demo',
      theme: ThemeData.dark(),
      home: const AnimatedContainerPage(),
    );
  }
}

class AnimatedContainerPage extends StatefulWidget {
  const AnimatedContainerPage({super.key});

  @override
  State<AnimatedContainerPage> createState() => _AnimatedContainerPageState();
}

class _AnimatedContainerPageState extends State<AnimatedContainerPage> {
  double _width = 100;
  double _height = 100;
  Color _color = Colors.blue;
  double _borderRadius = 8;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimatedContainer'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedContainer(
              width: _width,
              height: _height,
              decoration: BoxDecoration(
                color: _color,
                borderRadius: BorderRadius.circular(_borderRadius),
              ),
              duration: const Duration(seconds: 1),
              child: const Center(
                child: Text(
                  'Box',
                  style: TextStyle(color: Colors.white, fontSize: 16),
                ),
              ),
            ),
            const SizedBox(height: 30),
            Wrap(
              spacing: 8,
              runSpacing: 8,
              children: [
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _width = _width == 100 ? 200 : 100;
                      _height = _height == 100 ? 150 : 100;
                    });
                  },
                  child: const Text('Resize'),
                ),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _color = _color == Colors.blue ? Colors.red : Colors.blue;
                    });
                  },
                  child: const Text('Color'),
                ),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _borderRadius = _borderRadius == 8 ? 50 : 8;
                    });
                  },
                  child: const Text('Corner'),
                ),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _width = 100;
                      _height = 100;
                      _color = Colors.blue;
                      _borderRadius = 8;
                    });
                  },
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
```

AnimatedContainer is the Swiss Army knife of Flutter animations. It  
automatically animates changes to width, height, color, padding, margin,  
decoration, and more. When any property changes, it smoothly transitions to  
the new value, making complex multi-property animations effortless.  

## AnimatedPositioned

Animates widget position changes within a Stack using AnimatedPositioned.  

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
      title: 'AnimatedPositioned Demo',
      theme: ThemeData.dark(),
      home: const AnimatedPositionedPage(),
    );
  }
}

class AnimatedPositionedPage extends StatefulWidget {
  const AnimatedPositionedPage({super.key});

  @override
  State<AnimatedPositionedPage> createState() => _AnimatedPositionedPageState();
}

class _AnimatedPositionedPageState extends State<AnimatedPositionedPage> {
  bool _moved = false;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimatedPositioned'),
      ),
      body: Container(
        width: double.infinity,
        height: double.infinity,
        color: Colors.grey[900],
        child: Stack(
          children: [
            AnimatedPositioned(
              top: _moved ? 100 : 200,
              left: _moved ? 50 : 150,
              duration: const Duration(milliseconds: 800),
              child: Container(
                width: 80,
                height: 80,
                decoration: BoxDecoration(
                  color: Colors.orange,
                  borderRadius: BorderRadius.circular(8),
                ),
                child: const Center(
                  child: Icon(Icons.star, color: Colors.white, size: 30),
                ),
              ),
            ),
            Positioned(
              bottom: 50,
              left: 0,
              right: 0,
              child: Center(
                child: ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _moved = !_moved;
                    });
                  },
                  child: Text(_moved ? 'Move Back' : 'Move Forward'),
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

AnimatedPositioned smoothly moves widgets within a Stack layout. It animates  
changes to top, left, right, bottom, width, and height properties. This is  
ideal for creating floating elements, repositioning UI components, and  
building interactive layouts with smooth position transitions.  

## Bounce animation with curves

Demonstrates different animation curves for various effects.  

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
      title: 'Animation Curves',
      theme: ThemeData.dark(),
      home: const CurveAnimationPage(),
    );
  }
}

class CurveAnimationPage extends StatefulWidget {
  const CurveAnimationPage({super.key});

  @override
  State<CurveAnimationPage> createState() => _CurveAnimationPageState();
}

class _CurveAnimationPageState extends State<CurveAnimationPage> {
  bool _animate = false;
  Curve _selectedCurve = Curves.bounceOut;

  final List<MapEntry<String, Curve>> curves = [
    const MapEntry('Bounce Out', Curves.bounceOut),
    const MapEntry('Elastic Out', Curves.elasticOut),
    const MapEntry('Ease In Out', Curves.easeInOut),
    const MapEntry('Fast Out Slow In', Curves.fastOutSlowIn),
    const MapEntry('Decelerate', Curves.decelerate),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Animation Curves'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedScale(
              scale: _animate ? 1.5 : 1.0,
              duration: const Duration(milliseconds: 1200),
              curve: _selectedCurve,
              child: Container(
                width: 100,
                height: 100,
                decoration: BoxDecoration(
                  color: Colors.purple,
                  borderRadius: BorderRadius.circular(8),
                ),
                child: const Center(
                  child: Icon(Icons.favorite, color: Colors.white, size: 30),
                ),
              ),
            ),
            const SizedBox(height: 30),
            Text('Curve: ${curves.firstWhere((e) => e.value == _selectedCurve).key}'),
            const SizedBox(height: 20),
            Wrap(
              spacing: 8,
              children: curves.map((curve) {
                return ElevatedButton(
                  onPressed: () {
                    setState(() {
                      _selectedCurve = curve.value;
                    });
                  },
                  child: Text(curve.key),
                );
              }).toList(),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                setState(() {
                  _animate = !_animate;
                });
              },
              child: const Text('Animate'),
            ),
          ],
        ),
      ),
    );
  }
}
```

Animation curves control the rate of change during an animation. Curves.bounceOut  
creates a bouncing effect, Curves.elasticOut adds spring-like motion,  
Curves.easeInOut provides smooth acceleration and deceleration, and many more  
curves create different feels and personalities for your animations.  

## Color transition

Animates between different colors using AnimatedContainer.  

```dart
import 'package:flutter/material.dart';
import 'dart:math';
import 'dart:async';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Color Transition',
      theme: ThemeData.dark(),
      home: const ColorTransitionPage(),
    );
  }
}

class ColorTransitionPage extends StatefulWidget {
  const ColorTransitionPage({super.key});

  @override
  State<ColorTransitionPage> createState() => _ColorTransitionPageState();
}

class _ColorTransitionPageState extends State<ColorTransitionPage> {
  Color _currentColor = Colors.blue;
  final Random _random = Random();

  final List<Color> _colors = [
    Colors.red,
    Colors.green,
    Colors.blue,
    Colors.orange,
    Colors.purple,
    Colors.teal,
    Colors.pink,
    Colors.amber,
    Colors.indigo,
    Colors.cyan,
  ];

  void _changeColor() {
    setState(() {
      _currentColor = _colors[_random.nextInt(_colors.length)];
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Color Transition'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedContainer(
              width: 200,
              height: 200,
              decoration: BoxDecoration(
                color: _currentColor,
                borderRadius: BorderRadius.circular(100),
                boxShadow: [
                  BoxShadow(
                    color: _currentColor.withOpacity(0.3),
                    blurRadius: 20,
                    spreadRadius: 5,
                  ),
                ],
              ),
              duration: const Duration(milliseconds: 600),
              child: Center(
                child: Icon(
                  Icons.palette,
                  size: 60,
                  color: Colors.white,
                ),
              ),
            ),
            const SizedBox(height: 30),
            ElevatedButton(
              onPressed: _changeColor,
              child: const Text('Change Color'),
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              onPressed: () {
                Timer.periodic(const Duration(milliseconds: 500), (timer) {
                  if (mounted) {
                    _changeColor();
                  } else {
                    timer.cancel();
                  }
                });
              },
              child: const Text('Auto Change'),
            ),
          ],
        ),
      ),
    );
  }
}
```

Color transitions create vibrant, engaging animations. AnimatedContainer  
smoothly blends between colors using the specified duration. The example also  
demonstrates animated box shadows, which change color along with the container,  
creating a glowing effect that enhances the visual impact.  

## AnimationController basics

Using AnimationController for precise control over animations.  

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
      title: 'AnimationController Demo',
      theme: ThemeData.dark(),
      home: const AnimationControllerPage(),
    );
  }
}

class AnimationControllerPage extends StatefulWidget {
  const AnimationControllerPage({super.key});

  @override
  State<AnimationControllerPage> createState() => _AnimationControllerPageState();
}

class _AnimationControllerPageState extends State<AnimationControllerPage>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );
    _animation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    ));
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AnimationController'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedBuilder(
              animation: _animation,
              builder: (context, child) {
                return Transform.rotate(
                  angle: _animation.value * 2 * 3.14159, // 2π for full rotation
                  child: Opacity(
                    opacity: _animation.value,
                    child: Container(
                      width: 100 + (_animation.value * 50),
                      height: 100 + (_animation.value * 50),
                      decoration: BoxDecoration(
                        color: Colors.green,
                        borderRadius: BorderRadius.circular(8),
                      ),
                      child: const Center(
                        child: Icon(Icons.star, color: Colors.white, size: 30),
                      ),
                    ),
                  ),
                );
              },
            ),
            const SizedBox(height: 30),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () => _controller.forward(),
                  child: const Text('Forward'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () => _controller.reverse(),
                  child: const Text('Reverse'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () => _controller.repeat(),
                  child: const Text('Repeat'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () => _controller.reset(),
                  child: const Text('Reset'),
                ),
              ],
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              onPressed: () => _controller.stop(),
              child: const Text('Stop'),
            ),
          ],
        ),
      ),
    );
  }
}
```

AnimationController provides fine-grained control over animations. It manages  
animation timing, direction, and state. Combined with AnimatedBuilder, you can  
create complex animations that respond to multiple properties simultaneously.  
The controller offers methods like forward(), reverse(), repeat(), and reset().  

## Tween animations

Creating smooth value interpolation using Tween animations.  

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
      title: 'Tween Animations',
      theme: ThemeData.dark(),
      home: const TweenAnimationPage(),
    );
  }
}

class TweenAnimationPage extends StatefulWidget {
  const TweenAnimationPage({super.key});

  @override
  State<TweenAnimationPage> createState() => _TweenAnimationPageState();
}

class _TweenAnimationPageState extends State<TweenAnimationPage>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _sizeAnimation;
  late Animation<Color?> _colorAnimation;
  late Animation<double> _rotationAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 3),
      vsync: this,
    );

    _sizeAnimation = Tween<double>(
      begin: 50,
      end: 150,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: const Interval(0.0, 0.5, curve: Curves.easeOut),
    ));

    _colorAnimation = ColorTween(
      begin: Colors.red,
      end: Colors.blue,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: const Interval(0.2, 0.8, curve: Curves.linear),
    ));

    _rotationAnimation = Tween<double>(
      begin: 0,
      end: 2,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: const Interval(0.5, 1.0, curve: Curves.elasticOut),
    ));
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Tween Animations'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedBuilder(
              animation: _controller,
              builder: (context, child) {
                return Transform.rotate(
                  angle: _rotationAnimation.value * 3.14159,
                  child: Container(
                    width: _sizeAnimation.value,
                    height: _sizeAnimation.value,
                    decoration: BoxDecoration(
                      color: _colorAnimation.value ?? Colors.red,
                      borderRadius: BorderRadius.circular(8),
                    ),
                    child: const Center(
                      child: Icon(Icons.star, color: Colors.white, size: 30),
                    ),
                  ),
                );
              },
            ),
            const SizedBox(height: 30),
            LinearProgressIndicator(
              value: _controller.value,
              backgroundColor: Colors.grey[600],
              valueColor: const AlwaysStoppedAnimation<Color>(Colors.blue),
            ),
            const SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () => _controller.forward(),
                  child: const Text('Start'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () => _controller.reverse(),
                  child: const Text('Reverse'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () => _controller.reset(),
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
```

Tween animations interpolate between start and end values over time. Different  
Tweens handle different data types: Tween<double> for numbers, ColorTween for  
colors. The Interval class allows animations to occur during specific portions  
of the overall duration, creating staggered effects within a single controller.  

## Staggered animations

Creating sequential animations that follow each other.  

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
      title: 'Staggered Animations',
      theme: ThemeData.dark(),
      home: const StaggeredAnimationPage(),
    );
  }
}

class StaggeredAnimationPage extends StatefulWidget {
  const StaggeredAnimationPage({super.key});

  @override
  State<StaggeredAnimationPage> createState() => _StaggeredAnimationPageState();
}

class _StaggeredAnimationPageState extends State<StaggeredAnimationPage>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late List<Animation<double>> _animations;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );

    _animations = List.generate(5, (index) {
      final start = index * 0.1;
      final end = start + 0.3;
      return Tween<double>(
        begin: 0,
        end: 1,
      ).animate(CurvedAnimation(
        parent: _controller,
        curve: Interval(start, end.clamp(0.0, 1.0), curve: Curves.easeOut),
      ));
    });
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Staggered Animations'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedBuilder(
              animation: _controller,
              builder: (context, child) {
                return Row(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: _animations.asMap().entries.map((entry) {
                    final index = entry.key;
                    final animation = entry.value;
                    final colors = [
                      Colors.red,
                      Colors.orange,
                      Colors.yellow,
                      Colors.green,
                      Colors.blue,
                    ];

                    return Padding(
                      padding: const EdgeInsets.symmetric(horizontal: 4),
                      child: Transform.scale(
                        scale: animation.value,
                        child: Opacity(
                          opacity: animation.value,
                          child: Container(
                            width: 50,
                            height: 50,
                            decoration: BoxDecoration(
                              color: colors[index],
                              borderRadius: BorderRadius.circular(8),
                            ),
                            child: Center(
                              child: Text(
                                '${index + 1}',
                                style: const TextStyle(
                                  color: Colors.white,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                            ),
                          ),
                        ),
                      ),
                    );
                  }).toList(),
                );
              },
            ),
            const SizedBox(height: 30),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () => _controller.forward(),
                  child: const Text('Animate In'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () => _controller.reverse(),
                  child: const Text('Animate Out'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () => _controller.reset(),
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
```

Staggered animations create cascading effects where elements animate sequentially  
rather than simultaneously. Using Interval with different start and end points  
for each animation creates a wave-like effect. This technique is perfect for  
list item animations, loading sequences, and creating visual rhythm.  

## Hero animation

Smooth transitions between screens using Hero widgets.  

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
      title: 'Hero Animation',
      theme: ThemeData.dark(),
      home: const HeroAnimationPage(),
    );
  }
}

class HeroAnimationPage extends StatelessWidget {
  const HeroAnimationPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Hero Animation'),
      ),
      body: GridView.builder(
        padding: const EdgeInsets.all(16),
        gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 2,
          crossAxisSpacing: 16,
          mainAxisSpacing: 16,
        ),
        itemCount: 6,
        itemBuilder: (context, index) {
          final colors = [
            Colors.red,
            Colors.green,
            Colors.blue,
            Colors.orange,
            Colors.purple,
            Colors.teal,
          ];
          final icons = [
            Icons.star,
            Icons.favorite,
            Icons.home,
            Icons.work,
            Icons.school,
            Icons.restaurant,
          ];

          return GestureDetector(
            onTap: () {
              Navigator.of(context).push(
                MaterialPageRoute(
                  builder: (context) => DetailPage(
                    heroTag: 'hero-$index',
                    color: colors[index],
                    icon: icons[index],
                    title: 'Item ${index + 1}',
                  ),
                ),
              );
            },
            child: Hero(
              tag: 'hero-$index',
              child: Container(
                decoration: BoxDecoration(
                  color: colors[index],
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Center(
                  child: Icon(icons[index], color: Colors.white, size: 50),
                ),
              ),
            ),
          );
        },
      ),
    );
  }
}

class DetailPage extends StatelessWidget {
  final String heroTag;
  final Color color;
  final IconData icon;
  final String title;

  const DetailPage({
    super.key,
    required this.heroTag,
    required this.color,
    required this.icon,
    required this.title,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Center(
        child: Hero(
          tag: heroTag,
          child: Material(
            color: color,
            borderRadius: BorderRadius.circular(16),
            child: Container(
              width: 200,
              height: 200,
              decoration: BoxDecoration(
                color: color,
                borderRadius: BorderRadius.circular(16),
              ),
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Icon(icon, color: Colors.white, size: 80),
                  const SizedBox(height: 16),
                  Text(
                    title,
                    style: const TextStyle(
                      color: Colors.white,
                      fontSize: 24,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
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

Hero animations create seamless transitions between screens by animating a  
shared element. The Hero widget with matching tags on both screens automatically  
handles the transition. The element smoothly transforms from its position and  
size on the first screen to its position and size on the second screen.  

## Loading spinner

Custom rotating loading animation using AnimationController.  

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
      title: 'Loading Spinner',
      theme: ThemeData.dark(),
      home: const LoadingSpinnerPage(),
    );
  }
}

class LoadingSpinnerPage extends StatelessWidget {
  const LoadingSpinnerPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Loading Spinners'),
      ),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            CustomSpinner(
              size: 60,
              color: Colors.blue,
              strokeWidth: 6,
            ),
            PulsingDots(),
            RotatingSquares(),
          ],
        ),
      ),
    );
  }
}

class CustomSpinner extends StatefulWidget {
  final double size;
  final Color color;
  final double strokeWidth;

  const CustomSpinner({
    super.key,
    required this.size,
    required this.color,
    required this.strokeWidth,
  });

  @override
  State<CustomSpinner> createState() => _CustomSpinnerState();
}

class _CustomSpinnerState extends State<CustomSpinner>
    with TickerProviderStateMixin {
  late AnimationController _controller;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 1),
      vsync: this,
    )..repeat();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _controller,
      builder: (context, child) {
        return Transform.rotate(
          angle: _controller.value * 2 * 3.14159,
          child: CustomPaint(
            size: Size(widget.size, widget.size),
            painter: SpinnerPainter(
              color: widget.color,
              strokeWidth: widget.strokeWidth,
            ),
          ),
        );
      },
    );
  }
}

class SpinnerPainter extends CustomPainter {
  final Color color;
  final double strokeWidth;

  SpinnerPainter({required this.color, required this.strokeWidth});

  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()
      ..color = color
      ..strokeWidth = strokeWidth
      ..style = PaintingStyle.stroke
      ..strokeCap = StrokeCap.round;

    final center = Offset(size.width / 2, size.height / 2);
    final radius = size.width / 2 - strokeWidth / 2;

    canvas.drawArc(
      Rect.fromCircle(center: center, radius: radius),
      0,
      3.14159 * 1.5,
      false,
      paint,
    );
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => false;
}

class PulsingDots extends StatefulWidget {
  const PulsingDots({super.key});

  @override
  State<PulsingDots> createState() => _PulsingDotsState();
}

class _PulsingDotsState extends State<PulsingDots>
    with TickerProviderStateMixin {
  late List<AnimationController> _controllers;

  @override
  void initState() {
    super.initState();
    _controllers = List.generate(3, (index) {
      final controller = AnimationController(
        duration: const Duration(milliseconds: 600),
        vsync: this,
      );
      Future.delayed(Duration(milliseconds: index * 200), () {
        if (mounted) controller.repeat(reverse: true);
      });
      return controller;
    });
  }

  @override
  void dispose() {
    for (final controller in _controllers) {
      controller.dispose();
    }
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: _controllers.asMap().entries.map((entry) {
        final controller = entry.value;
        return AnimatedBuilder(
          animation: controller,
          builder: (context, child) {
            return Container(
              margin: const EdgeInsets.symmetric(horizontal: 4),
              child: Transform.scale(
                scale: 0.5 + (controller.value * 0.5),
                child: Container(
                  width: 16,
                  height: 16,
                  decoration: const BoxDecoration(
                    color: Colors.orange,
                    shape: BoxShape.circle,
                  ),
                ),
              ),
            );
          },
        );
      }).toList(),
    );
  }
}

class RotatingSquares extends StatefulWidget {
  const RotatingSquares({super.key});

  @override
  State<RotatingSquares> createState() => _RotatingSquaresState();
}

class _RotatingSquaresState extends State<RotatingSquares>
    with TickerProviderStateMixin {
  late AnimationController _controller;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    )..repeat();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _controller,
      builder: (context, child) {
        return Stack(
          alignment: Alignment.center,
          children: [
            Transform.rotate(
              angle: _controller.value * 2 * 3.14159,
              child: Container(
                width: 40,
                height: 40,
                decoration: BoxDecoration(
                  color: Colors.green.withOpacity(0.7),
                  borderRadius: BorderRadius.circular(4),
                ),
              ),
            ),
            Transform.rotate(
              angle: -_controller.value * 2 * 3.14159,
              child: Container(
                width: 30,
                height: 30,
                decoration: BoxDecoration(
                  color: Colors.red.withOpacity(0.7),
                  borderRadius: BorderRadius.circular(4),
                ),
              ),
            ),
          ],
        );
      },
    );
  }
}
```

Loading spinners provide visual feedback during data loading or processing.  
Custom spinners using AnimationController and CustomPainter offer full control  
over appearance. The examples show different approaches: rotating arcs, pulsing  
dots with staggered timing, and counter-rotating squares for complex effects.  

## Physics-based spring animation

Natural motion using SpringSimulation physics.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter/physics.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Spring Animation',
      theme: ThemeData.dark(),
      home: const SpringAnimationPage(),
    );
  }
}

class SpringAnimationPage extends StatefulWidget {
  const SpringAnimationPage({super.key});

  @override
  State<SpringAnimationPage> createState() => _SpringAnimationPageState();
}

class _SpringAnimationPageState extends State<SpringAnimationPage>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late SpringSimulation _springSimulation;
  double _dragPosition = 0.0;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      vsync: this,
      lowerBound: -200,
      upperBound: 200,
    );

    _springSimulation = SpringSimulation(
      const SpringDescription(
        mass: 1,
        stiffness: 500,
        damping: 15,
      ),
      0, // Starting position
      0, // Ending position
      0, // Initial velocity
    );
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  void _onPanUpdate(DragUpdateDetails details) {
    setState(() {
      _dragPosition += details.delta.dy;
      _dragPosition = _dragPosition.clamp(-200.0, 200.0);
    });
    _controller.value = _dragPosition;
  }

  void _onPanEnd(DragEndDetails details) {
    _controller.animateWith(_springSimulation);
    _dragPosition = 0.0;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Spring Animation'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'Drag the box up or down',
              style: TextStyle(fontSize: 18),
            ),
            const SizedBox(height: 50),
            AnimatedBuilder(
              animation: _controller,
              builder: (context, child) {
                return Transform.translate(
                  offset: Offset(0, _controller.value),
                  child: GestureDetector(
                    onPanUpdate: _onPanUpdate,
                    onPanEnd: _onPanEnd,
                    child: Container(
                      width: 100,
                      height: 100,
                      decoration: BoxDecoration(
                        color: Colors.cyan,
                        borderRadius: BorderRadius.circular(8),
                        boxShadow: [
                          BoxShadow(
                            color: Colors.cyan.withOpacity(0.3),
                            blurRadius: 10,
                            spreadRadius: 2,
                          ),
                        ],
                      ),
                      child: const Center(
                        child: Icon(Icons.touch_app, color: Colors.white, size: 30),
                      ),
                    ),
                  ),
                );
              },
            ),
            const SizedBox(height: 50),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () {
                    _controller.animateWith(SpringSimulation(
                      const SpringDescription(mass: 1, stiffness: 300, damping: 10),
                      _controller.value,
                      100,
                      0,
                    ));
                  },
                  child: const Text('Bounce Down'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () {
                    _controller.animateWith(SpringSimulation(
                      const SpringDescription(mass: 1, stiffness: 300, damping: 10),
                      _controller.value,
                      -100,
                      0,
                    ));
                  },
                  child: const Text('Bounce Up'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () {
                    _controller.animateWith(_springSimulation);
                  },
                  child: const Text('Return'),
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

SpringSimulation creates natural, physics-based motion that mimics real-world  
spring behavior. The SpringDescription defines mass, stiffness, and damping  
properties that control how the animation feels. Lower damping creates more  
bouncy motion, while higher stiffness makes the spring more responsive.  

## Particle system animation

Creating particle effects using custom animation.  

```dart
import 'package:flutter/material.dart';
import 'dart:math';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Particle System',
      theme: ThemeData.dark(),
      home: const ParticleSystemPage(),
    );
  }
}

class ParticleSystemPage extends StatefulWidget {
  const ParticleSystemPage({super.key});

  @override
  State<ParticleSystemPage> createState() => _ParticleSystemPageState();
}

class _ParticleSystemPageState extends State<ParticleSystemPage>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  List<Particle> particles = [];
  final Random _random = Random();

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 3),
      vsync: this,
    );
    _generateParticles();
  }

  void _generateParticles() {
    particles.clear();
    for (int i = 0; i < 20; i++) {
      particles.add(
        Particle(
          x: 200,
          y: 300,
          vx: (_random.nextDouble() - 0.5) * 200,
          vy: -_random.nextDouble() * 300 - 100,
          color: Color.fromRGBO(
            100 + _random.nextInt(155),
            100 + _random.nextInt(155),
            100 + _random.nextInt(155),
            1,
          ),
          size: 4 + _random.nextDouble() * 8,
          life: 1.0,
        ),
      );
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Particle System'),
      ),
      body: Center(
        child: Column(
          children: [
            Container(
              width: 400,
              height: 400,
              decoration: BoxDecoration(
                color: Colors.black,
                border: Border.all(color: Colors.white30),
              ),
              child: AnimatedBuilder(
                animation: _controller,
                builder: (context, child) {
                  return CustomPaint(
                    painter: ParticlePainter(particles, _controller.value),
                    size: const Size(400, 400),
                  );
                },
              ),
            ),
            const SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () {
                    _generateParticles();
                    _controller.reset();
                    _controller.forward();
                  },
                  child: const Text('Explode'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () {
                    _controller.repeat();
                  },
                  child: const Text('Repeat'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () {
                    _controller.stop();
                  },
                  child: const Text('Stop'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}

class Particle {
  double x, y;
  double vx, vy;
  Color color;
  double size;
  double life;

  Particle({
    required this.x,
    required this.y,
    required this.vx,
    required this.vy,
    required this.color,
    required this.size,
    required this.life,
  });
}

class ParticlePainter extends CustomPainter {
  final List<Particle> particles;
  final double progress;

  ParticlePainter(this.particles, this.progress);

  @override
  void paint(Canvas canvas, Size size) {
    for (final particle in particles) {
      final currentX = particle.x + particle.vx * progress;
      final currentY = particle.y + particle.vy * progress + 150 * progress * progress;
      final currentLife = particle.life * (1 - progress);
      final currentSize = particle.size * currentLife;

      if (currentLife > 0 && currentX >= 0 && currentX <= size.width && currentY >= 0) {
        final paint = Paint()
          ..color = particle.color.withOpacity(currentLife.clamp(0.0, 1.0))
          ..style = PaintingStyle.fill;

        canvas.drawCircle(
          Offset(currentX, currentY),
          currentSize,
          paint,
        );
      }
    }
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}
```

Particle systems simulate complex effects like explosions, fire, or magic spells.  
Each particle has position, velocity, color, size, and life properties. The  
CustomPainter updates particle positions using physics equations, creating  
realistic motion with gravity effects and opacity fade-out over time.  

## Page transition animation

Custom page transitions with SharedAxisTransition-like effects.  

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
      title: 'Page Transitions',
      theme: ThemeData.dark(),
      home: const PageTransitionDemo(),
    );
  }
}

class PageTransitionDemo extends StatelessWidget {
  const PageTransitionDemo({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Page Transitions'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {
                Navigator.of(context).push(
                  SlideTransitionPageRoute(
                    child: const SecondPage(title: 'Slide Transition'),
                  ),
                );
              },
              child: const Text('Slide Transition'),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: () {
                Navigator.of(context).push(
                  FadeScaleTransitionPageRoute(
                    child: const SecondPage(title: 'Fade Scale Transition'),
                  ),
                );
              },
              child: const Text('Fade Scale Transition'),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: () {
                Navigator.of(context).push(
                  RotationTransitionPageRoute(
                    child: const SecondPage(title: 'Rotation Transition'),
                  ),
                );
              },
              child: const Text('Rotation Transition'),
            ),
          ],
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  final String title;

  const SecondPage({super.key, required this.title});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Container(
              width: 200,
              height: 200,
              decoration: BoxDecoration(
                color: Colors.purple,
                borderRadius: BorderRadius.circular(16),
              ),
              child: const Center(
                child: Icon(Icons.star, color: Colors.white, size: 80),
              ),
            ),
            const SizedBox(height: 20),
            Text(
              title,
              style: const TextStyle(fontSize: 24),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () => Navigator.of(context).pop(),
              child: const Text('Go Back'),
            ),
          ],
        ),
      ),
    );
  }
}

class SlideTransitionPageRoute<T> extends PageRouteBuilder<T> {
  final Widget child;

  SlideTransitionPageRoute({required this.child})
      : super(
          pageBuilder: (context, animation, secondaryAnimation) => child,
          transitionsBuilder: (context, animation, secondaryAnimation, child) {
            const begin = Offset(1.0, 0.0);
            const end = Offset.zero;
            const curve = Curves.easeInOut;

            var tween = Tween(begin: begin, end: end).chain(
              CurveTween(curve: curve),
            );

            return SlideTransition(
              position: animation.drive(tween),
              child: child,
            );
          },
        );
}

class FadeScaleTransitionPageRoute<T> extends PageRouteBuilder<T> {
  final Widget child;

  FadeScaleTransitionPageRoute({required this.child})
      : super(
          pageBuilder: (context, animation, secondaryAnimation) => child,
          transitionsBuilder: (context, animation, secondaryAnimation, child) {
            return FadeTransition(
              opacity: animation,
              child: ScaleTransition(
                scale: animation,
                child: child,
              ),
            );
          },
        );
}

class RotationTransitionPageRoute<T> extends PageRouteBuilder<T> {
  final Widget child;

  RotationTransitionPageRoute({required this.child})
      : super(
          pageBuilder: (context, animation, secondaryAnimation) => child,
          transitionsBuilder: (context, animation, secondaryAnimation, child) {
            return RotationTransition(
              turns: animation,
              child: FadeTransition(
                opacity: animation,
                child: child,
              ),
            );
          },
        );
}
```

Custom page transitions enhance navigation by replacing default slide animations.  
PageRouteBuilder allows complete control over how pages enter and exit. The  
transitionsBuilder function receives animation controllers for both entering  
and exiting pages, enabling sophisticated transition combinations.  

## Morphing shapes animation

Smooth shape transformations using CustomPainter and Tween.  

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
      title: 'Morphing Shapes',
      theme: ThemeData.dark(),
      home: const MorphingShapesPage(),
    );
  }
}

class MorphingShapesPage extends StatefulWidget {
  const MorphingShapesPage({super.key});

  @override
  State<MorphingShapesPage> createState() => _MorphingShapesPageState();
}

class _MorphingShapesPageState extends State<MorphingShapesPage>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _morphAnimation;
  int _shapeIndex = 0;

  final List<String> shapeNames = [
    'Circle',
    'Square',
    'Triangle',
    'Star',
    'Heart',
  ];

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );

    _morphAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    ));
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  void _nextShape() {
    if (_controller.isAnimating) return;

    setState(() {
      _shapeIndex = (_shapeIndex + 1) % shapeNames.length;
    });
    _controller.reset();
    _controller.forward();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Morphing Shapes'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedBuilder(
              animation: _morphAnimation,
              builder: (context, child) {
                return CustomPaint(
                  painter: MorphingShapePainter(
                    progress: _morphAnimation.value,
                    shapeIndex: _shapeIndex,
                  ),
                  size: const Size(200, 200),
                );
              },
            ),
            const SizedBox(height: 30),
            Text(
              'Current: ${shapeNames[_shapeIndex]}',
              style: const TextStyle(fontSize: 20),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _nextShape,
              child: const Text('Morph to Next Shape'),
            ),
          ],
        ),
      ),
    );
  }
}

class MorphingShapePainter extends CustomPainter {
  final double progress;
  final int shapeIndex;

  MorphingShapePainter({required this.progress, required this.shapeIndex});

  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()
      ..color = Color.lerp(Colors.blue, Colors.red, progress)!
      ..style = PaintingStyle.fill;

    final center = Offset(size.width / 2, size.height / 2);
    final radius = size.width / 3;

    canvas.translate(center.dx, center.dy);

    switch (shapeIndex) {
      case 0: // Circle
        _drawCircle(canvas, paint, radius, progress);
        break;
      case 1: // Square
        _drawSquare(canvas, paint, radius, progress);
        break;
      case 2: // Triangle
        _drawTriangle(canvas, paint, radius, progress);
        break;
      case 3: // Star
        _drawStar(canvas, paint, radius, progress);
        break;
      case 4: // Heart
        _drawHeart(canvas, paint, radius, progress);
        break;
    }
  }

  void _drawCircle(Canvas canvas, Paint paint, double radius, double progress) {
    final animatedRadius = radius * (0.5 + 0.5 * math.sin(progress * math.pi));
    canvas.drawCircle(Offset.zero, animatedRadius, paint);
  }

  void _drawSquare(Canvas canvas, Paint paint, double radius, double progress) {
    final size = radius * (0.5 + 0.5 * math.sin(progress * math.pi));
    final rect = Rect.fromCenter(center: Offset.zero, width: size * 2, height: size * 2);
    canvas.drawRect(rect, paint);
  }

  void _drawTriangle(Canvas canvas, Paint paint, double radius, double progress) {
    final animatedRadius = radius * (0.5 + 0.5 * math.sin(progress * math.pi));
    final path = Path();
    path.moveTo(0, -animatedRadius);
    path.lineTo(-animatedRadius * 0.866, animatedRadius * 0.5);
    path.lineTo(animatedRadius * 0.866, animatedRadius * 0.5);
    path.close();
    canvas.drawPath(path, paint);
  }

  void _drawStar(Canvas canvas, Paint paint, double radius, double progress) {
    final animatedRadius = radius * (0.5 + 0.5 * math.sin(progress * math.pi));
    final path = Path();
    const numPoints = 5;
    const step = math.pi / numPoints;

    for (int i = 0; i < numPoints * 2; i++) {
      final isOuter = i % 2 == 0;
      final r = isOuter ? animatedRadius : animatedRadius * 0.5;
      final x = r * math.cos(i * step - math.pi / 2);
      final y = r * math.sin(i * step - math.pi / 2);

      if (i == 0) {
        path.moveTo(x, y);
      } else {
        path.lineTo(x, y);
      }
    }
    path.close();
    canvas.drawPath(path, paint);
  }

  void _drawHeart(Canvas canvas, Paint paint, double radius, double progress) {
    final animatedRadius = radius * (0.5 + 0.5 * math.sin(progress * math.pi));
    final path = Path();
    
    final width = animatedRadius * 0.8;
    final height = animatedRadius * 0.9;

    path.moveTo(0, height * 0.3);
    
    path.cubicTo(-width * 0.5, -height * 0.1, -width, height * 0.1, -width * 0.5, height * 0.5);
    path.cubicTo(-width * 0.2, height * 0.7, 0, height, 0, height);
    path.cubicTo(0, height, width * 0.2, height * 0.7, width * 0.5, height * 0.5);
    path.cubicTo(width, height * 0.1, width * 0.5, -height * 0.1, 0, height * 0.3);
    
    canvas.drawPath(path, paint);
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}
```

Morphing shapes create mesmerizing transitions between different geometric forms.  
CustomPainter enables drawing complex shapes using paths and mathematical  
functions. Each shape animates its size based on sine waves, creating a  
pulsing effect while maintaining its distinct form.  

## Text animation effects

Animating text with typewriter and reveal effects.  

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
      title: 'Text Animations',
      theme: ThemeData.dark(),
      home: const TextAnimationPage(),
    );
  }
}

class TextAnimationPage extends StatelessWidget {
  const TextAnimationPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Text Animations'),
      ),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            TypewriterText(
              text: 'Flutter is amazing!',
              duration: Duration(milliseconds: 100),
            ),
            FadeInText(
              text: 'Smooth animations everywhere',
              duration: Duration(seconds: 2),
            ),
            ScaleInText(
              text: 'Scale & Bounce',
              duration: Duration(seconds: 1),
            ),
            SlideInText(
              text: 'Sliding text effect',
              duration: Duration(milliseconds: 800),
            ),
          ],
        ),
      ),
    );
  }
}

class TypewriterText extends StatefulWidget {
  final String text;
  final Duration duration;

  const TypewriterText({
    super.key,
    required this.text,
    required this.duration,
  });

  @override
  State<TypewriterText> createState() => _TypewriterTextState();
}

class _TypewriterTextState extends State<TypewriterText> {
  String _displayText = '';
  late Timer _timer;
  int _currentIndex = 0;

  @override
  void initState() {
    super.initState();
    _startTypewriter();
  }

  void _startTypewriter() {
    _timer = Timer.periodic(widget.duration, (timer) {
      if (_currentIndex < widget.text.length) {
        setState(() {
          _displayText += widget.text[_currentIndex];
          _currentIndex++;
        });
      } else {
        _timer.cancel();
        // Restart after a pause
        Timer(const Duration(seconds: 2), () {
          if (mounted) {
            setState(() {
              _displayText = '';
              _currentIndex = 0;
            });
            _startTypewriter();
          }
        });
      }
    });
  }

  @override
  void dispose() {
    _timer.cancel();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Text(
      _displayText + (_currentIndex < widget.text.length ? '|' : ''),
      style: const TextStyle(fontSize: 24, fontFamily: 'monospace'),
    );
  }
}

class FadeInText extends StatefulWidget {
  final String text;
  final Duration duration;

  const FadeInText({
    super.key,
    required this.text,
    required this.duration,
  });

  @override
  State<FadeInText> createState() => _FadeInTextState();
}

class _FadeInTextState extends State<FadeInText>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _fadeAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: widget.duration,
      vsync: this,
    );

    _fadeAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeIn,
    ));

    _controller.repeat(reverse: true);
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _fadeAnimation,
      builder: (context, child) {
        return Opacity(
          opacity: _fadeAnimation.value,
          child: Text(
            widget.text,
            style: const TextStyle(fontSize: 20, color: Colors.blue),
          ),
        );
      },
    );
  }
}

class ScaleInText extends StatefulWidget {
  final String text;
  final Duration duration;

  const ScaleInText({
    super.key,
    required this.text,
    required this.duration,
  });

  @override
  State<ScaleInText> createState() => _ScaleInTextState();
}

class _ScaleInTextState extends State<ScaleInText>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _scaleAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: widget.duration,
      vsync: this,
    );

    _scaleAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.elasticOut,
    ));

    _controller.repeat();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _scaleAnimation,
      builder: (context, child) {
        return Transform.scale(
          scale: _scaleAnimation.value,
          child: Text(
            widget.text,
            style: const TextStyle(fontSize: 22, color: Colors.orange),
          ),
        );
      },
    );
  }
}

class SlideInText extends StatefulWidget {
  final String text;
  final Duration duration;

  const SlideInText({
    super.key,
    required this.text,
    required this.duration,
  });

  @override
  State<SlideInText> createState() => _SlideInTextState();
}

class _SlideInTextState extends State<SlideInText>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<Offset> _slideAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: widget.duration,
      vsync: this,
    );

    _slideAnimation = Tween<Offset>(
      begin: const Offset(-1, 0),
      end: const Offset(1, 0),
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    ));

    _controller.repeat();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return SlideTransition(
      position: _slideAnimation,
      child: Text(
        widget.text,
        style: const TextStyle(fontSize: 18, color: Colors.green),
      ),
    );
  }
}
```

Text animations add personality and draw attention to important messages. The  
typewriter effect simulates typing with a blinking cursor, fade-in creates  
gentle appearance effects, scale animations with elastic curves create bouncy  
emphasis, and slide transitions move text across the screen smoothly.  

## Wave animation

Creating wave effects using sine functions and CustomPainter.  

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
      title: 'Wave Animation',
      theme: ThemeData.dark(),
      home: const WaveAnimationPage(),
    );
  }
}

class WaveAnimationPage extends StatefulWidget {
  const WaveAnimationPage({super.key});

  @override
  State<WaveAnimationPage> createState() => _WaveAnimationPageState();
}

class _WaveAnimationPageState extends State<WaveAnimationPage>
    with TickerProviderStateMixin {
  late AnimationController _waveController;

  @override
  void initState() {
    super.initState();
    _waveController = AnimationController(
      duration: const Duration(seconds: 3),
      vsync: this,
    )..repeat();
  }

  @override
  void dispose() {
    _waveController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Wave Animation'),
      ),
      body: Column(
        children: [
          Expanded(
            child: AnimatedBuilder(
              animation: _waveController,
              builder: (context, child) {
                return CustomPaint(
                  painter: WavePainter(_waveController.value),
                  size: Size.infinite,
                );
              },
            ),
          ),
          Container(
            padding: const EdgeInsets.all(16),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                ElevatedButton(
                  onPressed: () => _waveController.repeat(),
                  child: const Text('Start'),
                ),
                const SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () => _waveController.stop(),
                  child: const Text('Stop'),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}

class WavePainter extends CustomPainter {
  final double progress;

  WavePainter(this.progress);

  @override
  void paint(Canvas canvas, Size size) {
    final paint1 = Paint()
      ..color = Colors.blue.withOpacity(0.7)
      ..style = PaintingStyle.fill;

    final paint2 = Paint()
      ..color = Colors.cyan.withOpacity(0.5)
      ..style = PaintingStyle.fill;

    final paint3 = Paint()
      ..color = Colors.teal.withOpacity(0.3)
      ..style = PaintingStyle.fill;

    _drawWave(canvas, size, paint1, progress, 50, 20);
    _drawWave(canvas, size, paint2, progress + 0.3, 40, 15);
    _drawWave(canvas, size, paint3, progress + 0.6, 60, 25);
  }

  void _drawWave(Canvas canvas, Size size, Paint paint, double phase, 
                 double amplitude, double frequency) {
    final path = Path();
    final waveHeight = size.height * 0.7;
    
    path.moveTo(0, waveHeight);
    
    for (double x = 0; x <= size.width; x += 2) {
      final y = waveHeight + 
          amplitude * math.sin((x * frequency / size.width) + (phase * 2 * math.pi));
      path.lineTo(x, y);
    }
    
    path.lineTo(size.width, size.height);
    path.lineTo(0, size.height);
    path.close();
    
    canvas.drawPath(path, paint);
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}
```

Wave animations create flowing, organic motion using mathematical sine functions.  
Multiple waves with different frequencies, amplitudes, and phase offsets create  
complex, layered effects. This technique works well for water effects, audio  
visualizers, and background animations that need natural movement.  

## Ripple effect animation

Touch ripple effects using AnimationController and CustomPainter.  

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
      title: 'Ripple Effect',
      theme: ThemeData.dark(),
      home: const RippleEffectPage(),
    );
  }
}

class RippleEffectPage extends StatefulWidget {
  const RippleEffectPage({super.key});

  @override
  State<RippleEffectPage> createState() => _RippleEffectPageState();
}

class _RippleEffectPageState extends State<RippleEffectPage>
    with TickerProviderStateMixin {
  List<RippleAnimation> ripples = [];

  @override
  void dispose() {
    for (final ripple in ripples) {
      ripple.controller.dispose();
    }
    super.dispose();
  }

  void _addRipple(Offset position) {
    final controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );

    final ripple = RippleAnimation(
      controller: controller,
      position: position,
      color: Colors.blue.withOpacity(0.3),
    );

    setState(() {
      ripples.add(ripple);
    });

    controller.forward().then((_) {
      setState(() {
        ripples.remove(ripple);
      });
      controller.dispose();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Ripple Effect'),
      ),
      body: GestureDetector(
        onTapDown: (details) => _addRipple(details.localPosition),
        child: Container(
          width: double.infinity,
          height: double.infinity,
          color: Colors.black,
          child: Stack(
            children: [
              const Center(
                child: Text(
                  'Tap anywhere to create ripples',
                  style: TextStyle(fontSize: 18, color: Colors.white70),
                ),
              ),
              ...ripples.map((ripple) {
                return AnimatedBuilder(
                  animation: ripple.controller,
                  builder: (context, child) {
                    return CustomPaint(
                      painter: RipplePainter(
                        position: ripple.position,
                        progress: ripple.controller.value,
                        color: ripple.color,
                      ),
                      size: Size.infinite,
                    );
                  },
                );
              }).toList(),
            ],
          ),
        ),
      ),
    );
  }
}

class RippleAnimation {
  final AnimationController controller;
  final Offset position;
  final Color color;

  RippleAnimation({
    required this.controller,
    required this.position,
    required this.color,
  });
}

class RipplePainter extends CustomPainter {
  final Offset position;
  final double progress;
  final Color color;

  RipplePainter({
    required this.position,
    required this.progress,
    required this.color,
  });

  @override
  void paint(Canvas canvas, Size size) {
    final maxRadius = math.max(size.width, size.height) * 0.7;
    final radius = maxRadius * progress;
    final opacity = (1 - progress).clamp(0.0, 1.0);

    final paint = Paint()
      ..color = color.withOpacity(opacity)
      ..style = PaintingStyle.stroke
      ..strokeWidth = 3;

    canvas.drawCircle(position, radius, paint);

    // Draw inner ripple
    final innerPaint = Paint()
      ..color = color.withOpacity(opacity * 0.5)
      ..style = PaintingStyle.stroke
      ..strokeWidth = 1;

    canvas.drawCircle(position, radius * 0.7, innerPaint);
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}
```

Ripple effects provide visual feedback for user interactions. Each tap creates  
expanding circles that fade out over time. Multiple ripples can exist  
simultaneously, creating complex overlapping patterns. The effect uses  
CustomPainter to draw expanding circles with decreasing opacity.  

## Floating action button animation

Multi-state FAB with expanding options using AnimationController.  

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
      title: 'Animated FAB',
      theme: ThemeData.dark(),
      home: const AnimatedFabPage(),
    );
  }
}

class AnimatedFabPage extends StatefulWidget {
  const AnimatedFabPage({super.key});

  @override
  State<AnimatedFabPage> createState() => _AnimatedFabPageState();
}

class _AnimatedFabPageState extends State<AnimatedFabPage>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _expandAnimation;
  late Animation<double> _rotateAnimation;

  bool _isExpanded = false;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(milliseconds: 300),
      vsync: this,
    );

    _expandAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    ));

    _rotateAnimation = Tween<double>(
      begin: 0.0,
      end: 0.125, // 45 degrees
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    ));
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  void _toggle() {
    setState(() {
      _isExpanded = !_isExpanded;
    });

    if (_isExpanded) {
      _controller.forward();
    } else {
      _controller.reverse();
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Animated FAB'),
      ),
      body: const Center(
        child: Text(
          'Tap the floating action button',
          style: TextStyle(fontSize: 18),
        ),
      ),
      floatingActionButton: AnimatedBuilder(
        animation: _controller,
        builder: (context, child) {
          return Stack(
            alignment: Alignment.bottomRight,
            children: [
              // Option buttons
              ..._buildOptionButtons(),
              // Main FAB
              FloatingActionButton(
                onPressed: _toggle,
                child: RotationTransition(
                  turns: _rotateAnimation,
                  child: const Icon(Icons.add),
                ),
              ),
            ],
          );
        },
      ),
    );
  }

  List<Widget> _buildOptionButtons() {
    final options = [
      {'icon': Icons.message, 'label': 'Message', 'color': Colors.blue},
      {'icon': Icons.call, 'label': 'Call', 'color': Colors.green},
      {'icon': Icons.email, 'label': 'Email', 'color': Colors.orange},
    ];

    return options.asMap().entries.map((entry) {
      final index = entry.key;
      final option = entry.value;
      
      return Transform.translate(
        offset: Offset(
          0,
          -70.0 * (index + 1) * _expandAnimation.value,
        ),
        child: Transform.scale(
          scale: _expandAnimation.value,
          child: Opacity(
            opacity: _expandAnimation.value,
            child: Row(
              mainAxisSize: MainAxisSize.min,
              children: [
                Container(
                  padding: const EdgeInsets.symmetric(
                    horizontal: 12,
                    vertical: 6,
                  ),
                  decoration: BoxDecoration(
                    color: Colors.black87,
                    borderRadius: BorderRadius.circular(16),
                  ),
                  child: Text(
                    option['label'] as String,
                    style: const TextStyle(
                      color: Colors.white,
                      fontSize: 12,
                    ),
                  ),
                ),
                const SizedBox(width: 8),
                FloatingActionButton(
                  mini: true,
                  backgroundColor: option['color'] as Color,
                  onPressed: () {
                    ScaffoldMessenger.of(context).showSnackBar(
                      SnackBar(
                        content: Text('${option['label']} tapped!'),
                        duration: const Duration(seconds: 1),
                      ),
                    );
                  },
                  child: Icon(option['icon'] as IconData),
                ),
              ],
            ),
          ),
        ),
      );
    }).toList();
  }
}
```

Animated FABs enhance user experience by revealing additional actions on demand.  
The main button rotates when tapped, while option buttons scale and slide into  
view with staggered timing. Labels appear alongside icons, and the entire  
animation reverses smoothly when collapsed.  

## List item staggered entrance

Animating list items with progressive delays for smooth entrance effects.  

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
      title: 'Staggered List',
      theme: ThemeData.dark(),
      home: const StaggeredListPage(),
    );
  }
}

class StaggeredListPage extends StatefulWidget {
  const StaggeredListPage({super.key});

  @override
  State<StaggeredListPage> createState() => _StaggeredListPageState();
}

class _StaggeredListPageState extends State<StaggeredListPage>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  final List<String> items = [
    'Beautiful Animations',
    'Smooth Transitions',
    'Engaging UI Effects',
    'Interactive Elements',
    'Delightful Motion',
    'Visual Feedback',
    'Polished Experience',
    'Modern Design',
  ];

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: Duration(milliseconds: 300 * items.length),
      vsync: this,
    );
    _controller.forward();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  void _restart() {
    _controller.reset();
    _controller.forward();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Staggered List'),
        actions: [
          IconButton(
            icon: const Icon(Icons.replay),
            onPressed: _restart,
          ),
        ],
      ),
      body: ListView.builder(
        padding: const EdgeInsets.all(16),
        itemCount: items.length,
        itemBuilder: (context, index) {
          return AnimatedListItem(
            animation: _controller,
            index: index,
            totalItems: items.length,
            child: Card(
              margin: const EdgeInsets.only(bottom: 12),
              child: ListTile(
                leading: CircleAvatar(
                  backgroundColor: Colors.primaries[index % Colors.primaries.length],
                  child: Text('${index + 1}'),
                ),
                title: Text(items[index]),
                subtitle: Text('Item ${index + 1} description'),
                trailing: const Icon(Icons.arrow_forward_ios),
                onTap: () {
                  ScaffoldMessenger.of(context).showSnackBar(
                    SnackBar(
                      content: Text('Tapped: ${items[index]}'),
                      duration: const Duration(seconds: 1),
                    ),
                  );
                },
              ),
            ),
          );
        },
      ),
    );
  }
}

class AnimatedListItem extends StatelessWidget {
  final Animation<double> animation;
  final int index;
  final int totalItems;
  final Widget child;

  const AnimatedListItem({
    super.key,
    required this.animation,
    required this.index,
    required this.totalItems,
    required this.child,
  });

  @override
  Widget build(BuildContext context) {
    final itemInterval = 1.0 / totalItems;
    final start = index * itemInterval * 0.5;
    final end = start + itemInterval;

    final slideAnimation = Tween<Offset>(
      begin: const Offset(1, 0),
      end: Offset.zero,
    ).animate(CurvedAnimation(
      parent: animation,
      curve: Interval(start, end.clamp(0.0, 1.0), curve: Curves.easeOut),
    ));

    final fadeAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: animation,
      curve: Interval(start, end.clamp(0.0, 1.0), curve: Curves.easeOut),
    ));

    return SlideTransition(
      position: slideAnimation,
      child: FadeTransition(
        opacity: fadeAnimation,
        child: child,
      ),
    );
  }
}
```

Staggered list animations create elegant entrance effects where items appear  
progressively rather than all at once. Each item has its own animation interval  
based on its position, creating a cascading effect. This approach works well  
for any collection of items that need graceful presentation.  

## Breathing animation

Subtle pulsing effect using sine wave mathematics for organic motion.  

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
      title: 'Breathing Animation',
      theme: ThemeData.dark(),
      home: const BreathingAnimationPage(),
    );
  }
}

class BreathingAnimationPage extends StatefulWidget {
  const BreathingAnimationPage({super.key});

  @override
  State<BreathingAnimationPage> createState() => _BreathingAnimationPageState();
}

class _BreathingAnimationPageState extends State<BreathingAnimationPage>
    with TickerProviderStateMixin {
  late AnimationController _breathingController;
  late AnimationController _colorController;

  @override
  void initState() {
    super.initState();
    _breathingController = AnimationController(
      duration: const Duration(seconds: 4),
      vsync: this,
    )..repeat();

    _colorController = AnimationController(
      duration: const Duration(seconds: 6),
      vsync: this,
    )..repeat();
  }

  @override
  void dispose() {
    _breathingController.dispose();
    _colorController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Breathing Animation'),
      ),
      body: Center(
        child: AnimatedBuilder(
          animation: Listenable.merge([_breathingController, _colorController]),
          builder: (context, child) {
            // Breathing scale based on sine wave
            final breathingScale = 1.0 + 0.1 * math.sin(_breathingController.value * 2 * math.pi);
            
            // Color transition
            final color = ColorTween(
              begin: Colors.blue,
              end: Colors.purple,
            ).evaluate(_colorController);

            return Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                const Text(
                  'Relax and breathe',
                  style: TextStyle(fontSize: 24, fontWeight: FontWeight.w300),
                ),
                const SizedBox(height: 50),
                Transform.scale(
                  scale: breathingScale,
                  child: Container(
                    width: 200,
                    height: 200,
                    decoration: BoxDecoration(
                      shape: BoxShape.circle,
                      gradient: RadialGradient(
                        colors: [
                          color!.withOpacity(0.8),
                          color.withOpacity(0.2),
                        ],
                        stops: const [0.3, 1.0],
                      ),
                      boxShadow: [
                        BoxShadow(
                          color: color.withOpacity(0.3),
                          blurRadius: 20 * breathingScale,
                          spreadRadius: 5,
                        ),
                      ],
                    ),
                    child: Center(
                      child: Text(
                        _getBreathingText(_breathingController.value),
                        style: const TextStyle(
                          fontSize: 18,
                          fontWeight: FontWeight.w500,
                          color: Colors.white,
                        ),
                      ),
                    ),
                  ),
                ),
                const SizedBox(height: 30),
                AnimatedOpacity(
                  opacity: 0.5 + 0.5 * math.sin(_breathingController.value * 2 * math.pi),
                  duration: const Duration(milliseconds: 100),
                  child: const Text(
                    'Follow the circle\'s rhythm',
                    style: TextStyle(fontSize: 16, color: Colors.grey),
                  ),
                ),
              ],
            );
          },
        ),
      ),
    );
  }

  String _getBreathingText(double progress) {
    final cycleProgress = progress % 1.0;
    if (cycleProgress < 0.4) {
      return 'Breathe In';
    } else if (cycleProgress < 0.6) {
      return 'Hold';
    } else {
      return 'Breathe Out';
    }
  }
}
```

Breathing animations create calming, meditative effects using gentle pulsing  
motion. The sine wave mathematics produces natural, organic scaling that mimics  
human breathing patterns. Combined with color transitions and synchronized text  
cues, it creates a soothing user experience perfect for wellness apps.  
