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

