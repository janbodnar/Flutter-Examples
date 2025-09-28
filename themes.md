# Flutter Themes - 25 Essential Examples

This comprehensive guide covers Flutter theming with 25 practical examples,  
from basic light and dark themes to advanced theming techniques. Each example  
demonstrates different aspects of Flutter's theming system, helping you create  
consistent, beautiful, and accessible apps.

## Basic Light Theme

Creating a simple light theme with custom colors and styling.  

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
      title: 'Basic Light Theme',
      theme: ThemeData(
        brightness: Brightness.light,
        primarySwatch: Colors.blue,
        appBarTheme: const AppBarTheme(
          backgroundColor: Colors.blue,
          foregroundColor: Colors.white,
          elevation: 2,
        ),
        cardTheme: const CardTheme(
          elevation: 4,
          margin: EdgeInsets.all(8),
        ),
        textTheme: const TextTheme(
          headlineLarge: TextStyle(
            fontSize: 32,
            fontWeight: FontWeight.bold,
            color: Colors.black87,
          ),
          bodyLarge: TextStyle(
            fontSize: 16,
            color: Colors.black87,
          ),
        ),
      ),
      home: const HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic Light Theme'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Welcome',
              style: Theme.of(context).textTheme.headlineLarge,
            ),
            const SizedBox(height: 16),
            Text(
              'This is a basic light theme example with custom colors.',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
            const SizedBox(height: 20),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Text(
                  'This card uses the theme card styling.',
                  style: Theme.of(context).textTheme.bodyLarge,
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

This example demonstrates basic light theme creation using ThemeData.  
The primarySwatch defines the color palette, while appBarTheme and  
textTheme provide component-specific styling. Theme.of(context) accesses  
these theme properties throughout the widget tree.  

## Basic Dark Theme

Creating a dark theme with appropriate colors and contrast.  

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
      title: 'Basic Dark Theme',
      theme: ThemeData(
        brightness: Brightness.dark,
        primarySwatch: Colors.deepPurple,
        scaffoldBackgroundColor: const Color(0xFF121212),
        appBarTheme: const AppBarTheme(
          backgroundColor: Color(0xFF1F1F1F),
          foregroundColor: Colors.white,
          elevation: 0,
        ),
        cardTheme: const CardTheme(
          color: Color(0xFF1E1E1E),
          elevation: 2,
          margin: EdgeInsets.all(8),
        ),
        textTheme: const TextTheme(
          headlineLarge: TextStyle(
            fontSize: 32,
            fontWeight: FontWeight.bold,
            color: Colors.white,
          ),
          bodyLarge: TextStyle(
            fontSize: 16,
            color: Colors.white70,
          ),
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: Colors.deepPurple,
            foregroundColor: Colors.white,
          ),
        ),
      ),
      home: const HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Basic Dark Theme'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Dark Mode',
              style: Theme.of(context).textTheme.headlineLarge,
            ),
            const SizedBox(height: 16),
            Text(
              'This dark theme provides better viewing in low-light conditions.',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
            const SizedBox(height: 20),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Dark Theme Card',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 8),
                    Text(
                      'Cards adapt to dark theme colors automatically.',
                      style: Theme.of(context).textTheme.bodyMedium,
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {},
              child: const Text('Dark Theme Button'),
            ),
          ],
        ),
      ),
    );
  }
}
```

Dark themes require careful color selection for proper contrast and  
readability. This example uses darker background colors and lighter text  
colors. The scaffoldBackgroundColor sets the overall background, while  
component themes define specific element styling.  

## Light and Dark Theme Toggle

Implementing dynamic theme switching between light and dark modes.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  bool _isDarkMode = false;

  void _toggleTheme() {
    setState(() {
      _isDarkMode = !_isDarkMode;
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Theme Toggle',
      theme: ThemeData(
        brightness: Brightness.light,
        primarySwatch: Colors.teal,
        appBarTheme: const AppBarTheme(
          backgroundColor: Colors.teal,
          foregroundColor: Colors.white,
        ),
      ),
      darkTheme: ThemeData(
        brightness: Brightness.dark,
        primarySwatch: Colors.teal,
        appBarTheme: const AppBarTheme(
          backgroundColor: Color(0xFF1F1F1F),
          foregroundColor: Colors.white,
        ),
        scaffoldBackgroundColor: const Color(0xFF121212),
      ),
      themeMode: _isDarkMode ? ThemeMode.dark : ThemeMode.light,
      home: ThemeTogglePage(
        isDarkMode: _isDarkMode,
        onThemeToggle: _toggleTheme,
      ),
    );
  }
}

class ThemeTogglePage extends StatelessWidget {
  final bool isDarkMode;
  final VoidCallback onThemeToggle;

  const ThemeTogglePage({
    super.key,
    required this.isDarkMode,
    required this.onThemeToggle,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Theme Toggle'),
        actions: [
          IconButton(
            icon: Icon(isDarkMode ? Icons.light_mode : Icons.dark_mode),
            onPressed: onThemeToggle,
            tooltip: isDarkMode ? 'Switch to Light Mode' : 'Switch to Dark Mode',
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Current Theme: ${isDarkMode ? 'Dark' : 'Light'}',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
            const SizedBox(height: 20),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        Icon(
                          isDarkMode ? Icons.nightlight_round : Icons.wb_sunny,
                          size: 32,
                          color: Theme.of(context).primaryColor,
                        ),
                        const SizedBox(width: 12),
                        Expanded(
                          child: Text(
                            'Theme Switching',
                            style: Theme.of(context).textTheme.titleLarge,
                          ),
                        ),
                      ],
                    ),
                    const SizedBox(height: 12),
                    Text(
                      'Toggle between light and dark themes using the icon button '
                      'in the app bar. The entire app adapts to the selected theme.',
                      style: Theme.of(context).textTheme.bodyMedium,
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 20),
            Row(
              children: [
                ElevatedButton(
                  onPressed: () {},
                  child: const Text('Primary Button'),
                ),
                const SizedBox(width: 12),
                OutlinedButton(
                  onPressed: () {},
                  child: const Text('Outlined Button'),
                ),
              ],
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: onThemeToggle,
        tooltip: 'Toggle Theme',
        child: Icon(isDarkMode ? Icons.light_mode : Icons.dark_mode),
      ),
    );
  }
}
```

Theme toggling uses both theme and darkTheme properties in MaterialApp.  
The themeMode property controls which theme is active. State management  
tracks the current theme mode, allowing users to switch themes dynamically  
while maintaining the selection throughout the app session.  

## Custom Color Scheme

Creating themes with custom color schemes and brand colors.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const customColorScheme = ColorScheme(
      brightness: Brightness.light,
      primary: Color(0xFF6750A4),
      onPrimary: Color(0xFFFFFFFF),
      secondary: Color(0xFF625B71),
      onSecondary: Color(0xFFFFFFFF),
      error: Color(0xFFBA1A1A),
      onError: Color(0xFFFFFFFF),
      background: Color(0xFFFFFBFE),
      onBackground: Color(0xFF1C1B1F),
      surface: Color(0xFFFFFBFE),
      onSurface: Color(0xFF1C1B1F),
    );

    return MaterialApp(
      title: 'Custom Color Scheme',
      theme: ThemeData(
        colorScheme: customColorScheme,
        appBarTheme: AppBarTheme(
          backgroundColor: customColorScheme.primary,
          foregroundColor: customColorScheme.onPrimary,
          elevation: 2,
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: customColorScheme.primary,
            foregroundColor: customColorScheme.onPrimary,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(12),
            ),
          ),
        ),
        cardTheme: CardTheme(
          color: customColorScheme.surface,
          elevation: 3,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(12),
          ),
        ),
        textTheme: TextTheme(
          headlineLarge: TextStyle(
            fontSize: 32,
            fontWeight: FontWeight.bold,
            color: customColorScheme.onBackground,
          ),
          bodyLarge: TextStyle(
            fontSize: 16,
            color: customColorScheme.onBackground,
          ),
        ),
      ),
      home: const ColorSchemePage(),
    );
  }
}

class ColorSchemePage extends StatelessWidget {
  const ColorSchemePage({super.key});

