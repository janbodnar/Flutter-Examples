# Flutter Platform Integration - 25 Essential Examples

This comprehensive guide covers Flutter platform integration with 25  
practical examples demonstrating how to use native device features like  
camera, sensors, notifications, maps, and more. Each example builds on  
previous concepts, progressing from basic to advanced platform features.  

## Camera Basic Access

Accessing device camera for taking photos with basic functionality.  

```dart
import 'package:flutter/material.dart';
import 'package:camera/camera.dart';
import 'dart:io';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  final cameras = await availableCameras();
  runApp(MyApp(cameras: cameras));
}

class MyApp extends StatelessWidget {
  final List<CameraDescription> cameras;
  
  const MyApp({super.key, required this.cameras});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Basic Camera',
      home: CameraScreen(cameras: cameras),
    );
  }
}

class CameraScreen extends StatefulWidget {
  final List<CameraDescription> cameras;
  
  const CameraScreen({super.key, required this.cameras});

  @override
  State<CameraScreen> createState() => _CameraScreenState();
}

class _CameraScreenState extends State<CameraScreen> {
  late CameraController _controller;
  late Future<void> _initializeControllerFuture;

  @override
  void initState() {
    super.initState();
    _controller = CameraController(
      widget.cameras.first,
      ResolutionPreset.medium,
    );
    _initializeControllerFuture = _controller.initialize();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  Future<void> _takePicture() async {
    try {
      await _initializeControllerFuture;
      final image = await _controller.takePicture();
      
      if (!mounted) return;
      
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Picture saved to ${image.path}')),
      );
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error: $e')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic Camera'),
      ),
      body: FutureBuilder<void>(
        future: _initializeControllerFuture,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.done) {
            return CameraPreview(_controller);
          } else {
            return const Center(child: CircularProgressIndicator());
          }
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _takePicture,
        child: const Icon(Icons.camera_alt),
      ),
    );
  }
}
```

This example demonstrates basic camera integration using the camera  
plugin. It initializes the camera controller, displays a preview, and  
allows taking pictures. The camera is properly disposed when the widget  
is removed from the tree.  

## Image Picker Integration

Using image picker for selecting photos from gallery or camera.  

```dart
import 'package:flutter/material.dart';
import 'package:image_picker/image_picker.dart';
import 'dart:io';

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
      title: 'Image Picker',
      home: const ImagePickerScreen(),
    );
  }
}

class ImagePickerScreen extends StatefulWidget {
  const ImagePickerScreen({super.key});

  @override
  State<ImagePickerScreen> createState() => _ImagePickerScreenState();
}

class _ImagePickerScreenState extends State<ImagePickerScreen> {
  File? _image;
  final ImagePicker _picker = ImagePicker();

  Future<void> _pickImageFromCamera() async {
    try {
      final XFile? image = await _picker.pickImage(
        source: ImageSource.camera,
        maxWidth: 1800,
        maxHeight: 1800,
        imageQuality: 80,
      );
      
      if (image != null) {
        setState(() {
          _image = File(image.path);
        });
      }
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error picking image: $e')),
      );
    }
  }

  Future<void> _pickImageFromGallery() async {
    try {
      final XFile? image = await _picker.pickImage(
        source: ImageSource.gallery,
        maxWidth: 1800,
        maxHeight: 1800,
        imageQuality: 80,
      );
      
      if (image != null) {
        setState(() {
          _image = File(image.path);
        });
      }
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error picking image: $e')),
      );
    }
  }

  void _clearImage() {
    setState(() {
      _image = null;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Image Picker'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Container(
              height: 300,
              width: 300,
              decoration: BoxDecoration(
                border: Border.all(color: Colors.grey),
                borderRadius: BorderRadius.circular(8),
              ),
              child: _image != null
                  ? ClipRRect(
                      borderRadius: BorderRadius.circular(8),
                      child: Image.file(
                        _image!,
                        fit: BoxFit.cover,
                      ),
                    )
                  : const Icon(
                      Icons.image,
                      size: 100,
                      color: Colors.grey,
                    ),
            ),
            const SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                ElevatedButton.icon(
                  onPressed: _pickImageFromCamera,
                  icon: const Icon(Icons.camera_alt),
                  label: const Text('Camera'),
                ),
                ElevatedButton.icon(
                  onPressed: _pickImageFromGallery,
                  icon: const Icon(Icons.photo_library),
                  label: const Text('Gallery'),
                ),
                if (_image != null)
                  ElevatedButton.icon(
                    onPressed: _clearImage,
                    icon: const Icon(Icons.clear),
                    label: const Text('Clear'),
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.red,
                    ),
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

The ImagePicker plugin provides an easy way to select images from the  
device's camera or gallery. This example demonstrates proper error  
handling, image quality settings, and UI updates when images are  
selected or cleared.  

## Camera with Flash Control

Advanced camera usage with flash control and different capture modes.  

```dart
import 'package:flutter/material.dart';
import 'package:camera/camera.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  final cameras = await availableCameras();
  runApp(MyApp(cameras: cameras));
}

class MyApp extends StatelessWidget {
  final List<CameraDescription> cameras;
  
  const MyApp({super.key, required this.cameras});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Camera Flash Control',
      home: FlashCameraScreen(cameras: cameras),
    );
  }
}

class FlashCameraScreen extends StatefulWidget {
  final List<CameraDescription> cameras;
  
  const FlashCameraScreen({super.key, required this.cameras});

  @override
  State<FlashCameraScreen> createState() => _FlashCameraScreenState();
}

class _FlashCameraScreenState extends State<FlashCameraScreen> {
  late CameraController _controller;
  late Future<void> _initializeControllerFuture;
  FlashMode _currentFlashMode = FlashMode.auto;
  bool _isRecording = false;

  @override
  void initState() {
    super.initState();
    _controller = CameraController(
      widget.cameras.first,
      ResolutionPreset.high,
    );
    _initializeControllerFuture = _controller.initialize();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  Future<void> _toggleFlashMode() async {
    try {
      setState(() {
        _currentFlashMode = _currentFlashMode == FlashMode.auto
            ? FlashMode.always
            : _currentFlashMode == FlashMode.always
                ? FlashMode.off
                : FlashMode.auto;
      });
      await _controller.setFlashMode(_currentFlashMode);
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error changing flash mode: $e')),
      );
    }
  }

  Future<void> _takePicture() async {
    try {
      await _initializeControllerFuture;
      final image = await _controller.takePicture();
      
      if (!mounted) return;
      
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Picture saved to ${image.path}')),
      );
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error taking picture: $e')),
      );
    }
  }

  Future<void> _toggleRecording() async {
    try {
      if (_isRecording) {
        final video = await _controller.stopVideoRecording();
        setState(() {
          _isRecording = false;
        });
        if (mounted) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(content: Text('Video saved to ${video.path}')),
          );
        }
      } else {
        await _controller.startVideoRecording();
        setState(() {
          _isRecording = true;
        });
      }
    } catch (e) {
      setState(() {
        _isRecording = false;
      });
      if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Error recording video: $e')),
        );
      }
    }
  }

  String _getFlashModeText() {
    switch (_currentFlashMode) {
      case FlashMode.auto:
        return 'Auto';
      case FlashMode.always:
        return 'On';
      case FlashMode.off:
        return 'Off';
      default:
        return 'Auto';
    }
  }

  IconData _getFlashModeIcon() {
    switch (_currentFlashMode) {
      case FlashMode.auto:
        return Icons.flash_auto;
      case FlashMode.always:
        return Icons.flash_on;
      case FlashMode.off:
        return Icons.flash_off;
      default:
        return Icons.flash_auto;
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Camera with Flash'),
        actions: [
          IconButton(
            onPressed: _toggleFlashMode,
            icon: Icon(_getFlashModeIcon()),
            tooltip: 'Flash: ${_getFlashModeText()}',
          ),
        ],
      ),
      body: FutureBuilder<void>(
        future: _initializeControllerFuture,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.done) {
            return Stack(
              children: [
                Positioned.fill(
                  child: CameraPreview(_controller),
                ),
                if (_isRecording)
                  const Positioned(
                    top: 16,
                    left: 16,
                    child: Row(
                      children: [
                        Icon(Icons.fiber_manual_record, color: Colors.red),
                        SizedBox(width: 8),
                        Text(
                          'Recording...',
                          style: TextStyle(
                            color: Colors.white,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ],
                    ),
                  ),
              ],
            );
          } else {
            return const Center(child: CircularProgressIndicator());
          }
        },
      ),
      floatingActionButton: Column(
        mainAxisAlignment: MainAxisAlignment.end,
        children: [
          FloatingActionButton(
            heroTag: 'photo',
            onPressed: _takePicture,
            child: const Icon(Icons.camera_alt),
          ),
          const SizedBox(height: 16),
          FloatingActionButton(
            heroTag: 'video',
            onPressed: _toggleRecording,
            backgroundColor: _isRecording ? Colors.red : null,
            child: Icon(_isRecording ? Icons.stop : Icons.videocam),
          ),
        ],
      ),
    );
  }
}
```

This advanced camera example demonstrates flash mode control and video  
recording capabilities. Users can cycle through different flash modes  
(auto, on, off) and record videos in addition to taking photos. The UI  
provides visual feedback for recording status.  

## Accelerometer Sensor

Reading device accelerometer data for motion detection.  

```dart
import 'package:flutter/material.dart';
import 'package:sensors_plus/sensors_plus.dart';
import 'dart:async';
import 'dart:math';

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
      title: 'Accelerometer Sensor',
      home: const AccelerometerScreen(),
    );
  }
}

class AccelerometerScreen extends StatefulWidget {
  const AccelerometerScreen({super.key});

  @override
  State<AccelerometerScreen> createState() => _AccelerometerScreenState();
}

class _AccelerometerScreenState extends State<AccelerometerScreen> {
  StreamSubscription<AccelerometerEvent>? _accelerometerSubscription;
  double _x = 0.0, _y = 0.0, _z = 0.0;
  double _magnitude = 0.0;
  bool _isShaking = false;
  int _shakeCount = 0;
  DateTime? _lastShakeTime;

  @override
  void initState() {
    super.initState();
    _startAccelerometerListening();
  }

  @override
  void dispose() {
    _accelerometerSubscription?.cancel();
    super.dispose();
  }

  void _startAccelerometerListening() {
    _accelerometerSubscription = accelerometerEvents.listen(
      (AccelerometerEvent event) {
        setState(() {
          _x = event.x;
          _y = event.y;
          _z = event.z;
          _magnitude = sqrt(_x * _x + _y * _y + _z * _z);
          
          // Detect shake (threshold can be adjusted)
          if (_magnitude > 15.0) {
            final now = DateTime.now();
            if (_lastShakeTime == null || 
                now.difference(_lastShakeTime!).inMilliseconds > 500) {
              _lastShakeTime = now;
              _shakeCount++;
              _isShaking = true;
              
              // Reset shake indicator after 1 second
              Timer(const Duration(seconds: 1), () {
                if (mounted) {
                  setState(() {
                    _isShaking = false;
                  });
                }
              });
            }
          }
        });
      },
    );
  }

  Color _getAccelerationColor(double value) {
    final normalized = (value.abs() / 10).clamp(0.0, 1.0);
    return Color.lerp(Colors.blue, Colors.red, normalized)!;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Accelerometer Sensor'),
        actions: [
          IconButton(
            onPressed: () {
              setState(() {
                _shakeCount = 0;
              });
            },
            icon: const Icon(Icons.refresh),
            tooltip: 'Reset shake counter',
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Card(
              color: _isShaking ? Colors.red.withOpacity(0.2) : null,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        Icon(
                          _isShaking ? Icons.vibration : Icons.smartphone,
                          color: _isShaking ? Colors.red : null,
                        ),
                        const SizedBox(width: 8),
                        Text(
                          'Device Status',
                          style: Theme.of(context).textTheme.titleLarge,
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Text(
                      _isShaking ? 'SHAKING DETECTED!' : 'Stable',
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                        color: _isShaking ? Colors.red : Colors.green,
                      ),
                    ),
                    Text('Shake Count: $_shakeCount'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Raw Accelerometer Data',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 12),
                    _buildAccelerationBar('X-Axis', _x, _getAccelerationColor(_x)),
                    const SizedBox(height: 8),
                    _buildAccelerationBar('Y-Axis', _y, _getAccelerationColor(_y)),
                    const SizedBox(height: 8),
                    _buildAccelerationBar('Z-Axis', _z, _getAccelerationColor(_z)),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        const Icon(Icons.speed),
                        const SizedBox(width: 8),
                        Text(
                          'Magnitude: ${_magnitude.toStringAsFixed(2)} m/s²',
                          style: const TextStyle(
                            fontSize: 16,
                            fontWeight: FontWeight.w500,
                          ),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Device Orientation',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 12),
                    Text(_getOrientationDescription()),
                    const SizedBox(height: 8),
                    LinearProgressIndicator(
                      value: (_magnitude / 20).clamp(0.0, 1.0),
                      backgroundColor: Colors.grey[700],
                      valueColor: AlwaysStoppedAnimation<Color>(
                        _getAccelerationColor(_magnitude),
                      ),
                    ),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildAccelerationBar(String label, double value, Color color) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          '$label: ${value.toStringAsFixed(2)} m/s²',
          style: const TextStyle(fontWeight: FontWeight.w500),
        ),
        const SizedBox(height: 4),
        LinearProgressIndicator(
          value: (value.abs() / 10).clamp(0.0, 1.0),
          backgroundColor: Colors.grey[700],
          valueColor: AlwaysStoppedAnimation<Color>(color),
        ),
      ],
    );
  }

  String _getOrientationDescription() {
    if (_z > 8) {
      return 'Face up (flat on surface)';
    } else if (_z < -8) {
      return 'Face down';
    } else if (_x > 8) {
      return 'Right side down';
    } else if (_x < -8) {
      return 'Left side down';
    } else if (_y > 8) {
      return 'Top edge down';
    } else if (_y < -8) {
      return 'Bottom edge down';
    } else {
      return 'Device in motion or tilted';
    }
  }
}
```

This accelerometer example demonstrates real-time motion sensing with  
shake detection, orientation detection, and visual feedback. The app  
displays raw sensor data, calculates magnitude, and provides intuitive  
feedback about device movement and position.  

## Gyroscope Sensor

Using gyroscope sensor for detecting device rotation and angular velocity.  

```dart
import 'package:flutter/material.dart';
import 'package:sensors_plus/sensors_plus.dart';
import 'dart:async';
import 'dart:math';

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
      title: 'Gyroscope Sensor',
      home: const GyroscopeScreen(),
    );
  }
}

class GyroscopeScreen extends StatefulWidget {
  const GyroscopeScreen({super.key});

  @override
  State<GyroscopeScreen> createState() => _GyroscopeScreenState();
}

