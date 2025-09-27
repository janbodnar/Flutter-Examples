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