  @override
  Widget build(BuildContext context) {
    final colorScheme = Theme.of(context).colorScheme;

    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Color Scheme'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Brand Colors',
              style: Theme.of(context).textTheme.headlineLarge,
            ),
            const SizedBox(height: 20),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Color Palette',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 12),
                    _buildColorSwatch('Primary', colorScheme.primary),
                    _buildColorSwatch('Secondary', colorScheme.secondary),
                    _buildColorSwatch('Error', colorScheme.error),
                    _buildColorSwatch('Background', colorScheme.background),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 20),
            Row(
              children: [
                ElevatedButton(
                  onPressed: () {},
                  child: const Text('Primary Action'),
                ),
                const SizedBox(width: 12),
                FilledButton.tonal(
                  onPressed: () {},
                  child: const Text('Tonal Button'),
                ),
              ],
            ),
            const SizedBox(height: 12),
            OutlinedButton(
              onPressed: () {},
              child: const Text('Outlined Action'),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildColorSwatch(String name, Color color) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4.0),
      child: Row(
        children: [
          Container(
            width: 24,
            height: 24,
            decoration: BoxDecoration(
              color: color,
              borderRadius: BorderRadius.circular(4),
              border: Border.all(color: Colors.grey.shade300),
            ),
          ),
          const SizedBox(width: 12),
          Text(name),
          const Spacer(),
          Text(
            '#${color.value.toRadixString(16).substring(2).toUpperCase()}',
            style: const TextStyle(fontFamily: 'monospace'),
          ),
        ],
      ),
    );
  }
}
```

Custom color schemes provide consistent branding across your app.  
The ColorScheme class defines all necessary colors including primary,  
secondary, error, and surface colors. Each color has an "on" counterpart  
for text and icons displayed on that color background.  

## Material 3 Theme

Implementing Material 3 design system with dynamic colors and modern styling.  

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
      title: 'Material 3 Theme',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: const Color(0xFF6750A4),
          brightness: Brightness.light,
        ),
        appBarTheme: const AppBarTheme(
          centerTitle: false,
          elevation: 0,
          scrolledUnderElevation: 3,
        ),
        navigationBarTheme: const NavigationBarThemeData(
          labelBehavior: NavigationDestinationLabelBehavior.onlyShowSelected,
        ),
        cardTheme: const CardTheme(
          elevation: 1,
          margin: EdgeInsets.all(8),
        ),
        filledButtonTheme: FilledButtonThemeData(
          style: FilledButton.styleFrom(
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(20),
            ),
          ),
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            elevation: 1,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(20),
            ),
          ),
        ),
      ),
      home: const Material3HomePage(),
    );
  }
}

class Material3HomePage extends StatefulWidget {
  const Material3HomePage({super.key});

  @override
  State<Material3HomePage> createState() => _Material3HomePageState();
}

class _Material3HomePageState extends State<Material3HomePage> {
  int _selectedIndex = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Material 3 Design'),
        actions: [
          IconButton(
            icon: const Icon(Icons.more_vert),
            onPressed: () {},
          ),
        ],
      ),
      body: IndexedStack(
        index: _selectedIndex,
        children: [
          _buildHomeContent(),
          _buildExploreContent(),
          _buildProfileContent(),
        ],
      ),
      navigationBar: NavigationBar(
        selectedIndex: _selectedIndex,
        onDestinationSelected: (index) {
          setState(() {
            _selectedIndex = index;
          });
        },
        destinations: const [
          NavigationDestination(
            icon: Icon(Icons.home_outlined),
            selectedIcon: Icon(Icons.home),
            label: 'Home',
          ),
          NavigationDestination(
            icon: Icon(Icons.explore_outlined),
            selectedIcon: Icon(Icons.explore),
            label: 'Explore',
          ),
          NavigationDestination(
            icon: Icon(Icons.person_outline),
            selectedIcon: Icon(Icons.person),
            label: 'Profile',
          ),
        ],
      ),
    );
  }

  Widget _buildHomeContent() {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'Material 3 Features',
            style: Theme.of(context).textTheme.headlineMedium,
          ),
          const SizedBox(height: 20),
          Card(
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Dynamic Colors',
                    style: Theme.of(context).textTheme.titleLarge,
                  ),
                  const SizedBox(height: 8),
                  Text(
                    'Material 3 uses dynamic color generation from a seed color '
                    'to create harmonious color palettes.',
                    style: Theme.of(context).textTheme.bodyMedium,
                  ),
                ],
              ),
            ),
          ),
          const SizedBox(height: 16),
          Row(
            children: [
              FilledButton(
                onPressed: () {},
                child: const Text('Filled Button'),
              ),
              const SizedBox(width: 12),
              FilledButton.tonal(
                onPressed: () {},
                child: const Text('Tonal Button'),
              ),
            ],
          ),
          const SizedBox(height: 12),
          Row(
            children: [
              ElevatedButton(
                onPressed: () {},
                child: const Text('Elevated Button'),
              ),
              const SizedBox(width: 12),
              OutlinedButton(
                onPressed: () {},
                child: const Text('Outlined Button'),
              ),
            ],
          ),
        ],
      ),
    );
  }

  Widget _buildExploreContent() {
    return const Center(
      child: Text('Explore content goes here'),
    );
  }

  Widget _buildProfileContent() {
    return const Center(
      child: Text('Profile content goes here'),
    );
  }
}
```

Material 3 introduces modern design principles with dynamic colors,  
improved accessibility, and updated component designs. The useMaterial3  
flag enables new components like NavigationBar and FilledButton.  
ColorScheme.fromSeed generates a complete color palette from a single  
seed color, ensuring visual harmony throughout the app.  

## Custom Text Theme