class _GyroscopeScreenState extends State<GyroscopeScreen>
    with TickerProviderStateMixin {
  StreamSubscription<GyroscopeEvent>? _gyroscopeSubscription;
  double _x = 0.0, _y = 0.0, _z = 0.0;
  double _rotationX = 0.0, _rotationY = 0.0, _rotationZ = 0.0;
  bool _isSpinning = false;
  late AnimationController _animationController;

  @override
  void initState() {
    super.initState();
    _animationController = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    )..repeat();
    _startGyroscopeListening();
  }

  @override
  void dispose() {
    _gyroscopeSubscription?.cancel();
    _animationController.dispose();
    super.dispose();
  }

  void _startGyroscopeListening() {
    _gyroscopeSubscription = gyroscopeEvents.listen(
      (GyroscopeEvent event) {
        setState(() {
          _x = event.x;
          _y = event.y;
          _z = event.z;
          
          // Integrate angular velocity to get rotation
          _rotationX += _x * 0.1;
          _rotationY += _y * 0.1;
          _rotationZ += _z * 0.1;
          
          // Detect spinning (high angular velocity)
          final totalAngularVelocity = sqrt(_x * _x + _y * _y + _z * _z);
          _isSpinning = totalAngularVelocity > 5.0;
        });
      },
    );
  }

  void _resetRotation() {
    setState(() {
      _rotationX = 0.0;
      _rotationY = 0.0;
      _rotationZ = 0.0;
    });
  }

  Color _getRotationColor(double value) {
    final normalized = (value.abs() / 10).clamp(0.0, 1.0);
    return Color.lerp(Colors.green, Colors.orange, normalized)!;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Gyroscope Sensor'),
        actions: [
          IconButton(
            onPressed: _resetRotation,
            icon: const Icon(Icons.refresh),
            tooltip: 'Reset rotation',
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    Row(
                      children: [
                        Icon(
                          _isSpinning ? Icons.sync : Icons.rotation_3d,
                          color: _isSpinning ? Colors.orange : null,
                        ),
                        const SizedBox(width: 8),
                        Text(
                          'Device Rotation Status',
                          style: Theme.of(context).textTheme.titleLarge,
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Text(
                      _isSpinning ? 'RAPID ROTATION DETECTED!' : 'Normal rotation',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.bold,
                        color: _isSpinning ? Colors.orange : Colors.blue,
                      ),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Expanded(
              child: Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Angular Velocity (rad/s)',
                        style: Theme.of(context).textTheme.titleLarge,
                      ),
                      const SizedBox(height: 16),
                      Expanded(
                        child: Center(
                          child: AnimatedBuilder(
                            animation: _animationController,
                            builder: (context, child) {
                              return Transform(
                                alignment: Alignment.center,
                                transform: Matrix4.identity()
                                  ..rotateX(_rotationX)
                                  ..rotateY(_rotationY)
                                  ..rotateZ(_rotationZ),
                                child: Container(
                                  width: 120,
                                  height: 120,
                                  decoration: BoxDecoration(
                                    color: Colors.blue.withOpacity(0.8),
                                    borderRadius: BorderRadius.circular(12),
                                    boxShadow: [
                                      BoxShadow(
                                        color: Colors.blue.withOpacity(0.3),
                                        blurRadius: 20,
                                        spreadRadius: 2,
                                      ),
                                    ],
                                  ),
                                  child: const Icon(
                                    Icons.phone_android,
                                    size: 60,
                                    color: Colors.white,
                                  ),
                                ),
                              );
                            },
                          ),
                        ),
                      ),
                      const SizedBox(height: 16),
                      _buildRotationBar('X-Axis (Roll)', _x, _getRotationColor(_x)),
                      const SizedBox(height: 8),
                      _buildRotationBar('Y-Axis (Pitch)', _y, _getRotationColor(_y)),
                      const SizedBox(height: 8),
                      _buildRotationBar('Z-Axis (Yaw)', _z, _getRotationColor(_z)),
                    ],
                  ),
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceAround,
                  children: [
                    _buildRotationInfo('Roll', _rotationX),
                    _buildRotationInfo('Pitch', _rotationY),
                    _buildRotationInfo('Yaw', _rotationZ),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildRotationBar(String label, double value, Color color) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          '$label: ${value.toStringAsFixed(2)}',
          style: const TextStyle(fontWeight: FontWeight.w500),
        ),
        const SizedBox(height: 4),
        LinearProgressIndicator(
          value: (value.abs() / 10).clamp(0.0, 1.0),
          backgroundColor: Colors.grey[700],
          valueColor: AlwaysStoppedAnimation<Color>(color),
        ),
      ],
    );
  }

  Widget _buildRotationInfo(String label, double value) {
    return Column(
      children: [
        Text(
          label,
          style: const TextStyle(fontWeight: FontWeight.w500),
        ),
        Text(
          '${(value * 180 / pi).toStringAsFixed(0)}°',
          style: const TextStyle(fontSize: 16),
        ),
      ],
    );
  }
}
```

The gyroscope sensor provides angular velocity measurements around three  
axes. This example visualizes rotation in real-time with a 3D-transformed  
device representation and tracks accumulated rotation angles for each  
axis with proper conversion from radians to degrees.  

## Local Notifications

Creating and scheduling local notifications with different types and actions.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_local_notifications/flutter_local_notifications.dart';
import 'package:timezone/timezone.dart' as tz;
import 'package:timezone/data/latest.dart' as tz;

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  tz.initializeTimeZones();
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Local Notifications',
      home: const NotificationsScreen(),
    );
  }
}

class NotificationsScreen extends StatefulWidget {
  const NotificationsScreen({super.key});

  @override
  State<NotificationsScreen> createState() => _NotificationsScreenState();
}

class _NotificationsScreenState extends State<NotificationsScreen> {
  final FlutterLocalNotificationsPlugin _notificationsPlugin =
      FlutterLocalNotificationsPlugin();
  int _notificationId = 0;

  @override
  void initState() {
    super.initState();
    _initializeNotifications();
  }

  Future<void> _initializeNotifications() async {
    const AndroidInitializationSettings initializationSettingsAndroid =
        AndroidInitializationSettings('@mipmap/ic_launcher');

    const DarwinInitializationSettings initializationSettingsIOS =
        DarwinInitializationSettings(
      requestAlertPermission: true,
      requestBadgePermission: true,
      requestSoundPermission: true,
    );

    const InitializationSettings initializationSettings =
        InitializationSettings(
      android: initializationSettingsAndroid,
      iOS: initializationSettingsIOS,
    );

    await _notificationsPlugin.initialize(
      initializationSettings,
      onDidReceiveNotificationResponse: _onNotificationTapped,
    );

    // Request permissions for iOS
    await _notificationsPlugin
        .resolvePlatformSpecificImplementation<
            IOSFlutterLocalNotificationsPlugin>()
        ?.requestPermissions(
          alert: true,
          badge: true,
          sound: true,
        );
  }

  void _onNotificationTapped(NotificationResponse response) {
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text('Notification tapped: ${response.payload}'),
      ),
    );
  }

  Future<void> _showSimpleNotification() async {
    const AndroidNotificationDetails androidPlatformChannelSpecifics =
        AndroidNotificationDetails(
      'simple_channel',
      'Simple Notifications',
      channelDescription: 'Basic notification channel',
      importance: Importance.max,
      priority: Priority.high,
      icon: '@mipmap/ic_launcher',
    );

    const DarwinNotificationDetails iOSPlatformChannelSpecifics =
        DarwinNotificationDetails(
      presentAlert: true,
      presentBadge: true,
      presentSound: true,
    );

    const NotificationDetails platformChannelSpecifics = NotificationDetails(
      android: androidPlatformChannelSpecifics,
      iOS: iOSPlatformChannelSpecifics,
    );

    await _notificationsPlugin.show(
      _notificationId++,
      'Simple Notification',
      'This is a basic notification message',
      platformChannelSpecifics,
      payload: 'simple_notification',
    );
  }

  Future<void> _showScheduledNotification() async {
    const AndroidNotificationDetails androidPlatformChannelSpecifics =
        AndroidNotificationDetails(
      'scheduled_channel',
      'Scheduled Notifications',
      channelDescription: 'Notifications scheduled for future',
      importance: Importance.max,
      priority: Priority.high,
      icon: '@mipmap/ic_launcher',
    );

    const NotificationDetails platformChannelSpecifics = NotificationDetails(
      android: androidPlatformChannelSpecifics,
    );

    await _notificationsPlugin.zonedSchedule(
      _notificationId++,
      'Scheduled Notification',
      'This notification was scheduled 5 seconds ago',
      tz.TZDateTime.now(tz.local).add(const Duration(seconds: 5)),
      platformChannelSpecifics,
      androidAllowWhileIdle: true,
      uiLocalNotificationDateInterpretation:
          UILocalNotificationDateInterpretation.absoluteTime,
      payload: 'scheduled_notification',
    );

    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('Notification scheduled for 5 seconds from now'),
      ),
    );
  }

  Future<void> _showBigTextNotification() async {
    const AndroidNotificationDetails androidPlatformChannelSpecifics =
        AndroidNotificationDetails(
      'big_text_channel',
      'Big Text Notifications',
      channelDescription: 'Notifications with expanded text',
      importance: Importance.max,
      priority: Priority.high,
      styleInformation: BigTextStyleInformation(
        'This is a very long notification message that will be expanded '
        'when the user expands the notification. It can contain multiple '
        'lines of text and provides more detailed information to the user.',
        contentTitle: 'Expanded Title',
        summaryText: 'Summary of the notification',
      ),
      icon: '@mipmap/ic_launcher',
    );

    const NotificationDetails platformChannelSpecifics = NotificationDetails(
      android: androidPlatformChannelSpecifics,
    );

    await _notificationsPlugin.show(
      _notificationId++,
      'Big Text Notification',
      'Short description...',
      platformChannelSpecifics,
      payload: 'big_text_notification',
    );
  }

  Future<void> _showProgressNotification() async {
    for (int i = 0; i <= 100; i += 20) {
      final AndroidNotificationDetails androidPlatformChannelSpecifics =
          AndroidNotificationDetails(
        'progress_channel',
        'Progress Notifications',
        channelDescription: 'Notifications showing progress',
        importance: Importance.low,
        priority: Priority.low,
        onlyAlertOnce: true,
        showProgress: true,
        maxProgress: 100,
        progress: i,
        icon: '@mipmap/ic_launcher',
      );

      const NotificationDetails platformChannelSpecifics = NotificationDetails(
        android: androidPlatformChannelSpecifics,
      );

      await _notificationsPlugin.show(
        999, // Use same ID to update existing notification
        'Download Progress',
        'Downloading file... $i%',
        platformChannelSpecifics,
        payload: 'progress_notification',
      );

      await Future.delayed(const Duration(seconds: 1));
    }

    // Final notification when complete
    const AndroidNotificationDetails completedNotificationDetails =
        AndroidNotificationDetails(
      'progress_channel',
      'Progress Notifications',
      channelDescription: 'Notifications showing progress',
      importance: Importance.max,
      priority: Priority.high,
      icon: '@mipmap/ic_launcher',
    );

    const NotificationDetails completedPlatformChannelSpecifics =
        NotificationDetails(
      android: completedNotificationDetails,
    );

    await _notificationsPlugin.show(
      999,
      'Download Complete',
      'File has been downloaded successfully!',
      completedPlatformChannelSpecifics,
      payload: 'download_complete',
    );
  }

  Future<void> _cancelAllNotifications() async {
    await _notificationsPlugin.cancelAll();
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(content: Text('All notifications cancelled')),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Local Notifications'),
        actions: [
          IconButton(
            onPressed: _cancelAllNotifications,
            icon: const Icon(Icons.clear_all),
            tooltip: 'Cancel all notifications',
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Basic Notifications',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 12),
                    ElevatedButton.icon(
                      onPressed: _showSimpleNotification,
                      icon: const Icon(Icons.notifications),
                      label: const Text('Show Simple Notification'),
                    ),
                    const SizedBox(height: 8),
                    ElevatedButton.icon(
                      onPressed: _showScheduledNotification,
                      icon: const Icon(Icons.schedule),
                      label: const Text('Schedule Notification (5s)'),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Advanced Notifications',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 12),
                    ElevatedButton.icon(
                      onPressed: _showBigTextNotification,
                      icon: const Icon(Icons.text_fields),
                      label: const Text('Big Text Notification'),
                    ),
                    const SizedBox(height: 8),
                    ElevatedButton.icon(
                      onPressed: _showProgressNotification,
                      icon: const Icon(Icons.download),
                      label: const Text('Progress Notification'),
                    ),
                  ],
                ),
              ),
            ),
            const Spacer(),
            Card(
              color: Theme.of(context).colorScheme.errorContainer,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    const Icon(Icons.info_outline),
                    const SizedBox(height: 8),
                    const Text(
                      'Tap any notification to see interaction feedback',
                      textAlign: TextAlign.center,
                      style: TextStyle(fontSize: 12),
                    ),
                  ],
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

This comprehensive notification example demonstrates various types of  
local notifications including simple alerts, scheduled notifications,  
big text notifications, and progress notifications. It includes proper  
platform-specific configuration and handles user interactions.  

## Push Notifications with Firebase

Implementing Firebase Cloud Messaging for remote push notifications.  

```dart
import 'package:flutter/material.dart';
import 'package:firebase_messaging/firebase_messaging.dart';
import 'package:flutter_local_notifications/flutter_local_notifications.dart';

Future<void> _firebaseMessagingBackgroundHandler(RemoteMessage message) async {
  print('Handling a background message: ${message.messageId}');
}

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  FirebaseMessaging.onBackgroundMessage(_firebaseMessagingBackgroundHandler);
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Push Notifications',
      home: const PushNotificationsScreen(),
    );
  }
}

class PushNotificationsScreen extends StatefulWidget {
  const PushNotificationsScreen({super.key});

  @override
  State<PushNotificationsScreen> createState() => _PushNotificationsScreenState();
}

class _PushNotificationsScreenState extends State<PushNotificationsScreen> {
  final FirebaseMessaging _messaging = FirebaseMessaging.instance;
  final FlutterLocalNotificationsPlugin _localNotifications =
      FlutterLocalNotificationsPlugin();
  String? _fcmToken;
  List<RemoteMessage> _messages = [];

  @override
  void initState() {
    super.initState();
    _initializePushNotifications();
  }

  Future<void> _initializePushNotifications() async {
    // Request permissions
    NotificationSettings settings = await _messaging.requestPermission(
      alert: true,
      badge: true,
      sound: true,
      carPlay: false,
      criticalAlert: false,
      provisional: false,
    );

    if (settings.authorizationStatus == AuthorizationStatus.authorized) {
      print('User granted permission');
      
      // Get FCM token
      _fcmToken = await _messaging.getToken();
      setState(() {});
      
      // Initialize local notifications for foreground display
      await _initializeLocalNotifications();
      
      // Setup message handlers
      _setupMessageHandlers();
    }
  }

  Future<void> _initializeLocalNotifications() async {
    const AndroidInitializationSettings initializationSettingsAndroid =
        AndroidInitializationSettings('@mipmap/ic_launcher');

    const DarwinInitializationSettings initializationSettingsIOS =
        DarwinInitializationSettings();

    const InitializationSettings initializationSettings =
        InitializationSettings(
      android: initializationSettingsAndroid,
      iOS: initializationSettingsIOS,
    );

    await _localNotifications.initialize(initializationSettings);
  }

  void _setupMessageHandlers() {
    // Handle foreground messages
    FirebaseMessaging.onMessage.listen((RemoteMessage message) {
      setState(() {
        _messages.insert(0, message);
      });
      
      if (message.notification != null) {
        _showLocalNotification(message);
      }
    });

    // Handle message opened from terminated state
    FirebaseMessaging.onMessageOpenedApp.listen((RemoteMessage message) {
      setState(() {
        _messages.insert(0, message);
      });
      
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text('Opened from notification: ${message.notification?.title}'),
        ),
      );
    });
  }

  Future<void> _showLocalNotification(RemoteMessage message) async {
    const AndroidNotificationDetails androidPlatformChannelSpecifics =
        AndroidNotificationDetails(
      'firebase_channel',
      'Firebase Notifications',
      channelDescription: 'Firebase push notifications',
      importance: Importance.max,
      priority: Priority.high,
    );

    const NotificationDetails platformChannelSpecifics =
        NotificationDetails(android: androidPlatformChannelSpecifics);

    await _localNotifications.show(
      message.hashCode,
      message.notification?.title,
      message.notification?.body,
      platformChannelSpecifics,
    );
  }

  Future<void> _subscribeToTopic(String topic) async {
    await _messaging.subscribeToTopic(topic);
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(content: Text('Subscribed to topic: $topic')),
    );
  }

  Future<void> _unsubscribeFromTopic(String topic) async {
    await _messaging.unsubscribeFromTopic(topic);
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(content: Text('Unsubscribed from topic: $topic')),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Push Notifications'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'FCM Token',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 8),
                    if (_fcmToken != null) ...[
                      Text(
                        _fcmToken!,
                        style: const TextStyle(fontSize: 12),
                        maxLines: 3,
                        overflow: TextOverflow.ellipsis,
                      ),
                      const SizedBox(height: 8),
                      ElevatedButton.icon(
                        onPressed: () {
                          // Copy to clipboard functionality would go here
                          ScaffoldMessenger.of(context).showSnackBar(
                            const SnackBar(content: Text('Token copied to clipboard')),
                          );
                        },
                        icon: const Icon(Icons.copy),
                        label: const Text('Copy Token'),
                      ),
                    ] else
                      const Text('No token available'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Topic Subscriptions',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        Expanded(
                          child: ElevatedButton(
                            onPressed: () => _subscribeToTopic('news'),
                            child: const Text('Subscribe News'),
                          ),
                        ),
                        const SizedBox(width: 8),
                        Expanded(
                          child: ElevatedButton(
                            onPressed: () => _unsubscribeFromTopic('news'),
                            child: const Text('Unsubscribe News'),
                          ),
                        ),
                      ],
                    ),
                    const SizedBox(height: 8),
                    Row(
                      children: [
                        Expanded(
                          child: ElevatedButton(
                            onPressed: () => _subscribeToTopic('updates'),
                            child: const Text('Subscribe Updates'),
                          ),
                        ),
                        const SizedBox(width: 8),
                        Expanded(
                          child: ElevatedButton(
                            onPressed: () => _unsubscribeFromTopic('updates'),
                            child: const Text('Unsubscribe Updates'),
                          ),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Text(
              'Received Messages (${_messages.length})',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 8),
            Expanded(
              child: _messages.isEmpty
                  ? const Center(
                      child: Text('No messages received yet'),
                    )
                  : ListView.builder(
                      itemCount: _messages.length,
                      itemBuilder: (context, index) {
                        final message = _messages[index];
                        return Card(
                          child: ListTile(
                            leading: const Icon(Icons.message),
                            title: Text(
                              message.notification?.title ?? 'No title',
                              maxLines: 1,
                              overflow: TextOverflow.ellipsis,
                            ),
                            subtitle: Text(
                              message.notification?.body ?? message.data.toString(),
                              maxLines: 2,
                              overflow: TextOverflow.ellipsis,
                            ),
                            trailing: Text(
                              DateTime.now().toString().substring(11, 16),
                              style: Theme.of(context).textTheme.bodySmall,
                            ),
                          ),
                        );
                      },
                    ),
            ),
          ],
        ),
      ),
    );
  }
}
```

Firebase Cloud Messaging enables sending push notifications to your  
app from a server. This example demonstrates token management, topic  
subscriptions, and handling messages in different app states with  
proper foreground notification display.  

## Google Maps Integration

Integrating Google Maps with markers, custom info windows, and user interaction.  

```dart
import 'package:flutter/material.dart';
import 'package:google_maps_flutter/google_maps_flutter.dart';
import 'package:geolocator/geolocator.dart';

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
      title: 'Google Maps Integration',
      home: const MapsScreen(),
    );
  }
}

