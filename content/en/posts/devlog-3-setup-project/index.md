---
title: "Devlog #3: Initial Setup for a New Flutter Project"
author: code on sunday
description: This article will be updated further to create a comprehensive list supporting the quick setup of new Flutter projects.
date: 2024-03-12T09:23:10+07:00
tags:
  - HybridFlutterIDE
---
## Quickly Create a New Flutter Project in VSCode

First, open the VSCode Command Palette using `Ctrl Shift P` (Windows) or `Cmd Shift P` (MacOS).

Then type `flutter new` and select the first suggestion `Flutter: New project`.

![](images/Pasted%20image%2020240312064558.png)

Next, choose the template type you want to base your new Flutter project on. Select the second line: `Empty Application`.

![](images/Pasted%20image%2020240312064928.png)

Next, select the parent folder to contain the project folder you're about to create.

Finally, fill in the name of the project, which will also be the name of the project folder created inside the parent folder you just selected.

Note that the project name can only consist of alphabetic characters and the underscore `_`.

By doing this, you will create a new Flutter project with the least possible code, completely devoid of unnecessary comments and sample code.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MainApp());
}

class MainApp extends StatelessWidget {
  const MainApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: Scaffold(
        body: Center(
          child: Text('Hello World!'),
        ),
      ),
    );
  }
}
```

## Using very_good_analysis

By default, a new Flutter project already applies a set of analysis options from flutter_lints to help detect bad code spots (not adhering to conventions, practices) within the project.

However, I still find it insufficient. For example, when calling a function that returns a `Future`, sometimes I forget not to `await`. If I'm testing right there, I'll know and be able to correct it immediately, but if I leave it for a few hours, I might forget about it and have to sit down to debug to find out why the code isn't running as expected.

Therefore, I add the `very_good_analysis` package to have a more fully equipped set of analysis options. As in the above case, it will remind me that I haven't `awaited` right at that function call.

![](images/Pasted%20image%2020240312070341.png)
## Some Other Packages

| Purpose           | Package                                                                                                                                      |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| State management   | flutter_bloc                                                                                                                                 |
| UI theme           | carbon_design_system (currently being developed in [devlog #1](generate-color-tokens/index) and [devlog #2](devlog-2-creating-text-styles/index)) |
| Icons              | carbon_icons                                                                                                                                 |

I will add other packages after completing a few initial interfaces. But first, I need to sketch the UI. Stay tuned for the next article.