Customizing typography with font families, sizes, and weights.  

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
      title: 'Custom Text Theme',
      theme: ThemeData(
        textTheme: const TextTheme(
          displayLarge: TextStyle(
            fontSize: 57,
            fontWeight: FontWeight.w400,
            letterSpacing: -0.25,
            color: Color(0xFF1D1B20),
          ),
          displayMedium: TextStyle(
            fontSize: 45,
            fontWeight: FontWeight.w400,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          displaySmall: TextStyle(
            fontSize: 36,
            fontWeight: FontWeight.w400,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          headlineLarge: TextStyle(
            fontSize: 32,
            fontWeight: FontWeight.w700,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          headlineMedium: TextStyle(
            fontSize: 28,
            fontWeight: FontWeight.w600,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          headlineSmall: TextStyle(
            fontSize: 24,
            fontWeight: FontWeight.w600,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          titleLarge: TextStyle(
            fontSize: 22,
            fontWeight: FontWeight.w500,
            letterSpacing: 0,
            color: Color(0xFF1D1B20),
          ),
          titleMedium: TextStyle(
            fontSize: 16,
            fontWeight: FontWeight.w600,
            letterSpacing: 0.15,
            color: Color(0xFF1D1B20),
          ),
          titleSmall: TextStyle(
            fontSize: 14,
            fontWeight: FontWeight.w600,
            letterSpacing: 0.1,
            color: Color(0xFF1D1B20),
          ),
          bodyLarge: TextStyle(
            fontSize: 16,
            fontWeight: FontWeight.w400,
            letterSpacing: 0.5,
            color: Color(0xFF1D1B20),
          ),
          bodyMedium: TextStyle(
            fontSize: 14,
            fontWeight: FontWeight.w400,
            letterSpacing: 0.25,
            color: Color(0xFF1D1B20),
          ),
          bodySmall: TextStyle(
            fontSize: 12,
            fontWeight: FontWeight.w400,
            letterSpacing: 0.4,
            color: Color(0xFF49454F),
          ),
        ),
      ),
      home: const TextThemePage(),
    );
  }
}

class TextThemePage extends StatelessWidget {
  const TextThemePage({super.key});

  @override
  Widget build(BuildContext context) {
    final textTheme = Theme.of(context).textTheme;

    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Text Theme'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text('Display Large', style: textTheme.displayLarge),
              const SizedBox(height: 16),
              Text('Display Medium', style: textTheme.displayMedium),
              const SizedBox(height: 16),
              Text('Display Small', style: textTheme.displaySmall),
              const SizedBox(height: 20),
              Text('Headline Large', style: textTheme.headlineLarge),
              const SizedBox(height: 12),
              Text('Headline Medium', style: textTheme.headlineMedium),
              const SizedBox(height: 12),
              Text('Headline Small', style: textTheme.headlineSmall),
              const SizedBox(height: 20),
              Text('Title Large', style: textTheme.titleLarge),
              const SizedBox(height: 8),
              Text('Title Medium', style: textTheme.titleMedium),
              const SizedBox(height: 8),
              Text('Title Small', style: textTheme.titleSmall),
              const SizedBox(height: 20),
              Text(
                'Body Large: This is a paragraph using body large text style. '
                'It demonstrates the font size, weight, and spacing for '
                'primary body text content.',
                style: textTheme.bodyLarge,
              ),
              const SizedBox(height: 12),
              Text(
                'Body Medium: This text uses the body medium style, which is '
                'typically used for secondary text content and supporting '
                'information in your application.',
                style: textTheme.bodyMedium,
              ),
              const SizedBox(height: 12),
              Text(
                'Body Small: Used for captions, footnotes, and other small text.',
                style: textTheme.bodySmall,
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

Custom text themes define consistent typography throughout your app.  
Each text style specifies fontSize, fontWeight, letterSpacing, and color.  
The TextTheme class provides semantic names like displayLarge, headlineSmall,  
and bodyMedium for different text purposes and hierarchy levels.  

## Typography Scale Customization

Advanced typography with custom font scales and responsive text sizing.  

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
      title: 'Typography Scale',
      theme: ThemeData(
        useMaterial3: true,
        textTheme: _buildCustomTypographyScale(),
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
      ),
      home: const TypographyScalePage(),
    );
  }

  TextTheme _buildCustomTypographyScale() {
    const double baseSize = 14.0;
    const double ratio = 1.25; // Major third scale

    return TextTheme(
      displayLarge: TextStyle(
        fontSize: baseSize * ratio * ratio * ratio * ratio,
        fontWeight: FontWeight.w300,
        letterSpacing: -0.25,
        height: 1.12,
      ),
      displayMedium: TextStyle(
        fontSize: baseSize * ratio * ratio * ratio,
        fontWeight: FontWeight.w300,
        letterSpacing: 0,
        height: 1.16,
      ),
      displaySmall: TextStyle(
        fontSize: baseSize * ratio * ratio,
        fontWeight: FontWeight.w400,
        letterSpacing: 0,
        height: 1.22,
      ),
      headlineLarge: TextStyle(
        fontSize: baseSize * ratio * ratio,
        fontWeight: FontWeight.w600,
        letterSpacing: 0,
        height: 1.25,
      ),
      headlineMedium: TextStyle(
        fontSize: baseSize * ratio,
        fontWeight: FontWeight.w600,
        letterSpacing: 0,
        height: 1.29,
      ),
      headlineSmall: TextStyle(
        fontSize: baseSize,
        fontWeight: FontWeight.w600,
        letterSpacing: 0,
        height: 1.33,
      ),
      titleLarge: TextStyle(
        fontSize: baseSize,
        fontWeight: FontWeight.w500,
        letterSpacing: 0,
        height: 1.27,
      ),
      bodyLarge: TextStyle(
        fontSize: baseSize,
        fontWeight: FontWeight.w400,
        letterSpacing: 0.5,
        height: 1.43,
      ),
      bodyMedium: TextStyle(
        fontSize: baseSize * 0.875,
        fontWeight: FontWeight.w400,
        letterSpacing: 0.25,
        height: 1.43,
      ),
    );
  }
}

class TypographyScalePage extends StatelessWidget {
  const TypographyScalePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Typography Scale'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              _buildTypographyCard(
                context,
                'Display Typography',
                [
                  ('Display Large', Theme.of(context).textTheme.displayLarge),
                  ('Display Medium', Theme.of(context).textTheme.displayMedium),
                  ('Display Small', Theme.of(context).textTheme.displaySmall),
                ],
              ),
              const SizedBox(height: 20),
              _buildTypographyCard(
                context,
                'Headline Typography',
                [
                  ('Headline Large', Theme.of(context).textTheme.headlineLarge),
                  ('Headline Medium', Theme.of(context).textTheme.headlineMedium),
                  ('Headline Small', Theme.of(context).textTheme.headlineSmall),
                ],
              ),
              const SizedBox(height: 20),
              _buildTypographyCard(
                context,
                'Body Typography',
                [
                  ('Title Large', Theme.of(context).textTheme.titleLarge),
                  ('Body Large', Theme.of(context).textTheme.bodyLarge),
                  ('Body Medium', Theme.of(context).textTheme.bodyMedium),
                ],
              ),
              const SizedBox(height: 20),
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Responsive Typography',
                        style: Theme.of(context).textTheme.titleLarge,
                      ),
                      const SizedBox(height: 12),
                      Text(
                        'This typography scale uses a mathematical ratio (1.25) '
                        'to create harmonious size relationships. The base size '
                        'is 14px, and each level multiplies by the ratio.',
                        style: Theme.of(context).textTheme.bodyMedium,
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

  Widget _buildTypographyCard(
    BuildContext context,
    String title,
    List<(String, TextStyle?)> styles,
  ) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              title,
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 12),
            ...styles.map((style) => Padding(
              padding: const EdgeInsets.symmetric(vertical: 4.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    style.$1,
                    style: style.$2,
                  ),
                  if (style.$2 != null)
                    Text(
                      '${style.$2!.fontSize?.toStringAsFixed(1)}px, '
                      '${style.$2!.fontWeight.toString().split('.').last}',
                      style: Theme.of(context).textTheme.bodySmall?.copyWith(
                        color: Theme.of(context).colorScheme.outline,
                      ),
                    ),
                ],
              ),
            )),
          ],
        ),
      ),
    );
  }
}
```

Typography scales create visual hierarchy through consistent size  
relationships. Using mathematical ratios ensures harmonious proportions.  
The height property controls line spacing, while letterSpacing adjusts  
character spacing for optimal readability at different sizes.  

## Font Family Theme

Using custom fonts and Google Fonts in your theme configuration.  

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
      title: 'Font Family Theme',
      theme: ThemeData(
        fontFamily: 'Roboto',
        textTheme: const TextTheme(
          displayLarge: TextStyle(
            fontFamily: 'Playfair Display',
            fontSize: 57,
            fontWeight: FontWeight.w400,
            letterSpacing: -0.25,
          ),
          displayMedium: TextStyle(
            fontFamily: 'Playfair Display',
            fontSize: 45,
            fontWeight: FontWeight.w400,
          ),
          headlineLarge: TextStyle(
            fontFamily: 'Playfair Display',
            fontSize: 32,
            fontWeight: FontWeight.w600,
          ),
          headlineMedium: TextStyle(
            fontFamily: 'Lato',
            fontSize: 28,
            fontWeight: FontWeight.w600,
          ),
          headlineSmall: TextStyle(
            fontFamily: 'Lato',
            fontSize: 24,
            fontWeight: FontWeight.w600,
          ),
          titleLarge: TextStyle(
            fontFamily: 'Lato',
            fontSize: 22,
            fontWeight: FontWeight.w500,
          ),
          bodyLarge: TextStyle(
            fontFamily: 'Roboto',
            fontSize: 16,
            fontWeight: FontWeight.w400,
            letterSpacing: 0.5,
          ),
          bodyMedium: TextStyle(
            fontFamily: 'Roboto',
            fontSize: 14,
            fontWeight: FontWeight.w400,
            letterSpacing: 0.25,
          ),
          labelLarge: TextStyle(
            fontFamily: 'Roboto Mono',
            fontSize: 14,
            fontWeight: FontWeight.w500,
            letterSpacing: 1.25,
          ),
        ),
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepOrange),
      ),
      home: const FontFamilyPage(),
    );
  }
}

class FontFamilyPage extends StatelessWidget {
  const FontFamilyPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Font Family Theme'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Elegant Headlines',
                style: Theme.of(context).textTheme.displayMedium,
              ),
              const SizedBox(height: 8),
              Text(
                'Playfair Display for luxury appeal',
                style: Theme.of(context).textTheme.bodySmall,
              ),
              const SizedBox(height: 24),
              Text(
                'Clean Subheadings',
                style: Theme.of(context).textTheme.headlineMedium,
              ),
              const SizedBox(height: 8),
              Text(
                'Lato for modern readability',
                style: Theme.of(context).textTheme.bodySmall,
              ),
              const SizedBox(height: 24),
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Font Pairing Strategy',
                        style: Theme.of(context).textTheme.titleLarge,
                      ),
                      const SizedBox(height: 12),
                      Text(
                        'This theme combines three font families strategically:\n\n'
                        '• Playfair Display: Elegant serif for large displays\n'
                        '• Lato: Clean sans-serif for headings and titles\n'
                        '• Roboto: Reliable system font for body text\n'
                        '• Roboto Mono: Monospace for code and labels',
                        style: Theme.of(context).textTheme.bodyLarge,
                      ),
                    ],
                  ),
                ),
              ),
              const SizedBox(height: 20),
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Code Example',
                        style: Theme.of(context).textTheme.titleMedium,
                      ),
                      const SizedBox(height: 12),
                      Container(
                        padding: const EdgeInsets.all(12),
                        decoration: BoxDecoration(
                          color: Colors.grey.shade100,
                          borderRadius: BorderRadius.circular(8),
                        ),
                        child: Text(
                          'const fontFamily = "Roboto Mono";\nprint("Monospace for code");',
                          style: Theme.of(context).textTheme.labelLarge?.copyWith(
                            color: Colors.black87,
                          ),
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

Font family themes combine different typefaces for visual hierarchy  
and branding. Serif fonts work well for headlines, sans-serif for body  
text, and monospace for code. Consider contrast, readability, and brand  
personality when selecting font combinations.  

## Text Theme Inheritance

Understanding how text themes cascade and can be overridden locally.  

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
      title: 'Text Theme Inheritance',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.green),
        textTheme: const TextTheme(
          headlineLarge: TextStyle(
            fontSize: 32,
            fontWeight: FontWeight.bold,
            color: Colors.green,
          ),
          headlineMedium: TextStyle(
            fontSize: 28,
            fontWeight: FontWeight.w600,
            color: Colors.green,
          ),
          bodyLarge: TextStyle(
            fontSize: 16,
            color: Colors.black87,
            height: 1.5,
          ),
          bodyMedium: TextStyle(
            fontSize: 14,
            color: Colors.black54,
            height: 1.4,
          ),
        ),
      ),
      home: const TextInheritancePage(),
    );
  }
}

class TextInheritancePage extends StatelessWidget {
  const TextInheritancePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Text Theme Inheritance'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Global Theme Style',
              style: Theme.of(context).textTheme.headlineLarge,
            ),
            const SizedBox(height: 8),
            Text(
              'This uses the global theme color (green)',
              style: Theme.of(context).textTheme.bodyMedium,
            ),
            const SizedBox(height: 24),
            Text(
              'Override with copyWith',
              style: Theme.of(context).textTheme.headlineMedium?.copyWith(
                color: Colors.blue,
                fontStyle: FontStyle.italic,
              ),
            ),
            const SizedBox(height: 8),
            Text(
              'This overrides color and adds italic style',
              style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                color: Colors.blue,
              ),
            ),
            const SizedBox(height: 24),
            _buildInheritanceExample(context),
            const SizedBox(height: 24),
            Theme(
              data: Theme.of(context).copyWith(
                textTheme: Theme.of(context).textTheme.copyWith(
                  headlineMedium: Theme.of(context).textTheme.headlineMedium?.copyWith(
                    color: Colors.purple,
                  ),
                  bodyLarge: Theme.of(context).textTheme.bodyLarge?.copyWith(
                    color: Colors.purple.shade300,
                  ),
                ),
              ),
              child: _buildThemedSection(),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildInheritanceExample(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Inheritance Hierarchy',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            _buildInheritanceItem(
              context,
              'MaterialApp Theme',
              'Base text theme defined here',
              Colors.green.shade100,
            ),
            _buildInheritanceItem(
              context,
              '├─ copyWith Override',
              'Modifies specific properties',
              Colors.blue.shade100,
            ),
            _buildInheritanceItem(
              context,
              '└─ Local Theme Widget',
              'Creates scoped theme changes',
              Colors.purple.shade100,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildInheritanceItem(
    BuildContext context,
    String title,
    String description,
    Color backgroundColor,
  ) {
    return Container(
      margin: const EdgeInsets.symmetric(vertical: 4),
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: backgroundColor,
        borderRadius: BorderRadius.circular(8),
        border: Border.all(color: backgroundColor.withOpacity(0.3)),
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            title,
            style: Theme.of(context).textTheme.titleSmall,
          ),
          Text(
            description,
            style: Theme.of(context).textTheme.bodySmall,
          ),
        ],
      ),
    );
  }

  Widget _buildThemedSection() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Local Theme Override',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
            const SizedBox(height: 8),
            Text(
              'This section uses a purple theme that only applies to widgets '
              'within this Theme widget. The global app theme remains unchanged.',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
          ],
        ),
      ),
    );
  }
}
```