class MapsScreen extends StatefulWidget {
  const MapsScreen({super.key});

  @override
  State<MapsScreen> createState() => _MapsScreenState();
}

class _MapsScreenState extends State<MapsScreen> {
  GoogleMapController? _mapController;
  Position? _currentPosition;
  Set<Marker> _markers = {};
  Set<Polyline> _polylines = {};
  Set<Circle> _circles = {};
  MapType _currentMapType = MapType.normal;
  bool _isLoading = true;

  static const CameraPosition _initialPosition = CameraPosition(
    target: LatLng(37.7749, -122.4194), // San Francisco
    zoom: 12,
  );

  final List<LatLng> _routePoints = [
    const LatLng(37.7749, -122.4194),
    const LatLng(37.7849, -122.4094),
    const LatLng(37.7949, -122.3994),
    const LatLng(37.8049, -122.3894),
  ];

  @override
  void initState() {
    super.initState();
    _initializeMap();
  }

  Future<void> _initializeMap() async {
    await _getCurrentLocation();
    _createMarkers();
    _createPolylines();
    _createCircles();
    setState(() {
      _isLoading = false;
    });
  }

  Future<void> _getCurrentLocation() async {
    try {
      LocationPermission permission = await Geolocator.checkPermission();
      if (permission == LocationPermission.denied) {
        permission = await Geolocator.requestPermission();
        if (permission == LocationPermission.denied) {
          throw Exception('Location permissions are denied');
        }
      }

      _currentPosition = await Geolocator.getCurrentPosition(
        desiredAccuracy: LocationAccuracy.high,
      );
    } catch (e) {
      print('Error getting location: $e');
    }
  }

  void _createMarkers() {
    _markers = {
      const Marker(
        markerId: MarkerId('start'),
        position: LatLng(37.7749, -122.4194),
        infoWindow: InfoWindow(
          title: 'Start Point',
          snippet: 'This is where the journey begins',
        ),
        icon: BitmapDescriptor.defaultMarkerWithHue(BitmapDescriptor.hueGreen),
      ),
      const Marker(
        markerId: MarkerId('waypoint1'),
        position: LatLng(37.7849, -122.4094),
        infoWindow: InfoWindow(
          title: 'Waypoint 1',
          snippet: 'First stop on the route',
        ),
        icon: BitmapDescriptor.defaultMarkerWithHue(BitmapDescriptor.hueBlue),
      ),
      const Marker(
        markerId: MarkerId('waypoint2'),
        position: LatLng(37.7949, -122.3994),
        infoWindow: InfoWindow(
          title: 'Waypoint 2',
          snippet: 'Second stop on the route',
        ),
        icon: BitmapDescriptor.defaultMarkerWithHue(BitmapDescriptor.hueOrange),
      ),
      const Marker(
        markerId: MarkerId('end'),
        position: LatLng(37.8049, -122.3894),
        infoWindow: InfoWindow(
          title: 'End Point',
          snippet: 'Final destination reached',
        ),
        icon: BitmapDescriptor.defaultMarkerWithHue(BitmapDescriptor.hueRed),
      ),
    };

    if (_currentPosition != null) {
      _markers.add(
        Marker(
          markerId: const MarkerId('current_location'),
          position: LatLng(_currentPosition!.latitude, _currentPosition!.longitude),
          infoWindow: const InfoWindow(
            title: 'Your Location',
            snippet: 'You are here',
          ),
          icon: BitmapDescriptor.defaultMarkerWithHue(BitmapDescriptor.hueViolet),
        ),
      );
    }
  }

  void _createPolylines() {
    _polylines = {
      Polyline(
        polylineId: const PolylineId('route'),
        points: _routePoints,
        color: Colors.blue,
        width: 5,
        patterns: [PatternItem.dash(20), PatternItem.gap(20)],
      ),
    };
  }

  void _createCircles() {
    _circles = {
      const Circle(
        circleId: CircleId('area1'),
        center: LatLng(37.7749, -122.4194),
        radius: 1000,
        fillColor: Color.fromRGBO(255, 0, 0, 0.1),
        strokeColor: Colors.red,
        strokeWidth: 2,
      ),
      const Circle(
        circleId: CircleId('area2'),
        center: LatLng(37.8049, -122.3894),
        radius: 800,
        fillColor: Color.fromRGBO(0, 255, 0, 0.1),
        strokeColor: Colors.green,
        strokeWidth: 2,
      ),
    };
  }

  void _onMapCreated(GoogleMapController controller) {
    _mapController = controller;
  }

  void _changeMapType() {
    setState(() {
      _currentMapType = _currentMapType == MapType.normal
          ? MapType.satellite
          : _currentMapType == MapType.satellite
              ? MapType.hybrid
              : _currentMapType == MapType.hybrid
                  ? MapType.terrain
                  : MapType.normal;
    });
  }

  void _animateToCurrentLocation() {
    if (_currentPosition != null && _mapController != null) {
      _mapController!.animateCamera(
        CameraUpdate.newCameraPosition(
          CameraPosition(
            target: LatLng(_currentPosition!.latitude, _currentPosition!.longitude),
            zoom: 16,
          ),
        ),
      );
    }
  }

  void _onMapTapped(LatLng position) {
    final String markerId = 'tapped_${_markers.length}';
    setState(() {
      _markers.add(
        Marker(
          markerId: MarkerId(markerId),
          position: position,
          infoWindow: InfoWindow(
            title: 'Custom Marker',
            snippet: 'Lat: ${position.latitude.toStringAsFixed(4)}, '
                     'Lng: ${position.longitude.toStringAsFixed(4)}',
          ),
          icon: BitmapDescriptor.defaultMarkerWithHue(BitmapDescriptor.hueYellow),
        ),
      );
    });

    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(
          'Marker added at ${position.latitude.toStringAsFixed(4)}, '
          '${position.longitude.toStringAsFixed(4)}',
        ),
        duration: const Duration(seconds: 2),
      ),
    );
  }

  String _getMapTypeText() {
    switch (_currentMapType) {
      case MapType.normal:
        return 'Normal';
      case MapType.satellite:
        return 'Satellite';
      case MapType.hybrid:
        return 'Hybrid';
      case MapType.terrain:
        return 'Terrain';
      default:
        return 'Normal';
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Google Maps'),
        actions: [
          IconButton(
            onPressed: _changeMapType,
            icon: const Icon(Icons.layers),
            tooltip: 'Map Type: ${_getMapTypeText()}',
          ),
          IconButton(
            onPressed: _animateToCurrentLocation,
            icon: const Icon(Icons.my_location),
            tooltip: 'My Location',
          ),
        ],
      ),
      body: _isLoading
          ? const Center(child: CircularProgressIndicator())
          : GoogleMap(
              onMapCreated: _onMapCreated,
              initialCameraPosition: _initialPosition,
              mapType: _currentMapType,
              markers: _markers,
              polylines: _polylines,
              circles: _circles,
              onTap: _onMapTapped,
              myLocationEnabled: true,
              myLocationButtonEnabled: false,
              zoomControlsEnabled: false,
              compassEnabled: true,
              mapToolbarEnabled: false,
            ),
      bottomSheet: Container(
        height: 100,
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Map Controls',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                Expanded(
                  child: Text('Markers: ${_markers.length}'),
                ),
                Expanded(
                  child: Text('Mode: ${_getMapTypeText()}'),
                ),
                const Text('Tap map to add markers'),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
```

This Google Maps integration demonstrates comprehensive map functionality  
including multiple marker types, polylines for routes, circles for areas  
of interest, different map types, and user interaction for adding custom  
markers. It includes proper location permission handling and camera  
animation.  

## Location Services

Real-time location tracking with distance calculation and geofencing.  

```dart
import 'package:flutter/material.dart';
import 'package:geolocator/geolocator.dart';
import 'dart:async';

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
      title: 'Location Services',
      home: const LocationScreen(),
    );
  }
}

class LocationScreen extends StatefulWidget {
  const LocationScreen({super.key});

  @override
  State<LocationScreen> createState() => _LocationScreenState();
}

class _LocationScreenState extends State<LocationScreen> {
  Position? _currentPosition;
  Position? _previousPosition;
  StreamSubscription<Position>? _positionStream;
  double _totalDistance = 0.0;
  double _currentSpeed = 0.0;
  bool _isTracking = false;
  List<Position> _locationHistory = [];
  
  // Geofence example (Golden Gate Bridge area)
  static const LatLng _geofenceCenter = LatLng(37.8199, -122.4783);
  static const double _geofenceRadius = 100.0; // meters
  bool _isInGeofence = false;

  @override
  void initState() {
    super.initState();
    _checkLocationPermission();
  }

  @override
  void dispose() {
    _positionStream?.cancel();
    super.dispose();
  }

  Future<void> _checkLocationPermission() async {
    LocationPermission permission = await Geolocator.checkPermission();
    if (permission == LocationPermission.denied) {
      permission = await Geolocator.requestPermission();
      if (permission == LocationPermission.denied) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Location permissions denied')),
        );
        return;
      }
    }

    if (permission == LocationPermission.deniedForever) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Location permissions permanently denied')),
      );
      return;
    }

    _getCurrentLocation();
  }

  Future<void> _getCurrentLocation() async {
    try {
      Position position = await Geolocator.getCurrentPosition(
        desiredAccuracy: LocationAccuracy.high,
      );
      setState(() {
        _currentPosition = position;
        _currentSpeed = position.speed * 3.6; // Convert m/s to km/h
      });
      _checkGeofence(position);
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error getting location: $e')),
      );
    }
  }

  void _startLocationTracking() {
    if (_isTracking) return;

    const LocationSettings locationSettings = LocationSettings(
      accuracy: LocationAccuracy.high,
      distanceFilter: 5, // Update every 5 meters
    );

    _positionStream = Geolocator.getPositionStream(
      locationSettings: locationSettings,
    ).listen(
      (Position position) {
        setState(() {
          _previousPosition = _currentPosition;
          _currentPosition = position;
          _currentSpeed = position.speed * 3.6; // Convert to km/h
          _locationHistory.add(position);
          
          if (_previousPosition != null) {
            double distance = Geolocator.distanceBetween(
              _previousPosition!.latitude,
              _previousPosition!.longitude,
              position.latitude,
              position.longitude,
            );
            _totalDistance += distance;
          }
        });
        
        _checkGeofence(position);
      },
      onError: (e) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Location stream error: $e')),
        );
      },
    );

    setState(() {
      _isTracking = true;
    });
  }

  void _stopLocationTracking() {
    _positionStream?.cancel();
    setState(() {
      _isTracking = false;
    });
  }

  void _resetTracking() {
    _stopLocationTracking();
    setState(() {
      _totalDistance = 0.0;
      _currentSpeed = 0.0;
      _locationHistory.clear();
      _previousPosition = null;
    });
  }

  void _checkGeofence(Position position) {
    double distance = Geolocator.distanceBetween(
      position.latitude,
      position.longitude,
      _geofenceCenter.latitude,
      _geofenceCenter.longitude,
    );

    bool wasInGeofence = _isInGeofence;
    _isInGeofence = distance <= _geofenceRadius;

    if (!wasInGeofence && _isInGeofence) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Entered geofence area!'),
          backgroundColor: Colors.green,
        ),
      );
    } else if (wasInGeofence && !_isInGeofence) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Left geofence area!'),
          backgroundColor: Colors.orange,
        ),
      );
    }
  }

  String _formatDistance(double distance) {
    if (distance < 1000) {
      return '${distance.toStringAsFixed(1)} m';
    } else {
      return '${(distance / 1000).toStringAsFixed(2)} km';
    }
  }

  String _getAccuracyDescription(double accuracy) {
    if (accuracy < 5) return 'Excellent';
    if (accuracy < 10) return 'Good';
    if (accuracy < 20) return 'Fair';
    return 'Poor';
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Location Services'),
        actions: [
          IconButton(
            onPressed: _resetTracking,
            icon: const Icon(Icons.refresh),
            tooltip: 'Reset tracking',
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Card(
              color: _isInGeofence ? Colors.green.withOpacity(0.2) : null,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        Icon(
                          _isInGeofence ? Icons.location_on : Icons.location_off,
                          color: _isInGeofence ? Colors.green : null,
                        ),
                        const SizedBox(width: 8),
                        Text(
                          'Current Position',
                          style: Theme.of(context).textTheme.titleLarge,
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    if (_currentPosition != null) ...[
                      Text(
                        'Latitude: ${_currentPosition!.latitude.toStringAsFixed(6)}',
                      ),
                      Text(
                        'Longitude: ${_currentPosition!.longitude.toStringAsFixed(6)}',
                      ),
                      Text(
                        'Altitude: ${_currentPosition!.altitude.toStringAsFixed(1)} m',
                      ),
                      Text(
                        'Accuracy: ${_currentPosition!.accuracy.toStringAsFixed(1)} m '
                        '(${_getAccuracyDescription(_currentPosition!.accuracy)})',
                      ),
                      Text(
                        'Geofence Status: ${_isInGeofence ? "Inside" : "Outside"}',
                        style: TextStyle(
                          color: _isInGeofence ? Colors.green : Colors.orange,
                          fontWeight: FontWeight.w500,
                        ),
                      ),
                    ] else
                      const Text('Location not available'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        Icon(
                          _isTracking ? Icons.gps_fixed : Icons.gps_not_fixed,
                          color: _isTracking ? Colors.blue : null,
                        ),
                        const SizedBox(width: 8),
                        Text(
                          'Tracking Information',
                          style: Theme.of(context).textTheme.titleLarge,
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        Expanded(
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Text(
                                'Total Distance',
                                style: Theme.of(context).textTheme.labelMedium,
                              ),
                              Text(
                                _formatDistance(_totalDistance),
                                style: const TextStyle(
                                  fontSize: 20,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                            ],
                          ),
                        ),
                        Expanded(
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Text(
                                'Current Speed',
                                style: Theme.of(context).textTheme.labelMedium,
                              ),
                              Text(
                                '${_currentSpeed.toStringAsFixed(1)} km/h',
                                style: const TextStyle(
                                  fontSize: 20,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                            ],
                          ),
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Text(
                      'Location Points: ${_locationHistory.length}',
                      style: Theme.of(context).textTheme.labelMedium,
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Geofence Area',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 8),
                    Text(
                      'Center: ${_geofenceCenter.latitude.toStringAsFixed(4)}, '
                      '${_geofenceCenter.longitude.toStringAsFixed(4)}',
                    ),
                    Text('Radius: ${_geofenceRadius.toInt()} meters'),
                    if (_currentPosition != null) ...[
                      const SizedBox(height: 8),
                      Text(
                        'Distance to center: ${_formatDistance(
                          Geolocator.distanceBetween(
                            _currentPosition!.latitude,
                            _currentPosition!.longitude,
                            _geofenceCenter.latitude,
                            _geofenceCenter.longitude,
                          ),
                        )}',
                      ),
                    ],
                  ],
                ),
              ),
            ),
            const Spacer(),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton.icon(
                    onPressed: _getCurrentLocation,
                    icon: const Icon(Icons.location_searching),
                    label: const Text('Get Location'),
                  ),
                ),
                const SizedBox(width: 16),
                Expanded(
                  child: ElevatedButton.icon(
                    onPressed: _isTracking ? _stopLocationTracking : _startLocationTracking,
                    icon: Icon(_isTracking ? Icons.stop : Icons.play_arrow),
                    label: Text(_isTracking ? 'Stop Tracking' : 'Start Tracking'),
                    style: ElevatedButton.styleFrom(
                      backgroundColor: _isTracking ? Colors.red : Colors.green,
                    ),
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}

class LatLng {
  final double latitude;
  final double longitude;
  
  const LatLng(this.latitude, this.longitude);
}
```

This location services example provides comprehensive GPS functionality  
including real-time tracking, distance calculation, speed monitoring,  
and geofencing capabilities. It demonstrates proper permission handling  
and efficient position stream management with visual feedback.  

## Device Information

Accessing and displaying comprehensive device and platform information.  

```dart
import 'package:flutter/material.dart';
import 'package:device_info_plus/device_info_plus.dart';
import 'package:package_info_plus/package_info_plus.dart';
import 'package:connectivity_plus/connectivity_plus.dart';
import 'package:battery_plus/battery_plus.dart';
import 'dart:io';

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
      title: 'Device Information',
      home: const DeviceInfoScreen(),
    );
  }
}

class DeviceInfoScreen extends StatefulWidget {
  const DeviceInfoScreen({super.key});

  @override
  State<DeviceInfoScreen> createState() => _DeviceInfoScreenState();
}

