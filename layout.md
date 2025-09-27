# Flutter Layout

Flutter uses a flexible layout system based on widgets to create user  
interfaces. Every element in Flutter is a widget, and widgets are composed  
to build complex layouts. The layout system uses constraints, size, and  
position to determine how widgets are rendered on screen.  

Layout widgets don't have a visual representation themselves; instead,  
they control how other widgets are displayed. Understanding Flutter's  
layout system is essential for creating responsive and beautiful user  
interfaces that work across different screen sizes and orientations.  

## Container Layout

Container is the most versatile layout widget, combining common painting,  
positioning, and sizing widgets. It's similar to a div in web development  
but with more built-in styling capabilities.  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Container Layout')),
        body: Center(
          child: Container(
            width: 200,
            height: 100,
            padding: EdgeInsets.all(16),
            margin: EdgeInsets.all(20),
            decoration: BoxDecoration(
              color: Colors.blue[100],
              border: Border.all(color: Colors.blue, width: 2),
              borderRadius: BorderRadius.circular(8),
            ),
            child: Text(
              'Hello there!',
              style: TextStyle(fontSize: 18),
              textAlign: TextAlign.center,
            ),
          ),
        ),
      ),
    );
  }
}
```

The Container widget provides comprehensive styling options including  
padding, margin, decoration, and transformation. It automatically sizes  
itself to fit its child while respecting constraints from parent widgets.  
The decoration property enables complex styling like gradients, shadows,  
and borders.  

## Column Layout

Column arranges its children vertically in a linear fashion. It's one of  
the most commonly used layout widgets for creating vertical lists of  
widgets.  

```dart
import 'package:flutter/material.dart';

class ColumnLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Column Layout')),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        crossAxisAlignment: CrossAxisAlignment.center,
        children: [
          Container(
            padding: EdgeInsets.all(16),
            color: Colors.red[200],
            child: Text('First Item'),
          ),
          Container(
            padding: EdgeInsets.all(16),
            color: Colors.green[200],
            child: Text('Second Item'),
          ),
          Container(
            padding: EdgeInsets.all(16),
            color: Colors.blue[200],
            child: Text('Third Item'),
          ),
        ],
      ),
    );
  }
}
```

Column uses MainAxisAlignment to control spacing along the vertical axis  
and CrossAxisAlignment for horizontal alignment. The mainAxisAlignment  
property offers various spacing options like spaceEvenly, spaceBetween,  
and spaceAround for precise control over vertical spacing.  

## Row Layout

Row arranges its children horizontally, providing the counterpart to  
Column for creating horizontal layouts. It's essential for creating  
navigation bars, button groups, and horizontal content arrangements.  

```dart
import 'package:flutter/material.dart';

class RowLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Row Layout')),
      body: Center(
        child: Row(
          mainAxisAlignment: MainAxisAlignment.spaceBetween,
          crossAxisAlignment: CrossAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {},
              child: Text('Left'),
            ),
            Icon(Icons.star, size: 40, color: Colors.amber),
            ElevatedButton(
              onPressed: () {},
              child: Text('Right'),
            ),
          ],
        ),
      ),
    );
  }
}
```

Row distributes space horizontally among its children. The MainAxisAlignment  
controls horizontal spacing, while CrossAxisAlignment handles vertical  
alignment. Rows are particularly useful for creating toolbars, navigation  
elements, and horizontal button groups.  

## Stack Layout

Stack allows overlaying widgets on top of each other, similar to absolute  
positioning in web development. Children are positioned relative to the  
stack's edges using the Positioned widget.  

```dart
import 'package:flutter/material.dart';

class StackLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Stack Layout')),
      body: Center(
        child: Container(
          width: 300,
          height: 200,
          child: Stack(
            children: [
              Container(
                width: 300,
                height: 200,
                color: Colors.grey[300],
              ),
              Positioned(
                top: 20,
                left: 20,
                child: Container(
                  width: 100,
                  height: 60,
                  color: Colors.red,
                  child: Center(child: Text('Top Left')),
                ),
              ),
              Positioned(
                bottom: 20,
                right: 20,
                child: Container(
                  width: 100,
                  height: 60,
                  color: Colors.blue,
                  child: Center(child: Text('Bottom Right')),
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

Stack creates layered layouts where widgets can overlap. Positioned widgets  
specify exact locations using top, bottom, left, and right properties.  
Stack is invaluable for creating complex overlays, floating action buttons,  
and badges on top of other content.  

## Center Layout

Center is a simple widget that centers its child both horizontally and  
vertically within the available space. It's one of the most commonly  
used alignment widgets.  

```dart
import 'package:flutter/material.dart';

class CenterLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Center Layout')),
      body: Center(
        child: Container(
          padding: EdgeInsets.all(24),
          decoration: BoxDecoration(
            color: Colors.amber[200],
            shape: BoxShape.circle,
          ),
          child: Text(
            'Centered\nContent',
            textAlign: TextAlign.center,
            style: TextStyle(
              fontSize: 18,
              fontWeight: FontWeight.bold,
            ),
          ),
        ),
      ),
    );
  }
}
```

Center automatically calculates the available space and positions its  
child in the geometric center. It's equivalent to using Align with  
Alignment.center but provides a more semantic and readable approach  
for centering content.  

## Align Layout

Align provides precise control over child positioning within its bounds  
using alignment values ranging from -1.0 to 1.0 on both axes. It offers  
more flexibility than Center for positioning widgets.  

```dart
import 'package:flutter/material.dart';

class AlignLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Align Layout')),
      body: Container(
        width: double.infinity,
        height: double.infinity,
        child: Stack(
          children: [
            Align(
              alignment: Alignment.topLeft,
              child: Container(
                padding: EdgeInsets.all(8),
                color: Colors.red[200],
                child: Text('Top Left'),
              ),
            ),
            Align(
              alignment: Alignment.topRight,
              child: Container(
                padding: EdgeInsets.all(8),
                color: Colors.green[200],
                child: Text('Top Right'),
              ),
            ),
            Align(
              alignment: Alignment.center,
              child: Container(
                padding: EdgeInsets.all(8),
                color: Colors.blue[200],
                child: Text('Center'),
              ),
            ),
            Align(
              alignment: Alignment.bottomCenter,
              child: Container(
                padding: EdgeInsets.all(8),
                color: Colors.purple[200],
                child: Text('Bottom Center'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

Align uses predefined alignment constants like Alignment.topLeft or custom  
alignments like Alignment(0.5, -0.8). It's perfect for positioning  
floating elements, badges, and creating precise layouts without complex  
positioning calculations.  

## Expanded Layout

Expanded makes a child of Row, Column, or Flex fill the available space  
along the main axis. It's essential for creating responsive layouts that  
adapt to different screen sizes.  

```dart
import 'package:flutter/material.dart';

class ExpandedLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Expanded Layout')),
      body: Column(
        children: [
          Container(
            height: 100,
            color: Colors.red[200],
            child: Center(child: Text('Fixed Height')),
          ),
          Expanded(
            flex: 2,
            child: Container(
              color: Colors.green[200],
              child: Center(child: Text('Expanded (flex: 2)')),
            ),
          ),
          Expanded(
            flex: 1,
            child: Container(
              color: Colors.blue[200],
              child: Center(child: Text('Expanded (flex: 1)')),
            ),
          ),
          Container(
            height: 80,
            color: Colors.purple[200],
            child: Center(child: Text('Fixed Height')),
          ),
        ],
      ),
    );
  }
}
```

Expanded automatically calculates available space after fixed-size widgets  
are laid out. The flex property determines proportional space allocation  
when multiple Expanded widgets exist. Higher flex values receive more space  
proportionally.  

## Flexible Layout

Flexible allows a child to occupy available space but doesn't force it  
to fill the entire space like Expanded does. It provides more nuanced  
control over space distribution.  

```dart
import 'package:flutter/material.dart';

class FlexibleLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Flexible Layout')),
      body: Column(
        children: [
          Row(
            children: [
              Flexible(
                flex: 1,
                child: Container(
                  height: 60,
                  color: Colors.red[200],
                  child: Center(child: Text('Flexible 1')),
                ),
              ),
              Flexible(
                flex: 2,
                child: Container(
                  height: 60,
                  color: Colors.green[200],
                  child: Center(child: Text('Flexible 2')),
                ),
              ),
            ],
          ),
          SizedBox(height: 20),
          Row(
            children: [
              Expanded(
                flex: 1,
                child: Container(
                  height: 60,
                  color: Colors.blue[200],
                  child: Center(child: Text('Expanded 1')),
                ),
              ),
              Expanded(
                flex: 2,
                child: Container(
                  height: 60,
                  color: Colors.purple[200],
                  child: Center(child: Text('Expanded 2')),
                ),
              ),
            ],
          ),
        ],
      ),
    );
  }
}
```

Flexible widgets can shrink to their minimum size when space is limited,  
while Expanded widgets always fill their allocated space. This makes  
Flexible ideal for content that should be responsive but not necessarily  
fill all available space.  

## ListView Layout

ListView creates a scrollable list of widgets, essential for displaying  
dynamic content that exceeds screen boundaries. It efficiently manages  
memory by building only visible items.  

```dart
import 'package:flutter/material.dart';

class ListViewLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('ListView Layout')),
      body: ListView.builder(
        itemCount: 50,
        itemBuilder: (context, index) {
          return ListTile(
            leading: CircleAvatar(
              backgroundColor: Colors.blue[100],
              child: Text('${index + 1}'),
            ),
            title: Text('Item ${index + 1}'),
            subtitle: Text('Description for item ${index + 1}'),
            trailing: Icon(Icons.arrow_forward_ios),
            onTap: () {
              ScaffoldMessenger.of(context).showSnackBar(
                SnackBar(content: Text('Tapped item ${index + 1}')),
              );
            },
          );
        },
      ),
    );
  }
}
```

ListView.builder creates items on-demand as they become visible, making  
it memory-efficient for large lists. The itemBuilder function receives  
the build context and index, returning the widget for each position.  
This approach scales well for thousands of items.  

## GridView Layout

GridView arranges items in a 2D grid, perfect for image galleries,  
dashboards, and organized content displays. It provides various  
constructors for different grid configurations.  

```dart
import 'package:flutter/material.dart';

class GridViewLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('GridView Layout')),
      body: GridView.builder(
        padding: EdgeInsets.all(16),
        gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 2,
          crossAxisSpacing: 16,
          mainAxisSpacing: 16,
          childAspectRatio: 1.0,
        ),
        itemCount: 20,
        itemBuilder: (context, index) {
          return Container(
            decoration: BoxDecoration(
              color: Colors.primaries[index % Colors.primaries.length][200],
              borderRadius: BorderRadius.circular(8),
            ),
            child: Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Icon(
                    Icons.grid_on,
                    size: 40,
                    color: Colors.white,
                  ),
                  SizedBox(height: 8),
                  Text(
                    'Item ${index + 1}',
                    style: TextStyle(
                      color: Colors.white,
                      fontWeight: FontWeight.bold,
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
```

GridView uses SliverGridDelegate to control grid layout parameters like  
column count, spacing, and aspect ratio. The childAspectRatio property  
controls the width-to-height ratio of each grid item, ensuring consistent  
appearance across different screen sizes.  

## Wrap Layout

Wrap automatically wraps children to the next line when they exceed  
the available width, similar to flexbox wrap in CSS. It's perfect  
for tag lists, chip collections, and responsive button groups.  

```dart
import 'package:flutter/material.dart';

class WrapLayoutExample extends StatelessWidget {
  final List<String> tags = [
    'Flutter', 'Dart', 'Mobile', 'Development', 'Cross-platform',
    'UI', 'Widgets', 'Layout', 'Design', 'Programming'
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Wrap Layout')),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Tags:',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            SizedBox(height: 16),
            Wrap(
              spacing: 8.0,
              runSpacing: 8.0,
              children: tags.map((tag) => Chip(
                label: Text(tag),
                backgroundColor: Colors.blue[100],
                deleteIcon: Icon(Icons.close, size: 16),
                onDeleted: () {},
              )).toList(),
            ),
          ],
        ),
      ),
    );
  }
}
```

Wrap automatically flows children to new lines when space runs out.  
The spacing property controls horizontal gaps between items, while  
runSpacing controls vertical gaps between lines. This creates responsive  
layouts that adapt to different screen widths automatically.  

## SizedBox Layout

SizedBox creates fixed-size boxes or spacing between widgets. It's a  
simple but essential widget for adding consistent spacing and creating  
fixed-dimension containers.  

```dart
import 'package:flutter/material.dart';

class SizedBoxLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('SizedBox Layout')),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Container(
              color: Colors.red[200],
              child: Text('First widget'),
            ),
            SizedBox(height: 20), // Vertical spacing
            Container(
              color: Colors.green[200],
              child: Text('Second widget'),
            ),
            SizedBox(height: 40), // Larger vertical spacing
            Row(
              children: [
                Container(
                  color: Colors.blue[200],
                  child: Text('Left'),
                ),
                SizedBox(width: 30), // Horizontal spacing
                Container(
                  color: Colors.purple[200],
                  child: Text('Right'),
                ),
              ],
            ),
            SizedBox(height: 20),
            SizedBox(
              width: 200,
              height: 100,
              child: Container(
                color: Colors.amber[200],
                child: Center(child: Text('Fixed Size Box')),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

SizedBox is more efficient than Container for simple spacing because  
it doesn't create additional render objects. When used with only width  
or height specified, it creates invisible spacing. With both dimensions,  
it constrains its child to the specified size.  

## Padding Layout

Padding adds empty space around its child widget. While Container can  
also add padding, the Padding widget is more efficient when only  
spacing is needed without additional styling.  

```dart
import 'package:flutter/material.dart';

class PaddingLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Padding Layout')),
      body: Column(
        children: [
          // Uniform padding
          Padding(
            padding: EdgeInsets.all(24),
            child: Container(
              color: Colors.red[200],
              child: Text('Uniform padding (24px all sides)'),
            ),
          ),
          // Symmetric padding
          Padding(
            padding: EdgeInsets.symmetric(
              horizontal: 32,
              vertical: 16,
            ),
            child: Container(
              color: Colors.green[200],
              child: Text('Symmetric padding'),
            ),
          ),
          // Custom padding for each side
          Padding(
            padding: EdgeInsets.fromLTRB(8, 16, 24, 32),
            child: Container(
              color: Colors.blue[200],
              child: Text('Custom padding: L(8) T(16) R(24) B(32)'),
            ),
          ),
          // Only specific sides
          Padding(
            padding: EdgeInsets.only(
              left: 40,
              top: 20,
            ),
            child: Container(
              color: Colors.purple[200],
              child: Text('Left and top padding only'),
            ),
          ),
        ],
      ),
    );
  }
}
```

Padding offers various constructors for different spacing needs: all()  
for uniform spacing, symmetric() for horizontal/vertical pairs, fromLTRB()  
for individual sides, and only() for specific sides. This flexibility  
makes it perfect for fine-tuned spacing control.  

## SingleChildScrollView Layout

SingleChildScrollView makes its child scrollable when content exceeds  
the available space. It's ideal for forms, content that might overflow,  
and ensuring accessibility on smaller screens.  

```dart
import 'package:flutter/material.dart';

class SingleChildScrollViewExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('SingleChildScrollView')),
      body: SingleChildScrollView(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Scrollable Content',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
            SizedBox(height: 20),
            ...List.generate(20, (index) => Container(
              margin: EdgeInsets.only(bottom: 16),
              padding: EdgeInsets.all(16),
              decoration: BoxDecoration(
                color: Colors.blue[50 * ((index % 9) + 1)],
                borderRadius: BorderRadius.circular(8),
                border: Border.all(color: Colors.blue[200]!),
              ),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Card ${index + 1}',
                    style: TextStyle(
                      fontWeight: FontWeight.bold,
                      fontSize: 18,
                    ),
                  ),
                  SizedBox(height: 8),
                  Text(
                    'This is some content for card ${index + 1}. '
                    'It demonstrates how SingleChildScrollView handles '
                    'content that exceeds the available screen space.',
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

SingleChildScrollView wraps a single widget and makes it scrollable.  
Unlike ListView, it builds all content at once, making it suitable  
for smaller amounts of content that need scrolling. The padding property  
adds space around the scrollable content.  

## Flow Layout

Flow provides powerful custom layout capabilities, allowing precise  
control over child positioning using transformation matrices. It's  
useful for complex custom layouts and animations.  

```dart
import 'package:flutter/material.dart';

class FlowLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Flow Layout')),
      body: Center(
        child: Flow(
          delegate: CircularFlowDelegate(),
          children: List.generate(8, (index) => Container(
            width: 50,
            height: 50,
            decoration: BoxDecoration(
              color: Colors.primaries[index % Colors.primaries.length],
              shape: BoxShape.circle,
            ),
            child: Center(
              child: Text(
                '${index + 1}',
                style: TextStyle(
                  color: Colors.white,
                  fontWeight: FontWeight.bold,
                ),
              ),
            ),
          )),
        ),
      ),
    );
  }
}

class CircularFlowDelegate extends FlowDelegate {
  @override
  void paintChildren(FlowPaintingContext context) {
    final size = context.size;
    final radius = size.shortestSide / 3;
    final center = Offset(size.width / 2, size.height / 2);
    
    for (int i = 0; i < context.childCount; i++) {
      final angle = (i * 2 * 3.14159) / context.childCount;
      final x = center.dx + radius * cos(angle) - 25;
      final y = center.dy + radius * sin(angle) - 25;
      
      context.paintChild(i, transform: Matrix4.translationValues(x, y, 0));
    }
  }
  
  @override
  bool shouldRepaint(FlowDelegate oldDelegate) => false;
}
```

Flow uses a FlowDelegate to determine child positions. The paintChildren  
method receives a context with painting methods and child information.  
This example creates a circular arrangement by calculating positions  
using trigonometry and applying transformations.  

## Table Layout

Table creates a grid layout where all cells in a row have the same  
height, and all cells in a column can have different widths based  
on content or specified ratios.  

```dart
import 'package:flutter/material.dart';

class TableLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Table Layout')),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Table(
          border: TableBorder.all(color: Colors.grey),
          columnWidths: {
            0: FlexColumnWidth(2),
            1: FlexColumnWidth(3),
            2: FlexColumnWidth(1),
          },
          children: [
            TableRow(
              decoration: BoxDecoration(color: Colors.grey[200]),
              children: [
                TableCell(
                  child: Padding(
                    padding: EdgeInsets.all(8),
                    child: Text(
                      'Name',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                  ),
                ),
                TableCell(
                  child: Padding(
                    padding: EdgeInsets.all(8),
                    child: Text(
                      'Description',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                  ),
                ),
                TableCell(
                  child: Padding(
                    padding: EdgeInsets.all(8),
                    child: Text(
                      'Price',
                      style: TextStyle(fontWeight: FontWeight.bold),
                    ),
                  ),
                ),
              ],
            ),
            ...List.generate(5, (index) => TableRow(
              children: [
                TableCell(
                  child: Padding(
                    padding: EdgeInsets.all(8),
                    child: Text('Product ${index + 1}'),
                  ),
                ),
                TableCell(
                  child: Padding(
                    padding: EdgeInsets.all(8),
                    child: Text('Description for product ${index + 1}'),
                  ),
                ),
                TableCell(
                  child: Padding(
                    padding: EdgeInsets.all(8),
                    child: Text('\$${(index + 1) * 10}'),
                  ),
                ),
              ],
            )),
          ],
        ),
      ),
    );
  }
}
```

Table automatically handles row heights and allows flexible column  
widths through columnWidths property. FlexColumnWidth creates  
proportional column sizing, while FixedColumnWidth and  
IntrinsicColumnWidth provide other sizing strategies.  

## Transform Layout

Transform applies matrix transformations to its child widget,  
enabling rotation, scaling, translation, and skewing effects.  
It's powerful for animations and custom visual effects.  

```dart
import 'package:flutter/material.dart';
import 'dart:math' as math;

class TransformLayoutExample extends StatefulWidget {
  @override
  _TransformLayoutExampleState createState() => _TransformLayoutExampleState();
}