Text theme inheritance follows the widget tree hierarchy. Global themes  
set base styles, copyWith() creates variations, and Theme widgets create  
local overrides. This system allows consistent styling with targeted  
customization where needed.  

## Responsive Text Themes

Creating text themes that adapt to screen size and device characteristics.  

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
      title: 'Responsive Text Themes',
      theme: _buildResponsiveTheme(context),
      home: const ResponsiveTextPage(),
    );
  }

  ThemeData _buildResponsiveTheme(BuildContext context) {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(seedColor: Colors.teal),
      textTheme: _buildResponsiveTextTheme(context),
    );
  }

  TextTheme _buildResponsiveTextTheme(BuildContext context) {
    final size = MediaQuery.of(context).size;
    final isTablet = size.shortestSide >= 600;
    final isDesktop = size.width >= 1024;
    
    double scaleFactor = 1.0;
    if (isDesktop) {
      scaleFactor = 1.2;
    } else if (isTablet) {
      scaleFactor = 1.1;
    }

    return TextTheme(
      displayLarge: TextStyle(
        fontSize: 57 * scaleFactor,
        fontWeight: FontWeight.w400,
        letterSpacing: -0.25,
        height: isDesktop ? 1.0 : 1.12,
      ),
      displayMedium: TextStyle(
        fontSize: 45 * scaleFactor,
        fontWeight: FontWeight.w400,
        height: isDesktop ? 1.1 : 1.16,
      ),
      headlineLarge: TextStyle(
        fontSize: 32 * scaleFactor,
        fontWeight: FontWeight.w600,
        height: isDesktop ? 1.15 : 1.25,
      ),
      headlineMedium: TextStyle(
        fontSize: 28 * scaleFactor,
        fontWeight: FontWeight.w600,
        height: isDesktop ? 1.2 : 1.29,
      ),
      titleLarge: TextStyle(
        fontSize: 22 * scaleFactor,
        fontWeight: FontWeight.w500,
        height: 1.27,
      ),
      bodyLarge: TextStyle(
        fontSize: 16 * scaleFactor,
        fontWeight: FontWeight.w400,
        letterSpacing: 0.5,
        height: isDesktop ? 1.6 : 1.43,
      ),
      bodyMedium: TextStyle(
        fontSize: 14 * scaleFactor,
        fontWeight: FontWeight.w400,
        letterSpacing: 0.25,
        height: isDesktop ? 1.5 : 1.43,
      ),
    );
  }
}

class ResponsiveTextPage extends StatelessWidget {
  const ResponsiveTextPage({super.key});