class _DeviceInfoScreenState extends State<DeviceInfoScreen> {
  final DeviceInfoPlugin _deviceInfoPlugin = DeviceInfoPlugin();
  final Battery _battery = Battery();
  
  Map<String, dynamic> _deviceData = {};
  PackageInfo? _packageInfo;
  List<ConnectivityResult> _connectionStatus = [];
  BatteryState _batteryState = BatteryState.unknown;
  int _batteryLevel = 0;
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    _loadDeviceInfo();
  }

  Future<void> _loadDeviceInfo() async {
    try {
      await Future.wait([
        _loadDeviceData(),
        _loadPackageInfo(),
        _loadConnectivityInfo(),
        _loadBatteryInfo(),
      ]);
    } catch (e) {
      print('Error loading device info: $e');
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _loadDeviceData() async {
    try {
      if (Platform.isAndroid) {
        final androidInfo = await _deviceInfoPlugin.androidInfo;
        _deviceData = {
          'Platform': 'Android',
          'Model': androidInfo.model,
          'Manufacturer': androidInfo.manufacturer,
          'Brand': androidInfo.brand,
          'Product': androidInfo.product,
          'Device': androidInfo.device,
          'Hardware': androidInfo.hardware,
          'Android Version': androidInfo.version.release,
          'SDK Version': androidInfo.version.sdkInt.toString(),
          'Security Patch': androidInfo.version.securityPatch ?? 'Unknown',
          'Bootloader': androidInfo.bootloader,
          'Build ID': androidInfo.id,
          'Fingerprint': androidInfo.fingerprint,
          'Host': androidInfo.host,
          'Tags': androidInfo.tags,
          'Type': androidInfo.type,
          'Is Physical Device': androidInfo.isPhysicalDevice.toString(),
          'Supported ABIs': androidInfo.supportedAbis.join(', '),
          'System Features': androidInfo.systemFeatures.join(', '),
        };
      } else if (Platform.isIOS) {
        final iosInfo = await _deviceInfoPlugin.iosInfo;
        _deviceData = {
          'Platform': 'iOS',
          'Name': iosInfo.name,
          'Model': iosInfo.model,
          'System Name': iosInfo.systemName,
          'System Version': iosInfo.systemVersion,
          'Localized Model': iosInfo.localizedModel,
          'Identifier for Vendor': iosInfo.identifierForVendor ?? 'Unknown',
          'Is Physical Device': iosInfo.isPhysicalDevice.toString(),
          'UTC Offset Name': iosInfo.utsname.nodename,
          'System': iosInfo.utsname.sysname,
          'Release': iosInfo.utsname.release,
          'Version': iosInfo.utsname.version,
          'Machine': iosInfo.utsname.machine,
        };
      } else {
        _deviceData = {
          'Platform': Platform.operatingSystem,
          'Version': Platform.operatingSystemVersion,
          'Locale': Platform.localeName,
          'Number of Processors': Platform.numberOfProcessors.toString(),
          'Path Separator': Platform.pathSeparator,
          'Script': Platform.script.toString(),
          'Executable': Platform.executable,
          'Resolved Executable': Platform.resolvedExecutable,
        };
      }
    } catch (e) {
      _deviceData = {'Error': 'Failed to get device info: $e'};
    }
  }

  Future<void> _loadPackageInfo() async {
    _packageInfo = await PackageInfo.fromPlatform();
  }

  Future<void> _loadConnectivityInfo() async {
    _connectionStatus = await Connectivity().checkConnectivity();
    
    Connectivity().onConnectivityChanged.listen((List<ConnectivityResult> result) {
      setState(() {
        _connectionStatus = result;
      });
    });
  }

  Future<void> _loadBatteryInfo() async {
    _batteryLevel = await _battery.batteryLevel;
    _batteryState = await _battery.batteryState;
    
    _battery.onBatteryStateChanged.listen((BatteryState state) {
      setState(() {
        _batteryState = state;
      });
    });
  }

  String _getConnectivityText() {
    if (_connectionStatus.isEmpty) return 'Unknown';
    return _connectionStatus.map((result) {
      switch (result) {
        case ConnectivityResult.wifi:
          return 'WiFi';
        case ConnectivityResult.mobile:
          return 'Mobile Data';
        case ConnectivityResult.ethernet:
          return 'Ethernet';
        case ConnectivityResult.vpn:
          return 'VPN';
        case ConnectivityResult.bluetooth:
          return 'Bluetooth';
        case ConnectivityResult.other:
          return 'Other';
        case ConnectivityResult.none:
          return 'No Connection';
        default:
          return 'Unknown';
      }
    }).join(', ');
  }

  String _getBatteryStateText() {
    switch (_batteryState) {
      case BatteryState.charging:
        return 'Charging';
      case BatteryState.discharging:
        return 'Discharging';
      case BatteryState.full:
        return 'Full';
      case BatteryState.connectedNotCharging:
        return 'Connected (Not Charging)';
      case BatteryState.unknown:
        return 'Unknown';
      default:
        return 'Unknown';
    }
  }

  Color _getBatteryColor() {
    if (_batteryLevel > 50) return Colors.green;
    if (_batteryLevel > 20) return Colors.orange;
    return Colors.red;
  }

  IconData _getBatteryIcon() {
    if (_batteryState == BatteryState.charging) return Icons.battery_charging_full;
    if (_batteryLevel > 80) return Icons.battery_full;
    if (_batteryLevel > 60) return Icons.battery_5_bar;
    if (_batteryLevel > 40) return Icons.battery_4_bar;
    if (_batteryLevel > 20) return Icons.battery_3_bar;
    if (_batteryLevel > 10) return Icons.battery_2_bar;
    return Icons.battery_1_bar;
  }

  @override
  Widget build(BuildContext context) {
    if (_isLoading) {
      return const Scaffold(
        body: Center(child: CircularProgressIndicator()),
      );
    }

    return Scaffold(
      appBar: AppBar(
        title: const Text('Device Information'),
        actions: [
          IconButton(
            onPressed: () {
              setState(() {
                _isLoading = true;
              });
              _loadDeviceInfo();
            },
            icon: const Icon(Icons.refresh),
            tooltip: 'Refresh',
          ),
        ],
      ),
      body: ListView(
        padding: const EdgeInsets.all(16),
        children: [
          if (_packageInfo != null) ...[
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        const Icon(Icons.info),
                        const SizedBox(width: 8),
                        Text(
                          'App Information',
                          style: Theme.of(context).textTheme.titleLarge,
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    _buildInfoRow('App Name', _packageInfo!.appName),
                    _buildInfoRow('Package Name', _packageInfo!.packageName),
                    _buildInfoRow('Version', _packageInfo!.version),
                    _buildInfoRow('Build Number', _packageInfo!.buildNumber),
                    if (_packageInfo!.buildSignature.isNotEmpty)
                      _buildInfoRow('Build Signature', 
                        _packageInfo!.buildSignature.substring(0, 20) + '...'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
          ],
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Row(
                    children: [
                      const Icon(Icons.network_check),
                      const SizedBox(width: 8),
                      Text(
                        'Network & Battery',
                        style: Theme.of(context).textTheme.titleLarge,
                      ),
                    ],
                  ),
                  const SizedBox(height: 12),
                  Row(
                    children: [
                      Expanded(
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(
                              'Connectivity',
                              style: Theme.of(context).textTheme.labelMedium,
                            ),
                            Text(
                              _getConnectivityText(),
                              style: const TextStyle(fontSize: 16),
                            ),
                          ],
                        ),
                      ),
                      Column(
                        children: [
                          Icon(
                            _getBatteryIcon(),
                            color: _getBatteryColor(),
                            size: 32,
                          ),
                          Text(
                            '$_batteryLevel%',
                            style: TextStyle(
                              color: _getBatteryColor(),
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                          Text(
                            _getBatteryStateText(),
                            style: Theme.of(context).textTheme.bodySmall,
                          ),
                        ],
                      ),
                    ],
                  ),
                ],
              ),
            ),
          ),
          const SizedBox(height: 16),
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Row(
                    children: [
                      const Icon(Icons.phone_android),
                      const SizedBox(width: 8),
                      Text(
                        'Device Details',
                        style: Theme.of(context).textTheme.titleLarge,
                      ),
                    ],
                  ),
                  const SizedBox(height: 12),
                  ..._deviceData.entries.map((entry) => _buildInfoRow(
                    entry.key,
                    entry.value.toString(),
                  )).toList(),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildInfoRow(String label, String value) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          SizedBox(
            width: 120,
            child: Text(
              label,
              style: const TextStyle(
                fontWeight: FontWeight.w500,
                fontSize: 14,
              ),
            ),
          ),
          Expanded(
            child: Text(
              value,
              style: const TextStyle(fontSize: 14),
            ),
          ),
        ],
      ),
    );
  }
}
```

This comprehensive device information example demonstrates accessing  
various device details, app metadata, connectivity status, and battery  
information. It provides real-time updates for dynamic properties and  
handles platform-specific information appropriately.  

## File System Access

Reading, writing, and managing files with proper permissions and error handling.  

```dart
import 'package:flutter/material.dart';
import 'package:path_provider/path_provider.dart';
import 'package:permission_handler/permission_handler.dart';
import 'dart:io';

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
      title: 'File System Access',
      home: const FileSystemScreen(),
    );
  }
}

class FileSystemScreen extends StatefulWidget {
  const FileSystemScreen({super.key});

  @override
  State<FileSystemScreen> createState() => _FileSystemScreenState();
}

class _FileSystemScreenState extends State<FileSystemScreen> {
  final TextEditingController _textController = TextEditingController();
  final TextEditingController _filenameController = TextEditingController();
  String _fileContent = '';
  List<FileSystemEntity> _files = [];
  Directory? _currentDirectory;
  bool _isLoading = false;

  @override
  void initState() {
    super.initState();
    _filenameController.text = 'example.txt';
    _initializeFileSystem();
  }

  @override
  void dispose() {
    _textController.dispose();
    _filenameController.dispose();
    super.dispose();
  }

  Future<void> _initializeFileSystem() async {
    await _requestPermissions();
    await _loadDocumentsDirectory();
  }

  Future<void> _requestPermissions() async {
    if (Platform.isAndroid) {
      await [
        Permission.storage,
        Permission.manageExternalStorage,
      ].request();
    }
  }

  Future<void> _loadDocumentsDirectory() async {
    setState(() {
      _isLoading = true;
    });

    try {
      _currentDirectory = await getApplicationDocumentsDirectory();
      await _refreshFileList();
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error accessing directory: $e')),
      );
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _refreshFileList() async {
    if (_currentDirectory == null) return;

    try {
      final List<FileSystemEntity> files = _currentDirectory!
          .listSync()
          .where((entity) => entity is File)
          .toList();
      
      setState(() {
        _files = files;
      });
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error listing files: $e')),
      );
    }
  }

  Future<void> _writeFile() async {
    if (_currentDirectory == null || _filenameController.text.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Please enter a filename')),
      );
      return;
    }