class _TransformLayoutExampleState extends State<TransformLayoutExample>
    with TickerProviderStateMixin {
  late AnimationController _controller;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: Duration(seconds: 3),
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
    return Scaffold(
      appBar: AppBar(title: Text('Transform Layout')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            // Rotation
            AnimatedBuilder(
              animation: _controller,
              builder: (context, child) {
                return Transform.rotate(
                  angle: _controller.value * 2 * math.pi,
                  child: Container(
                    width: 100,
                    height: 100,
                    color: Colors.red,
                    child: Center(child: Text('Rotate')),
                  ),
                );
              },
            ),
            // Scale
            Transform.scale(
              scale: 1.5,
              child: Container(
                width: 100,
                height: 100,
                color: Colors.green,
                child: Center(child: Text('Scale')),
              ),
            ),
            // Translation
            Transform.translate(
              offset: Offset(50, 0),
              child: Container(
                width: 100,
                height: 100,
                color: Colors.blue,
                child: Center(child: Text('Translate')),
              ),
            ),
            // Skew
            Transform(
              transform: Matrix4.skewX(0.3),
              child: Container(
                width: 100,
                height: 100,
                color: Colors.purple,
                child: Center(child: Text('Skew')),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

Transform provides named constructors for common transformations:  
rotate(), scale(), and translate(). The generic Transform() constructor  
accepts a Matrix4 for complex transformations like skewing and 3D  
effects. Transformations don't affect layout; they only affect painting.  

## LayoutBuilder Responsive

LayoutBuilder provides access to parent constraints during build time,  
enabling responsive layouts that adapt to available space. It's essential  
for creating truly responsive Flutter applications.  

```dart
import 'package:flutter/material.dart';

class LayoutBuilderExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('LayoutBuilder Responsive')),
      body: LayoutBuilder(
        builder: (context, constraints) {
          if (constraints.maxWidth > 600) {
            // Wide screen layout
            return Row(
              children: [
                Expanded(
                  flex: 1,
                  child: Container(
                    color: Colors.blue[200],
                    child: Center(
                      child: Text(
                        'Sidebar\n(Wide Screen)',
                        textAlign: TextAlign.center,
                      ),
                    ),
                  ),
                ),
                Expanded(
                  flex: 3,
                  child: Container(
                    color: Colors.green[200],
                    padding: EdgeInsets.all(16),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          'Main Content',
                          style: TextStyle(
                            fontSize: 24,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        SizedBox(height: 16),
                        Text(
                          'This layout adapts based on screen width. '
                          'On wide screens (>600px), content is arranged '
                          'in a row with sidebar.',
                        ),
                      ],
                    ),
                  ),
                ),
              ],
            );
          } else {
            // Narrow screen layout
            return Column(
              children: [
                Container(
                  width: double.infinity,
                  height: 100,
                  color: Colors.blue[200],
                  child: Center(
                    child: Text('Header (Narrow Screen)'),
                  ),
                ),
                Expanded(
                  child: Container(
                    color: Colors.green[200],
                    padding: EdgeInsets.all(16),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text(
                          'Main Content',
                          style: TextStyle(
                            fontSize: 24,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        SizedBox(height: 16),
                        Text(
                          'On narrow screens (≤600px), the layout '
                          'switches to a column with header on top.',
                        ),
                      ],
                    ),
                  ),
                ),
              ],
            );
          }
        },
      ),
    );
  }
}
```

LayoutBuilder's builder function receives BoxConstraints containing  
maxWidth, maxHeight, minWidth, and minHeight. These constraints  
enable responsive decisions about layout structure, widget sizes,  
and content organization based on available space.  

## FractionalTranslation Layout

FractionalTranslation moves its child by a fraction of the child's  
size rather than absolute pixels. This creates resolution-independent  
positioning that scales naturally with content size.  

```dart
import 'package:flutter/material.dart';

class FractionalTranslationExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('FractionalTranslation')),
      body: Center(
        child: Stack(
          children: [
            Container(
              width: 200,
              height: 200,
              decoration: BoxDecoration(
                color: Colors.grey[300],
                border: Border.all(color: Colors.grey),
              ),
              child: Center(child: Text('Base Container')),
            ),
            FractionalTranslation(
              translation: Offset(-0.5, -0.5),
              child: Container(
                width: 100,
                height: 100,
                color: Colors.red.withOpacity(0.8),
                child: Center(
                  child: Text(
                    'Translated\n-50%, -50%',
                    textAlign: TextAlign.center,
                    style: TextStyle(color: Colors.white),
                  ),
                ),
              ),
            ),
            FractionalTranslation(
              translation: Offset(0.3, 0.3),
              child: Container(
                width: 80,
                height: 80,
                color: Colors.blue.withOpacity(0.8),
                child: Center(
                  child: Text(
                    '+30%\n+30%',
                    textAlign: TextAlign.center,
                    style: TextStyle(color: Colors.white),
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
```

FractionalTranslation accepts an Offset where -1.0 moves the widget  
by its full size in the negative direction, and 1.0 moves it by  
its full size in the positive direction. This creates predictable  
positioning regardless of the widget's actual dimensions.  

## Positioned Layout

Positioned works exclusively within Stack widgets to provide absolute  
positioning similar to CSS absolute positioning. It offers precise  
control over widget placement within the stack bounds.  

```dart
import 'package:flutter/material.dart';

class PositionedLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Positioned Layout')),
      body: Container(
        width: double.infinity,
        height: double.infinity,
        child: Stack(
          children: [
            // Background
            Container(
              color: Colors.grey[100],
              child: Center(
                child: Text(
                  'Stack Background',
                  style: TextStyle(fontSize: 24, color: Colors.grey[400]),
                ),
              ),
            ),
            // Top-left corner
            Positioned(
              top: 20,
              left: 20,
              child: Container(
                padding: EdgeInsets.all(8),
                color: Colors.red,
                child: Text('Top Left', style: TextStyle(color: Colors.white)),
              ),
            ),
            // Top-right corner
            Positioned(
              top: 20,
              right: 20,
              child: Container(
                padding: EdgeInsets.all(8),
                color: Colors.green,
                child: Text('Top Right', style: TextStyle(color: Colors.white)),
              ),
            ),
            // Center with specific size
            Positioned(
              top: 100,
              left: 50,
              width: 200,
              height: 100,
              child: Container(
                color: Colors.blue,
                child: Center(
                  child: Text(
                    'Fixed Position\n& Size',
                    textAlign: TextAlign.center,
                    style: TextStyle(color: Colors.white),
                  ),
                ),
              ),
            ),
            // Bottom spanning full width
            Positioned(
              bottom: 20,
              left: 20,
              right: 20,
              child: Container(
                height: 60,
                color: Colors.purple,
                child: Center(
                  child: Text(
                    'Bottom Span (left + right positioning)',
                    style: TextStyle(color: Colors.white),
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
```

Positioned uses top, bottom, left, and right properties to specify  
distances from stack edges. When width/height are specified, the  
widget has fixed dimensions. When left and right are both specified,  
the width is determined by the remaining space.  

## Spacer Layout

Spacer creates flexible empty space that expands to fill available  
space in Row, Column, or Flex widgets. It's more semantic than using  
Expanded with empty containers.  

```dart
import 'package:flutter/material.dart';

class SpacerLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Spacer Layout')),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          children: [
            // Row with spacers
            Container(
              height: 60,
              decoration: BoxDecoration(
                color: Colors.blue[50],
                border: Border.all(color: Colors.blue[200]!),
              ),
              child: Row(
                children: [
                  Container(
                    padding: EdgeInsets.all(8),
                    color: Colors.red[200],
                    child: Text('Start'),
                  ),
                  Spacer(), // Flexible space
                  Container(
                    padding: EdgeInsets.all(8),
                    color: Colors.green[200],
                    child: Text('Middle'),
                  ),
                  Spacer(flex: 2), // Double the space
                  Container(
                    padding: EdgeInsets.all(8),
                    color: Colors.blue[200],
                    child: Text('End'),
                  ),
                ],
              ),
            ),
            SizedBox(height: 20),
            // Column with spacers
            Container(
              height: 200,
              width: double.infinity,
              decoration: BoxDecoration(
                color: Colors.green[50],
                border: Border.all(color: Colors.green[200]!),
              ),
              child: Column(
                children: [
                  Container(
                    padding: EdgeInsets.all(8),
                    color: Colors.purple[200],
                    child: Text('Top'),
                  ),
                  Spacer(),
                  Container(
                    padding: EdgeInsets.all(8),
                    color: Colors.orange[200],
                    child: Text('Center'),
                  ),
                  Spacer(),
                  Container(
                    padding: EdgeInsets.all(8),
                    color: Colors.teal[200],
                    child: Text('Bottom'),
                  ),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

Spacer automatically fills available space, pushing other widgets  
apart. The flex property controls space distribution when multiple  
Spacers exist. It's cleaner than using Expanded with empty containers  
and clearly indicates the intent to create flexible spacing.  

## Divider Layout

Divider creates visual separation between content sections with  
customizable appearance. It automatically spans the full width  
and provides consistent visual hierarchy.  

```dart
import 'package:flutter/material.dart';

class DividerLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Divider Layout')),
      body: Column(
        children: [
          ListTile(
            leading: Icon(Icons.account_circle),
            title: Text('Profile'),
            subtitle: Text('Manage your account'),
          ),
          Divider(), // Default divider
          ListTile(
            leading: Icon(Icons.settings),
            title: Text('Settings'),
            subtitle: Text('App preferences'),
          ),
          Divider(
            height: 20,
            thickness: 2,
            color: Colors.blue,
            indent: 20,
            endIndent: 20,
          ),
          ListTile(
            leading: Icon(Icons.help),
            title: Text('Help & Support'),
            subtitle: Text('Get assistance'),
          ),
          Divider(
            height: 40,
            thickness: 1,
            color: Colors.grey[400],
          ),
          Container(
            padding: EdgeInsets.all(16),
            width: double.infinity,
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  'Section with Custom Dividers',
                  style: Theme.of(context).textTheme.headlineSmall,
                ),
                SizedBox(height: 8),
                Text('Content above divider'),
                Divider(
                  color: Colors.red,
                  thickness: 3,
                ),
                Text('Content below divider'),
              ],
            ),
          ),
          Spacer(),
          Divider(thickness: 2),
          Container(
            padding: EdgeInsets.all(16),
            child: Text('Footer Content'),
          ),
        ],
      ),
    );
  }
}
```

Divider properties include height (total height including spacing),  
thickness (line thickness), color, and indent/endIndent for margins.  
It automatically uses theme colors when no color is specified,  
maintaining consistent styling across the app.  

## AspectRatio Layout

AspectRatio forces its child to have a specific width-to-height ratio,  
maintaining proportions regardless of available space. It's essential  
for images, videos, and maintaining visual consistency.  

```dart
import 'package:flutter/material.dart';

class AspectRatioLayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('AspectRatio Layout')),
      body: SingleChildScrollView(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Square Aspect Ratio (1:1)',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            SizedBox(height: 8),
            AspectRatio(
              aspectRatio: 1.0,
              child: Container(
                color: Colors.blue[200],
                child: Center(
                  child: Text(
                    'Square\n1:1',
                    textAlign: TextAlign.center,
                    style: TextStyle(fontSize: 18),
                  ),
                ),
              ),
            ),
            SizedBox(height: 20),
            Text(
              'Video Aspect Ratio (16:9)',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            SizedBox(height: 8),
            AspectRatio(
              aspectRatio: 16 / 9,
              child: Container(
                color: Colors.green[200],
                child: Center(
                  child: Text(
                    'Video Format\n16:9',
                    textAlign: TextAlign.center,
                    style: TextStyle(fontSize: 18),
                  ),
                ),
              ),
            ),
            SizedBox(height: 20),
            Text(
              'Portrait Aspect Ratio (3:4)',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            SizedBox(height: 8),
            Row(
              children: [
                Expanded(
                  child: AspectRatio(
                    aspectRatio: 3 / 4,
                    child: Container(
                      color: Colors.red[200],
                      child: Center(
                        child: Text(
                          'Portrait\n3:4',
                          textAlign: TextAlign.center,
                          style: TextStyle(fontSize: 18),
                        ),
                      ),
                    ),
                  ),
                ),
                SizedBox(width: 16),
                Expanded(
                  child: AspectRatio(
                    aspectRatio: 3 / 4,
                    child: Container(
                      color: Colors.purple[200],
                      child: Center(
                        child: Text(
                          'Portrait\n3:4',
                          textAlign: TextAlign.center,
                          style: TextStyle(fontSize: 18),
                        ),
                      ),
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

AspectRatio calculates dimensions automatically based on available  
constraints and the specified ratio. A ratio of 2.0 means width is  
twice the height, while 0.5 means height is twice the width.  
This ensures consistent proportions across different screen sizes.  

## FractionallySizedBox Layout

FractionallySizedBox sizes its child as a fraction of available space.  
It's perfect for creating proportional layouts that adapt to different  
screen sizes while maintaining relative sizing relationships.  

```dart
import 'package:flutter/material.dart';

class FractionallySizedBoxExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('FractionallySizedBox')),
      body: Column(
        children: [
          Container(
            height: 100,
            color: Colors.grey[200],
            child: Column(
              children: [
                Text('Container showing full width'),
                SizedBox(height: 8),
                FractionallySizedBox(
                  widthFactor: 0.8,
                  child: Container(
                    height: 30,
                    color: Colors.blue[200],
                    child: Center(child: Text('80% Width')),
                  ),
                ),
                SizedBox(height: 8),
                FractionallySizedBox(
                  widthFactor: 0.5,
                  child: Container(
                    height: 30,
                    color: Colors.green[200],
                    child: Center(child: Text('50% Width')),
                  ),
                ),
              ],
            ),
          ),
          SizedBox(height: 20),
          Expanded(
            child: Row(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: [
                Expanded(
                  child: Container(
                    color: Colors.red[100],
                    child: Column(
                      children: [
                        Text('Height Fractions'),
                        SizedBox(height: 16),
                        FractionallySizedBox(
                          heightFactor: 0.3,
                          child: Container(
                            width: double.infinity,
                            color: Colors.red[300],
                            child: Center(child: Text('30% Height')),
                          ),
                        ),
                        SizedBox(height: 16),
                        FractionallySizedBox(
                          heightFactor: 0.2,
                          child: Container(
                            width: double.infinity,
                            color: Colors.red[500],
                            child: Center(child: Text('20% Height')),
                          ),
                        ),
                      ],
                    ),
                  ),
                ),
                SizedBox(width: 20),
                Expanded(
                  child: Container(
                    color: Colors.purple[100],
                    child: Center(
                      child: FractionallySizedBox(
                        widthFactor: 0.8,
                        heightFactor: 0.6,
                        child: Container(
                          color: Colors.purple[300],
                          child: Center(
                            child: Text(
                              '80% Width\n60% Height',
                              textAlign: TextAlign.center,
                            ),
                          ),
                        ),
                      ),
                    ),
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

FractionallySizedBox uses widthFactor and heightFactor properties  
to specify fractions from 0.0 to 1.0. A factor of 0.5 makes the  
child half the available size in that dimension. When a factor  
is null, the child uses its natural size in that dimension.  

## CustomScrollView Sliver Layout

CustomScrollView provides advanced scrolling with sliver widgets that  
can expand, contract, and transform during scrolling. Slivers enable  
sophisticated scroll effects and efficient rendering of large datasets.  

```dart
import 'package:flutter/material.dart';

class CustomScrollViewExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: CustomScrollView(
        slivers: [
          SliverAppBar(
            expandedHeight: 200,
            floating: true,
            pinned: true,
            flexibleSpace: FlexibleSpaceBar(
              title: Text('Sliver App Bar'),
              background: Container(
                decoration: BoxDecoration(
                  gradient: LinearGradient(
                    colors: [Colors.blue[400]!, Colors.blue[800]!],
                    begin: Alignment.topCenter,
                    end: Alignment.bottomCenter,
                  ),
                ),
                child: Center(
                  child: Icon(
                    Icons.flutter_dash,
                    size: 80,
                    color: Colors.white.withOpacity(0.3),
                  ),
                ),
              ),
            ),
          ),
          SliverToBoxAdapter(
            child: Container(
              padding: EdgeInsets.all(16),
              child: Text(
                'Welcome to Flutter Layouts!',
                style: Theme.of(context).textTheme.headlineMedium,
                textAlign: TextAlign.center,
              ),
            ),
          ),
          SliverGrid(
            delegate: SliverChildBuilderDelegate(
              (context, index) => Container(
                decoration: BoxDecoration(
                  color: Colors.primaries[index % Colors.primaries.length],
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Center(
                  child: Text(
                    'Grid ${index + 1}',
                    style: TextStyle(
                      color: Colors.white,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              ),
              childCount: 6,
            ),
            gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
              crossAxisCount: 2,
              childAspectRatio: 1.5,
              crossAxisSpacing: 8,
              mainAxisSpacing: 8,
            ),
          ),
          SliverToBoxAdapter(
            child: Padding(
              padding: EdgeInsets.all(16),
              child: Text(
                'List Items',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
            ),
          ),
          SliverList(
            delegate: SliverChildBuilderDelegate(
              (context, index) => ListTile(
                leading: CircleAvatar(
                  backgroundColor: Colors.teal[100],
                  child: Text('${index + 1}'),
                ),
                title: Text('Sliver List Item ${index + 1}'),
                subtitle: Text('This item is in a SliverList'),
                trailing: Icon(Icons.arrow_forward_ios, size: 16),
              ),
              childCount: 20,
            ),
          ),
        ],
      ),
    );
  }
}
```

CustomScrollView coordinates multiple sliver widgets to create seamless  
scrolling experiences. SliverAppBar can collapse and expand, SliverGrid  
provides efficient grid layouts, and SliverList handles dynamic content.  
This approach offers maximum flexibility for complex scrolling interfaces.  