  @override
  Widget build(BuildContext context) {
    final size = MediaQuery.of(context).size;
    final deviceType = _getDeviceType(size);

    return Scaffold(
      appBar: AppBar(
        title: const Text('Responsive Text Themes'),
      ),
      body: Padding(
        padding: EdgeInsets.all(_getResponsivePadding(size)),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              _buildDeviceInfo(context, deviceType, size),
              const SizedBox(height: 24),
              Text(
                'Responsive Headlines',
                style: Theme.of(context).textTheme.displayMedium,
              ),
              const SizedBox(height: 16),
              Text(
                'This text scales based on device type',
                style: Theme.of(context).textTheme.headlineMedium,
              ),
              const SizedBox(height: 20),
              LayoutBuilder(
                builder: (context, constraints) {
                  final columns = constraints.maxWidth > 800 ? 2 : 1;
                  
                  if (columns == 2) {
                    return Row(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Expanded(child: _buildTextSamples(context)),
                        const SizedBox(width: 20),
                        Expanded(child: _buildScalingInfo(context, deviceType)),
                      ],
                    );
                  } else {
                    return Column(
                      children: [
                        _buildTextSamples(context),
                        const SizedBox(height: 20),
                        _buildScalingInfo(context, deviceType),
                      ],
                    );
                  }
                },
              ),
            ],
          ),
        ),
      ),
    );
  }

  String _getDeviceType(Size size) {
    if (size.width >= 1024) return 'Desktop';
    if (size.shortestSide >= 600) return 'Tablet';
    return 'Mobile';
  }

  double _getResponsivePadding(Size size) {
    if (size.width >= 1024) return 32.0;
    if (size.shortestSide >= 600) return 24.0;
    return 16.0;
  }

  Widget _buildDeviceInfo(BuildContext context, String deviceType, Size size) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Row(
          children: [
            Icon(
              deviceType == 'Desktop' ? Icons.computer :
              deviceType == 'Tablet' ? Icons.tablet :
              Icons.phone_android,
              size: 32,
              color: Theme.of(context).primaryColor,
            ),
            const SizedBox(width: 16),
            Expanded(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Device: $deviceType',
                    style: Theme.of(context).textTheme.titleMedium,
                  ),
                  Text(
                    'Screen: ${size.width.toInt()} × ${size.height.toInt()}',
                    style: Theme.of(context).textTheme.bodyMedium,
                  ),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildTextSamples(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Text Samples',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            Text(
              'Large Headline',
              style: Theme.of(context).textTheme.headlineLarge,
            ),
            const SizedBox(height: 8),
            Text(
              'Medium Headline',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
            const SizedBox(height: 8),
            Text(
              'Body text adapts to screen size and device type for optimal '
              'readability across different form factors.',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildScalingInfo(BuildContext context, String deviceType) {
    final scaleFactor = deviceType == 'Desktop' ? 1.2 :
                       deviceType == 'Tablet' ? 1.1 : 1.0;

    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Scaling Information',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            Text(
              'Scale Factor: ${scaleFactor.toStringAsFixed(1)}×',
              style: Theme.of(context).textTheme.bodyLarge,
            ),
            const SizedBox(height: 8),
            Text(
              'Responsive text themes adjust font sizes and line heights '
              'based on device characteristics to maintain readability '
              'and visual hierarchy across platforms.',
              style: Theme.of(context).textTheme.bodyMedium,
            ),
          ],
        ),
      ),
    );
  }
}
```

Responsive text themes adapt to different screen sizes and device types.  
Scale factors adjust font sizes appropriately for mobile, tablet, and  
desktop displays. Line height and letter spacing also adjust to maintain  
optimal readability across form factors.  

## AppBar Theme Customization

Customizing AppBar appearance with colors, elevation, and text styles.  

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
      title: 'AppBar Theme Customization',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
        appBarTheme: const AppBarTheme(
          backgroundColor: Color(0xFF1E88E5),
          foregroundColor: Colors.white,
          elevation: 4,
          shadowColor: Colors.black26,
          surfaceTintColor: Colors.transparent,
          centerTitle: true,
          titleTextStyle: TextStyle(
            fontSize: 20,
            fontWeight: FontWeight.w600,
            color: Colors.white,
            letterSpacing: 0.15,
          ),
          actionsIconTheme: IconThemeData(
            color: Colors.white,
            size: 24,
          ),
          iconTheme: IconThemeData(
            color: Colors.white,
            size: 24,
          ),
          systemOverlayStyle: SystemUiOverlayStyle.light,
        ),
      ),
      home: const AppBarThemePage(),
    );
  }
}

class AppBarThemePage extends StatefulWidget {
  const AppBarThemePage({super.key});

  @override
  State<AppBarThemePage> createState() => _AppBarThemePageState();
}

class _AppBarThemePageState extends State<AppBarThemePage>
    with TickerProviderStateMixin {
  late TabController _tabController;
  bool _hasNotifications = true;

  @override
  void initState() {
    super.initState();
    _tabController = TabController(length: 3, vsync: this);
  }

  @override
  void dispose() {
    _tabController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: NestedScrollView(
        headerSliverBuilder: (context, innerBoxIsScrolled) {
          return [
            SliverAppBar(
              title: const Text('AppBar Themes'),
              pinned: true,
              floating: true,
              expandedHeight: 200,
              leading: IconButton(
                icon: const Icon(Icons.menu),
                onPressed: () {
                  _showCustomBottomSheet(context);
                },
              ),
              actions: [
                IconButton(
                  icon: Stack(
                    children: [
                      const Icon(Icons.notifications),
                      if (_hasNotifications)
                        Positioned(
                          right: 0,
                          top: 0,
                          child: Container(
                            width: 8,
                            height: 8,
                            decoration: const BoxDecoration(
                              color: Colors.red,
                              shape: BoxShape.circle,
                            ),
                          ),
                        ),
                    ],
                  ),
                  onPressed: () {
                    setState(() {
                      _hasNotifications = !_hasNotifications;
                    });
                  },
                ),
                IconButton(
                  icon: const Icon(Icons.search),
                  onPressed: () {
                    _showSearchDialog(context);
                  },
                ),
                PopupMenuButton<String>(
                  icon: const Icon(Icons.more_vert),
                  onSelected: (value) {
                    ScaffoldMessenger.of(context).showSnackBar(
                      SnackBar(content: Text('Selected: $value')),
                    );
                  },
                  itemBuilder: (context) => [
                    const PopupMenuItem(
                      value: 'settings',
                      child: Text('Settings'),
                    ),
                    const PopupMenuItem(
                      value: 'help',
                      child: Text('Help'),
                    ),
                    const PopupMenuItem(
                      value: 'about',
                      child: Text('About'),
                    ),
                  ],
                ),
              ],
              flexibleSpace: const FlexibleSpaceBar(
                title: Text(
                  'Expandable AppBar',
                  style: TextStyle(
                    fontSize: 16,
                    fontWeight: FontWeight.w500,
                  ),
                ),
                background: DecoratedBox(
                  decoration: BoxDecoration(
                    gradient: LinearGradient(
                      begin: Alignment.topCenter,
                      end: Alignment.bottomCenter,
                      colors: [
                        Color(0xFF1976D2),
                        Color(0xFF1E88E5),
                      ],
                    ),
                  ),
                ),
              ),
              bottom: TabBar(
                controller: _tabController,
                indicatorColor: Colors.white,
                labelColor: Colors.white,
                unselectedLabelColor: Colors.white70,
                tabs: const [
                  Tab(text: 'Overview'),
                  Tab(text: 'Examples'),
                  Tab(text: 'Variants'),
                ],
              ),
            ),
          ];
        },
        body: TabBarView(
          controller: _tabController,
          children: [
            _buildOverviewTab(),
            _buildExamplesTab(),
            _buildVariantsTab(),
          ],
        ),
      ),
    );
  }

  Widget _buildOverviewTab() {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            'AppBar Theme Properties',
            style: Theme.of(context).textTheme.headlineSmall,
          ),
          const SizedBox(height: 16),
          _buildPropertyCard(
            'backgroundColor',
            'Sets the AppBar background color',
            'Color(0xFF1E88E5)',
          ),
          _buildPropertyCard(
            'foregroundColor',
            'Color for text and icons',
            'Colors.white',
          ),
          _buildPropertyCard(
            'elevation',
            'Shadow depth below the AppBar',
            '4.0',
          ),
          _buildPropertyCard(
            'centerTitle',
            'Whether to center the title',
            'true',
          ),
        ],
      ),
    );
  }

  Widget _buildExamplesTab() {
    return const Padding(
      padding: EdgeInsets.all(16.0),
      child: Column(
        children: [
          Card(
            child: ListTile(
              leading: Icon(Icons.palette),
              title: Text('Color Customization'),
              subtitle: Text('Custom background and foreground colors'),
            ),
          ),
          Card(
            child: ListTile(
              leading: Icon(Icons.text_fields),
              title: Text('Typography'),
              subtitle: Text('Custom title text style and font weight'),
            ),
          ),
          Card(
            child: ListTile(
              leading: Icon(Icons.shadow),
              title: Text('Elevation & Shadow'),
              subtitle: Text('Configurable shadow depth and color'),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildVariantsTab() {
    return const Padding(
      padding: EdgeInsets.all(16.0),
      child: Column(
        children: [
          Card(
            child: ListTile(
              leading: Icon(Icons.expand),
              title: Text('SliverAppBar'),
              subtitle: Text('Collapsible and expandable app bars'),
            ),
          ),
          Card(
            child: ListTile(
              leading: Icon(Icons.tab),
              title: Text('TabBar Integration'),
              subtitle: Text('AppBar with integrated tab navigation'),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildPropertyCard(String property, String description, String value) {
    return Card(
      margin: const EdgeInsets.only(bottom: 8),
      child: Padding(
        padding: const EdgeInsets.all(12),
        child: Row(
          children: [
            Expanded(
              flex: 2,
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    property,
                    style: const TextStyle(
                      fontWeight: FontWeight.w600,
                      fontFamily: 'monospace',
                    ),
                  ),
                  Text(
                    description,
                    style: Theme.of(context).textTheme.bodySmall,
                  ),
                ],
              ),
            ),
            Expanded(
              child: Text(
                value,
                style: const TextStyle(
                  fontFamily: 'monospace',
                  color: Colors.blue,
                ),
                textAlign: TextAlign.right,
              ),
            ),
          ],
        ),
      ),
    );
  }

  void _showCustomBottomSheet(BuildContext context) {
    showModalBottomSheet(
      context: context,
      builder: (context) => Container(
        padding: const EdgeInsets.all(16),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            ListTile(
              leading: const Icon(Icons.home),
              title: const Text('Home'),
              onTap: () => Navigator.pop(context),
            ),
            ListTile(
              leading: const Icon(Icons.settings),
              title: const Text('Settings'),
              onTap: () => Navigator.pop(context),
            ),
          ],
        ),
      ),
    );
  }

  void _showSearchDialog(BuildContext context) {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Search'),
        content: const TextField(
          decoration: InputDecoration(
            hintText: 'Enter search query...',
            prefixIcon: Icon(Icons.search),
          ),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () => Navigator.pop(context),
            child: const Text('Search'),
          ),
        ],
      ),
    );
  }
}
```

AppBar themes control the appearance of app bars throughout your  
application. Key properties include backgroundColor, foregroundColor,  
elevation, and titleTextStyle. SliverAppBar provides additional  
customization for collapsible and expandable app bars with flexible  
space and scroll behaviors.  

## Button Theme Styling

Comprehensive button theming for all Flutter button types.  

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
      title: 'Button Theme Styling',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: Colors.deepPurple,
            foregroundColor: Colors.white,
            elevation: 3,
            shadowColor: Colors.deepPurple.withOpacity(0.5),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(16),
            ),
            padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
            textStyle: const TextStyle(
              fontSize: 16,
              fontWeight: FontWeight.w600,
              letterSpacing: 0.5,
            ),
          ),
        ),
        filledButtonTheme: FilledButtonThemeData(
          style: FilledButton.styleFrom(
            backgroundColor: Colors.deepPurple.shade600,
            foregroundColor: Colors.white,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(12),
            ),
            padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 12),
          ),
        ),
        outlinedButtonTheme: OutlinedButtonThemeData(
          style: OutlinedButton.styleFrom(
            foregroundColor: Colors.deepPurple,
            side: BorderSide(color: Colors.deepPurple, width: 2),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(12),
            ),
            padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 12),
          ),
        ),
        textButtonTheme: TextButtonThemeData(
          style: TextButton.styleFrom(
            foregroundColor: Colors.deepPurple,
            textStyle: const TextStyle(
              fontSize: 16,
              fontWeight: FontWeight.w500,
            ),
            padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
          ),
        ),
        iconButtonTheme: IconButtonThemeData(
          style: IconButton.styleFrom(
            backgroundColor: Colors.deepPurple.withOpacity(0.1),
            foregroundColor: Colors.deepPurple,
            padding: const EdgeInsets.all(12),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(8),
            ),
          ),
        ),
        floatingActionButtonTheme: const FloatingActionButtonThemeData(
          backgroundColor: Colors.deepPurple,
          foregroundColor: Colors.white,
          elevation: 6,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.all(Radius.circular(16)),
          ),
        ),
      ),
      home: const ButtonThemePage(),
    );
  }
}