    try {
      final File file = File('${_currentDirectory!.path}/${_filenameController.text}');
      await file.writeAsString(_textController.text);
      
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('File saved: ${file.path}')),
      );
      
      await _refreshFileList();
      _textController.clear();
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error writing file: $e')),
      );
    }
  }

  Future<void> _readFile(String filename) async {
    if (_currentDirectory == null) return;

    try {
      final File file = File('${_currentDirectory!.path}/$filename');
      if (await file.exists()) {
        final String content = await file.readAsString();
        setState(() {
          _fileContent = content;
        });
        _showFileContentDialog(filename, content);
      } else {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('File does not exist')),
        );
      }
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error reading file: $e')),
      );
    }
  }

  Future<void> _deleteFile(String filename) async {
    if (_currentDirectory == null) return;

    try {
      final File file = File('${_currentDirectory!.path}/$filename');
      if (await file.exists()) {
        await file.delete();
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('File deleted: $filename')),
        );
        await _refreshFileList();
      }
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error deleting file: $e')),
      );
    }
  }

  Future<void> _createDirectory(String name) async {
    if (_currentDirectory == null || name.isEmpty) return;

    try {
      final Directory newDir = Directory('${_currentDirectory!.path}/$name');
      await newDir.create();
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Directory created: $name')),
      );
      await _refreshFileList();
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error creating directory: $e')),
      );
    }
  }

  void _showFileContentDialog(String filename, String content) {
    showDialog(
      context: context,
      builder: (BuildContext context) {
        return AlertDialog(
          title: Text('File: $filename'),
          content: SingleChildScrollView(
            child: Text(
              content.isEmpty ? 'File is empty' : content,
              style: const TextStyle(fontFamily: 'monospace'),
            ),
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.of(context).pop(),
              child: const Text('Close'),
            ),
            TextButton(
              onPressed: () {
                Navigator.of(context).pop();
                _textController.text = content;
                _filenameController.text = filename;
              },
              child: const Text('Edit'),
            ),
          ],
        );
      },
    );
  }

  void _showCreateDirectoryDialog() {
    final TextEditingController dirController = TextEditingController();
    
    showDialog(
      context: context,
      builder: (BuildContext context) {
        return AlertDialog(
          title: const Text('Create Directory'),
          content: TextField(
            controller: dirController,
            decoration: const InputDecoration(
              labelText: 'Directory Name',
              hintText: 'Enter directory name',
            ),
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.of(context).pop(),
              child: const Text('Cancel'),
            ),
            TextButton(
              onPressed: () {
                Navigator.of(context).pop();
                _createDirectory(dirController.text);
              },
              child: const Text('Create'),
            ),
          ],
        );
      },
    );
  }

  String _formatFileSize(int bytes) {
    if (bytes < 1024) return '$bytes B';
    if (bytes < 1024 * 1024) return '${(bytes / 1024).toStringAsFixed(1)} KB';
    return '${(bytes / (1024 * 1024)).toStringAsFixed(1)} MB';
  }

  String _formatDate(DateTime date) {
    return '${date.day}/${date.month}/${date.year} ${date.hour}:${date.minute.toString().padLeft(2, '0')}';
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('File System'),
        actions: [
          IconButton(
            onPressed: _refreshFileList,
            icon: const Icon(Icons.refresh),
            tooltip: 'Refresh',
          ),
          IconButton(
            onPressed: _showCreateDirectoryDialog,
            icon: const Icon(Icons.create_new_folder),
            tooltip: 'Create Directory',
          ),
        ],
      ),
      body: _isLoading
          ? const Center(child: CircularProgressIndicator())
          : Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Card(
                    child: Padding(
                      padding: const EdgeInsets.all(16),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            'Write New File',
                            style: Theme.of(context).textTheme.titleMedium,
                          ),
                          const SizedBox(height: 12),
                          TextField(
                            controller: _filenameController,
                            decoration: const InputDecoration(
                              labelText: 'Filename',
                              hintText: 'Enter filename with extension',
                              prefixIcon: Icon(Icons.description),
                            ),
                          ),
                          const SizedBox(height: 12),
                          TextField(
                            controller: _textController,
                            maxLines: 4,
                            decoration: const InputDecoration(
                              labelText: 'File Content',
                              hintText: 'Enter the content to save',
                              border: OutlineInputBorder(),
                            ),
                          ),
                          const SizedBox(height: 12),
                          ElevatedButton.icon(
                            onPressed: _writeFile,
                            icon: const Icon(Icons.save),
                            label: const Text('Save File'),
                          ),
                        ],
                      ),
                    ),
                  ),
                  const SizedBox(height: 16),
                  Card(
                    child: Padding(
                      padding: const EdgeInsets.all(16),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Row(
                            children: [
                              Text(
                                'Files (${_files.length})',
                                style: Theme.of(context).textTheme.titleMedium,
                              ),
                              const Spacer(),
                              if (_currentDirectory != null)
                                Text(
                                  _currentDirectory!.path.split('/').last,
                                  style: Theme.of(context).textTheme.bodySmall,
                                ),
                            ],
                          ),
                          const SizedBox(height: 8),
                          if (_currentDirectory != null)
                            Text(
                              'Directory: ${_currentDirectory!.path}',
                              style: Theme.of(context).textTheme.bodySmall,
                              maxLines: 2,
                              overflow: TextOverflow.ellipsis,
                            ),
                        ],
                      ),
                    ),
                  ),
                  const SizedBox(height: 8),
                  Expanded(
                    child: _files.isEmpty
                        ? const Center(
                            child: Text('No files found'),
                          )
                        : ListView.builder(
                            itemCount: _files.length,
                            itemBuilder: (context, index) {
                              final file = _files[index] as File;
                              final filename = file.path.split('/').last;
                              
                              return FutureBuilder<FileStat>(
                                future: file.stat(),
                                builder: (context, snapshot) {
                                  final stat = snapshot.data;
                                  
                                  return Card(
                                    margin: const EdgeInsets.symmetric(vertical: 2),
                                    child: ListTile(
                                      leading: const Icon(Icons.description),
                                      title: Text(filename),
                                      subtitle: stat != null
                                          ? Text(
                                              '${_formatFileSize(stat.size)} • '
                                              '${_formatDate(stat.modified)}',
                                            )
                                          : const Text('Loading...'),
                                      trailing: PopupMenuButton<String>(
                                        onSelected: (value) {
                                          switch (value) {
                                            case 'read':
                                              _readFile(filename);
                                              break;
                                            case 'delete':
                                              _deleteFile(filename);
                                              break;
                                          }
                                        },
                                        itemBuilder: (context) => [
                                          const PopupMenuItem(
                                            value: 'read',
                                            child: Row(
                                              children: [
                                                Icon(Icons.visibility),
                                                SizedBox(width: 8),
                                                Text('Read'),
                                              ],
                                            ),
                                          ),
                                          const PopupMenuItem(
                                            value: 'delete',
                                            child: Row(
                                              children: [
                                                Icon(Icons.delete, color: Colors.red),
                                                SizedBox(width: 8),
                                                Text('Delete'),
                                              ],
                                            ),
                                          ),
                                        ],
                                      ),
                                    ),
                                  );
                                },
                              );
                            },
                          ),
                  ),
                ],
              ),
            ),
    );
  }
}
```

This file system example demonstrates comprehensive file operations  
including reading, writing, deleting files, and directory management.  
It includes proper permission handling, file metadata display, and  
user-friendly interfaces for file manipulation.  

## Platform Channels

Creating custom platform channels for native Android/iOS integration.  

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
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'Platform Channels',
      home: const PlatformChannelsScreen(),
    );
  }
}

class PlatformChannelsScreen extends StatefulWidget {
  const PlatformChannelsScreen({super.key});

  @override
  State<PlatformChannelsScreen> createState() => _PlatformChannelsScreenState();
}

class _PlatformChannelsScreenState extends State<PlatformChannelsScreen> {
  static const MethodChannel _methodChannel = 
      MethodChannel('com.example.platform_channels/methods');
  static const EventChannel _eventChannel = 
      EventChannel('com.example.platform_channels/events');
  static const BasicMessageChannel _messageChannel = 
      BasicMessageChannel('com.example.platform_channels/messages', 
      StandardMessageCodec());

  String _methodResult = 'No result yet';
  String _eventData = 'No events received';
  String _messageResult = 'No messages sent';
  List<String> _eventHistory = [];

  @override
  void initState() {
    super.initState();
    _setupEventChannel();
    _setupMessageChannel();
  }

  void _setupEventChannel() {
    try {
      _eventChannel.receiveBroadcastStream().listen(
        (dynamic event) {
          setState(() {
            _eventData = event.toString();
            _eventHistory.insert(0, 
              '${DateTime.now().toString().substring(11, 19)}: $event');
            if (_eventHistory.length > 10) {
              _eventHistory.removeLast();
            }
          });
        },
        onError: (dynamic error) {
          setState(() {
            _eventData = 'Error: ${error.message}';
          });
        },
      );
    } catch (e) {
      setState(() {
        _eventData = 'Failed to setup event channel: $e';
      });
    }
  }

  void _setupMessageChannel() {
    _messageChannel.setMessageHandler((dynamic message) async {
      setState(() {
        _messageResult = 'Received: ${message.toString()}';
      });
      return 'Flutter received: $message';
    });
  }

  Future<void> _invokeNativeMethod(String method) async {
    try {
      final dynamic result = await _methodChannel.invokeMethod(method);
      setState(() {
        _methodResult = result.toString();
      });
    } on PlatformException catch (e) {
      setState(() {
        _methodResult = 'Error: ${e.message}';
      });
    } catch (e) {
      setState(() {
        _methodResult = 'Unexpected error: $e';
      });
    }
  }

  Future<void> _invokeMethodWithArguments() async {
    try {
      final Map<String, dynamic> arguments = {
        'name': 'Flutter User',
        'age': 25,
        'isActive': true,
        'preferences': ['dark_mode', 'notifications'],
      };

      final dynamic result = await _methodChannel.invokeMethod(
        'processUserData',
        arguments,
      );
      
      setState(() {
        _methodResult = result.toString();
      });
    } on PlatformException catch (e) {
      setState(() {
        _methodResult = 'Error: ${e.message}';
      });
    } catch (e) {
      setState(() {
        _methodResult = 'Unexpected error: $e';
      });
    }
  }

  Future<void> _sendMessage(String message) async {
    try {
      final dynamic reply = await _messageChannel.send(message);
      setState(() {
        _messageResult = 'Sent: "$message", Reply: "$reply"';
      });
    } catch (e) {
      setState(() {
        _messageResult = 'Message error: $e';
      });
    }
  }

  Future<void> _performAsyncOperation() async {
    setState(() {
      _methodResult = 'Performing async operation...';
    });

    try {
      final dynamic result = await _methodChannel.invokeMethod('longRunningTask');
      setState(() {
        _methodResult = 'Async result: $result';
      });
    } on PlatformException catch (e) {
      setState(() {
        _methodResult = 'Async error: ${e.message}';
      });
    }
  }

  Widget _buildMethodChannelSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                const Icon(Icons.call_made),
                const SizedBox(width: 8),
                Text(
                  'Method Channel',
                  style: Theme.of(context).textTheme.titleLarge,
                ),
              ],
            ),
            const SizedBox(height: 12),
            Text(
              'Result: $_methodResult',
              style: const TextStyle(
                fontFamily: 'monospace',
                fontSize: 12,
              ),
            ),
            const SizedBox(height: 12),
            Wrap(
              spacing: 8,
              runSpacing: 8,
              children: [
                ElevatedButton(
                  onPressed: () => _invokeNativeMethod('getPlatformVersion'),
                  child: const Text('Get Platform Version'),
                ),
                ElevatedButton(
                  onPressed: () => _invokeNativeMethod('getDeviceId'),
                  child: const Text('Get Device ID'),
                ),
                ElevatedButton(
                  onPressed: () => _invokeNativeMethod('showNativeToast'),
                  child: const Text('Show Native Toast'),
                ),
                ElevatedButton(
                  onPressed: _invokeMethodWithArguments,
                  child: const Text('Send Complex Data'),
                ),
                ElevatedButton(
                  onPressed: _performAsyncOperation,
                  child: const Text('Async Operation'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildEventChannelSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                const Icon(Icons.stream),
                const SizedBox(width: 8),
                Text(
                  'Event Channel',
                  style: Theme.of(context).textTheme.titleLarge,
                ),
              ],
            ),
            const SizedBox(height: 12),
            Text(
              'Latest Event: $_eventData',
              style: const TextStyle(
                fontFamily: 'monospace',
                fontSize: 12,
              ),
            ),
            if (_eventHistory.isNotEmpty) ...[
              const SizedBox(height: 8),
              Text(
                'Event History:',
                style: Theme.of(context).textTheme.labelMedium,
              ),
              Container(
                height: 100,
                margin: const EdgeInsets.symmetric(vertical: 8),
                padding: const EdgeInsets.all(8),
                decoration: BoxDecoration(
                  border: Border.all(color: Colors.grey),
                  borderRadius: BorderRadius.circular(4),
                ),
                child: ListView.builder(
                  itemCount: _eventHistory.length,
                  itemBuilder: (context, index) {
                    return Text(
                      _eventHistory[index],
                      style: const TextStyle(
                        fontSize: 10,
                        fontFamily: 'monospace',
                      ),
                    );
                  },
                ),
              ),
            ],
            ElevatedButton(
              onPressed: () {
                setState(() {
                  _eventHistory.clear();
                });
              },
              child: const Text('Clear History'),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildMessageChannelSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                const Icon(Icons.message),
                const SizedBox(width: 8),
                Text(
                  'Message Channel',
                  style: Theme.of(context).textTheme.titleLarge,
                ),
              ],
            ),
            const SizedBox(height: 12),
            Text(
              'Status: $_messageResult',
              style: const TextStyle(
                fontFamily: 'monospace',
                fontSize: 12,
              ),
            ),
            const SizedBox(height: 12),
            Wrap(
              spacing: 8,
              runSpacing: 8,
              children: [
                ElevatedButton(
                  onPressed: () => _sendMessage('Hello from Flutter!'),
                  child: const Text('Send Hello'),
                ),
                ElevatedButton(
                  onPressed: () => _sendMessage('Ping'),
                  child: const Text('Send Ping'),
                ),
                ElevatedButton(
                  onPressed: () => _sendMessage(
                    '{"type": "json", "data": {"key": "value"}}',
                  ),
                  child: const Text('Send JSON'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Platform Channels'),
        actions: [
          IconButton(
            onPressed: () {
              showDialog(
                context: context,
                builder: (context) => AlertDialog(
                  title: const Text('Platform Channel Info'),
                  content: const Text(
                    'This example demonstrates different types of platform '
                    'channels for communicating between Flutter and native code:\n\n'
                    '• Method Channel: Invoke native methods\n'
                    '• Event Channel: Stream events from native\n'
                    '• Message Channel: Bidirectional messaging',
                  ),
                  actions: [
                    TextButton(
                      onPressed: () => Navigator.pop(context),
                      child: const Text('OK'),
                    ),
                  ],
                ),
              );
            },
            icon: const Icon(Icons.info),
          ),
        ],
      ),
      body: ListView(
        padding: const EdgeInsets.all(16),
        children: [
          _buildMethodChannelSection(),
          const SizedBox(height: 16),
          _buildEventChannelSection(),
          const SizedBox(height: 16),
          _buildMessageChannelSection(),
        ],
      ),
    );
  }
}
```

Platform channels enable communication between Flutter and native  
platform code. This example demonstrates method channels for invoking  
native functions, event channels for streaming data from native code,  
and message channels for bidirectional communication.  

## Biometric Authentication

Implementing fingerprint and face recognition authentication.  

```dart
import 'package:flutter/material.dart';
import 'package:local_auth/local_auth.dart';
import 'package:local_auth_android/local_auth_android.dart';
import 'package:local_auth_ios/local_auth_ios.dart';

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
      title: 'Biometric Authentication',
      home: const BiometricAuthScreen(),
    );
  }
}

class BiometricAuthScreen extends StatefulWidget {
  const BiometricAuthScreen({super.key});

  @override
  State<BiometricAuthScreen> createState() => _BiometricAuthScreenState();
}

class _BiometricAuthScreenState extends State<BiometricAuthScreen> {
  final LocalAuthentication _localAuth = LocalAuthentication();
  bool _isBiometricAvailable = false;
  List<BiometricType> _availableBiometrics = [];
  String _authStatus = 'Not authenticated';
  bool _isAuthenticating = false;

  @override
  void initState() {
    super.initState();
    _checkBiometricAvailability();
  }

  Future<void> _checkBiometricAvailability() async {
    try {
      final bool isAvailable = await _localAuth.canCheckBiometrics;
      final bool isDeviceSupported = await _localAuth.isDeviceSupported();
      final List<BiometricType> availableBiometrics = 
          await _localAuth.getAvailableBiometrics();

      setState(() {
        _isBiometricAvailable = isAvailable && isDeviceSupported;
        _availableBiometrics = availableBiometrics;
      });
    } catch (e) {
      setState(() {
        _authStatus = 'Error checking biometric availability: $e';
      });
    }
  }

  Future<void> _authenticateWithBiometric() async {
    if (!_isBiometricAvailable) {
      setState(() {
        _authStatus = 'Biometric authentication not available';
      });
      return;
    }

    setState(() {
      _isAuthenticating = true;
      _authStatus = 'Authenticating...';
    });

    try {
      final bool didAuthenticate = await _localAuth.authenticate(
        localizedReason: 'Please authenticate to access secure content',
        authMessages: const [
          AndroidAuthMessages(
            signInTitle: 'Biometric Authentication',
            cancelButton: 'Cancel',
            deviceCredentialsRequiredTitle: 'Device Credential Required',
            deviceCredentialsSetupDescription: 'Please set up device credentials',
            goToSettingsButton: 'Go to Settings',
            goToSettingsDescription: 'Set up credentials in device settings',
          ),
          IOSAuthMessages(
            cancelButton: 'Cancel',
            goToSettingsButton: 'Go to Settings',
            goToSettingsDescription: 'Set up biometrics or device passcode',
            lockOut: 'Reenable biometrics',
          ),
        ],
        options: const AuthenticationOptions(
          biometricOnly: false,
          stickyAuth: true,
        ),
      );

      setState(() {
        if (didAuthenticate) {
          _authStatus = 'Authentication successful!';
        } else {
          _authStatus = 'Authentication failed or cancelled';
        }
      });
    } catch (e) {
      setState(() {
        _authStatus = 'Authentication error: $e';
      });
    } finally {
      setState(() {
        _isAuthenticating = false;
      });
    }
  }

  Future<void> _authenticateWithBiometricOnly() async {
    if (_availableBiometrics.isEmpty) {
      setState(() {
        _authStatus = 'No biometric authentication methods available';
      });
      return;
    }

    setState(() {
      _isAuthenticating = true;
      _authStatus = 'Authenticating with biometric only...';
    });

    try {
      final bool didAuthenticate = await _localAuth.authenticate(
        localizedReason: 'Use biometric authentication to proceed',
        options: const AuthenticationOptions(
          biometricOnly: true,
          stickyAuth: true,
        ),
      );

      setState(() {
        if (didAuthenticate) {
          _authStatus = 'Biometric authentication successful!';
        } else {
          _authStatus = 'Biometric authentication failed';
        }
      });
    } catch (e) {
      setState(() {
        _authStatus = 'Biometric authentication error: $e';
      });
    } finally {
      setState(() {
        _isAuthenticating = false;
      });
    }
  }

  String _getBiometricTypeDescription(BiometricType type) {
    switch (type) {
      case BiometricType.face:
        return 'Face Recognition';
      case BiometricType.fingerprint:
        return 'Fingerprint';
      case BiometricType.iris:
        return 'Iris Scan';
      case BiometricType.weak:
        return 'Weak Biometric';
      case BiometricType.strong:
        return 'Strong Biometric';
      default:
        return 'Unknown';
    }
  }

  IconData _getBiometricTypeIcon(BiometricType type) {
    switch (type) {
      case BiometricType.face:
        return Icons.face;
      case BiometricType.fingerprint:
        return Icons.fingerprint;
      case BiometricType.iris:
        return Icons.visibility;
      default:
        return Icons.security;
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Biometric Authentication'),
        actions: [
          IconButton(
            onPressed: _checkBiometricAvailability,
            icon: const Icon(Icons.refresh),
            tooltip: 'Refresh biometric status',
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Card(
              color: _isBiometricAvailable 
                  ? Colors.green.withOpacity(0.1)
                  : Colors.red.withOpacity(0.1),
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        Icon(
                          _isBiometricAvailable 
                              ? Icons.verified_user 
                              : Icons.security,
                          color: _isBiometricAvailable 
                              ? Colors.green 
                              : Colors.orange,
                        ),
                        const SizedBox(width: 8),
                        Text(
                          'Biometric Availability',
                          style: Theme.of(context).textTheme.titleLarge,
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Text(
                      _isBiometricAvailable 
                          ? 'Biometric authentication is available'
                          : 'Biometric authentication is not available',
                      style: TextStyle(
                        color: _isBiometricAvailable 
                            ? Colors.green 
                            : Colors.orange,
                        fontWeight: FontWeight.w500,
                      ),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            if (_availableBiometrics.isNotEmpty) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Available Biometric Methods',
                        style: Theme.of(context).textTheme.titleMedium,
                      ),
                      const SizedBox(height: 12),
                      ...List.generate(_availableBiometrics.length, (index) {
                        final biometric = _availableBiometrics[index];
                        return Padding(
                          padding: const EdgeInsets.symmetric(vertical: 4),
                          child: Row(
                            children: [
                              Icon(
                                _getBiometricTypeIcon(biometric),
                                color: Colors.blue,
                              ),
                              const SizedBox(width: 12),
                              Text(_getBiometricTypeDescription(biometric)),
                            ],
                          ),
                        );
                      }),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 16),
            ],
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Authentication Status',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 12),
                    Container(
                      width: double.infinity,
                      padding: const EdgeInsets.all(12),
                      decoration: BoxDecoration(
                        color: Theme.of(context).colorScheme.surface,
                        border: Border.all(
                          color: Theme.of(context).colorScheme.outline,
                        ),
                        borderRadius: BorderRadius.circular(8),
                      ),
                      child: Text(
                        _authStatus,
                        style: const TextStyle(
                          fontFamily: 'monospace',
                          fontSize: 14,
                        ),
                      ),
                    ),
                  ],
                ),
              ),
            ),
            const Spacer(),
            if (_isBiometricAvailable) ...[
              SizedBox(
                width: double.infinity,
                child: ElevatedButton.icon(
                  onPressed: _isAuthenticating ? null : _authenticateWithBiometric,
                  icon: _isAuthenticating 
                      ? const SizedBox(
                          width: 16,
                          height: 16,
                          child: CircularProgressIndicator(strokeWidth: 2),
                        )
                      : const Icon(Icons.security),
                  label: Text(
                    _isAuthenticating 
                        ? 'Authenticating...' 
                        : 'Authenticate (Biometric + Passcode)',
                  ),
                ),
              ),
              const SizedBox(height: 12),
              if (_availableBiometrics.isNotEmpty)
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton.icon(
                    onPressed: _isAuthenticating ? null : _authenticateWithBiometricOnly,
                    icon: const Icon(Icons.fingerprint),
                    label: const Text('Authenticate (Biometric Only)'),
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.blue,
                    ),
                  ),
                ),
            ] else
              Card(
                color: Theme.of(context).colorScheme.errorContainer,
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    children: [
                      const Icon(Icons.warning),
                      const SizedBox(height: 8),
                      const Text(
                        'Biometric authentication is not available on this device.',
                        textAlign: TextAlign.center,
                      ),
                      const SizedBox(height: 8),
                      const Text(
                        'Make sure biometric authentication is set up in device settings.',
                        textAlign: TextAlign.center,
                        style: TextStyle(fontSize: 12),
                      ),
                    ],
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

This biometric authentication example demonstrates secure user  
authentication using fingerprint, face recognition, and other biometric  
methods. It includes proper fallback to device credentials and handles  
various authentication scenarios with comprehensive error handling.  

## Barcode and QR Code Scanner

Scanning barcodes and QR codes with camera integration and result processing.  

```dart
import 'package:flutter/material.dart';
import 'package:qr_code_scanner/qr_code_scanner.dart';
import 'package:permission_handler/permission_handler.dart';
import 'dart:io';

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
      title: 'Barcode Scanner',
      home: const BarcodeScannerScreen(),
    );
  }
}

