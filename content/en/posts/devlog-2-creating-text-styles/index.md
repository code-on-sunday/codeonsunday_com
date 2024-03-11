---
title: "Devlog #2: Creating a Series of TextStyle in Flutter with ChatGPT"
author: code on sunday
date: 2024-03-12T06:31:49+07:00
description: In episode 2, I will implement a series of TextStyle objects based on the specifications documented by the Carbon Design System.
tags:
  - HybridFlutterIDE
---
In episode 2, I will implement a series of TextStyle objects based on the specifications documented by the Carbon Design System.
## Planning Before Implementation

Before getting started with the implementation, I spent about 30 minutes studying the Text (referred to as Typography) documentation of the Carbon Design System.
Each style in the documentation, similar to Flutter's TextStyle, includes size, line height, weight, and letter spacing. Each style has its unique name, similar to the color token I mentioned in [episode 1](../generate-color-tokens/index).

```
body-compact-01  
Type: IBM Plex Sans  
Size: 14px / .875rem  
Line height: 18px / 1.125rem  
Weight: 400 / Regular  
Letter spacing: .16px  
Type set: Productive
```

However, some styles have multiple sets of specifications corresponding to the same token, depending on what the current breakpoint is. I temporarily call these responsive styles. For example:
- At screen sizes < 672 (breakpoint `sm`)
```
fluid-display-03  
Type: IBM Plex Sans  
Size: 42px / 2.625rem  
Line height: 50px / 3.125rem  
Weight: 300 / Light  
Letter spacing: 0px  
Type set: Expressive
```
- At screen sizes < 1056 (breakpoint `md`)
```
fluid-display-03  
Type: IBM Plex Sans  
Size: 54px / 3.375rem  
Line height: 64px / 4rem  
Weight: 300 / Light  
Letter spacing: 0px  
Type set: Expressive
```

If these responsive styles are frequently used in design, I definitely have to define them completely. However, for the current needs, perhaps I only need the fixed styles. Deciding like this will help me reduce a lot of work time.

## Using ChatGPT To Code TextStyle

Now, my job is simply to read each set of specifications corresponding to each style to write the corresponding Flutter code. Although this task is simple, it's quite tedious, so why not use ChatGPT?

![](images/Pasted%20image%2020240311215252.png)

With just 2 simple prompts, I got the code for all Utility styles.

Reference here: https://chat.openai.com/share/cce03137-0d4b-49ab-a3ff-91647383be38

The code will look like this:

```dart
extension TextStyleGetter on BuildContext {
  TextStyle get code01 => TextStyle(
        fontFamily: 'IBM Plex Mono',
        fontSize: 12.0,
        height: 16.0 / 12.0,
        fontWeight: FontWeight.w400,
        letterSpacing: 0.32,
      );
}
```

Here, I return TextStyle through an extension of BuildContext so that later, if I need to return different styles depending on the screen size, I won't need to modify the code in places that are using these styles.

After some copying and pasting, I have created a long list of necessary TextStyles, and now I'm ready to start making some beautiful widgets in episode 3!

Source code can be seen here: [code-on-sunday/carbon-design-system (github.com)](https://github.com/code-on-sunday/carbon-design-system)