class ButtonThemePage extends StatefulWidget {
  const ButtonThemePage({super.key});

  @override
  State<ButtonThemePage> createState() => _ButtonThemePageState();
}

class _ButtonThemePageState extends State<ButtonThemePage> {
  bool _isLoading = false;
  bool _isEnabled = true;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Button Theme Styling'),
        actions: [
          Switch(
            value: _isEnabled,
            onChanged: (value) {
              setState(() {
                _isEnabled = value;
              });
            },
          ),
        ],
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: SingleChildScrollView(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Button Styles',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
              const SizedBox(height: 20),
              _buildButtonSection('Elevated Buttons', [
                ElevatedButton(
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Primary Action'),
                ),
                const SizedBox(height: 8),
                ElevatedButton.icon(
                  onPressed: _isEnabled ? () {} : null,
                  icon: const Icon(Icons.favorite),
                  label: const Text('With Icon'),
                ),
                const SizedBox(height: 8),
                ElevatedButton(
                  onPressed: _isEnabled ? _handleAsyncAction : null,
                  child: _isLoading
                      ? const SizedBox(
                          width: 20,
                          height: 20,
                          child: CircularProgressIndicator(
                            strokeWidth: 2,
                            valueColor: AlwaysStoppedAnimation<Color>(Colors.white),
                          ),
                        )
                      : const Text('Async Action'),
                ),
              ]),
              const SizedBox(height: 24),
              _buildButtonSection('Filled Buttons', [
                FilledButton(
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Filled Button'),
                ),
                const SizedBox(height: 8),
                FilledButton.icon(
                  onPressed: _isEnabled ? () {} : null,
                  icon: const Icon(Icons.download),
                  label: const Text('Download'),
                ),
                const SizedBox(height: 8),
                FilledButton.tonal(
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Tonal Variant'),
                ),
              ]),
              const SizedBox(height: 24),
              _buildButtonSection('Outlined Buttons', [
                OutlinedButton(
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Outlined'),
                ),
                const SizedBox(height: 8),
                OutlinedButton.icon(
                  onPressed: _isEnabled ? () {} : null,
                  icon: const Icon(Icons.edit),
                  label: const Text('Edit'),
                ),
              ]),
              const SizedBox(height: 24),
              _buildButtonSection('Text Buttons', [
                TextButton(
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Text Button'),
                ),
                const SizedBox(height: 8),
                TextButton.icon(
                  onPressed: _isEnabled ? () {} : null,
                  icon: const Icon(Icons.share),
                  label: const Text('Share'),
                ),
              ]),
              const SizedBox(height: 24),
              _buildButtonSection('Icon Buttons', [
                Row(
                  children: [
                    IconButton(
                      onPressed: _isEnabled ? () {} : null,
                      icon: const Icon(Icons.thumb_up),
                    ),
                    IconButton(
                      onPressed: _isEnabled ? () {} : null,
                      icon: const Icon(Icons.bookmark),
                    ),
                    IconButton(
                      onPressed: _isEnabled ? () {} : null,
                      icon: const Icon(Icons.more_horiz),
                    ),
                    const Spacer(),
                    IconButton.filled(
                      onPressed: _isEnabled ? () {} : null,
                      icon: const Icon(Icons.add),
                    ),
                  ],
                ),
              ]),
              const SizedBox(height: 24),
              _buildButtonCustomization(),
            ],
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _isEnabled ? () {} : null,
        tooltip: 'Themed FAB',
        child: const Icon(Icons.add),
      ),
    );
  }

  Widget _buildButtonSection(String title, List<Widget> buttons) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              title,
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 12),
            ...buttons,
          ],
        ),
      ),
    );
  }

  Widget _buildButtonCustomization() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Button Customization',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 12),
            const Text(
              'Button themes define consistent styling across your app:\n\n'
              '• Shape and border radius\n'
              '• Colors and elevation\n'
              '• Padding and text styles\n'
              '• State-dependent behaviors',
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                ElevatedButton(
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.green,
                    foregroundColor: Colors.white,
                    shape: const CircleBorder(),
                    padding: const EdgeInsets.all(16),
                  ),
                  onPressed: _isEnabled ? () {} : null,
                  child: const Icon(Icons.check),
                ),
                const SizedBox(width: 12),
                ElevatedButton(
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.orange,
                    foregroundColor: Colors.white,
                    shape: const StadiumBorder(),
                  ),
                  onPressed: _isEnabled ? () {} : null,
                  child: const Text('Custom Shape'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Future<void> _handleAsyncAction() async {
    setState(() {
      _isLoading = true;
    });

    await Future.delayed(const Duration(seconds: 2));

    if (mounted) {
      setState(() {
        _isLoading = false;
      });
    }
  }
}
```

Button themes provide consistent styling across all button types in your  
app. Each button type has its own theme data: ElevatedButtonTheme,  
FilledButtonTheme, OutlinedButtonTheme, TextButtonTheme, and  
IconButtonTheme. Themes can be overridden locally using the style  
property for specific customization needs.  

## Card Theme Configuration

Customizing card appearance with elevation, shapes, and colors.  

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
      title: 'Card Theme Configuration',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.teal),
        cardTheme: const CardTheme(
          elevation: 2,
          shadowColor: Color(0x1A000000),
          surfaceTintColor: Color(0xFFE0F2F1),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            side: BorderSide(color: Color(0xFFE0E0E0), width: 0.5),
          ),
          margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
          clipBehavior: Clip.antiAlias,
        ),
      ),
      home: const CardThemePage(),
    );
  }
}

class CardThemePage extends StatefulWidget {
  const CardThemePage({super.key});

  @override
  State<CardThemePage> createState() => _CardThemePageState();
}

class _CardThemePageState extends State<CardThemePage> {
  double _currentElevation = 2;
  BorderRadius _currentBorderRadius = BorderRadius.circular(12);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Card Theme Configuration'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Card Variations',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 20),
            _buildBasicCard(),
            _buildContentCard(),
            _buildImageCard(),
            _buildInteractiveCard(),
            const SizedBox(height: 24),
            _buildCustomizationControls(),
            const SizedBox(height: 20),
            _buildCustomCard(),
          ],
        ),
      ),
    );
  }

  Widget _buildBasicCard() {
    return const Card(
      child: Padding(
        padding: EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Basic Card',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 8),
            Text(
              'This is a basic card using the global card theme. It demonstrates '
              'the default styling including elevation, border radius, and colors.',
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildContentCard() {
    return Card(
      child: Column(
        children: [
          const Padding(
            padding: EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Content Card',
                  style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                ),
                SizedBox(height: 8),
                Text(
                  'Cards can contain various types of content including text, '
                  'images, buttons, and other widgets.',
                ),
              ],
            ),
          ),
          const Divider(height: 1),
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.end,
              children: [
                TextButton(
                  onPressed: () {},
                  child: const Text('ACTION 1'),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: () {},
                  child: const Text('ACTION 2'),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildImageCard() {
    return Card(
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Container(
            height: 140,
            decoration: const BoxDecoration(
              gradient: LinearGradient(
                colors: [Colors.teal, Colors.tealAccent],
                begin: Alignment.topLeft,
                end: Alignment.bottomRight,
              ),
            ),
            child: const Center(
              child: Icon(
                Icons.landscape,
                size: 48,
                color: Colors.white,
              ),
            ),
          ),
          const Padding(
            padding: EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Image Card',
                  style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                ),
                SizedBox(height: 8),
                Text(
                  'Cards with images benefit from the clipBehavior property '
                  'to ensure content respects the card\'s border radius.',
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildInteractiveCard() {
    return Card(
      child: InkWell(
        onTap: () {
          ScaffoldMessenger.of(context).showSnackBar(
            const SnackBar(content: Text('Card tapped!')),
          );
        },
        borderRadius: BorderRadius.circular(12),
        child: const Padding(
          padding: EdgeInsets.all(16.0),
          child: Row(
            children: [
              Icon(Icons.touch_app, size: 32, color: Colors.teal),
              SizedBox(width: 16),
              Expanded(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Interactive Card',
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                    ),
                    SizedBox(height: 4),
                    Text('Tap this card to see the ripple effect.'),
                  ],
                ),
              ),
              Icon(Icons.arrow_forward_ios, size: 16),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildCustomizationControls() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Customization Controls',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Text(
              'Elevation: ${_currentElevation.toStringAsFixed(1)}',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            Slider(
              value: _currentElevation,
              min: 0,
              max: 10,
              divisions: 20,
              onChanged: (value) {
                setState(() {
                  _currentElevation = value;
                });
              },
            ),
            const SizedBox(height: 16),
            Text(
              'Border Radius',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                _buildRadiusOption('Square', BorderRadius.zero),
                const SizedBox(width: 8),
                _buildRadiusOption('Rounded', BorderRadius.circular(8)),
                const SizedBox(width: 8),
                _buildRadiusOption('Circular', BorderRadius.circular(20)),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildRadiusOption(String label, BorderRadius radius) {
    final isSelected = _currentBorderRadius == radius;
    return Expanded(
      child: OutlinedButton(
        style: OutlinedButton.styleFrom(
          backgroundColor: isSelected ? Colors.teal.withOpacity(0.1) : null,
          foregroundColor: isSelected ? Colors.teal : null,
          side: BorderSide(
            color: isSelected ? Colors.teal : Colors.grey,
            width: isSelected ? 2 : 1,
          ),
        ),
        onPressed: () {
          setState(() {
            _currentBorderRadius = radius;
          });
        },
        child: Text(label),
      ),
    );
  }

  Widget _buildCustomCard() {
    return Card(
      elevation: _currentElevation,
      shape: RoundedRectangleBorder(
        borderRadius: _currentBorderRadius,
        side: const BorderSide(color: Colors.teal, width: 1),
      ),
      child: const Padding(
        padding: EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Custom Card',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 8),
            Text(
              'This card uses the customization settings above. Try adjusting '
              'the elevation and border radius to see the changes.',
            ),
          ],
        ),
      ),
    );
  }
}
```