class BarcodeScannerScreen extends StatefulWidget {
  const BarcodeScannerScreen({super.key});

  @override
  State<BarcodeScannerScreen> createState() => _BarcodeScannerScreenState();
}

class _BarcodeScannerScreenState extends State<BarcodeScannerScreen> {
  final GlobalKey qrKey = GlobalKey(debugLabel: 'QR');
  QRViewController? _qrController;
  Barcode? _lastScannedCode;
  bool _flashOn = false;
  bool _frontCamera = false;
  List<Barcode> _scanHistory = [];

  @override
  void initState() {
    super.initState();
    _requestCameraPermission();
  }

  @override
  void dispose() {
    _qrController?.dispose();
    super.dispose();
  }

  Future<void> _requestCameraPermission() async {
    final status = await Permission.camera.request();
    if (status != PermissionStatus.granted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Camera permission is required for scanning'),
        ),
      );
    }
  }

  void _onQRViewCreated(QRViewController controller) {
    setState(() {
      _qrController = controller;
    });

    controller.scannedDataStream.listen((scanData) {
      if (_lastScannedCode?.code != scanData.code) {
        setState(() {
          _lastScannedCode = scanData;
          _scanHistory.insert(0, scanData);
          if (_scanHistory.length > 20) {
            _scanHistory.removeLast();
          }
        });
        
        // Vibrate on successful scan (if available)
        _showScanResultDialog(scanData);
      }
    });
  }

  void _showScanResultDialog(Barcode scanData) {
    showDialog(
      context: context,
      builder: (BuildContext context) {
        return AlertDialog(
          title: Text('${_getBarcodeTypeText(scanData.format)} Scanned'),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Content:',
                style: Theme.of(context).textTheme.labelMedium,
              ),
              const SizedBox(height: 4),
              Container(
                width: double.infinity,
                padding: const EdgeInsets.all(8),
                decoration: BoxDecoration(
                  color: Theme.of(context).colorScheme.surface,
                  border: Border.all(
                    color: Theme.of(context).colorScheme.outline,
                  ),
                  borderRadius: BorderRadius.circular(4),
                ),
                child: Text(
                  scanData.code ?? 'No data',
                  style: const TextStyle(
                    fontFamily: 'monospace',
                    fontSize: 12,
                  ),
                ),
              ),
              const SizedBox(height: 12),
              Text(
                'Format: ${_getBarcodeTypeText(scanData.format)}',
                style: Theme.of(context).textTheme.bodySmall,
              ),
            ],
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.of(context).pop(),
              child: const Text('Continue Scanning'),
            ),
            TextButton(
              onPressed: () {
                Navigator.of(context).pop();
                _processScannedData(scanData);
              },
              child: const Text('Process'),
            ),
          ],
        );
      },
    );
  }

  void _processScannedData(Barcode scanData) {
    final String? code = scanData.code;
    if (code == null) return;

    // Check if it's a URL
    if (code.startsWith('http://') || code.startsWith('https://')) {
      _showActionDialog('URL Detected', 'Open in browser?', code, () {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Would open: $code')),
        );
      });
    }
    // Check if it's an email
    else if (code.contains('@') && code.contains('.')) {
      _showActionDialog('Email Detected', 'Compose email?', code, () {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Would compose email to: $code')),
        );
      });
    }
    // Check if it's a phone number
    else if (RegExp(r'^\+?[\d\s\-\(\)]+$').hasMatch(code)) {
      _showActionDialog('Phone Number Detected', 'Call this number?', code, () {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Would call: $code')),
        );
      });
    }
    // WiFi QR Code
    else if (code.startsWith('WIFI:')) {
      _showActionDialog('WiFi Config Detected', 'Connect to network?', 
          code.split(';').take(3).join('\n'), () {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Would configure WiFi')),
        );
      });
    }
    else {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Data processed')),
      );
    }
  }

  void _showActionDialog(String title, String message, String data, VoidCallback action) {
    showDialog(
      context: context,
      builder: (BuildContext context) {
        return AlertDialog(
          title: Text(title),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(message),
              const SizedBox(height: 8),
              Container(
                width: double.infinity,
                padding: const EdgeInsets.all(8),
                decoration: BoxDecoration(
                  color: Theme.of(context).colorScheme.surface,
                  borderRadius: BorderRadius.circular(4),
                ),
                child: Text(
                  data,
                  style: const TextStyle(fontSize: 12),
                ),
              ),
            ],
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.of(context).pop(),
              child: const Text('Cancel'),
            ),
            TextButton(
              onPressed: () {
                Navigator.of(context).pop();
                action();
              },
              child: const Text('Yes'),
            ),
          ],
        );
      },
    );
  }

  String _getBarcodeTypeText(BarcodeFormat format) {
    switch (format) {
      case BarcodeFormat.qrcode:
        return 'QR Code';
      case BarcodeFormat.ean13:
        return 'EAN-13';
      case BarcodeFormat.ean8:
        return 'EAN-8';
      case BarcodeFormat.code128:
        return 'Code 128';
      case BarcodeFormat.code39:
        return 'Code 39';
      case BarcodeFormat.code93:
        return 'Code 93';
      case BarcodeFormat.codabar:
        return 'Codabar';
      case BarcodeFormat.dataMatrix:
        return 'Data Matrix';
      case BarcodeFormat.pdf417:
        return 'PDF417';
      case BarcodeFormat.interleaved2of5:
        return 'Interleaved 2 of 5';
      case BarcodeFormat.upca:
        return 'UPC-A';
      case BarcodeFormat.upce:
        return 'UPC-E';
      case BarcodeFormat.aztec:
        return 'Aztec';
      default:
        return 'Unknown';
    }
  }

  Future<void> _toggleFlash() async {
    if (_qrController != null) {
      await _qrController!.toggleFlash();
      setState(() {
        _flashOn = !_flashOn;
      });
    }
  }

  Future<void> _flipCamera() async {
    if (_qrController != null) {
      await _qrController!.flipCamera();
      setState(() {
        _frontCamera = !_frontCamera;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Barcode Scanner'),
        actions: [
          IconButton(
            onPressed: _toggleFlash,
            icon: Icon(_flashOn ? Icons.flash_on : Icons.flash_off),
            tooltip: 'Toggle Flash',
          ),
          IconButton(
            onPressed: _flipCamera,
            icon: const Icon(Icons.flip_camera_ios),
            tooltip: 'Flip Camera',
          ),
        ],
      ),
      body: Column(
        children: [
          Expanded(
            flex: 4,
            child: Stack(
              children: [
                QRView(
                  key: qrKey,
                  onQRViewCreated: _onQRViewCreated,
                  overlay: QrScannerOverlayShape(
                    borderColor: Colors.blue,
                    borderRadius: 10,
                    borderLength: 30,
                    borderWidth: 10,
                    cutOutSize: MediaQuery.of(context).size.width * 0.7,
                  ),
                ),
                Positioned(
                  bottom: 16,
                  left: 16,
                  right: 16,
                  child: Container(
                    padding: const EdgeInsets.all(12),
                    decoration: BoxDecoration(
                      color: Colors.black54,
                      borderRadius: BorderRadius.circular(8),
                    ),
                    child: Text(
                      _lastScannedCode?.code ?? 'Aim camera at a barcode or QR code',
                      style: const TextStyle(
                        color: Colors.white,
                        fontSize: 12,
                      ),
                      textAlign: TextAlign.center,
                      maxLines: 2,
                      overflow: TextOverflow.ellipsis,
                    ),
                  ),
                ),
              ],
            ),
          ),
          Expanded(
            flex: 1,
            child: Container(
              width: double.infinity,
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Row(
                    children: [
                      Text(
                        'Scan History (${_scanHistory.length})',
                        style: Theme.of(context).textTheme.titleMedium,
                      ),
                      const Spacer(),
                      if (_scanHistory.isNotEmpty)
                        TextButton(
                          onPressed: () {
                            setState(() {
                              _scanHistory.clear();
                            });
                          },
                          child: const Text('Clear'),
                        ),
                    ],
                  ),
                  Expanded(
                    child: _scanHistory.isEmpty
                        ? const Center(
                            child: Text(
                              'No scans yet',
                              style: TextStyle(color: Colors.grey),
                            ),
                          )
                        : ListView.builder(
                            scrollDirection: Axis.horizontal,
                            itemCount: _scanHistory.length,
                            itemBuilder: (context, index) {
                              final scan = _scanHistory[index];
                              return Container(
                                width: 200,
                                margin: const EdgeInsets.only(right: 8),
                                child: Card(
                                  child: Padding(
                                    padding: const EdgeInsets.all(8),
                                    child: Column(
                                      crossAxisAlignment: CrossAxisAlignment.start,
                                      children: [
                                        Text(
                                          _getBarcodeTypeText(scan.format),
                                          style: const TextStyle(
                                            fontWeight: FontWeight.bold,
                                            fontSize: 12,
                                          ),
                                        ),
                                        const SizedBox(height: 4),
                                        Expanded(
                                          child: Text(
                                            scan.code ?? 'No data',
                                            style: const TextStyle(fontSize: 10),
                                            maxLines: 3,
                                            overflow: TextOverflow.ellipsis,
                                          ),
                                        ),
                                      ],
                                    ),
                                  ),
                                ),
                              );
                            },
                          ),
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }
}
```

This barcode scanner example provides comprehensive scanning capabilities  
for QR codes and various barcode formats. It includes smart content  
recognition for URLs, emails, phone numbers, and WiFi configurations  
with appropriate action suggestions and scan history tracking.  

## Media Player Integration

Playing audio and video files with platform-specific media controls.  

```dart
import 'package:flutter/material.dart';
import 'package:video_player/video_player.dart';
import 'package:audioplayers/audioplayers.dart';
import 'dart:io';

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
      title: 'Media Player',
      home: const MediaPlayerScreen(),
    );
  }
}

class MediaPlayerScreen extends StatefulWidget {
  const MediaPlayerScreen({super.key});

  @override
  State<MediaPlayerScreen> createState() => _MediaPlayerScreenState();
}

class _MediaPlayerScreenState extends State<MediaPlayerScreen>
    with TickerProviderStateMixin {
  VideoPlayerController? _videoController;
  AudioPlayer _audioPlayer = AudioPlayer();
  
  bool _isVideoLoading = false;
  bool _isAudioPlaying = false;
  Duration _audioDuration = Duration.zero;
  Duration _audioPosition = Duration.zero;
  
  late AnimationController _playButtonAnimationController;
  late Animation<double> _playButtonAnimation;

  // Sample media URLs
  final String _sampleVideoUrl = 
      'https://flutter.github.io/assets-for-api-docs/assets/videos/bee.mp4';
  final String _sampleAudioUrl = 
      'https://www.soundjay.com/misc/sounds/bell-ringing-05.wav';

  @override
  void initState() {
    super.initState();
    _setupAnimations();
    _setupAudioPlayer();
  }

  @override
  void dispose() {
    _videoController?.dispose();
    _audioPlayer.dispose();
    _playButtonAnimationController.dispose();
    super.dispose();
  }

  void _setupAnimations() {
    _playButtonAnimationController = AnimationController(
      duration: const Duration(milliseconds: 300),
      vsync: this,
    );
    _playButtonAnimation = Tween<double>(
      begin: 1.0,
      end: 1.2,
    ).animate(CurvedAnimation(
      parent: _playButtonAnimationController,
      curve: Curves.elasticOut,
    ));
  }

  void _setupAudioPlayer() {
    _audioPlayer.onPlayerStateChanged.listen((PlayerState state) {
      setState(() {
        _isAudioPlaying = state == PlayerState.playing;
      });
      
      if (state == PlayerState.playing) {
        _playButtonAnimationController.forward();
      } else {
        _playButtonAnimationController.reverse();
      }
    });

    _audioPlayer.onDurationChanged.listen((Duration duration) {
      setState(() {
        _audioDuration = duration;
      });
    });

    _audioPlayer.onPositionChanged.listen((Duration position) {
      setState(() {
        _audioPosition = position;
      });
    });
  }

  Future<void> _initializeVideoPlayer() async {
    setState(() {
      _isVideoLoading = true;
    });

    try {
      _videoController = VideoPlayerController.networkUrl(
        Uri.parse(_sampleVideoUrl),
      );
      
      await _videoController!.initialize();
      await _videoController!.setLooping(true);
      
      setState(() {
        _isVideoLoading = false;
      });
    } catch (e) {
      setState(() {
        _isVideoLoading = false;
      });
      
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Failed to load video: $e')),
      );
    }
  }

  void _toggleVideoPlayback() {
    if (_videoController == null) return;
    
    setState(() {
      if (_videoController!.value.isPlaying) {
        _videoController!.pause();
      } else {
        _videoController!.play();
      }
    });
  }

  Future<void> _playAudio() async {
    try {
      if (_isAudioPlaying) {
        await _audioPlayer.pause();
      } else {
        await _audioPlayer.play(UrlSource(_sampleAudioUrl));
      }
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Failed to play audio: $e')),
      );
    }
  }

  Future<void> _stopAudio() async {
    await _audioPlayer.stop();
  }

  void _seekAudio(Duration position) {
    _audioPlayer.seek(position);
  }

  String _formatDuration(Duration duration) {
    String twoDigits(int n) => n.toString().padLeft(2, '0');
    final minutes = twoDigits(duration.inMinutes.remainder(60));
    final seconds = twoDigits(duration.inSeconds.remainder(60));
    return '$minutes:$seconds';
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Media Player'),
        actions: [
          IconButton(
            onPressed: () {
              showDialog(
                context: context,
                builder: (context) => AlertDialog(
                  title: const Text('Media Player Info'),
                  content: const Text(
                    'This example demonstrates video and audio playback '
                    'using platform-native media players with custom controls.',
                  ),
                  actions: [
                    TextButton(
                      onPressed: () => Navigator.pop(context),
                      child: const Text('OK'),
                    ),
                  ],
                ),
              );
            },
            icon: const Icon(Icons.info),
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        const Icon(Icons.videocam),
                        const SizedBox(width: 8),
                        Text(
                          'Video Player',
                          style: Theme.of(context).textTheme.titleLarge,
                        ),
                      ],
                    ),
                    const SizedBox(height: 16),
                    if (_videoController == null) ...[
                      Container(
                        height: 200,
                        width: double.infinity,
                        decoration: BoxDecoration(
                          color: Colors.black12,
                          borderRadius: BorderRadius.circular(8),
                        ),
                        child: Column(
                          mainAxisAlignment: MainAxisAlignment.center,
                          children: [
                            const Icon(Icons.video_library, size: 48),
                            const SizedBox(height: 16),
                            ElevatedButton.icon(
                              onPressed: _isVideoLoading ? null : _initializeVideoPlayer,
                              icon: _isVideoLoading 
                                  ? const SizedBox(
                                      width: 16,
                                      height: 16,
                                      child: CircularProgressIndicator(strokeWidth: 2),
                                    )
                                  : const Icon(Icons.play_circle),
                              label: Text(_isVideoLoading ? 'Loading...' : 'Load Video'),
                            ),
                          ],
                        ),
                      ),
                    ] else ...[
                      AspectRatio(
                        aspectRatio: _videoController!.value.aspectRatio,
                        child: Stack(
                          children: [
                            VideoPlayer(_videoController!),
                            Positioned.fill(
                              child: Container(
                                color: Colors.black26,
                                child: Center(
                                  child: IconButton(
                                    onPressed: _toggleVideoPlayback,
                                    icon: Icon(
                                      _videoController!.value.isPlaying
                                          ? Icons.pause_circle_filled
                                          : Icons.play_circle_filled,
                                      size: 64,
                                      color: Colors.white,
                                    ),
                                  ),
                                ),
                              ),
                            ),
                          ],
                        ),
                      ),
                      const SizedBox(height: 8),
                      VideoProgressIndicator(
                        _videoController!,
                        allowScrubbing: true,
                        colors: const VideoProgressColors(
                          playedColor: Colors.blue,
                          bufferedColor: Colors.grey,
                          backgroundColor: Colors.black26,
                        ),
                      ),
                      const SizedBox(height: 8),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                        children: [
                          IconButton(
                            onPressed: () {
                              final position = _videoController!.value.position;
                              _videoController!.seekTo(
                                Duration(seconds: (position.inSeconds - 10).clamp(0, 
                                    _videoController!.value.duration.inSeconds)),
                              );
                            },
                            icon: const Icon(Icons.replay_10),
                          ),
                          IconButton(
                            onPressed: _toggleVideoPlayback,
                            icon: Icon(
                              _videoController!.value.isPlaying
                                  ? Icons.pause
                                  : Icons.play_arrow,
                            ),
                          ),
                          IconButton(
                            onPressed: () {
                              final position = _videoController!.value.position;
                              final duration = _videoController!.value.duration;
                              _videoController!.seekTo(
                                Duration(seconds: (position.inSeconds + 10).clamp(0, 
                                    duration.inSeconds)),
                              );
                            },
                            icon: const Icon(Icons.forward_10),
                          ),
                        ],
                      ),
                    ],
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        const Icon(Icons.audiotrack),
                        const SizedBox(width: 8),
                        Text(
                          'Audio Player',
                          style: Theme.of(context).textTheme.titleLarge,
                        ),
                      ],
                    ),
                    const SizedBox(height: 16),
                    Center(
                      child: AnimatedBuilder(
                        animation: _playButtonAnimation,
                        builder: (context, child) {
                          return Transform.scale(
                            scale: _playButtonAnimation.value,
                            child: Container(
                              width: 120,
                              height: 120,
                              decoration: BoxDecoration(
                                shape: BoxShape.circle,
                                gradient: LinearGradient(
                                  colors: _isAudioPlaying
                                      ? [Colors.orange, Colors.red]
                                      : [Colors.blue, Colors.purple],
                                ),
                                boxShadow: [
                                  BoxShadow(
                                    color: _isAudioPlaying
                                        ? Colors.orange.withOpacity(0.3)
                                        : Colors.blue.withOpacity(0.3),
                                    blurRadius: 20,
                                    spreadRadius: 2,
                                  ),
                                ],
                              ),
                              child: IconButton(
                                onPressed: _playAudio,
                                icon: Icon(
                                  _isAudioPlaying ? Icons.pause : Icons.play_arrow,
                                  size: 48,
                                  color: Colors.white,
                                ),
                              ),
                            ),
                          );
                        },
                      ),
                    ),
                    const SizedBox(height: 20),
                    Row(
                      children: [
                        Text(_formatDuration(_audioPosition)),
                        Expanded(
                          child: Slider(
                            value: _audioDuration.inSeconds > 0
                                ? _audioPosition.inSeconds.toDouble()
                                : 0.0,
                            max: _audioDuration.inSeconds.toDouble(),
                            onChanged: (value) {
                              _seekAudio(Duration(seconds: value.toInt()));
                            },
                          ),
                        ),
                        Text(_formatDuration(_audioDuration)),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Row(
                      mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                      children: [
                        IconButton(
                          onPressed: () {
                            _seekAudio(Duration(
                              seconds: (_audioPosition.inSeconds - 15).clamp(0, 
                                  _audioDuration.inSeconds),
                            ));
                          },
                          icon: const Icon(Icons.replay_15),
                        ),
                        IconButton(
                          onPressed: _playAudio,
                          icon: Icon(_isAudioPlaying ? Icons.pause : Icons.play_arrow),
                        ),
                        IconButton(
                          onPressed: _stopAudio,
                          icon: const Icon(Icons.stop),
                        ),
                        IconButton(
                          onPressed: () {
                            _seekAudio(Duration(
                              seconds: (_audioPosition.inSeconds + 15).clamp(0, 
                                  _audioDuration.inSeconds),
                            ));
                          },
                          icon: const Icon(Icons.forward_15),
                        ),
                      ],
                    ),
                  ],
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

This media player example demonstrates comprehensive audio and video  
playback capabilities with custom controls, progress tracking, and  
animated UI elements. It includes proper resource management and  
platform-native media handling for optimal performance.  

## Bluetooth Integration

Scanning, connecting, and communicating with Bluetooth devices.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bluetooth_serial/flutter_bluetooth_serial.dart';
import 'package:permission_handler/permission_handler.dart';
import 'dart:typed_data';
import 'dart:convert';

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
      title: 'Bluetooth Integration',
      home: const BluetoothScreen(),
    );
  }
}

