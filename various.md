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