Card themes define consistent styling for all Card widgets in your app.  
Key properties include elevation for shadow depth, shape for border radius  
and borders, margin for spacing, and clipBehavior for content clipping.  
The surfaceTintColor property provides Material 3 surface tinting effects.  

## Input Decoration Theme

Styling text fields and form inputs with consistent decoration themes.  

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
      title: 'Input Decoration Theme',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        inputDecorationTheme: const InputDecorationTheme(
          filled: true,
          fillColor: Color(0xFFF5F5F5),
          border: OutlineInputBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            borderSide: BorderSide(color: Color(0xFFE0E0E0)),
          ),
          enabledBorder: OutlineInputBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            borderSide: BorderSide(color: Color(0xFFE0E0E0)),
          ),
          focusedBorder: OutlineInputBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            borderSide: BorderSide(color: Colors.blue, width: 2),
          ),
          errorBorder: OutlineInputBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            borderSide: BorderSide(color: Colors.red, width: 2),
          ),
          focusedErrorBorder: OutlineInputBorder(
            borderRadius: BorderRadius.all(Radius.circular(12)),
            borderSide: BorderSide(color: Colors.red, width: 2),
          ),
          labelStyle: TextStyle(
            fontSize: 16,
            fontWeight: FontWeight.w500,
            color: Color(0xFF666666),
          ),
          hintStyle: TextStyle(
            fontSize: 16,
            color: Color(0xFF999999),
          ),
          helperStyle: TextStyle(
            fontSize: 14,
            color: Color(0xFF666666),
          ),
          errorStyle: TextStyle(
            fontSize: 14,
            color: Colors.red,
            fontWeight: FontWeight.w500,
          ),
          contentPadding: EdgeInsets.symmetric(horizontal: 16, vertical: 16),
          prefixIconColor: Color(0xFF666666),
          suffixIconColor: Color(0xFF666666),
        ),
      ),
      home: const InputDecorationPage(),
    );
  }
}

class InputDecorationPage extends StatefulWidget {
  const InputDecorationPage({super.key});

  @override
  State<InputDecorationPage> createState() => _InputDecorationPageState();
}