class BluetoothScreen extends StatefulWidget {
  const BluetoothScreen({super.key});

  @override
  State<BluetoothScreen> createState() => _BluetoothScreenState();
}

class _BluetoothScreenState extends State<BluetoothScreen> {
  FlutterBluetoothSerial _bluetooth = FlutterBluetoothSerial.instance;
  BluetoothConnection? _connection;
  
  bool _bluetoothEnabled = false;
  bool _isDiscovering = false;
  bool _isConnected = false;
  List<BluetoothDiscoveryResult> _discoveredDevices = [];
  List<BluetoothDevice> _pairedDevices = [];
  BluetoothDevice? _connectedDevice;
  
  final TextEditingController _messageController = TextEditingController();
  List<String> _messages = [];

  @override
  void initState() {
    super.initState();
    _initBluetooth();
  }

  @override
  void dispose() {
    _connection?.dispose();
    _messageController.dispose();
    super.dispose();
  }

  Future<void> _initBluetooth() async {
    // Request permissions
    await [
      Permission.bluetooth,
      Permission.bluetoothConnect,
      Permission.bluetoothScan,
      Permission.location,
    ].request();

    // Check if Bluetooth is enabled
    bool? isEnabled = await _bluetooth.isEnabled;
    setState(() {
      _bluetoothEnabled = isEnabled ?? false;
    });

    // Get paired devices
    if (_bluetoothEnabled) {
      _loadPairedDevices();
    }

    // Listen to Bluetooth state changes
    _bluetooth.onStateChanged().listen((BluetoothState state) {
      setState(() {
        _bluetoothEnabled = state == BluetoothState.STATE_ON;
      });
      
      if (_bluetoothEnabled) {
        _loadPairedDevices();
      }
    });
  }

  Future<void> _loadPairedDevices() async {
    try {
      List<BluetoothDevice> devices = await _bluetooth.getBondedDevices();
      setState(() {
        _pairedDevices = devices;
      });
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error loading paired devices: $e')),
      );
    }
  }

  Future<void> _enableBluetooth() async {
    bool? result = await _bluetooth.requestEnable();
    if (result == true) {
      setState(() {
        _bluetoothEnabled = true;
      });
      _loadPairedDevices();
    }
  }

  Future<void> _startDiscovery() async {
    if (!_bluetoothEnabled) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Bluetooth is not enabled')),
      );
      return;
    }

    setState(() {
      _isDiscovering = true;
      _discoveredDevices.clear();
    });

    try {
      _bluetooth.startDiscovery().listen((BluetoothDiscoveryResult result) {
        setState(() {
          _discoveredDevices.add(result);
        });
      }).onDone(() {
        setState(() {
          _isDiscovering = false;
        });
      });
    } catch (e) {
      setState(() {
        _isDiscovering = false;
      });
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Discovery error: $e')),
      );
    }
  }

  Future<void> _stopDiscovery() async {
    await _bluetooth.cancelDiscovery();
    setState(() {
      _isDiscovering = false;
    });
  }

  Future<void> _connectToDevice(BluetoothDevice device) async {
    try {
      setState(() {
        _isConnected = false;
      });

      _connection = await BluetoothConnection.toAddress(device.address);
      
      setState(() {
        _isConnected = true;
        _connectedDevice = device;
      });

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Connected to ${device.name}')),
      );

      // Listen for incoming data
      _connection!.input!.listen((Uint8List data) {
        String message = String.fromCharCodes(data);
        setState(() {
          _messages.add('Received: $message');
        });
      }).onDone(() {
        setState(() {
          _isConnected = false;
          _connectedDevice = null;
        });
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text('Disconnected')),
        );
      });

    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Connection failed: $e')),
      );
    }
  }

  Future<void> _disconnect() async {
    if (_connection != null) {
      await _connection!.finish();
      setState(() {
        _isConnected = false;
        _connectedDevice = null;
      });
    }
  }

  Future<void> _sendMessage() async {
    if (_connection == null || _messageController.text.isEmpty) return;

    try {
      String message = _messageController.text;
      _connection!.output.add(Uint8List.fromList(utf8.encode(message)));
      await _connection!.output.allSent;

      setState(() {
        _messages.add('Sent: $message');
        _messageController.clear();
      });
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Send failed: $e')),
      );
    }
  }

  Widget _buildDeviceList(String title, List<BluetoothDevice> devices, 
      {bool showConnect = true}) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              '$title (${devices.length})',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 12),
            if (devices.isEmpty)
              const Text('No devices found')
            else
              ...devices.map((device) => Card(
                margin: const EdgeInsets.symmetric(vertical: 2),
                child: ListTile(
                  leading: Icon(
                    _getDeviceIcon(device),
                    color: _isDeviceConnected(device) ? Colors.green : null,
                  ),
                  title: Text(device.name ?? 'Unknown Device'),
                  subtitle: Text(device.address),
                  trailing: showConnect
                      ? _isDeviceConnected(device)
                          ? IconButton(
                              onPressed: _disconnect,
                              icon: const Icon(Icons.bluetooth_connected, 
                                  color: Colors.red),
                              tooltip: 'Disconnect',
                            )
                          : IconButton(
                              onPressed: _isConnected 
                                  ? null 
                                  : () => _connectToDevice(device),
                              icon: const Icon(Icons.bluetooth),
                              tooltip: 'Connect',
                            )
                      : null,
                ),
              )).toList(),
          ],
        ),
      ),
    );
  }

  IconData _getDeviceIcon(BluetoothDevice device) {
    if (device.type == BluetoothDeviceType.phone) return Icons.phone_android;
    if (device.type == BluetoothDeviceType.computer) return Icons.computer;
    if (device.type == BluetoothDeviceType.headphones) return Icons.headphones;
    if (device.type == BluetoothDeviceType.audioVideo) return Icons.speaker;
    return Icons.bluetooth;
  }

  bool _isDeviceConnected(BluetoothDevice device) {
    return _connectedDevice?.address == device.address;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Bluetooth Integration'),
        actions: [
          IconButton(
            onPressed: _bluetoothEnabled
                ? (_isDiscovering ? _stopDiscovery : _startDiscovery)
                : _enableBluetooth,
            icon: Icon(
              _bluetoothEnabled
                  ? (_isDiscovering ? Icons.stop : Icons.bluetooth_searching)
                  : Icons.bluetooth_disabled,
            ),
            tooltip: _bluetoothEnabled
                ? (_isDiscovering ? 'Stop Discovery' : 'Start Discovery')
                : 'Enable Bluetooth',
          ),
        ],
      ),
      body: !_bluetoothEnabled
          ? Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  const Icon(Icons.bluetooth_disabled, size: 64),
                  const SizedBox(height: 16),
                  const Text('Bluetooth is disabled'),
                  const SizedBox(height: 16),
                  ElevatedButton.icon(
                    onPressed: _enableBluetooth,
                    icon: const Icon(Icons.bluetooth),
                    label: const Text('Enable Bluetooth'),
                  ),
                ],
              ),
            )
          : SingleChildScrollView(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  if (_isConnected) ...[
                    Card(
                      color: Colors.green.withOpacity(0.1),
                      child: Padding(
                        padding: const EdgeInsets.all(16),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Row(
                              children: [
                                const Icon(Icons.bluetooth_connected, 
                                    color: Colors.green),
                                const SizedBox(width: 8),
                                Text(
                                  'Connected to ${_connectedDevice!.name}',
                                  style: const TextStyle(
                                    fontWeight: FontWeight.bold,
                                    color: Colors.green,
                                  ),
                                ),
                              ],
                            ),
                            const SizedBox(height: 12),
                            TextField(
                              controller: _messageController,
                              decoration: const InputDecoration(
                                labelText: 'Message',
                                hintText: 'Enter message to send',
                              ),
                              onSubmitted: (_) => _sendMessage(),
                            ),
                            const SizedBox(height: 8),
                            ElevatedButton.icon(
                              onPressed: _sendMessage,
                              icon: const Icon(Icons.send),
                              label: const Text('Send Message'),
                            ),
                            if (_messages.isNotEmpty) ...[
                              const SizedBox(height: 12),
                              const Text('Messages:'),
                              Container(
                                height: 100,
                                margin: const EdgeInsets.symmetric(vertical: 8),
                                padding: const EdgeInsets.all(8),
                                decoration: BoxDecoration(
                                  border: Border.all(color: Colors.grey),
                                  borderRadius: BorderRadius.circular(4),
                                ),
                                child: ListView.builder(
                                  reverse: true,
                                  itemCount: _messages.length,
                                  itemBuilder: (context, index) {
                                    final reversedIndex = _messages.length - 1 - index;
                                    return Text(
                                      _messages[reversedIndex],
                                      style: const TextStyle(fontSize: 12),
                                    );
                                  },
                                ),
                              ),
                            ],
                          ],
                        ),
                      ),
                    ),
                    const SizedBox(height: 16),
                  ],
                  _buildDeviceList('Paired Devices', _pairedDevices),
                  const SizedBox(height: 16),
                  if (_isDiscovering || _discoveredDevices.isNotEmpty)
                    _buildDeviceList(
                      _isDiscovering 
                          ? 'Discovering Devices...' 
                          : 'Discovered Devices',
                      _discoveredDevices.map((result) => result.device).toList(),
                    ),
                ],
              ),
            ),
    );
  }
}
```

This Bluetooth integration example provides comprehensive device  
management including discovery, pairing, connection, and data  
communication. It includes proper permission handling, connection  
status management, and bidirectional messaging capabilities.  

## WiFi Network Scanner

Scanning available WiFi networks and managing network connections.  

```dart
import 'package:flutter/material.dart';
import 'package:wifi_scan/wifi_scan.dart';
import 'package:permission_handler/permission_handler.dart';
import 'package:connectivity_plus/connectivity_plus.dart';

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
      title: 'WiFi Scanner',
      home: const WiFiScannerScreen(),
    );
  }
}

class WiFiScannerScreen extends StatefulWidget {
  const WiFiScannerScreen({super.key});

  @override
  State<WiFiScannerScreen> createState() => _WiFiScannerScreenState();
}

class _WiFiScannerScreenState extends State<WiFiScannerScreen> {
  List<WiFiAccessPoint> _accessPoints = [];
  bool _isScanning = false;
  bool _canGetScannedResults = false;
  bool _canStartScan = false;
  String? _currentSSID;
  ConnectivityResult _connectionStatus = ConnectivityResult.none;

  @override
  void initState() {
    super.initState();
    _initWiFi();
  }

  Future<void> _initWiFi() async {
    // Request permissions
    await _requestPermissions();
    
    // Check capabilities
    await _checkCapabilities();
    
    // Get current connection status
    await _getCurrentConnection();
    
    // Listen to connectivity changes
    Connectivity().onConnectivityChanged.listen((List<ConnectivityResult> results) {
      if (results.isNotEmpty) {
        setState(() {
          _connectionStatus = results.first;
        });
      }
    });
  }

  Future<void> _requestPermissions() async {
    await [
      Permission.location,
      Permission.nearbyWifiDevices,
    ].request();
  }

  Future<void> _checkCapabilities() async {
    final canGetScannedResults = await WiFiScan.instance.canGetScannedResults();
    final canStartScan = await WiFiScan.instance.canStartScan();
    
    setState(() {
      _canGetScannedResults = canGetScannedResults == CanGetScannedResults.yes;
      _canStartScan = canStartScan == CanStartScan.yes;
    });
  }

  Future<void> _getCurrentConnection() async {
    try {
      final connectivityResult = await Connectivity().checkConnectivity();
      setState(() {
        _connectionStatus = connectivityResult.first;
      });
      
      // Get current SSID (platform-specific implementation would be needed)
      // This is a simplified example
      if (_connectionStatus == ConnectivityResult.wifi) {
        setState(() {
          _currentSSID = "Current Network"; // Placeholder
        });
      }
    } catch (e) {
      print('Error getting current connection: $e');
    }
  }

