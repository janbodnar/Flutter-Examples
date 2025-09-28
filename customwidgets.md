# Flutter Custom Widgets & Themes - 30 Essential Examples

This comprehensive guide covers Flutter custom widgets and advanced theming  
with 30 practical examples, from basic custom widgets to complex reusable  
components and sophisticated theming systems. Each example demonstrates  
different aspects of widget creation and theme customization, helping you  
create maintainable, consistent, and beautiful Flutter applications.

## Basic Custom Widget

Creating a simple custom widget with reusable properties.  

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
      title: 'Custom Widget',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        useMaterial3: true,
      ),
      home: const HomePage(),
    );
  }
}

class CustomCard extends StatelessWidget {
  final String title;
  final String description;
  final IconData? icon;
  final Color? backgroundColor;
  final VoidCallback? onTap;

  const CustomCard({
    super.key,
    required this.title,
    required this.description,
    this.icon,
    this.backgroundColor,
    this.onTap,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      color: backgroundColor,
      elevation: 4,
      margin: const EdgeInsets.all(8),
      child: InkWell(
        onTap: onTap,
        borderRadius: BorderRadius.circular(12),
        child: Padding(
          padding: const EdgeInsets.all(16),
          child: Row(
            children: [
              if (icon != null) ...[
                Icon(
                  icon,
                  size: 32,
                  color: Theme.of(context).colorScheme.primary,
                ),
                const SizedBox(width: 16),
              ],
              Expanded(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      title,
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    const SizedBox(height: 8),
                    Text(
                      description,
                      style: Theme.of(context).textTheme.bodyMedium,
                    ),
                  ],
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Widget Example'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: ListView(
        padding: const EdgeInsets.all(16),
        children: [
          CustomCard(
            title: 'Settings',
            description: 'Configure your application preferences',
            icon: Icons.settings,
            onTap: () => print('Settings tapped'),
          ),
          CustomCard(
            title: 'Profile',
            description: 'Manage your user profile information',
            icon: Icons.person,
            backgroundColor: Colors.blue.shade50,
            onTap: () => print('Profile tapped'),
          ),
          CustomCard(
            title: 'Notifications',
            description: 'Control notification settings and preferences',
            icon: Icons.notifications,
            backgroundColor: Colors.green.shade50,
            onTap: () => print('Notifications tapped'),
          ),
        ],
      ),
    );
  }
}
```

This example demonstrates creating a reusable CustomCard widget with  
configurable properties. The widget accepts title, description, icon,  
background color, and tap callback, making it flexible for different  
use cases while maintaining consistent styling and behavior.  

## Custom Button Widget

Building a sophisticated button widget with multiple variants and states.  

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
      title: 'Custom Button Widget',
      theme: ThemeData(
        primarySwatch: Colors.purple,
        useMaterial3: true,
      ),
      home: const ButtonDemoPage(),
    );
  }
}

enum ButtonVariant { primary, secondary, outlined, text, danger }

enum ButtonSize { small, medium, large }

class CustomButton extends StatefulWidget {
  final String text;
  final VoidCallback? onPressed;
  final ButtonVariant variant;
  final ButtonSize size;
  final IconData? icon;
  final bool loading;
  final double? width;

  const CustomButton({
    super.key,
    required this.text,
    this.onPressed,
    this.variant = ButtonVariant.primary,
    this.size = ButtonSize.medium,
    this.icon,
    this.loading = false,
    this.width,
  });

  @override
  State<CustomButton> createState() => _CustomButtonState();
}

class _CustomButtonState extends State<CustomButton>
    with TickerProviderStateMixin {
  late AnimationController _animationController;
  late Animation<double> _scaleAnimation;

  @override
  void initState() {
    super.initState();
    _animationController = AnimationController(
      duration: const Duration(milliseconds: 150),
      vsync: this,
    );
    _scaleAnimation = Tween<double>(
      begin: 1.0,
      end: 0.95,
    ).animate(CurvedAnimation(
      parent: _animationController,
      curve: Curves.easeInOut,
    ));
  }

  @override
  void dispose() {
    _animationController.dispose();
    super.dispose();
  }

  Color _getBackgroundColor(BuildContext context) {
    final theme = Theme.of(context);
    switch (widget.variant) {
      case ButtonVariant.primary:
        return theme.colorScheme.primary;
      case ButtonVariant.secondary:
        return theme.colorScheme.secondary;
      case ButtonVariant.outlined:
        return Colors.transparent;
      case ButtonVariant.text:
        return Colors.transparent;
      case ButtonVariant.danger:
        return theme.colorScheme.error;
    }
  }

  Color _getTextColor(BuildContext context) {
    final theme = Theme.of(context);
    switch (widget.variant) {
      case ButtonVariant.primary:
        return theme.colorScheme.onPrimary;
      case ButtonVariant.secondary:
        return theme.colorScheme.onSecondary;
      case ButtonVariant.outlined:
        return theme.colorScheme.primary;
      case ButtonVariant.text:
        return theme.colorScheme.primary;
      case ButtonVariant.danger:
        return theme.colorScheme.onError;
    }
  }

  EdgeInsets _getPadding() {
    switch (widget.size) {
      case ButtonSize.small:
        return const EdgeInsets.symmetric(horizontal: 12, vertical: 8);
      case ButtonSize.medium:
        return const EdgeInsets.symmetric(horizontal: 16, vertical: 12);
      case ButtonSize.large:
        return const EdgeInsets.symmetric(horizontal: 24, vertical: 16);
    }
  }

  double _getFontSize() {
    switch (widget.size) {
      case ButtonSize.small:
        return 14;
      case ButtonSize.medium:
        return 16;
      case ButtonSize.large:
        return 18;
    }
  }

  @override
  Widget build(BuildContext context) {
    final backgroundColor = _getBackgroundColor(context);
    final textColor = _getTextColor(context);
    final isDisabled = widget.onPressed == null || widget.loading;

    return ScaleTransition(
      scale: _scaleAnimation,
      child: SizedBox(
        width: widget.width,
        child: Material(
          color: backgroundColor.withOpacity(isDisabled ? 0.5 : 1.0),
          borderRadius: BorderRadius.circular(8),
          elevation: widget.variant == ButtonVariant.text ? 0 : 2,
          child: InkWell(
            onTap: isDisabled ? null : widget.onPressed,
            onTapDown: (_) => _animationController.forward(),
            onTapUp: (_) => _animationController.reverse(),
            onTapCancel: () => _animationController.reverse(),
            borderRadius: BorderRadius.circular(8),
            child: Container(
              padding: _getPadding(),
              decoration: widget.variant == ButtonVariant.outlined
                  ? BoxDecoration(
                      border: Border.all(
                        color: textColor.withOpacity(isDisabled ? 0.5 : 1.0),
                        width: 1.5,
                      ),
                      borderRadius: BorderRadius.circular(8),
                    )
                  : null,
              child: Row(
                mainAxisSize: MainAxisSize.min,
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  if (widget.loading)
                    SizedBox(
                      width: _getFontSize(),
                      height: _getFontSize(),
                      child: CircularProgressIndicator(
                        strokeWidth: 2,
                        valueColor: AlwaysStoppedAnimation<Color>(textColor),
                      ),
                    )
                  else if (widget.icon != null)
                    Icon(
                      widget.icon,
                      size: _getFontSize() + 2,
                      color: textColor.withOpacity(isDisabled ? 0.5 : 1.0),
                    ),
                  if ((widget.icon != null || widget.loading) && 
                      widget.text.isNotEmpty)
                    const SizedBox(width: 8),
                  if (widget.text.isNotEmpty)
                    Text(
                      widget.text,
                      style: TextStyle(
                        fontSize: _getFontSize(),
                        fontWeight: FontWeight.w600,
                        color: textColor.withOpacity(isDisabled ? 0.5 : 1.0),
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

class ButtonDemoPage extends StatefulWidget {
  const ButtonDemoPage({super.key});

  @override
  State<ButtonDemoPage> createState() => _ButtonDemoPageState();
}

class _ButtonDemoPageState extends State<ButtonDemoPage> {
  bool _loading = false;

  void _toggleLoading() {
    setState(() {
      _loading = !_loading;
    });
    
    if (_loading) {
      Future.delayed(const Duration(seconds: 2), () {
        if (mounted) {
          setState(() {
            _loading = false;
          });
        }
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Button Widget'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            Text(
              'Button Variants',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            CustomButton(
              text: 'Primary Button',
              variant: ButtonVariant.primary,
              onPressed: () => print('Primary pressed'),
            ),
            const SizedBox(height: 12),
            CustomButton(
              text: 'Secondary Button',
              variant: ButtonVariant.secondary,
              onPressed: () => print('Secondary pressed'),
            ),
            const SizedBox(height: 12),
            CustomButton(
              text: 'Outlined Button',
              variant: ButtonVariant.outlined,
              onPressed: () => print('Outlined pressed'),
            ),
            const SizedBox(height: 12),
            CustomButton(
              text: 'Text Button',
              variant: ButtonVariant.text,
              onPressed: () => print('Text pressed'),
            ),
            const SizedBox(height: 12),
            CustomButton(
              text: 'Danger Button',
              variant: ButtonVariant.danger,
              onPressed: () => print('Danger pressed'),
            ),
            const SizedBox(height: 24),
            Text(
              'Button Sizes',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                CustomButton(
                  text: 'Small',
                  size: ButtonSize.small,
                  onPressed: () => print('Small pressed'),
                ),
                const SizedBox(width: 12),
                CustomButton(
                  text: 'Medium',
                  size: ButtonSize.medium,
                  onPressed: () => print('Medium pressed'),
                ),
                const SizedBox(width: 12),
                CustomButton(
                  text: 'Large',
                  size: ButtonSize.large,
                  onPressed: () => print('Large pressed'),
                ),
              ],
            ),
            const SizedBox(height: 24),
            Text(
              'Button with Icons',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            CustomButton(
              text: 'Download',
              icon: Icons.download,
              onPressed: () => print('Download pressed'),
            ),
            const SizedBox(height: 12),
            CustomButton(
              text: 'Loading State',
              loading: _loading,
              onPressed: _toggleLoading,
            ),
            const SizedBox(height: 12),
            CustomButton(
              text: 'Disabled Button',
              onPressed: null,
            ),
          ],
        ),
      ),
    );
  }
}
```

This advanced custom button demonstrates multiple variants, sizes, states,  
and animations. It includes loading states, disabled states, icon support,  
and press animations. The widget follows Material Design principles while  
providing extensive customization options through enums and properties.  

## Stateful Custom Widget

Creating a custom widget that manages its own state and interactions.  

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
      title: 'Stateful Custom Widget',
      theme: ThemeData(
        primarySwatch: Colors.indigo,
        useMaterial3: true,
      ),
      home: const RatingDemoPage(),
    );
  }
}

class StarRating extends StatefulWidget {
  final int maxRating;
  final double initialRating;
  final double size;
  final Color activeColor;
  final Color inactiveColor;
  final ValueChanged<double>? onRatingChanged;
  final bool allowHalfRating;
  final bool readOnly;

  const StarRating({
    super.key,
    this.maxRating = 5,
    this.initialRating = 0,
    this.size = 32,
    this.activeColor = Colors.amber,
    this.inactiveColor = Colors.grey,
    this.onRatingChanged,
    this.allowHalfRating = true,
    this.readOnly = false,
  });

  @override
  State<StarRating> createState() => _StarRatingState();
}

class _StarRatingState extends State<StarRating>
    with TickerProviderStateMixin {
  late double _currentRating;
  late AnimationController _animationController;
  late List<AnimationController> _starAnimations;

  @override
  void initState() {
    super.initState();
    _currentRating = widget.initialRating;
    
    _animationController = AnimationController(
      duration: const Duration(milliseconds: 200),
      vsync: this,
    );

    _starAnimations = List.generate(
      widget.maxRating,
      (index) => AnimationController(
        duration: Duration(milliseconds: 300 + (index * 100)),
        vsync: this,
      ),
    );

    _animateStars();
  }

  @override
  void dispose() {
    _animationController.dispose();
    for (var controller in _starAnimations) {
      controller.dispose();
    }
    super.dispose();
  }

  void _animateStars() {
    for (int i = 0; i < _starAnimations.length; i++) {
      Future.delayed(Duration(milliseconds: i * 100), () {
        if (mounted) {
          _starAnimations[i].forward();
        }
      });
    }
  }

  void _updateRating(double rating) {
    if (widget.readOnly) return;
    
    setState(() {
      _currentRating = rating;
    });
    
    widget.onRatingChanged?.call(rating);
    
    // Trigger animation for visual feedback
    _animationController.forward().then((_) {
      _animationController.reverse();
    });
  }

  Widget _buildStar(int index) {
    final starValue = index + 1.0;
    final isActive = _currentRating >= starValue;
    final isHalfActive = widget.allowHalfRating && 
        _currentRating >= starValue - 0.5 && 
        _currentRating < starValue;

    return ScaleTransition(
      scale: Tween<double>(begin: 0, end: 1).animate(
        CurvedAnimation(
          parent: _starAnimations[index],
          curve: Curves.elasticOut,
        ),
      ),
      child: ScaleTransition(
        scale: Tween<double>(begin: 1, end: 1.2).animate(
          CurvedAnimation(
            parent: _animationController,
            curve: Curves.elasticOut,
          ),
        ),
        child: GestureDetector(
          onTap: widget.readOnly ? null : () => _updateRating(starValue),
          onPanUpdate: widget.readOnly ? null : (details) {
            final RenderBox box = context.findRenderObject() as RenderBox;
            final localPosition = box.globalToLocal(details.globalPosition);
            final starWidth = box.size.width / widget.maxRating;
            final rating = (localPosition.dx / starWidth).clamp(0.0, widget.maxRating.toDouble());
            
            if (widget.allowHalfRating) {
              final roundedRating = (rating * 2).round() / 2;
              _updateRating(roundedRating);
            } else {
              _updateRating(rating.round().toDouble());
            }
          },
          child: Container(
            padding: const EdgeInsets.all(4),
            child: Stack(
              children: [
                Icon(
                  Icons.star,
                  size: widget.size,
                  color: widget.inactiveColor,
                ),
                if (isHalfActive)
                  ClipRect(
                    clipper: HalfClipper(),
                    child: Icon(
                      Icons.star,
                      size: widget.size,
                      color: widget.activeColor,
                    ),
                  )
                else if (isActive)
                  Icon(
                    Icons.star,
                    size: widget.size,
                    color: widget.activeColor,
                  ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisSize: MainAxisSize.min,
      children: List.generate(
        widget.maxRating,
        (index) => _buildStar(index),
      ),
    );
  }
}

class HalfClipper extends CustomClipper<Rect> {
  @override
  Rect getClip(Size size) {
    return Rect.fromLTRB(0, 0, size.width / 2, size.height);
  }

  @override
  bool shouldReclip(CustomClipper<Rect> oldClipper) => false;
}

class RatingDemoPage extends StatefulWidget {
  const RatingDemoPage({super.key});

  @override
  State<RatingDemoPage> createState() => _RatingDemoPageState();
}

class _RatingDemoPageState extends State<RatingDemoPage> {
  double _rating1 = 3.0;
  double _rating2 = 4.5;
  double _rating3 = 2.0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Star Rating Widget'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Interactive Star Rating',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text('Rate this product:'),
                    const SizedBox(height: 8),
                    StarRating(
                      initialRating: _rating1,
                      onRatingChanged: (rating) {
                        setState(() {
                          _rating1 = rating;
                        });
                      },
                    ),
                    const SizedBox(height: 8),
                    Text('Rating: ${_rating1.toStringAsFixed(1)} stars'),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 24),
            Text(
              'Different Sizes & Colors',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text('Small rating (20px):'),
                    const SizedBox(height: 8),
                    StarRating(
                      size: 20,
                      activeColor: Colors.red,
                      initialRating: _rating2,
                      onRatingChanged: (rating) {
                        setState(() {
                          _rating2 = rating;
                        });
                      },
                    ),
                    const SizedBox(height: 16),
                    const Text('Large rating (48px):'),
                    const SizedBox(height: 8),
                    StarRating(
                      size: 48,
                      activeColor: Colors.green,
                      initialRating: _rating3,
                      onRatingChanged: (rating) {
                        setState(() {
                          _rating3 = rating;
                        });
                      },
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 24),
            Text(
              'Read-Only Rating',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text('Average rating: 4.3 stars'),
                    const SizedBox(height: 8),
                    const StarRating(
                      initialRating: 4.3,
                      readOnly: true,
                      activeColor: Colors.orange,
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(height: 24),
            Text(
              'No Half Rating',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text('Whole stars only:'),
                    const SizedBox(height: 8),
                    StarRating(
                      allowHalfRating: false,
                      activeColor: Colors.purple,
                      onRatingChanged: (rating) {
                        print('Whole star rating: $rating');
                      },
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

This example demonstrates a complex stateful custom widget with animations,  
gesture handling, and multiple configuration options. The StarRating widget  
manages its internal state, supports half-star ratings, different sizes and  
colors, read-only mode, and provides smooth animations and user feedback.  

## Custom Input Widget

Building a sophisticated form input widget with validation and styling.  

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
      title: 'Custom Input Widget',
      theme: ThemeData(
        primarySwatch: Colors.teal,
        useMaterial3: true,
      ),
      home: const InputDemoPage(),
    );
  }
}

class CustomInput extends StatefulWidget {
  final String label;
  final String? hint;
  final String? helperText;
  final IconData? prefixIcon;
  final Widget? suffix;
  final TextEditingController? controller;
  final String? Function(String?)? validator;
  final TextInputType? keyboardType;
  final bool obscureText;
  final bool readOnly;
  final int? maxLines;
  final int? maxLength;
  final List<TextInputFormatter>? inputFormatters;
  final void Function(String)? onChanged;
  final void Function()? onTap;
  final Color? focusColor;
  final Color? errorColor;
  final bool required;
  final String? initialValue;

  const CustomInput({
    super.key,
    required this.label,
    this.hint,
    this.helperText,
    this.prefixIcon,
    this.suffix,
    this.controller,
    this.validator,
    this.keyboardType,
    this.obscureText = false,
    this.readOnly = false,
    this.maxLines = 1,
    this.maxLength,
    this.inputFormatters,
    this.onChanged,
    this.onTap,
    this.focusColor,
    this.errorColor,
    this.required = false,
    this.initialValue,
  });

  @override
  State<CustomInput> createState() => _CustomInputState();
}

class _CustomInputState extends State<CustomInput>
    with TickerProviderStateMixin {
  late TextEditingController _controller;
  late FocusNode _focusNode;
  late AnimationController _animationController;
  late AnimationController _shakeController;
  late Animation<double> _labelAnimation;
  late Animation<double> _shakeAnimation;
  
  String? _errorText;
  bool _isFocused = false;
  bool _hasContent = false;

  @override
  void initState() {
    super.initState();
    
    _controller = widget.controller ?? TextEditingController(
      text: widget.initialValue,
    );
    _focusNode = FocusNode();
    
    _animationController = AnimationController(
      duration: const Duration(milliseconds: 200),
      vsync: this,
    );
    
    _shakeController = AnimationController(
      duration: const Duration(milliseconds: 500),
      vsync: this,
    );
    
    _labelAnimation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _animationController,
      curve: Curves.easeOut,
    ));
    
    _shakeAnimation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _shakeController,
      curve: Curves.elasticIn,
    ));

    _focusNode.addListener(_onFocusChange);
    _controller.addListener(_onTextChange);
    
    _hasContent = _controller.text.isNotEmpty;
    if (_hasContent || _isFocused) {
      _animationController.value = 1;
    }
  }

  @override
  void dispose() {
    if (widget.controller == null) {
      _controller.dispose();
    }
    _focusNode.dispose();
    _animationController.dispose();
    _shakeController.dispose();
    super.dispose();
  }

  void _onFocusChange() {
    setState(() {
      _isFocused = _focusNode.hasFocus;
    });
    
    if (_isFocused || _hasContent) {
      _animationController.forward();
    } else {
      _animationController.reverse();
    }
  }

  void _onTextChange() {
    final hasContent = _controller.text.isNotEmpty;
    if (hasContent != _hasContent) {
      setState(() {
        _hasContent = hasContent;
      });
      
      if (_hasContent || _isFocused) {
        _animationController.forward();
      } else {
        _animationController.reverse();
      }
    }
    
    widget.onChanged?.call(_controller.text);
    
    if (_errorText != null) {
      _validate();
    }
  }

  void _validate() {
    if (widget.validator != null) {
      final error = widget.validator!(_controller.text);
      if (error != _errorText) {
        setState(() {
          _errorText = error;
        });
        
        if (error != null) {
          _shakeController.forward().then((_) {
            _shakeController.reset();
          });
        }
      }
    }
  }

  Color _getBorderColor() {
    if (_errorText != null) {
      return widget.errorColor ?? Colors.red;
    } else if (_isFocused) {
      return widget.focusColor ?? Theme.of(context).colorScheme.primary;
    } else {
      return Colors.grey.shade400;
    }
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    return AnimatedBuilder(
      animation: _shakeAnimation,
      builder: (context, child) {
        final shakeOffset = _shakeAnimation.value * 10 * 
            (1 - _shakeAnimation.value) * 2;
        
        return Transform.translate(
          offset: Offset(shakeOffset, 0),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Container(
                decoration: BoxDecoration(
                  borderRadius: BorderRadius.circular(8),
                  border: Border.all(
                    color: _getBorderColor(),
                    width: _isFocused ? 2 : 1,
                  ),
                ),
                child: Stack(
                  children: [
                    TextFormField(
                      controller: _controller,
                      focusNode: _focusNode,
                      keyboardType: widget.keyboardType,
                      obscureText: widget.obscureText,
                      readOnly: widget.readOnly,
                      maxLines: widget.maxLines,
                      maxLength: widget.maxLength,
                      inputFormatters: widget.inputFormatters,
                      onTap: widget.onTap,
                      style: theme.textTheme.bodyLarge,
                      decoration: InputDecoration(
                        hintText: (_isFocused || _hasContent) ? widget.hint : null,
                        prefixIcon: widget.prefixIcon != null
                            ? Icon(
                                widget.prefixIcon,
                                color: _getBorderColor(),
                              )
                            : null,
                        suffix: widget.suffix,
                        border: InputBorder.none,
                        contentPadding: EdgeInsets.symmetric(
                          horizontal: widget.prefixIcon != null ? 8 : 16,
                          vertical: 16,
                        ),
                        counterText: '',
                      ),
                    ),
                    // Animated floating label
                    AnimatedBuilder(
                      animation: _labelAnimation,
                      builder: (context, child) {
                        final progress = _labelAnimation.value;
                        final scale = 0.75 + (0.25 * (1 - progress));
                        final offsetY = 16 + (-8 * progress);
                        final offsetX = widget.prefixIcon != null
                            ? 48 + (-32 * progress)
                            : 16 + (-16 * progress);
                        
                        return Positioned(
                          left: offsetX,
                          top: offsetY,
                          child: Transform.scale(
                            scale: scale,
                            alignment: Alignment.centerLeft,
                            child: Container(
                              padding: progress > 0.5
                                  ? const EdgeInsets.symmetric(horizontal: 4)
                                  : EdgeInsets.zero,
                              color: progress > 0.5
                                  ? theme.scaffoldBackgroundColor
                                  : Colors.transparent,
                              child: Text(
                                widget.label + (widget.required ? ' *' : ''),
                                style: TextStyle(
                                  color: _getBorderColor(),
                                  fontWeight: progress > 0.5
                                      ? FontWeight.w500
                                      : FontWeight.normal,
                                ),
                              ),
                            ),
                          ),
                        );
                      },
                    ),
                  ],
                ),
              ),
              if (_errorText != null) ...[
                const SizedBox(height: 4),
                Text(
                  _errorText!,
                  style: TextStyle(
                    color: widget.errorColor ?? Colors.red,
                    fontSize: 12,
                  ),
                ),
              ] else if (widget.helperText != null) ...[
                const SizedBox(height: 4),
                Text(
                  widget.helperText!,
                  style: TextStyle(
                    color: Colors.grey.shade600,
                    fontSize: 12,
                  ),
                ),
              ],
              if (widget.maxLength != null) ...[
                const SizedBox(height: 4),
                Align(
                  alignment: Alignment.centerRight,
                  child: Text(
                    '${_controller.text.length}/${widget.maxLength}',
                    style: TextStyle(
                      color: Colors.grey.shade600,
                      fontSize: 12,
                    ),
                  ),
                ),
              ],
            ],
          ),
        );
      },
    );
  }
}

class InputDemoPage extends StatefulWidget {
  const InputDemoPage({super.key});

  @override
  State<InputDemoPage> createState() => _InputDemoPageState();
}

class _InputDemoPageState extends State<InputDemoPage> {
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _phoneController = TextEditingController();
  final _passwordController = TextEditingController();
  final _bioController = TextEditingController();

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _phoneController.dispose();
    _passwordController.dispose();
    _bioController.dispose();
    super.dispose();
  }

  String? _validateEmail(String? value) {
    if (value == null || value.isEmpty) {
      return 'Email is required';
    }
    final emailRegex = RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$');
    if (!emailRegex.hasMatch(value)) {
      return 'Enter a valid email address';
    }
    return null;
  }

  String? _validatePassword(String? value) {
    if (value == null || value.isEmpty) {
      return 'Password is required';
    }
    if (value.length < 8) {
      return 'Password must be at least 8 characters';
    }
    return null;
  }

  String? _validatePhone(String? value) {
    if (value == null || value.isEmpty) {
      return null; // Optional field
    }
    final phoneRegex = RegExp(r'^\+?[\d\s\-\(\)]{10,}$');
    if (!phoneRegex.hasMatch(value)) {
      return 'Enter a valid phone number';
    }
    return null;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Input Widget'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            Text(
              'User Registration Form',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 24),
            CustomInput(
              label: 'Full Name',
              hint: 'Enter your full name',
              prefixIcon: Icons.person,
              controller: _nameController,
              required: true,
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Name is required';
                }
                return null;
              },
            ),
            const SizedBox(height: 20),
            CustomInput(
              label: 'Email Address',
              hint: 'your.email@example.com',
              prefixIcon: Icons.email,
              controller: _emailController,
              keyboardType: TextInputType.emailAddress,
              required: true,
              validator: _validateEmail,
            ),
            const SizedBox(height: 20),
            CustomInput(
              label: 'Phone Number',
              hint: '+1 (555) 123-4567',
              prefixIcon: Icons.phone,
              controller: _phoneController,
              keyboardType: TextInputType.phone,
              helperText: 'Optional - for account recovery',
              validator: _validatePhone,
              inputFormatters: [
                FilteringTextInputFormatter.allow(RegExp(r'[\d\s\-\(\)\+]')),
              ],
            ),
            const SizedBox(height: 20),
            CustomInput(
              label: 'Password',
              hint: 'Create a strong password',
              prefixIcon: Icons.lock,
              controller: _passwordController,
              obscureText: true,
              required: true,
              validator: _validatePassword,
              helperText: 'Must be at least 8 characters long',
            ),
            const SizedBox(height: 20),
            CustomInput(
              label: 'Bio',
              hint: 'Tell us about yourself...',
              prefixIcon: Icons.description,
              controller: _bioController,
              maxLines: 4,
              maxLength: 200,
              helperText: 'Optional - share a brief bio',
            ),
            const SizedBox(height: 32),
            ElevatedButton(
              onPressed: () {
                // Validate all fields
                final fields = [
                  _nameController,
                  _emailController,
                  _phoneController,
                  _passwordController,
                ];
                
                // Trigger validation by calling setState
                setState(() {});
                
                ScaffoldMessenger.of(context).showSnackBar(
                  const SnackBar(
                    content: Text('Form validation triggered!'),
                  ),
                );
              },
              child: const Padding(
                padding: EdgeInsets.symmetric(vertical: 16),
                child: Text('Create Account'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

This sophisticated custom input widget features animated floating labels,  
validation with shake animations, customizable styling, helper text,  
character counting, and comprehensive form field behaviors. It demonstrates  
advanced animation techniques and form handling best practices.  

## Extending Built-in Widgets

Creating enhanced versions of existing Flutter widgets with additional features.  

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
      title: 'Enhanced Widgets',
      theme: ThemeData(
        primarySwatch: Colors.deepPurple,
        useMaterial3: true,
      ),
      home: const EnhancedWidgetsPage(),
    );
  }
}

// Enhanced Card with additional features
class EnhancedCard extends Card {
  final Widget? header;
  final Widget? footer;
  final EdgeInsets? padding;
  final bool showBorder;
  final Color? borderColor;
  final double borderWidth;
  final bool animated;
  final Duration animationDuration;

  const EnhancedCard({
    super.key,
    super.child,
    super.color,
    super.shadowColor,
    super.surfaceTintColor,
    super.elevation = 1.0,
    super.shape,
    super.borderOnForeground = true,
    super.margin,
    super.clipBehavior,
    super.semanticContainer = true,
    this.header,
    this.footer,
    this.padding = const EdgeInsets.all(16),
    this.showBorder = false,
    this.borderColor,
    this.borderWidth = 1.0,
    this.animated = true,
    this.animationDuration = const Duration(milliseconds: 200),
  });

  @override
  Widget build(BuildContext context) {
    Widget cardChild = child;

    // Add padding if specified
    if (padding != null) {
      cardChild = Padding(padding: padding!, child: cardChild);
    }

    // Build the card content with header and footer
    if (header != null || footer != null) {
      cardChild = Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        mainAxisSize: MainAxisSize.min,
        children: [
          if (header != null) ...[
            DefaultTextStyle(
              style: Theme.of(context).textTheme.titleMedium!,
              child: header!,
            ),
            const SizedBox(height: 12),
          ],
          if (child != null) Expanded(flex: 0, child: child!),
          if (footer != null) ...[
            const SizedBox(height: 12),
            DefaultTextStyle(
              style: Theme.of(context).textTheme.bodySmall!,
              child: footer!,
            ),
          ],
        ],
      );
    }

    Widget result = Card(
      color: color,
      shadowColor: shadowColor,
      surfaceTintColor: surfaceTintColor,
      elevation: elevation,
      shape: showBorder 
          ? RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(12),
              side: BorderSide(
                color: borderColor ?? Theme.of(context).dividerColor,
                width: borderWidth,
              ),
            )
          : shape,
      borderOnForeground: borderOnForeground,
      margin: margin,
      clipBehavior: clipBehavior,
      semanticContainer: semanticContainer,
      child: cardChild,
    );

    // Add animation if enabled
    if (animated) {
      result = AnimatedContainer(
        duration: animationDuration,
        child: result,
      );
    }

    return result;
  }
}

// Enhanced ListTile with additional customization
class EnhancedListTile extends ListTile {
  final Color? backgroundColor;
  final Color? hoverColor;
  final BorderRadius? borderRadius;
  final EdgeInsets? margin;
  final bool showDivider;
  final Widget? badge;

  const EnhancedListTile({
    super.key,
    super.leading,
    super.title,
    super.subtitle,
    super.trailing,
    super.isThreeLine = false,
    super.dense,
    super.visualDensity,
    super.shape,
    super.style,
    super.selectedColor,
    super.iconColor,
    super.textColor,
    super.titleTextStyle,
    super.subtitleTextStyle,
    super.leadingAndTrailingTextStyle,
    super.contentPadding,
    super.enabled = true,
    super.onTap,
    super.onLongPress,
    super.onFocusChange,
    super.mouseCursor,
    super.selected = false,
    super.focusColor,
    super.hoverColor,
    super.splashColor,
    super.focusNode,
    super.autofocus = false,
    super.tileColor,
    super.selectedTileColor,
    super.enableFeedback,
    super.horizontalTitleGap,
    super.minVerticalPadding,
    super.minLeadingWidth,
    super.titleAlignment,
    this.backgroundColor,
    this.hoverColor,
    this.borderRadius,
    this.margin,
    this.showDivider = false,
    this.badge,
  });

  @override
  Widget build(BuildContext context) {
    Widget listTile = ListTile(
      leading: leading,
      title: title,
      subtitle: subtitle,
      trailing: badge != null 
          ? Row(
              mainAxisSize: MainAxisSize.min,
              children: [
                if (trailing != null) trailing!,
                const SizedBox(width: 8),
                badge!,
              ],
            )
          : trailing,
      isThreeLine: isThreeLine,
      dense: dense,
      visualDensity: visualDensity,
      shape: shape,
      style: style,
      selectedColor: selectedColor,
      iconColor: iconColor,
      textColor: textColor,
      titleTextStyle: titleTextStyle,
      subtitleTextStyle: subtitleTextStyle,
      leadingAndTrailingTextStyle: leadingAndTrailingTextStyle,
      contentPadding: contentPadding,
      enabled: enabled,
      onTap: onTap,
      onLongPress: onLongPress,
      onFocusChange: onFocusChange,
      mouseCursor: mouseCursor,
      selected: selected,
      focusColor: focusColor,
      hoverColor: hoverColor ?? this.hoverColor,
      splashColor: splashColor,
      focusNode: focusNode,
      autofocus: autofocus,
      tileColor: tileColor ?? backgroundColor,
      selectedTileColor: selectedTileColor,
      enableFeedback: enableFeedback,
      horizontalTitleGap: horizontalTitleGap,
      minVerticalPadding: minVerticalPadding,
      minLeadingWidth: minLeadingWidth,
      titleAlignment: titleAlignment,
    );

    // Add border radius if specified
    if (borderRadius != null) {
      listTile = ClipRRect(
        borderRadius: borderRadius!,
        child: listTile,
      );
    }

    // Add margin if specified
    if (margin != null) {
      listTile = Container(
        margin: margin,
        child: listTile,
      );
    }

    // Add divider if enabled
    if (showDivider) {
      listTile = Column(
        children: [
          listTile,
          const Divider(height: 1),
        ],
      );
    }

    return listTile;
  }
}

// Enhanced AppBar with gradient and additional features
class GradientAppBar extends StatelessWidget implements PreferredSizeWidget {
  final Widget? title;
  final List<Widget>? actions;
  final Widget? leading;
  final bool automaticallyImplyLeading;
  final Color? backgroundColor;
  final Gradient? gradient;
  final double elevation;
  final Color? shadowColor;
  final ShapeBorder? shape;
  final double? titleSpacing;
  final double toolbarHeight;
  final bool centerTitle;
  final TextStyle? titleTextStyle;

  const GradientAppBar({
    super.key,
    this.title,
    this.actions,
    this.leading,
    this.automaticallyImplyLeading = true,
    this.backgroundColor,
    this.gradient,
    this.elevation = 4.0,
    this.shadowColor,
    this.shape,
    this.titleSpacing,
    this.toolbarHeight = kToolbarHeight,
    this.centerTitle = false,
    this.titleTextStyle,
  });

  @override
  Widget build(BuildContext context) {
    return Container(
      height: preferredSize.height,
      decoration: BoxDecoration(
        gradient: gradient,
        color: backgroundColor,
        boxShadow: elevation > 0
            ? [
                BoxShadow(
                  color: shadowColor ?? Colors.black26,
                  offset: Offset(0, elevation),
                  blurRadius: elevation * 2,
                ),
              ]
            : null,
      ),
      child: AppBar(
        title: title,
        actions: actions,
        leading: leading,
        automaticallyImplyLeading: automaticallyImplyLeading,
        backgroundColor: Colors.transparent,
        elevation: 0,
        shadowColor: Colors.transparent,
        shape: shape,
        titleSpacing: titleSpacing,
        toolbarHeight: toolbarHeight,
        centerTitle: centerTitle,
        titleTextStyle: titleTextStyle,
      ),
    );
  }

  @override
  Size get preferredSize => Size.fromHeight(toolbarHeight);
}

class EnhancedWidgetsPage extends StatefulWidget {
  const EnhancedWidgetsPage({super.key});

  @override
  State<EnhancedWidgetsPage> createState() => _EnhancedWidgetsPageState();
}

class _EnhancedWidgetsPageState extends State<EnhancedWidgetsPage> {
  int _selectedIndex = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: GradientAppBar(
        title: const Text(
          'Enhanced Widgets',
          style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold),
        ),
        gradient: LinearGradient(
          colors: [
            Theme.of(context).primaryColor,
            Theme.of(context).primaryColor.withOpacity(0.7),
          ],
        ),
        centerTitle: true,
        elevation: 8.0,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Enhanced Cards',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            EnhancedCard(
              header: const Row(
                children: [
                  Icon(Icons.notifications, color: Colors.orange),
                  SizedBox(width: 8),
                  Text('Notification Settings'),
                ],
              ),
              footer: const Text(
                'Last updated: 2 hours ago',
                style: TextStyle(color: Colors.grey),
              ),
              showBorder: true,
              borderColor: Colors.orange,
              elevation: 2,
              child: const Text(
                'Configure how you receive notifications from the app. '
                'You can customize notification types, frequency, and delivery methods.',
              ),
            ),
            const SizedBox(height: 16),
            EnhancedCard(
              header: const Row(
                children: [
                  Icon(Icons.security, color: Colors.green),
                  SizedBox(width: 8),
                  Text('Security Features'),
                ],
              ),
              showBorder: true,
              borderColor: Colors.green,
              backgroundColor: Colors.green.shade50,
              child: Column(
                children: [
                  const Text(
                    'Your account is protected with advanced security features.',
                  ),
                  const SizedBox(height: 12),
                  Row(
                    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                    children: [
                      Column(
                        children: [
                          Icon(Icons.lock, color: Colors.green.shade700),
                          const SizedBox(height: 4),
                          const Text('2FA', style: TextStyle(fontSize: 12)),
                        ],
                      ),
                      Column(
                        children: [
                          Icon(Icons.verified, color: Colors.green.shade700),
                          const SizedBox(height: 4),
                          const Text('Verified', style: TextStyle(fontSize: 12)),
                        ],
                      ),
                      Column(
                        children: [
                          Icon(Icons.shield, color: Colors.green.shade700),
                          const SizedBox(height: 4),
                          const Text('Protected', style: TextStyle(fontSize: 12)),
                        ],
                      ),
                    ],
                  ),
                ],
              ),
            ),
            const SizedBox(height: 32),
            Text(
              'Enhanced List Tiles',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            Card(
              child: Column(
                children: [
                  EnhancedListTile(
                    leading: const CircleAvatar(
                      backgroundColor: Colors.blue,
                      child: Icon(Icons.person, color: Colors.white),
                    ),
                    title: const Text('John Doe'),
                    subtitle: const Text('Software Developer'),
                    trailing: const Icon(Icons.arrow_forward_ios),
                    badge: Container(
                      padding: const EdgeInsets.symmetric(
                        horizontal: 8, 
                        vertical: 4,
                      ),
                      decoration: BoxDecoration(
                        color: Colors.green,
                        borderRadius: BorderRadius.circular(12),
                      ),
                      child: const Text(
                        'Online',
                        style: TextStyle(
                          color: Colors.white,
                          fontSize: 10,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                    onTap: () => print('John Doe tapped'),
                    showDivider: true,
                  ),
                  EnhancedListTile(
                    leading: const CircleAvatar(
                      backgroundColor: Colors.purple,
                      child: Icon(Icons.person, color: Colors.white),
                    ),
                    title: const Text('Jane Smith'),
                    subtitle: const Text('UI/UX Designer'),
                    trailing: const Icon(Icons.arrow_forward_ios),
                    badge: Container(
                      padding: const EdgeInsets.symmetric(
                        horizontal: 8, 
                        vertical: 4,
                      ),
                      decoration: BoxDecoration(
                        color: Colors.orange,
                        borderRadius: BorderRadius.circular(12),
                      ),
                      child: const Text(
                        'Away',
                        style: TextStyle(
                          color: Colors.white,
                          fontSize: 10,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                    backgroundColor: Colors.purple.shade50,
                    borderRadius: BorderRadius.circular(8),
                    onTap: () => print('Jane Smith tapped'),
                    showDivider: true,
                  ),
                  EnhancedListTile(
                    leading: const CircleAvatar(
                      backgroundColor: Colors.teal,
                      child: Icon(Icons.person, color: Colors.white),
                    ),
                    title: const Text('Mike Johnson'),
                    subtitle: const Text('Project Manager'),
                    trailing: const Icon(Icons.arrow_forward_ios),
                    badge: Container(
                      width: 12,
                      height: 12,
                      decoration: const BoxDecoration(
                        color: Colors.red,
                        shape: BoxShape.circle,
                      ),
                    ),
                    onTap: () => print('Mike Johnson tapped'),
                  ),
                ],
              ),
            ),
            const SizedBox(height: 32),
            Text(
              'Feature Comparison',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: Card(
                    child: Padding(
                      padding: const EdgeInsets.all(16),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          const Text(
                            'Standard Card',
                            style: TextStyle(fontWeight: FontWeight.bold),
                          ),
                          const SizedBox(height: 8),
                          const Text(
                            '• Basic elevation\n'
                            '• Simple content\n'
                            '• Limited styling',
                            style: TextStyle(fontSize: 12),
                          ),
                        ],
                      ),
                    ),
                  ),
                ),
                const SizedBox(width: 16),
                Expanded(
                  child: EnhancedCard(
                    header: const Text(
                      'Enhanced Card',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    showBorder: true,
                    borderColor: Theme.of(context).primaryColor,
                    child: const Text(
                      '• Header & footer support\n'
                      '• Custom borders\n'
                      '• Advanced styling\n'
                      '• Animation support',
                      style: TextStyle(fontSize: 12),
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
```

This example demonstrates how to extend built-in Flutter widgets like Card,  
ListTile, and AppBar with additional features while maintaining their core  
functionality. The enhanced widgets add headers, footers, borders, badges,  
gradients, and other customization options beyond the standard widgets.  

## Custom Navigation Widget

Building a reusable bottom navigation bar with advanced features.  

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
      title: 'Custom Navigation',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        useMaterial3: true,
      ),
      home: const NavigationDemoPage(),
    );
  }
}

class CustomBottomNavItem {
  final IconData icon;
  final IconData? activeIcon;
  final String label;
  final Color? color;
  final String? badge;

  const CustomBottomNavItem({
    required this.icon,
    required this.label,
    this.activeIcon,
    this.color,
    this.badge,
  });
}

class CustomBottomNavigationBar extends StatefulWidget {
  final List<CustomBottomNavItem> items;
  final int currentIndex;
  final ValueChanged<int>? onTap;
  final Color? backgroundColor;
  final Color? selectedItemColor;
  final Color? unselectedItemColor;
  final double elevation;
  final bool showLabels;
  final bool animateSelection;
  final Duration animationDuration;

  const CustomBottomNavigationBar({
    super.key,
    required this.items,
    required this.currentIndex,
    this.onTap,
    this.backgroundColor,
    this.selectedItemColor,
    this.unselectedItemColor,
    this.elevation = 8.0,
    this.showLabels = true,
    this.animateSelection = true,
    this.animationDuration = const Duration(milliseconds: 300),
  });

  @override
  State<CustomBottomNavigationBar> createState() => 
      _CustomBottomNavigationBarState();
}

class _CustomBottomNavigationBarState extends State<CustomBottomNavigationBar>
    with TickerProviderStateMixin {
  late List<AnimationController> _animationControllers;
  late List<Animation<double>> _scaleAnimations;
  late List<Animation<double>> _rotateAnimations;

  @override
  void initState() {
    super.initState();
    _initializeAnimations();
  }

  void _initializeAnimations() {
    _animationControllers = List.generate(
      widget.items.length,
      (index) => AnimationController(
        duration: widget.animationDuration,
        vsync: this,
      ),
    );

    _scaleAnimations = _animationControllers.map((controller) {
      return Tween<double>(begin: 1.0, end: 1.2).animate(
        CurvedAnimation(parent: controller, curve: Curves.elasticOut),
      );
    }).toList();

    _rotateAnimations = _animationControllers.map((controller) {
      return Tween<double>(begin: 0.0, end: 0.1).animate(
        CurvedAnimation(parent: controller, curve: Curves.easeInOut),
      );
    }).toList();

    // Start animation for current index
    if (widget.currentIndex < _animationControllers.length) {
      _animationControllers[widget.currentIndex].forward();
    }
  }

  @override
  void didUpdateWidget(CustomBottomNavigationBar oldWidget) {
    super.didUpdateWidget(oldWidget);
    if (oldWidget.currentIndex != widget.currentIndex) {
      // Reset previous animation
      if (oldWidget.currentIndex < _animationControllers.length) {
        _animationControllers[oldWidget.currentIndex].reverse();
      }
      // Start new animation
      if (widget.currentIndex < _animationControllers.length) {
        _animationControllers[widget.currentIndex].forward();
      }
    }
  }

  @override
  void dispose() {
    for (var controller in _animationControllers) {
      controller.dispose();
    }
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final selectedColor = widget.selectedItemColor ?? theme.primaryColor;
    final unselectedColor = widget.unselectedItemColor ?? Colors.grey;

    return Material(
      elevation: widget.elevation,
      color: widget.backgroundColor ?? theme.scaffoldBackgroundColor,
      child: Container(
        height: 80,
        padding: const EdgeInsets.symmetric(vertical: 8),
        child: Row(
          children: widget.items.asMap().entries.map((entry) {
            final index = entry.key;
            final item = entry.value;
            final isSelected = index == widget.currentIndex;

            return Expanded(
              child: GestureDetector(
                onTap: () {
                  widget.onTap?.call(index);
                },
                child: AnimatedBuilder(
                  animation: _animationControllers[index],
                  builder: (context, child) {
                    return Transform.scale(
                      scale: widget.animateSelection 
                          ? _scaleAnimations[index].value
                          : 1.0,
                      child: Transform.rotate(
                        angle: widget.animateSelection 
                            ? _rotateAnimations[index].value
                            : 0.0,
                        child: Container(
                          decoration: BoxDecoration(
                            color: isSelected 
                                ? selectedColor.withOpacity(0.1)
                                : Colors.transparent,
                            borderRadius: BorderRadius.circular(12),
                          ),
                          padding: const EdgeInsets.symmetric(
                            horizontal: 8, 
                            vertical: 4,
                          ),
                          child: Column(
                            mainAxisAlignment: MainAxisAlignment.center,
                            mainAxisSize: MainAxisSize.min,
                            children: [
                              Stack(
                                children: [
                                  Icon(
                                    isSelected 
                                        ? (item.activeIcon ?? item.icon)
                                        : item.icon,
                                    color: isSelected 
                                        ? (item.color ?? selectedColor)
                                        : unselectedColor,
                                    size: 28,
                                  ),
                                  if (item.badge != null)
                                    Positioned(
                                      right: -2,
                                      top: -2,
                                      child: Container(
                                        padding: const EdgeInsets.all(2),
                                        constraints: const BoxConstraints(
                                          minWidth: 16,
                                          minHeight: 16,
                                        ),
                                        decoration: BoxDecoration(
                                          color: Colors.red,
                                          borderRadius: BorderRadius.circular(8),
                                        ),
                                        child: Text(
                                          item.badge!,
                                          style: const TextStyle(
                                            color: Colors.white,
                                            fontSize: 10,
                                            fontWeight: FontWeight.bold,
                                          ),
                                          textAlign: TextAlign.center,
                                        ),
                                      ),
                                    ),
                                ],
                              ),
                              if (widget.showLabels) ...[
                                const SizedBox(height: 4),
                                Text(
                                  item.label,
                                  style: TextStyle(
                                    fontSize: 12,
                                    color: isSelected 
                                        ? (item.color ?? selectedColor)
                                        : unselectedColor,
                                    fontWeight: isSelected 
                                        ? FontWeight.w600 
                                        : FontWeight.normal,
                                  ),
                                  maxLines: 1,
                                  overflow: TextOverflow.ellipsis,
                                ),
                              ],
                            ],
                          ),
                        ),
                      ),
                    );
                  },
                ),
              ),
            );
          }).toList(),
        ),
      ),
    );
  }
}

class NavigationDemoPage extends StatefulWidget {
  const NavigationDemoPage({super.key});

  @override
  State<NavigationDemoPage> createState() => _NavigationDemoPageState();
}

class _NavigationDemoPageState extends State<NavigationDemoPage> {
  int _currentIndex = 0;
  
  final List<CustomBottomNavItem> _navItems = [
    const CustomBottomNavItem(
      icon: Icons.home_outlined,
      activeIcon: Icons.home,
      label: 'Home',
      color: Colors.blue,
    ),
    const CustomBottomNavItem(
      icon: Icons.search_outlined,
      activeIcon: Icons.search,
      label: 'Search',
      color: Colors.green,
    ),
    const CustomBottomNavItem(
      icon: Icons.favorite_outline,
      activeIcon: Icons.favorite,
      label: 'Favorites',
      color: Colors.red,
      badge: '5',
    ),
    const CustomBottomNavItem(
      icon: Icons.notifications_outlined,
      activeIcon: Icons.notifications,
      label: 'Notifications',
      color: Colors.orange,
      badge: '12',
    ),
    const CustomBottomNavItem(
      icon: Icons.person_outline,
      activeIcon: Icons.person,
      label: 'Profile',
      color: Colors.purple,
    ),
  ];

  final List<Widget> _pages = [
    const _HomePage(),
    const _SearchPage(),
    const _FavoritesPage(),
    const _NotificationsPage(),
    const _ProfilePage(),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('${_navItems[_currentIndex].label} Page'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: IndexedStack(
        index: _currentIndex,
        children: _pages,
      ),
      bottomNavigationBar: CustomBottomNavigationBar(
        items: _navItems,
        currentIndex: _currentIndex,
        onTap: (index) {
          setState(() {
            _currentIndex = index;
          });
        },
        selectedItemColor: Colors.blue,
        animateSelection: true,
        elevation: 10,
      ),
    );
  }
}

class _HomePage extends StatelessWidget {
  const _HomePage();

  @override
  Widget build(BuildContext context) {
    return const Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(Icons.home, size: 100, color: Colors.blue),
          SizedBox(height: 16),
          Text(
            'Home Page',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
          SizedBox(height: 8),
          Text('Welcome to your home dashboard!'),
        ],
      ),
    );
  }
}

class _SearchPage extends StatelessWidget {
  const _SearchPage();

  @override
  Widget build(BuildContext context) {
    return const Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(Icons.search, size: 100, color: Colors.green),
          SizedBox(height: 16),
          Text(
            'Search Page',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
          SizedBox(height: 8),
          Text('Find what you\'re looking for!'),
        ],
      ),
    );
  }
}

class _FavoritesPage extends StatelessWidget {
  const _FavoritesPage();

  @override
  Widget build(BuildContext context) {
    return const Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(Icons.favorite, size: 100, color: Colors.red),
          SizedBox(height: 16),
          Text(
            'Favorites Page',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
          SizedBox(height: 8),
          Text('Your favorite items (5)'),
        ],
      ),
    );
  }
}

class _NotificationsPage extends StatelessWidget {
  const _NotificationsPage();

  @override
  Widget build(BuildContext context) {
    return const Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(Icons.notifications, size: 100, color: Colors.orange),
          SizedBox(height: 16),
          Text(
            'Notifications Page',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
          SizedBox(height: 8),
          Text('You have 12 new notifications'),
        ],
      ),
    );
  }
}

class _ProfilePage extends StatelessWidget {
  const _ProfilePage();

  @override
  Widget build(BuildContext context) {
    return const Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(Icons.person, size: 100, color: Colors.purple),
          SizedBox(height: 16),
          Text(
            'Profile Page',
            style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
          ),
          SizedBox(height: 8),
          Text('Manage your account settings'),
        ],
      ),
    );
  }
}
```

This custom navigation widget features animated selections, badge support,  
custom colors per item, smooth transitions, and configurable styling options.  
It demonstrates advanced animation techniques and provides a more engaging  
user experience compared to standard bottom navigation bars.  

## Composite Custom Widget

Creating complex widgets that combine multiple components and behaviors.  

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
      title: 'Composite Widgets',
      theme: ThemeData(
        primarySwatch: Colors.cyan,
        useMaterial3: true,
      ),
      home: const ProductCardDemoPage(),
    );
  }
}

class ProductCard extends StatefulWidget {
  final String name;
  final String description;
  final double price;
  final double? originalPrice;
  final String imageUrl;
  final double rating;
  final int reviewCount;
  final List<String> tags;
  final bool isWishlisted;
  final bool isInCart;
  final VoidCallback? onWishlistToggle;
  final VoidCallback? onAddToCart;
  final VoidCallback? onTap;
  final Color? accentColor;

  const ProductCard({
    super.key,
    required this.name,
    required this.description,
    required this.price,
    this.originalPrice,
    required this.imageUrl,
    required this.rating,
    required this.reviewCount,
    this.tags = const [],
    this.isWishlisted = false,
    this.isInCart = false,
    this.onWishlistToggle,
    this.onAddToCart,
    this.onTap,
    this.accentColor,
  });

  @override
  State<ProductCard> createState() => _ProductCardState();
}

class _ProductCardState extends State<ProductCard>
    with TickerProviderStateMixin {
  late AnimationController _wishlistController;
  late AnimationController _cartController;
  late AnimationController _scaleController;
  late Animation<double> _wishlistScaleAnimation;
  late Animation<double> _cartBounceAnimation;
  late Animation<double> _cardScaleAnimation;

  @override
  void initState() {
    super.initState();
    
    _wishlistController = AnimationController(
      duration: const Duration(milliseconds: 300),
      vsync: this,
    );
    
    _cartController = AnimationController(
      duration: const Duration(milliseconds: 400),
      vsync: this,
    );
    
    _scaleController = AnimationController(
      duration: const Duration(milliseconds: 150),
      vsync: this,
    );

    _wishlistScaleAnimation = Tween<double>(
      begin: 1.0,
      end: 1.3,
    ).animate(CurvedAnimation(
      parent: _wishlistController,
      curve: Curves.elasticOut,
    ));

    _cartBounceAnimation = Tween<double>(
      begin: 1.0,
      end: 1.2,
    ).animate(CurvedAnimation(
      parent: _cartController,
      curve: Curves.elasticOut,
    ));

    _cardScaleAnimation = Tween<double>(
      begin: 1.0,
      end: 0.98,
    ).animate(CurvedAnimation(
      parent: _scaleController,
      curve: Curves.easeInOut,
    ));
  }

  @override
  void dispose() {
    _wishlistController.dispose();
    _cartController.dispose();
    _scaleController.dispose();
    super.dispose();
  }

  void _handleWishlistTap() {
    _wishlistController.forward().then((_) {
      _wishlistController.reverse();
    });
    widget.onWishlistToggle?.call();
  }

  void _handleAddToCartTap() {
    _cartController.forward().then((_) {
      _cartController.reverse();
    });
    widget.onAddToCart?.call();
  }

  Widget _buildRatingStars() {
    return Row(
      mainAxisSize: MainAxisSize.min,
      children: List.generate(5, (index) {
        final isFilled = index < widget.rating.floor();
        final isHalf = index == widget.rating.floor() && 
                       widget.rating % 1 >= 0.5;
        
        return Icon(
          isFilled 
              ? Icons.star
              : isHalf
                  ? Icons.star_half
                  : Icons.star_border,
          size: 16,
          color: Colors.amber,
        );
      }),
    );
  }

  Widget _buildPriceDisplay() {
    final hasDiscount = widget.originalPrice != null && 
                       widget.originalPrice! > widget.price;

    return Row(
      children: [
        Text(
          '\$${widget.price.toStringAsFixed(2)}',
          style: TextStyle(
            fontSize: 18,
            fontWeight: FontWeight.bold,
            color: hasDiscount 
                ? (widget.accentColor ?? Colors.green) 
                : null,
          ),
        ),
        if (hasDiscount) ...[
          const SizedBox(width: 8),
          Text(
            '\$${widget.originalPrice!.toStringAsFixed(2)}',
            style: const TextStyle(
              fontSize: 14,
              decoration: TextDecoration.lineThrough,
              color: Colors.grey,
            ),
          ),
          const SizedBox(width: 4),
          Container(
            padding: const EdgeInsets.symmetric(horizontal: 6, vertical: 2),
            decoration: BoxDecoration(
              color: widget.accentColor ?? Colors.red,
              borderRadius: BorderRadius.circular(4),
            ),
            child: Text(
              '${(((widget.originalPrice! - widget.price) / widget.originalPrice!) * 100).round()}% OFF',
              style: const TextStyle(
                color: Colors.white,
                fontSize: 10,
                fontWeight: FontWeight.bold,
              ),
            ),
          ),
        ],
      ],
    );
  }

  Widget _buildTags() {
    if (widget.tags.isEmpty) return const SizedBox.shrink();
    
    return Wrap(
      spacing: 4,
      runSpacing: 4,
      children: widget.tags.take(3).map((tag) {
        return Container(
          padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
          decoration: BoxDecoration(
            color: (widget.accentColor ?? Theme.of(context).primaryColor)
                .withOpacity(0.1),
            borderRadius: BorderRadius.circular(12),
            border: Border.all(
              color: (widget.accentColor ?? Theme.of(context).primaryColor)
                  .withOpacity(0.3),
            ),
          ),
          child: Text(
            tag,
            style: TextStyle(
              fontSize: 10,
              color: widget.accentColor ?? Theme.of(context).primaryColor,
              fontWeight: FontWeight.w500,
            ),
          ),
        );
      }).toList(),
    );
  }

  @override
  Widget build(BuildContext context) {
    return ScaleTransition(
      scale: _cardScaleAnimation,
      child: Card(
        elevation: 4,
        clipBehavior: Clip.antiAlias,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(16),
        ),
        child: InkWell(
          onTap: widget.onTap,
          onTapDown: (_) => _scaleController.forward(),
          onTapUp: (_) => _scaleController.reverse(),
          onTapCancel: () => _scaleController.reverse(),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              // Image and wishlist button
              Stack(
                children: [
                  AspectRatio(
                    aspectRatio: 16 / 10,
                    child: Container(
                      decoration: BoxDecoration(
                        gradient: LinearGradient(
                          colors: [
                            Colors.grey.shade200,
                            Colors.grey.shade100,
                          ],
                        ),
                      ),
                      child: widget.imageUrl.startsWith('http')
                          ? Image.network(
                              widget.imageUrl,
                              fit: BoxFit.cover,
                              errorBuilder: (context, error, stackTrace) {
                                return const Icon(
                                  Icons.image_not_supported,
                                  size: 50,
                                  color: Colors.grey,
                                );
                              },
                            )
                          : Center(
                              child: Icon(
                                Icons.image,
                                size: 50,
                                color: Colors.grey.shade400,
                              ),
                            ),
                    ),
                  ),
                  Positioned(
                    top: 8,
                    right: 8,
                    child: ScaleTransition(
                      scale: _wishlistScaleAnimation,
                      child: Container(
                        decoration: BoxDecoration(
                          color: Colors.white,
                          shape: BoxShape.circle,
                          boxShadow: [
                            BoxShadow(
                              color: Colors.black.withOpacity(0.1),
                              blurRadius: 4,
                              offset: const Offset(0, 2),
                            ),
                          ],
                        ),
                        child: IconButton(
                          icon: Icon(
                            widget.isWishlisted
                                ? Icons.favorite
                                : Icons.favorite_border,
                            color: widget.isWishlisted
                                ? Colors.red
                                : Colors.grey.shade600,
                          ),
                          onPressed: _handleWishlistTap,
                          splashRadius: 20,
                        ),
                      ),
                    ),
                  ),
                  if (widget.tags.isNotEmpty)
                    Positioned(
                      top: 8,
                      left: 8,
                      child: Container(
                        padding: const EdgeInsets.symmetric(
                          horizontal: 8, 
                          vertical: 4,
                        ),
                        decoration: BoxDecoration(
                          color: widget.accentColor ?? Colors.blue,
                          borderRadius: BorderRadius.circular(8),
                        ),
                        child: Text(
                          widget.tags.first,
                          style: const TextStyle(
                            color: Colors.white,
                            fontSize: 10,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ),
                    ),
                ],
              ),
              
              // Content
              Padding(
                padding: const EdgeInsets.all(12),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    // Name and rating
                    Row(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Expanded(
                          child: Text(
                            widget.name,
                            style: const TextStyle(
                              fontSize: 16,
                              fontWeight: FontWeight.bold,
                            ),
                            maxLines: 2,
                            overflow: TextOverflow.ellipsis,
                          ),
                        ),
                        const SizedBox(width: 8),
                        Column(
                          crossAxisAlignment: CrossAxisAlignment.end,
                          children: [
                            _buildRatingStars(),
                            const SizedBox(height: 2),
                            Text(
                              '(${widget.reviewCount})',
                              style: TextStyle(
                                fontSize: 12,
                                color: Colors.grey.shade600,
                              ),
                            ),
                          ],
                        ),
                      ],
                    ),
                    
                    const SizedBox(height: 8),
                    
                    // Description
                    Text(
                      widget.description,
                      style: TextStyle(
                        fontSize: 14,
                        color: Colors.grey.shade700,
                        height: 1.3,
                      ),
                      maxLines: 2,
                      overflow: TextOverflow.ellipsis,
                    ),
                    
                    const SizedBox(height: 12),
                    
                    // Tags
                    _buildTags(),
                    
                    if (widget.tags.isNotEmpty) const SizedBox(height: 12),
                    
                    // Price and cart button
                    Row(
                      children: [
                        Expanded(
                          child: _buildPriceDisplay(),
                        ),
                        ScaleTransition(
                          scale: _cartBounceAnimation,
                          child: ElevatedButton.icon(
                            onPressed: widget.isInCart ? null : _handleAddToCartTap,
                            icon: Icon(
                              widget.isInCart 
                                  ? Icons.check 
                                  : Icons.shopping_cart_outlined,
                              size: 18,
                            ),
                            label: Text(
                              widget.isInCart ? 'Added' : 'Add',
                              style: const TextStyle(fontSize: 12),
                            ),
                            style: ElevatedButton.styleFrom(
                              backgroundColor: widget.isInCart 
                                  ? Colors.green 
                                  : (widget.accentColor ?? Theme.of(context).primaryColor),
                              foregroundColor: Colors.white,
                              padding: const EdgeInsets.symmetric(
                                horizontal: 12, 
                                vertical: 8,
                              ),
                              shape: RoundedRectangleBorder(
                                borderRadius: BorderRadius.circular(8),
                              ),
                            ),
                          ),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

class ProductCardDemoPage extends StatefulWidget {
  const ProductCardDemoPage({super.key});

  @override
  State<ProductCardDemoPage> createState() => _ProductCardDemoPageState();
}

class _ProductCardDemoPageState extends State<ProductCardDemoPage> {
  final Set<int> _wishlistedItems = {};
  final Set<int> _cartItems = {};

  final List<Map<String, dynamic>> _products = [
    {
      'name': 'Wireless Headphones Pro',
      'description': 'Premium noise-cancelling headphones with 30-hour battery life and superior sound quality.',
      'price': 199.99,
      'originalPrice': 299.99,
      'imageUrl': 'https://example.com/headphones.jpg',
      'rating': 4.5,
      'reviewCount': 1247,
      'tags': ['Best Seller', 'Premium', 'Wireless'],
      'accentColor': Colors.purple,
    },
    {
      'name': 'Smart Fitness Watch',
      'description': 'Advanced fitness tracking with heart rate monitor, GPS, and 7-day battery life.',
      'price': 299.00,
      'imageUrl': 'https://example.com/watch.jpg',
      'rating': 4.2,
      'reviewCount': 892,
      'tags': ['Fitness', 'Smart', 'Health'],
      'accentColor': Colors.blue,
    },
    {
      'name': 'Portable Bluetooth Speaker',
      'description': 'Compact waterproof speaker with 360-degree sound and 12-hour playtime.',
      'price': 79.99,
      'originalPrice': 99.99,
      'imageUrl': 'https://example.com/speaker.jpg',
      'rating': 4.7,
      'reviewCount': 2156,
      'tags': ['Waterproof', 'Portable', 'Bluetooth'],
      'accentColor': Colors.orange,
    },
    {
      'name': 'Gaming Mechanical Keyboard',
      'description': 'RGB backlit mechanical keyboard with customizable keys and macro support.',
      'price': 149.00,
      'imageUrl': 'https://example.com/keyboard.jpg',
      'rating': 4.4,
      'reviewCount': 567,
      'tags': ['Gaming', 'RGB', 'Mechanical'],
      'accentColor': Colors.red,
    },
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Product Cards'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        actions: [
          Stack(
            children: [
              IconButton(
                icon: const Icon(Icons.favorite),
                onPressed: () {
                  ScaffoldMessenger.of(context).showSnackBar(
                    SnackBar(
                      content: Text('${_wishlistedItems.length} items in wishlist'),
                    ),
                  );
                },
              ),
              if (_wishlistedItems.isNotEmpty)
                Positioned(
                  right: 8,
                  top: 8,
                  child: Container(
                    padding: const EdgeInsets.all(2),
                    decoration: const BoxDecoration(
                      color: Colors.red,
                      shape: BoxShape.circle,
                    ),
                    constraints: const BoxConstraints(
                      minWidth: 16,
                      minHeight: 16,
                    ),
                    child: Text(
                      '${_wishlistedItems.length}',
                      style: const TextStyle(
                        color: Colors.white,
                        fontSize: 10,
                      ),
                      textAlign: TextAlign.center,
                    ),
                  ),
                ),
            ],
          ),
          Stack(
            children: [
              IconButton(
                icon: const Icon(Icons.shopping_cart),
                onPressed: () {
                  ScaffoldMessenger.of(context).showSnackBar(
                    SnackBar(
                      content: Text('${_cartItems.length} items in cart'),
                    ),
                  );
                },
              ),
              if (_cartItems.isNotEmpty)
                Positioned(
                  right: 8,
                  top: 8,
                  child: Container(
                    padding: const EdgeInsets.all(2),
                    decoration: const BoxDecoration(
                      color: Colors.blue,
                      shape: BoxShape.circle,
                    ),
                    constraints: const BoxConstraints(
                      minWidth: 16,
                      minHeight: 16,
                    ),
                    child: Text(
                      '${_cartItems.length}',
                      style: const TextStyle(
                        color: Colors.white,
                        fontSize: 10,
                      ),
                      textAlign: TextAlign.center,
                    ),
                  ),
                ),
            ],
          ),
        ],
      ),
      body: GridView.builder(
        padding: const EdgeInsets.all(16),
        gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 2,
          crossAxisSpacing: 16,
          mainAxisSpacing: 16,
          childAspectRatio: 0.75,
        ),
        itemCount: _products.length,
        itemBuilder: (context, index) {
          final product = _products[index];
          return ProductCard(
            name: product['name'],
            description: product['description'],
            price: product['price'],
            originalPrice: product['originalPrice'],
            imageUrl: product['imageUrl'],
            rating: product['rating'],
            reviewCount: product['reviewCount'],
            tags: List<String>.from(product['tags']),
            accentColor: product['accentColor'],
            isWishlisted: _wishlistedItems.contains(index),
            isInCart: _cartItems.contains(index),
            onWishlistToggle: () {
              setState(() {
                if (_wishlistedItems.contains(index)) {
                  _wishlistedItems.remove(index);
                } else {
                  _wishlistedItems.add(index);
                }
              });
              
              ScaffoldMessenger.of(context).showSnackBar(
                SnackBar(
                  content: Text(
                    _wishlistedItems.contains(index)
                        ? 'Added to wishlist'
                        : 'Removed from wishlist',
                  ),
                  duration: const Duration(seconds: 1),
                ),
              );
            },
            onAddToCart: () {
              setState(() {
                _cartItems.add(index);
              });
              
              ScaffoldMessenger.of(context).showSnackBar(
                const SnackBar(
                  content: Text('Added to cart'),
                  duration: Duration(seconds: 1),
                ),
              );
            },
            onTap: () {
              showDialog(
                context: context,
                builder: (context) => AlertDialog(
                  title: Text(product['name']),
                  content: Column(
                    mainAxisSize: MainAxisSize.min,
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(product['description']),
                      const SizedBox(height: 8),
                      Text(
                        'Price: \$${product['price']}',
                        style: const TextStyle(fontWeight: FontWeight.bold),
                      ),
                      Text('Rating: ${product['rating']}/5'),
                      Text('Reviews: ${product['reviewCount']}'),
                    ],
                  ),
                  actions: [
                    TextButton(
                      onPressed: () => Navigator.pop(context),
                      child: const Text('Close'),
                    ),
                  ],
                ),
              );
            },
          );
        },
      ),
    );
  }
}
```

This composite widget demonstrates how to combine multiple features into a  
cohesive product card component. It includes image display, ratings, pricing,  
tags, wishlist functionality, cart management, animations, and responsive  
interactions, showcasing advanced custom widget development techniques.  

## Custom Theme Builder

Creating a comprehensive theme system with customizable design tokens.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class CustomTheme {
  // Color palette
  final Color primaryColor;
  final Color secondaryColor;
  final Color accentColor;
  final Color backgroundColor;
  final Color surfaceColor;
  final Color errorColor;
  final Color successColor;
  final Color warningColor;
  final Color infoColor;

  // Text colors
  final Color textPrimary;
  final Color textSecondary;
  final Color textDisabled;

  // Spacing scale
  final double spacingXs;
  final double spacingSm;
  final double spacingMd;
  final double spacingLg;
  final double spacingXl;
  final double spacingXxl;

  // Typography scale
  final double textXs;
  final double textSm;
  final double textMd;
  final double textLg;
  final double textXl;
  final double textXxl;

  // Border radius scale
  final double radiusSm;
  final double radiusMd;
  final double radiusLg;
  final double radiusXl;

  // Shadow elevations
  final double elevationSm;
  final double elevationMd;
  final double elevationLg;

  const CustomTheme({
    required this.primaryColor,
    required this.secondaryColor,
    required this.accentColor,
    required this.backgroundColor,
    required this.surfaceColor,
    required this.errorColor,
    required this.successColor,
    required this.warningColor,
    required this.infoColor,
    required this.textPrimary,
    required this.textSecondary,
    required this.textDisabled,
    this.spacingXs = 4,
    this.spacingSm = 8,
    this.spacingMd = 16,
    this.spacingLg = 24,
    this.spacingXl = 32,
    this.spacingXxl = 48,
    this.textXs = 12,
    this.textSm = 14,
    this.textMd = 16,
    this.textLg = 18,
    this.textXl = 24,
    this.textXxl = 32,
    this.radiusSm = 4,
    this.radiusMd = 8,
    this.radiusLg = 12,
    this.radiusXl = 16,
    this.elevationSm = 2,
    this.elevationMd = 4,
    this.elevationLg = 8,
  });

  // Predefined themes
  static const CustomTheme light = CustomTheme(
    primaryColor: Color(0xFF2563EB),
    secondaryColor: Color(0xFF64748B),
    accentColor: Color(0xFF06B6D4),
    backgroundColor: Color(0xFFF8FAFC),
    surfaceColor: Color(0xFFFFFFFF),
    errorColor: Color(0xFFEF4444),
    successColor: Color(0xFF10B981),
    warningColor: Color(0xFFF59E0B),
    infoColor: Color(0xFF3B82F6),
    textPrimary: Color(0xFF1E293B),
    textSecondary: Color(0xFF64748B),
    textDisabled: Color(0xFFCBD5E1),
  );

  static const CustomTheme dark = CustomTheme(
    primaryColor: Color(0xFF3B82F6),
    secondaryColor: Color(0xFF64748B),
    accentColor: Color(0xFF06B6D4),
    backgroundColor: Color(0xFF0F172A),
    surfaceColor: Color(0xFF1E293B),
    errorColor: Color(0xFFF87171),
    successColor: Color(0xFF34D399),
    warningColor: Color(0xFFFBBF24),
    infoColor: Color(0xFF60A5FA),
    textPrimary: Color(0xFFF1F5F9),
    textSecondary: Color(0xFF94A3B8),
    textDisabled: Color(0xFF475569),
  );

  static const CustomTheme nature = CustomTheme(
    primaryColor: Color(0xFF16A34A),
    secondaryColor: Color(0xFF15803D),
    accentColor: Color(0xFF84CC16),
    backgroundColor: Color(0xFFF0FDF4),
    surfaceColor: Color(0xFFFFFFFF),
    errorColor: Color(0xFFDC2626),
    successColor: Color(0xFF059669),
    warningColor: Color(0xFFD97706),
    infoColor: Color(0xFF0284C7),
    textPrimary: Color(0xFF14532D),
    textSecondary: Color(0xFF166534),
    textDisabled: Color(0xFF86EFAC),
  );

  // Convert to Material ThemeData
  ThemeData toMaterialTheme() {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme(
        brightness: backgroundColor.computeLuminance() > 0.5 
            ? Brightness.light 
            : Brightness.dark,
        primary: primaryColor,
        onPrimary: _getContrastColor(primaryColor),
        secondary: secondaryColor,
        onSecondary: _getContrastColor(secondaryColor),
        tertiary: accentColor,
        onTertiary: _getContrastColor(accentColor),
        error: errorColor,
        onError: _getContrastColor(errorColor),
        background: backgroundColor,
        onBackground: textPrimary,
        surface: surfaceColor,
        onSurface: textPrimary,
        surfaceVariant: _lighten(surfaceColor, 0.1),
        onSurfaceVariant: textSecondary,
        outline: textDisabled,
      ),
      textTheme: TextTheme(
        displayLarge: TextStyle(
          fontSize: textXxl,
          fontWeight: FontWeight.bold,
          color: textPrimary,
        ),
        headlineLarge: TextStyle(
          fontSize: textXl,
          fontWeight: FontWeight.bold,
          color: textPrimary,
        ),
        titleLarge: TextStyle(
          fontSize: textLg,
          fontWeight: FontWeight.w600,
          color: textPrimary,
        ),
        bodyLarge: TextStyle(
          fontSize: textMd,
          color: textPrimary,
        ),
        bodyMedium: TextStyle(
          fontSize: textSm,
          color: textSecondary,
        ),
        labelSmall: TextStyle(
          fontSize: textXs,
          color: textDisabled,
        ),
      ),
      cardTheme: CardTheme(
        color: surfaceColor,
        elevation: elevationSm,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(radiusMd),
        ),
        margin: EdgeInsets.all(spacingSm),
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          backgroundColor: primaryColor,
          foregroundColor: _getContrastColor(primaryColor),
          elevation: elevationSm,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(radiusSm),
          ),
          padding: EdgeInsets.symmetric(
            horizontal: spacingMd,
            vertical: spacingSm,
          ),
        ),
      ),
      outlinedButtonTheme: OutlinedButtonThemeData(
        style: OutlinedButton.styleFrom(
          foregroundColor: primaryColor,
          side: BorderSide(color: primaryColor),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(radiusSm),
          ),
          padding: EdgeInsets.symmetric(
            horizontal: spacingMd,
            vertical: spacingSm,
          ),
        ),
      ),
      inputDecorationTheme: InputDecorationTheme(
        border: OutlineInputBorder(
          borderRadius: BorderRadius.circular(radiusSm),
          borderSide: BorderSide(color: textDisabled),
        ),
        focusedBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(radiusSm),
          borderSide: BorderSide(color: primaryColor, width: 2),
        ),
        errorBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(radiusSm),
          borderSide: BorderSide(color: errorColor),
        ),
        contentPadding: EdgeInsets.all(spacingMd),
      ),
    );
  }

  Color _getContrastColor(Color color) {
    return color.computeLuminance() > 0.5 ? Colors.black : Colors.white;
  }

  Color _lighten(Color color, double amount) {
    final hsl = HSLColor.fromColor(color);
    return hsl.withLightness((hsl.lightness + amount).clamp(0.0, 1.0)).toColor();
  }
}

class ThemedWidget extends StatelessWidget {
  final CustomTheme customTheme;
  final String title;
  final String description;
  final IconData icon;

  const ThemedWidget({
    super.key,
    required this.customTheme,
    required this.title,
    required this.description,
    required this.icon,
  });

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(customTheme.spacingMd),
      decoration: BoxDecoration(
        color: customTheme.surfaceColor,
        borderRadius: BorderRadius.circular(customTheme.radiusLg),
        boxShadow: [
          BoxShadow(
            color: Colors.black.withOpacity(0.1),
            blurRadius: customTheme.elevationMd,
            offset: Offset(0, customTheme.elevationSm),
          ),
        ],
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Row(
            children: [
              Container(
                padding: EdgeInsets.all(customTheme.spacingSm),
                decoration: BoxDecoration(
                  color: customTheme.primaryColor.withOpacity(0.1),
                  borderRadius: BorderRadius.circular(customTheme.radiusSm),
                ),
                child: Icon(
                  icon,
                  color: customTheme.primaryColor,
                  size: customTheme.textXl,
                ),
              ),
              SizedBox(width: customTheme.spacingMd),
              Expanded(
                child: Text(
                  title,
                  style: TextStyle(
                    fontSize: customTheme.textLg,
                    fontWeight: FontWeight.bold,
                    color: customTheme.textPrimary,
                  ),
                ),
              ),
            ],
          ),
          SizedBox(height: customTheme.spacingSm),
          Text(
            description,
            style: TextStyle(
              fontSize: customTheme.textSm,
              color: customTheme.textSecondary,
              height: 1.4,
            ),
          ),
          SizedBox(height: customTheme.spacingMd),
          Row(
            children: [
              Expanded(
                child: Container(
                  padding: EdgeInsets.symmetric(
                    horizontal: customTheme.spacingMd,
                    vertical: customTheme.spacingSm,
                  ),
                  decoration: BoxDecoration(
                    color: customTheme.primaryColor,
                    borderRadius: BorderRadius.circular(customTheme.radiusSm),
                  ),
                  child: Text(
                    'Primary Action',
                    textAlign: TextAlign.center,
                    style: TextStyle(
                      color: customTheme._getContrastColor(customTheme.primaryColor),
                      fontWeight: FontWeight.w600,
                      fontSize: customTheme.textSm,
                    ),
                  ),
                ),
              ),
              SizedBox(width: customTheme.spacingSm),
              Expanded(
                child: Container(
                  padding: EdgeInsets.symmetric(
                    horizontal: customTheme.spacingMd,
                    vertical: customTheme.spacingSm,
                  ),
                  decoration: BoxDecoration(
                    border: Border.all(color: customTheme.primaryColor),
                    borderRadius: BorderRadius.circular(customTheme.radiusSm),
                  ),
                  child: Text(
                    'Secondary',
                    textAlign: TextAlign.center,
                    style: TextStyle(
                      color: customTheme.primaryColor,
                      fontWeight: FontWeight.w600,
                      fontSize: customTheme.textSm,
                    ),
                  ),
                ),
              ),
            ],
          ),
          SizedBox(height: customTheme.spacingMd),
          Row(
            children: [
              _buildStatusChip('Success', customTheme.successColor),
              SizedBox(width: customTheme.spacingXs),
              _buildStatusChip('Warning', customTheme.warningColor),
              SizedBox(width: customTheme.spacingXs),
              _buildStatusChip('Error', customTheme.errorColor),
            ],
          ),
        ],
      ),
    );
  }

  Widget _buildStatusChip(String label, Color color) {
    return Container(
      padding: EdgeInsets.symmetric(
        horizontal: customTheme.spacingSm,
        vertical: customTheme.spacingXs,
      ),
      decoration: BoxDecoration(
        color: color.withOpacity(0.1),
        borderRadius: BorderRadius.circular(customTheme.radiusSm),
        border: Border.all(color: color.withOpacity(0.3)),
      ),
      child: Text(
        label,
        style: TextStyle(
          color: color,
          fontSize: customTheme.textXs,
          fontWeight: FontWeight.w500,
        ),
      ),
    );
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Custom Theme Builder',
      theme: CustomTheme.light.toMaterialTheme(),
      home: const ThemeDemoPage(),
    );
  }
}

class ThemeDemoPage extends StatefulWidget {
  const ThemeDemoPage({super.key});

  @override
  State<ThemeDemoPage> createState() => _ThemeDemoPageState();
}

class _ThemeDemoPageState extends State<ThemeDemoPage> {
  CustomTheme _currentTheme = CustomTheme.light;
  
  final Map<String, CustomTheme> _themes = {
    'Light': CustomTheme.light,
    'Dark': CustomTheme.dark,
    'Nature': CustomTheme.nature,
  };

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: _currentTheme.toMaterialTheme(),
      home: Builder(
        builder: (context) => Scaffold(
          backgroundColor: _currentTheme.backgroundColor,
          appBar: AppBar(
            title: const Text('Custom Theme System'),
            backgroundColor: _currentTheme.primaryColor,
            foregroundColor: _currentTheme._getContrastColor(_currentTheme.primaryColor),
            actions: [
              PopupMenuButton<String>(
                icon: const Icon(Icons.palette),
                onSelected: (String themeName) {
                  setState(() {
                    _currentTheme = _themes[themeName]!;
                  });
                },
                itemBuilder: (context) => _themes.keys.map((String themeName) {
                  return PopupMenuItem<String>(
                    value: themeName,
                    child: Row(
                      children: [
                        Container(
                          width: 20,
                          height: 20,
                          decoration: BoxDecoration(
                            color: _themes[themeName]!.primaryColor,
                            shape: BoxShape.circle,
                          ),
                        ),
                        const SizedBox(width: 8),
                        Text(themeName),
                        if (_currentTheme == _themes[themeName])
                          const Padding(
                            padding: EdgeInsets.only(left: 8),
                            child: Icon(Icons.check, size: 16),
                          ),
                      ],
                    ),
                  );
                }).toList(),
              ),
            ],
          ),
          body: SingleChildScrollView(
            padding: EdgeInsets.all(_currentTheme.spacingMd),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Theme Showcase',
                  style: TextStyle(
                    fontSize: _currentTheme.textXl,
                    fontWeight: FontWeight.bold,
                    color: _currentTheme.textPrimary,
                  ),
                ),
                SizedBox(height: _currentTheme.spacingSm),
                Text(
                  'This demonstrates a comprehensive custom theming system with design tokens.',
                  style: TextStyle(
                    fontSize: _currentTheme.textMd,
                    color: _currentTheme.textSecondary,
                    height: 1.4,
                  ),
                ),
                SizedBox(height: _currentTheme.spacingXl),
                ThemedWidget(
                  customTheme: _currentTheme,
                  title: 'User Interface',
                  description: 'Modern and intuitive user interface components built with consistent design tokens and theming.',
                  icon: Icons.design_services,
                ),
                SizedBox(height: _currentTheme.spacingLg),
                ThemedWidget(
                  customTheme: _currentTheme,
                  title: 'Data Analytics',
                  description: 'Comprehensive data visualization and analytics tools for better insights and decision making.',
                  icon: Icons.analytics,
                ),
                SizedBox(height: _currentTheme.spacingLg),
                ThemedWidget(
                  customTheme: _currentTheme,
                  title: 'Security Features',
                  description: 'Advanced security protocols and encryption to keep your data safe and protected.',
                  icon: Icons.security,
                ),
                SizedBox(height: _currentTheme.spacingXl),
                
                // Color palette showcase
                Text(
                  'Color Palette',
                  style: TextStyle(
                    fontSize: _currentTheme.textLg,
                    fontWeight: FontWeight.bold,
                    color: _currentTheme.textPrimary,
                  ),
                ),
                SizedBox(height: _currentTheme.spacingMd),
                GridView.count(
                  shrinkWrap: true,
                  physics: const NeverScrollableScrollPhysics(),
                  crossAxisCount: 4,
                  crossAxisSpacing: _currentTheme.spacingSm,
                  mainAxisSpacing: _currentTheme.spacingSm,
                  childAspectRatio: 1,
                  children: [
                    _buildColorSwatch('Primary', _currentTheme.primaryColor),
                    _buildColorSwatch('Secondary', _currentTheme.secondaryColor),
                    _buildColorSwatch('Accent', _currentTheme.accentColor),
                    _buildColorSwatch('Success', _currentTheme.successColor),
                    _buildColorSwatch('Warning', _currentTheme.warningColor),
                    _buildColorSwatch('Error', _currentTheme.errorColor),
                    _buildColorSwatch('Info', _currentTheme.infoColor),
                    _buildColorSwatch('Surface', _currentTheme.surfaceColor),
                  ],
                ),
                SizedBox(height: _currentTheme.spacingXl),
                
                // Typography showcase
                Text(
                  'Typography Scale',
                  style: TextStyle(
                    fontSize: _currentTheme.textLg,
                    fontWeight: FontWeight.bold,
                    color: _currentTheme.textPrimary,
                  ),
                ),
                SizedBox(height: _currentTheme.spacingMd),
                Card(
                  child: Padding(
                    padding: EdgeInsets.all(_currentTheme.spacingMd),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          'Display Large (${_currentTheme.textXxl}px)',
                          style: TextStyle(
                            fontSize: _currentTheme.textXxl,
                            fontWeight: FontWeight.bold,
                            color: _currentTheme.textPrimary,
                          ),
                        ),
                        SizedBox(height: _currentTheme.spacingSm),
                        Text(
                          'Headline Large (${_currentTheme.textXl}px)',
                          style: TextStyle(
                            fontSize: _currentTheme.textXl,
                            fontWeight: FontWeight.bold,
                            color: _currentTheme.textPrimary,
                          ),
                        ),
                        SizedBox(height: _currentTheme.spacingSm),
                        Text(
                          'Title Large (${_currentTheme.textLg}px)',
                          style: TextStyle(
                            fontSize: _currentTheme.textLg,
                            fontWeight: FontWeight.w600,
                            color: _currentTheme.textPrimary,
                          ),
                        ),
                        SizedBox(height: _currentTheme.spacingSm),
                        Text(
                          'Body Large (${_currentTheme.textMd}px)',
                          style: TextStyle(
                            fontSize: _currentTheme.textMd,
                            color: _currentTheme.textPrimary,
                          ),
                        ),
                        SizedBox(height: _currentTheme.spacingSm),
                        Text(
                          'Body Medium (${_currentTheme.textSm}px)',
                          style: TextStyle(
                            fontSize: _currentTheme.textSm,
                            color: _currentTheme.textSecondary,
                          ),
                        ),
                        SizedBox(height: _currentTheme.spacingSm),
                        Text(
                          'Label Small (${_currentTheme.textXs}px)',
                          style: TextStyle(
                            fontSize: _currentTheme.textXs,
                            color: _currentTheme.textDisabled,
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
      ),
    );
  }

  Widget _buildColorSwatch(String name, Color color) {
    return Column(
      children: [
        Expanded(
          child: Container(
            decoration: BoxDecoration(
              color: color,
              borderRadius: BorderRadius.circular(_currentTheme.radiusSm),
              boxShadow: [
                BoxShadow(
                  color: Colors.black.withOpacity(0.1),
                  blurRadius: _currentTheme.elevationSm,
                  offset: const Offset(0, 2),
                ),
              ],
            ),
          ),
        ),
        SizedBox(height: _currentTheme.spacingXs),
        Text(
          name,
          style: TextStyle(
            fontSize: _currentTheme.textXs,
            color: _currentTheme.textSecondary,
          ),
          textAlign: TextAlign.center,
          maxLines: 1,
          overflow: TextOverflow.ellipsis,
        ),
      ],
    );
  }
}
```

This comprehensive theme system demonstrates how to create reusable design  
tokens and convert them into Material Theme data. It includes color palettes,  
typography scales, spacing systems, and component theming, providing a  
complete foundation for consistent app-wide styling and theming.  

## Adaptive Theme Widget

Building widgets that adapt to different themes and screen sizes automatically.  

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
      title: 'Adaptive Theme Widget',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.deepPurple,
          brightness: Brightness.light,
        ),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.deepPurple,
          brightness: Brightness.dark,
        ),
      ),
      home: const AdaptiveDemoPage(),
    );
  }
}

class AdaptiveCard extends StatelessWidget {
  final Widget child;
  final EdgeInsets? padding;
  final Color? backgroundColor;
  final double? elevation;
  final BorderRadius? borderRadius;

  const AdaptiveCard({
    super.key,
    required this.child,
    this.padding,
    this.backgroundColor,
    this.elevation,
    this.borderRadius,
  });

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final colorScheme = theme.colorScheme;
    final isDark = theme.brightness == Brightness.dark;
    
    // Adaptive values based on theme
    final adaptivePadding = padding ?? EdgeInsets.all(
      MediaQuery.of(context).size.width > 600 ? 24 : 16,
    );
    
    final adaptiveElevation = elevation ?? (isDark ? 8 : 4);
    
    final adaptiveBackgroundColor = backgroundColor ?? 
        (isDark ? colorScheme.surface : colorScheme.background);
    
    final adaptiveBorderRadius = borderRadius ?? BorderRadius.circular(
      MediaQuery.of(context).size.width > 600 ? 16 : 12,
    );

    return Card(
      color: adaptiveBackgroundColor,
      elevation: adaptiveElevation,
      shape: RoundedRectangleBorder(
        borderRadius: adaptiveBorderRadius,
      ),
      child: Padding(
        padding: adaptivePadding,
        child: child,
      ),
    );
  }
}

class AdaptiveButton extends StatelessWidget {
  final String text;
  final VoidCallback? onPressed;
  final IconData? icon;
  final ButtonType type;
  final ButtonSize size;

  const AdaptiveButton({
    super.key,
    required this.text,
    this.onPressed,
    this.icon,
    this.type = ButtonType.primary,
    this.size = ButtonSize.medium,
  });

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final colorScheme = theme.colorScheme;
    final isDark = theme.brightness == Brightness.dark;
    final isTablet = MediaQuery.of(context).size.width > 600;
    
    // Adaptive sizing
    EdgeInsets padding;
    double fontSize;
    double iconSize;
    
    switch (size) {
      case ButtonSize.small:
        padding = EdgeInsets.symmetric(
          horizontal: isTablet ? 16 : 12,
          vertical: isTablet ? 10 : 8,
        );
        fontSize = isTablet ? 14 : 12;
        iconSize = isTablet ? 18 : 16;
        break;
      case ButtonSize.medium:
        padding = EdgeInsets.symmetric(
          horizontal: isTablet ? 24 : 16,
          vertical: isTablet ? 14 : 12,
        );
        fontSize = isTablet ? 16 : 14;
        iconSize = isTablet ? 20 : 18;
        break;
      case ButtonSize.large:
        padding = EdgeInsets.symmetric(
          horizontal: isTablet ? 32 : 24,
          vertical: isTablet ? 18 : 16,
        );
        fontSize = isTablet ? 18 : 16;
        iconSize = isTablet ? 24 : 20;
        break;
    }

    // Adaptive colors based on type and theme
    Color backgroundColor;
    Color foregroundColor;
    Color? borderColor;
    
    switch (type) {
      case ButtonType.primary:
        backgroundColor = colorScheme.primary;
        foregroundColor = colorScheme.onPrimary;
        break;
      case ButtonType.secondary:
        backgroundColor = isDark 
            ? colorScheme.secondary.withOpacity(0.3)
            : colorScheme.secondary.withOpacity(0.1);
        foregroundColor = colorScheme.secondary;
        break;
      case ButtonType.outlined:
        backgroundColor = Colors.transparent;
        foregroundColor = colorScheme.primary;
        borderColor = colorScheme.primary;
        break;
      case ButtonType.ghost:
        backgroundColor = Colors.transparent;
        foregroundColor = colorScheme.onSurface;
        break;
    }

    final buttonStyle = ElevatedButton.styleFrom(
      backgroundColor: backgroundColor,
      foregroundColor: foregroundColor,
      padding: padding,
      elevation: type == ButtonType.outlined || type == ButtonType.ghost ? 0 : null,
      side: borderColor != null ? BorderSide(color: borderColor) : null,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(8),
      ),
    );

    final buttonChild = Row(
      mainAxisSize: MainAxisSize.min,
      children: [
        if (icon != null) ...[
          Icon(icon, size: iconSize),
          const SizedBox(width: 8),
        ],
        Text(
          text,
          style: TextStyle(
            fontSize: fontSize,
            fontWeight: FontWeight.w600,
          ),
        ),
      ],
    );

    return ElevatedButton(
      onPressed: onPressed,
      style: buttonStyle,
      child: buttonChild,
    );
  }
}

enum ButtonType { primary, secondary, outlined, ghost }
enum ButtonSize { small, medium, large }

class AdaptiveTextField extends StatelessWidget {
  final String label;
  final String? hint;
  final TextEditingController? controller;
  final String? Function(String?)? validator;
  final TextInputType? keyboardType;
  final bool obscureText;
  final IconData? prefixIcon;
  final Widget? suffix;
  final int maxLines;

  const AdaptiveTextField({
    super.key,
    required this.label,
    this.hint,
    this.controller,
    this.validator,
    this.keyboardType,
    this.obscureText = false,
    this.prefixIcon,
    this.suffix,
    this.maxLines = 1,
  });

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final colorScheme = theme.colorScheme;
    final isDark = theme.brightness == Brightness.dark;
    final isTablet = MediaQuery.of(context).size.width > 600;
    
    // Adaptive styling
    final borderRadius = BorderRadius.circular(isTablet ? 12 : 8);
    final contentPadding = EdgeInsets.all(isTablet ? 20 : 16);
    final fontSize = isTablet ? 16.0 : 14.0;
    
    final inputDecoration = InputDecoration(
      labelText: label,
      hintText: hint,
      prefixIcon: prefixIcon != null ? Icon(prefixIcon) : null,
      suffix: suffix,
      border: OutlineInputBorder(
        borderRadius: borderRadius,
        borderSide: BorderSide(
          color: isDark ? colorScheme.outline : Colors.grey.shade300,
        ),
      ),
      enabledBorder: OutlineInputBorder(
        borderRadius: borderRadius,
        borderSide: BorderSide(
          color: isDark ? colorScheme.outline : Colors.grey.shade300,
        ),
      ),
      focusedBorder: OutlineInputBorder(
        borderRadius: borderRadius,
        borderSide: BorderSide(
          color: colorScheme.primary,
          width: 2,
        ),
      ),
      errorBorder: OutlineInputBorder(
        borderRadius: borderRadius,
        borderSide: BorderSide(
          color: colorScheme.error,
        ),
      ),
      focusedErrorBorder: OutlineInputBorder(
        borderRadius: borderRadius,
        borderSide: BorderSide(
          color: colorScheme.error,
          width: 2,
        ),
      ),
      contentPadding: contentPadding,
      filled: true,
      fillColor: isDark 
          ? colorScheme.surface.withOpacity(0.5)
          : colorScheme.background,
    );

    return TextFormField(
      controller: controller,
      validator: validator,
      keyboardType: keyboardType,
      obscureText: obscureText,
      maxLines: maxLines,
      style: TextStyle(fontSize: fontSize),
      decoration: inputDecoration,
    );
  }
}

class AdaptiveAppBar extends StatelessWidget implements PreferredSizeWidget {
  final String title;
  final List<Widget>? actions;
  final Widget? leading;
  final bool centerTitle;

  const AdaptiveAppBar({
    super.key,
    required this.title,
    this.actions,
    this.leading,
    this.centerTitle = true,
  });

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final colorScheme = theme.colorScheme;
    final isDark = theme.brightness == Brightness.dark;
    final isTablet = MediaQuery.of(context).size.width > 600;
    
    // Adaptive styling
    final titleStyle = TextStyle(
      fontSize: isTablet ? 22 : 20,
      fontWeight: FontWeight.bold,
      color: colorScheme.onSurface,
    );
    
    final backgroundColor = isDark 
        ? colorScheme.surface.withOpacity(0.95)
        : colorScheme.background.withOpacity(0.95);
    
    final elevation = isDark ? 8.0 : 4.0;

    return AppBar(
      title: Text(title, style: titleStyle),
      centerTitle: centerTitle,
      leading: leading,
      actions: actions,
      backgroundColor: backgroundColor,
      foregroundColor: colorScheme.onSurface,
      elevation: elevation,
      toolbarHeight: isTablet ? 80 : kToolbarHeight,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.vertical(
          bottom: Radius.circular(isTablet ? 16 : 8),
        ),
      ),
    );
  }

  @override
  Size get preferredSize => Size.fromHeight(
    MediaQuery.of(
      NavigationService.navigatorKey.currentContext!
    ).size.width > 600 ? 80 : kToolbarHeight
  );
}

class NavigationService {
  static final GlobalKey<NavigatorState> navigatorKey = 
      GlobalKey<NavigatorState>();
}

class AdaptiveDemoPage extends StatefulWidget {
  const AdaptiveDemoPage({super.key});

  @override
  State<AdaptiveDemoPage> createState() => _AdaptiveDemoPageState();
}

class _AdaptiveDemoPageState extends State<AdaptiveDemoPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _messageController = TextEditingController();

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _messageController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final isTablet = MediaQuery.of(context).size.width > 600;
    final theme = Theme.of(context);
    
    return Scaffold(
      appBar: AdaptiveAppBar(
        title: 'Adaptive Widgets',
        actions: [
          IconButton(
            icon: Icon(
              theme.brightness == Brightness.dark 
                  ? Icons.light_mode 
                  : Icons.dark_mode,
            ),
            onPressed: () {
              // Theme switching would be handled by parent widget
            },
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: EdgeInsets.all(isTablet ? 24 : 16),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Adaptive Design System',
                style: TextStyle(
                  fontSize: isTablet ? 28 : 24,
                  fontWeight: FontWeight.bold,
                ),
              ),
              const SizedBox(height: 8),
              Text(
                'Components that automatically adapt to different themes and screen sizes.',
                style: TextStyle(
                  fontSize: isTablet ? 16 : 14,
                  color: theme.colorScheme.onSurface.withOpacity(0.7),
                ),
              ),
              SizedBox(height: isTablet ? 32 : 24),
              
              AdaptiveCard(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Contact Form',
                      style: TextStyle(
                        fontSize: isTablet ? 20 : 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    SizedBox(height: isTablet ? 20 : 16),
                    AdaptiveTextField(
                      label: 'Full Name',
                      hint: 'Enter your full name',
                      prefixIcon: Icons.person,
                      controller: _nameController,
                      validator: (value) {
                        if (value?.isEmpty ?? true) {
                          return 'Name is required';
                        }
                        return null;
                      },
                    ),
                    SizedBox(height: isTablet ? 20 : 16),
                    AdaptiveTextField(
                      label: 'Email Address',
                      hint: 'your.email@example.com',
                      prefixIcon: Icons.email,
                      controller: _emailController,
                      keyboardType: TextInputType.emailAddress,
                      validator: (value) {
                        if (value?.isEmpty ?? true) {
                          return 'Email is required';
                        }
                        if (!value!.contains('@')) {
                          return 'Enter a valid email';
                        }
                        return null;
                      },
                    ),
                    SizedBox(height: isTablet ? 20 : 16),
                    AdaptiveTextField(
                      label: 'Message',
                      hint: 'Tell us about your project...',
                      prefixIcon: Icons.message,
                      controller: _messageController,
                      maxLines: 4,
                      validator: (value) {
                        if (value?.isEmpty ?? true) {
                          return 'Message is required';
                        }
                        return null;
                      },
                    ),
                    SizedBox(height: isTablet ? 24 : 20),
                    Row(
                      children: [
                        Expanded(
                          child: AdaptiveButton(
                            text: 'Send Message',
                            icon: Icons.send,
                            type: ButtonType.primary,
                            size: isTablet ? ButtonSize.large : ButtonSize.medium,
                            onPressed: () {
                              if (_formKey.currentState?.validate() ?? false) {
                                ScaffoldMessenger.of(context).showSnackBar(
                                  const SnackBar(
                                    content: Text('Message sent successfully!'),
                                  ),
                                );
                              }
                            },
                          ),
                        ),
                        const SizedBox(width: 12),
                        AdaptiveButton(
                          text: 'Clear',
                          icon: Icons.clear,
                          type: ButtonType.outlined,
                          size: isTablet ? ButtonSize.large : ButtonSize.medium,
                          onPressed: () {
                            _nameController.clear();
                            _emailController.clear();
                            _messageController.clear();
                          },
                        ),
                      ],
                    ),
                  ],
                ),
              ),
              
              SizedBox(height: isTablet ? 32 : 24),
              
              AdaptiveCard(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Button Variations',
                      style: TextStyle(
                        fontSize: isTablet ? 20 : 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    SizedBox(height: isTablet ? 20 : 16),
                    Wrap(
                      spacing: 12,
                      runSpacing: 12,
                      children: [
                        AdaptiveButton(
                          text: 'Primary',
                          type: ButtonType.primary,
                          onPressed: () {},
                        ),
                        AdaptiveButton(
                          text: 'Secondary',
                          type: ButtonType.secondary,
                          onPressed: () {},
                        ),
                        AdaptiveButton(
                          text: 'Outlined',
                          type: ButtonType.outlined,
                          onPressed: () {},
                        ),
                        AdaptiveButton(
                          text: 'Ghost',
                          type: ButtonType.ghost,
                          onPressed: () {},
                        ),
                      ],
                    ),
                    SizedBox(height: isTablet ? 20 : 16),
                    Text(
                      'Button Sizes',
                      style: TextStyle(
                        fontSize: isTablet ? 16 : 14,
                        fontWeight: FontWeight.w600,
                      ),
                    ),
                    SizedBox(height: isTablet ? 12 : 8),
                    Wrap(
                      spacing: 8,
                      runSpacing: 8,
                      children: [
                        AdaptiveButton(
                          text: 'Small',
                          size: ButtonSize.small,
                          onPressed: () {},
                        ),
                        AdaptiveButton(
                          text: 'Medium',
                          size: ButtonSize.medium,
                          onPressed: () {},
                        ),
                        AdaptiveButton(
                          text: 'Large',
                          size: ButtonSize.large,
                          onPressed: () {},
                        ),
                      ],
                    ),
                  ],
                ),
              ),
              
              SizedBox(height: isTablet ? 32 : 24),
              
              AdaptiveCard(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Responsive Layout Info',
                      style: TextStyle(
                        fontSize: isTablet ? 20 : 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    SizedBox(height: isTablet ? 16 : 12),
                    _buildInfoRow('Screen Width', '${MediaQuery.of(context).size.width.toInt()}px'),
                    _buildInfoRow('Screen Height', '${MediaQuery.of(context).size.height.toInt()}px'),
                    _buildInfoRow('Device Type', isTablet ? 'Tablet/Desktop' : 'Mobile'),
                    _buildInfoRow('Theme Mode', theme.brightness.name),
                    _buildInfoRow('Text Scale', MediaQuery.of(context).textScaleFactor.toStringAsFixed(2)),
                  ],
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildInfoRow(String label, String value) {
    final theme = Theme.of(context);
    final isTablet = MediaQuery.of(context).size.width > 600;
    
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: [
          Text(
            label,
            style: TextStyle(
              fontSize: isTablet ? 14 : 12,
              color: theme.colorScheme.onSurface.withOpacity(0.7),
            ),
          ),
          Text(
            value,
            style: TextStyle(
              fontSize: isTablet ? 14 : 12,
              fontWeight: FontWeight.w600,
            ),
          ),
        ],
      ),
    );
  }
}
```

This adaptive theme system demonstrates widgets that automatically respond  
to different screen sizes and theme modes. Components adjust their sizing,  
spacing, colors, and behavior based on device characteristics and theme  
settings, providing optimal user experiences across different contexts.  

## Theme-Aware Icon Widget

Creating icons that adapt to themes and provide visual feedback.  

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
      title: 'Theme-Aware Icons',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.blue,
          brightness: Brightness.dark,
        ),
      ),
      home: const ThemeIconDemoPage(),
    );
  }
}

class ThemeAwareIcon extends StatefulWidget {
  final IconData icon;
  final IconData? activeIcon;
  final double? size;
  final Color? color;
  final Color? activeColor;
  final bool isActive;
  final VoidCallback? onTap;
  final String? tooltip;
  final bool animated;
  final Duration animationDuration;
  final bool showIndicator;
  final IconStyle style;

  const ThemeAwareIcon({
    super.key,
    required this.icon,
    this.activeIcon,
    this.size,
    this.color,
    this.activeColor,
    this.isActive = false,
    this.onTap,
    this.tooltip,
    this.animated = true,
    this.animationDuration = const Duration(milliseconds: 200),
    this.showIndicator = false,
    this.style = IconStyle.normal,
  });

  @override
  State<ThemeAwareIcon> createState() => _ThemeAwareIconState();
}

enum IconStyle { normal, outlined, filled, soft }

class _ThemeAwareIconState extends State<ThemeAwareIcon>
    with TickerProviderStateMixin {
  late AnimationController _scaleController;
  late AnimationController _rotationController;
  late AnimationController _colorController;
  late Animation<double> _scaleAnimation;
  late Animation<double> _rotationAnimation;
  late Animation<Color?> _colorAnimation;

  @override
  void initState() {
    super.initState();
    _initializeAnimations();
  }

  void _initializeAnimations() {
    _scaleController = AnimationController(
      duration: widget.animationDuration,
      vsync: this,
    );
    
    _rotationController = AnimationController(
      duration: Duration(milliseconds: widget.animationDuration.inMilliseconds * 2),
      vsync: this,
    );
    
    _colorController = AnimationController(
      duration: widget.animationDuration,
      vsync: this,
    );

    _scaleAnimation = Tween<double>(
      begin: 1.0,
      end: 1.2,
    ).animate(CurvedAnimation(
      parent: _scaleController,
      curve: Curves.elasticOut,
    ));

    _rotationAnimation = Tween<double>(
      begin: 0.0,
      end: 0.25,
    ).animate(CurvedAnimation(
      parent: _rotationController,
      curve: Curves.easeInOut,
    ));

    if (widget.isActive) {
      _scaleController.forward();
      _colorController.forward();
    }
  }

  @override
  void didUpdateWidget(ThemeAwareIcon oldWidget) {
    super.didUpdateWidget(oldWidget);
    
    if (oldWidget.isActive != widget.isActive) {
      if (widget.isActive) {
        _scaleController.forward();
        _colorController.forward();
      } else {
        _scaleController.reverse();
        _colorController.reverse();
      }
    }
  }

  @override
  void dispose() {
    _scaleController.dispose();
    _rotationController.dispose();
    _colorController.dispose();
    super.dispose();
  }

  Color _getIconColor(BuildContext context) {
    final theme = Theme.of(context);
    final colorScheme = theme.colorScheme;
    
    if (widget.isActive && widget.activeColor != null) {
      return widget.activeColor!;
    } else if (widget.isActive) {
      return colorScheme.primary;
    } else if (widget.color != null) {
      return widget.color!;
    } else {
      return colorScheme.onSurface;
    }
  }

  Widget _buildIconWithStyle(BuildContext context, IconData iconData) {
    final theme = Theme.of(context);
    final colorScheme = theme.colorScheme;
    final iconColor = _getIconColor(context);
    final iconSize = widget.size ?? 24.0;
    
    switch (widget.style) {
      case IconStyle.normal:
        return Icon(
          iconData,
          size: iconSize,
          color: iconColor,
        );
        
      case IconStyle.outlined:
        return Container(
          padding: const EdgeInsets.all(8),
          decoration: BoxDecoration(
            border: Border.all(
              color: iconColor.withOpacity(0.3),
              width: 1.5,
            ),
            borderRadius: BorderRadius.circular(8),
          ),
          child: Icon(
            iconData,
            size: iconSize,
            color: iconColor,
          ),
        );
        
      case IconStyle.filled:
        return Container(
          padding: const EdgeInsets.all(8),
          decoration: BoxDecoration(
            color: iconColor,
            borderRadius: BorderRadius.circular(8),
          ),
          child: Icon(
            iconData,
            size: iconSize,
            color: iconColor.computeLuminance() > 0.5 
                ? Colors.black 
                : Colors.white,
          ),
        );
        
      case IconStyle.soft:
        return Container(
          padding: const EdgeInsets.all(8),
          decoration: BoxDecoration(
            color: iconColor.withOpacity(0.1),
            borderRadius: BorderRadius.circular(8),
          ),
          child: Icon(
            iconData,
            size: iconSize,
            color: iconColor,
          ),
        );
    }
  }

  void _handleTap() {
    if (widget.animated) {
      _rotationController.forward().then((_) {
        _rotationController.reverse();
      });
    }
    widget.onTap?.call();
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final iconData = widget.isActive && widget.activeIcon != null 
        ? widget.activeIcon! 
        : widget.icon;

    _colorAnimation = ColorTween(
      begin: theme.colorScheme.onSurface,
      end: _getIconColor(context),
    ).animate(_colorController);

    Widget iconWidget = _buildIconWithStyle(context, iconData);

    if (widget.animated) {
      iconWidget = AnimatedBuilder(
        animation: Listenable.merge([
          _scaleAnimation,
          _rotationAnimation,
          _colorAnimation,
        ]),
        builder: (context, child) {
          return Transform.scale(
            scale: _scaleAnimation.value,
            child: Transform.rotate(
              angle: _rotationAnimation.value,
              child: iconWidget,
            ),
          );
        },
      );
    }

    if (widget.showIndicator && widget.isActive) {
      iconWidget = Stack(
        clipBehavior: Clip.none,
        children: [
          iconWidget,
          Positioned(
            right: -2,
            top: -2,
            child: Container(
              width: 8,
              height: 8,
              decoration: BoxDecoration(
                color: theme.colorScheme.primary,
                shape: BoxShape.circle,
              ),
            ),
          ),
        ],
      );
    }

    if (widget.onTap != null) {
      iconWidget = GestureDetector(
        onTap: _handleTap,
        child: iconWidget,
      );
    }

    if (widget.tooltip != null) {
      iconWidget = Tooltip(
        message: widget.tooltip!,
        child: iconWidget,
      );
    }

    return iconWidget;
  }
}

class AnimatedIconButton extends StatefulWidget {
  final IconData icon;
  final IconData? activeIcon;
  final String label;
  final bool isSelected;
  final VoidCallback? onPressed;
  final Color? color;
  final Color? selectedColor;

  const AnimatedIconButton({
    super.key,
    required this.icon,
    required this.label,
    this.activeIcon,
    this.isSelected = false,
    this.onPressed,
    this.color,
    this.selectedColor,
  });

  @override
  State<AnimatedIconButton> createState() => _AnimatedIconButtonState();
}

class _AnimatedIconButtonState extends State<AnimatedIconButton>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _scaleAnimation;
  late Animation<Offset> _slideAnimation;

  @override
  void initState() {
    super.initState();
    
    _controller = AnimationController(
      duration: const Duration(milliseconds: 300),
      vsync: this,
    );

    _scaleAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.elasticOut,
    ));

    _slideAnimation = Tween<Offset>(
      begin: const Offset(0, 1),
      end: Offset.zero,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeOut,
    ));

    if (widget.isSelected) {
      _controller.forward();
    }
  }

  @override
  void didUpdateWidget(AnimatedIconButton oldWidget) {
    super.didUpdateWidget(oldWidget);
    
    if (oldWidget.isSelected != widget.isSelected) {
      if (widget.isSelected) {
        _controller.forward();
      } else {
        _controller.reverse();
      }
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final colorScheme = theme.colorScheme;
    
    final iconColor = widget.isSelected 
        ? (widget.selectedColor ?? colorScheme.primary)
        : (widget.color ?? colorScheme.onSurface);
    
    final textColor = widget.isSelected 
        ? (widget.selectedColor ?? colorScheme.primary)
        : colorScheme.onSurface.withOpacity(0.7);

    return GestureDetector(
      onTap: widget.onPressed,
      child: Container(
        padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
        decoration: BoxDecoration(
          color: widget.isSelected 
              ? iconColor.withOpacity(0.1)
              : Colors.transparent,
          borderRadius: BorderRadius.circular(12),
        ),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            ThemeAwareIcon(
              icon: widget.icon,
              activeIcon: widget.activeIcon,
              isActive: widget.isSelected,
              size: 28,
              color: iconColor,
              animated: true,
            ),
            const SizedBox(height: 4),
            SlideTransition(
              position: _slideAnimation,
              child: ScaleTransition(
                scale: _scaleAnimation,
                child: Text(
                  widget.label,
                  style: TextStyle(
                    fontSize: 12,
                    color: textColor,
                    fontWeight: widget.isSelected 
                        ? FontWeight.w600 
                        : FontWeight.normal,
                  ),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class ThemeIconDemoPage extends StatefulWidget {
  const ThemeIconDemoPage({super.key});

  @override
  State<ThemeIconDemoPage> createState() => _ThemeIconDemoPageState();
}

class _ThemeIconDemoPageState extends State<ThemeIconDemoPage> {
  int _selectedTab = 0;
  final Set<int> _favoriteItems = {0, 2};
  bool _isLiked = false;
  bool _isBookmarked = false;
  bool _isFollowing = false;

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Theme-Aware Icons'),
        backgroundColor: theme.colorScheme.inversePrimary,
        actions: [
          ThemeAwareIcon(
            icon: Icons.search,
            tooltip: 'Search',
            onTap: () => print('Search tapped'),
          ),
          const SizedBox(width: 8),
          ThemeAwareIcon(
            icon: Icons.more_vert,
            tooltip: 'More options',
            onTap: () => print('More tapped'),
          ),
          const SizedBox(width: 16),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Interactive Icon Styles',
              style: theme.textTheme.headlineSmall,
            ),
            const SizedBox(height: 20),
            
            // Icon Styles Demo
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Icon Style Variations',
                      style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 16),
                    Row(
                      mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                      children: [
                        Column(
                          children: [
                            ThemeAwareIcon(
                              icon: Icons.home,
                              style: IconStyle.normal,
                              size: 32,
                              onTap: () => print('Normal home'),
                            ),
                            const SizedBox(height: 8),
                            const Text('Normal', style: TextStyle(fontSize: 12)),
                          ],
                        ),
                        Column(
                          children: [
                            ThemeAwareIcon(
                              icon: Icons.home,
                              style: IconStyle.outlined,
                              size: 32,
                              isActive: true,
                              onTap: () => print('Outlined home'),
                            ),
                            const SizedBox(height: 8),
                            const Text('Outlined', style: TextStyle(fontSize: 12)),
                          ],
                        ),
                        Column(
                          children: [
                            ThemeAwareIcon(
                              icon: Icons.home,
                              style: IconStyle.filled,
                              size: 32,
                              isActive: true,
                              onTap: () => print('Filled home'),
                            ),
                            const SizedBox(height: 8),
                            const Text('Filled', style: TextStyle(fontSize: 12)),
                          ],
                        ),
                        Column(
                          children: [
                            ThemeAwareIcon(
                              icon: Icons.home,
                              style: IconStyle.soft,
                              size: 32,
                              isActive: true,
                              onTap: () => print('Soft home'),
                            ),
                            const SizedBox(height: 8),
                            const Text('Soft', style: TextStyle(fontSize: 12)),
                          ],
                        ),
                      ],
                    ),
                  ],
                ),
              ),
            ),
            
            const SizedBox(height: 20),
            Text(
              'Interactive Actions',
              style: theme.textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            // Action Icons
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                  children: [
                    ThemeAwareIcon(
                      icon: Icons.favorite_border,
                      activeIcon: Icons.favorite,
                      isActive: _isLiked,
                      activeColor: Colors.red,
                      tooltip: _isLiked ? 'Unlike' : 'Like',
                      showIndicator: _isLiked,
                      onTap: () {
                        setState(() {
                          _isLiked = !_isLiked;
                        });
                      },
                    ),
                    ThemeAwareIcon(
                      icon: Icons.bookmark_border,
                      activeIcon: Icons.bookmark,
                      isActive: _isBookmarked,
                      activeColor: Colors.orange,
                      tooltip: _isBookmarked ? 'Remove bookmark' : 'Bookmark',
                      onTap: () {
                        setState(() {
                          _isBookmarked = !_isBookmarked;
                        });
                      },
                    ),
                    ThemeAwareIcon(
                      icon: Icons.person_add_outlined,
                      activeIcon: Icons.person_add,
                      isActive: _isFollowing,
                      activeColor: Colors.green,
                      tooltip: _isFollowing ? 'Unfollow' : 'Follow',
                      onTap: () {
                        setState(() {
                          _isFollowing = !_isFollowing;
                        });
                      },
                    ),
                    ThemeAwareIcon(
                      icon: Icons.share_outlined,
                      tooltip: 'Share',
                      onTap: () {
                        ScaffoldMessenger.of(context).showSnackBar(
                          const SnackBar(content: Text('Shared!')),
                        );
                      },
                    ),
                  ],
                ),
              ),
            ),
            
            const SizedBox(height: 20),
            Text(
              'Tab Navigation',
              style: theme.textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            // Tab Navigation
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                  children: [
                    AnimatedIconButton(
                      icon: Icons.home_outlined,
                      activeIcon: Icons.home,
                      label: 'Home',
                      isSelected: _selectedTab == 0,
                      onPressed: () => setState(() => _selectedTab = 0),
                    ),
                    AnimatedIconButton(
                      icon: Icons.search_outlined,
                      activeIcon: Icons.search,
                      label: 'Search',
                      isSelected: _selectedTab == 1,
                      onPressed: () => setState(() => _selectedTab = 1),
                    ),
                    AnimatedIconButton(
                      icon: Icons.notifications_outlined,
                      activeIcon: Icons.notifications,
                      label: 'Notifications',
                      isSelected: _selectedTab == 2,
                      onPressed: () => setState(() => _selectedTab = 2),
                    ),
                    AnimatedIconButton(
                      icon: Icons.person_outlined,
                      activeIcon: Icons.person,
                      label: 'Profile',
                      isSelected: _selectedTab == 3,
                      onPressed: () => setState(() => _selectedTab = 3),
                    ),
                  ],
                ),
              ),
            ),
            
            const SizedBox(height: 20),
            Text(
              'Content Grid',
              style: theme.textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            // Content Grid with Favorite Icons
            GridView.builder(
              shrinkWrap: true,
              physics: const NeverScrollableScrollPhysics(),
              gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
                crossAxisCount: 2,
                crossAxisSpacing: 16,
                mainAxisSpacing: 16,
                childAspectRatio: 1.2,
              ),
              itemCount: 4,
              itemBuilder: (context, index) {
                final isFavorite = _favoriteItems.contains(index);
                final colors = [Colors.blue, Colors.green, Colors.purple, Colors.orange];
                final icons = [Icons.photo, Icons.video_library, Icons.music_note, Icons.article];
                final titles = ['Photos', 'Videos', 'Music', 'Articles'];
                
                return Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16),
                    child: Column(
                      children: [
                        Row(
                          mainAxisAlignment: MainAxisAlignment.spaceBetween,
                          children: [
                            Icon(
                              icons[index],
                              size: 32,
                              color: colors[index],
                            ),
                            ThemeAwareIcon(
                              icon: Icons.favorite_border,
                              activeIcon: Icons.favorite,
                              isActive: isFavorite,
                              activeColor: Colors.red,
                              tooltip: isFavorite ? 'Remove from favorites' : 'Add to favorites',
                              onTap: () {
                                setState(() {
                                  if (isFavorite) {
                                    _favoriteItems.remove(index);
                                  } else {
                                    _favoriteItems.add(index);
                                  }
                                });
                              },
                            ),
                          ],
                        ),
                        const Spacer(),
                        Text(
                          titles[index],
                          style: const TextStyle(
                            fontSize: 16,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        const SizedBox(height: 4),
                        Text(
                          '${(index + 1) * 24} items',
                          style: TextStyle(
                            fontSize: 12,
                            color: theme.colorScheme.onSurface.withOpacity(0.6),
                          ),
                        ),
                      ],
                    ),
                  ),
                );
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

This example demonstrates theme-aware icons that adapt their appearance and  
behavior based on the current theme and state. The icons support multiple  
styles, animations, active states, tooltips, and visual indicators,  
providing rich interactive experiences while maintaining theme consistency.  

## Multi-Theme Support Widget

Implementing widgets that work seamlessly across multiple theme variations.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

enum AppThemeMode { light, dark, system, custom }

class AppThemeConfig {
  final String name;
  final ThemeData lightTheme;
  final ThemeData darkTheme;
  final Color primaryColor;
  final Color accentColor;

  const AppThemeConfig({
    required this.name,
    required this.lightTheme,
    required this.darkTheme,
    required this.primaryColor,
    required this.accentColor,
  });

  static final Map<String, AppThemeConfig> themes = {
    'Blue': AppThemeConfig(
      name: 'Blue',
      primaryColor: Colors.blue,
      accentColor: Colors.cyan,
      lightTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.blue,
          brightness: Brightness.light,
        ),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.blue,
          brightness: Brightness.dark,
        ),
      ),
    ),
    'Green': AppThemeConfig(
      name: 'Green',
      primaryColor: Colors.green,
      accentColor: Colors.lightGreen,
      lightTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.green,
          brightness: Brightness.light,
        ),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.green,
          brightness: Brightness.dark,
        ),
      ),
    ),
    'Purple': AppThemeConfig(
      name: 'Purple',
      primaryColor: Colors.purple,
      accentColor: Colors.deepPurple,
      lightTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.purple,
          brightness: Brightness.light,
        ),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.purple,
          brightness: Brightness.dark,
        ),
      ),
    ),
    'Orange': AppThemeConfig(
      name: 'Orange',
      primaryColor: Colors.orange,
      accentColor: Colors.deepOrange,
      lightTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.orange,
          brightness: Brightness.light,
        ),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.orange,
          brightness: Brightness.dark,
        ),
      ),
    ),
  };
}

class ThemeProvider extends ChangeNotifier {
  AppThemeMode _themeMode = AppThemeMode.system;
  String _selectedTheme = 'Blue';
  
  AppThemeMode get themeMode => _themeMode;
  String get selectedTheme => _selectedTheme;
  
  AppThemeConfig get currentThemeConfig => 
      AppThemeConfig.themes[_selectedTheme]!;
  
  ThemeData get lightTheme => currentThemeConfig.lightTheme;
  ThemeData get darkTheme => currentThemeConfig.darkTheme;
  
  void setThemeMode(AppThemeMode mode) {
    _themeMode = mode;
    notifyListeners();
  }
  
  void setTheme(String themeName) {
    _selectedTheme = themeName;
    notifyListeners();
  }
  
  ThemeMode get materialThemeMode {
    switch (_themeMode) {
      case AppThemeMode.light:
        return ThemeMode.light;
      case AppThemeMode.dark:
        return ThemeMode.dark;
      case AppThemeMode.system:
        return ThemeMode.system;
      case AppThemeMode.custom:
        return ThemeMode.light;
    }
  }
}

class MultiThemeCard extends StatelessWidget {
  final String title;
  final String subtitle;
  final Widget? leading;
  final Widget? trailing;
  final List<Widget> actions;
  final VoidCallback? onTap;
  final EdgeInsets? padding;

  const MultiThemeCard({
    super.key,
    required this.title,
    required this.subtitle,
    this.leading,
    this.trailing,
    this.actions = const [],
    this.onTap,
    this.padding,
  });

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final colorScheme = theme.colorScheme;
    final isDark = theme.brightness == Brightness.dark;
    
    // Adaptive colors based on theme
    final cardColor = isDark 
        ? colorScheme.surface.withOpacity(0.8)
        : colorScheme.background;
    
    final borderColor = isDark
        ? colorScheme.outline.withOpacity(0.3)
        : colorScheme.outline.withOpacity(0.1);
    
    final shadowColor = isDark
        ? Colors.black.withOpacity(0.3)
        : Colors.black.withOpacity(0.1);

    return Card(
      color: cardColor,
      elevation: isDark ? 8 : 2,
      shadowColor: shadowColor,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(16),
        side: BorderSide(
          color: borderColor,
          width: 1,
        ),
      ),
      child: InkWell(
        onTap: onTap,
        borderRadius: BorderRadius.circular(16),
        child: Padding(
          padding: padding ?? const EdgeInsets.all(20),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Row(
                children: [
                  if (leading != null) ...[
                    leading!,
                    const SizedBox(width: 16),
                  ],
                  Expanded(
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          title,
                          style: theme.textTheme.titleLarge?.copyWith(
                            fontWeight: FontWeight.bold,
                            color: colorScheme.onSurface,
                          ),
                        ),
                        const SizedBox(height: 4),
                        Text(
                          subtitle,
                          style: theme.textTheme.bodyMedium?.copyWith(
                            color: colorScheme.onSurface.withOpacity(0.7),
                          ),
                        ),
                      ],
                    ),
                  ),
                  if (trailing != null) trailing!,
                ],
              ),
              if (actions.isNotEmpty) ...[
                const SizedBox(height: 16),
                Row(
                  children: actions
                      .map((action) => Padding(
                            padding: const EdgeInsets.only(right: 8),
                            child: action,
                          ))
                      .toList(),
                ),
              ],
            ],
          ),
        ),
      ),
    );
  }
}

class ThemePreviewWidget extends StatelessWidget {
  final AppThemeConfig themeConfig;
  final bool isSelected;
  final VoidCallback? onTap;

  const ThemePreviewWidget({
    super.key,
    required this.themeConfig,
    this.isSelected = false,
    this.onTap,
  });

  @override
  Widget build(BuildContext context) {
    final currentTheme = Theme.of(context);
    
    return GestureDetector(
      onTap: onTap,
      child: Container(
        margin: const EdgeInsets.all(4),
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(12),
          border: Border.all(
            color: isSelected 
                ? themeConfig.primaryColor
                : Colors.grey.withOpacity(0.3),
            width: isSelected ? 3 : 1,
          ),
        ),
        child: ClipRRect(
          borderRadius: BorderRadius.circular(12),
          child: Column(
            children: [
              // Light theme preview
              Expanded(
                child: Container(
                  width: double.infinity,
                  color: themeConfig.lightTheme.colorScheme.background,
                  child: Column(
                    children: [
                      // App bar preview
                      Container(
                        height: 24,
                        color: themeConfig.primaryColor,
                      ),
                      Expanded(
                        child: Padding(
                          padding: const EdgeInsets.all(8),
                          child: Column(
                            children: [
                              // Card preview
                              Expanded(
                                child: Container(
                                  decoration: BoxDecoration(
                                    color: themeConfig.lightTheme.colorScheme.surface,
                                    borderRadius: BorderRadius.circular(6),
                                    boxShadow: [
                                      BoxShadow(
                                        color: Colors.black.withOpacity(0.1),
                                        blurRadius: 2,
                                        offset: const Offset(0, 1),
                                      ),
                                    ],
                                  ),
                                  child: Row(
                                    children: [
                                      Expanded(
                                        child: Container(
                                          margin: const EdgeInsets.all(4),
                                          decoration: BoxDecoration(
                                            color: themeConfig.primaryColor.withOpacity(0.1),
                                            borderRadius: BorderRadius.circular(4),
                                          ),
                                        ),
                                      ),
                                      Container(
                                        width: 20,
                                        margin: const EdgeInsets.all(4),
                                        decoration: BoxDecoration(
                                          color: themeConfig.accentColor,
                                          borderRadius: BorderRadius.circular(4),
                                        ),
                                      ),
                                    ],
                                  ),
                                ),
                              ),
                              const SizedBox(height: 4),
                              // Button preview
                              Container(
                                height: 12,
                                decoration: BoxDecoration(
                                  color: themeConfig.primaryColor,
                                  borderRadius: BorderRadius.circular(6),
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
              // Dark theme preview
              Expanded(
                child: Container(
                  width: double.infinity,
                  color: themeConfig.darkTheme.colorScheme.background,
                  child: Column(
                    children: [
                      // App bar preview
                      Container(
                        height: 24,
                        color: themeConfig.primaryColor,
                      ),
                      Expanded(
                        child: Padding(
                          padding: const EdgeInsets.all(8),
                          child: Column(
                            children: [
                              // Card preview
                              Expanded(
                                child: Container(
                                  decoration: BoxDecoration(
                                    color: themeConfig.darkTheme.colorScheme.surface,
                                    borderRadius: BorderRadius.circular(6),
                                  ),
                                  child: Row(
                                    children: [
                                      Expanded(
                                        child: Container(
                                          margin: const EdgeInsets.all(4),
                                          decoration: BoxDecoration(
                                            color: themeConfig.primaryColor.withOpacity(0.2),
                                            borderRadius: BorderRadius.circular(4),
                                          ),
                                        ),
                                      ),
                                      Container(
                                        width: 20,
                                        margin: const EdgeInsets.all(4),
                                        decoration: BoxDecoration(
                                          color: themeConfig.accentColor,
                                          borderRadius: BorderRadius.circular(4),
                                        ),
                                      ),
                                    ],
                                  ),
                                ),
                              ),
                              const SizedBox(height: 4),
                              // Button preview
                              Container(
                                height: 12,
                                decoration: BoxDecoration(
                                  color: themeConfig.primaryColor,
                                  borderRadius: BorderRadius.circular(6),
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
            ],
          ),
        ),
      ),
    );
  }
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  final ThemeProvider _themeProvider = ThemeProvider();

  @override
  Widget build(BuildContext context) {
    return ListenableBuilder(
      listenable: _themeProvider,
      builder: (context, child) {
        return MaterialApp(
          title: 'Multi-Theme Support',
          theme: _themeProvider.lightTheme,
          darkTheme: _themeProvider.darkTheme,
          themeMode: _themeProvider.materialThemeMode,
          home: MultiThemeDemoPage(themeProvider: _themeProvider),
        );
      },
    );
  }
}

class MultiThemeDemoPage extends StatelessWidget {
  final ThemeProvider themeProvider;

  const MultiThemeDemoPage({super.key, required this.themeProvider});

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Multi-Theme Support'),
        backgroundColor: theme.colorScheme.inversePrimary,
        actions: [
          PopupMenuButton<AppThemeMode>(
            icon: Icon(_getThemeModeIcon(themeProvider.themeMode)),
            onSelected: (mode) => themeProvider.setThemeMode(mode),
            itemBuilder: (context) => [
              const PopupMenuItem(
                value: AppThemeMode.light,
                child: Row(
                  children: [
                    Icon(Icons.light_mode),
                    SizedBox(width: 8),
                    Text('Light'),
                  ],
                ),
              ),
              const PopupMenuItem(
                value: AppThemeMode.dark,
                child: Row(
                  children: [
                    Icon(Icons.dark_mode),
                    SizedBox(width: 8),
                    Text('Dark'),
                  ],
                ),
              ),
              const PopupMenuItem(
                value: AppThemeMode.system,
                child: Row(
                  children: [
                    Icon(Icons.settings_brightness),
                    SizedBox(width: 8),
                    Text('System'),
                  ],
                ),
              ),
            ],
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Theme Selection',
              style: theme.textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            // Theme previews
            SizedBox(
              height: 120,
              child: ListView.builder(
                scrollDirection: Axis.horizontal,
                itemCount: AppThemeConfig.themes.length,
                itemBuilder: (context, index) {
                  final themeName = AppThemeConfig.themes.keys.elementAt(index);
                  final themeConfig = AppThemeConfig.themes[themeName]!;
                  final isSelected = themeProvider.selectedTheme == themeName;
                  
                  return SizedBox(
                    width: 100,
                    child: Column(
                      children: [
                        Expanded(
                          child: ThemePreviewWidget(
                            themeConfig: themeConfig,
                            isSelected: isSelected,
                            onTap: () => themeProvider.setTheme(themeName),
                          ),
                        ),
                        const SizedBox(height: 8),
                        Text(
                          themeName,
                          style: TextStyle(
                            fontSize: 12,
                            fontWeight: isSelected 
                                ? FontWeight.bold 
                                : FontWeight.normal,
                            color: isSelected 
                                ? themeConfig.primaryColor 
                                : theme.colorScheme.onSurface,
                          ),
                        ),
                      ],
                    ),
                  );
                },
              ),
            ),
            
            const SizedBox(height: 32),
            Text(
              'Theme Features',
              style: theme.textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            // Feature cards
            MultiThemeCard(
              title: 'Adaptive Colors',
              subtitle: 'Colors automatically adapt to light and dark themes',
              leading: Container(
                width: 40,
                height: 40,
                decoration: BoxDecoration(
                  color: theme.colorScheme.primary.withOpacity(0.1),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Icon(
                  Icons.palette,
                  color: theme.colorScheme.primary,
                ),
              ),
              actions: [
                TextButton(
                  onPressed: () {},
                  child: const Text('Customize'),
                ),
              ],
            ),
            
            const SizedBox(height: 16),
            
            MultiThemeCard(
              title: 'Typography Scaling',
              subtitle: 'Text styles that work across all theme variations',
              leading: Container(
                width: 40,
                height: 40,
                decoration: BoxDecoration(
                  color: theme.colorScheme.secondary.withOpacity(0.1),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Icon(
                  Icons.text_fields,
                  color: theme.colorScheme.secondary,
                ),
              ),
              trailing: Icon(
                Icons.arrow_forward_ios,
                size: 16,
                color: theme.colorScheme.onSurface.withOpacity(0.5),
              ),
            ),
            
            const SizedBox(height: 16),
            
            MultiThemeCard(
              title: 'Component Theming',
              subtitle: 'UI components styled consistently across themes',
              leading: Container(
                width: 40,
                height: 40,
                decoration: BoxDecoration(
                  color: theme.colorScheme.tertiary.withOpacity(0.1),
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Icon(
                  Icons.widgets,
                  color: theme.colorScheme.tertiary,
                ),
              ),
              actions: [
                OutlinedButton(
                  onPressed: () {},
                  child: const Text('View'),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: () {},
                  child: const Text('Edit'),
                ),
              ],
            ),
            
            const SizedBox(height: 32),
            Text(
              'Current Theme Info',
              style: theme.textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    _buildInfoRow('Selected Theme', themeProvider.selectedTheme),
                    _buildInfoRow('Theme Mode', themeProvider.themeMode.name),
                    _buildInfoRow('Brightness', theme.brightness.name),
                    _buildInfoRow('Primary Color', 
                      '#${theme.colorScheme.primary.value.toRadixString(16).padLeft(8, '0').substring(2)}'),
                    _buildInfoRow('Surface Color', 
                      '#${theme.colorScheme.surface.value.toRadixString(16).padLeft(8, '0').substring(2)}'),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildInfoRow(String label, String value) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: [
          Text(
            label,
            style: const TextStyle(fontWeight: FontWeight.w500),
          ),
          Text(
            value,
            style: const TextStyle(fontFamily: 'monospace'),
          ),
        ],
      ),
    );
  }

  IconData _getThemeModeIcon(AppThemeMode mode) {
    switch (mode) {
      case AppThemeMode.light:
        return Icons.light_mode;
      case AppThemeMode.dark:
        return Icons.dark_mode;
      case AppThemeMode.system:
        return Icons.settings_brightness;
      case AppThemeMode.custom:
        return Icons.tune;
    }
  }
}
```

This comprehensive multi-theme system demonstrates how to create widgets that  
work seamlessly across different theme configurations. It includes theme  
previews, adaptive colors, consistent typography, and dynamic theme switching  
while maintaining component integrity across all theme variations.  

## Widget Builder Pattern

Creating flexible widgets using the builder pattern for maximum customization.  

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
      title: 'Widget Builder Pattern',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.teal),
      ),
      home: const BuilderPatternDemoPage(),
    );
  }
}

class ListBuilder<T> extends StatelessWidget {
  final List<T> items;
  final Widget Function(BuildContext context, T item, int index) itemBuilder;
  final Widget Function(BuildContext context)? emptyBuilder;
  final Widget Function(BuildContext context)? loadingBuilder;
  final Widget Function(BuildContext context, Object error)? errorBuilder;
  final EdgeInsets? padding;
  final ScrollPhysics? physics;
  final bool shrinkWrap;
  final Widget? header;
  final Widget? footer;
  final Widget Function(BuildContext context, int index)? separatorBuilder;
  final bool isLoading;
  final Object? error;

  const ListBuilder({
    super.key,
    required this.items,
    required this.itemBuilder,
    this.emptyBuilder,
    this.loadingBuilder,
    this.errorBuilder,
    this.padding,
    this.physics,
    this.shrinkWrap = false,
    this.header,
    this.footer,
    this.separatorBuilder,
    this.isLoading = false,
    this.error,
  });

  @override
  Widget build(BuildContext context) {
    if (error != null) {
      return errorBuilder?.call(context, error!) ??
          _buildDefaultError(context, error!);
    }

    if (isLoading) {
      return loadingBuilder?.call(context) ?? _buildDefaultLoading(context);
    }

    if (items.isEmpty) {
      return emptyBuilder?.call(context) ?? _buildDefaultEmpty(context);
    }

    final children = <Widget>[];

    if (header != null) {
      children.add(header!);
    }

    for (int i = 0; i < items.length; i++) {
      children.add(itemBuilder(context, items[i], i));
      
      if (separatorBuilder != null && i < items.length - 1) {
        children.add(separatorBuilder!(context, i));
      }
    }

    if (footer != null) {
      children.add(footer!);
    }

    return ListView(
      padding: padding,
      physics: physics,
      shrinkWrap: shrinkWrap,
      children: children,
    );
  }

  Widget _buildDefaultEmpty(BuildContext context) {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(
            Icons.inbox,
            size: 64,
            color: Colors.grey.shade400,
          ),
          const SizedBox(height: 16),
          Text(
            'No items found',
            style: TextStyle(
              fontSize: 16,
              color: Colors.grey.shade600,
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildDefaultLoading(BuildContext context) {
    return const Center(
      child: CircularProgressIndicator(),
    );
  }

  Widget _buildDefaultError(BuildContext context, Object error) {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(
            Icons.error,
            size: 64,
            color: Colors.red.shade400,
          ),
          const SizedBox(height: 16),
          Text(
            'Error: $error',
            style: TextStyle(
              fontSize: 16,
              color: Colors.red.shade600,
            ),
            textAlign: TextAlign.center,
          ),
        ],
      ),
    );
  }
}

class CardBuilder extends StatelessWidget {
  final Widget Function(BuildContext context)? titleBuilder;
  final Widget Function(BuildContext context)? subtitleBuilder;
  final Widget Function(BuildContext context)? leadingBuilder;
  final Widget Function(BuildContext context)? trailingBuilder;
  final Widget Function(BuildContext context)? contentBuilder;
  final Widget Function(BuildContext context)? actionsBuilder;
  final EdgeInsets? padding;
  final Color? backgroundColor;
  final double? elevation;
  final ShapeBorder? shape;
  final VoidCallback? onTap;

  const CardBuilder({
    super.key,
    this.titleBuilder,
    this.subtitleBuilder,
    this.leadingBuilder,
    this.trailingBuilder,
    this.contentBuilder,
    this.actionsBuilder,
    this.padding,
    this.backgroundColor,
    this.elevation,
    this.shape,
    this.onTap,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      color: backgroundColor,
      elevation: elevation,
      shape: shape,
      child: InkWell(
        onTap: onTap,
        borderRadius: BorderRadius.circular(12),
        child: Padding(
          padding: padding ?? const EdgeInsets.all(16),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              // Header section
              if (titleBuilder != null || 
                  subtitleBuilder != null || 
                  leadingBuilder != null || 
                  trailingBuilder != null)
                Row(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    if (leadingBuilder != null) ...[
                      leadingBuilder!(context),
                      const SizedBox(width: 16),
                    ],
                    Expanded(
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          if (titleBuilder != null)
                            DefaultTextStyle(
                              style: Theme.of(context).textTheme.titleMedium!,
                              child: titleBuilder!(context),
                            ),
                          if (subtitleBuilder != null) ...[
                            const SizedBox(height: 4),
                            DefaultTextStyle(
                              style: Theme.of(context).textTheme.bodySmall!,
                              child: subtitleBuilder!(context),
                            ),
                          ],
                        ],
                      ),
                    ),
                    if (trailingBuilder != null) ...[
                      const SizedBox(width: 16),
                      trailingBuilder!(context),
                    ],
                  ],
                ),
              
              // Content section
              if (contentBuilder != null) ...[
                if (titleBuilder != null || subtitleBuilder != null)
                  const SizedBox(height: 16),
                contentBuilder!(context),
              ],
              
              // Actions section
              if (actionsBuilder != null) ...[
                const SizedBox(height: 16),
                actionsBuilder!(context),
              ],
            ],
          ),
        ),
      ),
    );
  }
}

class FormBuilder extends StatefulWidget {
  final List<FormFieldBuilder> fields;
  final Widget Function(BuildContext context, VoidCallback? onSubmit)? actionsBuilder;
  final void Function(Map<String, dynamic> values)? onSubmit;
  final String? Function(Map<String, dynamic> values)? validator;
  final EdgeInsets? padding;

  const FormBuilder({
    super.key,
    required this.fields,
    this.actionsBuilder,
    this.onSubmit,
    this.validator,
    this.padding,
  });

  @override
  State<FormBuilder> createState() => _FormBuilderState();
}

class FormFieldBuilder {
  final String key;
  final String label;
  final String? hint;
  final String? Function(String?)? validator;
  final TextInputType? keyboardType;
  final bool obscureText;
  final Widget? prefix;
  final Widget? suffix;
  final int maxLines;
  final String? initialValue;

  const FormFieldBuilder({
    required this.key,
    required this.label,
    this.hint,
    this.validator,
    this.keyboardType,
    this.obscureText = false,
    this.prefix,
    this.suffix,
    this.maxLines = 1,
    this.initialValue,
  });
}

class _FormBuilderState extends State<FormBuilder> {
  final _formKey = GlobalKey<FormState>();
  final Map<String, TextEditingController> _controllers = {};

  @override
  void initState() {
    super.initState();
    
    for (final field in widget.fields) {
      _controllers[field.key] = TextEditingController(
        text: field.initialValue,
      );
    }
  }

  @override
  void dispose() {
    for (final controller in _controllers.values) {
      controller.dispose();
    }
    super.dispose();
  }

  Map<String, dynamic> get _values {
    return _controllers.map(
      (key, controller) => MapEntry(key, controller.text),
    );
  }

  void _handleSubmit() {
    if (_formKey.currentState?.validate() ?? false) {
      final values = _values;
      final validationError = widget.validator?.call(values);
      
      if (validationError != null) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text(validationError)),
        );
        return;
      }
      
      widget.onSubmit?.call(values);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: widget.padding ?? EdgeInsets.zero,
      child: Form(
        key: _formKey,
        child: Column(
          children: [
            ...widget.fields.map((field) {
              return Padding(
                padding: const EdgeInsets.only(bottom: 16),
                child: TextFormField(
                  controller: _controllers[field.key],
                  decoration: InputDecoration(
                    labelText: field.label,
                    hintText: field.hint,
                    prefixIcon: field.prefix,
                    suffixIcon: field.suffix,
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(8),
                    ),
                  ),
                  validator: field.validator,
                  keyboardType: field.keyboardType,
                  obscureText: field.obscureText,
                  maxLines: field.maxLines,
                ),
              );
            }),
            
            if (widget.actionsBuilder != null)
              widget.actionsBuilder!(context, _handleSubmit),
          ],
        ),
      ),
    );
  }
}

class BuilderPatternDemoPage extends StatefulWidget {
  const BuilderPatternDemoPage({super.key});

  @override
  State<BuilderPatternDemoPage> createState() => _BuilderPatternDemoPageState();
}

class _BuilderPatternDemoPageState extends State<BuilderPatternDemoPage> {
  final List<String> _items = [];
  bool _isLoading = false;
  Object? _error;

  final List<Map<String, dynamic>> _sampleData = [
    {'title': 'Task 1', 'description': 'Complete project documentation'},
    {'title': 'Task 2', 'description': 'Review code and fix bugs'},
    {'title': 'Task 3', 'description': 'Prepare presentation slides'},
    {'title': 'Task 4', 'description': 'Update test cases'},
    {'title': 'Task 5', 'description': 'Deploy to production'},
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Widget Builder Pattern'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Flexible List Builder',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            // Control buttons
            Row(
              children: [
                ElevatedButton(
                  onPressed: _loadItems,
                  child: const Text('Load Items'),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: _clearItems,
                  child: const Text('Clear'),
                ),
                const SizedBox(width: 8),
                ElevatedButton(
                  onPressed: _simulateError,
                  child: const Text('Error'),
                ),
              ],
            ),
            
            const SizedBox(height: 16),
            
            // List builder demo
            SizedBox(
              height: 300,
              child: Card(
                child: ListBuilder<String>(
                  items: _items,
                  isLoading: _isLoading,
                  error: _error,
                  padding: const EdgeInsets.all(16),
                  header: _items.isNotEmpty && !_isLoading && _error == null
                      ? Padding(
                          padding: const EdgeInsets.only(bottom: 16),
                          child: Text(
                            'Items (${_items.length})',
                            style: Theme.of(context).textTheme.titleMedium,
                          ),
                        )
                      : null,
                  itemBuilder: (context, item, index) {
                    return ListTile(
                      leading: CircleAvatar(
                        child: Text('${index + 1}'),
                      ),
                      title: Text(item),
                      subtitle: Text('Item #${index + 1}'),
                      trailing: IconButton(
                        icon: const Icon(Icons.delete),
                        onPressed: () {
                          setState(() {
                            _items.removeAt(index);
                          });
                        },
                      ),
                    );
                  },
                  separatorBuilder: (context, index) => const Divider(),
                  emptyBuilder: (context) => const Center(
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        Icon(Icons.list, size: 64, color: Colors.grey),
                        SizedBox(height: 16),
                        Text('No items yet'),
                        SizedBox(height: 8),
                        Text('Tap "Load Items" to add some'),
                      ],
                    ),
                  ),
                ),
              ),
            ),
            
            const SizedBox(height: 32),
            Text(
              'Flexible Card Builder',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            // Card builder demo
            ...List.generate(_sampleData.length, (index) {
              final data = _sampleData[index];
              return Padding(
                padding: const EdgeInsets.only(bottom: 16),
                child: CardBuilder(
                  titleBuilder: (context) => Text(data['title']),
                  subtitleBuilder: (context) => Text(data['description']),
                  leadingBuilder: (context) => Container(
                    width: 40,
                    height: 40,
                    decoration: BoxDecoration(
                      color: Theme.of(context).colorScheme.primary.withOpacity(0.1),
                      borderRadius: BorderRadius.circular(8),
                    ),
                    child: Icon(
                      Icons.task_alt,
                      color: Theme.of(context).colorScheme.primary,
                    ),
                  ),
                  trailingBuilder: (context) => PopupMenuButton<String>(
                    onSelected: (value) {
                      ScaffoldMessenger.of(context).showSnackBar(
                        SnackBar(content: Text('Selected: $value')),
                      );
                    },
                    itemBuilder: (context) => [
                      const PopupMenuItem(
                        value: 'edit',
                        child: Text('Edit'),
                      ),
                      const PopupMenuItem(
                        value: 'delete',
                        child: Text('Delete'),
                      ),
                    ],
                  ),
                  contentBuilder: (context) => Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      LinearProgressIndicator(
                        value: (index + 1) / _sampleData.length,
                        backgroundColor: Colors.grey.shade300,
                      ),
                      const SizedBox(height: 8),
                      Text(
                        'Progress: ${((index + 1) / _sampleData.length * 100).toInt()}%',
                        style: Theme.of(context).textTheme.bodySmall,
                      ),
                    ],
                  ),
                  actionsBuilder: (context) => Row(
                    children: [
                      TextButton(
                        onPressed: () {},
                        child: const Text('View'),
                      ),
                      const SizedBox(width: 8),
                      ElevatedButton(
                        onPressed: () {},
                        child: const Text('Complete'),
                      ),
                    ],
                  ),
                  onTap: () {
                    ScaffoldMessenger.of(context).showSnackBar(
                      SnackBar(content: Text('Tapped: ${data['title']}')),
                    );
                  },
                ),
              );
            }),
            
            const SizedBox(height: 32),
            Text(
              'Form Builder',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            // Form builder demo
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: FormBuilder(
                  fields: const [
                    FormFieldBuilder(
                      key: 'name',
                      label: 'Full Name',
                      hint: 'Enter your full name',
                      validator: _validateRequired,
                      prefix: Icon(Icons.person),
                    ),
                    FormFieldBuilder(
                      key: 'email',
                      label: 'Email',
                      hint: 'your.email@example.com',
                      keyboardType: TextInputType.emailAddress,
                      validator: _validateEmail,
                      prefix: Icon(Icons.email),
                    ),
                    FormFieldBuilder(
                      key: 'phone',
                      label: 'Phone Number',
                      hint: '+1 (555) 123-4567',
                      keyboardType: TextInputType.phone,
                      prefix: Icon(Icons.phone),
                    ),
                    FormFieldBuilder(
                      key: 'message',
                      label: 'Message',
                      hint: 'Tell us about yourself...',
                      maxLines: 3,
                      validator: _validateRequired,
                      prefix: Icon(Icons.message),
                    ),
                  ],
                  actionsBuilder: (context, onSubmit) => Row(
                    children: [
                      Expanded(
                        child: OutlinedButton(
                          onPressed: () {
                            // Clear form logic would go here
                          },
                          child: const Text('Clear'),
                        ),
                      ),
                      const SizedBox(width: 16),
                      Expanded(
                        child: ElevatedButton(
                          onPressed: onSubmit,
                          child: const Text('Submit'),
                        ),
                      ),
                    ],
                  ),
                  validator: (values) {
                    if (values['name']?.isEmpty ?? true) {
                      return 'Name is required';
                    }
                    if (values['email']?.isEmpty ?? true) {
                      return 'Email is required';
                    }
                    return null;
                  },
                  onSubmit: (values) {
                    showDialog(
                      context: context,
                      builder: (context) => AlertDialog(
                        title: const Text('Form Submitted'),
                        content: Column(
                          mainAxisSize: MainAxisSize.min,
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: values.entries
                              .where((entry) => entry.value.isNotEmpty)
                              .map((entry) => Text('${entry.key}: ${entry.value}'))
                              .toList(),
                        ),
                        actions: [
                          TextButton(
                            onPressed: () => Navigator.of(context).pop(),
                            child: const Text('OK'),
                          ),
                        ],
                      ),
                    );
                  },
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  static String? _validateRequired(String? value) {
    if (value?.isEmpty ?? true) {
      return 'This field is required';
    }
    return null;
  }

  static String? _validateEmail(String? value) {
    if (value?.isEmpty ?? true) {
      return 'Email is required';
    }
    if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value!)) {
      return 'Enter a valid email address';
    }
    return null;
  }

  void _loadItems() {
    setState(() {
      _isLoading = true;
      _error = null;
    });

    Future.delayed(const Duration(seconds: 1), () {
      if (mounted) {
        setState(() {
          _isLoading = false;
          _items.addAll([
            'Item ${_items.length + 1}',
            'Item ${_items.length + 2}',
            'Item ${_items.length + 3}',
          ]);
        });
      }
    });
  }

  void _clearItems() {
    setState(() {
      _items.clear();
      _error = null;
    });
  }

  void _simulateError() {
    setState(() {
      _error = Exception('Failed to load data');
      _isLoading = false;
    });
  }
}
```

The builder pattern provides maximum flexibility for creating reusable widgets.  
By using builder functions, components can adapt their content, layout, and  
behavior based on specific needs while maintaining consistent structure and  
styling. This approach promotes code reuse and customization.  

## Dark Mode Transition Effects

Creating smooth animations and transitions when switching between themes.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class ThemeAnimationController extends ChangeNotifier {
  bool _isDarkMode = false;
  bool _isAnimating = false;

  bool get isDarkMode => _isDarkMode;
  bool get isAnimating => _isAnimating;

  ThemeMode get themeMode => _isDarkMode ? ThemeMode.dark : ThemeMode.light;

  Future<void> toggleTheme() async {
    _isAnimating = true;
    notifyListeners();

    // Add a small delay for smooth animation
    await Future.delayed(const Duration(milliseconds: 100));

    _isDarkMode = !_isDarkMode;
    notifyListeners();

    // Reset animation state
    await Future.delayed(const Duration(milliseconds: 300));
    _isAnimating = false;
    notifyListeners();
  }
}

class AnimatedThemeSwitch extends StatefulWidget {
  final bool isDarkMode;
  final VoidCallback? onToggle;
  final Duration animationDuration;

  const AnimatedThemeSwitch({
    super.key,
    required this.isDarkMode,
    this.onToggle,
    this.animationDuration = const Duration(milliseconds: 300),
  });

  @override
  State<AnimatedThemeSwitch> createState() => _AnimatedThemeSwitchState();
}

class _AnimatedThemeSwitchState extends State<AnimatedThemeSwitch>
    with TickerProviderStateMixin {
  late AnimationController _switchController;
  late AnimationController _iconController;
  late Animation<double> _switchAnimation;
  late Animation<double> _iconRotation;

  @override
  void initState() {
    super.initState();
    
    _switchController = AnimationController(
      duration: widget.animationDuration,
      vsync: this,
    );
    
    _iconController = AnimationController(
      duration: const Duration(milliseconds: 400),
      vsync: this,
    );

    _switchAnimation = CurvedAnimation(
      parent: _switchController,
      curve: Curves.easeInOut,
    );

    _iconRotation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _iconController,
      curve: Curves.easeInOut,
    ));

    if (widget.isDarkMode) {
      _switchController.value = 1.0;
    }
  }

  @override
  void didUpdateWidget(AnimatedThemeSwitch oldWidget) {
    super.didUpdateWidget(oldWidget);
    
    if (oldWidget.isDarkMode != widget.isDarkMode) {
      _handleThemeChange();
    }
  }

  void _handleThemeChange() {
    _iconController.forward().then((_) {
      _iconController.reverse();
    });

    if (widget.isDarkMode) {
      _switchController.forward();
    } else {
      _switchController.reverse();
    }
  }

  @override
  void dispose() {
    _switchController.dispose();
    _iconController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    return GestureDetector(
      onTap: widget.onToggle,
      child: AnimatedBuilder(
        animation: Listenable.merge([_switchAnimation, _iconRotation]),
        builder: (context, child) {
          return Container(
            width: 60,
            height: 30,
            padding: const EdgeInsets.all(2),
            decoration: BoxDecoration(
              borderRadius: BorderRadius.circular(15),
              color: Color.lerp(
                Colors.grey.shade300,
                theme.colorScheme.primary,
                _switchAnimation.value,
              ),
              boxShadow: [
                BoxShadow(
                  color: Colors.black.withOpacity(0.2),
                  blurRadius: 4,
                  offset: const Offset(0, 2),
                ),
              ],
            ),
            child: Stack(
              children: [
                // Background icons
                Positioned(
                  left: 6,
                  top: 6,
                  child: Icon(
                    Icons.wb_sunny,
                    size: 18,
                    color: Colors.orange.withOpacity(
                      1 - _switchAnimation.value,
                    ),
                  ),
                ),
                Positioned(
                  right: 6,
                  top: 6,
                  child: Icon(
                    Icons.nightlight_round,
                    size: 18,
                    color: Colors.white.withOpacity(_switchAnimation.value),
                  ),
                ),
                // Animated thumb
                AnimatedPositioned(
                  duration: widget.animationDuration,
                  curve: Curves.easeInOut,
                  left: widget.isDarkMode ? 30 : 2,
                  top: 2,
                  child: Transform.rotate(
                    angle: _iconRotation.value * 2 * 3.14159,
                    child: Container(
                      width: 26,
                      height: 26,
                      decoration: BoxDecoration(
                        shape: BoxShape.circle,
                        color: Colors.white,
                        boxShadow: [
                          BoxShadow(
                            color: Colors.black.withOpacity(0.3),
                            blurRadius: 3,
                            offset: const Offset(0, 1),
                          ),
                        ],
                      ),
                      child: Icon(
                        widget.isDarkMode 
                            ? Icons.nightlight_round
                            : Icons.wb_sunny,
                        size: 16,
                        color: widget.isDarkMode 
                            ? theme.colorScheme.primary
                            : Colors.orange,
                      ),
                    ),
                  ),
                ),
              ],
            ),
          );
        },
      ),
    );
  }
}

class ThemeTransitionContainer extends StatefulWidget {
  final Widget child;
  final bool isDarkMode;
  final Duration transitionDuration;

  const ThemeTransitionContainer({
    super.key,
    required this.child,
    required this.isDarkMode,
    this.transitionDuration = const Duration(milliseconds: 300),
  });

  @override
  State<ThemeTransitionContainer> createState() => 
      _ThemeTransitionContainerState();
}

class _ThemeTransitionContainerState extends State<ThemeTransitionContainer>
    with TickerProviderStateMixin {
  late AnimationController _colorController;
  late AnimationController _scaleController;
  late Animation<double> _scaleAnimation;

  @override
  void initState() {
    super.initState();
    
    _colorController = AnimationController(
      duration: widget.transitionDuration,
      vsync: this,
    );
    
    _scaleController = AnimationController(
      duration: const Duration(milliseconds: 200),
      vsync: this,
    );

    _scaleAnimation = Tween<double>(
      begin: 1.0,
      end: 1.05,
    ).animate(CurvedAnimation(
      parent: _scaleController,
      curve: Curves.easeOut,
    ));

    if (widget.isDarkMode) {
      _colorController.value = 1.0;
    }
  }

  @override
  void didUpdateWidget(ThemeTransitionContainer oldWidget) {
    super.didUpdateWidget(oldWidget);
    
    if (oldWidget.isDarkMode != widget.isDarkMode) {
      _handleThemeTransition();
    }
  }

  void _handleThemeTransition() async {
    _scaleController.forward();
    
    if (widget.isDarkMode) {
      await _colorController.forward();
    } else {
      await _colorController.reverse();
    }
    
    _scaleController.reverse();
  }

  @override
  void dispose() {
    _colorController.dispose();
    _scaleController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: Listenable.merge([_colorController, _scaleAnimation]),
      builder: (context, child) {
        return Transform.scale(
          scale: _scaleAnimation.value,
          child: AnimatedContainer(
            duration: widget.transitionDuration,
            child: widget.child,
          ),
        );
      },
    );
  }
}

class AnimatedIcon extends StatefulWidget {
  final IconData lightIcon;
  final IconData darkIcon;
  final bool isDarkMode;
  final double size;
  final Color? lightColor;
  final Color? darkColor;

  const AnimatedIcon({
    super.key,
    required this.lightIcon,
    required this.darkIcon,
    required this.isDarkMode,
    this.size = 24,
    this.lightColor,
    this.darkColor,
  });

  @override
  State<AnimatedIcon> createState() => _AnimatedIconState();
}

class _AnimatedIconState extends State<AnimatedIcon>
    with TickerProviderStateMixin {
  late AnimationController _fadeController;
  late AnimationController _rotationController;

  @override
  void initState() {
    super.initState();
    
    _fadeController = AnimationController(
      duration: const Duration(milliseconds: 300),
      vsync: this,
    );
    
    _rotationController = AnimationController(
      duration: const Duration(milliseconds: 400),
      vsync: this,
    );

    if (widget.isDarkMode) {
      _fadeController.value = 1.0;
    }
  }

  @override
  void didUpdateWidget(AnimatedIcon oldWidget) {
    super.didUpdateWidget(oldWidget);
    
    if (oldWidget.isDarkMode != widget.isDarkMode) {
      _handleIconTransition();
    }
  }

  void _handleIconTransition() async {
    _rotationController.forward().then((_) {
      _rotationController.reverse();
    });

    if (widget.isDarkMode) {
      await _fadeController.forward();
    } else {
      await _fadeController.reverse();
    }
  }

  @override
  void dispose() {
    _fadeController.dispose();
    _rotationController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    return AnimatedBuilder(
      animation: Listenable.merge([_fadeController, _rotationController]),
      builder: (context, child) {
        return Transform.rotate(
          angle: _rotationController.value * 0.5,
          child: Stack(
            alignment: Alignment.center,
            children: [
              Opacity(
                opacity: 1 - _fadeController.value,
                child: Icon(
                  widget.lightIcon,
                  size: widget.size,
                  color: widget.lightColor ?? theme.iconTheme.color,
                ),
              ),
              Opacity(
                opacity: _fadeController.value,
                child: Icon(
                  widget.darkIcon,
                  size: widget.size,
                  color: widget.darkColor ?? theme.iconTheme.color,
                ),
              ),
            ],
          ),
        );
      },
    );
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (_) => ThemeAnimationController(),
      child: Consumer<ThemeAnimationController>(
        builder: (context, themeController, _) {
          return MaterialApp(
            title: 'Dark Mode Transitions',
            theme: ThemeData(
              useMaterial3: true,
              colorScheme: ColorScheme.fromSeed(
                seedColor: Colors.blue,
                brightness: Brightness.light,
              ),
            ),
            darkTheme: ThemeData(
              useMaterial3: true,
              colorScheme: ColorScheme.fromSeed(
                seedColor: Colors.blue,
                brightness: Brightness.dark,
              ),
            ),
            themeMode: themeController.themeMode,
            home: DarkModeTransitionsPage(
              themeController: themeController,
            ),
          );
        },
      ),
    );
  }
}

class ChangeNotifierProvider<T extends ChangeNotifier> extends StatefulWidget {
  final T Function() create;
  final Widget child;

  const ChangeNotifierProvider({
    super.key,
    required this.create,
    required this.child,
  });

  @override
  State<ChangeNotifierProvider<T>> createState() => 
      _ChangeNotifierProviderState<T>();
}

class _ChangeNotifierProviderState<T extends ChangeNotifier> 
    extends State<ChangeNotifierProvider<T>> {
  late T _notifier;

  @override
  void initState() {
    super.initState();
    _notifier = widget.create();
  }

  @override
  void dispose() {
    _notifier.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return _InheritedProvider<T>(
      notifier: _notifier,
      child: widget.child,
    );
  }
}

class _InheritedProvider<T extends ChangeNotifier> extends InheritedWidget {
  final T notifier;

  const _InheritedProvider({
    required this.notifier,
    required super.child,
  });

  @override
  bool updateShouldNotify(covariant _InheritedProvider<T> oldWidget) {
    return notifier != oldWidget.notifier;
  }

  static T of<T extends ChangeNotifier>(BuildContext context) {
    final provider = context.dependOnInheritedWidgetOfExactType<_InheritedProvider<T>>();
    return provider!.notifier;
  }
}

class Consumer<T extends ChangeNotifier> extends StatefulWidget {
  final Widget Function(BuildContext context, T value, Widget? child) builder;
  final Widget? child;

  const Consumer({
    super.key,
    required this.builder,
    this.child,
  });

  @override
  State<Consumer<T>> createState() => _ConsumerState<T>();
}

class _ConsumerState<T extends ChangeNotifier> extends State<Consumer<T>> {
  T? _notifier;

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    final newNotifier = _InheritedProvider.of<T>(context);
    if (_notifier != newNotifier) {
      _notifier?.removeListener(_onNotifierChanged);
      newNotifier.addListener(_onNotifierChanged);
      _notifier = newNotifier;
    }
  }

  @override
  void dispose() {
    _notifier?.removeListener(_onNotifierChanged);
    super.dispose();
  }

  void _onNotifierChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return widget.builder(context, _notifier!, widget.child);
  }
}

class DarkModeTransitionsPage extends StatefulWidget {
  final ThemeAnimationController themeController;

  const DarkModeTransitionsPage({
    super.key,
    required this.themeController,
  });

  @override
  State<DarkModeTransitionsPage> createState() => _DarkModeTransitionsPageState();
}

class _DarkModeTransitionsPageState extends State<DarkModeTransitionsPage> {
  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final isDark = theme.brightness == Brightness.dark;
    
    return ThemeTransitionContainer(
      isDarkMode: isDark,
      child: Scaffold(
        appBar: AppBar(
          title: const Text('Dark Mode Transitions'),
          backgroundColor: theme.colorScheme.inversePrimary,
          actions: [
            AnimatedThemeSwitch(
              isDarkMode: widget.themeController.isDarkMode,
              onToggle: widget.themeController.toggleTheme,
            ),
            const SizedBox(width: 16),
          ],
        ),
        body: SingleChildScrollView(
          padding: const EdgeInsets.all(16),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Theme Transition Effects',
                style: theme.textTheme.headlineSmall,
              ),
              const SizedBox(height: 16),
              
              // Animated icons showcase
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(20),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Animated Icons',
                        style: theme.textTheme.titleMedium,
                      ),
                      const SizedBox(height: 16),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                        children: [
                          Column(
                            children: [
                              AnimatedIcon(
                                lightIcon: Icons.wb_sunny,
                                darkIcon: Icons.nightlight_round,
                                isDarkMode: isDark,
                                size: 40,
                                lightColor: Colors.orange,
                                darkColor: Colors.blue,
                              ),
                              const SizedBox(height: 8),
                              const Text('Weather', style: TextStyle(fontSize: 12)),
                            ],
                          ),
                          Column(
                            children: [
                              AnimatedIcon(
                                lightIcon: Icons.visibility,
                                darkIcon: Icons.visibility_off,
                                isDarkMode: isDark,
                                size: 40,
                              ),
                              const SizedBox(height: 8),
                              const Text('Visibility', style: TextStyle(fontSize: 12)),
                            ],
                          ),
                          Column(
                            children: [
                              AnimatedIcon(
                                lightIcon: Icons.light_mode,
                                darkIcon: Icons.dark_mode,
                                isDarkMode: isDark,
                                size: 40,
                              ),
                              const SizedBox(height: 8),
                              const Text('Theme', style: TextStyle(fontSize: 12)),
                            ],
                          ),
                          Column(
                            children: [
                              AnimatedIcon(
                                lightIcon: Icons.brightness_high,
                                darkIcon: Icons.brightness_2,
                                isDarkMode: isDark,
                                size: 40,
                              ),
                              const SizedBox(height: 8),
                              const Text('Brightness', style: TextStyle(fontSize: 12)),
                            ],
                          ),
                        ],
                      ),
                    ],
                  ),
                ),
              ),
              
              const SizedBox(height: 20),
              
              // Color transition cards
              Text(
                'Color Transitions',
                style: theme.textTheme.titleMedium,
              ),
              const SizedBox(height: 16),
              
              Row(
                children: [
                  Expanded(
                    child: AnimatedContainer(
                      duration: const Duration(milliseconds: 300),
                      height: 100,
                      decoration: BoxDecoration(
                        borderRadius: BorderRadius.circular(12),
                        gradient: LinearGradient(
                          colors: isDark
                              ? [Colors.purple.shade800, Colors.blue.shade800]
                              : [Colors.blue.shade300, Colors.purple.shade300],
                        ),
                      ),
                      child: Center(
                        child: Text(
                          'Primary',
                          style: TextStyle(
                            color: Colors.white,
                            fontWeight: FontWeight.bold,
                            fontSize: 16,
                          ),
                        ),
                      ),
                    ),
                  ),
                  const SizedBox(width: 16),
                  Expanded(
                    child: AnimatedContainer(
                      duration: const Duration(milliseconds: 300),
                      height: 100,
                      decoration: BoxDecoration(
                        borderRadius: BorderRadius.circular(12),
                        gradient: LinearGradient(
                          colors: isDark
                              ? [Colors.green.shade800, Colors.teal.shade800]
                              : [Colors.green.shade300, Colors.teal.shade300],
                        ),
                      ),
                      child: Center(
                        child: Text(
                          'Secondary',
                          style: TextStyle(
                            color: Colors.white,
                            fontWeight: FontWeight.bold,
                            fontSize: 16,
                          ),
                        ),
                      ),
                    ),
                  ),
                ],
              ),
              
              const SizedBox(height: 20),
              
              // Interactive elements
              Text(
                'Interactive Elements',
                style: theme.textTheme.titleMedium,
              ),
              const SizedBox(height: 16),
              
              ListView.builder(
                shrinkWrap: true,
                physics: const NeverScrollableScrollPhysics(),
                itemCount: 5,
                itemBuilder: (context, index) {
                  return Padding(
                    padding: const EdgeInsets.only(bottom: 12),
                    child: AnimatedContainer(
                      duration: Duration(milliseconds: 300 + (index * 50)),
                      curve: Curves.easeOut,
                      child: Card(
                        child: ListTile(
                          leading: CircleAvatar(
                            backgroundColor: theme.colorScheme.primary,
                            child: Text(
                              '${index + 1}',
                              style: TextStyle(
                                color: theme.colorScheme.onPrimary,
                                fontWeight: FontWeight.bold,
                              ),
                            ),
                          ),
                          title: Text('Item ${index + 1}'),
                          subtitle: Text(
                            'This is a sample item with theme transitions',
                          ),
                          trailing: AnimatedIcon(
                            lightIcon: Icons.arrow_forward_ios,
                            darkIcon: Icons.arrow_forward,
                            isDarkMode: isDark,
                            size: 16,
                          ),
                          onTap: () {
                            ScaffoldMessenger.of(context).showSnackBar(
                              SnackBar(
                                content: Text('Tapped item ${index + 1}'),
                                backgroundColor: theme.colorScheme.primary,
                              ),
                            );
                          },
                        ),
                      ),
                    ),
                  );
                },
              ),
              
              const SizedBox(height: 20),
              
              // Theme info card
              Card(
                color: theme.colorScheme.primaryContainer,
                child: Padding(
                  padding: const EdgeInsets.all(20),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Row(
                        children: [
                          AnimatedIcon(
                            lightIcon: Icons.info_outline,
                            darkIcon: Icons.info,
                            isDarkMode: isDark,
                            size: 24,
                          ),
                          const SizedBox(width: 8),
                          Text(
                            'Theme Information',
                            style: theme.textTheme.titleMedium?.copyWith(
                              color: theme.colorScheme.onPrimaryContainer,
                            ),
                          ),
                        ],
                      ),
                      const SizedBox(height: 12),
                      Text(
                        'Current theme: ${isDark ? 'Dark' : 'Light'}\n'
                        'Transition animations: Enabled\n'
                        'Animation duration: 300ms',
                        style: theme.textTheme.bodyMedium?.copyWith(
                          color: theme.colorScheme.onPrimaryContainer,
                        ),
                      ),
                    ],
                  ),
                ),
              ),
            ],
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: widget.themeController.toggleTheme,
          tooltip: 'Toggle Theme',
          child: AnimatedIcon(
            lightIcon: Icons.brightness_6,
            darkIcon: Icons.brightness_4,
            isDarkMode: isDark,
          ),
        ),
      ),
    );
  }
}
```

This example demonstrates smooth theme transition effects with custom  
animations for icons, colors, and UI elements. The transitions provide  
visual feedback when switching between light and dark modes, creating  
an engaging user experience with coordinated animations and effects.  

## Custom Loading Widget

Creating sophisticated loading indicators with theme integration.  

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
      title: 'Custom Loading Widgets',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.indigo,
          brightness: Brightness.dark,
        ),
      ),
      home: const LoadingWidgetDemoPage(),
    );
  }
}

class CustomLoadingWidget extends StatefulWidget {
  final LoadingType type;
  final double size;
  final Color? color;
  final Duration duration;
  final String? message;

  const CustomLoadingWidget({
    super.key,
    this.type = LoadingType.spinner,
    this.size = 40.0,
    this.color,
    this.duration = const Duration(seconds: 1),
    this.message,
  });

  @override
  State<CustomLoadingWidget> createState() => _CustomLoadingWidgetState();
}

enum LoadingType { spinner, pulse, wave, dots, ripple }

class _CustomLoadingWidgetState extends State<CustomLoadingWidget>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: widget.duration,
      vsync: this,
    )..repeat();
    
    _animation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(_controller);
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final color = widget.color ?? theme.colorScheme.primary;
    
    Widget loadingWidget;
    
    switch (widget.type) {
      case LoadingType.spinner:
        loadingWidget = _buildSpinner(color);
        break;
      case LoadingType.pulse:
        loadingWidget = _buildPulse(color);
        break;
      case LoadingType.wave:
        loadingWidget = _buildWave(color);
        break;
      case LoadingType.dots:
        loadingWidget = _buildDots(color);
        break;
      case LoadingType.ripple:
        loadingWidget = _buildRipple(color);
        break;
    }
    
    return Column(
      mainAxisSize: MainAxisSize.min,
      children: [
        SizedBox(
          width: widget.size,
          height: widget.size,
          child: loadingWidget,
        ),
        if (widget.message != null) ...[
          const SizedBox(height: 16),
          Text(
            widget.message!,
            style: theme.textTheme.bodyMedium?.copyWith(
              color: theme.colorScheme.onSurface.withOpacity(0.7),
            ),
            textAlign: TextAlign.center,
          ),
        ],
      ],
    );
  }

  Widget _buildSpinner(Color color) {
    return AnimatedBuilder(
      animation: _animation,
      builder: (context, child) {
        return Transform.rotate(
          angle: _animation.value * 2 * math.pi,
          child: Container(
            width: widget.size,
            height: widget.size,
            decoration: BoxDecoration(
              shape: BoxShape.circle,
              border: Border.all(
                color: color.withOpacity(0.3),
                width: 3,
              ),
            ),
            child: Container(
              decoration: BoxDecoration(
                shape: BoxShape.circle,
                border: Border(
                  top: BorderSide(color: color, width: 3),
                  right: BorderSide(color: Colors.transparent, width: 3),
                  bottom: BorderSide(color: Colors.transparent, width: 3),
                  left: BorderSide(color: Colors.transparent, width: 3),
                ),
              ),
            ),
          ),
        );
      },
    );
  }

  Widget _buildPulse(Color color) {
    return AnimatedBuilder(
      animation: _animation,
      builder: (context, child) {
        final scale = 0.8 + (math.sin(_animation.value * 2 * math.pi) * 0.2);
        final opacity = 0.5 + (math.sin(_animation.value * 2 * math.pi) * 0.5);
        
        return Transform.scale(
          scale: scale,
          child: Container(
            width: widget.size,
            height: widget.size,
            decoration: BoxDecoration(
              shape: BoxShape.circle,
              color: color.withOpacity(opacity),
            ),
          ),
        );
      },
    );
  }

  Widget _buildWave(Color color) {
    return AnimatedBuilder(
      animation: _animation,
      builder: (context, child) {
        return Row(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: List.generate(3, (index) {
            final delay = index * 0.2;
            final waveValue = math.sin((_animation.value + delay) * 2 * math.pi);
            final height = widget.size * 0.3 + (waveValue * widget.size * 0.2);
            
            return Container(
              width: widget.size * 0.2,
              height: height.abs(),
              decoration: BoxDecoration(
                color: color,
                borderRadius: BorderRadius.circular(widget.size * 0.1),
              ),
            );
          }),
        );
      },
    );
  }

  Widget _buildDots(Color color) {
    return AnimatedBuilder(
      animation: _animation,
      builder: (context, child) {
        return Row(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: List.generate(3, (index) {
            final delay = index * 0.3;
            final animValue = (_animation.value + delay) % 1.0;
            final opacity = animValue < 0.5 
                ? animValue * 2 
                : 2 - (animValue * 2);
            
            return Container(
              width: widget.size * 0.25,
              height: widget.size * 0.25,
              decoration: BoxDecoration(
                shape: BoxShape.circle,
                color: color.withOpacity(opacity),
              ),
            );
          }),
        );
      },
    );
  }

  Widget _buildRipple(Color color) {
    return AnimatedBuilder(
      animation: _animation,
      builder: (context, child) {
        return Stack(
          alignment: Alignment.center,
          children: List.generate(3, (index) {
            final delay = index * 0.3;
            final animValue = (_animation.value + delay) % 1.0;
            final scale = animValue;
            final opacity = 1 - animValue;
            
            return Transform.scale(
              scale: scale,
              child: Container(
                width: widget.size,
                height: widget.size,
                decoration: BoxDecoration(
                  shape: BoxShape.circle,
                  border: Border.all(
                    color: color.withOpacity(opacity * 0.8),
                    width: 2,
                  ),
                ),
              ),
            );
          }),
        );
      },
    );
  }
}

class LoadingOverlay extends StatefulWidget {
  final Widget child;
  final bool isLoading;
  final String? message;
  final LoadingType loadingType;
  final Color? overlayColor;
  final Color? loadingColor;

  const LoadingOverlay({
    super.key,
    required this.child,
    required this.isLoading,
    this.message,
    this.loadingType = LoadingType.spinner,
    this.overlayColor,
    this.loadingColor,
  });

  @override
  State<LoadingOverlay> createState() => _LoadingOverlayState();
}

class _LoadingOverlayState extends State<LoadingOverlay>
    with TickerProviderStateMixin {
  late AnimationController _overlayController;
  late Animation<double> _overlayAnimation;

  @override
  void initState() {
    super.initState();
    _overlayController = AnimationController(
      duration: const Duration(milliseconds: 300),
      vsync: this,
    );
    
    _overlayAnimation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _overlayController,
      curve: Curves.easeInOut,
    ));

    if (widget.isLoading) {
      _overlayController.forward();
    }
  }

  @override
  void didUpdateWidget(LoadingOverlay oldWidget) {
    super.didUpdateWidget(oldWidget);
    
    if (oldWidget.isLoading != widget.isLoading) {
      if (widget.isLoading) {
        _overlayController.forward();
      } else {
        _overlayController.reverse();
      }
    }
  }

  @override
  void dispose() {
    _overlayController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    return Stack(
      children: [
        widget.child,
        if (widget.isLoading)
          AnimatedBuilder(
            animation: _overlayAnimation,
            builder: (context, child) {
              return Opacity(
                opacity: _overlayAnimation.value,
                child: Container(
                  color: (widget.overlayColor ?? Colors.black)
                      .withOpacity(0.5 * _overlayAnimation.value),
                  child: Center(
                    child: Container(
                      padding: const EdgeInsets.all(32),
                      decoration: BoxDecoration(
                        color: theme.colorScheme.surface,
                        borderRadius: BorderRadius.circular(16),
                        boxShadow: [
                          BoxShadow(
                            color: Colors.black.withOpacity(0.3),
                            blurRadius: 10,
                            offset: const Offset(0, 5),
                          ),
                        ],
                      ),
                      child: CustomLoadingWidget(
                        type: widget.loadingType,
                        color: widget.loadingColor,
                        message: widget.message,
                      ),
                    ),
                  ),
                ),
              );
            },
          ),
      ],
    );
  }
}

class LoadingWidgetDemoPage extends StatefulWidget {
  const LoadingWidgetDemoPage({super.key});

  @override
  State<LoadingWidgetDemoPage> createState() => _LoadingWidgetDemoPageState();
}

class _LoadingWidgetDemoPageState extends State<LoadingWidgetDemoPage> {
  bool _isLoading = false;
  LoadingType _selectedType = LoadingType.spinner;

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    return LoadingOverlay(
      isLoading: _isLoading,
      message: 'Loading data...',
      loadingType: _selectedType,
      child: Scaffold(
        appBar: AppBar(
          title: const Text('Custom Loading Widgets'),
          backgroundColor: theme.colorScheme.inversePrimary,
        ),
        body: SingleChildScrollView(
          padding: const EdgeInsets.all(16),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'Loading Types',
                style: theme.textTheme.headlineSmall,
              ),
              const SizedBox(height: 20),
              
              // Loading types showcase
              GridView.count(
                shrinkWrap: true,
                physics: const NeverScrollableScrollPhysics(),
                crossAxisCount: 2,
                crossAxisSpacing: 16,
                mainAxisSpacing: 16,
                childAspectRatio: 1.2,
                children: LoadingType.values.map((type) {
                  return Card(
                    child: InkWell(
                      onTap: () {
                        setState(() {
                          _selectedType = type;
                        });
                      },
                      borderRadius: BorderRadius.circular(12),
                      child: Padding(
                        padding: const EdgeInsets.all(16),
                        child: Column(
                          mainAxisAlignment: MainAxisAlignment.center,
                          children: [
                            CustomLoadingWidget(
                              type: type,
                              size: 40,
                            ),
                            const SizedBox(height: 12),
                            Text(
                              type.name.toUpperCase(),
                              style: TextStyle(
                                fontWeight: _selectedType == type 
                                    ? FontWeight.bold 
                                    : FontWeight.normal,
                                color: _selectedType == type 
                                    ? theme.colorScheme.primary 
                                    : null,
                              ),
                            ),
                          ],
                        ),
                      ),
                    ),
                  );
                }).toList(),
              ),
              
              const SizedBox(height: 32),
              
              // Controls
              Text(
                'Controls',
                style: theme.textTheme.headlineSmall,
              ),
              const SizedBox(height: 16),
              
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Selected Type: ${_selectedType.name.toUpperCase()}',
                        style: theme.textTheme.titleMedium,
                      ),
                      const SizedBox(height: 16),
                      Row(
                        children: [
                          Expanded(
                            child: ElevatedButton(
                              onPressed: () {
                                setState(() {
                                  _isLoading = true;
                                });
                                
                                Future.delayed(const Duration(seconds: 3), () {
                                  if (mounted) {
                                    setState(() {
                                      _isLoading = false;
                                    });
                                  }
                                });
                              },
                              child: const Text('Show Loading Overlay'),
                            ),
                          ),
                          const SizedBox(width: 16),
                          Expanded(
                            child: OutlinedButton(
                              onPressed: _isLoading 
                                  ? () {
                                      setState(() {
                                        _isLoading = false;
                                      });
                                    }
                                  : null,
                              child: const Text('Hide Loading'),
                            ),
                          ),
                        ],
                      ),
                    ],
                  ),
                ),
              ),
              
              const SizedBox(height: 32),
              
              // Different sizes and colors
              Text(
                'Customization Examples',
                style: theme.textTheme.headlineSmall,
              ),
              const SizedBox(height: 16),
              
              Card(
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Different Sizes',
                        style: theme.textTheme.titleMedium,
                      ),
                      const SizedBox(height: 16),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                        children: [
                          CustomLoadingWidget(
                            type: _selectedType,
                            size: 20,
                          ),
                          CustomLoadingWidget(
                            type: _selectedType,
                            size: 40,
                          ),
                          CustomLoadingWidget(
                            type: _selectedType,
                            size: 60,
                          ),
                        ],
                      ),
                      const SizedBox(height: 32),
                      Text(
                        'Different Colors',
                        style: theme.textTheme.titleMedium,
                      ),
                      const SizedBox(height: 16),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                        children: [
                          CustomLoadingWidget(
                            type: _selectedType,
                            size: 40,
                            color: Colors.red,
                          ),
                          CustomLoadingWidget(
                            type: _selectedType,
                            size: 40,
                            color: Colors.green,
                          ),
                          CustomLoadingWidget(
                            type: _selectedType,
                            size: 40,
                            color: Colors.orange,
                          ),
                        ],
                      ),
                      const SizedBox(height: 32),
                      Text(
                        'With Messages',
                        style: theme.textTheme.titleMedium,
                      ),
                      const SizedBox(height: 16),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                        children: [
                          Expanded(
                            child: CustomLoadingWidget(
                              type: _selectedType,
                              size: 40,
                              message: 'Loading...',
                            ),
                          ),
                          const SizedBox(width: 16),
                          Expanded(
                            child: CustomLoadingWidget(
                              type: _selectedType,
                              size: 40,
                              message: 'Please wait...',
                              color: Colors.purple,
                            ),
                          ),
                        ],
                      ),
                    ],
                  ),
                ),
              ),
              
              const SizedBox(height: 32),
              
              // Usage examples
              Text(
                'Usage in Lists',
                style: theme.textTheme.headlineSmall,
              ),
              const SizedBox(height: 16),
              
              ListView.builder(
                shrinkWrap: true,
                physics: const NeverScrollableScrollPhysics(),
                itemCount: 3,
                itemBuilder: (context, index) {
                  final isLoading = index == 1; // Simulate loading for middle item
                  
                  return Card(
                    margin: const EdgeInsets.only(bottom: 8),
                    child: ListTile(
                      leading: isLoading
                          ? SizedBox(
                              width: 40,
                              height: 40,
                              child: CustomLoadingWidget(
                                type: LoadingType.pulse,
                                size: 30,
                              ),
                            )
                          : CircleAvatar(
                              child: Text('${index + 1}'),
                            ),
                      title: Text('List Item ${index + 1}'),
                      subtitle: isLoading
                          ? const Text('Loading content...')
                          : const Text('This is a sample list item'),
                      trailing: isLoading
                          ? CustomLoadingWidget(
                              type: LoadingType.dots,
                              size: 20,
                            )
                          : const Icon(Icons.arrow_forward_ios),
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

This comprehensive loading widget system provides multiple animation types,  
customizable colors and sizes, overlay functionality, and theme integration.  
The widgets can be used standalone or as overlays, with smooth animations  
and consistent styling that adapts to different themes and contexts.  

## Responsive Layout Widget

Creating widgets that adapt to different screen sizes and orientations.  

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
      title: 'Responsive Layout Widget',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.green),
      ),
      home: const ResponsiveLayoutDemoPage(),
    );
  }
}

enum ScreenSize { small, medium, large, extraLarge }

class ResponsiveWidget extends StatelessWidget {
  final Widget Function(BuildContext context, ScreenSize screenSize)? builder;
  final Widget? small;
  final Widget? medium;
  final Widget? large;
  final Widget? extraLarge;

  const ResponsiveWidget({
    super.key,
    this.builder,
    this.small,
    this.medium,
    this.large,
    this.extraLarge,
  }) : assert(
          builder != null ||
          (small != null || medium != null || large != null || extraLarge != null),
          'Either builder or at least one size-specific widget must be provided',
        );

  static ScreenSize getScreenSize(BuildContext context) {
    final width = MediaQuery.of(context).size.width;
    
    if (width < 600) {
      return ScreenSize.small;
    } else if (width < 900) {
      return ScreenSize.medium;
    } else if (width < 1200) {
      return ScreenSize.large;
    } else {
      return ScreenSize.extraLarge;
    }
  }

  @override
  Widget build(BuildContext context) {
    final screenSize = getScreenSize(context);
    
    if (builder != null) {
      return builder!(context, screenSize);
    }
    
    switch (screenSize) {
      case ScreenSize.small:
        return small ?? medium ?? large ?? extraLarge!;
      case ScreenSize.medium:
        return medium ?? small ?? large ?? extraLarge!;
      case ScreenSize.large:
        return large ?? medium ?? small ?? extraLarge!;
      case ScreenSize.extraLarge:
        return extraLarge ?? large ?? medium ?? small!;
    }
  }
}

class ResponsiveGrid extends StatelessWidget {
  final List<Widget> children;
  final int smallColumns;
  final int mediumColumns;
  final int largeColumns;
  final int extraLargeColumns;
  final double spacing;
  final double runSpacing;
  final EdgeInsets padding;

  const ResponsiveGrid({
    super.key,
    required this.children,
    this.smallColumns = 1,
    this.mediumColumns = 2,
    this.largeColumns = 3,
    this.extraLargeColumns = 4,
    this.spacing = 16.0,
    this.runSpacing = 16.0,
    this.padding = EdgeInsets.zero,
  });

  int _getColumns(BuildContext context) {
    final screenSize = ResponsiveWidget.getScreenSize(context);
    
    switch (screenSize) {
      case ScreenSize.small:
        return smallColumns;
      case ScreenSize.medium:
        return mediumColumns;
      case ScreenSize.large:
        return largeColumns;
      case ScreenSize.extraLarge:
        return extraLargeColumns;
    }
  }

  @override
  Widget build(BuildContext context) {
    final columns = _getColumns(context);
    
    return Padding(
      padding: padding,
      child: Column(
        children: _buildRows(columns),
      ),
    );
  }

  List<Widget> _buildRows(int columns) {
    final rows = <Widget>[];
    
    for (int i = 0; i < children.length; i += columns) {
      final rowChildren = <Widget>[];
      
      for (int j = 0; j < columns; j++) {
        final index = i + j;
        if (index < children.length) {
          rowChildren.add(Expanded(child: children[index]));
          if (j < columns - 1) {
            rowChildren.add(SizedBox(width: spacing));
          }
        } else {
          rowChildren.add(const Expanded(child: SizedBox.shrink()));
          if (j < columns - 1) {
            rowChildren.add(SizedBox(width: spacing));
          }
        }
      }
      
      rows.add(Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: rowChildren,
      ));
      
      if (i + columns < children.length) {
        rows.add(SizedBox(height: runSpacing));
      }
    }
    
    return rows;
  }
}

class ResponsiveCard extends StatelessWidget {
  final Widget child;
  final EdgeInsets? smallPadding;
  final EdgeInsets? mediumPadding;
  final EdgeInsets? largePadding;
  final EdgeInsets? extraLargePadding;
  final double? smallElevation;
  final double? mediumElevation;
  final double? largeElevation;
  final double? extraLargeElevation;

  const ResponsiveCard({
    super.key,
    required this.child,
    this.smallPadding,
    this.mediumPadding,
    this.largePadding,
    this.extraLargePadding,
    this.smallElevation,
    this.mediumElevation,
    this.largeElevation,
    this.extraLargeElevation,
  });

  EdgeInsets _getPadding(ScreenSize screenSize) {
    switch (screenSize) {
      case ScreenSize.small:
        return smallPadding ?? const EdgeInsets.all(12);
      case ScreenSize.medium:
        return mediumPadding ?? const EdgeInsets.all(16);
      case ScreenSize.large:
        return largePadding ?? const EdgeInsets.all(20);
      case ScreenSize.extraLarge:
        return extraLargePadding ?? const EdgeInsets.all(24);
    }
  }

  double _getElevation(ScreenSize screenSize) {
    switch (screenSize) {
      case ScreenSize.small:
        return smallElevation ?? 2;
      case ScreenSize.medium:
        return mediumElevation ?? 4;
      case ScreenSize.large:
        return largeElevation ?? 6;
      case ScreenSize.extraLarge:
        return extraLargeElevation ?? 8;
    }
  }

  @override
  Widget build(BuildContext context) {
    return ResponsiveWidget(
      builder: (context, screenSize) {
        return Card(
          elevation: _getElevation(screenSize),
          child: Padding(
            padding: _getPadding(screenSize),
            child: child,
          ),
        );
      },
    );
  }
}

class ResponsiveNavigation extends StatelessWidget {
  final List<NavigationItem> items;
  final int selectedIndex;
  final ValueChanged<int>? onItemTapped;

  const ResponsiveNavigation({
    super.key,
    required this.items,
    required this.selectedIndex,
    this.onItemTapped,
  });

  @override
  Widget build(BuildContext context) {
    return ResponsiveWidget(
      builder: (context, screenSize) {
        switch (screenSize) {
          case ScreenSize.small:
            return _buildBottomNavigation();
          case ScreenSize.medium:
            return _buildBottomNavigation();
          case ScreenSize.large:
          case ScreenSize.extraLarge:
            return _buildSideNavigation();
        }
      },
    );
  }

  Widget _buildBottomNavigation() {
    return BottomNavigationBar(
      currentIndex: selectedIndex,
      onTap: onItemTapped,
      type: BottomNavigationBarType.fixed,
      items: items.map((item) => BottomNavigationBarItem(
        icon: Icon(item.icon),
        label: item.label,
      )).toList(),
    );
  }

  Widget _buildSideNavigation() {
    return NavigationRail(
      selectedIndex: selectedIndex,
      onDestinationSelected: onItemTapped,
      labelType: NavigationRailLabelType.all,
      destinations: items.map((item) => NavigationRailDestination(
        icon: Icon(item.icon),
        label: Text(item.label),
      )).toList(),
    );
  }
}

class NavigationItem {
  final IconData icon;
  final String label;

  const NavigationItem({
    required this.icon,
    required this.label,
  });
}

class ResponsiveLayoutDemoPage extends StatefulWidget {
  const ResponsiveLayoutDemoPage({super.key};

  @override
  State<ResponsiveLayoutDemoPage> createState() => 
      _ResponsiveLayoutDemoPageState();
}

class _ResponsiveLayoutDemoPageState extends State<ResponsiveLayoutDemoPage> {
  int _selectedIndex = 0;
  
  final List<NavigationItem> _navItems = const [
    NavigationItem(icon: Icons.home, label: 'Home'),
    NavigationItem(icon: Icons.search, label: 'Search'),
    NavigationItem(icon: Icons.favorite, label: 'Favorites'),
    NavigationItem(icon: Icons.person, label: 'Profile'),
  ];

  @override
  Widget build(BuildContext context) {
    return ResponsiveWidget(
      builder: (context, screenSize) {
        final isLargeScreen = screenSize == ScreenSize.large || 
                            screenSize == ScreenSize.extraLarge;
        
        return Scaffold(
          appBar: AppBar(
            title: const Text('Responsive Layout Demo'),
            backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          ),
          body: Row(
            children: [
              // Side navigation for large screens
              if (isLargeScreen)
                ResponsiveNavigation(
                  items: _navItems,
                  selectedIndex: _selectedIndex,
                  onItemTapped: (index) {
                    setState(() {
                      _selectedIndex = index;
                    });
                  },
                ),
              
              // Main content
              Expanded(
                child: _buildMainContent(screenSize),
              ),
            ],
          ),
          
          // Bottom navigation for small/medium screens
          bottomNavigationBar: isLargeScreen 
              ? null 
              : ResponsiveNavigation(
                  items: _navItems,
                  selectedIndex: _selectedIndex,
                  onItemTapped: (index) {
                    setState(() {
                      _selectedIndex = index;
                    });
                  },
                ),
        );
      },
    );
  }

  Widget _buildMainContent(ScreenSize screenSize) {
    return SingleChildScrollView(
      padding: EdgeInsets.all(screenSize == ScreenSize.small ? 16 : 24),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          // Screen size indicator
          ResponsiveCard(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Current Screen Size',
                  style: Theme.of(context).textTheme.titleLarge,
                ),
                const SizedBox(height: 8),
                Text(
                  '${screenSize.name.toUpperCase()} (${MediaQuery.of(context).size.width.toInt()}px)',
                  style: Theme.of(context).textTheme.headlineSmall?.copyWith(
                    color: Theme.of(context).colorScheme.primary,
                    fontWeight: FontWeight.bold,
                  ),
                ),
                const SizedBox(height: 8),
                Text(
                  'This layout adapts based on screen size and orientation.',
                ),
              ],
            ),
          ),
          
          const SizedBox(height: 24),
          
          // Responsive grid example
          Text(
            'Responsive Grid',
            style: Theme.of(context).textTheme.headlineSmall,
          ),
          const SizedBox(height: 16),
          
          ResponsiveGrid(
            smallColumns: 1,
            mediumColumns: 2,
            largeColumns: 3,
            extraLargeColumns: 4,
            children: List.generate(8, (index) {
              final colors = [
                Colors.blue, Colors.green, Colors.orange, Colors.purple,
                Colors.red, Colors.teal, Colors.pink, Colors.indigo,
              ];
              
              return ResponsiveCard(
                smallPadding: const EdgeInsets.all(12),
                mediumPadding: const EdgeInsets.all(16),
                largePadding: const EdgeInsets.all(20),
                extraLargePadding: const EdgeInsets.all(24),
                child: Column(
                  children: [
                    Container(
                      height: 80,
                      decoration: BoxDecoration(
                        color: colors[index].withOpacity(0.1),
                        borderRadius: BorderRadius.circular(8),
                        border: Border.all(
                          color: colors[index].withOpacity(0.3),
                        ),
                      ),
                      child: Center(
                        child: Icon(
                          Icons.widgets,
                          size: 40,
                          color: colors[index],
                        ),
                      ),
                    ),
                    const SizedBox(height: 12),
                    Text(
                      'Card ${index + 1}',
                      style: Theme.of(context).textTheme.titleMedium,
                    ),
                    const SizedBox(height: 4),
                    Text(
                      'Responsive card that adapts to screen size',
                      style: Theme.of(context).textTheme.bodySmall,
                      textAlign: TextAlign.center,
                    ),
                  ],
                ),
              );
            }),
          ),
          
          const SizedBox(height: 32),
          
          // Different layouts per screen size
          Text(
            'Layout Variations',
            style: Theme.of(context).textTheme.headlineSmall,
          ),
          const SizedBox(height: 16),
          
          ResponsiveWidget(
            small: _buildSmallLayout(),
            medium: _buildMediumLayout(),
            large: _buildLargeLayout(),
            extraLarge: _buildExtraLargeLayout(),
          ),
          
          const SizedBox(height: 32),
          
          // Responsive text example
          Text(
            'Responsive Typography',
            style: Theme.of(context).textTheme.headlineSmall,
          ),
          const SizedBox(height: 16),
          
          ResponsiveCard(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                ResponsiveWidget(
                  builder: (context, screenSize) {
                    double fontSize;
                    switch (screenSize) {
                      case ScreenSize.small:
                        fontSize = 18;
                        break;
                      case ScreenSize.medium:
                        fontSize = 22;
                        break;
                      case ScreenSize.large:
                        fontSize = 26;
                        break;
                      case ScreenSize.extraLarge:
                        fontSize = 30;
                        break;
                    }
                    
                    return Text(
                      'Responsive Heading',
                      style: TextStyle(
                        fontSize: fontSize,
                        fontWeight: FontWeight.bold,
                      ),
                    );
                  },
                ),
                const SizedBox(height: 8),
                ResponsiveWidget(
                  builder: (context, screenSize) {
                    int maxLines;
                    switch (screenSize) {
                      case ScreenSize.small:
                        maxLines = 3;
                        break;
                      case ScreenSize.medium:
                        maxLines = 2;
                        break;
                      case ScreenSize.large:
                      case ScreenSize.extraLarge:
                        maxLines = 1;
                        break;
                    }
                    
                    return Text(
                      'This text adapts its line length and size based on the available screen space. '
                      'On smaller screens, it shows more lines, while on larger screens, it shows fewer lines.',
                      maxLines: maxLines,
                      overflow: TextOverflow.ellipsis,
                    );
                  },
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildSmallLayout() {
    return Column(
      children: [
        ResponsiveCard(
          child: ListTile(
            leading: const Icon(Icons.phone_android),
            title: const Text('Mobile Layout'),
            subtitle: const Text('Optimized for small screens'),
          ),
        ),
        const SizedBox(height: 8),
        ResponsiveCard(
          child: const Padding(
            padding: EdgeInsets.all(16),
            child: Text(
              'This is a single-column layout perfect for mobile devices. '
              'Content is stacked vertically for easy scrolling.',
            ),
          ),
        ),
      ],
    );
  }

  Widget _buildMediumLayout() {
    return Row(
      children: [
        Expanded(
          flex: 2,
          child: ResponsiveCard(
            child: Column(
              children: [
                const ListTile(
                  leading: Icon(Icons.tablet_android),
                  title: Text('Tablet Layout'),
                  subtitle: Text('Two-column design'),
                ),
                const Padding(
                  padding: EdgeInsets.all(16),
                  child: Text(
                    'This layout uses two columns to make better use of the available space on tablets.',
                  ),
                ),
              ],
            ),
          ),
        ),
        const SizedBox(width: 16),
        Expanded(
          flex: 1,
          child: ResponsiveCard(
            child: Container(
              height: 100,
              decoration: BoxDecoration(
                color: Colors.blue.withOpacity(0.1),
                borderRadius: BorderRadius.circular(8),
              ),
              child: const Center(
                child: Icon(Icons.image, size: 40),
              ),
            ),
          ),
        ),
      ],
    );
  }

  Widget _buildLargeLayout() {
    return Column(
      children: [
        Row(
          children: [
            Expanded(
              flex: 2,
              child: ResponsiveCard(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const ListTile(
                      leading: Icon(Icons.computer),
                      title: Text('Desktop Layout'),
                      subtitle: Text('Multi-column with sidebar'),
                    ),
                    const Padding(
                      padding: EdgeInsets.all(16),
                      child: Text(
                        'Desktop layouts can use multiple columns and sidebars '
                        'to display more information efficiently.',
                      ),
                    ),
                  ],
                ),
              ),
            ),
            const SizedBox(width: 16),
            Expanded(
              flex: 1,
              child: Column(
                children: [
                  ResponsiveCard(
                    child: Container(
                      height: 60,
                      decoration: BoxDecoration(
                        color: Colors.green.withOpacity(0.1),
                        borderRadius: BorderRadius.circular(8),
                      ),
                      child: const Center(
                        child: Text('Sidebar 1'),
                      ),
                    ),
                  ),
                  const SizedBox(height: 8),
                  ResponsiveCard(
                    child: Container(
                      height: 60,
                      decoration: BoxDecoration(
                        color: Colors.orange.withOpacity(0.1),
                        borderRadius: BorderRadius.circular(8),
                      ),
                      child: const Center(
                        child: Text('Sidebar 2'),
                      ),
                    ),
                  ),
                ],
              ),
            ),
          ],
        ),
      ],
    );
  }

  Widget _buildExtraLargeLayout() {
    return Row(
      children: [
        Expanded(
          flex: 1,
          child: ResponsiveCard(
            child: Column(
              children: [
                const Icon(Icons.tv, size: 40),
                const SizedBox(height: 8),
                const Text('Large Screen'),
                const SizedBox(height: 8),
                Container(
                  height: 80,
                  decoration: BoxDecoration(
                    color: Colors.purple.withOpacity(0.1),
                    borderRadius: BorderRadius.circular(8),
                  ),
                ),
              ],
            ),
          ),
        ),
        const SizedBox(width: 16),
        Expanded(
          flex: 3,
          child: ResponsiveCard(
            child: const Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Extra Large Layout',
                  style: TextStyle(
                    fontSize: 24,
                    fontWeight: FontWeight.bold,
                  ),
                ),
                SizedBox(height: 16),
                Text(
                  'This layout is optimized for very large screens like '
                  'ultra-wide monitors or TVs. It makes full use of the '
                  'available horizontal space with multiple content areas.',
                ),
              ],
            ),
          ),
        ),
        const SizedBox(width: 16),
        Expanded(
          flex: 1,
          child: Column(
            children: List.generate(3, (index) {
              return Padding(
                padding: const EdgeInsets.only(bottom: 8),
                child: ResponsiveCard(
                  child: Container(
                    height: 50,
                    decoration: BoxDecoration(
                      color: Colors.teal.withOpacity(0.1),
                      borderRadius: BorderRadius.circular(8),
                    ),
                    child: Center(
                      child: Text('Widget ${index + 1}'),
                    ),
                  ),
                ),
              );
            }),
          ),
        ),
      ],
    );
  }
}
```

This responsive layout system provides flexible widgets that adapt to  
different screen sizes and orientations. The system includes responsive  
grids, cards, navigation, and layout builders that automatically adjust  
their presentation based on available space and device characteristics.  

## Theme Inheritance System

Building a hierarchical theme system for complex applications.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class AppTheme {
  final ThemeData materialTheme;
  final CustomThemeExtension customExtension;

  const AppTheme({
    required this.materialTheme,
    required this.customExtension,
  });

  static AppTheme light() {
    return AppTheme(
      materialTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.blue,
          brightness: Brightness.light,
        ),
      ),
      customExtension: const CustomThemeExtension(
        cardBorderRadius: 12,
        buttonBorderRadius: 8,
        inputBorderRadius: 6,
        primaryGradient: LinearGradient(
          colors: [Color(0xFF2196F3), Color(0xFF21CBF3)],
        ),
        secondaryGradient: LinearGradient(
          colors: [Color(0xFF4CAF50), Color(0xFF8BC34A)],
        ),
        customSpacing: CustomSpacing(
          xs: 4,
          sm: 8,
          md: 16,
          lg: 24,
          xl: 32,
          xxl: 48,
        ),
      ),
    );
  }

  static AppTheme dark() {
    return AppTheme(
      materialTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.blue,
          brightness: Brightness.dark,
        ),
      ),
      customExtension: const CustomThemeExtension(
        cardBorderRadius: 16,
        buttonBorderRadius: 12,
        inputBorderRadius: 8,
        primaryGradient: LinearGradient(
          colors: [Color(0xFF1976D2), Color(0xFF0277BD)],
        ),
        secondaryGradient: LinearGradient(
          colors: [Color(0xFF388E3C), Color(0xFF689F38)],
        ),
        customSpacing: CustomSpacing(
          xs: 6,
          sm: 12,
          md: 20,
          lg: 28,
          xl: 36,
          xxl: 52,
        ),
      ),
    );
  }

  AppTheme copyWith({
    ThemeData? materialTheme,
    CustomThemeExtension? customExtension,
  }) {
    return AppTheme(
      materialTheme: materialTheme ?? this.materialTheme,
      customExtension: customExtension ?? this.customExtension,
    );
  }
}

class CustomThemeExtension extends ThemeExtension<CustomThemeExtension> {
  final double cardBorderRadius;
  final double buttonBorderRadius;
  final double inputBorderRadius;
  final Gradient primaryGradient;
  final Gradient secondaryGradient;
  final CustomSpacing customSpacing;

  const CustomThemeExtension({
    required this.cardBorderRadius,
    required this.buttonBorderRadius,
    required this.inputBorderRadius,
    required this.primaryGradient,
    required this.secondaryGradient,
    required this.customSpacing,
  });

  @override
  CustomThemeExtension copyWith({
    double? cardBorderRadius,
    double? buttonBorderRadius,
    double? inputBorderRadius,
    Gradient? primaryGradient,
    Gradient? secondaryGradient,
    CustomSpacing? customSpacing,
  }) {
    return CustomThemeExtension(
      cardBorderRadius: cardBorderRadius ?? this.cardBorderRadius,
      buttonBorderRadius: buttonBorderRadius ?? this.buttonBorderRadius,
      inputBorderRadius: inputBorderRadius ?? this.inputBorderRadius,
      primaryGradient: primaryGradient ?? this.primaryGradient,
      secondaryGradient: secondaryGradient ?? this.secondaryGradient,
      customSpacing: customSpacing ?? this.customSpacing,
    );
  }

  @override
  CustomThemeExtension lerp(
    CustomThemeExtension? other,
    double t,
  ) {
    if (other is! CustomThemeExtension) {
      return this;
    }
    return CustomThemeExtension(
      cardBorderRadius: lerpDouble(cardBorderRadius, other.cardBorderRadius, t) ?? cardBorderRadius,
      buttonBorderRadius: lerpDouble(buttonBorderRadius, other.buttonBorderRadius, t) ?? buttonBorderRadius,
      inputBorderRadius: lerpDouble(inputBorderRadius, other.inputBorderRadius, t) ?? inputBorderRadius,
      primaryGradient: Gradient.lerp(primaryGradient, other.primaryGradient, t) ?? primaryGradient,
      secondaryGradient: Gradient.lerp(secondaryGradient, other.secondaryGradient, t) ?? secondaryGradient,
      customSpacing: CustomSpacing.lerp(customSpacing, other.customSpacing, t),
    );
  }
}

class CustomSpacing {
  final double xs;
  final double sm;
  final double md;
  final double lg;
  final double xl;
  final double xxl;

  const CustomSpacing({
    required this.xs,
    required this.sm,
    required this.md,
    required this.lg,
    required this.xl,
    required this.xxl,
  });

  static CustomSpacing lerp(CustomSpacing a, CustomSpacing b, double t) {
    return CustomSpacing(
      xs: lerpDouble(a.xs, b.xs, t) ?? a.xs,
      sm: lerpDouble(a.sm, b.sm, t) ?? a.sm,
      md: lerpDouble(a.md, b.md, t) ?? a.md,
      lg: lerpDouble(a.lg, b.lg, t) ?? a.lg,
      xl: lerpDouble(a.xl, b.xl, t) ?? a.xl,
      xxl: lerpDouble(a.xxl, b.xxl, t) ?? a.xxl,
    );
  }
}

class ThemeProvider extends ChangeNotifier {
  AppTheme _currentTheme = AppTheme.light();
  
  AppTheme get currentTheme => _currentTheme;
  
  void setTheme(AppTheme theme) {
    _currentTheme = theme;
    notifyListeners();
  }
  
  void toggleTheme() {
    if (_currentTheme.materialTheme.brightness == Brightness.light) {
      setTheme(AppTheme.dark());
    } else {
      setTheme(AppTheme.light());
    }
  }
}

class InheritedAppTheme extends InheritedWidget {
  final AppTheme theme;

  const InheritedAppTheme({
    super.key,
    required this.theme,
    required super.child,
  });

  static AppTheme of(BuildContext context) {
    final inheritedTheme = context.dependOnInheritedWidgetOfExactType<InheritedAppTheme>();
    return inheritedTheme?.theme ?? AppTheme.light();
  }

  @override
  bool updateShouldNotify(InheritedAppTheme oldWidget) {
    return theme != oldWidget.theme;
  }
}

class ThemedContainer extends StatelessWidget {
  final Widget child;
  final Color? backgroundColor;
  final EdgeInsets? padding;
  final EdgeInsets? margin;
  final bool useGradient;

  const ThemedContainer({
    super.key,
    required this.child,
    this.backgroundColor,
    this.padding,
    this.margin,
    this.useGradient = false,
  });

  @override
  Widget build(BuildContext context) {
    final appTheme = InheritedAppTheme.of(context);
    final colorScheme = Theme.of(context).colorScheme;
    
    return Container(
      padding: padding ?? EdgeInsets.all(appTheme.customExtension.customSpacing.md),
      margin: margin ?? EdgeInsets.all(appTheme.customExtension.customSpacing.sm),
      decoration: BoxDecoration(
        color: useGradient ? null : (backgroundColor ?? colorScheme.surface),
        gradient: useGradient ? appTheme.customExtension.primaryGradient : null,
        borderRadius: BorderRadius.circular(appTheme.customExtension.cardBorderRadius),
        boxShadow: [
          BoxShadow(
            color: Colors.black.withOpacity(0.1),
            blurRadius: 8,
            offset: const Offset(0, 2),
          ),
        ],
      ),
      child: child,
    );
  }
}

class ThemedButton extends StatelessWidget {
  final String text;
  final VoidCallback? onPressed;
  final ButtonVariant variant;
  final bool isLoading;

  const ThemedButton({
    super.key,
    required this.text,
    this.onPressed,
    this.variant = ButtonVariant.primary,
    this.isLoading = false,
  });

  @override
  Widget build(BuildContext context) {
    final appTheme = InheritedAppTheme.of(context);
    final colorScheme = Theme.of(context).colorScheme;
    
    Color backgroundColor;
    Color textColor;
    Gradient? gradient;
    
    switch (variant) {
      case ButtonVariant.primary:
        gradient = appTheme.customExtension.primaryGradient;
        textColor = Colors.white;
        break;
      case ButtonVariant.secondary:
        gradient = appTheme.customExtension.secondaryGradient;
        textColor = Colors.white;
        break;
      case ButtonVariant.outline:
        backgroundColor = Colors.transparent;
        textColor = colorScheme.primary;
        break;
      case ButtonVariant.text:
        backgroundColor = Colors.transparent;
        textColor = colorScheme.primary;
        break;
    }

    return Container(
      height: 48,
      decoration: BoxDecoration(
        color: gradient == null ? (backgroundColor ?? colorScheme.primary) : null,
        gradient: gradient,
        borderRadius: BorderRadius.circular(appTheme.customExtension.buttonBorderRadius),
        border: variant == ButtonVariant.outline
            ? Border.all(color: colorScheme.primary)
            : null,
      ),
      child: Material(
        color: Colors.transparent,
        child: InkWell(
          onTap: isLoading ? null : onPressed,
          borderRadius: BorderRadius.circular(appTheme.customExtension.buttonBorderRadius),
          child: Center(
            child: isLoading
                ? SizedBox(
                    width: 20,
                    height: 20,
                    child: CircularProgressIndicator(
                      strokeWidth: 2,
                      valueColor: AlwaysStoppedAnimation<Color>(textColor),
                    ),
                  )
                : Text(
                    text,
                    style: TextStyle(
                      color: textColor,
                      fontWeight: FontWeight.w600,
                      fontSize: 16,
                    ),
                  ),
          ),
        ),
      ),
    );
  }
}

enum ButtonVariant { primary, secondary, outline, text }

class ThemedInput extends StatelessWidget {
  final String label;
  final String? hint;
  final TextEditingController? controller;
  final String? Function(String?)? validator;
  final bool obscureText;
  final IconData? prefixIcon;
  final Widget? suffixIcon;

  const ThemedInput({
    super.key,
    required this.label,
    this.hint,
    this.controller,
    this.validator,
    this.obscureText = false,
    this.prefixIcon,
    this.suffixIcon,
  });

  @override
  Widget build(BuildContext context) {
    final appTheme = InheritedAppTheme.of(context);
    final colorScheme = Theme.of(context).colorScheme;
    
    return TextFormField(
      controller: controller,
      validator: validator,
      obscureText: obscureText,
      decoration: InputDecoration(
        labelText: label,
        hintText: hint,
        prefixIcon: prefixIcon != null ? Icon(prefixIcon) : null,
        suffixIcon: suffixIcon,
        border: OutlineInputBorder(
          borderRadius: BorderRadius.circular(appTheme.customExtension.inputBorderRadius),
          borderSide: BorderSide(color: colorScheme.outline),
        ),
        focusedBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(appTheme.customExtension.inputBorderRadius),
          borderSide: BorderSide(color: colorScheme.primary, width: 2),
        ),
        enabledBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(appTheme.customExtension.inputBorderRadius),
          borderSide: BorderSide(color: colorScheme.outline.withOpacity(0.5)),
        ),
        contentPadding: EdgeInsets.all(appTheme.customExtension.customSpacing.md),
        filled: true,
        fillColor: colorScheme.surface,
      ),
    );
  }
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (_) => ThemeProvider(),
      child: Consumer<ThemeProvider>(
        builder: (context, themeProvider, _) {
          return InheritedAppTheme(
            theme: themeProvider.currentTheme,
            child: MaterialApp(
              title: 'Theme Inheritance System',
              theme: themeProvider.currentTheme.materialTheme.copyWith(
                extensions: [themeProvider.currentTheme.customExtension],
              ),
              home: const ThemeInheritanceDemoPage(),
            ),
          );
        },
      ),
    );
  }
}

// Simple provider implementations
class ChangeNotifierProvider<T extends ChangeNotifier> extends StatefulWidget {
  final T Function() create;
  final Widget child;

  const ChangeNotifierProvider({
    super.key,
    required this.create,
    required this.child,
  });

  @override
  State<ChangeNotifierProvider<T>> createState() => _ChangeNotifierProviderState<T>();
}

class _ChangeNotifierProviderState<T extends ChangeNotifier> extends State<ChangeNotifierProvider<T>> {
  late T _notifier;

  @override
  void initState() {
    super.initState();
    _notifier = widget.create();
  }

  @override
  void dispose() {
    _notifier.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return _InheritedProvider<T>(
      notifier: _notifier,
      child: widget.child,
    );
  }
}

class _InheritedProvider<T extends ChangeNotifier> extends InheritedWidget {
  final T notifier;

  const _InheritedProvider({
    required this.notifier,
    required super.child,
  });

  @override
  bool updateShouldNotify(covariant _InheritedProvider<T> oldWidget) {
    return notifier != oldWidget.notifier;
  }

  static T of<T extends ChangeNotifier>(BuildContext context) {
    final provider = context.dependOnInheritedWidgetOfExactType<_InheritedProvider<T>>();
    return provider!.notifier;
  }
}

class Consumer<T extends ChangeNotifier> extends StatefulWidget {
  final Widget Function(BuildContext context, T value, Widget? child) builder;
  final Widget? child;

  const Consumer({
    super.key,
    required this.builder,
    this.child,
  });

  @override
  State<Consumer<T>> createState() => _ConsumerState<T>();
}

class _ConsumerState<T extends ChangeNotifier> extends State<Consumer<T>> {
  T? _notifier;

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    final newNotifier = _InheritedProvider.of<T>(context);
    if (_notifier != newNotifier) {
      _notifier?.removeListener(_onNotifierChanged);
      newNotifier.addListener(_onNotifierChanged);
      _notifier = newNotifier;
    }
  }

  @override
  void dispose() {
    _notifier?.removeListener(_onNotifierChanged);
    super.dispose();
  }

  void _onNotifierChanged() {
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return widget.builder(context, _notifier!, widget.child);
  }
}

class ThemeInheritanceDemoPage extends StatefulWidget {
  const ThemeInheritanceDemoPage({super.key};

  @override
  State<ThemeInheritanceDemoPage> createState() => _ThemeInheritanceDemoPageState();
}

class _ThemeInheritanceDemoPageState extends State<ThemeInheritanceDemoPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  bool _isLoading = false;

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final themeProvider = _InheritedProvider.of<ThemeProvider>(context);
    final appTheme = InheritedAppTheme.of(context);
    final spacing = appTheme.customExtension.customSpacing;
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Theme Inheritance System'),
        actions: [
          IconButton(
            icon: Icon(
              Theme.of(context).brightness == Brightness.dark
                  ? Icons.light_mode
                  : Icons.dark_mode,
            ),
            onPressed: themeProvider.toggleTheme,
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: EdgeInsets.all(spacing.md),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              ThemedContainer(
                useGradient: true,
                child: Column(
                  children: [
                    const Icon(
                      Icons.palette,
                      size: 48,
                      color: Colors.white,
                    ),
                    SizedBox(height: spacing.sm),
                    const Text(
                      'Theme Inheritance Demo',
                      style: TextStyle(
                        fontSize: 24,
                        fontWeight: FontWeight.bold,
                        color: Colors.white,
                      ),
                    ),
                    SizedBox(height: spacing.xs),
                    const Text(
                      'Components inherit theme properties automatically',
                      style: TextStyle(
                        color: Colors.white70,
                      ),
                      textAlign: TextAlign.center,
                    ),
                  ],
                ),
              ),
              
              SizedBox(height: spacing.lg),
              
              Text(
                'Form Example',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
              
              SizedBox(height: spacing.md),
              
              ThemedContainer(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Contact Information',
                      style: Theme.of(context).textTheme.titleLarge,
                    ),
                    SizedBox(height: spacing.md),
                    ThemedInput(
                      label: 'Full Name',
                      hint: 'Enter your full name',
                      controller: _nameController,
                      prefixIcon: Icons.person,
                      validator: (value) {
                        if (value?.isEmpty ?? true) {
                          return 'Name is required';
                        }
                        return null;
                      },
                    ),
                    SizedBox(height: spacing.md),
                    ThemedInput(
                      label: 'Email Address',
                      hint: 'your.email@example.com',
                      controller: _emailController,
                      prefixIcon: Icons.email,
                      validator: (value) {
                        if (value?.isEmpty ?? true) {
                          return 'Email is required';
                        }
                        if (!value!.contains('@')) {
                          return 'Enter a valid email';
                        }
                        return null;
                      },
                    ),
                    SizedBox(height: spacing.lg),
                    Row(
                      children: [
                        Expanded(
                          child: ThemedButton(
                            text: 'Submit',
                            variant: ButtonVariant.primary,
                            isLoading: _isLoading,
                            onPressed: () async {
                              if (_formKey.currentState?.validate() ?? false) {
                                setState(() {
                                  _isLoading = true;
                                });
                                
                                await Future.delayed(const Duration(seconds: 2));
                                
                                setState(() {
                                  _isLoading = false;
                                });
                                
                                if (mounted) {
                                  ScaffoldMessenger.of(context).showSnackBar(
                                    const SnackBar(
                                      content: Text('Form submitted successfully!'),
                                    ),
                                  );
                                }
                              }
                            },
                          ),
                        ),
                        SizedBox(width: spacing.sm),
                        Expanded(
                          child: ThemedButton(
                            text: 'Clear',
                            variant: ButtonVariant.outline,
                            onPressed: () {
                              _nameController.clear();
                              _emailController.clear();
                            },
                          ),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
              
              SizedBox(height: spacing.lg),
              
              Text(
                'Button Variants',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
              
              SizedBox(height: spacing.md),
              
              ThemedContainer(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.stretch,
                  children: [
                    ThemedButton(
                      text: 'Primary Button',
                      variant: ButtonVariant.primary,
                      onPressed: () {},
                    ),
                    SizedBox(height: spacing.sm),
                    ThemedButton(
                      text: 'Secondary Button',
                      variant: ButtonVariant.secondary,
                      onPressed: () {},
                    ),
                    SizedBox(height: spacing.sm),
                    ThemedButton(
                      text: 'Outline Button',
                      variant: ButtonVariant.outline,
                      onPressed: () {},
                    ),
                    SizedBox(height: spacing.sm),
                    ThemedButton(
                      text: 'Text Button',
                      variant: ButtonVariant.text,
                      onPressed: () {},
                    ),
                  ],
                ),
              ),
              
              SizedBox(height: spacing.lg),
              
              Text(
                'Theme Properties',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
              
              SizedBox(height: spacing.md),
              
              ThemedContainer(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    _buildThemeProperty('Card Border Radius', '${appTheme.customExtension.cardBorderRadius}px'),
                    _buildThemeProperty('Button Border Radius', '${appTheme.customExtension.buttonBorderRadius}px'),
                    _buildThemeProperty('Input Border Radius', '${appTheme.customExtension.inputBorderRadius}px'),
                    _buildThemeProperty('Small Spacing', '${spacing.sm}px'),
                    _buildThemeProperty('Medium Spacing', '${spacing.md}px'),
                    _buildThemeProperty('Large Spacing', '${spacing.lg}px'),
                    _buildThemeProperty('Theme Mode', Theme.of(context).brightness.name),
                  ],
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildThemeProperty(String label, String value) {
    final spacing = InheritedAppTheme.of(context).customExtension.customSpacing;
    
    return Padding(
      padding: EdgeInsets.symmetric(vertical: spacing.xs),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: [
          Text(
            label,
            style: const TextStyle(fontWeight: FontWeight.w500),
          ),
          Text(
            value,
            style: TextStyle(
              fontFamily: 'monospace',
              color: Theme.of(context).colorScheme.primary,
            ),
          ),
        ],
      ),
    );
  }
}
```

This theme inheritance system demonstrates how to create a hierarchical  
theming architecture where components automatically inherit styling  
properties. The system uses InheritedWidget for efficient theme  
propagation and ThemeExtension for custom design tokens and properties.  

## Custom Animation Widget

Building reusable animated widgets with customizable transitions.  

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
      title: 'Custom Animation Widgets',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.purple),
      ),
      home: const AnimationDemoPage(),
    );
  }
}

class AnimatedEntry extends StatefulWidget {
  final Widget child;
  final Duration delay;
  final Duration duration;
  final Curve curve;
  final AnimationType animationType;
  final Offset? slideOffset;
  final double? rotationAngle;
  final double? scaleStart;

  const AnimatedEntry({
    super.key,
    required this.child,
    this.delay = Duration.zero,
    this.duration = const Duration(milliseconds: 600),
    this.curve = Curves.easeOutBack,
    this.animationType = AnimationType.fadeSlideUp,
    this.slideOffset,
    this.rotationAngle,
    this.scaleStart,
  });

  @override
  State<AnimatedEntry> createState() => _AnimatedEntryState();
}

enum AnimationType { 
  fadeIn, 
  slideUp, 
  slideDown, 
  slideLeft, 
  slideRight,
  fadeSlideUp,
  fadeSlideDown, 
  scaleUp,
  rotation,
  bounce
}

class _AnimatedEntryState extends State<AnimatedEntry>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _fadeAnimation;
  late Animation<Offset> _slideAnimation;
  late Animation<double> _scaleAnimation;
  late Animation<double> _rotationAnimation;

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
      curve: widget.curve,
    ));

    _slideAnimation = Tween<Offset>(
      begin: _getSlideBegin(),
      end: Offset.zero,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: widget.curve,
    ));

    _scaleAnimation = Tween<double>(
      begin: widget.scaleStart ?? 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: widget.curve,
    ));

    _rotationAnimation = Tween<double>(
      begin: 0.0,
      end: widget.rotationAngle ?? 1.0,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: widget.curve,
    ));

    Future.delayed(widget.delay, () {
      if (mounted) {
        _controller.forward();
      }
    });
  }

  Offset _getSlideBegin() {
    if (widget.slideOffset != null) return widget.slideOffset!;
    
    switch (widget.animationType) {
      case AnimationType.slideUp:
      case AnimationType.fadeSlideUp:
        return const Offset(0, 1);
      case AnimationType.slideDown:
      case AnimationType.fadeSlideDown:
        return const Offset(0, -1);
      case AnimationType.slideLeft:
        return const Offset(-1, 0);
      case AnimationType.slideRight:
        return const Offset(1, 0);
      default:
        return Offset.zero;
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    switch (widget.animationType) {
      case AnimationType.fadeIn:
        return FadeTransition(
          opacity: _fadeAnimation,
          child: widget.child,
        );
      
      case AnimationType.slideUp:
      case AnimationType.slideDown:
      case AnimationType.slideLeft:
      case AnimationType.slideRight:
        return SlideTransition(
          position: _slideAnimation,
          child: widget.child,
        );
      
      case AnimationType.fadeSlideUp:
      case AnimationType.fadeSlideDown:
        return SlideTransition(
          position: _slideAnimation,
          child: FadeTransition(
            opacity: _fadeAnimation,
            child: widget.child,
          ),
        );
      
      case AnimationType.scaleUp:
        return ScaleTransition(
          scale: _scaleAnimation,
          child: widget.child,
        );
      
      case AnimationType.rotation:
        return AnimatedBuilder(
          animation: _rotationAnimation,
          builder: (context, child) {
            return Transform.rotate(
              angle: _rotationAnimation.value * 2 * math.pi,
              child: FadeTransition(
                opacity: _fadeAnimation,
                child: widget.child,
              ),
            );
          },
        );
      
      case AnimationType.bounce:
        return AnimatedBuilder(
          animation: _controller,
          builder: (context, child) {
            final bounceValue = Curves.elasticOut.transform(_controller.value);
            return Transform.scale(
              scale: bounceValue,
              child: FadeTransition(
                opacity: _fadeAnimation,
                child: widget.child,
              ),
            );
          },
        );
    }
  }
}

class StaggeredList extends StatefulWidget {
  final List<Widget> children;
  final Duration staggerDelay;
  final Duration itemDuration;
  final AnimationType animationType;
  final Axis direction;

  const StaggeredList({
    super.key,
    required this.children,
    this.staggerDelay = const Duration(milliseconds: 100),
    this.itemDuration = const Duration(milliseconds: 600),
    this.animationType = AnimationType.fadeSlideUp,
    this.direction = Axis.vertical,
  });

  @override
  State<StaggeredList> createState() => _StaggeredListState();
}

class _StaggeredListState extends State<StaggeredList> {
  @override
  Widget build(BuildContext context) {
    return direction == Axis.vertical
        ? Column(
            children: _buildAnimatedChildren(),
          )
        : Row(
            children: _buildAnimatedChildren(),
          );
  }

  List<Widget> _buildAnimatedChildren() {
    return widget.children.asMap().entries.map((entry) {
      final index = entry.key;
      final child = entry.value;
      
      return AnimatedEntry(
        delay: Duration(milliseconds: index * widget.staggerDelay.inMilliseconds),
        duration: widget.itemDuration,
        animationType: widget.animationType,
        child: child,
      );
    }).toList();
  }

  Axis get direction => widget.direction;
}

class PulseAnimation extends StatefulWidget {
  final Widget child;
  final Duration duration;
  final double minScale;
  final double maxScale;
  final bool infinite;

  const PulseAnimation({
    super.key,
    required this.child,
    this.duration = const Duration(seconds: 1),
    this.minScale = 0.95,
    this.maxScale = 1.05,
    this.infinite = true,
  });

  @override
  State<PulseAnimation> createState() => _PulseAnimationState();
}

class _PulseAnimationState extends State<PulseAnimation>
    with SingleTickerProviderStateMixin {
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
      begin: widget.minScale,
      end: widget.maxScale,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    ));

    if (widget.infinite) {
      _controller.repeat(reverse: true);
    } else {
      _controller.forward();
    }
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
          child: widget.child,
        );
      },
    );
  }
}

class ShimmerEffect extends StatefulWidget {
  final Widget child;
  final Color? highlightColor;
  final Color? baseColor;
  final Duration duration;

  const ShimmerEffect({
    super.key,
    required this.child,
    this.highlightColor,
    this.baseColor,
    this.duration = const Duration(milliseconds: 1500),
  });

  @override
  State<ShimmerEffect> createState() => _ShimmerEffectState();
}

class _ShimmerEffectState extends State<ShimmerEffect>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();
    
    _controller = AnimationController(
      duration: widget.duration,
      vsync: this,
    )..repeat();

    _animation = Tween<double>(
      begin: -2,
      end: 2,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOutSine,
    ));
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final baseColor = widget.baseColor ?? theme.colorScheme.surface;
    final highlightColor = widget.highlightColor ?? 
        theme.colorScheme.onSurface.withOpacity(0.1);

    return AnimatedBuilder(
      animation: _animation,
      builder: (context, child) {
        return ShaderMask(
          blendMode: BlendMode.srcATop,
          shaderCallback: (rect) {
            return LinearGradient(
              begin: Alignment.topLeft,
              end: Alignment.bottomRight,
              colors: [
                baseColor,
                highlightColor,
                baseColor,
              ],
              stops: [
                math.max(0.0, _animation.value - 0.3),
                _animation.value,
                math.min(1.0, _animation.value + 0.3),
              ],
              transform: GradientRotation(_animation.value),
            ).createShader(rect);
          },
          child: widget.child,
        );
      },
    );
  }
}

class AnimationDemoPage extends StatefulWidget {
  const AnimationDemoPage({super.key});

  @override
  State<AnimationDemoPage> createState() => _AnimationDemoPageState();
}

class _AnimationDemoPageState extends State<AnimationDemoPage> {
  bool _showAnimations = false;

  @override
  void initState() {
    super.initState();
    // Start animations after a short delay
    Future.delayed(const Duration(milliseconds: 300), () {
      if (mounted) {
        setState(() {
          _showAnimations = true;
        });
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Animation Widgets'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: () {
              setState(() {
                _showAnimations = false;
              });
              Future.delayed(const Duration(milliseconds: 100), () {
                if (mounted) {
                  setState(() {
                    _showAnimations = true;
                  });
                }
              });
            },
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            if (_showAnimations) ...[
              AnimatedEntry(
                animationType: AnimationType.fadeSlideUp,
                duration: const Duration(milliseconds: 800),
                child: Text(
                  'Animation Types',
                  style: Theme.of(context).textTheme.headlineSmall,
                ),
              ),
              
              const SizedBox(height: 20),
              
              // Animation types grid
              StaggeredList(
                staggerDelay: const Duration(milliseconds: 150),
                children: [
                  _buildAnimationCard('Fade In', AnimationType.fadeIn),
                  _buildAnimationCard('Slide Up', AnimationType.slideUp),
                  _buildAnimationCard('Scale Up', AnimationType.scaleUp),
                  _buildAnimationCard('Bounce', AnimationType.bounce),
                ],
              ),
              
              const SizedBox(height: 32),
              
              AnimatedEntry(
                delay: const Duration(milliseconds: 800),
                animationType: AnimationType.fadeSlideUp,
                child: Text(
                  'Staggered List Example',
                  style: Theme.of(context).textTheme.headlineSmall,
                ),
              ),
              
              const SizedBox(height: 20),
              
              StaggeredList(
                staggerDelay: const Duration(milliseconds: 100),
                animationType: AnimationType.fadeSlideLeft,
                children: List.generate(5, (index) {
                  return Card(
                    margin: const EdgeInsets.only(bottom: 8),
                    child: ListTile(
                      leading: CircleAvatar(
                        backgroundColor: Colors.purple.shade100,
                        child: Text('${index + 1}'),
                      ),
                      title: Text('Staggered Item ${index + 1}'),
                      subtitle: const Text('Each item animates with a delay'),
                      trailing: const Icon(Icons.arrow_forward_ios),
                    ),
                  );
                }),
              ),
              
              const SizedBox(height: 32),
              
              AnimatedEntry(
                delay: const Duration(milliseconds: 1500),
                animationType: AnimationType.fadeSlideUp,
                child: Text(
                  'Special Effects',
                  style: Theme.of(context).textTheme.headlineSmall,
                ),
              ),
              
              const SizedBox(height: 20),
              
              AnimatedEntry(
                delay: const Duration(milliseconds: 1800),
                animationType: AnimationType.scaleUp,
                child: Row(
                  children: [
                    Expanded(
                      child: Card(
                        child: Padding(
                          padding: const EdgeInsets.all(16),
                          child: Column(
                            children: [
                              PulseAnimation(
                                child: Container(
                                  width: 60,
                                  height: 60,
                                  decoration: BoxDecoration(
                                    color: Colors.red.shade100,
                                    shape: BoxShape.circle,
                                  ),
                                  child: const Icon(
                                    Icons.favorite,
                                    color: Colors.red,
                                    size: 30,
                                  ),
                                ),
                              ),
                              const SizedBox(height: 12),
                              const Text('Pulse Animation'),
                            ],
                          ),
                        ),
                      ),
                    ),
                    const SizedBox(width: 16),
                    Expanded(
                      child: Card(
                        child: Padding(
                          padding: const EdgeInsets.all(16),
                          child: Column(
                            children: [
                              ShimmerEffect(
                                child: Container(
                                  width: 60,
                                  height: 60,
                                  decoration: BoxDecoration(
                                    color: Colors.blue.shade100,
                                    borderRadius: BorderRadius.circular(8),
                                  ),
                                  child: const Icon(
                                    Icons.auto_awesome,
                                    color: Colors.blue,
                                    size: 30,
                                  ),
                                ),
                              ),
                              const SizedBox(height: 12),
                              const Text('Shimmer Effect'),
                            ],
                          ),
                        ),
                      ),
                    ),
                  ],
                ),
              ),
              
              const SizedBox(height: 20),
              
              // Complex animated layout
              AnimatedEntry(
                delay: const Duration(milliseconds: 2200),
                animationType: AnimationType.fadeSlideUp,
                child: Card(
                  child: Padding(
                    padding: const EdgeInsets.all(20),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Row(
                          children: [
                            PulseAnimation(
                              duration: const Duration(milliseconds: 800),
                              child: Container(
                                width: 12,
                                height: 12,
                                decoration: const BoxDecoration(
                                  color: Colors.green,
                                  shape: BoxShape.circle,
                                ),
                              ),
                            ),
                            const SizedBox(width: 8),
                            const Text(
                              'Live Animation Demo',
                              style: TextStyle(
                                fontSize: 18,
                                fontWeight: FontWeight.bold,
                              ),
                            ),
                          ],
                        ),
                        const SizedBox(height: 16),
                        const Text(
                          'This section demonstrates multiple animations working together:',
                        ),
                        const SizedBox(height: 12),
                        Row(
                          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                          children: [
                            AnimatedEntry(
                              delay: const Duration(milliseconds: 2500),
                              animationType: AnimationType.rotation,
                              child: Container(
                                width: 40,
                                height: 40,
                                decoration: BoxDecoration(
                                  color: Colors.orange.shade100,
                                  borderRadius: BorderRadius.circular(8),
                                ),
                                child: const Icon(Icons.refresh, color: Colors.orange),
                              ),
                            ),
                            AnimatedEntry(
                              delay: const Duration(milliseconds: 2700),
                              animationType: AnimationType.bounce,
                              child: Container(
                                width: 40,
                                height: 40,
                                decoration: BoxDecoration(
                                  color: Colors.purple.shade100,
                                  borderRadius: BorderRadius.circular(8),
                                ),
                                child: const Icon(Icons.star, color: Colors.purple),
                              ),
                            ),
                            AnimatedEntry(
                              delay: const Duration(milliseconds: 2900),
                              animationType: AnimationType.scaleUp,
                              child: Container(
                                width: 40,
                                height: 40,
                                decoration: BoxDecoration(
                                  color: Colors.teal.shade100,
                                  borderRadius: BorderRadius.circular(8),
                                ),
                                child: const Icon(Icons.healing, color: Colors.teal),
                              ),
                            ),
                          ],
                        ),
                      ],
                    ),
                  ),
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }

  Widget _buildAnimationCard(String title, AnimationType type) {
    return Card(
      child: InkWell(
        onTap: () {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(content: Text('$title animation tapped')),
          );
        },
        borderRadius: BorderRadius.circular(12),
        child: Padding(
          padding: const EdgeInsets.all(16),
          child: Column(
            children: [
              AnimatedEntry(
                animationType: type,
                delay: const Duration(milliseconds: 500),
                child: Container(
                  width: 50,
                  height: 50,
                  decoration: BoxDecoration(
                    color: Theme.of(context).colorScheme.primaryContainer,
                    borderRadius: BorderRadius.circular(12),
                  ),
                  child: Icon(
                    Icons.animation,
                    color: Theme.of(context).colorScheme.onPrimaryContainer,
                  ),
                ),
              ),
              const SizedBox(height: 12),
              Text(
                title,
                style: Theme.of(context).textTheme.titleMedium,
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

This comprehensive animation system provides reusable animated widgets with  
multiple transition types, staggered animations, and special effects like  
pulse and shimmer. Components can be easily configured with different  
timing, curves, and animation styles for rich user interactions.  

## Custom Progress Indicator

Creating sophisticated progress indicators with theme integration.  

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
      title: 'Custom Progress Indicators',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepOrange),
      ),
      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.deepOrange,
          brightness: Brightness.dark,
        ),
      ),
      home: const ProgressIndicatorDemoPage(),
    );
  }
}

class CustomProgressIndicator extends StatefulWidget {
  final double value;
  final double size;
  final double strokeWidth;
  final Color? backgroundColor;
  final Color? foregroundColor;
  final Gradient? gradient;
  final ProgressStyle style;
  final bool showPercentage;
  final TextStyle? textStyle;
  final Widget? child;
  final Duration animationDuration;

  const CustomProgressIndicator({
    super.key,
    required this.value,
    this.size = 80,
    this.strokeWidth = 8,
    this.backgroundColor,
    this.foregroundColor,
    this.gradient,
    this.style = ProgressStyle.circular,
    this.showPercentage = false,
    this.textStyle,
    this.child,
    this.animationDuration = const Duration(milliseconds: 300),
  });

  @override
  State<CustomProgressIndicator> createState() => _CustomProgressIndicatorState();
}

enum ProgressStyle { circular, linear, ring, wave, dots }

class _CustomProgressIndicatorState extends State<CustomProgressIndicator>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late AnimationController _waveController;
  late Animation<double> _animation;
  double _currentValue = 0;

  @override
  void initState() {
    super.initState();
    
    _controller = AnimationController(
      duration: widget.animationDuration,
      vsync: this,
    );
    
    _waveController = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );

    _animation = Tween<double>(
      begin: _currentValue,
      end: widget.value,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeOutCubic,
    ));

    if (widget.style == ProgressStyle.wave) {
      _waveController.repeat();
    }

    _controller.forward();
  }

  @override
  void didUpdateWidget(CustomProgressIndicator oldWidget) {
    super.didUpdateWidget(oldWidget);
    
    if (oldWidget.value != widget.value) {
      _animation = Tween<double>(
        begin: _currentValue,
        end: widget.value,
      ).animate(CurvedAnimation(
        parent: _controller,
        curve: Curves.easeOutCubic,
      ));
      
      _controller.reset();
      _controller.forward();
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    _waveController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final colorScheme = theme.colorScheme;
    
    final backgroundColor = widget.backgroundColor ?? 
        colorScheme.outline.withOpacity(0.3);
    final foregroundColor = widget.foregroundColor ?? 
        colorScheme.primary;

    return AnimatedBuilder(
      animation: _animation,
      builder: (context, child) {
        _currentValue = _animation.value;
        
        switch (widget.style) {
          case ProgressStyle.circular:
            return _buildCircularProgress(backgroundColor, foregroundColor);
          case ProgressStyle.linear:
            return _buildLinearProgress(backgroundColor, foregroundColor);
          case ProgressStyle.ring:
            return _buildRingProgress(backgroundColor, foregroundColor);
          case ProgressStyle.wave:
            return _buildWaveProgress(backgroundColor, foregroundColor);
          case ProgressStyle.dots:
            return _buildDotsProgress(backgroundColor, foregroundColor);
        }
      },
    );
  }

  Widget _buildCircularProgress(Color backgroundColor, Color foregroundColor) {
    return SizedBox(
      width: widget.size,
      height: widget.size,
      child: Stack(
        alignment: Alignment.center,
        children: [
          SizedBox(
            width: widget.size,
            height: widget.size,
            child: CircularProgressIndicator(
              value: 1.0,
              strokeWidth: widget.strokeWidth,
              valueColor: AlwaysStoppedAnimation<Color>(backgroundColor),
            ),
          ),
          SizedBox(
            width: widget.size,
            height: widget.size,
            child: widget.gradient != null
                ? CustomPaint(
                    painter: GradientCircularProgressPainter(
                      progress: _currentValue,
                      gradient: widget.gradient!,
                      strokeWidth: widget.strokeWidth,
                    ),
                  )
                : CircularProgressIndicator(
                    value: _currentValue,
                    strokeWidth: widget.strokeWidth,
                    valueColor: AlwaysStoppedAnimation<Color>(foregroundColor),
                  ),
          ),
          if (widget.showPercentage)
            Text(
              '${(_currentValue * 100).round()}%',
              style: widget.textStyle ?? 
                  TextStyle(
                    fontWeight: FontWeight.bold,
                    fontSize: widget.size * 0.15,
                  ),
            ),
          if (widget.child != null) widget.child!,
        ],
      ),
    );
  }

  Widget _buildLinearProgress(Color backgroundColor, Color foregroundColor) {
    return Container(
      width: widget.size,
      height: widget.strokeWidth,
      decoration: BoxDecoration(
        color: backgroundColor,
        borderRadius: BorderRadius.circular(widget.strokeWidth / 2),
      ),
      child: FractionallySizedBox(
        alignment: Alignment.centerLeft,
        widthFactor: _currentValue,
        child: Container(
          decoration: BoxDecoration(
            color: foregroundColor,
            gradient: widget.gradient,
            borderRadius: BorderRadius.circular(widget.strokeWidth / 2),
          ),
        ),
      ),
    );
  }

  Widget _buildRingProgress(Color backgroundColor, Color foregroundColor) {
    return SizedBox(
      width: widget.size,
      height: widget.size,
      child: Stack(
        alignment: Alignment.center,
        children: [
          CustomPaint(
            size: Size(widget.size, widget.size),
            painter: RingProgressPainter(
              progress: _currentValue,
              backgroundColor: backgroundColor,
              foregroundColor: foregroundColor,
              strokeWidth: widget.strokeWidth,
            ),
          ),
          if (widget.showPercentage)
            Text(
              '${(_currentValue * 100).round()}%',
              style: widget.textStyle ?? 
                  TextStyle(
                    fontWeight: FontWeight.bold,
                    fontSize: widget.size * 0.12,
                  ),
            ),
        ],
      ),
    );
  }

  Widget _buildWaveProgress(Color backgroundColor, Color foregroundColor) {
    return AnimatedBuilder(
      animation: _waveController,
      builder: (context, child) {
        return SizedBox(
          width: widget.size,
          height: widget.size,
          child: CustomPaint(
            painter: WaveProgressPainter(
              progress: _currentValue,
              waveProgress: _waveController.value,
              backgroundColor: backgroundColor,
              foregroundColor: foregroundColor,
            ),
          ),
        );
      },
    );
  }

  Widget _buildDotsProgress(Color backgroundColor, Color foregroundColor) {
    return SizedBox(
      width: widget.size,
      height: widget.strokeWidth,
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: List.generate(5, (index) {
          final dotProgress = ((_currentValue * 5) - index).clamp(0.0, 1.0);
          return Container(
            width: widget.strokeWidth,
            height: widget.strokeWidth,
            decoration: BoxDecoration(
              shape: BoxShape.circle,
              color: Color.lerp(
                backgroundColor,
                foregroundColor,
                dotProgress,
              ),
            ),
          );
        }),
      ),
    );
  }
}

class GradientCircularProgressPainter extends CustomPainter {
  final double progress;
  final Gradient gradient;
  final double strokeWidth;

  GradientCircularProgressPainter({
    required this.progress,
    required this.gradient,
    required this.strokeWidth,
  });

  @override
  void paint(Canvas canvas, Size size) {
    final center = Offset(size.width / 2, size.height / 2);
    final radius = (size.width - strokeWidth) / 2;
    
    final rect = Rect.fromCircle(center: center, radius: radius);
    final paint = Paint()
      ..shader = gradient.createShader(rect)
      ..strokeCap = StrokeCap.round
      ..style = PaintingStyle.stroke
      ..strokeWidth = strokeWidth;

    const startAngle = -math.pi / 2;
    final sweepAngle = progress * 2 * math.pi;

    canvas.drawArc(
      rect,
      startAngle,
      sweepAngle,
      false,
      paint,
    );
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

class RingProgressPainter extends CustomPainter {
  final double progress;
  final Color backgroundColor;
  final Color foregroundColor;
  final double strokeWidth;

  RingProgressPainter({
    required this.progress,
    required this.backgroundColor,
    required this.foregroundColor,
    required this.strokeWidth,
  });

  @override
  void paint(Canvas canvas, Size size) {
    final center = Offset(size.width / 2, size.height / 2);
    final radius = (size.width - strokeWidth) / 2;
    
    // Background ring
    final backgroundPaint = Paint()
      ..color = backgroundColor
      ..strokeCap = StrokeCap.round
      ..style = PaintingStyle.stroke
      ..strokeWidth = strokeWidth;
    
    canvas.drawCircle(center, radius, backgroundPaint);

    // Progress ring with segments
    const segments = 20;
    final segmentAngle = (2 * math.pi) / segments;
    final progressSegments = (progress * segments).floor();
    
    final foregroundPaint = Paint()
      ..color = foregroundColor
      ..strokeCap = StrokeCap.round
      ..style = PaintingStyle.stroke
      ..strokeWidth = strokeWidth;

    for (int i = 0; i < progressSegments; i++) {
      final startAngle = -math.pi / 2 + (i * segmentAngle);
      final sweepAngle = segmentAngle * 0.8; // Small gap between segments
      
      canvas.drawArc(
        Rect.fromCircle(center: center, radius: radius),
        startAngle,
        sweepAngle,
        false,
        foregroundPaint,
      );
    }
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

class WaveProgressPainter extends CustomPainter {
  final double progress;
  final double waveProgress;
  final Color backgroundColor;
  final Color foregroundColor;

  WaveProgressPainter({
    required this.progress,
    required this.waveProgress,
    required this.backgroundColor,
    required this.foregroundColor,
  });

  @override
  void paint(Canvas canvas, Size size) {
    final backgroundPaint = Paint()
      ..color = backgroundColor
      ..style = PaintingStyle.fill;
    
    canvas.drawRRect(
      RRect.fromRectAndRadius(
        Rect.fromLTWH(0, 0, size.width, size.height),
        const Radius.circular(8),
      ),
      backgroundPaint,
    );

    if (progress > 0) {
      final progressHeight = size.height * progress;
      final waveHeight = 4.0;
      final waveLength = size.width;
      
      final path = Path();
      path.moveTo(0, size.height);
      path.lineTo(0, size.height - progressHeight + waveHeight);
      
      for (double x = 0; x <= size.width; x++) {
        final y = size.height - progressHeight + 
                  math.sin((x / waveLength * 2 * math.pi) + (waveProgress * 2 * math.pi)) * waveHeight;
        path.lineTo(x, y);
      }
      
      path.lineTo(size.width, size.height);
      path.close();

      final foregroundPaint = Paint()
        ..color = foregroundColor
        ..style = PaintingStyle.fill;
      
      canvas.drawPath(path, foregroundPaint);
    }
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

class ProgressIndicatorDemoPage extends StatefulWidget {
  const ProgressIndicatorDemoPage({super.key};

  @override
  State<ProgressIndicatorDemoPage> createState() => 
      _ProgressIndicatorDemoPageState();
}

class _ProgressIndicatorDemoPageState extends State<ProgressIndicatorDemoPage> {
  double _progressValue = 0.7;
  bool _isAnimating = false;

  void _simulateProgress() async {
    setState(() {
      _isAnimating = true;
      _progressValue = 0;
    });

    for (double i = 0; i <= 1.0; i += 0.1) {
      await Future.delayed(const Duration(milliseconds: 200));
      if (mounted) {
        setState(() {
          _progressValue = i;
        });
      }
    }

    setState(() {
      _isAnimating = false;
    });
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Progress Indicators'),
        backgroundColor: theme.colorScheme.inversePrimary,
        actions: [
          IconButton(
            icon: const Icon(Icons.play_arrow),
            onPressed: _isAnimating ? null : _simulateProgress,
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Progress Value: ${(_progressValue * 100).toInt()}%',
              style: theme.textTheme.headlineSmall,
            ),
            
            const SizedBox(height: 16),
            
            Slider(
              value: _progressValue,
              onChanged: _isAnimating ? null : (value) {
                setState(() {
                  _progressValue = value;
                });
              },
              divisions: 100,
              label: '${(_progressValue * 100).round()}%',
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Circular Progress',
              style: theme.textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            
            Wrap(
              spacing: 20,
              runSpacing: 20,
              children: [
                Column(
                  children: [
                    CustomProgressIndicator(
                      value: _progressValue,
                      style: ProgressStyle.circular,
                      showPercentage: true,
                      size: 100,
                    ),
                    const SizedBox(height: 8),
                    const Text('Standard'),
                  ],
                ),
                Column(
                  children: [
                    CustomProgressIndicator(
                      value: _progressValue,
                      style: ProgressStyle.circular,
                      size: 100,
                      gradient: LinearGradient(
                        colors: [
                          theme.colorScheme.primary,
                          theme.colorScheme.secondary,
                        ],
                      ),
                      child: Icon(
                        Icons.download,
                        color: theme.colorScheme.primary,
                      ),
                    ),
                    const SizedBox(height: 8),
                    const Text('Gradient'),
                  ],
                ),
                Column(
                  children: [
                    CustomProgressIndicator(
                      value: _progressValue,
                      style: ProgressStyle.ring,
                      size: 100,
                      strokeWidth: 12,
                      showPercentage: true,
                    ),
                    const SizedBox(height: 8),
                    const Text('Segmented'),
                  ],
                ),
              ],
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Linear Progress',
              style: theme.textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            
            Column(
              children: [
                Row(
                  children: [
                    const Text('Standard: '),
                    Expanded(
                      child: CustomProgressIndicator(
                        value: _progressValue,
                        style: ProgressStyle.linear,
                        size: double.infinity,
                        strokeWidth: 8,
                      ),
                    ),
                    const SizedBox(width: 8),
                    Text('${(_progressValue * 100).toInt()}%'),
                  ],
                ),
                
                const SizedBox(height: 16),
                
                Row(
                  children: [
                    const Text('Gradient: '),
                    Expanded(
                      child: CustomProgressIndicator(
                        value: _progressValue,
                        style: ProgressStyle.linear,
                        size: double.infinity,
                        strokeWidth: 12,
                        gradient: LinearGradient(
                          colors: [
                            Colors.orange,
                            Colors.red,
                          ],
                        ),
                      ),
                    ),
                    const SizedBox(width: 8),
                    Text('${(_progressValue * 100).toInt()}%'),
                  ],
                ),
                
                const SizedBox(height: 16),
                
                Row(
                  children: [
                    const Text('Dots: '),
                    Expanded(
                      child: CustomProgressIndicator(
                        value: _progressValue,
                        style: ProgressStyle.dots,
                        size: double.infinity,
                        strokeWidth: 12,
                      ),
                    ),
                    const SizedBox(width: 8),
                    Text('${(_progressValue * 100).toInt()}%'),
                  ],
                ),
              ],
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Special Effects',
              style: theme.textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            
            Row(
              children: [
                Expanded(
                  child: Column(
                    children: [
                      CustomProgressIndicator(
                        value: _progressValue,
                        style: ProgressStyle.wave,
                        size: 120,
                      ),
                      const SizedBox(height: 8),
                      const Text('Wave Progress'),
                    ],
                  ),
                ),
                const SizedBox(width: 20),
                Expanded(
                  child: Column(
                    children: [
                      Container(
                        width: 120,
                        height: 120,
                        padding: const EdgeInsets.all(16),
                        decoration: BoxDecoration(
                          color: theme.colorScheme.primaryContainer,
                          borderRadius: BorderRadius.circular(16),
                        ),
                        child: CustomProgressIndicator(
                          value: _progressValue,
                          style: ProgressStyle.circular,
                          size: 80,
                          strokeWidth: 6,
                          showPercentage: true,
                          textStyle: TextStyle(
                            color: theme.colorScheme.onPrimaryContainer,
                            fontWeight: FontWeight.bold,
                          ),
                          foregroundColor: theme.colorScheme.primary,
                        ),
                      ),
                      const SizedBox(height: 8),
                      const Text('Themed Container'),
                    ],
                  ),
                ),
              ],
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Real-world Examples',
              style: theme.textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'File Download Progress',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 8),
                    const Text('document.pdf • 2.4 MB'),
                    const SizedBox(height: 12),
                    CustomProgressIndicator(
                      value: _progressValue,
                      style: ProgressStyle.linear,
                      size: double.infinity,
                      strokeWidth: 6,
                      gradient: const LinearGradient(
                        colors: [Colors.blue, Colors.cyan],
                      ),
                    ),
                    const SizedBox(height: 8),
                    Row(
                      mainAxisAlignment: MainAxisAlignment.spaceBetween,
                      children: [
                        Text('${(_progressValue * 2.4).toStringAsFixed(1)} MB'),
                        Text('${(_progressValue * 100).toInt()}%'),
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
                child: Row(
                  children: [
                    CustomProgressIndicator(
                      value: _progressValue,
                      style: ProgressStyle.circular,
                      size: 60,
                      strokeWidth: 4,
                      showPercentage: true,
                      textStyle: const TextStyle(fontSize: 10),
                    ),
                    const SizedBox(width: 16),
                    Expanded(
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          const Text(
                            'System Update',
                            style: TextStyle(fontWeight: FontWeight.bold),
                          ),
                          const SizedBox(height: 4),
                          Text(
                            'Installing update ${(_progressValue * 100).toInt()}% complete',
                            style: theme.textTheme.bodySmall,
                          ),
                          const SizedBox(height: 8),
                          CustomProgressIndicator(
                            value: _progressValue,
                            style: ProgressStyle.linear,
                            size: double.infinity,
                            strokeWidth: 4,
                          ),
                        ],
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
}
```

This custom progress indicator system provides multiple styles including  
circular, linear, ring, wave, and dots with gradient support, animations,  
and theme integration. The indicators can display percentages, custom  
content, and work seamlessly in real-world scenarios like downloads.  

## Custom Slider Widget

Creating advanced slider components with rich customization options.  

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
      title: 'Custom Slider Widgets',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.green),
      ),
      home: const CustomSliderDemoPage(),
    );
  }
}

class CustomSlider extends StatefulWidget {
  final double value;
  final ValueChanged<double>? onChanged;
  final ValueChanged<double>? onChangeEnd;
  final double min;
  final double max;
  final int? divisions;
  final String? label;
  final Color? activeColor;
  final Color? inactiveColor;
  final Color? thumbColor;
  final SliderStyle style;
  final double height;
  final bool showLabels;
  final List<String>? customLabels;

  const CustomSlider({
    super.key,
    required this.value,
    this.onChanged,
    this.onChangeEnd,
    this.min = 0.0,
    this.max = 1.0,
    this.divisions,
    this.label,
    this.activeColor,
    this.inactiveColor,
    this.thumbColor,
    this.style = SliderStyle.material,
    this.height = 40,
    this.showLabels = false,
    this.customLabels,
  });

  @override
  State<CustomSlider> createState() => _CustomSliderState();
}

enum SliderStyle { material, cupertino, custom, gradient, segmented }

class _CustomSliderState extends State<CustomSlider> {
  late double _currentValue;

  @override
  void initState() {
    super.initState();
    _currentValue = widget.value;
  }

  @override
  void didUpdateWidget(CustomSlider oldWidget) {
    super.didUpdateWidget(oldWidget);
    if (oldWidget.value != widget.value) {
      _currentValue = widget.value;
    }
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    switch (widget.style) {
      case SliderStyle.material:
        return _buildMaterialSlider(theme);
      case SliderStyle.cupertino:
        return _buildCupertinoSlider(theme);
      case SliderStyle.custom:
        return _buildCustomSlider(theme);
      case SliderStyle.gradient:
        return _buildGradientSlider(theme);
      case SliderStyle.segmented:
        return _buildSegmentedSlider(theme);
    }
  }

  Widget _buildMaterialSlider(ThemeData theme) {
    return Column(
      children: [
        if (widget.showLabels)
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              Text(widget.min.toString()),
              if (widget.label != null)
                Text(widget.label!)
              else
                Text(_currentValue.toStringAsFixed(1)),
              Text(widget.max.toString()),
            ],
          ),
        Slider(
          value: _currentValue,
          onChanged: (value) {
            setState(() {
              _currentValue = value;
            });
            widget.onChanged?.call(value);
          },
          onChangeEnd: widget.onChangeEnd,
          min: widget.min,
          max: widget.max,
          divisions: widget.divisions,
          label: widget.label,
          activeColor: widget.activeColor,
          inactiveColor: widget.inactiveColor,
          thumbColor: widget.thumbColor,
        ),
      ],
    );
  }

  Widget _buildCupertinoSlider(ThemeData theme) {
    return Column(
      children: [
        if (widget.showLabels)
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 16),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text(widget.min.toString()),
                Text(_currentValue.toStringAsFixed(1)),
                Text(widget.max.toString()),
              ],
            ),
          ),
        Container(
          height: widget.height,
          padding: const EdgeInsets.symmetric(horizontal: 16),
          child: SliderTheme(
            data: SliderTheme.of(context).copyWith(
              trackHeight: 4,
              thumbShape: const RoundSliderThumbShape(
                enabledThumbRadius: 8,
              ),
              overlayShape: const RoundSliderOverlayShape(
                overlayRadius: 16,
              ),
            ),
            child: Slider(
              value: _currentValue,
              onChanged: (value) {
                setState(() {
                  _currentValue = value;
                });
                widget.onChanged?.call(value);
              },
              onChangeEnd: widget.onChangeEnd,
              min: widget.min,
              max: widget.max,
              activeColor: widget.activeColor ?? theme.colorScheme.primary,
              inactiveColor: widget.inactiveColor ?? 
                  theme.colorScheme.outline.withOpacity(0.3),
            ),
          ),
        ),
      ],
    );
  }

  Widget _buildCustomSlider(ThemeData theme) {
    return Container(
      height: widget.height,
      padding: const EdgeInsets.symmetric(horizontal: 16),
      child: CustomPaint(
        size: Size(double.infinity, widget.height),
        painter: CustomSliderPainter(
          value: _currentValue,
          min: widget.min,
          max: widget.max,
          activeColor: widget.activeColor ?? theme.colorScheme.primary,
          inactiveColor: widget.inactiveColor ?? 
              theme.colorScheme.outline.withOpacity(0.3),
          thumbColor: widget.thumbColor ?? theme.colorScheme.primary,
        ),
        child: GestureDetector(
          onPanStart: _handlePanStart,
          onPanUpdate: _handlePanUpdate,
          onPanEnd: _handlePanEnd,
        ),
      ),
    );
  }

  Widget _buildGradientSlider(ThemeData theme) {
    return Container(
      height: widget.height,
      padding: const EdgeInsets.symmetric(horizontal: 16),
      child: CustomPaint(
        size: Size(double.infinity, widget.height),
        painter: GradientSliderPainter(
          value: _currentValue,
          min: widget.min,
          max: widget.max,
          gradient: LinearGradient(
            colors: [
              theme.colorScheme.primary,
              theme.colorScheme.secondary,
              theme.colorScheme.tertiary,
            ],
          ),
          inactiveColor: theme.colorScheme.outline.withOpacity(0.3),
          thumbColor: Colors.white,
        ),
        child: GestureDetector(
          onPanStart: _handlePanStart,
          onPanUpdate: _handlePanUpdate,
          onPanEnd: _handlePanEnd,
        ),
      ),
    );
  }

  Widget _buildSegmentedSlider(ThemeData theme) {
    final segments = widget.divisions ?? 5;
    final segmentWidth = (MediaQuery.of(context).size.width - 64) / segments;
    
    return Column(
      children: [
        Container(
          height: widget.height,
          padding: const EdgeInsets.symmetric(horizontal: 16),
          child: Row(
            children: List.generate(segments, (index) {
              final segmentValue = widget.min + 
                  (index * (widget.max - widget.min) / (segments - 1));
              final isActive = _currentValue >= segmentValue;
              
              return GestureDetector(
                onTap: () {
                  setState(() {
                    _currentValue = segmentValue;
                  });
                  widget.onChanged?.call(segmentValue);
                },
                child: Container(
                  width: segmentWidth,
                  height: 8,
                  margin: const EdgeInsets.symmetric(horizontal: 2),
                  decoration: BoxDecoration(
                    color: isActive 
                        ? (widget.activeColor ?? theme.colorScheme.primary)
                        : (widget.inactiveColor ?? 
                           theme.colorScheme.outline.withOpacity(0.3)),
                    borderRadius: BorderRadius.circular(4),
                  ),
                ),
              );
            }),
          ),
        ),
        if (widget.customLabels != null)
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 16),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: widget.customLabels!.map((label) => 
                  Text(label, style: theme.textTheme.bodySmall)).toList(),
            ),
          ),
      ],
    );
  }

  void _handlePanStart(DragStartDetails details) {
    _updateValueFromPosition(details.localPosition);
  }

  void _handlePanUpdate(DragUpdateDetails details) {
    _updateValueFromPosition(details.localPosition);
  }

  void _handlePanEnd(DragEndDetails details) {
    widget.onChangeEnd?.call(_currentValue);
  }

  void _updateValueFromPosition(Offset position) {
    final RenderBox renderBox = context.findRenderObject() as RenderBox;
    final width = renderBox.size.width - 32; // Account for padding
    final ratio = (position.dx - 16).clamp(0.0, width) / width;
    final newValue = widget.min + (ratio * (widget.max - widget.min));
    
    setState(() {
      _currentValue = newValue.clamp(widget.min, widget.max);
    });
    
    widget.onChanged?.call(_currentValue);
  }
}

class CustomSliderPainter extends CustomPainter {
  final double value;
  final double min;
  final double max;
  final Color activeColor;
  final Color inactiveColor;
  final Color thumbColor;

  CustomSliderPainter({
    required this.value,
    required this.min,
    required this.max,
    required this.activeColor,
    required this.inactiveColor,
    required this.thumbColor,
  });

  @override
  void paint(Canvas canvas, Size size) {
    final trackY = size.height / 2;
    final trackHeight = 6.0;
    final thumbRadius = 12.0;
    
    final progress = (value - min) / (max - min);
    final thumbX = progress * size.width;

    // Draw inactive track
    final inactivePaint = Paint()
      ..color = inactiveColor
      ..strokeCap = StrokeCap.round;
    
    canvas.drawRRect(
      RRect.fromRectAndRadius(
        Rect.fromLTWH(0, trackY - trackHeight / 2, size.width, trackHeight),
        Radius.circular(trackHeight / 2),
      ),
      inactivePaint,
    );

    // Draw active track
    final activePaint = Paint()
      ..color = activeColor
      ..strokeCap = StrokeCap.round;
    
    canvas.drawRRect(
      RRect.fromRectAndRadius(
        Rect.fromLTWH(0, trackY - trackHeight / 2, thumbX, trackHeight),
        Radius.circular(trackHeight / 2),
      ),
      activePaint,
    );

    // Draw thumb
    final thumbPaint = Paint()
      ..color = thumbColor
      ..style = PaintingStyle.fill;
    
    final shadowPaint = Paint()
      ..color = Colors.black.withOpacity(0.2)
      ..maskFilter = const MaskFilter.blur(BlurStyle.normal, 4);
    
    canvas.drawCircle(
      Offset(thumbX, trackY + 1),
      thumbRadius,
      shadowPaint,
    );
    
    canvas.drawCircle(
      Offset(thumbX, trackY),
      thumbRadius,
      thumbPaint,
    );
    
    // Inner thumb circle
    final innerThumbPaint = Paint()
      ..color = activeColor
      ..style = PaintingStyle.fill;
    
    canvas.drawCircle(
      Offset(thumbX, trackY),
      thumbRadius * 0.6,
      innerThumbPaint,
    );
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

class GradientSliderPainter extends CustomPainter {
  final double value;
  final double min;
  final double max;
  final Gradient gradient;
  final Color inactiveColor;
  final Color thumbColor;

  GradientSliderPainter({
    required this.value,
    required this.min,
    required this.max,
    required this.gradient,
    required this.inactiveColor,
    required this.thumbColor,
  });

  @override
  void paint(Canvas canvas, Size size) {
    final trackY = size.height / 2;
    final trackHeight = 8.0;
    final thumbRadius = 14.0;
    
    final progress = (value - min) / (max - min);
    final thumbX = progress * size.width;

    // Draw gradient track background
    final trackRect = Rect.fromLTWH(0, trackY - trackHeight / 2, size.width, trackHeight);
    final gradientPaint = Paint()
      ..shader = gradient.createShader(trackRect);
    
    canvas.drawRRect(
      RRect.fromRectAndRadius(trackRect, Radius.circular(trackHeight / 2)),
      gradientPaint,
    );

    // Draw inactive overlay
    final inactivePaint = Paint()
      ..color = inactiveColor;
    
    canvas.drawRRect(
      RRect.fromRectAndRadius(
        Rect.fromLTWH(thumbX, trackY - trackHeight / 2, size.width - thumbX, trackHeight),
        Radius.circular(trackHeight / 2),
      ),
      inactivePaint,
    );

    // Draw thumb with gradient
    final thumbShadowPaint = Paint()
      ..color = Colors.black.withOpacity(0.3)
      ..maskFilter = const MaskFilter.blur(BlurStyle.normal, 6);
    
    canvas.drawCircle(
      Offset(thumbX, trackY + 2),
      thumbRadius,
      thumbShadowPaint,
    );
    
    final thumbPaint = Paint()
      ..color = thumbColor
      ..style = PaintingStyle.fill;
    
    canvas.drawCircle(
      Offset(thumbX, trackY),
      thumbRadius,
      thumbPaint,
    );
    
    // Thumb border
    final thumbBorderPaint = Paint()
      ..color = Colors.white.withOpacity(0.8)
      ..style = PaintingStyle.stroke
      ..strokeWidth = 2;
    
    canvas.drawCircle(
      Offset(thumbX, trackY),
      thumbRadius - 1,
      thumbBorderPaint,
    );
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

class RangeSlider extends StatefulWidget {
  final double startValue;
  final double endValue;
  final ValueChanged<RangeValues>? onChanged;
  final double min;
  final double max;
  final Color? activeColor;
  final Color? inactiveColor;

  const RangeSlider({
    super.key,
    required this.startValue,
    required this.endValue,
    this.onChanged,
    this.min = 0.0,
    this.max = 1.0,
    this.activeColor,
    this.inactiveColor,
  });

  @override
  State<RangeSlider> createState() => _RangeSliderState();
}

class _RangeSliderState extends State<RangeSlider> {
  late RangeValues _currentRange;

  @override
  void initState() {
    super.initState();
    _currentRange = RangeValues(widget.startValue, widget.endValue);
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    return Column(
      children: [
        RangeSlider(
          values: _currentRange,
          onChanged: (values) {
            setState(() {
              _currentRange = values;
            });
            widget.onChanged?.call(values);
          },
          min: widget.min,
          max: widget.max,
          activeColor: widget.activeColor ?? theme.colorScheme.primary,
          inactiveColor: widget.inactiveColor ?? 
              theme.colorScheme.outline.withOpacity(0.3),
        ),
        Padding(
          padding: const EdgeInsets.symmetric(horizontal: 16),
          child: Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              Text('${_currentRange.start.toStringAsFixed(1)} - ${_currentRange.end.toStringAsFixed(1)}'),
              Text('Range: ${(_currentRange.end - _currentRange.start).toStringAsFixed(1)}'),
            ],
          ),
        ),
      ],
    );
  }
}

class CustomSliderDemoPage extends StatefulWidget {
  const CustomSliderDemoPage({super.key});

  @override
  State<CustomSliderDemoPage> createState() => _CustomSliderDemoPageState();
}

class _CustomSliderDemoPageState extends State<CustomSliderDemoPage> {
  double _materialValue = 0.5;
  double _customValue = 0.3;
  double _gradientValue = 0.7;
  double _segmentedValue = 2.0;
  RangeValues _rangeValues = const RangeValues(0.2, 0.8);

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Slider Widgets'),
        backgroundColor: theme.colorScheme.inversePrimary,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Material Design Slider',
              style: theme.textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            CustomSlider(
              value: _materialValue,
              style: SliderStyle.material,
              showLabels: true,
              divisions: 10,
              onChanged: (value) {
                setState(() {
                  _materialValue = value;
                });
              },
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Cupertino Style Slider',
              style: theme.textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            CustomSlider(
              value: _materialValue,
              style: SliderStyle.cupertino,
              showLabels: true,
              activeColor: Colors.blue,
              onChanged: (value) {
                setState(() {
                  _materialValue = value;
                });
              },
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Custom Painted Slider',
              style: theme.textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            CustomSlider(
              value: _customValue,
              style: SliderStyle.custom,
              height: 50,
              activeColor: Colors.purple,
              thumbColor: Colors.white,
              onChanged: (value) {
                setState(() {
                  _customValue = value;
                });
              },
            ),
            const SizedBox(height: 8),
            Center(
              child: Text(
                'Value: ${_customValue.toStringAsFixed(2)}',
                style: theme.textTheme.bodyLarge,
              ),
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Gradient Slider',
              style: theme.textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            CustomSlider(
              value: _gradientValue,
              style: SliderStyle.gradient,
              height: 50,
              onChanged: (value) {
                setState(() {
                  _gradientValue = value;
                });
              },
            ),
            const SizedBox(height: 8),
            Center(
              child: Text(
                'Value: ${_gradientValue.toStringAsFixed(2)}',
                style: theme.textTheme.bodyLarge,
              ),
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Segmented Slider',
              style: theme.textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            CustomSlider(
              value: _segmentedValue,
              min: 0,
              max: 4,
              style: SliderStyle.segmented,
              divisions: 5,
              customLabels: const ['XS', 'S', 'M', 'L', 'XL'],
              activeColor: Colors.orange,
              onChanged: (value) {
                setState(() {
                  _segmentedValue = value;
                });
              },
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Range Slider',
              style: theme.textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            RangeSlider(
              startValue: _rangeValues.start,
              endValue: _rangeValues.end,
              activeColor: Colors.green,
              onChanged: (values) {
                setState(() {
                  _rangeValues = values;
                });
              },
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Themed Variations',
              style: theme.textTheme.titleLarge,
            ),
            const SizedBox(height: 16),
            
            Card(
              color: theme.colorScheme.primaryContainer,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Volume Control',
                      style: TextStyle(
                        color: theme.colorScheme.onPrimaryContainer,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 8),
                    Row(
                      children: [
                        Icon(
                          Icons.volume_down,
                          color: theme.colorScheme.onPrimaryContainer,
                        ),
                        Expanded(
                          child: CustomSlider(
                            value: _materialValue,
                            style: SliderStyle.material,
                            activeColor: theme.colorScheme.onPrimaryContainer,
                            inactiveColor: theme.colorScheme.onPrimaryContainer.withOpacity(0.3),
                            thumbColor: theme.colorScheme.onPrimaryContainer,
                            onChanged: (value) {
                              setState(() {
                                _materialValue = value;
                              });
                            },
                          ),
                        ),
                        Icon(
                          Icons.volume_up,
                          color: theme.colorScheme.onPrimaryContainer,
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
                    const Text(
                      'Temperature Control',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                    const SizedBox(height: 16),
                    CustomSlider(
                      value: _gradientValue,
                      style: SliderStyle.gradient,
                      height: 40,
                      onChanged: (value) {
                        setState(() {
                          _gradientValue = value;
                        });
                      },
                    ),
                    const SizedBox(height: 8),
                    Row(
                      mainAxisAlignment: MainAxisAlignment.spaceBetween,
                      children: [
                        Row(
                          children: [
                            const Icon(Icons.ac_unit, color: Colors.blue, size: 16),
                            Text(' 16°C', style: theme.textTheme.bodySmall),
                          ],
                        ),
                        Text(
                          '${(16 + (_gradientValue * 14)).toStringAsFixed(0)}°C',
                          style: theme.textTheme.titleMedium,
                        ),
                        Row(
                          children: [
                            const Icon(Icons.whatshot, color: Colors.red, size: 16),
                            Text(' 30°C', style: theme.textTheme.bodySmall),
                          ],
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

This advanced slider system provides multiple styles including Material,  
Cupertino, custom painted, gradient, and segmented sliders. It includes  
range sliders, custom painters, gesture handling, and theme integration  
for creating rich interactive controls in Flutter applications.  

## Tab System Widget

Building customizable tab systems with advanced features and animations.  

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
      title: 'Custom Tab System',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.teal),
      ),
      home: const TabSystemDemoPage(),
    );
  }
}

class CustomTabController extends ChangeNotifier {
  int _selectedIndex = 0;
  final int length;

  CustomTabController({required this.length});

  int get selectedIndex => _selectedIndex;

  void animateTo(int index) {
    if (index >= 0 && index < length && index != _selectedIndex) {
      _selectedIndex = index;
      notifyListeners();
    }
  }
}

class CustomTabBar extends StatefulWidget {
  final List<CustomTab> tabs;
  final CustomTabController controller;
  final TabBarStyle style;
  final Color? selectedColor;
  final Color? unselectedColor;
  final Color? indicatorColor;
  final double? indicatorWeight;
  final bool isScrollable;
  final EdgeInsets padding;

  const CustomTabBar({
    super.key,
    required this.tabs,
    required this.controller,
    this.style = TabBarStyle.material,
    this.selectedColor,
    this.unselectedColor,
    this.indicatorColor,
    this.indicatorWeight = 3.0,
    this.isScrollable = false,
    this.padding = const EdgeInsets.all(8),
  });

  @override
  State<CustomTabBar> createState() => _CustomTabBarState();
}

enum TabBarStyle { 
  material, 
  pills, 
  underline, 
  buttons, 
  segmented,
  sidebar 
}

class CustomTab {
  final String text;
  final IconData? icon;
  final Widget? customIcon;
  final String? badge;

  const CustomTab({
    required this.text,
    this.icon,
    this.customIcon,
    this.badge,
  });
}

class _CustomTabBarState extends State<CustomTabBar>
    with TickerProviderStateMixin {
  late AnimationController _animationController;
  late Animation<double> _indicatorAnimation;

  @override
  void initState() {
    super.initState();
    
    _animationController = AnimationController(
      duration: const Duration(milliseconds: 300),
      vsync: this,
    );

    _indicatorAnimation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _animationController,
      curve: Curves.easeInOut,
    ));

    widget.controller.addListener(_onControllerChanged);
  }

  void _onControllerChanged() {
    _animationController.forward().then((_) {
      _animationController.reset();
    });
    setState(() {});
  }

  @override
  void dispose() {
    widget.controller.removeListener(_onControllerChanged);
    _animationController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    switch (widget.style) {
      case TabBarStyle.material:
        return _buildMaterialTabBar(theme);
      case TabBarStyle.pills:
        return _buildPillTabBar(theme);
      case TabBarStyle.underline:
        return _buildUnderlineTabBar(theme);
      case TabBarStyle.buttons:
        return _buildButtonTabBar(theme);
      case TabBarStyle.segmented:
        return _buildSegmentedTabBar(theme);
      case TabBarStyle.sidebar:
        return _buildSidebarTabBar(theme);
    }
  }

  Widget _buildMaterialTabBar(ThemeData theme) {
    return Container(
      padding: widget.padding,
      child: widget.isScrollable 
          ? SingleChildScrollView(
              scrollDirection: Axis.horizontal,
              child: Row(children: _buildTabItems(theme)),
            )
          : Row(children: _buildTabItems(theme)),
    );
  }

  Widget _buildPillTabBar(ThemeData theme) {
    return Container(
      padding: widget.padding,
      decoration: BoxDecoration(
        color: theme.colorScheme.surfaceVariant,
        borderRadius: BorderRadius.circular(25),
      ),
      child: Row(
        mainAxisSize: MainAxisSize.min,
        children: widget.tabs.asMap().entries.map((entry) {
          final index = entry.key;
          final tab = entry.value;
          final isSelected = index == widget.controller.selectedIndex;

          return Expanded(
            child: GestureDetector(
              onTap: () => widget.controller.animateTo(index),
              child: AnimatedContainer(
                duration: const Duration(milliseconds: 200),
                padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
                decoration: BoxDecoration(
                  color: isSelected 
                      ? theme.colorScheme.primary
                      : Colors.transparent,
                  borderRadius: BorderRadius.circular(20),
                ),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.center,
                  mainAxisSize: MainAxisSize.min,
                  children: [
                    if (tab.icon != null) ...[
                      Icon(
                        tab.icon,
                        size: 18,
                        color: isSelected 
                            ? theme.colorScheme.onPrimary
                            : theme.colorScheme.onSurfaceVariant,
                      ),
                      const SizedBox(width: 8),
                    ],
                    Text(
                      tab.text,
                      style: TextStyle(
                        color: isSelected 
                            ? theme.colorScheme.onPrimary
                            : theme.colorScheme.onSurfaceVariant,
                        fontWeight: isSelected 
                            ? FontWeight.w600 
                            : FontWeight.normal,
                      ),
                    ),
                    if (tab.badge != null) ...[
                      const SizedBox(width: 4),
                      Container(
                        padding: const EdgeInsets.symmetric(horizontal: 6, vertical: 2),
                        decoration: BoxDecoration(
                          color: Colors.red,
                          borderRadius: BorderRadius.circular(10),
                        ),
                        child: Text(
                          tab.badge!,
                          style: const TextStyle(
                            color: Colors.white,
                            fontSize: 10,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ),
                    ],
                  ],
                ),
              ),
            ),
          );
        }).toList(),
      ),
    );
  }

  Widget _buildUnderlineTabBar(ThemeData theme) {
    return Container(
      padding: widget.padding,
      child: Column(
        children: [
          Row(
            children: widget.tabs.asMap().entries.map((entry) {
              final index = entry.key;
              final tab = entry.value;
              final isSelected = index == widget.controller.selectedIndex;

              return Expanded(
                child: GestureDetector(
                  onTap: () => widget.controller.animateTo(index),
                  child: Container(
                    padding: const EdgeInsets.symmetric(vertical: 12),
                    child: Column(
                      children: [
                        if (tab.icon != null) ...[
                          Icon(
                            tab.icon,
                            size: 24,
                            color: isSelected 
                                ? (widget.selectedColor ?? theme.colorScheme.primary)
                                : (widget.unselectedColor ?? theme.colorScheme.onSurface.withOpacity(0.6)),
                          ),
                          const SizedBox(height: 4),
                        ],
                        Text(
                          tab.text,
                          style: TextStyle(
                            color: isSelected 
                                ? (widget.selectedColor ?? theme.colorScheme.primary)
                                : (widget.unselectedColor ?? theme.colorScheme.onSurface.withOpacity(0.6)),
                            fontWeight: isSelected ? FontWeight.w600 : FontWeight.normal,
                          ),
                        ),
                      ],
                    ),
                  ),
                ),
              );
            }).toList(),
          ),
          Container(
            height: widget.indicatorWeight,
            child: AnimatedAlign(
              duration: const Duration(milliseconds: 300),
              alignment: Alignment(
                -1 + (2 * widget.controller.selectedIndex / (widget.tabs.length - 1)),
                0,
              ),
              child: Container(
                width: MediaQuery.of(context).size.width / widget.tabs.length - 16,
                height: widget.indicatorWeight,
                decoration: BoxDecoration(
                  color: widget.indicatorColor ?? theme.colorScheme.primary,
                  borderRadius: BorderRadius.circular(widget.indicatorWeight! / 2),
                ),
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildButtonTabBar(ThemeData theme) {
    return Container(
      padding: widget.padding,
      child: Wrap(
        spacing: 8,
        runSpacing: 8,
        children: widget.tabs.asMap().entries.map((entry) {
          final index = entry.key;
          final tab = entry.value;
          final isSelected = index == widget.controller.selectedIndex;

          return GestureDetector(
            onTap: () => widget.controller.animateTo(index),
            child: AnimatedContainer(
              duration: const Duration(milliseconds: 200),
              padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
              decoration: BoxDecoration(
                color: isSelected 
                    ? theme.colorScheme.primary
                    : theme.colorScheme.surfaceVariant,
                borderRadius: BorderRadius.circular(8),
                border: Border.all(
                  color: isSelected 
                      ? theme.colorScheme.primary
                      : theme.colorScheme.outline.withOpacity(0.5),
                ),
              ),
              child: Row(
                mainAxisSize: MainAxisSize.min,
                children: [
                  if (tab.icon != null) ...[
                    Icon(
                      tab.icon,
                      size: 16,
                      color: isSelected 
                          ? theme.colorScheme.onPrimary
                          : theme.colorScheme.onSurfaceVariant,
                    ),
                    const SizedBox(width: 8),
                  ],
                  Text(
                    tab.text,
                    style: TextStyle(
                      color: isSelected 
                          ? theme.colorScheme.onPrimary
                          : theme.colorScheme.onSurfaceVariant,
                      fontWeight: isSelected ? FontWeight.w600 : FontWeight.normal,
                    ),
                  ),
                ],
              ),
            ),
          );
        }).toList(),
      ),
    );
  }

  Widget _buildSegmentedTabBar(ThemeData theme) {
    return Container(
      padding: widget.padding,
      child: Container(
        decoration: BoxDecoration(
          border: Border.all(color: theme.colorScheme.outline),
          borderRadius: BorderRadius.circular(8),
        ),
        child: Row(
          children: widget.tabs.asMap().entries.map((entry) {
            final index = entry.key;
            final tab = entry.value;
            final isSelected = index == widget.controller.selectedIndex;
            final isFirst = index == 0;
            final isLast = index == widget.tabs.length - 1;

            return Expanded(
              child: GestureDetector(
                onTap: () => widget.controller.animateTo(index),
                child: Container(
                  padding: const EdgeInsets.symmetric(vertical: 12),
                  decoration: BoxDecoration(
                    color: isSelected 
                        ? theme.colorScheme.primary
                        : Colors.transparent,
                    borderRadius: BorderRadius.only(
                      topLeft: isFirst ? const Radius.circular(7) : Radius.zero,
                      bottomLeft: isFirst ? const Radius.circular(7) : Radius.zero,
                      topRight: isLast ? const Radius.circular(7) : Radius.zero,
                      bottomRight: isLast ? const Radius.circular(7) : Radius.zero,
                    ),
                  ),
                  child: Text(
                    tab.text,
                    textAlign: TextAlign.center,
                    style: TextStyle(
                      color: isSelected 
                          ? theme.colorScheme.onPrimary
                          : theme.colorScheme.onSurface,
                      fontWeight: isSelected ? FontWeight.w600 : FontWeight.normal,
                    ),
                  ),
                ),
              ),
            );
          }).toList(),
        ),
      ),
    );
  }

  Widget _buildSidebarTabBar(ThemeData theme) {
    return Container(
      width: 200,
      padding: widget.padding,
      child: Column(
        children: widget.tabs.asMap().entries.map((entry) {
          final index = entry.key;
          final tab = entry.value;
          final isSelected = index == widget.controller.selectedIndex;

          return GestureDetector(
            onTap: () => widget.controller.animateTo(index),
            child: Container(
              width: double.infinity,
              margin: const EdgeInsets.only(bottom: 4),
              padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
              decoration: BoxDecoration(
                color: isSelected 
                    ? theme.colorScheme.primary.withOpacity(0.1)
                    : Colors.transparent,
                borderRadius: BorderRadius.circular(8),
                border: isSelected 
                    ? Border.all(color: theme.colorScheme.primary)
                    : null,
              ),
              child: Row(
                children: [
                  if (tab.icon != null) ...[
                    Icon(
                      tab.icon,
                      size: 20,
                      color: isSelected 
                          ? theme.colorScheme.primary
                          : theme.colorScheme.onSurface,
                    ),
                    const SizedBox(width: 12),
                  ],
                  Expanded(
                    child: Text(
                      tab.text,
                      style: TextStyle(
                        color: isSelected 
                            ? theme.colorScheme.primary
                            : theme.colorScheme.onSurface,
                        fontWeight: isSelected ? FontWeight.w600 : FontWeight.normal,
                      ),
                    ),
                  ),
                  if (tab.badge != null)
                    Container(
                      padding: const EdgeInsets.symmetric(horizontal: 6, vertical: 2),
                      decoration: BoxDecoration(
                        color: Colors.red,
                        borderRadius: BorderRadius.circular(10),
                      ),
                      child: Text(
                        tab.badge!,
                        style: const TextStyle(
                          color: Colors.white,
                          fontSize: 10,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                ],
              ),
            ),
          );
        }).toList(),
      ),
    );
  }

  List<Widget> _buildTabItems(ThemeData theme) {
    return widget.tabs.asMap().entries.map((entry) {
      final index = entry.key;
      final tab = entry.value;
      final isSelected = index == widget.controller.selectedIndex;

      return Expanded(
        child: GestureDetector(
          onTap: () => widget.controller.animateTo(index),
          child: Container(
            padding: const EdgeInsets.symmetric(vertical: 12),
            child: Column(
              mainAxisSize: MainAxisSize.min,
              children: [
                if (tab.icon != null) ...[
                  Icon(
                    tab.icon,
                    size: 24,
                    color: isSelected 
                        ? (widget.selectedColor ?? theme.colorScheme.primary)
                        : (widget.unselectedColor ?? theme.colorScheme.onSurface.withOpacity(0.6)),
                  ),
                  const SizedBox(height: 4),
                ],
                Text(
                  tab.text,
                  style: TextStyle(
                    color: isSelected 
                        ? (widget.selectedColor ?? theme.colorScheme.primary)
                        : (widget.unselectedColor ?? theme.colorScheme.onSurface.withOpacity(0.6)),
                    fontWeight: isSelected ? FontWeight.w600 : FontWeight.normal,
                  ),
                ),
              ],
            ),
          ),
        ),
      );
    }).toList();
  }
}

class CustomTabView extends StatelessWidget {
  final CustomTabController controller;
  final List<Widget> children;

  const CustomTabView({
    super.key,
    required this.controller,
    required this.children,
  });

  @override
  Widget build(BuildContext context) {
    return ListenableBuilder(
      listenable: controller,
      builder: (context, child) {
        return IndexedStack(
          index: controller.selectedIndex,
          children: children,
        );
      },
    );
  }
}

class TabSystemDemoPage extends StatefulWidget {
  const TabSystemDemoPage({super.key});

  @override
  State<TabSystemDemoPage> createState() => _TabSystemDemoPageState();
}

class _TabSystemDemoPageState extends State<TabSystemDemoPage> {
  late CustomTabController _controller1;
  late CustomTabController _controller2;
  late CustomTabController _controller3;
  late CustomTabController _controller4;
  late CustomTabController _sidebarController;

  final List<CustomTab> _tabs = const [
    CustomTab(text: 'Home', icon: Icons.home),
    CustomTab(text: 'Search', icon: Icons.search),
    CustomTab(text: 'Profile', icon: Icons.person, badge: '3'),
    CustomTab(text: 'Settings', icon: Icons.settings),
  ];

  @override
  void initState() {
    super.initState();
    _controller1 = CustomTabController(length: _tabs.length);
    _controller2 = CustomTabController(length: _tabs.length);
    _controller3 = CustomTabController(length: _tabs.length);
    _controller4 = CustomTabController(length: _tabs.length);
    _sidebarController = CustomTabController(length: _tabs.length);
  }

  @override
  void dispose() {
    _controller1.dispose();
    _controller2.dispose();
    _controller3.dispose();
    _controller4.dispose();
    _sidebarController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Tab System'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Material Design Tabs',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            CustomTabBar(
              tabs: _tabs,
              controller: _controller1,
              style: TabBarStyle.material,
            ),
            SizedBox(
              height: 100,
              child: CustomTabView(
                controller: _controller1,
                children: [
                  _buildTabContent('Home Content', Icons.home),
                  _buildTabContent('Search Content', Icons.search),
                  _buildTabContent('Profile Content', Icons.person),
                  _buildTabContent('Settings Content', Icons.settings),
                ],
              ),
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Pill Style Tabs',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            CustomTabBar(
              tabs: _tabs,
              controller: _controller2,
              style: TabBarStyle.pills,
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Underline Tabs',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            CustomTabBar(
              tabs: _tabs,
              controller: _controller3,
              style: TabBarStyle.underline,
              indicatorColor: Colors.orange,
              selectedColor: Colors.orange,
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Button Style Tabs',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            CustomTabBar(
              tabs: _tabs,
              controller: _controller4,
              style: TabBarStyle.buttons,
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Segmented Control',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            CustomTabBar(
              tabs: const [
                CustomTab(text: 'All'),
                CustomTab(text: 'Active'),
                CustomTab(text: 'Completed'),
              ],
              controller: CustomTabController(length: 3),
              style: TabBarStyle.segmented,
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Sidebar Navigation',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            Row(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                CustomTabBar(
                  tabs: [
                    const CustomTab(text: 'Dashboard', icon: Icons.dashboard),
                    const CustomTab(text: 'Analytics', icon: Icons.analytics),
                    const CustomTab(text: 'Users', icon: Icons.people, badge: '12'),
                    const CustomTab(text: 'Settings', icon: Icons.settings),
                    const CustomTab(text: 'Help', icon: Icons.help),
                  ],
                  controller: _sidebarController,
                  style: TabBarStyle.sidebar,
                ),
                Expanded(
                  child: Container(
                    height: 250,
                    padding: const EdgeInsets.all(16),
                    decoration: BoxDecoration(
                      color: Theme.of(context).colorScheme.surface,
                      borderRadius: BorderRadius.circular(8),
                      border: Border.all(
                        color: Theme.of(context).colorScheme.outline.withOpacity(0.3),
                      ),
                    ),
                    child: CustomTabView(
                      controller: _sidebarController,
                      children: [
                        _buildSidebarContent('Dashboard', 'Overview of your application metrics'),
                        _buildSidebarContent('Analytics', 'Detailed analytics and reports'),
                        _buildSidebarContent('Users', 'Manage user accounts and permissions'),
                        _buildSidebarContent('Settings', 'Configure application settings'),
                        _buildSidebarContent('Help', 'Get help and support'),
                      ],
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

  Widget _buildTabContent(String title, IconData icon) {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(icon, size: 32, color: Theme.of(context).colorScheme.primary),
          const SizedBox(height: 8),
          Text(title),
        ],
      ),
    );
  }

  Widget _buildSidebarContent(String title, String description) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          title,
          style: Theme.of(context).textTheme.headlineSmall,
        ),
        const SizedBox(height: 8),
        Text(
          description,
          style: Theme.of(context).textTheme.bodyMedium,
        ),
        const SizedBox(height: 16),
        Expanded(
          child: Container(
            width: double.infinity,
            decoration: BoxDecoration(
              color: Theme.of(context).colorScheme.primaryContainer.withOpacity(0.3),
              borderRadius: BorderRadius.circular(8),
            ),
            child: Center(
              child: Text(
                '$title Content Area',
                style: Theme.of(context).textTheme.bodyLarge,
              ),
            ),
          ),
        ),
      ],
    );
  }
}
```

This comprehensive tab system provides multiple styles including Material  
Design, pills, underline, buttons, segmented controls, and sidebar  
navigation. Each style supports icons, badges, animations, and theme  
integration for creating flexible navigation experiences.  

## Notification Widget

Creating sophisticated notification components with animations and theming.  

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
      title: 'Notification Widgets',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
      ),
      home: const NotificationDemoPage(),
    );
  }
}

enum NotificationType { success, error, warning, info }

class CustomNotification extends StatefulWidget {
  final String title;
  final String message;
  final NotificationType type;
  final Duration? duration;
  final VoidCallback? onDismiss;
  final Widget? action;
  final bool showCloseButton;
  final EdgeInsets margin;

  const CustomNotification({
    super.key,
    required this.title,
    required this.message,
    required this.type,
    this.duration,
    this.onDismiss,
    this.action,
    this.showCloseButton = true,
    this.margin = const EdgeInsets.all(8),
  });

  @override
  State<CustomNotification> createState() => _CustomNotificationState();
}

class _CustomNotificationState extends State<CustomNotification>
    with TickerProviderStateMixin {
  late AnimationController _slideController;
  late AnimationController _fadeController;
  late Animation<Offset> _slideAnimation;
  late Animation<double> _fadeAnimation;

  @override
  void initState() {
    super.initState();
    
    _slideController = AnimationController(
      duration: const Duration(milliseconds: 400),
      vsync: this,
    );
    
    _fadeController = AnimationController(
      duration: const Duration(milliseconds: 200),
      vsync: this,
    );

    _slideAnimation = Tween<Offset>(
      begin: const Offset(1, 0),
      end: Offset.zero,
    ).animate(CurvedAnimation(
      parent: _slideController,
      curve: Curves.easeOut,
    ));

    _fadeAnimation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(_fadeController);

    _slideController.forward();
    _fadeController.forward();

    if (widget.duration != null) {
      Future.delayed(widget.duration!, () {
        if (mounted) {
          _dismiss();
        }
      });
    }
  }

  @override
  void dispose() {
    _slideController.dispose();
    _fadeController.dispose();
    super.dispose();
  }

  void _dismiss() async {
    await _fadeController.reverse();
    await _slideController.reverse();
    widget.onDismiss?.call();
  }

  Color _getColor(ThemeData theme) {
    switch (widget.type) {
      case NotificationType.success:
        return Colors.green;
      case NotificationType.error:
        return Colors.red;
      case NotificationType.warning:
        return Colors.orange;
      case NotificationType.info:
        return theme.colorScheme.primary;
    }
  }

  IconData _getIcon() {
    switch (widget.type) {
      case NotificationType.success:
        return Icons.check_circle;
      case NotificationType.error:
        return Icons.error;
      case NotificationType.warning:
        return Icons.warning;
      case NotificationType.info:
        return Icons.info;
    }
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final color = _getColor(theme);
    
    return SlideTransition(
      position: _slideAnimation,
      child: FadeTransition(
        opacity: _fadeAnimation,
        child: Container(
          margin: widget.margin,
          decoration: BoxDecoration(
            color: theme.colorScheme.surface,
            borderRadius: BorderRadius.circular(12),
            border: Border.all(
              color: color,
              width: 2,
            ),
            boxShadow: [
              BoxShadow(
                color: Colors.black.withOpacity(0.1),
                blurRadius: 8,
                offset: const Offset(0, 4),
              ),
            ],
          ),
          child: Padding(
            padding: const EdgeInsets.all(16),
            child: Row(
              children: [
                Container(
                  padding: const EdgeInsets.all(8),
                  decoration: BoxDecoration(
                    color: color.withOpacity(0.1),
                    shape: BoxShape.circle,
                  ),
                  child: Icon(
                    _getIcon(),
                    color: color,
                    size: 24,
                  ),
                ),
                const SizedBox(width: 16),
                Expanded(
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      Text(
                        widget.title,
                        style: theme.textTheme.titleMedium?.copyWith(
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      const SizedBox(height: 4),
                      Text(
                        widget.message,
                        style: theme.textTheme.bodyMedium,
                      ),
                      if (widget.action != null) ...[
                        const SizedBox(height: 8),
                        widget.action!,
                      ],
                    ],
                  ),
                ),
                if (widget.showCloseButton)
                  IconButton(
                    icon: const Icon(Icons.close),
                    onPressed: _dismiss,
                    iconSize: 20,
                  ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}

class NotificationManager extends ChangeNotifier {
  final List<NotificationItem> _notifications = [];
  
  List<NotificationItem> get notifications => _notifications;
  
  void showNotification({
    required String title,
    required String message,
    required NotificationType type,
    Duration? duration,
    Widget? action,
  }) {
    final notification = NotificationItem(
      id: DateTime.now().millisecondsSinceEpoch.toString(),
      title: title,
      message: message,
      type: type,
      duration: duration ?? const Duration(seconds: 4),
      action: action,
    );
    
    _notifications.insert(0, notification);
    notifyListeners();
    
    Future.delayed(notification.duration, () {
      removeNotification(notification.id);
    });
  }
  
  void removeNotification(String id) {
    _notifications.removeWhere((n) => n.id == id);
    notifyListeners();
  }
  
  void clearAll() {
    _notifications.clear();
    notifyListeners();
  }
}

class NotificationItem {
  final String id;
  final String title;
  final String message;
  final NotificationType type;
  final Duration duration;
  final Widget? action;

  NotificationItem({
    required this.id,
    required this.title,
    required this.message,
    required this.type,
    required this.duration,
    this.action,
  });
}

class NotificationOverlay extends StatelessWidget {
  final NotificationManager manager;

  const NotificationOverlay({
    super.key,
    required this.manager,
  });

  @override
  Widget build(BuildContext context) {
    return ListenableBuilder(
      listenable: manager,
      builder: (context, child) {
        return Positioned(
          top: MediaQuery.of(context).padding.top + 16,
          right: 16,
          left: 16,
          child: Column(
            children: manager.notifications.map((notification) {
              return CustomNotification(
                key: Key(notification.id),
                title: notification.title,
                message: notification.message,
                type: notification.type,
                action: notification.action,
                onDismiss: () {
                  manager.removeNotification(notification.id);
                },
              );
            }).toList(),
          ),
        );
      },
    );
  }
}

class ToastNotification extends StatelessWidget {
  final String message;
  final NotificationType type;
  final Duration duration;

  const ToastNotification({
    super.key,
    required this.message,
    this.type = NotificationType.info,
    this.duration = const Duration(seconds: 2),
  });

  static void show(
    BuildContext context, {
    required String message,
    NotificationType type = NotificationType.info,
    Duration duration = const Duration(seconds: 2),
  }) {
    final overlay = Overlay.of(context);
    late OverlayEntry overlayEntry;
    
    overlayEntry = OverlayEntry(
      builder: (context) => Positioned(
        bottom: 100,
        left: 16,
        right: 16,
        child: Material(
          color: Colors.transparent,
          child: ToastNotification(
            message: message,
            type: type,
            duration: duration,
          ),
        ),
      ),
    );
    
    overlay.insert(overlayEntry);
    
    Future.delayed(duration, () {
      overlayEntry.remove();
    });
  }

  Color _getColor(ThemeData theme) {
    switch (type) {
      case NotificationType.success:
        return Colors.green;
      case NotificationType.error:
        return Colors.red;
      case NotificationType.warning:
        return Colors.orange;
      case NotificationType.info:
        return theme.colorScheme.primary;
    }
  }

  IconData _getIcon() {
    switch (type) {
      case NotificationType.success:
        return Icons.check_circle;
      case NotificationType.error:
        return Icons.error;
      case NotificationType.warning:
        return Icons.warning;
      case NotificationType.info:
        return Icons.info;
    }
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final color = _getColor(theme);
    
    return TweenAnimationBuilder<double>(
      duration: const Duration(milliseconds: 300),
      tween: Tween<double>(begin: 0, end: 1),
      builder: (context, value, child) {
        return Transform.scale(
          scale: value,
          child: Opacity(
            opacity: value,
            child: Container(
              padding: const EdgeInsets.all(16),
              decoration: BoxDecoration(
                color: color,
                borderRadius: BorderRadius.circular(8),
                boxShadow: [
                  BoxShadow(
                    color: Colors.black.withOpacity(0.2),
                    blurRadius: 8,
                    offset: const Offset(0, 4),
                  ),
                ],
              ),
              child: Row(
                mainAxisAlignment: MainAxisAlignment.center,
                mainAxisSize: MainAxisSize.min,
                children: [
                  Icon(
                    _getIcon(),
                    color: Colors.white,
                    size: 20,
                  ),
                  const SizedBox(width: 8),
                  Flexible(
                    child: Text(
                      message,
                      style: const TextStyle(
                        color: Colors.white,
                        fontWeight: FontWeight.w600,
                      ),
                      textAlign: TextAlign.center,
                    ),
                  ),
                ],
              ),
            ),
          ),
        );
      },
    );
  }
}

class NotificationDemoPage extends StatefulWidget {
  const NotificationDemoPage({super.key});

  @override
  State<NotificationDemoPage> createState() => _NotificationDemoPageState();
}

class _NotificationDemoPageState extends State<NotificationDemoPage> {
  final NotificationManager _notificationManager = NotificationManager();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Stack(
        children: [
          Scaffold(
            appBar: AppBar(
              title: const Text('Notification Widgets'),
              backgroundColor: Theme.of(context).colorScheme.inversePrimary,
              actions: [
                IconButton(
                  icon: const Icon(Icons.clear_all),
                  onPressed: _notificationManager.clearAll,
                  tooltip: 'Clear all notifications',
                ),
              ],
            ),
            body: SingleChildScrollView(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.stretch,
                children: [
                  Text(
                    'Notification Examples',
                    style: Theme.of(context).textTheme.headlineSmall,
                  ),
                  const SizedBox(height: 16),
                  
                  // Static notification examples
                  CustomNotification(
                    title: 'Success Notification',
                    message: 'Your action was completed successfully!',
                    type: NotificationType.success,
                    showCloseButton: false,
                  ),
                  
                  CustomNotification(
                    title: 'Error Notification',
                    message: 'Something went wrong. Please try again.',
                    type: NotificationType.error,
                    showCloseButton: false,
                    action: TextButton(
                      onPressed: () {},
                      child: const Text('Retry'),
                    ),
                  ),
                  
                  CustomNotification(
                    title: 'Warning',
                    message: 'Your session will expire in 5 minutes.',
                    type: NotificationType.warning,
                    showCloseButton: false,
                    action: Row(
                      mainAxisSize: MainAxisSize.min,
                      children: [
                        TextButton(
                          onPressed: () {},
                          child: const Text('Extend'),
                        ),
                        TextButton(
                          onPressed: () {},
                          child: const Text('Logout'),
                        ),
                      ],
                    ),
                  ),
                  
                  CustomNotification(
                    title: 'Information',
                    message: 'A new update is available for download.',
                    type: NotificationType.info,
                    showCloseButton: false,
                  ),
                  
                  const SizedBox(height: 32),
                  
                  Text(
                    'Interactive Notifications',
                    style: Theme.of(context).textTheme.headlineSmall,
                  ),
                  const SizedBox(height: 16),
                  
                  Wrap(
                    spacing: 8,
                    runSpacing: 8,
                    children: [
                      ElevatedButton.icon(
                        icon: const Icon(Icons.check_circle),
                        label: const Text('Success'),
                        onPressed: () {
                          _notificationManager.showNotification(
                            title: 'Success!',
                            message: 'Operation completed successfully.',
                            type: NotificationType.success,
                          );
                        },
                      ),
                      ElevatedButton.icon(
                        icon: const Icon(Icons.error),
                        label: const Text('Error'),
                        onPressed: () {
                          _notificationManager.showNotification(
                            title: 'Error',
                            message: 'Failed to complete the operation.',
                            type: NotificationType.error,
                            action: TextButton(
                              onPressed: () {},
                              child: const Text('Retry'),
                            ),
                          );
                        },
                      ),
                      ElevatedButton.icon(
                        icon: const Icon(Icons.warning),
                        label: const Text('Warning'),
                        onPressed: () {
                          _notificationManager.showNotification(
                            title: 'Warning',
                            message: 'Please review your settings.',
                            type: NotificationType.warning,
                          );
                        },
                      ),
                      ElevatedButton.icon(
                        icon: const Icon(Icons.info),
                        label: const Text('Info'),
                        onPressed: () {
                          _notificationManager.showNotification(
                            title: 'Information',
                            message: 'New features are now available!',
                            type: NotificationType.info,
                          );
                        },
                      ),
                    ],
                  ),
                  
                  const SizedBox(height: 32),
                  
                  Text(
                    'Toast Notifications',
                    style: Theme.of(context).textTheme.headlineSmall,
                  ),
                  const SizedBox(height: 16),
                  
                  Wrap(
                    spacing: 8,
                    runSpacing: 8,
                    children: [
                      ElevatedButton(
                        onPressed: () {
                          ToastNotification.show(
                            context,
                            message: 'Item saved successfully!',
                            type: NotificationType.success,
                          );
                        },
                        child: const Text('Success Toast'),
                      ),
                      ElevatedButton(
                        onPressed: () {
                          ToastNotification.show(
                            context,
                            message: 'Network connection failed',
                            type: NotificationType.error,
                          );
                        },
                        child: const Text('Error Toast'),
                      ),
                      ElevatedButton(
                        onPressed: () {
                          ToastNotification.show(
                            context,
                            message: 'Battery level is low',
                            type: NotificationType.warning,
                          );
                        },
                        child: const Text('Warning Toast'),
                      ),
                      ElevatedButton(
                        onPressed: () {
                          ToastNotification.show(
                            context,
                            message: 'Welcome to the app!',
                            type: NotificationType.info,
                          );
                        },
                        child: const Text('Info Toast'),
                      ),
                    ],
                  ),
                  
                  const SizedBox(height: 32),
                  
                  Text(
                    'Batch Operations',
                    style: Theme.of(context).textTheme.headlineSmall,
                  ),
                  const SizedBox(height: 16),
                  
                  Row(
                    children: [
                      Expanded(
                        child: ElevatedButton(
                          onPressed: () {
                            // Show multiple notifications
                            final notifications = [
                              ('Processing started', NotificationType.info),
                              ('Uploading files', NotificationType.info),
                              ('Processing complete', NotificationType.success),
                            ];
                            
                            for (int i = 0; i < notifications.length; i++) {
                              Future.delayed(Duration(milliseconds: i * 500), () {
                                _notificationManager.showNotification(
                                  title: 'Batch Process',
                                  message: notifications[i].$1,
                                  type: notifications[i].$2,
                                );
                              });
                            }
                          },
                          child: const Text('Batch Process'),
                        ),
                      ),
                      const SizedBox(width: 16),
                      Expanded(
                        child: ElevatedButton(
                          onPressed: _notificationManager.clearAll,
                          child: const Text('Clear All'),
                        ),
                      ),
                    ],
                  ),
                  
                  const SizedBox(height: 100), // Space for bottom notifications
                ],
              ),
            ),
          ),
          NotificationOverlay(manager: _notificationManager),
        ],
      ),
    );
  }
}
```

This notification system provides various notification styles including  
overlay notifications, toast messages, and different types (success, error,  
warning, info). Features include animations, auto-dismissal, actions, and  
a notification manager for handling multiple notifications efficiently.  

## Data Visualization Widget

Creating interactive charts and data visualization components.  

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
      title: 'Data Visualization Widgets',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.cyan),
      ),
      home: const DataVisualizationDemoPage(),
    );
  }
}

class ChartData {
  final String label;
  final double value;
  final Color? color;

  ChartData({
    required this.label,
    required this.value,
    this.color,
  });
}

class PieChart extends StatefulWidget {
  final List<ChartData> data;
  final double size;
  final bool showLabels;
  final bool showValues;
  final bool animated;

  const PieChart({
    super.key,
    required this.data,
    this.size = 200,
    this.showLabels = true,
    this.showValues = true,
    this.animated = true,
  });

  @override
  State<PieChart> createState() => _PieChartState();
}

class _PieChartState extends State<PieChart>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;
  int _selectedIndex = -1;

  @override
  void initState() {
    super.initState();
    
    _controller = AnimationController(
      duration: const Duration(milliseconds: 800),
      vsync: this,
    );

    _animation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeOutCubic,
    ));

    if (widget.animated) {
      _controller.forward();
    } else {
      _animation = const AlwaysStoppedAnimation(1.0);
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        SizedBox(
          width: widget.size,
          height: widget.size,
          child: AnimatedBuilder(
            animation: _animation,
            builder: (context, child) {
              return CustomPaint(
                size: Size(widget.size, widget.size),
                painter: PieChartPainter(
                  data: widget.data,
                  progress: _animation.value,
                  selectedIndex: _selectedIndex,
                ),
                child: GestureDetector(
                  onTapDown: (details) {
                    final center = Offset(widget.size / 2, widget.size / 2);
                    final distance = (details.localPosition - center).distance;
                    final radius = widget.size / 2 - 20;
                    
                    if (distance <= radius) {
                      final angle = math.atan2(
                        details.localPosition.dy - center.dy,
                        details.localPosition.dx - center.dx,
                      );
                      
                      final normalizedAngle = (angle + math.pi / 2) % (2 * math.pi);
                      final total = widget.data.fold(0.0, (sum, item) => sum + item.value);
                      
                      double currentAngle = 0;
                      for (int i = 0; i < widget.data.length; i++) {
                        final segmentAngle = (widget.data[i].value / total) * 2 * math.pi;
                        if (normalizedAngle >= currentAngle && 
                            normalizedAngle < currentAngle + segmentAngle) {
                          setState(() {
                            _selectedIndex = _selectedIndex == i ? -1 : i;
                          });
                          break;
                        }
                        currentAngle += segmentAngle;
                      }
                    }
                  },
                ),
              );
            },
          ),
        ),
        if (widget.showLabels) ...[
          const SizedBox(height: 16),
          _buildLegend(),
        ],
      ],
    );
  }

  Widget _buildLegend() {
    return Wrap(
      spacing: 16,
      runSpacing: 8,
      children: widget.data.asMap().entries.map((entry) {
        final index = entry.key;
        final item = entry.value;
        final isSelected = index == _selectedIndex;
        final colors = _getColors();
        
        return GestureDetector(
          onTap: () {
            setState(() {
              _selectedIndex = _selectedIndex == index ? -1 : index;
            });
          },
          child: Container(
            padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
            decoration: BoxDecoration(
              color: isSelected 
                  ? colors[index % colors.length].withOpacity(0.1)
                  : null,
              borderRadius: BorderRadius.circular(4),
            ),
            child: Row(
              mainAxisSize: MainAxisSize.min,
              children: [
                Container(
                  width: 12,
                  height: 12,
                  decoration: BoxDecoration(
                    color: item.color ?? colors[index % colors.length],
                    shape: BoxShape.circle,
                  ),
                ),
                const SizedBox(width: 8),
                Text(
                  widget.showValues 
                      ? '${item.label} (${item.value.toStringAsFixed(1)})'
                      : item.label,
                  style: TextStyle(
                    fontWeight: isSelected ? FontWeight.bold : FontWeight.normal,
                  ),
                ),
              ],
            ),
          ),
        );
      }).toList(),
    );
  }

  List<Color> _getColors() {
    return [
      Colors.blue,
      Colors.red,
      Colors.green,
      Colors.orange,
      Colors.purple,
      Colors.teal,
      Colors.pink,
      Colors.indigo,
    ];
  }
}

class PieChartPainter extends CustomPainter {
  final List<ChartData> data;
  final double progress;
  final int selectedIndex;

  PieChartPainter({
    required this.data,
    required this.progress,
    required this.selectedIndex,
  });

  @override
  void paint(Canvas canvas, Size size) {
    final center = Offset(size.width / 2, size.height / 2);
    final radius = size.width / 2 - 20;
    final total = data.fold(0.0, (sum, item) => sum + item.value);
    
    if (total == 0) return;

    final colors = _getColors();
    double startAngle = -math.pi / 2;

    for (int i = 0; i < data.length; i++) {
      final item = data[i];
      final sweepAngle = (item.value / total) * 2 * math.pi * progress;
      final isSelected = i == selectedIndex;
      final currentRadius = isSelected ? radius + 10 : radius;
      
      final paint = Paint()
        ..color = item.color ?? colors[i % colors.length]
        ..style = PaintingStyle.fill;

      final rect = Rect.fromCircle(center: center, radius: currentRadius);
      
      canvas.drawArc(
        rect,
        startAngle,
        sweepAngle,
        true,
        paint,
      );

      // Draw stroke
      final strokePaint = Paint()
        ..color = Colors.white
        ..style = PaintingStyle.stroke
        ..strokeWidth = 2;
      
      canvas.drawArc(
        rect,
        startAngle,
        sweepAngle,
        true,
        strokePaint,
      );

      startAngle += sweepAngle;
    }
  }

  List<Color> _getColors() {
    return [
      Colors.blue,
      Colors.red,
      Colors.green,
      Colors.orange,
      Colors.purple,
      Colors.teal,
      Colors.pink,
      Colors.indigo,
    ];
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

class BarChart extends StatefulWidget {
  final List<ChartData> data;
  final double width;
  final double height;
  final bool showValues;
  final bool animated;

  const BarChart({
    super.key,
    required this.data,
    this.width = 300,
    this.height = 200,
    this.showValues = true,
    this.animated = true,
  });

  @override
  State<BarChart> createState() => _BarChartState();
}

class _BarChartState extends State<BarChart>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;
  int _selectedIndex = -1;

  @override
  void initState() {
    super.initState();
    
    _controller = AnimationController(
      duration: const Duration(milliseconds: 800),
      vsync: this,
    );

    _animation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeOutCubic,
    ));

    if (widget.animated) {
      _controller.forward();
    } else {
      _animation = const AlwaysStoppedAnimation(1.0);
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        SizedBox(
          width: widget.width,
          height: widget.height,
          child: AnimatedBuilder(
            animation: _animation,
            builder: (context, child) {
              return CustomPaint(
                size: Size(widget.width, widget.height),
                painter: BarChartPainter(
                  data: widget.data,
                  progress: _animation.value,
                  selectedIndex: _selectedIndex,
                  showValues: widget.showValues,
                ),
                child: GestureDetector(
                  onTapDown: (details) {
                    final barWidth = widget.width / widget.data.length;
                    final index = (details.localPosition.dx / barWidth).floor();
                    
                    if (index >= 0 && index < widget.data.length) {
                      setState(() {
                        _selectedIndex = _selectedIndex == index ? -1 : index;
                      });
                    }
                  },
                ),
              );
            },
          ),
        ),
      ],
    );
  }
}

class BarChartPainter extends CustomPainter {
  final List<ChartData> data;
  final double progress;
  final int selectedIndex;
  final bool showValues;

  BarChartPainter({
    required this.data,
    required this.progress,
    required this.selectedIndex,
    required this.showValues,
  });

  @override
  void paint(Canvas canvas, Size size) {
    if (data.isEmpty) return;

    final maxValue = data.map((e) => e.value).reduce(math.max);
    final barWidth = size.width / data.length;
    final colors = _getColors();

    for (int i = 0; i < data.length; i++) {
      final item = data[i];
      final barHeight = (item.value / maxValue) * size.height * 0.8 * progress;
      final isSelected = i == selectedIndex;
      
      final rect = Rect.fromLTWH(
        i * barWidth + barWidth * 0.1,
        size.height - barHeight - 20,
        barWidth * 0.8,
        barHeight,
      );

      final paint = Paint()
        ..color = (item.color ?? colors[i % colors.length])
            .withOpacity(isSelected ? 1.0 : 0.8)
        ..style = PaintingStyle.fill;

      canvas.drawRRect(
        RRect.fromRectAndRadius(rect, const Radius.circular(4)),
        paint,
      );

      // Draw border for selected bar
      if (isSelected) {
        final borderPaint = Paint()
          ..color = Colors.white
          ..style = PaintingStyle.stroke
          ..strokeWidth = 2;
        
        canvas.drawRRect(
          RRect.fromRectAndRadius(rect, const Radius.circular(4)),
          borderPaint,
        );
      }

      // Draw value on top of bar
      if (showValues && progress > 0.8) {
        final textPainter = TextPainter(
          text: TextSpan(
            text: item.value.toStringAsFixed(0),
            style: TextStyle(
              color: Colors.black87,
              fontSize: 12,
              fontWeight: FontWeight.w600,
            ),
          ),
          textDirection: TextDirection.ltr,
        );
        
        textPainter.layout();
        
        final textOffset = Offset(
          rect.left + (rect.width - textPainter.width) / 2,
          rect.top - textPainter.height - 4,
        );
        
        textPainter.paint(canvas, textOffset);
      }

      // Draw label below bar
      final labelPainter = TextPainter(
        text: TextSpan(
          text: item.label,
          style: TextStyle(
            color: Colors.black87,
            fontSize: 10,
            fontWeight: isSelected ? FontWeight.bold : FontWeight.normal,
          ),
        ),
        textDirection: TextDirection.ltr,
      );
      
      labelPainter.layout();
      
      final labelOffset = Offset(
        rect.left + (rect.width - labelPainter.width) / 2,
        size.height - 15,
      );
      
      labelPainter.paint(canvas, labelOffset);
    }
  }

  List<Color> _getColors() {
    return [
      Colors.blue,
      Colors.red,
      Colors.green,
      Colors.orange,
      Colors.purple,
      Colors.teal,
      Colors.pink,
      Colors.indigo,
    ];
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

class LineChart extends StatefulWidget {
  final List<ChartData> data;
  final double width;
  final double height;
  final bool showPoints;
  final bool filled;

  const LineChart({
    super.key,
    required this.data,
    this.width = 300,
    this.height = 200,
    this.showPoints = true,
    this.filled = false,
  });

  @override
  State<LineChart> createState() => _LineChartState();
}

class _LineChartState extends State<LineChart>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();
    
    _controller = AnimationController(
      duration: const Duration(milliseconds: 1000),
      vsync: this,
    );

    _animation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeOutCubic,
    ));

    _controller.forward();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return SizedBox(
      width: widget.width,
      height: widget.height,
      child: AnimatedBuilder(
        animation: _animation,
        builder: (context, child) {
          return CustomPaint(
            size: Size(widget.width, widget.height),
            painter: LineChartPainter(
              data: widget.data,
              progress: _animation.value,
              showPoints: widget.showPoints,
              filled: widget.filled,
            ),
          );
        },
      ),
    );
  }
}

class LineChartPainter extends CustomPainter {
  final List<ChartData> data;
  final double progress;
  final bool showPoints;
  final bool filled;

  LineChartPainter({
    required this.data,
    required this.progress,
    required this.showPoints,
    required this.filled,
  });

  @override
  void paint(Canvas canvas, Size size) {
    if (data.length < 2) return;

    final maxValue = data.map((e) => e.value).reduce(math.max);
    final minValue = data.map((e) => e.value).reduce(math.min);
    final range = maxValue - minValue;
    
    final path = Path();
    final points = <Offset>[];
    
    for (int i = 0; i < data.length; i++) {
      final x = (i / (data.length - 1)) * size.width;
      final normalizedValue = range > 0 ? (data[i].value - minValue) / range : 0.5;
      final y = size.height - (normalizedValue * size.height * 0.8) - 20;
      
      points.add(Offset(x, y));
      
      if (i == 0) {
        path.moveTo(x, y);
      } else {
        path.lineTo(x, y);
      }
    }

    // Create animated path
    final pathMetrics = path.computeMetrics();
    final animatedPath = Path();
    
    for (final metric in pathMetrics) {
      final extractPath = metric.extractPath(0, metric.length * progress);
      animatedPath.addPath(extractPath, Offset.zero);
    }

    // Draw filled area
    if (filled) {
      final fillPath = Path.from(animatedPath);
      if (points.isNotEmpty) {
        fillPath.lineTo(points.last.dx, size.height);
        fillPath.lineTo(points.first.dx, size.height);
        fillPath.close();
      }
      
      final fillPaint = Paint()
        ..color = Colors.blue.withOpacity(0.3)
        ..style = PaintingStyle.fill;
      
      canvas.drawPath(fillPath, fillPaint);
    }

    // Draw line
    final linePaint = Paint()
      ..color = Colors.blue
      ..strokeWidth = 3
      ..style = PaintingStyle.stroke
      ..strokeCap = StrokeCap.round
      ..strokeJoin = StrokeJoin.round;

    canvas.drawPath(animatedPath, linePaint);

    // Draw points
    if (showPoints && progress > 0.7) {
      final pointPaint = Paint()
        ..color = Colors.blue
        ..style = PaintingStyle.fill;
      
      final pointBorderPaint = Paint()
        ..color = Colors.white
        ..style = PaintingStyle.stroke
        ..strokeWidth = 2;

      final visiblePoints = (points.length * progress).round();
      for (int i = 0; i < visiblePoints; i++) {
        canvas.drawCircle(points[i], 6, pointBorderPaint);
        canvas.drawCircle(points[i], 4, pointPaint);
      }
    }
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

class DataVisualizationDemoPage extends StatefulWidget {
  const DataVisualizationDemoPage({super.key};

  @override
  State<DataVisualizationDemoPage> createState() => 
      _DataVisualizationDemoPageState();
}

class _DataVisualizationDemoPageState extends State<DataVisualizationDemoPage> {
  final List<ChartData> _pieData = [
    ChartData(label: 'Mobile', value: 45),
    ChartData(label: 'Desktop', value: 30),
    ChartData(label: 'Tablet', value: 20),
    ChartData(label: 'Other', value: 5),
  ];

  final List<ChartData> _barData = [
    ChartData(label: 'Jan', value: 65),
    ChartData(label: 'Feb', value: 78),
    ChartData(label: 'Mar', value: 90),
    ChartData(label: 'Apr', value: 85),
    ChartData(label: 'May', value: 95),
    ChartData(label: 'Jun', value: 88),
  ];

  final List<ChartData> _lineData = [
    ChartData(label: 'Week 1', value: 20),
    ChartData(label: 'Week 2', value: 35),
    ChartData(label: 'Week 3', value: 28),
    ChartData(label: 'Week 4', value: 45),
    ChartData(label: 'Week 5', value: 52),
    ChartData(label: 'Week 6', value: 38),
    ChartData(label: 'Week 7', value: 65),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Data Visualization Widgets'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Pie Chart',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    const Text(
                      'Device Usage Distribution',
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 16),
                    PieChart(
                      data: _pieData,
                      size: 250,
                      showLabels: true,
                      showValues: true,
                    ),
                  ],
                ),
              ),
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Bar Chart',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    const Text(
                      'Monthly Sales Data',
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 16),
                    BarChart(
                      data: _barData,
                      width: MediaQuery.of(context).size.width - 64,
                      height: 250,
                      showValues: true,
                    ),
                  ],
                ),
              ),
            ),
            
            const SizedBox(height: 32),
            
            Text(
              'Line Chart',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 16),
            
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  children: [
                    const Text(
                      'Weekly Growth Trend',
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 16),
                    LineChart(
                      data: _lineData,
                      width: MediaQuery.of(context).size.width - 64,
                      height: 200,
                      showPoints: true,
                      filled: false,
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
                  children: [
                    const Text(
                      'Filled Area Chart',
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 16),
                    LineChart(
                      data: _lineData,
                      width: MediaQuery.of(context).size.width - 64,
                      height: 200,
                      showPoints: true,
                      filled: true,
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

This data visualization system provides interactive pie charts, bar charts,  
and line charts with animations, selection handling, and theme integration.  
Charts support custom colors, labels, values, and various display options  
for creating engaging data presentations in Flutter applications.  

## Custom Timeline Widget

Creating a timeline widget for displaying chronological events and milestones.  

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
      title: 'Custom Timeline Widget',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepOrange),
      ),
      home: const TimelineDemoPage(),
    );
  }
}

class TimelineEvent {
  final String title;
  final String description;
  final DateTime date;
  final IconData? icon;
  final Color? color;
  final Widget? customIcon;

  TimelineEvent({
    required this.title,
    required this.description,
    required this.date,
    this.icon,
    this.color,
    this.customIcon,
  });
}

class CustomTimeline extends StatefulWidget {
  final List<TimelineEvent> events;
  final bool showDates;
  final Color? lineColor;
  final double lineWidth;
  final bool animated;

  const CustomTimeline({
    super.key,
    required this.events,
    this.showDates = true,
    this.lineColor,
    this.lineWidth = 2,
    this.animated = true,
  });

  @override
  State<CustomTimeline> createState() => _CustomTimelineState();
}

class _CustomTimelineState extends State<CustomTimeline>
    with TickerProviderStateMixin {
  late List<AnimationController> _controllers;
  late List<Animation<double>> _animations;

  @override
  void initState() {
    super.initState();
    
    if (widget.animated) {
      _controllers = List.generate(
        widget.events.length,
        (index) => AnimationController(
          duration: Duration(milliseconds: 300 + (index * 100)),
          vsync: this,
        ),
      );

      _animations = _controllers.map((controller) {
        return Tween<double>(begin: 0, end: 1).animate(
          CurvedAnimation(parent: controller, curve: Curves.easeOutBack),
        );
      }).toList();

      // Start animations with delay
      for (int i = 0; i < _controllers.length; i++) {
        Future.delayed(Duration(milliseconds: i * 150), () {
          if (mounted) _controllers[i].forward();
        });
      }
    }
  }

  @override
  void dispose() {
    if (widget.animated) {
      for (var controller in _controllers) {
        controller.dispose();
      }
    }
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    return Column(
      children: widget.events.asMap().entries.map((entry) {
        final index = entry.key;
        final event = entry.value;
        final isLast = index == widget.events.length - 1;

        Widget child = _buildTimelineItem(theme, event, index, isLast);

        if (widget.animated) {
          child = AnimatedBuilder(
            animation: _animations[index],
            builder: (context, child) {
              return Transform.scale(
                scale: _animations[index].value,
                alignment: Alignment.centerLeft,
                child: Opacity(
                  opacity: _animations[index].value,
                  child: child,
                ),
              );
            },
            child: child,
          );
        }

        return child;
      }).toList(),
    );
  }

  Widget _buildTimelineItem(
    ThemeData theme,
    TimelineEvent event,
    int index,
    bool isLast,
  ) {
    final eventColor = event.color ?? theme.colorScheme.primary;
    final lineColor = widget.lineColor ?? theme.colorScheme.outline;

    return IntrinsicHeight(
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          // Timeline line and dot
          SizedBox(
            width: 60,
            child: Column(
              children: [
                // Date
                if (widget.showDates) ...[
                  Container(
                    padding: const EdgeInsets.symmetric(
                      horizontal: 8,
                      vertical: 4,
                    ),
                    decoration: BoxDecoration(
                      color: eventColor.withOpacity(0.1),
                      borderRadius: BorderRadius.circular(12),
                    ),
                    child: Text(
                      '${event.date.day}/${event.date.month}',
                      style: TextStyle(
                        fontSize: 10,
                        fontWeight: FontWeight.w600,
                        color: eventColor,
                      ),
                    ),
                  ),
                  const SizedBox(height: 8),
                ],
                // Icon/Dot
                Container(
                  width: 32,
                  height: 32,
                  decoration: BoxDecoration(
                    color: eventColor,
                    shape: BoxShape.circle,
                    boxShadow: [
                      BoxShadow(
                        color: eventColor.withOpacity(0.3),
                        blurRadius: 8,
                        offset: const Offset(0, 2),
                      ),
                    ],
                  ),
                  child: event.customIcon ??
                      Icon(
                        event.icon ?? Icons.timeline,
                        color: Colors.white,
                        size: 16,
                      ),
                ),
                // Line
                if (!isLast)
                  Expanded(
                    child: Container(
                      width: widget.lineWidth,
                      margin: const EdgeInsets.only(top: 8),
                      decoration: BoxDecoration(
                        color: lineColor,
                        borderRadius: BorderRadius.circular(1),
                      ),
                    ),
                  ),
              ],
            ),
          ),
          // Content
          Expanded(
            child: Container(
              margin: const EdgeInsets.only(left: 16, bottom: 32),
              child: Card(
                elevation: 2,
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        event.title,
                        style: theme.textTheme.titleMedium?.copyWith(
                          fontWeight: FontWeight.bold,
                          color: eventColor,
                        ),
                      ),
                      const SizedBox(height: 8),
                      Text(
                        event.description,
                        style: theme.textTheme.bodyMedium,
                      ),
                      if (widget.showDates) ...[
                        const SizedBox(height: 8),
                        Text(
                          '${event.date.day}/${event.date.month}/${event.date.year}',
                          style: theme.textTheme.bodySmall?.copyWith(
                            color: theme.colorScheme.onSurface.withOpacity(0.6),
                          ),
                        ),
                      ],
                    ],
                  ),
                ),
              ),
            ),
          ),
        ],
      ),
    );
  }
}

class TimelineDemoPage extends StatelessWidget {
  const TimelineDemoPage({super.key});

  @override
  Widget build(BuildContext context) {
    final events = [
      TimelineEvent(
        title: 'Project Started',
        description: 'Initial planning and requirements gathering phase',
        date: DateTime(2024, 1, 15),
        icon: Icons.start,
        color: Colors.green,
      ),
      TimelineEvent(
        title: 'Design Phase',
        description: 'UI/UX design and prototyping completed',
        date: DateTime(2024, 2, 10),
        icon: Icons.design_services,
        color: Colors.blue,
      ),
      TimelineEvent(
        title: 'Development',
        description: 'Core development and feature implementation',
        date: DateTime(2024, 3, 5),
        icon: Icons.code,
        color: Colors.orange,
      ),
      TimelineEvent(
        title: 'Testing Phase',
        description: 'Quality assurance and bug fixes',
        date: DateTime(2024, 3, 25),
        icon: Icons.bug_report,
        color: Colors.red,
      ),
      TimelineEvent(
        title: 'Deployment',
        description: 'Production deployment and launch',
        date: DateTime(2024, 4, 10),
        icon: Icons.rocket_launch,
        color: Colors.purple,
      ),
    ];

    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Timeline Widget'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Project Timeline',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 24),
            CustomTimeline(
              events: events,
              showDates: true,
              animated: true,
            ),
          ],
        ),
      ),
    );
  }
}
```

This timeline widget displays chronological events with icons, dates, and  
descriptions. Features include animated reveals, customizable colors and  
icons, flexible date display, and smooth visual flow for presenting  
project milestones, history, or any sequential information.  

## Widget State Management

Creating widgets that efficiently manage complex state and data flows.  

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
      title: 'Widget State Management',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.teal),
      ),
      home: const StateManagementDemoPage(),
    );
  }
}

class ShoppingItem {
  final String id;
  final String name;
  final double price;
  int quantity;
  bool selected;

  ShoppingItem({
    required this.id,
    required this.name,
    required this.price,
    this.quantity = 1,
    this.selected = false,
  });

  double get total => price * quantity;
}

class ShoppingCartNotifier extends ChangeNotifier {
  final List<ShoppingItem> _items = [];
  
  List<ShoppingItem> get items => List.unmodifiable(_items);
  
  double get totalPrice => _items.fold(0, (sum, item) => sum + item.total);
  
  int get totalItems => _items.fold(0, (sum, item) => sum + item.quantity);
  
  int get selectedCount => _items.where((item) => item.selected).length;
  
  void addItem(ShoppingItem item) {
    final existingIndex = _items.indexWhere((i) => i.id == item.id);
    if (existingIndex >= 0) {
      _items[existingIndex].quantity += item.quantity;
    } else {
      _items.add(item);
    }
    notifyListeners();
  }
  
  void removeItem(String id) {
    _items.removeWhere((item) => item.id == id);
    notifyListeners();
  }
  
  void updateQuantity(String id, int quantity) {
    final index = _items.indexWhere((item) => item.id == id);
    if (index >= 0) {
      if (quantity > 0) {
        _items[index].quantity = quantity;
      } else {
        _items.removeAt(index);
      }
      notifyListeners();
    }
  }
  
  void toggleSelection(String id) {
    final index = _items.indexWhere((item) => item.id == id);
    if (index >= 0) {
      _items[index].selected = !_items[index].selected;
      notifyListeners();
    }
  }
  
  void clearSelected() {
    _items.removeWhere((item) => item.selected);
    notifyListeners();
  }
  
  void selectAll() {
    for (var item in _items) {
      item.selected = true;
    }
    notifyListeners();
  }
  
  void deselectAll() {
    for (var item in _items) {
      item.selected = false;
    }
    notifyListeners();
  }
}

class ShoppingCartWidget extends StatefulWidget {
  final ShoppingCartNotifier cartNotifier;

  const ShoppingCartWidget({
    super.key,
    required this.cartNotifier,
  });

  @override
  State<ShoppingCartWidget> createState() => _ShoppingCartWidgetState();
}

class _ShoppingCartWidgetState extends State<ShoppingCartWidget>
    with TickerProviderStateMixin {
  late AnimationController _selectionController;
  late Animation<double> _selectionAnimation;
  
  @override
  void initState() {
    super.initState();
    
    _selectionController = AnimationController(
      duration: const Duration(milliseconds: 200),
      vsync: this,
    );
    
    _selectionAnimation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(_selectionController);
    
    widget.cartNotifier.addListener(_onCartChanged);
  }
  
  @override
  void dispose() {
    widget.cartNotifier.removeListener(_onCartChanged);
    _selectionController.dispose();
    super.dispose();
  }
  
  void _onCartChanged() {
    if (widget.cartNotifier.selectedCount > 0) {
      _selectionController.forward();
    } else {
      _selectionController.reverse();
    }
  }

  @override
  Widget build(BuildContext context) {
    return ListenableBuilder(
      listenable: widget.cartNotifier,
      builder: (context, child) {
        return Column(
          children: [
            _buildHeader(),
            _buildSelectionBar(),
            Expanded(child: _buildItemList()),
            _buildFooter(),
          ],
        );
      },
    );
  }

  Widget _buildHeader() {
    return Container(
      padding: const EdgeInsets.all(16),
      decoration: BoxDecoration(
        color: Theme.of(context).colorScheme.primaryContainer,
        borderRadius: const BorderRadius.vertical(top: Radius.circular(16)),
      ),
      child: Row(
        children: [
          Icon(
            Icons.shopping_cart,
            color: Theme.of(context).colorScheme.onPrimaryContainer,
          ),
          const SizedBox(width: 8),
          Text(
            'Shopping Cart',
            style: Theme.of(context).textTheme.titleLarge?.copyWith(
              color: Theme.of(context).colorScheme.onPrimaryContainer,
              fontWeight: FontWeight.bold,
            ),
          ),
          const Spacer(),
          Container(
            padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 4),
            decoration: BoxDecoration(
              color: Theme.of(context).colorScheme.primary,
              borderRadius: BorderRadius.circular(12),
            ),
            child: Text(
              '${widget.cartNotifier.totalItems}',
              style: TextStyle(
                color: Theme.of(context).colorScheme.onPrimary,
                fontWeight: FontWeight.bold,
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildSelectionBar() {
    return AnimatedBuilder(
      animation: _selectionAnimation,
      builder: (context, child) {
        return SizeTransition(
          sizeFactor: _selectionAnimation,
          child: Container(
            padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
            color: Theme.of(context).colorScheme.secondaryContainer,
            child: Row(
              children: [
                Text(
                  '${widget.cartNotifier.selectedCount} selected',
                  style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                    color: Theme.of(context).colorScheme.onSecondaryContainer,
                  ),
                ),
                const Spacer(),
                TextButton(
                  onPressed: widget.cartNotifier.selectAll,
                  child: const Text('Select All'),
                ),
                TextButton(
                  onPressed: widget.cartNotifier.deselectAll,
                  child: const Text('Deselect All'),
                ),
                TextButton(
                  onPressed: widget.cartNotifier.clearSelected,
                  style: TextButton.styleFrom(
                    foregroundColor: Theme.of(context).colorScheme.error,
                  ),
                  child: const Text('Delete Selected'),
                ),
              ],
            ),
          ),
        );
      },
    );
  }

  Widget _buildItemList() {
    if (widget.cartNotifier.items.isEmpty) {
      return Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(
              Icons.shopping_cart_outlined,
              size: 64,
              color: Theme.of(context).colorScheme.onSurface.withOpacity(0.5),
            ),
            const SizedBox(height: 16),
            Text(
              'Your cart is empty',
              style: Theme.of(context).textTheme.titleMedium?.copyWith(
                color: Theme.of(context).colorScheme.onSurface.withOpacity(0.5),
              ),
            ),
          ],
        ),
      );
    }

    return ListView.builder(
      itemCount: widget.cartNotifier.items.length,
      itemBuilder: (context, index) {
        final item = widget.cartNotifier.items[index];
        return _buildCartItem(item);
      },
    );
  }

  Widget _buildCartItem(ShoppingItem item) {
    return Card(
      margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 4),
      child: Padding(
        padding: const EdgeInsets.all(12),
        child: Row(
          children: [
            Checkbox(
              value: item.selected,
              onChanged: (_) => widget.cartNotifier.toggleSelection(item.id),
            ),
            Expanded(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    item.name,
                    style: Theme.of(context).textTheme.titleMedium,
                  ),
                  Text(
                    '\$${item.price.toStringAsFixed(2)} each',
                    style: Theme.of(context).textTheme.bodySmall?.copyWith(
                      color: Theme.of(context).colorScheme.onSurface.withOpacity(0.6),
                    ),
                  ),
                ],
              ),
            ),
            Row(
              mainAxisSize: MainAxisSize.min,
              children: [
                IconButton(
                  onPressed: () => widget.cartNotifier.updateQuantity(
                    item.id,
                    item.quantity - 1,
                  ),
                  icon: const Icon(Icons.remove),
                ),
                Container(
                  padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 4),
                  decoration: BoxDecoration(
                    border: Border.all(color: Theme.of(context).dividerColor),
                    borderRadius: BorderRadius.circular(4),
                  ),
                  child: Text('${item.quantity}'),
                ),
                IconButton(
                  onPressed: () => widget.cartNotifier.updateQuantity(
                    item.id,
                    item.quantity + 1,
                  ),
                  icon: const Icon(Icons.add),
                ),
              ],
            ),
            Column(
              crossAxisAlignment: CrossAxisAlignment.end,
              children: [
                Text(
                  '\$${item.total.toStringAsFixed(2)}',
                  style: Theme.of(context).textTheme.titleMedium?.copyWith(
                    fontWeight: FontWeight.bold,
                  ),
                ),
                IconButton(
                  onPressed: () => widget.cartNotifier.removeItem(item.id),
                  icon: Icon(
                    Icons.delete,
                    color: Theme.of(context).colorScheme.error,
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildFooter() {
    return Container(
      padding: const EdgeInsets.all(16),
      decoration: BoxDecoration(
        color: Theme.of(context).colorScheme.surface,
        border: Border(
          top: BorderSide(color: Theme.of(context).dividerColor),
        ),
      ),
      child: Column(
        children: [
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              Text(
                'Total:',
                style: Theme.of(context).textTheme.titleLarge?.copyWith(
                  fontWeight: FontWeight.bold,
                ),
              ),
              Text(
                '\$${widget.cartNotifier.totalPrice.toStringAsFixed(2)}',
                style: Theme.of(context).textTheme.titleLarge?.copyWith(
                  fontWeight: FontWeight.bold,
                  color: Theme.of(context).colorScheme.primary,
                ),
              ),
            ],
          ),
          const SizedBox(height: 16),
          Row(
            children: [
              Expanded(
                child: OutlinedButton(
                  onPressed: () {
                    // Add sample items for demo
                    widget.cartNotifier.addItem(ShoppingItem(
                      id: DateTime.now().millisecondsSinceEpoch.toString(),
                      name: 'Sample Item',
                      price: 19.99,
                    ));
                  },
                  child: const Text('Add Sample Item'),
                ),
              ),
              const SizedBox(width: 16),
              Expanded(
                child: ElevatedButton(
                  onPressed: widget.cartNotifier.items.isNotEmpty
                      ? () {
                          ScaffoldMessenger.of(context).showSnackBar(
                            SnackBar(
                              content: Text(
                                'Checkout: \$${widget.cartNotifier.totalPrice.toStringAsFixed(2)}',
                              ),
                            ),
                          );
                        }
                      : null,
                  child: const Text('Checkout'),
                ),
              ),
            ],
          ),
        ],
      ),
    );
  }
}

class StateManagementDemoPage extends StatefulWidget {
  const StateManagementDemoPage({super.key});

  @override
  State<StateManagementDemoPage> createState() => _StateManagementDemoPageState();
}

class _StateManagementDemoPageState extends State<StateManagementDemoPage> {
  final ShoppingCartNotifier _cartNotifier = ShoppingCartNotifier();

  @override
  void initState() {
    super.initState();
    
    // Add some sample items
    _cartNotifier.addItem(ShoppingItem(
      id: '1',
      name: 'Wireless Headphones',
      price: 99.99,
    ));
    _cartNotifier.addItem(ShoppingItem(
      id: '2',
      name: 'Phone Case',
      price: 24.99,
      quantity: 2,
    ));
    _cartNotifier.addItem(ShoppingItem(
      id: '3',
      name: 'Charging Cable',
      price: 15.99,
    ));
  }

  @override
  void dispose() {
    _cartNotifier.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Widget State Management'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: ShoppingCartWidget(cartNotifier: _cartNotifier),
    );
  }
}
```

This example demonstrates advanced state management using ChangeNotifier  
for a shopping cart widget. Features include reactive UI updates, bulk  
operations, selection management, and complex state synchronization  
between multiple widget components efficiently.  

## Custom Gesture Widget

Building widgets with advanced gesture recognition and touch interactions.  

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
      title: 'Custom Gesture Widget',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.purple),
      ),
      home: const GestureDemoPage(),
    );
  }
}

class SwipeDirection {
  static const int none = 0;
  static const int left = 1;
  static const int right = 2;
  static const int up = 3;
  static const int down = 4;
}

class SwipeActionWidget extends StatefulWidget {
  final Widget child;
  final VoidCallback? onSwipeLeft;
  final VoidCallback? onSwipeRight;
  final VoidCallback? onSwipeUp;
  final VoidCallback? onSwipeDown;
  final VoidCallback? onTap;
  final VoidCallback? onDoubleTap;
  final VoidCallback? onLongPress;
  final double swipeThreshold;
  final Duration animationDuration;
  final Color? highlightColor;

  const SwipeActionWidget({
    super.key,
    required this.child,
    this.onSwipeLeft,
    this.onSwipeRight,
    this.onSwipeUp,
    this.onSwipeDown,
    this.onTap,
    this.onDoubleTap,
    this.onLongPress,
    this.swipeThreshold = 100,
    this.animationDuration = const Duration(milliseconds: 200),
    this.highlightColor,
  });

  @override
  State<SwipeActionWidget> createState() => _SwipeActionWidgetState();
}

class _SwipeActionWidgetState extends State<SwipeActionWidget>
    with TickerProviderStateMixin {
  late AnimationController _offsetController;
  late AnimationController _scaleController;
  late AnimationController _rotationController;
  late Animation<Offset> _offsetAnimation;
  late Animation<double> _scaleAnimation;
  late Animation<double> _rotationAnimation;
  
  Offset _startPosition = Offset.zero;
  Offset _currentOffset = Offset.zero;
  bool _isPressed = false;
  int _currentSwipeDirection = SwipeDirection.none;

  @override
  void initState() {
    super.initState();
    
    _offsetController = AnimationController(
      duration: widget.animationDuration,
      vsync: this,
    );
    
    _scaleController = AnimationController(
      duration: const Duration(milliseconds: 100),
      vsync: this,
    );
    
    _rotationController = AnimationController(
      duration: widget.animationDuration,
      vsync: this,
    );

    _offsetAnimation = Tween<Offset>(
      begin: Offset.zero,
      end: Offset.zero,
    ).animate(CurvedAnimation(
      parent: _offsetController,
      curve: Curves.easeOutBack,
    ));

    _scaleAnimation = Tween<double>(
      begin: 1.0,
      end: 0.95,
    ).animate(_scaleController);

    _rotationAnimation = Tween<double>(
      begin: 0,
      end: 0,
    ).animate(CurvedAnimation(
      parent: _rotationController,
      curve: Curves.elasticOut,
    ));
  }

  @override
  void dispose() {
    _offsetController.dispose();
    _scaleController.dispose();
    _rotationController.dispose();
    super.dispose();
  }

  void _onPanStart(DragStartDetails details) {
    _startPosition = details.localPosition;
    _scaleController.forward();
    setState(() {
      _isPressed = true;
    });
  }

  void _onPanUpdate(DragUpdateDetails details) {
    final delta = details.localPosition - _startPosition;
    
    setState(() {
      _currentOffset = delta;
    });

    // Determine swipe direction
    if (delta.dx.abs() > delta.dy.abs()) {
      _currentSwipeDirection = delta.dx > 0 ? SwipeDirection.right : SwipeDirection.left;
    } else {
      _currentSwipeDirection = delta.dy > 0 ? SwipeDirection.down : SwipeDirection.up;
    }

    // Update rotation based on horizontal swipe
    if (_currentSwipeDirection == SwipeDirection.left || 
        _currentSwipeDirection == SwipeDirection.right) {
      final rotationValue = (delta.dx / widget.swipeThreshold).clamp(-0.2, 0.2);
      _rotationAnimation = Tween<double>(
        begin: _rotationAnimation.value,
        end: rotationValue,
      ).animate(_rotationController);
      _rotationController.forward();
    }
  }

  void _onPanEnd(DragEndDetails details) {
    _scaleController.reverse();
    setState(() {
      _isPressed = false;
    });

    final distance = _currentOffset.distance;
    
    if (distance > widget.swipeThreshold) {
      _triggerSwipeAction();
    }
    
    // Reset position
    _resetPosition();
  }

  void _triggerSwipeAction() {
    switch (_currentSwipeDirection) {
      case SwipeDirection.left:
        widget.onSwipeLeft?.call();
        break;
      case SwipeDirection.right:
        widget.onSwipeRight?.call();
        break;
      case SwipeDirection.up:
        widget.onSwipeUp?.call();
        break;
      case SwipeDirection.down:
        widget.onSwipeDown?.call();
        break;
    }
  }

  void _resetPosition() {
    _offsetAnimation = Tween<Offset>(
      begin: _currentOffset,
      end: Offset.zero,
    ).animate(CurvedAnimation(
      parent: _offsetController,
      curve: Curves.easeOutBack,
    ));
    
    _rotationAnimation = Tween<double>(
      begin: _rotationAnimation.value,
      end: 0,
    ).animate(_rotationController);
    
    _offsetController.forward().then((_) {
      _offsetController.reset();
      setState(() {
        _currentOffset = Offset.zero;
        _currentSwipeDirection = SwipeDirection.none;
      });
    });
    
    _rotationController.forward().then((_) {
      _rotationController.reset();
    });
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: widget.onTap,
      onDoubleTap: widget.onDoubleTap,
      onLongPress: widget.onLongPress,
      onPanStart: _onPanStart,
      onPanUpdate: _onPanUpdate,
      onPanEnd: _onPanEnd,
      child: AnimatedBuilder(
        animation: Listenable.merge([
          _offsetController,
          _scaleController,
          _rotationController,
        ]),
        builder: (context, child) {
          return Transform.translate(
            offset: _offsetController.isAnimating 
                ? _offsetAnimation.value 
                : _currentOffset,
            child: Transform.scale(
              scale: _scaleAnimation.value,
              child: Transform.rotate(
                angle: _rotationAnimation.value,
                child: Container(
                  decoration: BoxDecoration(
                    boxShadow: _isPressed
                        ? [
                            BoxShadow(
                              color: (widget.highlightColor ?? 
                                     Theme.of(context).colorScheme.primary)
                                  .withOpacity(0.3),
                              blurRadius: 20,
                              spreadRadius: 2,
                            ),
                          ]
                        : null,
                  ),
                  child: widget.child,
                ),
              ),
            ),
          );
        },
      ),
    );
  }
}

class InteractiveCard extends StatelessWidget {
  final String title;
  final String subtitle;
  final IconData icon;
  final Color color;
  final VoidCallback? onAction;

  const InteractiveCard({
    super.key,
    required this.title,
    required this.subtitle,
    required this.icon,
    required this.color,
    this.onAction,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      elevation: 2,
      child: Container(
        padding: const EdgeInsets.all(16),
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(12),
          gradient: LinearGradient(
            colors: [
              color.withOpacity(0.1),
              color.withOpacity(0.05),
            ],
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
          ),
        ),
        child: Row(
          children: [
            Container(
              padding: const EdgeInsets.all(12),
              decoration: BoxDecoration(
                color: color.withOpacity(0.2),
                shape: BoxShape.circle,
              ),
              child: Icon(
                icon,
                color: color,
                size: 24,
              ),
            ),
            const SizedBox(width: 16),
            Expanded(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    title,
                    style: Theme.of(context).textTheme.titleMedium?.copyWith(
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  const SizedBox(height: 4),
                  Text(
                    subtitle,
                    style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                      color: Theme.of(context).colorScheme.onSurface.withOpacity(0.7),
                    ),
                  ),
                ],
              ),
            ),
            Icon(
              Icons.touch_app,
              color: color.withOpacity(0.5),
            ),
          ],
        ),
      ),
    );
  }
}

class GestureDemoPage extends StatefulWidget {
  const GestureDemoPage({super.key});

  @override
  State<GestureDemoPage> createState() => _GestureDemoPageState();
}

class _GestureDemoPageState extends State<GestureDemoPage> {
  String _lastAction = 'No action yet';
  int _tapCount = 0;
  int _swipeCount = 0;

  void _showAction(String action) {
    setState(() {
      _lastAction = action;
      if (action.contains('Tap')) {
        _tapCount++;
      } else if (action.contains('Swipe')) {
        _swipeCount++;
      }
    });

    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(action),
        duration: const Duration(seconds: 1),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Custom Gesture Widget'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Card(
              color: Theme.of(context).colorScheme.primaryContainer,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Gesture Statistics',
                      style: Theme.of(context).textTheme.titleLarge?.copyWith(
                        color: Theme.of(context).colorScheme.onPrimaryContainer,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 12),
                    Row(
                      children: [
                        Expanded(
                          child: Column(
                            children: [
                              Text(
                                '$_tapCount',
                                style: Theme.of(context).textTheme.headlineMedium?.copyWith(
                                  color: Theme.of(context).colorScheme.onPrimaryContainer,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                              Text(
                                'Taps',
                                style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                                  color: Theme.of(context).colorScheme.onPrimaryContainer,
                                ),
                              ),
                            ],
                          ),
                        ),
                        Expanded(
                          child: Column(
                            children: [
                              Text(
                                '$_swipeCount',
                                style: Theme.of(context).textTheme.headlineMedium?.copyWith(
                                  color: Theme.of(context).colorScheme.onPrimaryContainer,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                              Text(
                                'Swipes',
                                style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                                  color: Theme.of(context).colorScheme.onPrimaryContainer,
                                ),
                              ),
                            ],
                          ),
                        ),
                      ],
                    ),
                    const SizedBox(height: 8),
                    Text(
                      'Last action: $_lastAction',
                      style: Theme.of(context).textTheme.bodySmall?.copyWith(
                        color: Theme.of(context).colorScheme.onPrimaryContainer.withOpacity(0.8),
                      ),
                    ),
                  ],
                ),
              ),
            ),
            
            const SizedBox(height: 24),
            
            Text(
              'Try Different Gestures',
              style: Theme.of(context).textTheme.titleLarge,
            ),
            const SizedBox(height: 8),
            Text(
              'Tap, double tap, long press, or swipe in any direction',
              style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                color: Theme.of(context).colorScheme.onSurface.withOpacity(0.7),
              ),
            ),
            
            const SizedBox(height: 16),
            
            SwipeActionWidget(
              onTap: () => _showAction('Single Tap'),
              onDoubleTap: () => _showAction('Double Tap'),
              onLongPress: () => _showAction('Long Press'),
              onSwipeLeft: () => _showAction('Swipe Left'),
              onSwipeRight: () => _showAction('Swipe Right'),
              onSwipeUp: () => _showAction('Swipe Up'),
              onSwipeDown: () => _showAction('Swipe Down'),
              highlightColor: Colors.purple,
              child: InteractiveCard(
                title: 'Email Notification',
                subtitle: 'You have 3 unread messages',
                icon: Icons.email,
                color: Colors.blue,
              ),
            ),
            
            const SizedBox(height: 16),
            
            SwipeActionWidget(
              onTap: () => _showAction('Calendar Tap'),
              onDoubleTap: () => _showAction('Calendar Double Tap'),
              onLongPress: () => _showAction('Calendar Long Press'),
              onSwipeLeft: () => _showAction('Archive Calendar'),
              onSwipeRight: () => _showAction('Mark Calendar Done'),
              onSwipeUp: () => _showAction('Calendar Priority Up'),
              onSwipeDown: () => _showAction('Calendar Priority Down'),
              highlightColor: Colors.green,
              child: InteractiveCard(
                title: 'Meeting Reminder',
                subtitle: 'Team standup in 30 minutes',
                icon: Icons.calendar_today,
                color: Colors.green,
              ),
            ),
            
            const SizedBox(height: 16),
            
            SwipeActionWidget(
              onTap: () => _showAction('Task Tap'),
              onDoubleTap: () => _showAction('Task Double Tap'),
              onLongPress: () => _showAction('Task Long Press'),
              onSwipeLeft: () => _showAction('Delete Task'),
              onSwipeRight: () => _showAction('Complete Task'),
              onSwipeUp: () => _showAction('Task High Priority'),
              onSwipeDown: () => _showAction('Task Low Priority'),
              highlightColor: Colors.orange,
              child: InteractiveCard(
                title: 'Project Deadline',
                subtitle: 'Submit final report by Friday',
                icon: Icons.assignment,
                color: Colors.orange,
              ),
            ),
            
            const SizedBox(height: 32),
            
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Gesture Guide',
                      style: Theme.of(context).textTheme.titleMedium?.copyWith(
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 12),
                    _buildGestureItem('Tap', 'Single touch', Icons.touch_app),
                    _buildGestureItem('Double Tap', 'Two quick taps', Icons.double_arrow),
                    _buildGestureItem('Long Press', 'Hold for 500ms', Icons.press_and_hold),
                    _buildGestureItem('Swipe Left', 'Drag left to delete', Icons.swipe_left),
                    _buildGestureItem('Swipe Right', 'Drag right to complete', Icons.swipe_right),
                    _buildGestureItem('Swipe Up', 'Drag up for priority', Icons.swipe_up),
                    _buildGestureItem('Swipe Down', 'Drag down to defer', Icons.swipe_down),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildGestureItem(String gesture, String description, IconData icon) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        children: [
          Icon(
            icon,
            size: 20,
            color: Theme.of(context).colorScheme.primary,
          ),
          const SizedBox(width: 12),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  gesture,
                  style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                    fontWeight: FontWeight.w600,
                  ),
                ),
                Text(
                  description,
                  style: Theme.of(context).textTheme.bodySmall?.copyWith(
                    color: Theme.of(context).colorScheme.onSurface.withOpacity(0.6),
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}
```

This gesture widget provides comprehensive touch interaction handling  
including taps, swipes, long press, and visual feedback. Features include  
directional swipe detection, animation feedback, threshold configuration,  
and multi-gesture support for creating rich interactive experiences.  

## Dynamic Color Palette Widget

Creating widgets that generate and apply dynamic color schemes and palettes.  

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
      title: 'Dynamic Color Palette Widget',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const ColorPaletteDemoPage(),
    );
  }
}

class ColorPalette {
  final String name;
  final Color primary;
  final List<Color> colors;
  final ColorScheme lightScheme;
  final ColorScheme darkScheme;

  ColorPalette({
    required this.name,
    required this.primary,
    required this.colors,
    required this.lightScheme,
    required this.darkScheme,
  });

  static ColorPalette fromSeedColor(Color seedColor, String name) {
    final lightScheme = ColorScheme.fromSeed(
      seedColor: seedColor,
      brightness: Brightness.light,
    );
    
    final darkScheme = ColorScheme.fromSeed(
      seedColor: seedColor,
      brightness: Brightness.dark,
    );

    final colors = _generateColorShades(seedColor);

    return ColorPalette(
      name: name,
      primary: seedColor,
      colors: colors,
      lightScheme: lightScheme,
      darkScheme: darkScheme,
    );
  }

  static List<Color> _generateColorShades(Color baseColor) {
    final hsl = HSLColor.fromColor(baseColor);
    final shades = <Color>[];
    
    // Generate lighter shades
    for (int i = 0; i < 5; i++) {
      final lightness = math.min(1.0, hsl.lightness + (0.15 * (i + 1)));
      shades.add(hsl.withLightness(lightness).toColor());
    }
    
    // Add base color
    shades.add(baseColor);
    
    // Generate darker shades
    for (int i = 0; i < 5; i++) {
      final lightness = math.max(0.0, hsl.lightness - (0.15 * (i + 1)));
      shades.add(hsl.withLightness(lightness).toColor());
    }
    
    return shades;
  }

  static ColorPalette generateRandomPalette() {
    final random = math.Random();
    final hue = random.nextDouble() * 360;
    final saturation = 0.5 + (random.nextDouble() * 0.5);
    final lightness = 0.3 + (random.nextDouble() * 0.4);
    
    final baseColor = HSLColor.fromAHSL(1.0, hue, saturation, lightness).toColor();
    
    return ColorPalette.fromSeedColor(
      baseColor,
      'Random Palette ${random.nextInt(1000)}',
    );
  }

  static List<ColorPalette> getPresetPalettes() {
    return [
      ColorPalette.fromSeedColor(Colors.blue, 'Ocean Blue'),
      ColorPalette.fromSeedColor(Colors.green, 'Forest Green'),
      ColorPalette.fromSeedColor(Colors.red, 'Sunset Red'),
      ColorPalette.fromSeedColor(Colors.purple, 'Royal Purple'),
      ColorPalette.fromSeedColor(Colors.orange, 'Autumn Orange'),
      ColorPalette.fromSeedColor(Colors.teal, 'Teal Breeze'),
      ColorPalette.fromSeedColor(Colors.indigo, 'Deep Indigo'),
      ColorPalette.fromSeedColor(Colors.pink, 'Rose Pink'),
    ];
  }
}

class DynamicColorWidget extends StatefulWidget {
  final ColorPalette palette;
  final bool isDarkMode;
  final Widget child;

  const DynamicColorWidget({
    super.key,
    required this.palette,
    required this.isDarkMode,
    required this.child,
  });

  @override
  State<DynamicColorWidget> createState() => _DynamicColorWidgetState();
}

class _DynamicColorWidgetState extends State<DynamicColorWidget>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _colorAnimation;

  @override
  void initState() {
    super.initState();
    
    _controller = AnimationController(
      duration: const Duration(milliseconds: 800),
      vsync: this,
    );
    
    _colorAnimation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    ));
    
    _controller.forward();
  }

  @override
  void didUpdateWidget(DynamicColorWidget oldWidget) {
    super.didUpdateWidget(oldWidget);
    
    if (oldWidget.palette != widget.palette || 
        oldWidget.isDarkMode != widget.isDarkMode) {
      _controller.reset();
      _controller.forward();
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _colorAnimation,
      builder: (context, child) {
        final scheme = widget.isDarkMode 
            ? widget.palette.darkScheme 
            : widget.palette.lightScheme;
            
        return Theme(
          data: Theme.of(context).copyWith(
            colorScheme: scheme,
          ),
          child: widget.child,
        );
      },
    );
  }
}

class ColorPaletteSelector extends StatefulWidget {
  final List<ColorPalette> palettes;
  final ColorPalette selectedPalette;
  final ValueChanged<ColorPalette> onPaletteChanged;

  const ColorPaletteSelector({
    super.key,
    required this.palettes,
    required this.selectedPalette,
    required this.onPaletteChanged,
  });

  @override
  State<ColorPaletteSelector> createState() => _ColorPaletteSelectorState();
}

class _ColorPaletteSelectorState extends State<ColorPaletteSelector> {
  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          'Select Color Palette',
          style: Theme.of(context).textTheme.titleMedium?.copyWith(
            fontWeight: FontWeight.bold,
          ),
        ),
        const SizedBox(height: 16),
        SizedBox(
          height: 120,
          child: ListView.builder(
            scrollDirection: Axis.horizontal,
            itemCount: widget.palettes.length,
            itemBuilder: (context, index) {
              final palette = widget.palettes[index];
              final isSelected = palette == widget.selectedPalette;
              
              return GestureDetector(
                onTap: () => widget.onPaletteChanged(palette),
                child: Container(
                  width: 100,
                  margin: const EdgeInsets.only(right: 12),
                  decoration: BoxDecoration(
                    borderRadius: BorderRadius.circular(12),
                    border: isSelected
                        ? Border.all(
                            color: Theme.of(context).colorScheme.primary,
                            width: 3,
                          )
                        : null,
                    boxShadow: isSelected
                        ? [
                            BoxShadow(
                              color: Theme.of(context).colorScheme.primary.withOpacity(0.3),
                              blurRadius: 8,
                              spreadRadius: 2,
                            ),
                          ]
                        : [
                            BoxShadow(
                              color: Colors.black.withOpacity(0.1),
                              blurRadius: 4,
                              offset: const Offset(0, 2),
                            ),
                          ],
                  ),
                  child: Column(
                    children: [
                      Expanded(
                        child: Container(
                          decoration: BoxDecoration(
                            borderRadius: const BorderRadius.vertical(
                              top: Radius.circular(12),
                            ),
                            gradient: LinearGradient(
                              colors: palette.colors.take(5).toList(),
                              begin: Alignment.topCenter,
                              end: Alignment.bottomCenter,
                            ),
                          ),
                        ),
                      ),
                      Container(
                        padding: const EdgeInsets.all(8),
                        decoration: BoxDecoration(
                          color: Theme.of(context).colorScheme.surface,
                          borderRadius: const BorderRadius.vertical(
                            bottom: Radius.circular(12),
                          ),
                        ),
                        child: Text(
                          palette.name,
                          style: Theme.of(context).textTheme.bodySmall?.copyWith(
                            fontWeight: FontWeight.w600,
                          ),
                          textAlign: TextAlign.center,
                          maxLines: 2,
                          overflow: TextOverflow.ellipsis,
                        ),
                      ),
                    ],
                  ),
                ),
              );
            },
          ),
        ),
      ],
    );
  }
}

class ColorShadeDisplay extends StatelessWidget {
  final List<Color> colors;
  final String title;

  const ColorShadeDisplay({
    super.key,
    required this.colors,
    required this.title,
  });

  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          title,
          style: Theme.of(context).textTheme.titleMedium?.copyWith(
            fontWeight: FontWeight.bold,
          ),
        ),
        const SizedBox(height: 8),
        Container(
          height: 60,
          decoration: BoxDecoration(
            borderRadius: BorderRadius.circular(8),
            boxShadow: [
              BoxShadow(
                color: Colors.black.withOpacity(0.1),
                blurRadius: 4,
                offset: const Offset(0, 2),
              ),
            ],
          ),
          child: Row(
            children: colors.asMap().entries.map((entry) {
              final index = entry.key;
              final color = entry.value;
              final isFirst = index == 0;
              final isLast = index == colors.length - 1;
              
              return Expanded(
                child: GestureDetector(
                  onTap: () {
                    ScaffoldMessenger.of(context).showSnackBar(
                      SnackBar(
                        content: Text(
                          'Color: ${color.value.toRadixString(16).toUpperCase()}',
                        ),
                        backgroundColor: color,
                      ),
                    );
                  },
                  child: Container(
                    decoration: BoxDecoration(
                      color: color,
                      borderRadius: BorderRadius.only(
                        topLeft: isFirst ? const Radius.circular(8) : Radius.zero,
                        bottomLeft: isFirst ? const Radius.circular(8) : Radius.zero,
                        topRight: isLast ? const Radius.circular(8) : Radius.zero,
                        bottomRight: isLast ? const Radius.circular(8) : Radius.zero,
                      ),
                    ),
                    child: Center(
                      child: Text(
                        '${index + 1}',
                        style: TextStyle(
                          color: _getContrastColor(color),
                          fontSize: 12,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                  ),
                ),
              );
            }).toList(),
          ),
        ),
      ],
    );
  }

  Color _getContrastColor(Color color) {
    final luminance = color.computeLuminance();
    return luminance > 0.5 ? Colors.black : Colors.white;
  }
}

class ColorPaletteDemoPage extends StatefulWidget {
  const ColorPaletteDemoPage({super.key});

  @override
  State<ColorPaletteDemoPage> createState() => _ColorPaletteDemoPageState();
}

class _ColorPaletteDemoPageState extends State<ColorPaletteDemoPage> {
  late List<ColorPalette> _palettes;
  late ColorPalette _selectedPalette;
  bool _isDarkMode = false;

  @override
  void initState() {
    super.initState();
    
    _palettes = ColorPalette.getPresetPalettes();
    _selectedPalette = _palettes.first;
  }

  void _addRandomPalette() {
    setState(() {
      _palettes.add(ColorPalette.generateRandomPalette());
    });
  }

  @override
  Widget build(BuildContext context) {
    return DynamicColorWidget(
      palette: _selectedPalette,
      isDarkMode: _isDarkMode,
      child: Builder(
        builder: (context) => Scaffold(
          appBar: AppBar(
            title: const Text('Dynamic Color Palette'),
            backgroundColor: Theme.of(context).colorScheme.inversePrimary,
            actions: [
              Switch(
                value: _isDarkMode,
                onChanged: (value) {
                  setState(() {
                    _isDarkMode = value;
                  });
                },
              ),
              IconButton(
                icon: const Icon(Icons.add),
                onPressed: _addRandomPalette,
                tooltip: 'Add Random Palette',
              ),
            ],
          ),
          body: SingleChildScrollView(
            padding: const EdgeInsets.all(16),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                ColorPaletteSelector(
                  palettes: _palettes,
                  selectedPalette: _selectedPalette,
                  onPaletteChanged: (palette) {
                    setState(() {
                      _selectedPalette = palette;
                    });
                  },
                ),
                
                const SizedBox(height: 32),
                
                ColorShadeDisplay(
                  colors: _selectedPalette.colors,
                  title: '${_selectedPalette.name} Shades',
                ),
                
                const SizedBox(height: 32),
                
                Text(
                  'Theme Preview',
                  style: Theme.of(context).textTheme.titleMedium?.copyWith(
                    fontWeight: FontWeight.bold,
                  ),
                ),
                const SizedBox(height: 16),
                
                Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16),
                    child: Column(
                      children: [
                        ListTile(
                          leading: CircleAvatar(
                            backgroundColor: Theme.of(context).colorScheme.primary,
                            child: Icon(
                              Icons.person,
                              color: Theme.of(context).colorScheme.onPrimary,
                            ),
                          ),
                          title: const Text('John Doe'),
                          subtitle: const Text('Software Developer'),
                          trailing: const Icon(Icons.more_vert),
                        ),
                        const SizedBox(height: 16),
                        Row(
                          children: [
                            Expanded(
                              child: ElevatedButton(
                                onPressed: () {},
                                child: const Text('Primary'),
                              ),
                            ),
                            const SizedBox(width: 8),
                            Expanded(
                              child: OutlinedButton(
                                onPressed: () {},
                                child: const Text('Secondary'),
                              ),
                            ),
                          ],
                        ),
                        const SizedBox(height: 8),
                        Row(
                          children: [
                            Expanded(
                              child: FilledButton(
                                onPressed: () {},
                                child: const Text('Filled'),
                              ),
                            ),
                            const SizedBox(width: 8),
                            Expanded(
                              child: TextButton(
                                onPressed: () {},
                                child: const Text('Text'),
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
                  color: Theme.of(context).colorScheme.primaryContainer,
                  child: Padding(
                    padding: const EdgeInsets.all(16),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          'Color Information',
                          style: Theme.of(context).textTheme.titleMedium?.copyWith(
                            color: Theme.of(context).colorScheme.onPrimaryContainer,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        const SizedBox(height: 8),
                        Text(
                          'Primary: #${_selectedPalette.primary.value.toRadixString(16).substring(2).toUpperCase()}',
                          style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                            color: Theme.of(context).colorScheme.onPrimaryContainer,
                          ),
                        ),
                        Text(
                          'Mode: ${_isDarkMode ? 'Dark' : 'Light'}',
                          style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                            color: Theme.of(context).colorScheme.onPrimaryContainer,
                          ),
                        ),
                        Text(
                          'Shades: ${_selectedPalette.colors.length}',
                          style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                            color: Theme.of(context).colorScheme.onPrimaryContainer,
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
      ),
    );
  }
}
```

This dynamic color palette system generates comprehensive color schemes  
from seed colors, provides theme switching, and creates harmonious color  
variations. Features include preset and random palettes, dark/light mode  
support, and real-time theme preview functionality.  

## Floating Action Button Variants

Creating sophisticated floating action button variations with custom behaviors.  

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
      title: 'Floating Action Button Variants',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
      ),
      home: const FABVariantsDemoPage(),
    );
  }
}

enum FABAction {
  add,
  edit,
  delete,
  share,
  favorite,
  message,
  call,
  settings,
}

class FABActionData {
  final FABAction action;
  final IconData icon;
  final String label;
  final Color? color;
  final VoidCallback? onPressed;

  FABActionData({
    required this.action,
    required this.icon,
    required this.label,
    this.color,
    this.onPressed,
  });
}

class ExpandableFAB extends StatefulWidget {
  final List<FABActionData> actions;
  final IconData primaryIcon;
  final Color? backgroundColor;
  final Duration animationDuration;
  final double spacing;

  const ExpandableFAB({
    super.key,
    required this.actions,
    this.primaryIcon = Icons.add,
    this.backgroundColor,
    this.animationDuration = const Duration(milliseconds: 300),
    this.spacing = 60.0,
  });

  @override
  State<ExpandableFAB> createState() => _ExpandableFABState();
}

class _ExpandableFABState extends State<ExpandableFAB>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late AnimationController _labelController;
  late List<Animation<double>> _itemAnimations;
  late Animation<double> _rotationAnimation;
  late Animation<double> _labelAnimation;
  
  bool _isExpanded = false;

  @override
  void initState() {
    super.initState();
    
    _controller = AnimationController(
      duration: widget.animationDuration,
      vsync: this,
    );
    
    _labelController = AnimationController(
      duration: const Duration(milliseconds: 200),
      vsync: this,
    );

    _rotationAnimation = Tween<double>(
      begin: 0,
      end: 0.75,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    ));

    _labelAnimation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(_labelController);

    _itemAnimations = widget.actions.asMap().entries.map((entry) {
      final index = entry.key;
      final begin = 0.8 + (index * 0.1);
      final end = math.min(1.0, begin + 0.2);
      
      return Tween<double>(
        begin: 0,
        end: 1,
      ).animate(CurvedAnimation(
        parent: _controller,
        curve: Interval(begin, end, curve: Curves.elasticOut),
      ));
    }).toList();
  }

  @override
  void dispose() {
    _controller.dispose();
    _labelController.dispose();
    super.dispose();
  }

  void _toggle() {
    setState(() {
      _isExpanded = !_isExpanded;
    });
    
    if (_isExpanded) {
      _controller.forward();
      _labelController.forward();
    } else {
      _controller.reverse();
      _labelController.reverse();
    }
  }

  @override
  Widget build(BuildContext context) {
    return Stack(
      clipBehavior: Clip.none,
      children: [
        // Backdrop
        if (_isExpanded)
          Positioned.fill(
            child: GestureDetector(
              onTap: _toggle,
              child: AnimatedBuilder(
                animation: _controller,
                builder: (context, child) {
                  return Container(
                    color: Colors.black.withOpacity(0.3 * _controller.value),
                  );
                },
              ),
            ),
          ),
        
        // Action items
        ...widget.actions.asMap().entries.map((entry) {
          final index = entry.key;
          final actionData = entry.value;
          final animation = _itemAnimations[index];
          final offset = widget.spacing * (index + 1);

          return AnimatedBuilder(
            animation: animation,
            builder: (context, child) {
              final animationValue = animation.value;
              
              return Positioned(
                bottom: offset * animationValue,
                right: 0,
                child: Transform.scale(
                  scale: animationValue,
                  child: Opacity(
                    opacity: animationValue,
                    child: Row(
                      mainAxisSize: MainAxisSize.min,
                      children: [
                        // Label
                        AnimatedBuilder(
                          animation: _labelAnimation,
                          builder: (context, child) {
                            return Transform.translate(
                              offset: Offset(20 * (1 - _labelAnimation.value), 0),
                              child: Opacity(
                                opacity: _labelAnimation.value,
                                child: Container(
                                  padding: const EdgeInsets.symmetric(
                                    horizontal: 12,
                                    vertical: 6,
                                  ),
                                  decoration: BoxDecoration(
                                    color: Theme.of(context).colorScheme.surface,
                                    borderRadius: BorderRadius.circular(16),
                                    boxShadow: [
                                      BoxShadow(
                                        color: Colors.black.withOpacity(0.2),
                                        blurRadius: 4,
                                        offset: const Offset(0, 2),
                                      ),
                                    ],
                                  ),
                                  child: Text(
                                    actionData.label,
                                    style: Theme.of(context).textTheme.bodySmall?.copyWith(
                                      fontWeight: FontWeight.w600,
                                    ),
                                  ),
                                ),
                              ),
                            );
                          },
                        ),
                        const SizedBox(width: 12),
                        
                        // Action button
                        FloatingActionButton.small(
                          heroTag: 'fab_${actionData.action.name}',
                          onPressed: () {
                            actionData.onPressed?.call();
                            _toggle();
                          },
                          backgroundColor: actionData.color ??
                              Theme.of(context).colorScheme.secondary,
                          child: Icon(actionData.icon),
                        ),
                      ],
                    ),
                  ),
                ),
              );
            },
          );
        }),
        
        // Main FAB
        AnimatedBuilder(
          animation: _rotationAnimation,
          builder: (context, child) {
            return Transform.rotate(
              angle: _rotationAnimation.value * 2 * math.pi,
              child: FloatingActionButton(
                heroTag: 'main_fab',
                onPressed: _toggle,
                backgroundColor: widget.backgroundColor,
                child: Icon(
                  _isExpanded ? Icons.close : widget.primaryIcon,
                ),
              ),
            );
          },
        ),
      ],
    );
  }
}

class MorphingFAB extends StatefulWidget {
  final List<FABActionData> actions;
  final Duration morphDuration;
  final double size;

  const MorphingFAB({
    super.key,
    required this.actions,
    this.morphDuration = const Duration(milliseconds: 400),
    this.size = 56,
  });

  @override
  State<MorphingFAB> createState() => _MorphingFABState();
}

class _MorphingFABState extends State<MorphingFAB>
    with TickerProviderStateMixin {
  late AnimationController _morphController;
  late Animation<double> _morphAnimation;
  
  int _currentIndex = 0;
  bool _isMorphing = false;

  @override
  void initState() {
    super.initState();
    
    _morphController = AnimationController(
      duration: widget.morphDuration,
      vsync: this,
    );
    
    _morphAnimation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _morphController,
      curve: Curves.easeInOut,
    ));
  }

  @override
  void dispose() {
    _morphController.dispose();
    super.dispose();
  }

  void _morphToNext() async {
    if (_isMorphing || widget.actions.isEmpty) return;
    
    setState(() {
      _isMorphing = true;
    });
    
    await _morphController.forward();
    
    setState(() {
      _currentIndex = (_currentIndex + 1) % widget.actions.length;
    });
    
    await _morphController.reverse();
    
    setState(() {
      _isMorphing = false;
    });
  }

  @override
  Widget build(BuildContext context) {
    if (widget.actions.isEmpty) {
      return const SizedBox.shrink();
    }
    
    final currentAction = widget.actions[_currentIndex];
    final nextAction = widget.actions[(_currentIndex + 1) % widget.actions.length];
    
    return GestureDetector(
      onTap: _isMorphing ? null : currentAction.onPressed,
      onLongPress: _morphToNext,
      child: AnimatedBuilder(
        animation: _morphAnimation,
        builder: (context, child) {
          final progress = _morphAnimation.value;
          final scale = 1.0 + (progress * 0.2);
          
          return Transform.scale(
            scale: scale,
            child: Container(
              width: widget.size,
              height: widget.size,
              decoration: BoxDecoration(
                shape: BoxShape.circle,
                color: Color.lerp(
                  currentAction.color ?? Theme.of(context).colorScheme.primary,
                  nextAction.color ?? Theme.of(context).colorScheme.primary,
                  progress,
                ),
                boxShadow: [
                  BoxShadow(
                    color: Colors.black.withOpacity(0.3),
                    blurRadius: 8 + (progress * 4),
                    offset: Offset(0, 4 + (progress * 2)),
                  ),
                ],
              ),
              child: Stack(
                alignment: Alignment.center,
                children: [
                  Opacity(
                    opacity: 1 - progress,
                    child: Icon(
                      currentAction.icon,
                      color: Colors.white,
                      size: 24,
                    ),
                  ),
                  Opacity(
                    opacity: progress,
                    child: Icon(
                      nextAction.icon,
                      color: Colors.white,
                      size: 24,
                    ),
                  ),
                ],
              ),
            ),
          );
        },
      ),
    );
  }
}

class DragFAB extends StatefulWidget {
  final Widget child;
  final VoidCallback? onPressed;
  final VoidCallback? onDragEnd;

  const DragFAB({
    super.key,
    required this.child,
    this.onPressed,
    this.onDragEnd,
  });

  @override
  State<DragFAB> createState() => _DragFABState();
}

class _DragFABState extends State<DragFAB> with TickerProviderStateMixin {
  late AnimationController _snapController;
  late Animation<Offset> _snapAnimation;
  
  Offset _position = Offset.zero;
  bool _isDragging = false;

  @override
  void initState() {
    super.initState();
    
    _snapController = AnimationController(
      duration: const Duration(milliseconds: 300),
      vsync: this,
    );
  }

  @override
  void dispose() {
    _snapController.dispose();
    super.dispose();
  }

  void _snapToEdge() {
    final screenWidth = MediaQuery.of(context).size.width;
    final snapToLeft = _position.dx < screenWidth / 2;
    final targetX = snapToLeft ? -screenWidth / 2 + 28 : screenWidth / 2 - 28;
    
    _snapAnimation = Tween<Offset>(
      begin: _position,
      end: Offset(targetX, _position.dy),
    ).animate(CurvedAnimation(
      parent: _snapController,
      curve: Curves.easeOut,
    ));
    
    _snapController.forward().then((_) {
      setState(() {
        _position = _snapAnimation.value;
      });
      _snapController.reset();
      widget.onDragEnd?.call();
    });
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _snapController,
      builder: (context, child) {
        final currentPosition = _snapController.isAnimating 
            ? _snapAnimation.value 
            : _position;
            
        return Transform.translate(
          offset: currentPosition,
          child: GestureDetector(
            onTap: _isDragging ? null : widget.onPressed,
            onPanStart: (details) {
              setState(() {
                _isDragging = true;
              });
            },
            onPanUpdate: (details) {
              setState(() {
                _position += details.delta;
              });
            },
            onPanEnd: (details) {
              setState(() {
                _isDragging = false;
              });
              _snapToEdge();
            },
            child: Transform.scale(
              scale: _isDragging ? 1.1 : 1.0,
              child: widget.child,
            ),
          ),
        );
      },
    );
  }
}

class FABVariantsDemoPage extends StatefulWidget {
  const FABVariantsDemoPage({super.key});

  @override
  State<FABVariantsDemoPage> createState() => _FABVariantsDemoPageState();
}

class _FABVariantsDemoPageState extends State<FABVariantsDemoPage>
    with TickerProviderStateMixin {
  int _selectedVariant = 0;
  String _lastAction = 'No action yet';

  void _showAction(String action) {
    setState(() {
      _lastAction = action;
    });
    
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(action),
        duration: const Duration(seconds: 1),
      ),
    );
  }

  List<FABActionData> _getActions() {
    return [
      FABActionData(
        action: FABAction.add,
        icon: Icons.add,
        label: 'Add Item',
        color: Colors.green,
        onPressed: () => _showAction('Add Item'),
      ),
      FABActionData(
        action: FABAction.edit,
        icon: Icons.edit,
        label: 'Edit',
        color: Colors.blue,
        onPressed: () => _showAction('Edit Item'),
      ),
      FABActionData(
        action: FABAction.share,
        icon: Icons.share,
        label: 'Share',
        color: Colors.orange,
        onPressed: () => _showAction('Share Item'),
      ),
      FABActionData(
        action: FABAction.delete,
        icon: Icons.delete,
        label: 'Delete',
        color: Colors.red,
        onPressed: () => _showAction('Delete Item'),
      ),
    ];
  }

  Widget _buildVariantSelector() {
    final variants = ['Expandable', 'Morphing', 'Draggable'];
    
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'FAB Variants',
              style: Theme.of(context).textTheme.titleMedium?.copyWith(
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 12),
            Wrap(
              spacing: 8,
              children: variants.asMap().entries.map((entry) {
                final index = entry.key;
                final variant = entry.value;
                final isSelected = _selectedVariant == index;
                
                return ChoiceChip(
                  label: Text(variant),
                  selected: isSelected,
                  onSelected: (selected) {
                    setState(() {
                      _selectedVariant = index;
                    });
                  },
                );
              }).toList(),
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
        title: const Text('Floating Action Button Variants'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            _buildVariantSelector(),
            
            const SizedBox(height: 24),
            
            Card(
              color: Theme.of(context).colorScheme.primaryContainer,
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Last Action',
                      style: Theme.of(context).textTheme.titleMedium?.copyWith(
                        color: Theme.of(context).colorScheme.onPrimaryContainer,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    const SizedBox(height: 8),
                    Text(
                      _lastAction,
                      style: Theme.of(context).textTheme.bodyLarge?.copyWith(
                        color: Theme.of(context).colorScheme.onPrimaryContainer,
                      ),
                    ),
                  ],
                ),
              ),
            ),
            
            const SizedBox(height: 24),
            
            Text(
              'Instructions',
              style: Theme.of(context).textTheme.titleMedium?.copyWith(
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 8),
            
            Card(
              child: Padding(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    _buildInstruction('Expandable FAB', 
                        'Tap to expand/collapse action menu'),
                    _buildInstruction('Morphing FAB', 
                        'Tap to use, long press to morph'),
                    _buildInstruction('Draggable FAB', 
                        'Drag around screen, snaps to edges'),
                  ],
                ),
              ),
            ),
            
            const SizedBox(height: 100), // Space for FAB
          ],
        ),
      ),
      floatingActionButton: _buildSelectedFAB(),
    );
  }

  Widget _buildInstruction(String title, String description) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            title,
            style: Theme.of(context).textTheme.bodyMedium?.copyWith(
              fontWeight: FontWeight.w600,
            ),
          ),
          Text(
            description,
            style: Theme.of(context).textTheme.bodySmall?.copyWith(
              color: Theme.of(context).colorScheme.onSurface.withOpacity(0.7),
            ),
          ),
          const SizedBox(height: 8),
        ],
      ),
    );
  }

  Widget _buildSelectedFAB() {
    switch (_selectedVariant) {
      case 0:
        return ExpandableFAB(
          actions: _getActions(),
          primaryIcon: Icons.menu,
        );
      case 1:
        return MorphingFAB(
          actions: _getActions(),
        );
      case 2:
        return DragFAB(
          onPressed: () => _showAction('Draggable FAB Pressed'),
          onDragEnd: () => _showAction('FAB Snapped to Edge'),
          child: FloatingActionButton(
            onPressed: null,
            child: const Icon(Icons.drag_indicator),
          ),
        );
      default:
        return FloatingActionButton(
          onPressed: () => _showAction('Standard FAB'),
          child: const Icon(Icons.add),
        );
    }
  }
}
```

This floating action button collection provides expandable menus, morphing  
animations, and draggable functionality. Features include smooth  
transitions, customizable actions, edge snapping, and multiple interaction  
patterns for enhanced user experience in mobile applications.  

## Advanced Form Builder Widget

Creating sophisticated form widgets with dynamic field generation and validation.  

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
      title: 'Advanced Form Builder',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const FormBuilderDemoPage(),
    );
  }
}

enum FormFieldType {
  text,
  email,
  password,
  number,
  phone,
  dropdown,
  checkbox,
  radio,
  slider,
  date,
  time,
  textArea,
}

class FormFieldConfig {
  final String key;
  final String label;
  final FormFieldType type;
  final String? hint;
  final bool required;
  final String? Function(String?)? validator;
  final List<String>? options;
  final double? min;
  final double? max;
  final int? maxLines;
  final dynamic defaultValue;
  final Map<String, dynamic>? properties;

  FormFieldConfig({
    required this.key,
    required this.label,
    required this.type,
    this.hint,
    this.required = false,
    this.validator,
    this.options,
    this.min,
    this.max,
    this.maxLines,
    this.defaultValue,
    this.properties,
  });
}

class DynamicFormBuilder extends StatefulWidget {
  final List<FormFieldConfig> fields;
  final void Function(Map<String, dynamic>)? onSubmit;
  final String submitButtonText;
  final bool showResetButton;

  const DynamicFormBuilder({
    super.key,
    required this.fields,
    this.onSubmit,
    this.submitButtonText = 'Submit',
    this.showResetButton = true,
  });

  @override
  State<DynamicFormBuilder> createState() => _DynamicFormBuilderState();
}

class _DynamicFormBuilderState extends State<DynamicFormBuilder> {
  final _formKey = GlobalKey<FormState>();
  final Map<String, dynamic> _formData = {};
  final Map<String, TextEditingController> _controllers = {};

  @override
  void initState() {
    super.initState();
    _initializeFormData();
  }

  @override
  void dispose() {
    for (var controller in _controllers.values) {
      controller.dispose();
    }
    super.dispose();
  }

  void _initializeFormData() {
    for (var field in widget.fields) {
      _formData[field.key] = field.defaultValue;
      
      if (field.type == FormFieldType.text ||
          field.type == FormFieldType.email ||
          field.type == FormFieldType.password ||
          field.type == FormFieldType.number ||
          field.type == FormFieldType.phone ||
          field.type == FormFieldType.textArea) {
        _controllers[field.key] = TextEditingController(
          text: field.defaultValue?.toString() ?? '',
        );
      }
    }
  }

  void _handleSubmit() {
    if (_formKey.currentState!.validate()) {
      _formKey.currentState!.save();
      widget.onSubmit?.call(Map.from(_formData));
    }
  }

  void _handleReset() {
    _formKey.currentState!.reset();
    for (var field in widget.fields) {
      _formData[field.key] = field.defaultValue;
      if (_controllers.containsKey(field.key)) {
        _controllers[field.key]!.text = field.defaultValue?.toString() ?? '';
      }
    }
    setState(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Form(
      key: _formKey,
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.stretch,
        children: [
          ...widget.fields.map((field) => Padding(
            padding: const EdgeInsets.only(bottom: 16),
            child: _buildField(field),
          )),
          
          const SizedBox(height: 24),
          
          Row(
            children: [
              if (widget.showResetButton) ...[
                Expanded(
                  child: OutlinedButton(
                    onPressed: _handleReset,
                    child: const Text('Reset'),
                  ),
                ),
                const SizedBox(width: 16),
              ],
              Expanded(
                flex: widget.showResetButton ? 1 : 2,
                child: ElevatedButton(
                  onPressed: _handleSubmit,
                  child: Text(widget.submitButtonText),
                ),
              ),
            ],
          ),
        ],
      ),
    );
  }

  Widget _buildField(FormFieldConfig field) {
    switch (field.type) {
      case FormFieldType.text:
      case FormFieldType.email:
      case FormFieldType.password:
      case FormFieldType.phone:
        return _buildTextFormField(field);
      
      case FormFieldType.number:
        return _buildNumberFormField(field);
      
      case FormFieldType.textArea:
        return _buildTextAreaFormField(field);
      
      case FormFieldType.dropdown:
        return _buildDropdownFormField(field);
      
      case FormFieldType.checkbox:
        return _buildCheckboxFormField(field);
      
      case FormFieldType.radio:
        return _buildRadioFormField(field);
      
      case FormFieldType.slider:
        return _buildSliderFormField(field);
      
      case FormFieldType.date:
        return _buildDateFormField(field);
      
      case FormFieldType.time:
        return _buildTimeFormField(field);
      
      default:
        return _buildTextFormField(field);
    }
  }

  Widget _buildTextFormField(FormFieldConfig field) {
    return TextFormField(
      controller: _controllers[field.key],
      decoration: InputDecoration(
        labelText: field.label + (field.required ? ' *' : ''),
        hintText: field.hint,
        border: const OutlineInputBorder(),
      ),
      keyboardType: _getKeyboardType(field.type),
      obscureText: field.type == FormFieldType.password,
      inputFormatters: _getInputFormatters(field.type),
      validator: (value) {
        if (field.required && (value == null || value.isEmpty)) {
          return '${field.label} is required';
        }
        return field.validator?.call(value);
      },
      onSaved: (value) {
        _formData[field.key] = value;
      },
    );
  }

  Widget _buildNumberFormField(FormFieldConfig field) {
    return TextFormField(
      controller: _controllers[field.key],
      decoration: InputDecoration(
        labelText: field.label + (field.required ? ' *' : ''),
        hintText: field.hint,
        border: const OutlineInputBorder(),
        suffixIcon: field.properties?['unit'] != null
            ? Padding(
                padding: const EdgeInsets.all(16),
                child: Text(field.properties!['unit']),
              )
            : null,
      ),
      keyboardType: TextInputType.number,
      inputFormatters: [FilteringTextInputFormatter.allow(RegExp(r'[0-9.]'))],
      validator: (value) {
        if (field.required && (value == null || value.isEmpty)) {
          return '${field.label} is required';
        }
        if (value != null && value.isNotEmpty) {
          final numValue = double.tryParse(value);
          if (numValue == null) {
            return 'Please enter a valid number';
          }
          if (field.min != null && numValue < field.min!) {
            return 'Value must be at least ${field.min}';
          }
          if (field.max != null && numValue > field.max!) {
            return 'Value must not exceed ${field.max}';
          }
        }
        return field.validator?.call(value);
      },
      onSaved: (value) {
        _formData[field.key] = value != null ? double.tryParse(value) : null;
      },
    );
  }

  Widget _buildTextAreaFormField(FormFieldConfig field) {
    return TextFormField(
      controller: _controllers[field.key],
      decoration: InputDecoration(
        labelText: field.label + (field.required ? ' *' : ''),
        hintText: field.hint,
        border: const OutlineInputBorder(),
        alignLabelWithHint: true,
      ),
      maxLines: field.maxLines ?? 4,
      validator: (value) {
        if (field.required && (value == null || value.isEmpty)) {
          return '${field.label} is required';
        }
        return field.validator?.call(value);
      },
      onSaved: (value) {
        _formData[field.key] = value;
      },
    );
  }

  Widget _buildDropdownFormField(FormFieldConfig field) {
    return DropdownButtonFormField<String>(
      value: _formData[field.key],
      decoration: InputDecoration(
        labelText: field.label + (field.required ? ' *' : ''),
        hintText: field.hint,
        border: const OutlineInputBorder(),
      ),
      items: field.options?.map((option) {
        return DropdownMenuItem<String>(
          value: option,
          child: Text(option),
        );
      }).toList(),
      validator: (value) {
        if (field.required && value == null) {
          return 'Please select ${field.label.toLowerCase()}';
        }
        return null;
      },
      onChanged: (value) {
        setState(() {
          _formData[field.key] = value;
        });
      },
      onSaved: (value) {
        _formData[field.key] = value;
      },
    );
  }

  Widget _buildCheckboxFormField(FormFieldConfig field) {
    return FormField<bool>(
      initialValue: _formData[field.key] ?? false,
      validator: (value) {
        if (field.required && value != true) {
          return '${field.label} is required';
        }
        return null;
      },
      onSaved: (value) {
        _formData[field.key] = value;
      },
      builder: (FormFieldState<bool> state) {
        return Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            CheckboxListTile(
              title: Text(field.label + (field.required ? ' *' : '')),
              subtitle: field.hint != null ? Text(field.hint!) : null,
              value: state.value ?? false,
              onChanged: (value) {
                state.didChange(value);
                _formData[field.key] = value;
              },
              controlAffinity: ListTileControlAffinity.leading,
              contentPadding: EdgeInsets.zero,
            ),
            if (state.hasError)
              Padding(
                padding: const EdgeInsets.only(left: 16, top: 8),
                child: Text(
                  state.errorText!,
                  style: TextStyle(
                    color: Theme.of(context).colorScheme.error,
                    fontSize: 12,
                  ),
                ),
              ),
          ],
        );
      },
    );
  }

  Widget _buildRadioFormField(FormFieldConfig field) {
    return FormField<String>(
      initialValue: _formData[field.key],
      validator: (value) {
        if (field.required && value == null) {
          return 'Please select an option for ${field.label.toLowerCase()}';
        }
        return null;
      },
      onSaved: (value) {
        _formData[field.key] = value;
      },
      builder: (FormFieldState<String> state) {
        return Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              field.label + (field.required ? ' *' : ''),
              style: Theme.of(context).textTheme.titleMedium,
            ),
            if (field.hint != null) ...[
              const SizedBox(height: 4),
              Text(
                field.hint!,
                style: Theme.of(context).textTheme.bodySmall?.copyWith(
                  color: Theme.of(context).colorScheme.onSurface.withOpacity(0.6),
                ),
              ),
            ],
            const SizedBox(height: 8),
            ...field.options?.map((option) {
              return RadioListTile<String>(
                title: Text(option),
                value: option,
                groupValue: state.value,
                onChanged: (value) {
                  state.didChange(value);
                  _formData[field.key] = value;
                },
                contentPadding: EdgeInsets.zero,
              );
            }) ?? [],
            if (state.hasError)
              Padding(
                padding: const EdgeInsets.only(top: 8),
                child: Text(
                  state.errorText!,
                  style: TextStyle(
                    color: Theme.of(context).colorScheme.error,
                    fontSize: 12,
                  ),
                ),
              ),
          ],
        );
      },
    );
  }

  Widget _buildSliderFormField(FormFieldConfig field) {
    return FormField<double>(
      initialValue: _formData[field.key]?.toDouble() ?? field.min ?? 0.0,
      onSaved: (value) {
        _formData[field.key] = value;
      },
      builder: (FormFieldState<double> state) {
        return Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text(
                  field.label + (field.required ? ' *' : ''),
                  style: Theme.of(context).textTheme.titleMedium,
                ),
                Text(
                  '${state.value?.toStringAsFixed(1)}',
                  style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ],
            ),
            if (field.hint != null) ...[
              const SizedBox(height: 4),
              Text(
                field.hint!,
                style: Theme.of(context).textTheme.bodySmall?.copyWith(
                  color: Theme.of(context).colorScheme.onSurface.withOpacity(0.6),
                ),
              ),
            ],
            Slider(
              value: state.value ?? 0.0,
              min: field.min ?? 0.0,
              max: field.max ?? 100.0,
              divisions: ((field.max ?? 100.0) - (field.min ?? 0.0)).toInt(),
              onChanged: (value) {
                state.didChange(value);
                setState(() {
                  _formData[field.key] = value;
                });
              },
            ),
          ],
        );
      },
    );
  }

  Widget _buildDateFormField(FormFieldConfig field) {
    return FormField<DateTime>(
      initialValue: _formData[field.key],
      validator: (value) {
        if (field.required && value == null) {
          return 'Please select a date';
        }
        return null;
      },
      onSaved: (value) {
        _formData[field.key] = value;
      },
      builder: (FormFieldState<DateTime> state) {
        return InkWell(
          onTap: () async {
            final date = await showDatePicker(
              context: context,
              initialDate: state.value ?? DateTime.now(),
              firstDate: DateTime(1900),
              lastDate: DateTime(2100),
            );
            if (date != null) {
              state.didChange(date);
            }
          },
          child: InputDecorator(
            decoration: InputDecoration(
              labelText: field.label + (field.required ? ' *' : ''),
              hintText: field.hint ?? 'Select date',
              border: const OutlineInputBorder(),
              suffixIcon: const Icon(Icons.calendar_today),
              errorText: state.errorText,
            ),
            child: Text(
              state.value != null 
                  ? '${state.value!.day}/${state.value!.month}/${state.value!.year}'
                  : '',
            ),
          ),
        );
      },
    );
  }

  Widget _buildTimeFormField(FormFieldConfig field) {
    return FormField<TimeOfDay>(
      initialValue: _formData[field.key],
      validator: (value) {
        if (field.required && value == null) {
          return 'Please select a time';
        }
        return null;
      },
      onSaved: (value) {
        _formData[field.key] = value;
      },
      builder: (FormFieldState<TimeOfDay> state) {
        return InkWell(
          onTap: () async {
            final time = await showTimePicker(
              context: context,
              initialTime: state.value ?? TimeOfDay.now(),
            );
            if (time != null) {
              state.didChange(time);
            }
          },
          child: InputDecorator(
            decoration: InputDecoration(
              labelText: field.label + (field.required ? ' *' : ''),
              hintText: field.hint ?? 'Select time',
              border: const OutlineInputBorder(),
              suffixIcon: const Icon(Icons.access_time),
              errorText: state.errorText,
            ),
            child: Text(
              state.value != null 
                  ? state.value!.format(context)
                  : '',
            ),
          ),
        );
      },
    );
  }

  TextInputType _getKeyboardType(FormFieldType type) {
    switch (type) {
      case FormFieldType.email:
        return TextInputType.emailAddress;
      case FormFieldType.number:
        return TextInputType.number;
      case FormFieldType.phone:
        return TextInputType.phone;
      default:
        return TextInputType.text;
    }
  }

  List<TextInputFormatter>? _getInputFormatters(FormFieldType type) {
    switch (type) {
      case FormFieldType.phone:
        return [FilteringTextInputFormatter.allow(RegExp(r'[0-9+\-\s\(\)]'))];
      case FormFieldType.number:
        return [FilteringTextInputFormatter.allow(RegExp(r'[0-9.]'))];
      default:
        return null;
    }
  }
}

class FormBuilderDemoPage extends StatefulWidget {
  const FormBuilderDemoPage({super.key});

  @override
  State<FormBuilderDemoPage> createState() => _FormBuilderDemoPageState();
}

class _FormBuilderDemoPageState extends State<FormBuilderDemoPage> {
  final List<FormFieldConfig> _formFields = [
    FormFieldConfig(
      key: 'name',
      label: 'Full Name',
      type: FormFieldType.text,
      hint: 'Enter your full name',
      required: true,
    ),
    FormFieldConfig(
      key: 'email',
      label: 'Email Address',
      type: FormFieldType.email,
      hint: 'Enter your email',
      required: true,
      validator: (value) {
        if (value != null && !RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value)) {
          return 'Please enter a valid email';
        }
        return null;
      },
    ),
    FormFieldConfig(
      key: 'age',
      label: 'Age',
      type: FormFieldType.number,
      hint: 'Enter your age',
      min: 18,
      max: 100,
      properties: {'unit': 'years'},
    ),
    FormFieldConfig(
      key: 'gender',
      label: 'Gender',
      type: FormFieldType.radio,
      options: ['Male', 'Female', 'Other', 'Prefer not to say'],
      required: true,
    ),
    FormFieldConfig(
      key: 'country',
      label: 'Country',
      type: FormFieldType.dropdown,
      hint: 'Select your country',
      options: ['USA', 'Canada', 'UK', 'Germany', 'France', 'Other'],
      required: true,
    ),
    FormFieldConfig(
      key: 'experience',
      label: 'Years of Experience',
      type: FormFieldType.slider,
      hint: 'Select your years of experience',
      min: 0,
      max: 20,
      defaultValue: 5.0,
    ),
    FormFieldConfig(
      key: 'birthdate',
      label: 'Date of Birth',
      type: FormFieldType.date,
      hint: 'Select your birth date',
    ),
    FormFieldConfig(
      key: 'preferred_time',
      label: 'Preferred Contact Time',
      type: FormFieldType.time,
      hint: 'Select preferred time',
    ),
    FormFieldConfig(
      key: 'newsletter',
      label: 'Subscribe to newsletter',
      type: FormFieldType.checkbox,
      hint: 'Receive updates and promotions',
    ),
    FormFieldConfig(
      key: 'comments',
      label: 'Additional Comments',
      type: FormFieldType.textArea,
      hint: 'Any additional information...',
      maxLines: 4,
    ),
  ];

  Map<String, dynamic>? _lastSubmittedData;

  void _handleFormSubmit(Map<String, dynamic> data) {
    setState(() {
      _lastSubmittedData = data;
    });

    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('Form submitted successfully!'),
        backgroundColor: Colors.green,
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Advanced Form Builder'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Dynamic Form Example',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 8),
            Text(
              'This form is dynamically generated from field configurations.',
              style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                color: Theme.of(context).colorScheme.onSurface.withOpacity(0.7),
              ),
            ),
            const SizedBox(height: 24),
            
            DynamicFormBuilder(
              fields: _formFields,
              onSubmit: _handleFormSubmit,
              submitButtonText: 'Submit Form',
              showResetButton: true,
            ),
            
            if (_lastSubmittedData != null) ...[
              const SizedBox(height: 32),
              Card(
                color: Theme.of(context).colorScheme.primaryContainer,
                child: Padding(
                  padding: const EdgeInsets.all(16),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Last Submitted Data',
                        style: Theme.of(context).textTheme.titleMedium?.copyWith(
                          color: Theme.of(context).colorScheme.onPrimaryContainer,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      const SizedBox(height: 12),
                      ..._lastSubmittedData!.entries.map((entry) {
                        return Padding(
                          padding: const EdgeInsets.symmetric(vertical: 2),
                          child: Text(
                            '${entry.key}: ${entry.value?.toString() ?? 'null'}',
                            style: Theme.of(context).textTheme.bodySmall?.copyWith(
                              color: Theme.of(context).colorScheme.onPrimaryContainer,
                            ),
                          ),
                        );
                      }),
                    ],
                  ),
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }
}
```

This advanced form builder creates dynamic forms from configuration objects.  
Features include multiple field types, validation, conditional logic,  
custom formatters, date/time pickers, and form state management for  
building complex, data-driven forms efficiently.  

## Performance Monitoring Widget

Creating widgets that monitor and display performance metrics and diagnostics.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter/scheduler.dart';
import 'dart:async';
import 'dart:math' as math;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Performance Monitoring Widget',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.teal),
      ),
      home: const PerformanceMonitorDemoPage(),
    );
  }
}

class PerformanceMetrics {
  final double fps;
  final double frameTime;
  final int frameCount;
  final double cpuUsage;
  final double memoryUsage;
  final DateTime timestamp;

  PerformanceMetrics({
    required this.fps,
    required this.frameTime,
    required this.frameCount,
    required this.cpuUsage,
    required this.memoryUsage,
    required this.timestamp,
  });
}

class PerformanceMonitor extends ChangeNotifier {
  final List<PerformanceMetrics> _metrics = [];
  final int _maxSamples = 100;
  Timer? _timer;
  int _frameCount = 0;
  DateTime _lastFrameTime = DateTime.now();
  final List<double> _frameTimes = [];
  
  List<PerformanceMetrics> get metrics => List.unmodifiable(_metrics);
  
  PerformanceMetrics? get currentMetrics => _metrics.isNotEmpty ? _metrics.last : null;
  
  double get averageFPS {
    if (_metrics.isEmpty) return 0.0;
    final sum = _metrics.map((m) => m.fps).reduce((a, b) => a + b);
    return sum / _metrics.length;
  }
  
  double get averageFrameTime {
    if (_metrics.isEmpty) return 0.0;
    final sum = _metrics.map((m) => m.frameTime).reduce((a, b) => a + b);
    return sum / _metrics.length;
  }

  void startMonitoring() {
    _timer = Timer.periodic(const Duration(milliseconds: 500), (_) {
      _collectMetrics();
    });
    
    SchedulerBinding.instance.addPostFrameCallback(_onFrame);
  }

  void stopMonitoring() {
    _timer?.cancel();
    _timer = null;
  }

  void _onFrame(Duration timestamp) {
    _frameCount++;
    
    final now = DateTime.now();
    final frameTime = now.difference(_lastFrameTime).inMicroseconds / 1000.0;
    _frameTimes.add(frameTime);
    
    if (_frameTimes.length > 60) {
      _frameTimes.removeAt(0);
    }
    
    _lastFrameTime = now;
    
    SchedulerBinding.instance.addPostFrameCallback(_onFrame);
  }

  void _collectMetrics() {
    final now = DateTime.now();
    
    // Calculate FPS
    final fps = _frameTimes.isNotEmpty 
        ? math.min(60.0, 1000.0 / _frameTimes.last)
        : 0.0;
    
    // Calculate average frame time
    final avgFrameTime = _frameTimes.isNotEmpty
        ? _frameTimes.reduce((a, b) => a + b) / _frameTimes.length
        : 0.0;
    
    // Simulate CPU and memory usage (in a real app, you'd get these from platform channels)
    final cpuUsage = 20 + (math.Random().nextDouble() * 40);
    final memoryUsage = 50 + (math.Random().nextDouble() * 30);
    
    final metrics = PerformanceMetrics(
      fps: fps,
      frameTime: avgFrameTime,
      frameCount: _frameCount,
      cpuUsage: cpuUsage,
      memoryUsage: memoryUsage,
      timestamp: now,
    );
    
    _metrics.add(metrics);
    
    if (_metrics.length > _maxSamples) {
      _metrics.removeAt(0);
    }
    
    notifyListeners();
  }

  @override
  void dispose() {
    stopMonitoring();
    super.dispose();
  }
}

class PerformanceOverlay extends StatefulWidget {
  final Widget child;
  final bool showOverlay;
  final PerformanceMonitor monitor;

  const PerformanceOverlay({
    super.key,
    required this.child,
    required this.monitor,
    this.showOverlay = false,
  });

  @override
  State<PerformanceOverlay> createState() => _PerformanceOverlayState();
}

class _PerformanceOverlayState extends State<PerformanceOverlay> {
  @override
  Widget build(BuildContext context) {
    return Stack(
      children: [
        widget.child,
        if (widget.showOverlay)
          Positioned(
            top: MediaQuery.of(context).padding.top + 10,
            left: 10,
            child: ListenableBuilder(
              listenable: widget.monitor,
              builder: (context, child) {
                final metrics = widget.monitor.currentMetrics;
                if (metrics == null) return const SizedBox.shrink();
                
                return Container(
                  padding: const EdgeInsets.all(8),
                  decoration: BoxDecoration(
                    color: Colors.black.withOpacity(0.8),
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      Text(
                        'FPS: ${metrics.fps.toStringAsFixed(1)}',
                        style: TextStyle(
                          color: _getFPSColor(metrics.fps),
                          fontSize: 12,
                          fontFamily: 'monospace',
                        ),
                      ),
                      Text(
                        'Frame: ${metrics.frameTime.toStringAsFixed(1)}ms',
                        style: TextStyle(
                          color: _getFrameTimeColor(metrics.frameTime),
                          fontSize: 12,
                          fontFamily: 'monospace',
                        ),
                      ),
                      Text(
                        'CPU: ${metrics.cpuUsage.toStringAsFixed(1)}%',
                        style: TextStyle(
                          color: _getCPUColor(metrics.cpuUsage),
                          fontSize: 12,
                          fontFamily: 'monospace',
                        ),
                      ),
                      Text(
                        'MEM: ${metrics.memoryUsage.toStringAsFixed(1)}%',
                        style: TextStyle(
                          color: _getMemoryColor(metrics.memoryUsage),
                          fontSize: 12,
                          fontFamily: 'monospace',
                        ),
                      ),
                    ],
                  ),
                );
              },
            ),
          ),
      ],
    );
  }

  Color _getFPSColor(double fps) {
    if (fps >= 50) return Colors.green;
    if (fps >= 30) return Colors.yellow;
    return Colors.red;
  }

  Color _getFrameTimeColor(double frameTime) {
    if (frameTime <= 16.7) return Colors.green;
    if (frameTime <= 33.3) return Colors.yellow;
    return Colors.red;
  }

  Color _getCPUColor(double cpu) {
    if (cpu <= 50) return Colors.green;
    if (cpu <= 75) return Colors.yellow;
    return Colors.red;
  }

  Color _getMemoryColor(double memory) {
    if (memory <= 60) return Colors.green;
    if (memory <= 80) return Colors.yellow;
    return Colors.red;
  }
}

class PerformanceChart extends StatefulWidget {
  final List<PerformanceMetrics> metrics;
  final String title;
  final Color color;
  final double Function(PerformanceMetrics) valueExtractor;
  final double minValue;
  final double maxValue;
  final String unit;

  const PerformanceChart({
    super.key,
    required this.metrics,
    required this.title,
    required this.color,
    required this.valueExtractor,
    required this.minValue,
    required this.maxValue,
    required this.unit,
  });

  @override
  State<PerformanceChart> createState() => _PerformanceChartState();
}

class _PerformanceChartState extends State<PerformanceChart>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();
    
    _controller = AnimationController(
      duration: const Duration(milliseconds: 500),
      vsync: this,
    );
    
    _animation = Tween<double>(
      begin: 0,
      end: 1,
    ).animate(CurvedAnimation(
      parent: _controller,
      curve: Curves.easeOut,
    ));
    
    _controller.forward();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text(
                  widget.title,
                  style: Theme.of(context).textTheme.titleMedium?.copyWith(
                    fontWeight: FontWeight.bold,
                  ),
                ),
                if (widget.metrics.isNotEmpty)
                  Text(
                    '${widget.valueExtractor(widget.metrics.last).toStringAsFixed(1)} ${widget.unit}',
                    style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                      color: widget.color,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
              ],
            ),
            const SizedBox(height: 16),
            SizedBox(
              height: 120,
              child: AnimatedBuilder(
                animation: _animation,
                builder: (context, child) {
                  return CustomPaint(
                    size: const Size(double.infinity, 120),
                    painter: PerformanceChartPainter(
                      metrics: widget.metrics,
                      color: widget.color,
                      valueExtractor: widget.valueExtractor,
                      minValue: widget.minValue,
                      maxValue: widget.maxValue,
                      progress: _animation.value,
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

class PerformanceChartPainter extends CustomPainter {
  final List<PerformanceMetrics> metrics;
  final Color color;
  final double Function(PerformanceMetrics) valueExtractor;
  final double minValue;
  final double maxValue;
  final double progress;

  PerformanceChartPainter({
    required this.metrics,
    required this.color,
    required this.valueExtractor,
    required this.minValue,
    required this.maxValue,
    required this.progress,
  });

  @override
  void paint(Canvas canvas, Size size) {
    if (metrics.isEmpty) return;

    final paint = Paint()
      ..color = color
      ..strokeWidth = 2
      ..style = PaintingStyle.stroke
      ..strokeCap = StrokeCap.round;

    final fillPaint = Paint()
      ..color = color.withOpacity(0.2)
      ..style = PaintingStyle.fill;

    final path = Path();
    final fillPath = Path();
    
    final visibleMetrics = (metrics.length * progress).round();
    final metricsToShow = metrics.take(visibleMetrics).toList();
    
    if (metricsToShow.isEmpty) return;

    final stepX = size.width / math.max(1, metricsToShow.length - 1);
    
    fillPath.moveTo(0, size.height);
    
    for (int i = 0; i < metricsToShow.length; i++) {
      final value = valueExtractor(metricsToShow[i]);
      final normalizedValue = ((value - minValue) / (maxValue - minValue))
          .clamp(0.0, 1.0);
      final y = size.height - (normalizedValue * size.height);
      final x = i * stepX;
      
      if (i == 0) {
        path.moveTo(x, y);
        fillPath.lineTo(x, y);
      } else {
        path.lineTo(x, y);
        fillPath.lineTo(x, y);
      }
    }
    
    fillPath.lineTo(size.width, size.height);
    fillPath.close();
    
    canvas.drawPath(fillPath, fillPaint);
    canvas.drawPath(path, paint);
    
    // Draw dots at data points
    final dotPaint = Paint()
      ..color = color
      ..style = PaintingStyle.fill;
    
    for (int i = 0; i < metricsToShow.length; i++) {
      final value = valueExtractor(metricsToShow[i]);
      final normalizedValue = ((value - minValue) / (maxValue - minValue))
          .clamp(0.0, 1.0);
      final y = size.height - (normalizedValue * size.height);
      final x = i * stepX;
      
      canvas.drawCircle(Offset(x, y), 3, dotPaint);
    }
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

class PerformanceMonitorDemoPage extends StatefulWidget {
  const PerformanceMonitorDemoPage({super.key});

  @override
  State<PerformanceMonitorDemoPage> createState() => _PerformanceMonitorDemoPageState();
}

class _PerformanceMonitorDemoPageState extends State<PerformanceMonitorDemoPage>
    with TickerProviderStateMixin {
  final PerformanceMonitor _monitor = PerformanceMonitor();
  bool _showOverlay = false;
  bool _isStressing = false;
  late List<AnimationController> _stressControllers;

  @override
  void initState() {
    super.initState();
    _monitor.startMonitoring();
    
    // Create stress test animations
    _stressControllers = List.generate(20, (index) {
      final controller = AnimationController(
        duration: Duration(milliseconds: 500 + (index * 100)),
        vsync: this,
      );
      controller.repeat(reverse: true);
      return controller;
    });
    
    // Stop stress animations initially
    for (var controller in _stressControllers) {
      controller.stop();
    }
  }

  @override
  void dispose() {
    _monitor.stopMonitoring();
    _monitor.dispose();
    for (var controller in _stressControllers) {
      controller.dispose();
    }
    super.dispose();
  }

  void _toggleStressTest() {
    setState(() {
      _isStressing = !_isStressing;
    });
    
    if (_isStressing) {
      for (var controller in _stressControllers) {
        controller.repeat(reverse: true);
      }
    } else {
      for (var controller in _stressControllers) {
        controller.stop();
      }
    }
  }

  @override
  Widget build(BuildContext context) {
    return PerformanceOverlay(
      monitor: _monitor,
      showOverlay: _showOverlay,
      child: Scaffold(
        appBar: AppBar(
          title: const Text('Performance Monitoring'),
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          actions: [
            IconButton(
              icon: Icon(_showOverlay ? Icons.visibility_off : Icons.visibility),
              onPressed: () {
                setState(() {
                  _showOverlay = !_showOverlay;
                });
              },
              tooltip: 'Toggle Overlay',
            ),
          ],
        ),
        body: Stack(
          children: [
            SingleChildScrollView(
              padding: const EdgeInsets.all(16),
              child: ListenableBuilder(
                listenable: _monitor,
                builder: (context, child) {
                  return Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      _buildControlPanel(),
                      
                      const SizedBox(height: 24),
                      
                      _buildCurrentMetrics(),
                      
                      const SizedBox(height: 24),
                      
                      PerformanceChart(
                        metrics: _monitor.metrics,
                        title: 'FPS',
                        color: Colors.green,
                        valueExtractor: (m) => m.fps,
                        minValue: 0,
                        maxValue: 60,
                        unit: 'fps',
                      ),
                      
                      const SizedBox(height: 16),
                      
                      PerformanceChart(
                        metrics: _monitor.metrics,
                        title: 'Frame Time',
                        color: Colors.blue,
                        valueExtractor: (m) => m.frameTime,
                        minValue: 0,
                        maxValue: 50,
                        unit: 'ms',
                      ),
                      
                      const SizedBox(height: 16),
                      
                      PerformanceChart(
                        metrics: _monitor.metrics,
                        title: 'CPU Usage',
                        color: Colors.orange,
                        valueExtractor: (m) => m.cpuUsage,
                        minValue: 0,
                        maxValue: 100,
                        unit: '%',
                      ),
                      
                      const SizedBox(height: 16),
                      
                      PerformanceChart(
                        metrics: _monitor.metrics,
                        title: 'Memory Usage',
                        color: Colors.purple,
                        valueExtractor: (m) => m.memoryUsage,
                        minValue: 0,
                        maxValue: 100,
                        unit: '%',
                      ),
                    ],
                  );
                },
              ),
            ),
            
            // Stress test animations
            if (_isStressing)
              ...List.generate(20, (index) {
                return AnimatedBuilder(
                  animation: _stressControllers[index],
                  builder: (context, child) {
                    final size = MediaQuery.of(context).size;
                    final x = (index % 4) * (size.width / 4) + 50;
                    final baseY = (index ~/ 4) * 100 + 200;
                    final y = baseY + (_stressControllers[index].value * 100);
                    
                    return Positioned(
                      left: x,
                      top: y,
                      child: Container(
                        width: 30,
                        height: 30,
                        decoration: BoxDecoration(
                          color: Colors.red.withOpacity(0.7),
                          shape: BoxShape.circle,
                        ),
                      ),
                    );
                  },
                );
              }),
          ],
        ),
      ),
    );
  }

  Widget _buildControlPanel() {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Performance Controls',
              style: Theme.of(context).textTheme.titleMedium?.copyWith(
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(
                  child: ElevatedButton.icon(
                    onPressed: _toggleStressTest,
                    icon: Icon(_isStressing ? Icons.stop : Icons.play_arrow),
                    label: Text(_isStressing ? 'Stop Stress Test' : 'Start Stress Test'),
                    style: ElevatedButton.styleFrom(
                      backgroundColor: _isStressing ? Colors.red : Colors.green,
                    ),
                  ),
                ),
                const SizedBox(width: 16),
                Expanded(
                  child: OutlinedButton.icon(
                    onPressed: () {
                      setState(() {
                        _showOverlay = !_showOverlay;
                      });
                    },
                    icon: Icon(_showOverlay ? Icons.visibility_off : Icons.visibility),
                    label: Text(_showOverlay ? 'Hide Overlay' : 'Show Overlay'),
                  ),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildCurrentMetrics() {
    final metrics = _monitor.currentMetrics;
    if (metrics == null) {
      return const Card(
        child: Padding(
          padding: EdgeInsets.all(16),
          child: Text('Collecting performance data...'),
        ),
      );
    }

    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Current Performance',
              style: Theme.of(context).textTheme.titleMedium?.copyWith(
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 16),
            Row(
              children: [
                Expanded(child: _buildMetricTile('FPS', '${metrics.fps.toStringAsFixed(1)}', Colors.green)),
                Expanded(child: _buildMetricTile('Frame Time', '${metrics.frameTime.toStringAsFixed(1)}ms', Colors.blue)),
              ],
            ),
            const SizedBox(height: 8),
            Row(
              children: [
                Expanded(child: _buildMetricTile('CPU', '${metrics.cpuUsage.toStringAsFixed(1)}%', Colors.orange)),
                Expanded(child: _buildMetricTile('Memory', '${metrics.memoryUsage.toStringAsFixed(1)}%', Colors.purple)),
              ],
            ),
            const SizedBox(height: 16),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text(
                  'Avg FPS: ${_monitor.averageFPS.toStringAsFixed(1)}',
                  style: Theme.of(context).textTheme.bodySmall,
                ),
                Text(
                  'Avg Frame Time: ${_monitor.averageFrameTime.toStringAsFixed(1)}ms',
                  style: Theme.of(context).textTheme.bodySmall,
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildMetricTile(String label, String value, Color color) {
    return Container(
      margin: const EdgeInsets.all(4),
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: color.withOpacity(0.1),
        borderRadius: BorderRadius.circular(8),
        border: Border.all(color: color.withOpacity(0.3)),
      ),
      child: Column(
        children: [
          Text(
            value,
            style: Theme.of(context).textTheme.headlineSmall?.copyWith(
              color: color,
              fontWeight: FontWeight.bold,
            ),
          ),
          const SizedBox(height: 4),
          Text(
            label,
            style: Theme.of(context).textTheme.bodySmall?.copyWith(
              color: color,
            ),
          ),
        ],
      ),
    );
  }
}
```

This performance monitoring system tracks FPS, frame times, CPU and memory  
usage with real-time charts and overlay display. Features include stress  
testing, metric visualization, historical data tracking, and performance  
alerts for optimizing Flutter application performance.  

## Multi-Language Widget System

Creating widgets with comprehensive internationalization and localization support.  

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
      title: 'Multi-Language Widget System',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
      ),
      home: const MultiLanguageDemoPage(),
    );
  }
}

class AppLocalization {
  final String languageCode;
  final Map<String, String> _translations;

  AppLocalization(this.languageCode, this._translations);

  String translate(String key, {Map<String, String>? params}) {
    String translation = _translations[key] ?? key;
    
    if (params != null) {
      params.forEach((paramKey, paramValue) {
        translation = translation.replaceAll('{$paramKey}', paramValue);
      });
    }
    
    return translation;
  }

  String get currentLanguage => _translations['language_name'] ?? languageCode;

  static final Map<String, AppLocalization> _localizations = {
    'en': AppLocalization('en', {
      'language_name': 'English',
      'app_title': 'Multi-Language App',
      'welcome_message': 'Welcome to our application!',
      'hello_user': 'Hello, {username}!',
      'select_language': 'Select Language',
      'settings': 'Settings',
      'home': 'Home',
      'profile': 'Profile',
      'notifications': 'Notifications',
      'logout': 'Logout',
      'current_time': 'Current time: {time}',
      'items_count': '{count} items',
      'loading': 'Loading...',
      'error': 'An error occurred',
      'success': 'Operation successful',
      'cancel': 'Cancel',
      'confirm': 'Confirm',
      'description': 'This is a demonstration of multi-language support.',
      'change_theme': 'Change Theme',
      'dark_mode': 'Dark Mode',
      'light_mode': 'Light Mode',
    }),
    
    'es': AppLocalization('es', {
      'language_name': 'Español',
      'app_title': 'Aplicación Multiidioma',
      'welcome_message': '¡Bienvenido a nuestra aplicación!',
      'hello_user': '¡Hola, {username}!',
      'select_language': 'Seleccionar Idioma',
      'settings': 'Configuración',
      'home': 'Inicio',
      'profile': 'Perfil',
      'notifications': 'Notificaciones',
      'logout': 'Cerrar Sesión',
      'current_time': 'Hora actual: {time}',
      'items_count': '{count} elementos',
      'loading': 'Cargando...',
      'error': 'Se produjo un error',
      'success': 'Operación exitosa',
      'cancel': 'Cancelar',
      'confirm': 'Confirmar',
      'description': 'Esta es una demostración del soporte multiidioma.',
      'change_theme': 'Cambiar Tema',
      'dark_mode': 'Modo Oscuro',
      'light_mode': 'Modo Claro',
    }),
    
    'fr': AppLocalization('fr', {
      'language_name': 'Français',
      'app_title': 'Application Multilingue',
      'welcome_message': 'Bienvenue dans notre application!',
      'hello_user': 'Bonjour, {username}!',
      'select_language': 'Sélectionner la Langue',
      'settings': 'Paramètres',
      'home': 'Accueil',
      'profile': 'Profil',
      'notifications': 'Notifications',
      'logout': 'Déconnexion',
      'current_time': 'Heure actuelle: {time}',
      'items_count': '{count} éléments',
      'loading': 'Chargement...',
      'error': 'Une erreur s\'est produite',
      'success': 'Opération réussie',
      'cancel': 'Annuler',
      'confirm': 'Confirmer',
      'description': 'Ceci est une démonstration du support multilingue.',
      'change_theme': 'Changer le Thème',
      'dark_mode': 'Mode Sombre',
      'light_mode': 'Mode Clair',
    }),
    
    'de': AppLocalization('de', {
      'language_name': 'Deutsch',
      'app_title': 'Mehrsprachige Anwendung',
      'welcome_message': 'Willkommen in unserer Anwendung!',
      'hello_user': 'Hallo, {username}!',
      'select_language': 'Sprache Auswählen',
      'settings': 'Einstellungen',
      'home': 'Startseite',
      'profile': 'Profil',
      'notifications': 'Benachrichtigungen',
      'logout': 'Abmelden',
      'current_time': 'Aktuelle Zeit: {time}',
      'items_count': '{count} Elemente',
      'loading': 'Laden...',
      'error': 'Ein Fehler ist aufgetreten',
      'success': 'Operation erfolgreich',
      'cancel': 'Abbrechen',
      'confirm': 'Bestätigen',
      'description': 'Dies ist eine Demonstration der Mehrsprachunterstützung.',
      'change_theme': 'Thema Ändern',
      'dark_mode': 'Dunkler Modus',
      'light_mode': 'Heller Modus',
    }),
    
    'ja': AppLocalization('ja', {
      'language_name': '日本語',
      'app_title': '多言語アプリ',
      'welcome_message': 'アプリケーションへようこそ！',
      'hello_user': 'こんにちは、{username}さん！',
      'select_language': '言語を選択',
      'settings': '設定',
      'home': 'ホーム',
      'profile': 'プロフィール',
      'notifications': '通知',
      'logout': 'ログアウト',
      'current_time': '現在時刻: {time}',
      'items_count': '{count}項目',
      'loading': '読み込み中...',
      'error': 'エラーが発生しました',
      'success': '操作が成功しました',
      'cancel': 'キャンセル',
      'confirm': '確認',
      'description': 'これは多言語サポートのデモンストレーションです。',
      'change_theme': 'テーマ変更',
      'dark_mode': 'ダークモード',
      'light_mode': 'ライトモード',
    }),
  };

  static AppLocalization of(String languageCode) {
    return _localizations[languageCode] ?? _localizations['en']!;
  }

  static List<String> get supportedLanguages => _localizations.keys.toList();
}

class LocalizationProvider extends ChangeNotifier {
  String _currentLanguage = 'en';
  
  String get currentLanguage => _currentLanguage;
  AppLocalization get localization => AppLocalization.of(_currentLanguage);
  
  void setLanguage(String languageCode) {
    if (_currentLanguage != languageCode) {
      _currentLanguage = languageCode;
      notifyListeners();
    }
  }

  String translate(String key, {Map<String, String>? params}) {
    return localization.translate(key, params: params);
  }
}

class LocalizedWidget extends StatelessWidget {
  final String textKey;
  final TextStyle? style;
  final Map<String, String>? params;
  final TextAlign? textAlign;

  const LocalizedWidget({
    super.key,
    required this.textKey,
    this.style,
    this.params,
    this.textAlign,
  });

  @override
  Widget build(BuildContext context) {
    return Consumer<LocalizationProvider>(
      builder: (context, localization, child) {
        return Text(
          localization.translate(textKey, params: params),
          style: style,
          textAlign: textAlign,
        );
      },
    );
  }
}

class LocalizedButton extends StatelessWidget {
  final String textKey;
  final VoidCallback? onPressed;
  final ButtonStyle? style;
  final Map<String, String>? params;

  const LocalizedButton({
    super.key,
    required this.textKey,
    this.onPressed,
    this.style,
    this.params,
  });

  @override
  Widget build(BuildContext context) {
    return Consumer<LocalizationProvider>(
      builder: (context, localization, child) {
        return ElevatedButton(
          onPressed: onPressed,
          style: style,
          child: Text(localization.translate(textKey, params: params)),
        );
      },
    );
  }
}

class LanguageSelector extends StatefulWidget {
  final LocalizationProvider localizationProvider;

  const LanguageSelector({
    super.key,
    required this.localizationProvider,
  });

  @override
  State<LanguageSelector> createState() => _LanguageSelectorState();
}

class _LanguageSelectorState extends State<LanguageSelector> {
  @override
  Widget build(BuildContext context) {
    return Consumer<LocalizationProvider>(
      builder: (context, localization, child) {
        return Card(
          child: Padding(
            padding: const EdgeInsets.all(16),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                LocalizedWidget(
                  textKey: 'select_language',
                  style: Theme.of(context).textTheme.titleMedium?.copyWith(
                    fontWeight: FontWeight.bold,
                  ),
                ),
                const SizedBox(height: 16),
                Wrap(
                  spacing: 8,
                  runSpacing: 8,
                  children: AppLocalization.supportedLanguages.map((lang) {
                    final isSelected = lang == localization.currentLanguage;
                    final langName = AppLocalization.of(lang).currentLanguage;
                    
                    return FilterChip(
                      label: Text(langName),
                      selected: isSelected,
                      onSelected: (selected) {
                        if (selected) {
                          widget.localizationProvider.setLanguage(lang);
                        }
                      },
                      selectedColor: Theme.of(context).colorScheme.primary.withOpacity(0.2),
                      checkmarkColor: Theme.of(context).colorScheme.primary,
                    );
                  }).toList(),
                ),
              ],
            ),
          ),
        );
      },
    );
  }
}

class Consumer<T extends ChangeNotifier> extends StatelessWidget {
  final Widget Function(BuildContext context, T value, Widget? child) builder;
  final Widget? child;

  const Consumer({
    super.key,
    required this.builder,
    this.child,
  });

  @override
  Widget build(BuildContext context) {
    // This is a simplified consumer implementation
    // In a real app, you'd use Provider package or similar state management
    final provider = context.findAncestorWidgetOfExactType<_InheritedProvider<T>>();
    return builder(context, provider!.value, child);
  }
}

class _InheritedProvider<T extends ChangeNotifier> extends InheritedNotifier<T> {
  final T value;

  const _InheritedProvider({
    required this.value,
    required super.child,
  }) : super(notifier: value);
}

class ProviderWidget<T extends ChangeNotifier> extends StatelessWidget {
  final T value;
  final Widget child;

  const ProviderWidget({
    super.key,
    required this.value,
    required this.child,
  });

  @override
  Widget build(BuildContext context) {
    return _InheritedProvider<T>(
      value: value,
      child: child,
    );
  }
}

class LocalizedListTile extends StatelessWidget {
  final String titleKey;
  final String? subtitleKey;
  final IconData? icon;
  final VoidCallback? onTap;
  final Map<String, String>? titleParams;
  final Map<String, String>? subtitleParams;

  const LocalizedListTile({
    super.key,
    required this.titleKey,
    this.subtitleKey,
    this.icon,
    this.onTap,
    this.titleParams,
    this.subtitleParams,
  });

  @override
  Widget build(BuildContext context) {
    return Consumer<LocalizationProvider>(
      builder: (context, localization, child) {
        return ListTile(
          leading: icon != null ? Icon(icon) : null,
          title: Text(localization.translate(titleKey, params: titleParams)),
          subtitle: subtitleKey != null 
              ? Text(localization.translate(subtitleKey!, params: subtitleParams))
              : null,
          onTap: onTap,
        );
      },
    );
  }
}

class MultiLanguageDemoPage extends StatefulWidget {
  const MultiLanguageDemoPage({super.key});

  @override
  State<MultiLanguageDemoPage> createState() => _MultiLanguageDemoPageState();
}

class _MultiLanguageDemoPageState extends State<MultiLanguageDemoPage> {
  final LocalizationProvider _localizationProvider = LocalizationProvider();
  bool _isDarkMode = false;

  @override
  Widget build(BuildContext context) {
    return ProviderWidget<LocalizationProvider>(
      value: _localizationProvider,
      child: MaterialApp(
        theme: ThemeData(
          useMaterial3: true,
          colorScheme: ColorScheme.fromSeed(
            seedColor: Colors.blue,
            brightness: _isDarkMode ? Brightness.dark : Brightness.light,
          ),
        ),
        home: Consumer<LocalizationProvider>(
          builder: (context, localization, child) {
            return Scaffold(
              appBar: AppBar(
                title: LocalizedWidget(
                  textKey: 'app_title',
                  style: const TextStyle(fontWeight: FontWeight.bold),
                ),
                backgroundColor: Theme.of(context).colorScheme.inversePrimary,
                actions: [
                  IconButton(
                    icon: Icon(_isDarkMode ? Icons.light_mode : Icons.dark_mode),
                    onPressed: () {
                      setState(() {
                        _isDarkMode = !_isDarkMode;
                      });
                    },
                    tooltip: localization.translate(_isDarkMode ? 'light_mode' : 'dark_mode'),
                  ),
                ],
              ),
              body: SingleChildScrollView(
                padding: const EdgeInsets.all(16),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    // Language Selector
                    LanguageSelector(localizationProvider: _localizationProvider),
                    
                    const SizedBox(height: 24),
                    
                    // Welcome Section
                    Card(
                      color: Theme.of(context).colorScheme.primaryContainer,
                      child: Padding(
                        padding: const EdgeInsets.all(16),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            LocalizedWidget(
                              textKey: 'welcome_message',
                              style: Theme.of(context).textTheme.headlineSmall?.copyWith(
                                color: Theme.of(context).colorScheme.onPrimaryContainer,
                                fontWeight: FontWeight.bold,
                              ),
                            ),
                            const SizedBox(height: 8),
                            LocalizedWidget(
                              textKey: 'hello_user',
                              params: {'username': 'Flutter Developer'},
                              style: Theme.of(context).textTheme.titleMedium?.copyWith(
                                color: Theme.of(context).colorScheme.onPrimaryContainer,
                              ),
                            ),
                            const SizedBox(height: 12),
                            LocalizedWidget(
                              textKey: 'description',
                              style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                                color: Theme.of(context).colorScheme.onPrimaryContainer.withOpacity(0.8),
                              ),
                            ),
                          ],
                        ),
                      ),
                    ),
                    
                    const SizedBox(height: 24),
                    
                    // Menu Items
                    Card(
                      child: Column(
                        children: [
                          LocalizedListTile(
                            titleKey: 'home',
                            icon: Icons.home,
                            onTap: () {
                              ScaffoldMessenger.of(context).showSnackBar(
                                SnackBar(
                                  content: LocalizedWidget(textKey: 'home'),
                                ),
                              );
                            },
                          ),
                          const Divider(height: 1),
                          LocalizedListTile(
                            titleKey: 'profile',
                            icon: Icons.person,
                            onTap: () {
                              ScaffoldMessenger.of(context).showSnackBar(
                                SnackBar(
                                  content: LocalizedWidget(textKey: 'profile'),
                                ),
                              );
                            },
                          ),
                          const Divider(height: 1),
                          LocalizedListTile(
                            titleKey: 'notifications',
                            subtitleKey: 'items_count',
                            subtitleParams: {'count': '3'},
                            icon: Icons.notifications,
                            onTap: () {
                              ScaffoldMessenger.of(context).showSnackBar(
                                SnackBar(
                                  content: LocalizedWidget(textKey: 'notifications'),
                                ),
                              );
                            },
                          ),
                          const Divider(height: 1),
                          LocalizedListTile(
                            titleKey: 'settings',
                            icon: Icons.settings,
                            onTap: () {
                              ScaffoldMessenger.of(context).showSnackBar(
                                SnackBar(
                                  content: LocalizedWidget(textKey: 'settings'),
                                ),
                              );
                            },
                          ),
                        ],
                      ),
                    ),
                    
                    const SizedBox(height: 24),
                    
                    // Dynamic Content
                    Card(
                      child: Padding(
                        padding: const EdgeInsets.all(16),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            LocalizedWidget(
                              textKey: 'current_time',
                              params: {'time': TimeOfDay.now().format(context)},
                              style: Theme.of(context).textTheme.bodyLarge,
                            ),
                            const SizedBox(height: 16),
                            Row(
                              children: [
                                Expanded(
                                  child: LocalizedButton(
                                    textKey: 'success',
                                    onPressed: () {
                                      ScaffoldMessenger.of(context).showSnackBar(
                                        SnackBar(
                                          content: LocalizedWidget(textKey: 'success'),
                                          backgroundColor: Colors.green,
                                        ),
                                      );
                                    },
                                  ),
                                ),
                                const SizedBox(width: 8),
                                Expanded(
                                  child: LocalizedButton(
                                    textKey: 'error',
                                    onPressed: () {
                                      ScaffoldMessenger.of(context).showSnackBar(
                                        SnackBar(
                                          content: LocalizedWidget(textKey: 'error'),
                                          backgroundColor: Colors.red,
                                        ),
                                      );
                                    },
                                    style: ElevatedButton.styleFrom(
                                      backgroundColor: Colors.red,
                                      foregroundColor: Colors.white,
                                    ),
                                  ),
                                ),
                              ],
                            ),
                          ],
                        ),
                      ),
                    ),
                    
                    const SizedBox(height: 24),
                    
                    // Language Information
                    Card(
                      child: Padding(
                        padding: const EdgeInsets.all(16),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(
                              'Current Language',
                              style: Theme.of(context).textTheme.titleMedium?.copyWith(
                                fontWeight: FontWeight.bold,
                              ),
                            ),
                            const SizedBox(height: 8),
                            Text(
                              localization.localization.currentLanguage,
                              style: Theme.of(context).textTheme.bodyLarge?.copyWith(
                                color: Theme.of(context).colorScheme.primary,
                                fontWeight: FontWeight.w600,
                              ),
                            ),
                            const SizedBox(height: 8),
                            Text(
                              'Code: ${localization.currentLanguage}',
                              style: Theme.of(context).textTheme.bodySmall?.copyWith(
                                color: Theme.of(context).colorScheme.onSurface.withOpacity(0.6),
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
          },
        ),
      ),
    );
  }
}
```

This multi-language system provides complete internationalization support  
with dynamic language switching, parameterized translations, localized  
widgets, and extensible language management. Features include real-time  
language updates, custom translation parameters, and reusable components.  

## Accessibility-Enhanced Custom Widgets

Creating widgets with comprehensive accessibility support and assistive technology integration.  

```dart
import 'package:flutter/material.dart';
import 'package:flutter/semantics.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Accessibility-Enhanced Custom Widgets',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.green),
      ),
      home: const AccessibilityDemoPage(),
    );
  }
}

class AccessibleButton extends StatefulWidget {
  final String text;
  final VoidCallback? onPressed;
  final IconData? icon;
  final String? semanticLabel;
  final String? tooltip;
  final bool isEnabled;
  final bool isLoading;
  final Color? backgroundColor;
  final Color? foregroundColor;

  const AccessibleButton({
    super.key,
    required this.text,
    this.onPressed,
    this.icon,
    this.semanticLabel,
    this.tooltip,
    this.isEnabled = true,
    this.isLoading = false,
    this.backgroundColor,
    this.foregroundColor,
  });

  @override
  State<AccessibleButton> createState() => _AccessibleButtonState();
}

class _AccessibleButtonState extends State<AccessibleButton> {
  bool _isPressed = false;
  bool _isHovered = false;
  bool _isFocused = false;

  @override
  Widget build(BuildContext context) {
    final isDisabled = !widget.isEnabled || widget.onPressed == null || widget.isLoading;
    
    return Semantics(
      label: widget.semanticLabel ?? widget.text,
      hint: widget.tooltip,
      button: true,
      enabled: !isDisabled,
      child: Tooltip(
        message: widget.tooltip ?? '',
        child: Focus(
          onFocusChange: (focused) {
            setState(() {
              _isFocused = focused;
            });
          },
          child: MouseRegion(
            onEnter: (_) => setState(() => _isHovered = true),
            onExit: (_) => setState(() => _isHovered = false),
            child: GestureDetector(
              onTapDown: isDisabled ? null : (_) => setState(() => _isPressed = true),
              onTapUp: isDisabled ? null : (_) => setState(() => _isPressed = false),
              onTapCancel: () => setState(() => _isPressed = false),
              onTap: isDisabled ? null : widget.onPressed,
              child: AnimatedContainer(
                duration: const Duration(milliseconds: 150),
                padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
                decoration: BoxDecoration(
                  color: _getBackgroundColor(context, isDisabled),
                  borderRadius: BorderRadius.circular(8),
                  border: _isFocused
                      ? Border.all(
                          color: Theme.of(context).colorScheme.primary,
                          width: 2,
                        )
                      : null,
                  boxShadow: _isPressed || isDisabled
                      ? null
                      : [
                          BoxShadow(
                            color: Colors.black.withOpacity(0.2),
                            blurRadius: _isHovered ? 8 : 4,
                            offset: Offset(0, _isHovered ? 4 : 2),
                          ),
                        ],
                ),
                transform: Matrix4.identity()
                  ..scale(_isPressed ? 0.95 : 1.0),
                child: Row(
                  mainAxisSize: MainAxisSize.min,
                  children: [
                    if (widget.isLoading) ...[
                      SizedBox(
                        width: 16,
                        height: 16,
                        child: CircularProgressIndicator(
                          strokeWidth: 2,
                          valueColor: AlwaysStoppedAnimation<Color>(
                            _getForegroundColor(context, isDisabled),
                          ),
                        ),
                      ),
                      const SizedBox(width: 8),
                    ] else if (widget.icon != null) ...[
                      Icon(
                        widget.icon,
                        size: 18,
                        color: _getForegroundColor(context, isDisabled),
                      ),
                      const SizedBox(width: 8),
                    ],
                    Text(
                      widget.text,
                      style: Theme.of(context).textTheme.labelLarge?.copyWith(
                        color: _getForegroundColor(context, isDisabled),
                        fontWeight: FontWeight.w600,
                      ),
                    ),
                  ],
                ),
              ),
            ),
          ),
        ),
      ),
    );
  }

  Color _getBackgroundColor(BuildContext context, bool isDisabled) {
    if (isDisabled) {
      return Theme.of(context).colorScheme.onSurface.withOpacity(0.12);
    }
    
    Color baseColor = widget.backgroundColor ?? Theme.of(context).colorScheme.primary;
    
    if (_isPressed) {
      return baseColor.withOpacity(0.8);
    } else if (_isHovered) {
      return baseColor.withOpacity(0.9);
    }
    
    return baseColor;
  }

  Color _getForegroundColor(BuildContext context, bool isDisabled) {
    if (isDisabled) {
      return Theme.of(context).colorScheme.onSurface.withOpacity(0.38);
    }
    
    return widget.foregroundColor ?? Theme.of(context).colorScheme.onPrimary;
  }
}

class AccessibleCard extends StatelessWidget {
  final Widget child;
  final String? semanticLabel;
  final VoidCallback? onTap;
  final bool isInteractive;
  final Color? backgroundColor;
  final EdgeInsets? padding;

  const AccessibleCard({
    super.key,
    required this.child,
    this.semanticLabel,
    this.onTap,
    this.isInteractive = false,
    this.backgroundColor,
    this.padding,
  });

  @override
  Widget build(BuildContext context) {
    Widget card = Container(
      padding: padding ?? const EdgeInsets.all(16),
      decoration: BoxDecoration(
        color: backgroundColor ?? Theme.of(context).colorScheme.surface,
        borderRadius: BorderRadius.circular(12),
        border: Border.all(
          color: Theme.of(context).colorScheme.outline.withOpacity(0.2),
        ),
        boxShadow: [
          BoxShadow(
            color: Colors.black.withOpacity(0.1),
            blurRadius: 4,
            offset: const Offset(0, 2),
          ),
        ],
      ),
      child: child,
    );

    if (isInteractive && onTap != null) {
      return Semantics(
        label: semanticLabel,
        button: true,
        child: InkWell(
          onTap: onTap,
          borderRadius: BorderRadius.circular(12),
          child: card,
        ),
      );
    } else {
      return Semantics(
        label: semanticLabel,
        container: true,
        child: card,
      );
    }
  }
}

class AccessibleSlider extends StatefulWidget {
  final double value;
  final double min;
  final double max;
  final ValueChanged<double>? onChanged;
  final String label;
  final String? semanticFormatterCallback;
  final int? divisions;

  const AccessibleSlider({
    super.key,
    required this.value,
    required this.min,
    required this.max,
    required this.onChanged,
    required this.label,
    this.semanticFormatterCallback,
    this.divisions,
  });

  @override
  State<AccessibleSlider> createState() => _AccessibleSliderState();
}

class _AccessibleSliderState extends State<AccessibleSlider> {
  late double _currentValue;

  @override
  void initState() {
    super.initState();
    _currentValue = widget.value;
  }

  @override
  void didUpdateWidget(AccessibleSlider oldWidget) {
    super.didUpdateWidget(oldWidget);
    if (widget.value != oldWidget.value) {
      _currentValue = widget.value;
    }
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Semantics(
          label: widget.label,
          child: Text(
            widget.label,
            style: Theme.of(context).textTheme.titleMedium?.copyWith(
              fontWeight: FontWeight.w600,
            ),
          ),
        ),
        const SizedBox(height: 8),
        Semantics(
          slider: true,
          value: _currentValue.round(),
          increasedValue: (_currentValue + 1).round(),
          decreasedValue: (_currentValue - 1).round(),
          label: '${widget.label}: ${_currentValue.round()}',
          child: SliderTheme(
            data: SliderTheme.of(context).copyWith(
              thumbShape: const RoundSliderThumbShape(enabledThumbRadius: 12),
              trackHeight: 6,
              overlayShape: const RoundSliderOverlayShape(overlayRadius: 20),
            ),
            child: Slider(
              value: _currentValue,
              min: widget.min,
              max: widget.max,
              divisions: widget.divisions,
              onChanged: widget.onChanged == null
                  ? null
                  : (value) {
                      setState(() {
                        _currentValue = value;
                      });
                      widget.onChanged!(value);
                      
                      // Provide haptic feedback
                      if (widget.divisions != null) {
                        SemanticsService.announce(
                          '${widget.label}: ${value.round()}',
                          TextDirection.ltr,
                        );
                      }
                    },
            ),
          ),
        ),
        Row(
          mainAxisAlignment: MainAxisAlignment.spaceBetween,
          children: [
            Text(
              '${widget.min.round()}',
              style: Theme.of(context).textTheme.bodySmall,
            ),
            Text(
              'Current: ${_currentValue.round()}',
              style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                fontWeight: FontWeight.w600,
                color: Theme.of(context).colorScheme.primary,
              ),
            ),
            Text(
              '${widget.max.round()}',
              style: Theme.of(context).textTheme.bodySmall,
            ),
          ],
        ),
      ],
    );
  }
}

class AccessibleFormField extends StatefulWidget {
  final String label;
  final String? hint;
  final TextEditingController? controller;
  final String? Function(String?)? validator;
  final TextInputType? keyboardType;
  final bool obscureText;
  final bool required;
  final IconData? prefixIcon;
  final String? errorMessage;

  const AccessibleFormField({
    super.key,
    required this.label,
    this.hint,
    this.controller,
    this.validator,
    this.keyboardType,
    this.obscureText = false,
    this.required = false,
    this.prefixIcon,
    this.errorMessage,
  });

  @override
  State<AccessibleFormField> createState() => _AccessibleFormFieldState();
}

class _AccessibleFormFieldState extends State<AccessibleFormField> {
  final FocusNode _focusNode = FocusNode();
  bool _isFocused = false;

  @override
  void initState() {
    super.initState();
    _focusNode.addListener(() {
      setState(() {
        _isFocused = _focusNode.hasFocus;
      });
    });
  }

  @override
  void dispose() {
    _focusNode.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final hasError = widget.errorMessage != null;
    
    return Semantics(
      label: '${widget.label}${widget.required ? ' required' : ''}',
      hint: widget.hint,
      textField: true,
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(
            '${widget.label}${widget.required ? ' *' : ''}',
            style: Theme.of(context).textTheme.titleMedium?.copyWith(
              fontWeight: FontWeight.w600,
              color: hasError
                  ? Theme.of(context).colorScheme.error
                  : Theme.of(context).colorScheme.onSurface,
            ),
          ),
          const SizedBox(height: 8),
          TextFormField(
            controller: widget.controller,
            focusNode: _focusNode,
            decoration: InputDecoration(
              hintText: widget.hint,
              prefixIcon: widget.prefixIcon != null ? Icon(widget.prefixIcon) : null,
              border: OutlineInputBorder(
                borderRadius: BorderRadius.circular(8),
                borderSide: BorderSide(
                  color: hasError
                      ? Theme.of(context).colorScheme.error
                      : Theme.of(context).colorScheme.outline,
                ),
              ),
              focusedBorder: OutlineInputBorder(
                borderRadius: BorderRadius.circular(8),
                borderSide: BorderSide(
                  color: hasError
                      ? Theme.of(context).colorScheme.error
                      : Theme.of(context).colorScheme.primary,
                  width: 2,
                ),
              ),
              errorBorder: OutlineInputBorder(
                borderRadius: BorderRadius.circular(8),
                borderSide: BorderSide(
                  color: Theme.of(context).colorScheme.error,
                ),
              ),
              filled: true,
              fillColor: _isFocused
                  ? Theme.of(context).colorScheme.primaryContainer.withOpacity(0.1)
                  : Theme.of(context).colorScheme.surface,
            ),
            keyboardType: widget.keyboardType,
            obscureText: widget.obscureText,
            validator: widget.validator,
          ),
          if (hasError) ...[
            const SizedBox(height: 4),
            Semantics(
              liveRegion: true,
              child: Text(
                widget.errorMessage!,
                style: Theme.of(context).textTheme.bodySmall?.copyWith(
                  color: Theme.of(context).colorScheme.error,
                ),
              ),
            ),
          ],
        ],
      ),
    );
  }
}

class AccessibilityDemoPage extends StatefulWidget {
  const AccessibilityDemoPage({super.key});

  @override
  State<AccessibilityDemoPage> createState() => _AccessibilityDemoPageState();
}

class _AccessibilityDemoPageState extends State<AccessibilityDemoPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  
  double _volumeValue = 50;
  double _brightnessValue = 75;
  bool _isLoading = false;
  String? _nameError;
  String? _emailError;

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    super.dispose();
  }

  void _validateForm() {
    setState(() {
      _nameError = null;
      _emailError = null;
    });

    if (_nameController.text.isEmpty) {
      setState(() {
        _nameError = 'Name is required';
      });
    }

    if (_emailController.text.isEmpty) {
      setState(() {
        _emailError = 'Email is required';
      });
    } else if (!_emailController.text.contains('@')) {
      setState(() {
        _emailError = 'Please enter a valid email address';
      });
    }

    if (_nameError == null && _emailError == null) {
      _submitForm();
    } else {
      // Announce error to screen readers
      SemanticsService.announce(
        'Form has errors. Please check the fields and try again.',
        TextDirection.ltr,
      );
    }
  }

  void _submitForm() async {
    setState(() {
      _isLoading = true;
    });

    // Simulate form submission
    await Future.delayed(const Duration(seconds: 2));

    setState(() {
      _isLoading = false;
    });

    if (mounted) {
      SemanticsService.announce(
        'Form submitted successfully',
        TextDirection.ltr,
      );
      
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Form submitted successfully!'),
          backgroundColor: Colors.green,
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Accessibility-Enhanced Widgets'),
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              // Header with semantic information
              Semantics(
                header: true,
                child: Text(
                  'Accessible Form Example',
                  style: Theme.of(context).textTheme.headlineSmall?.copyWith(
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
              
              const SizedBox(height: 8),
              
              Semantics(
                child: Text(
                  'This form demonstrates accessibility features including screen reader support, keyboard navigation, and proper semantic labels.',
                  style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                    color: Theme.of(context).colorScheme.onSurface.withOpacity(0.7),
                  ),
                ),
              ),

              const SizedBox(height: 24),

              // Form Fields
              AccessibleCard(
                semanticLabel: 'Personal Information Form',
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Semantics(
                      header: true,
                      child: Text(
                        'Personal Information',
                        style: Theme.of(context).textTheme.titleLarge?.copyWith(
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                    
                    const SizedBox(height: 16),

                    AccessibleFormField(
                      label: 'Full Name',
                      hint: 'Enter your full name',
                      controller: _nameController,
                      prefixIcon: Icons.person,
                      required: true,
                      errorMessage: _nameError,
                    ),

                    const SizedBox(height: 16),

                    AccessibleFormField(
                      label: 'Email Address',
                      hint: 'Enter your email address',
                      controller: _emailController,
                      prefixIcon: Icons.email,
                      keyboardType: TextInputType.emailAddress,
                      required: true,
                      errorMessage: _emailError,
                    ),
                  ],
                ),
              ),

              const SizedBox(height: 24),

              // Settings Controls
              AccessibleCard(
                semanticLabel: 'Settings Controls',
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Semantics(
                      header: true,
                      child: Text(
                        'Settings',
                        style: Theme.of(context).textTheme.titleLarge?.copyWith(
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                    
                    const SizedBox(height: 16),

                    AccessibleSlider(
                      label: 'Volume',
                      value: _volumeValue,
                      min: 0,
                      max: 100,
                      divisions: 10,
                      onChanged: (value) {
                        setState(() {
                          _volumeValue = value;
                        });
                      },
                    ),

                    const SizedBox(height: 24),

                    AccessibleSlider(
                      label: 'Brightness',
                      value: _brightnessValue,
                      min: 0,
                      max: 100,
                      divisions: 10,
                      onChanged: (value) {
                        setState(() {
                          _brightnessValue = value;
                        });
                      },
                    ),
                  ],
                ),
              ),

              const SizedBox(height: 24),

              // Action Buttons
              Row(
                children: [
                  Expanded(
                    child: AccessibleButton(
                      text: 'Cancel',
                      icon: Icons.cancel,
                      semanticLabel: 'Cancel form submission',
                      tooltip: 'Clear the form and cancel',
                      onPressed: () {
                        _nameController.clear();
                        _emailController.clear();
                        setState(() {
                          _nameError = null;
                          _emailError = null;
                          _volumeValue = 50;
                          _brightnessValue = 75;
                        });
                        
                        SemanticsService.announce(
                          'Form cleared',
                          TextDirection.ltr,
                        );
                      },
                      backgroundColor: Theme.of(context).colorScheme.secondary,
                    ),
                  ),
                  
                  const SizedBox(width: 16),
                  
                  Expanded(
                    child: AccessibleButton(
                      text: 'Submit',
                      icon: Icons.send,
                      semanticLabel: 'Submit the form',
                      tooltip: 'Submit the form with entered information',
                      isLoading: _isLoading,
                      onPressed: _isLoading ? null : _validateForm,
                    ),
                  ),
                ],
              ),

              const SizedBox(height: 24),

              // Accessibility Information
              AccessibleCard(
                semanticLabel: 'Accessibility Information',
                backgroundColor: Theme.of(context).colorScheme.primaryContainer,
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        Icon(
                          Icons.accessibility,
                          color: Theme.of(context).colorScheme.onPrimaryContainer,
                        ),
                        const SizedBox(width: 8),
                        Text(
                          'Accessibility Features',
                          style: Theme.of(context).textTheme.titleMedium?.copyWith(
                            color: Theme.of(context).colorScheme.onPrimaryContainer,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ],
                    ),
                    
                    const SizedBox(height: 12),
                    
                    Text(
                      '• Screen reader compatible\n'
                      '• Keyboard navigation support\n'
                      '• Focus indicators\n'
                      '• Semantic labels and hints\n'
                      '• High contrast support\n'
                      '• Live region announcements',
                      style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                        color: Theme.of(context).colorScheme.onPrimaryContainer,
                      ),
                    ),
                  ],
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

This accessibility system provides comprehensive support for screen readers,  
keyboard navigation, semantic labels, focus management, and assistive  
technologies. Features include proper ARIA attributes, live regions,  
high contrast support, and full keyboard accessibility compliance.  