class _InputDecorationPageState extends State<InputDecorationPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _phoneController = TextEditingController();
  final _passwordController = TextEditingController();
  final _messageController = TextEditingController();

  bool _obscurePassword = true;
  String? _selectedCountry;
  bool _agreeToTerms = false;

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _phoneController.dispose();
    _passwordController.dispose();
    _messageController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Input Decoration Theme'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Form Example',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
              const SizedBox(height: 20),
              _buildBasicTextField(),
              const SizedBox(height: 16),
              _buildEmailTextField(),
              const SizedBox(height: 16),
              _buildPhoneTextField(),
              const SizedBox(height: 16),
              _buildPasswordTextField(),
              const SizedBox(height: 16),
              _buildDropdownField(),
              const SizedBox(height: 16),
              _buildMultilineTextField(),
              const SizedBox(height: 16),
              _buildCheckboxField(),
              const SizedBox(height: 24),
              _buildInputVariations(),
              const SizedBox(height: 24),
              _buildActionButtons(),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildBasicTextField() {
    return TextFormField(
      controller: _nameController,
      decoration: const InputDecoration(
        labelText: 'Full Name',
        hintText: 'Enter your full name',
        prefixIcon: Icon(Icons.person),
        helperText: 'This field uses the global input decoration theme',
      ),
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Please enter your name';
        }
        return null;
      },
    );
  }

  Widget _buildEmailTextField() {
    return TextFormField(
      controller: _emailController,
      keyboardType: TextInputType.emailAddress,
      decoration: const InputDecoration(
        labelText: 'Email Address',
        hintText: 'Enter your email',
        prefixIcon: Icon(Icons.email),
        suffixIcon: Icon(Icons.check_circle, color: Colors.green),
      ),
      validator: (value) {
        if (value == null || value.isEmpty) {
          return 'Please enter your email';
        }
        if (!value.contains('@')) {
          return 'Please enter a valid email';
        }
        return null;
      },
    );
  }

  Widget _buildPhoneTextField() {
    return TextFormField(
      controller: _phoneController,
      keyboardType: TextInputType.phone,
      decoration: const InputDecoration(
        labelText: 'Phone Number',
        hintText: '+1 (555) 123-4567',
        prefixIcon: Icon(Icons.phone),
        prefixText: '+1 ',
      ),
    );
  }

  Widget _buildPasswordTextField() {
    return TextFormField(
      controller: _passwordController,
      obscureText: _obscurePassword,
      decoration: InputDecoration(
        labelText: 'Password',
        hintText: 'Enter your password',
        prefixIcon: const Icon(Icons.lock),
        suffixIcon: IconButton(
          icon: Icon(_obscurePassword ? Icons.visibility : Icons.visibility_off),
          onPressed: () {
            setState(() {
              _obscurePassword = !_obscurePassword;
            });
          },
        ),
        helperText: 'Password must be at least 8 characters',
      ),
      validator: (value) {
        if (value == null || value.length < 8) {
          return 'Password must be at least 8 characters';
        }
        return null;
      },
    );
  }

  Widget _buildDropdownField() {
    return DropdownButtonFormField<String>(
      value: _selectedCountry,
      decoration: const InputDecoration(
        labelText: 'Country',
        prefixIcon: Icon(Icons.public),
      ),
      items: const [
        DropdownMenuItem(value: 'US', child: Text('United States')),
        DropdownMenuItem(value: 'CA', child: Text('Canada')),
        DropdownMenuItem(value: 'UK', child: Text('United Kingdom')),
        DropdownMenuItem(value: 'DE', child: Text('Germany')),
        DropdownMenuItem(value: 'FR', child: Text('France')),
      ],
      onChanged: (value) {
        setState(() {
          _selectedCountry = value;
        });
      },
    );
  }

  Widget _buildMultilineTextField() {
    return TextFormField(
      controller: _messageController,
      maxLines: 4,
      decoration: const InputDecoration(
        labelText: 'Message',
        hintText: 'Enter your message here...',
        prefixIcon: Padding(
          padding: EdgeInsets.only(bottom: 60),
          child: Icon(Icons.message),
        ),
        alignLabelWithHint: true,
      ),
    );
  }

  Widget _buildCheckboxField() {
    return CheckboxListTile(
      value: _agreeToTerms,
      onChanged: (value) {
        setState(() {
          _agreeToTerms = value ?? false;
        });
      },
      title: const Text('I agree to the terms and conditions'),
      controlAffinity: ListTileControlAffinity.leading,
    );
  }

  Widget _buildInputVariations() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Input Variations',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            TextField(
              decoration: InputDecoration(
                labelText: 'Underline Style',
                border: const UnderlineInputBorder(),
                enabledBorder: const UnderlineInputBorder(
                  borderSide: BorderSide(color: Colors.grey),
                ),
                focusedBorder: UnderlineInputBorder(
                  borderSide: BorderSide(color: Theme.of(context).primaryColor, width: 2),
                ),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              decoration: InputDecoration(
                labelText: 'No Border Style',
                border: InputBorder.none,
                filled: true,
                fillColor: Colors.grey.shade100,
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              decoration: InputDecoration(
                labelText: 'Custom Colors',
                border: const OutlineInputBorder(),
                focusedBorder: const OutlineInputBorder(
                  borderSide: BorderSide(color: Colors.purple, width: 2),
                ),
                labelStyle: const TextStyle(color: Colors.purple),
                prefixIcon: const Icon(Icons.star, color: Colors.purple),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildActionButtons() {
    return Row(
      children: [
        Expanded(
          child: OutlinedButton(
            onPressed: () {
              _formKey.currentState?.reset();
              _nameController.clear();
              _emailController.clear();
              _phoneController.clear();
              _passwordController.clear();
              _messageController.clear();
              setState(() {
                _selectedCountry = null;
                _agreeToTerms = false;
              });
            },
            child: const Text('Reset'),
          ),
        ),
        const SizedBox(width: 16),
        Expanded(
          child: ElevatedButton(
            onPressed: () {
              if (_formKey.currentState?.validate() ?? false) {
                ScaffoldMessenger.of(context).showSnackBar(
                  const SnackBar(content: Text('Form submitted successfully!')),
                );
              }
            },
            child: const Text('Submit'),
          ),
        ),
      ],
    );
  }
}
```

Input decoration themes provide consistent styling for all text input  
fields in your app. Key properties include border styles for different  
states, fill colors, text styles for labels and hints, padding, and  
icon colors. This creates a cohesive form experience throughout your  
application.  

## Icon Theme Customization

Configuring icon themes for consistent iconography across your app.  

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
      title: 'Icon Theme Customization',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.amber),
        iconTheme: const IconThemeData(
          color: Color(0xFF424242),
          size: 24,
          opacity: 0.87,
        ),
        primaryIconTheme: const IconThemeData(
          color: Colors.white,
          size: 24,
          opacity: 1.0,
        ),
        appBarTheme: const AppBarTheme(
          backgroundColor: Colors.amber,
          iconTheme: IconThemeData(
            color: Colors.white,
            size: 24,
          ),
          actionsIconTheme: IconThemeData(
            color: Colors.white,
            size: 24,
          ),
        ),
      ),
      home: const IconThemePage(),
    );
  }
}

class IconThemePage extends StatefulWidget {
  const IconThemePage({super.key});

  @override
  State<IconThemePage> createState() => _IconThemePageState();
}

class _IconThemePageState extends State<IconThemePage> {
  int _selectedIndex = 0;
  double _iconSize = 24.0;
  double _iconOpacity = 1.0;
  Color _iconColor = const Color(0xFF424242);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Icon Theme Customization'),
        leading: const Icon(Icons.menu),
        actions: const [
          Icon(Icons.search),
          SizedBox(width: 8),
          Icon(Icons.notifications),
          SizedBox(width: 8),
          Icon(Icons.more_vert),
          SizedBox(width: 8),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Icon Theme Examples',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 20),
            _buildGlobalIconTheme(),
            const SizedBox(height: 20),
            _buildContextualIconThemes(),
            const SizedBox(height: 20),
            _buildIconSizeExamples(),
            const SizedBox(height: 20),
            _buildCustomIconTheme(),
            const SizedBox(height: 20),
            _buildIconCustomization(),
          ],
        ),
      ),
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _selectedIndex,
        onTap: (index) {
          setState(() {
            _selectedIndex = index;
          });
        },
        type: BottomNavigationBarType.fixed,
        items: const [
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.explore),
            label: 'Explore',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.favorite),
            label: 'Favorites',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.person),
            label: 'Profile',
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        child: const Icon(Icons.add),
      ),
    );
  }

  Widget _buildGlobalIconTheme() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Global Icon Theme',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            const Text(
              'These icons use the global icon theme settings:',
            ),
            const SizedBox(height: 16),
            Wrap(
              spacing: 16,
              runSpacing: 16,
              children: const [
                Icon(Icons.home),
                Icon(Icons.star),
                Icon(Icons.favorite),
                Icon(Icons.bookmark),
                Icon(Icons.share),
                Icon(Icons.download),
                Icon(Icons.settings),
                Icon(Icons.help),
              ],
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                const Text('Size: '),
                Text(
                  '${Theme.of(context).iconTheme.size}px',
                  style: const TextStyle(fontWeight: FontWeight.bold),
                ),
                const SizedBox(width: 20),
                const Text('Opacity: '),
                Text(
                  '${(Theme.of(context).iconTheme.opacity! * 100).toInt()}%',
                  style: const TextStyle(fontWeight: FontWeight.bold),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildContextualIconThemes() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Contextual Icon Themes',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 12),
            const Text(
              'Different contexts can have different icon themes:',
            ),
            const SizedBox(height: 16),
            _buildIconThemeExample(
              'Primary Icons (AppBar)',
              Theme.of(context).primaryIconTheme,
              const [
                Icon(Icons.menu),
                Icon(Icons.search),
                Icon(Icons.notifications),
                Icon(Icons.more_vert),
              ],
            ),
            const SizedBox(height: 16),
            _buildIconThemeExample(
              'Success Icons',
              const IconThemeData(color: Colors.green, size: 28),
              const [
                Icon(Icons.check_circle),
                Icon(Icons.verified),
                Icon(Icons.task_alt),
                Icon(Icons.done),
              ],
            ),
            const SizedBox(height: 16),
            _buildIconThemeExample(
              'Error Icons',
              const IconThemeData(color: Colors.red, size: 28),
              const [
                Icon(Icons.error),
                Icon(Icons.warning),
                Icon(Icons.cancel),
                Icon(Icons.dangerous),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildIconThemeExample(
    String title,
    IconThemeData theme,
    List<Icon> icons,
  ) {
    return Container(
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: Colors.grey.shade50,
        borderRadius: BorderRadius.circular(8),
        border: Border.all(color: Colors.grey.shade200),
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            title,
            style: Theme.of(context).textTheme.titleSmall,
          ),
          const SizedBox(height: 8),
          IconTheme(
            data: theme,
            child: Wrap(
              spacing: 16,
              children: icons,
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildIconSizeExamples() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Icon Size Examples',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: [
                _buildSizedIconExample('Small', 16, Icons.star),
                _buildSizedIconExample('Medium', 24, Icons.star),
                _buildSizedIconExample('Large', 32, Icons.star),
                _buildSizedIconExample('X-Large', 48, Icons.star),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildSizedIconExample(String label, double size, IconData icon) {
    return Column(
      children: [
        Icon(icon, size: size, color: Colors.amber),
        const SizedBox(height: 8),
        Text(
          label,
          style: Theme.of(context).textTheme.bodySmall,
        ),
        Text(
          '${size.toInt()}px',
          style: Theme.of(context).textTheme.bodySmall?.copyWith(
            color: Colors.grey,
          ),
        ),
      ],
    );
  }

  Widget _buildCustomIconTheme() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Custom Icon Theme',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            IconTheme(
              data: IconThemeData(
                color: _iconColor,
                size: _iconSize,
                opacity: _iconOpacity,
              ),
              child: const Wrap(
                spacing: 16,
                runSpacing: 16,
                children: [
                  Icon(Icons.favorite),
                  Icon(Icons.bookmark),
                  Icon(Icons.thumb_up),
                  Icon(Icons.share),
                  Icon(Icons.comment),
                  Icon(Icons.visibility),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildIconCustomization() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Icon Customization',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            Text(
              'Size: ${_iconSize.toInt()}px',
              style: Theme.of(context).textTheme.titleSmall,
            ),
            Slider(
              value: _iconSize,
              min: 16,
              max: 48,
              divisions: 16,
              onChanged: (value) {
                setState(() {
                  _iconSize = value;
                });
              },
            ),
            const SizedBox(height: 16),
            Text(
              'Opacity: ${(_iconOpacity * 100).toInt()}%',
              style: Theme.of(context).textTheme.titleSmall,
            ),
            Slider(
              value: _iconOpacity,
              min: 0.1,
              max: 1.0,
              divisions: 9,
              onChanged: (value) {
                setState(() {
                  _iconOpacity = value;
                });
              },
            ),
            const SizedBox(height: 16),
            Text(
              'Color',
              style: Theme.of(context).textTheme.titleSmall,
            ),
            const SizedBox(height: 8),
            Wrap(
              spacing: 8,
              children: [
                _buildColorOption(const Color(0xFF424242)),
                _buildColorOption(Colors.blue),
                _buildColorOption(Colors.red),
                _buildColorOption(Colors.green),
                _buildColorOption(Colors.orange),
                _buildColorOption(Colors.purple),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildColorOption(Color color) {
    final isSelected = _iconColor == color;
    return GestureDetector(
      onTap: () {
        setState(() {
          _iconColor = color;
        });
      },
      child: Container(
        width: 40,
        height: 40,
        decoration: BoxDecoration(
          color: color,
          shape: BoxShape.circle,
          border: Border.all(
            color: isSelected ? Colors.white : Colors.transparent,
            width: 3,
          ),
          boxShadow: isSelected
              ? [
                  BoxShadow(
                    color: color.withOpacity(0.5),
                    blurRadius: 8,
                    spreadRadius: 2,
                  ),
                ]
              : null,
        ),
        child: isSelected
            ? const Icon(Icons.check, color: Colors.white, size: 20)
            : null,
      ),
    );
  }
}
```

Icon themes provide consistent styling for icons throughout your app.  
The global iconTheme affects most icons, while primaryIconTheme applies  
to icons in primary-colored contexts like AppBars. Local IconTheme  
widgets can override global settings for specific sections or components.  