  Future<void> _startScan() async {
    if (!_canStartScan) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Cannot start WiFi scan on this device')),
      );
      return;
    }

    setState(() {
      _isScanning = true;
    });

    try {
      await WiFiScan.instance.startScan();
      // Wait a bit for scan to complete
      await Future.delayed(const Duration(seconds: 3));
      await _getScannedResults();
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Scan failed: $e')),
      );
    } finally {
      setState(() {
        _isScanning = false;
      });
    }
  }

  Future<void> _getScannedResults() async {
    if (!_canGetScannedResults) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Cannot get scan results on this device')),
      );
      return;
    }

    try {
      final results = await WiFiScan.instance.getScannedResults();
      setState(() {
        _accessPoints = results;
      });
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Failed to get results: $e')),
      );
    }
  }

  String _getSecurityText(WiFiAccessPoint ap) {
    if (ap.capabilities.contains('WPA3')) return 'WPA3';
    if (ap.capabilities.contains('WPA2')) return 'WPA2';
    if (ap.capabilities.contains('WPA')) return 'WPA';
    if (ap.capabilities.contains('WEP')) return 'WEP';
    return 'Open';
  }

  IconData _getSecurityIcon(WiFiAccessPoint ap) {
    if (ap.capabilities.isEmpty) return Icons.wifi;
    return Icons.wifi_lock;
  }

  Color _getSignalColor(int level) {
    if (level > -50) return Colors.green;
    if (level > -70) return Colors.orange;
    return Colors.red;
  }

  int _getSignalStrengthBars(int level) {
    if (level > -50) return 4;
    if (level > -60) return 3;
    if (level > -70) return 2;
    return 1;
  }

  Widget _buildSignalIcon(int level) {
    final bars = _getSignalStrengthBars(level);
    final color = _getSignalColor(level);
    
    switch (bars) {
      case 4:
        return Icon(Icons.wifi_4_bar, color: color);
      case 3:
        return Icon(Icons.wifi_3_bar, color: color);
      case 2:
        return Icon(Icons.wifi_2_bar, color: color);
      default:
        return Icon(Icons.wifi_1_bar, color: color);
    }
  }

  void _showNetworkDetails(WiFiAccessPoint ap) {
    showDialog(
      context: context,
      builder: (BuildContext context) {
        return AlertDialog(
          title: Text(ap.ssid.isNotEmpty ? ap.ssid : 'Hidden Network'),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              _buildDetailRow('BSSID', ap.bssid),
              _buildDetailRow('Security', _getSecurityText(ap)),
              _buildDetailRow('Frequency', '${ap.frequency} MHz'),
              _buildDetailRow('Signal Level', '${ap.level} dBm'),
              _buildDetailRow('Channel Width', ap.centerFreq0.toString()),
              _buildDetailRow('Capabilities', ap.capabilities),
            ],
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.of(context).pop(),
              child: const Text('Close'),
            ),
            if (ap.ssid.isNotEmpty)
              TextButton(
                onPressed: () {
                  Navigator.of(context).pop();
                  _showConnectDialog(ap);
                },
                child: const Text('Connect'),
              ),
          ],
        );
      },
    );
  }

  Widget _buildDetailRow(String label, String value) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          SizedBox(
            width: 100,
            child: Text(
              label,
              style: const TextStyle(fontWeight: FontWeight.w500),
            ),
          ),
          Expanded(
            child: Text(
              value,
              style: const TextStyle(fontSize: 12),
            ),
          ),
        ],
      ),
    );
  }

  void _showConnectDialog(WiFiAccessPoint ap) {
    final TextEditingController passwordController = TextEditingController();
    final bool isSecure = ap.capabilities.isNotEmpty;

    showDialog(
      context: context,
      builder: (BuildContext context) {
        return AlertDialog(
          title: Text('Connect to ${ap.ssid}'),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            children: [
              if (isSecure) ...[
                TextField(
                  controller: passwordController,
                  obscureText: true,
                  decoration: const InputDecoration(
                    labelText: 'Password',
                    hintText: 'Enter network password',
                  ),
                ),
                const SizedBox(height: 8),
              ],
              Text(
                isSecure
                    ? 'Enter the password for this secured network.'
                    : 'This is an open network.',
                style: Theme.of(context).textTheme.bodySmall,
              ),
            ],
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.of(context).pop(),
              child: const Text('Cancel'),
            ),
            TextButton(
              onPressed: () {
                Navigator.of(context).pop();
                _connectToNetwork(ap, passwordController.text);
              },
              child: const Text('Connect'),
            ),
          ],
        );
      },
    );
  }

  void _connectToNetwork(WiFiAccessPoint ap, String password) {
    // In a real app, you would use platform-specific code to connect
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(
          'Attempting to connect to ${ap.ssid}...\n'
          '(This would require platform-specific implementation)',
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('WiFi Scanner'),
        actions: [
          IconButton(
            onPressed: _canStartScan && !_isScanning ? _startScan : null,
            icon: _isScanning
                ? const SizedBox(
                    width: 16,
                    height: 16,
                    child: CircularProgressIndicator(strokeWidth: 2),
                  )
                : const Icon(Icons.wifi_find),
            tooltip: 'Scan Networks',
          ),
        ],
      ),
      body: Column(
        children: [
          Card(
            margin: const EdgeInsets.all(16),
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Row(
                    children: [
                      Icon(
                        _connectionStatus == ConnectivityResult.wifi
                            ? Icons.wifi
                            : Icons.wifi_off,
                        color: _connectionStatus == ConnectivityResult.wifi
                            ? Colors.green
                            : Colors.grey,
                      ),
                      const SizedBox(width: 8),
                      Text(
                        'Connection Status',
                        style: Theme.of(context).textTheme.titleMedium,
                      ),
                    ],
                  ),
                  const SizedBox(height: 8),
                  Text(
                    _connectionStatus == ConnectivityResult.wifi
                        ? 'Connected to WiFi${_currentSSID != null ? " ($_currentSSID)" : ""}'
                        : 'Not connected to WiFi',
                    style: TextStyle(
                      color: _connectionStatus == ConnectivityResult.wifi
                          ? Colors.green
                          : Colors.orange,
                    ),
                  ),
                  if (!_canStartScan || !_canGetScannedResults) ...[
                    const SizedBox(height: 8),
                    Text(
                      'WiFi scanning not available on this device',
                      style: TextStyle(
                        color: Colors.orange,
                        fontSize: 12,
                      ),
                    ),
                  ],
                ],
              ),
            ),
          ),
          Expanded(
            child: _accessPoints.isEmpty
                ? Center(
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        Icon(
                          Icons.wifi_find,
                          size: 64,
                          color: Colors.grey[600],
                        ),
                        const SizedBox(height: 16),
                        Text(
                          _isScanning
                              ? 'Scanning for networks...'
                              : 'No networks found',
                          style: TextStyle(color: Colors.grey[600]),
                        ),
                        if (!_isScanning && _canStartScan) ...[
                          const SizedBox(height: 16),
                          ElevatedButton.icon(
                            onPressed: _startScan,
                            icon: const Icon(Icons.wifi_find),
                            label: const Text('Start Scan'),
                          ),
                        ],
                      ],
                    ),
                  )
                : ListView.builder(
                    padding: const EdgeInsets.symmetric(horizontal: 16),
                    itemCount: _accessPoints.length,
                    itemBuilder: (context, index) {
                      final ap = _accessPoints[index];
                      return Card(
                        margin: const EdgeInsets.symmetric(vertical: 2),
                        child: ListTile(
                          leading: Column(
                            mainAxisAlignment: MainAxisAlignment.center,
                            children: [
                              _buildSignalIcon(ap.level),
                              const SizedBox(height: 2),
                              Icon(
                                _getSecurityIcon(ap),
                                size: 16,
                                color: Colors.grey,
                              ),
                            ],
                          ),
                          title: Text(
                            ap.ssid.isNotEmpty ? ap.ssid : 'Hidden Network',
                            style: TextStyle(
                              fontWeight: ap.ssid == _currentSSID
                                  ? FontWeight.bold
                                  : FontWeight.normal,
                            ),
                          ),
                          subtitle: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Text(
                                '${_getSecurityText(ap)} • ${ap.frequency} MHz',
                                style: const TextStyle(fontSize: 12),
                              ),
                              Text(
                                '${ap.level} dBm',
                                style: TextStyle(
                                  fontSize: 11,
                                  color: _getSignalColor(ap.level),
                                ),
                              ),
                            ],
                          ),
                          trailing: ap.ssid == _currentSSID
                              ? const Icon(Icons.check_circle, color: Colors.green)
                              : const Icon(Icons.chevron_right),
                          onTap: () => _showNetworkDetails(ap),
                        ),
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }
}
```

This WiFi scanner example demonstrates network discovery, signal strength  
analysis, and connection management. It includes proper permission  
handling, detailed network information display, and security type  
identification with an intuitive user interface for network selection.  

## App Shortcuts and Widget Integration

Creating app shortcuts and home screen widgets for quick access.  

```dart
import 'package:flutter/material.dart';
import 'package:quick_actions/quick_actions.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.dark,
      title: 'App Shortcuts',
      home: const AppShortcutsScreen(),
    );
  }
}

class AppShortcutsScreen extends StatefulWidget {
  const AppShortcutsScreen({super.key});

  @override
  State<AppShortcutsScreen> createState() => _AppShortcutsScreenState();
}

class _AppShortcutsScreenState extends State<AppShortcutsScreen> {
  final QuickActions _quickActions = const QuickActions();
  String? _shortcutType;
  List<String> _recentActions = [];
  int _shortcutUsageCount = 0;

  @override
  void initState() {
    super.initState();
    _initializeShortcuts();
    _loadUsageData();
  }

  Future<void> _initializeShortcuts() async {
    // Setup shortcut actions
    await _quickActions.setShortcutItems([
      const ShortcutItem(
        type: 'action_search',
        localizedTitle: 'Search',
        icon: 'ic_search',
      ),
      const ShortcutItem(
        type: 'action_camera',
        localizedTitle: 'Camera',
        icon: 'ic_camera',
      ),
      const ShortcutItem(
        type: 'action_favorites',
        localizedTitle: 'Favorites',
        icon: 'ic_favorites',
      ),
      const ShortcutItem(
        type: 'action_settings',
        localizedTitle: 'Settings',
        icon: 'ic_settings',
      ),
    ]);

    // Listen for shortcut actions
    _quickActions.initialize((String shortcutType) {
      _handleShortcutAction(shortcutType);
    });
  }

  Future<void> _loadUsageData() async {
    final prefs = await SharedPreferences.getInstance();
    setState(() {
      _shortcutUsageCount = prefs.getInt('shortcut_usage_count') ?? 0;
      _recentActions = prefs.getStringList('recent_actions') ?? [];
    });
  }

  Future<void> _saveUsageData() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setInt('shortcut_usage_count', _shortcutUsageCount);
    await prefs.setStringList('recent_actions', _recentActions);
  }

  void _handleShortcutAction(String shortcutType) {
    setState(() {
      _shortcutType = shortcutType;
      _shortcutUsageCount++;
      
      // Add to recent actions
      final actionName = _getActionName(shortcutType);
      _recentActions.insert(0, 
        '${DateTime.now().toString().substring(11, 16)}: $actionName');
      
      // Keep only last 10 actions
      if (_recentActions.length > 10) {
        _recentActions.removeLast();
      }
    });

    _saveUsageData();

    // Show feedback
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text('Shortcut activated: ${_getActionName(shortcutType)}'),
        behavior: SnackBarBehavior.floating,
      ),
    );

    // Navigate to appropriate screen
    _navigateToShortcutAction(shortcutType);
  }

  String _getActionName(String shortcutType) {
    switch (shortcutType) {
      case 'action_search':
        return 'Search';
      case 'action_camera':
        return 'Camera';
      case 'action_favorites':
        return 'Favorites';
      case 'action_settings':
        return 'Settings';
      default:
        return 'Unknown';
    }
  }

  IconData _getActionIcon(String shortcutType) {
    switch (shortcutType) {
      case 'action_search':
        return Icons.search;
      case 'action_camera':
        return Icons.camera_alt;
      case 'action_favorites':
        return Icons.favorite;
      case 'action_settings':
        return Icons.settings;
      default:
        return Icons.help;
    }
  }

  void _navigateToShortcutAction(String shortcutType) {
    Widget targetScreen;
    
    switch (shortcutType) {
      case 'action_search':
        targetScreen = const SearchScreen();
        break;
      case 'action_camera':
        targetScreen = const CameraScreen();
        break;
      case 'action_favorites':
        targetScreen = const FavoritesScreen();
        break;
      case 'action_settings':
        targetScreen = const SettingsScreen();
        break;
      default:
        return;
    }

    Navigator.of(context).push(
      MaterialPageRoute(builder: (context) => targetScreen),
    );
  }

  Future<void> _testShortcut(String shortcutType) async {
    _handleShortcutAction(shortcutType);
  }

  Future<void> _clearUsageData() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.remove('shortcut_usage_count');
    await prefs.remove('recent_actions');
    setState(() {
      _shortcutUsageCount = 0;
      _recentActions.clear();
      _shortcutType = null;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('App Shortcuts'),
        actions: [
          IconButton(
            onPressed: _clearUsageData,
            icon: const Icon(Icons.clear_all),
            tooltip: 'Clear Usage Data',
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        const Icon(Icons.shortcut),
                        const SizedBox(width: 8),
                        Text(
                          'App Shortcuts',
                          style: Theme.of(context).textTheme.titleLarge,
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    const Text(
                      'Long press the app icon on your home screen to access these shortcuts:',
                      style: TextStyle(fontSize: 14),
                    ),
                    const SizedBox(height: 16),
                    GridView.count(
                      shrinkWrap: true,
                      crossAxisCount: 2,
                      mainAxisSpacing: 8,
                      crossAxisSpacing: 8,
                      childAspectRatio: 3,
                      physics: const NeverScrollableScrollPhysics(),
                      children: [
                        _buildShortcutTile('action_search', 'Search'),
                        _buildShortcutTile('action_camera', 'Camera'),
                        _buildShortcutTile('action_favorites', 'Favorites'),
                        _buildShortcutTile('action_settings', 'Settings'),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        const Icon(Icons.analytics),
                        const SizedBox(width: 8),
                        Text(
                          'Usage Statistics',
                          style: Theme.of(context).textTheme.titleMedium,
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        Expanded(
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Text(
                                'Total Shortcuts Used',
                                style: Theme.of(context).textTheme.labelMedium,
                              ),
                              Text(
                                _shortcutUsageCount.toString(),
                                style: const TextStyle(
                                  fontSize: 24,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                            ],
                          ),
                        ),
                        if (_shortcutType != null)
                          Expanded(
                            child: Column(
                              crossAxisAlignment: CrossAxisAlignment.start,
                              children: [
                                Text(
                                  'Last Action',
                                  style: Theme.of(context).textTheme.labelMedium,
                                ),
                                Row(
                                  children: [
                                    Icon(
                                      _getActionIcon(_shortcutType!),
                                      size: 16,
                                    ),
                                    const SizedBox(width: 4),
                                    Text(
                                      _getActionName(_shortcutType!),
                                      style: const TextStyle(fontSize: 16),
                                    ),
                                  ],
                                ),
                              ],
                            ),
                          ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 16),
            if (_recentActions.isNotEmpty) ...[
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Recent Actions',
                        style: Theme.of(context).textTheme.titleMedium,
                      ),
                      const SizedBox(height: 12),
                      ...List.generate(
                        _recentActions.length.clamp(0, 5),
                        (index) => Padding(
                          padding: const EdgeInsets.symmetric(vertical: 2),
                          child: Text(
                            _recentActions[index],
                            style: const TextStyle(
                              fontSize: 12,
                              fontFamily: 'monospace',
                            ),
                          ),
                        ),
                      ),
                      if (_recentActions.length > 5)
                        Text(
                          '... and ${_recentActions.length - 5} more',
                          style: Theme.of(context).textTheme.bodySmall,
                        ),
                    ],
                  ),
                ),
              ),
            ],
            const Spacer(),
            Card(
              color: Theme.of(context).colorScheme.primaryContainer,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    const Icon(Icons.info_outline),
                    const SizedBox(height: 8),
                    const Text(
                      'App shortcuts provide quick access to common actions '
                      'directly from the home screen. Long press the app icon '
                      'to see available shortcuts.',
                      textAlign: TextAlign.center,
                      style: TextStyle(fontSize: 12),
                    ),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildShortcutTile(String shortcutType, String title) {
    return ElevatedButton(
      onPressed: () => _testShortcut(shortcutType),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(_getActionIcon(shortcutType), size: 16),
          const SizedBox(width: 4),
          Text(
            title,
            style: const TextStyle(fontSize: 12),
          ),
        ],
      ),
    );
  }
}

// Example target screens for shortcuts
class SearchScreen extends StatelessWidget {
  const SearchScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Search')),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.search, size: 64),
            SizedBox(height: 16),
            Text('Search Screen'),
            Text('Accessed via shortcut!'),
          ],
        ),
      ),
    );
  }
}

class CameraScreen extends StatelessWidget {
  const CameraScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Camera')),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.camera_alt, size: 64),
            SizedBox(height: 16),
            Text('Camera Screen'),
            Text('Accessed via shortcut!'),
          ],
        ),
      ),
    );
  }
}

class FavoritesScreen extends StatelessWidget {
  const FavoritesScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Favorites')),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.favorite, size: 64),
            SizedBox(height: 16),
            Text('Favorites Screen'),
            Text('Accessed via shortcut!'),
          ],
        ),
      ),
    );
  }
}

class SettingsScreen extends StatelessWidget {
  const SettingsScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Settings')),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.settings, size: 64),
            SizedBox(height: 16),
            Text('Settings Screen'),
            Text('Accessed via shortcut!'),
          ],
        ),
      ),
    );
  }
}
```

This app shortcuts example demonstrates creating dynamic home screen  
shortcuts for quick access to app features. It includes usage tracking,  
shortcut action handling, and proper navigation to target screens with  
comprehensive analytics and user feedback.  
