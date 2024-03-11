---
title: Utilizing Carbon Design System Figma Kit to Auto-generate Color Tokens
author: code on sunday
date: 2024-03-11T09:10:58+07:00
tags:
  - HybridFlutterIDE
description: To prepare for implementing a design system, I need to prepare some basic elements like color palettes and typography.
---
I plan to use the [Carbon Design System](https://carbondesignsystem.com/) as the theme for our upcoming Hybrid Flutter IDE product. It is a design system with very comprehensive documentation, perhaps even more so than the Material Design System.

I also appreciate the clean, simple, and professional style of this system. It's very suitable for creating interfaces aimed at increasing work efficiency, such as an IDE interface for Flutter developers, for example.

To prepare for implementing a design system, I need to prepare some basic elements like color palettes and typography.

In this article, I will briefly explain how I built the color system in code based on the Carbon Design System.
## What Are Color Tokens?

To avoid using specific color values (black, yellow, red, etc.) in design, design systems use the concept of `color tokens` as intermediaries.

For example, define a color token named `$background`. This token will have a specific value depending on what theme is being used.
- Light theme: `$background = white`
- Dark theme: `$background = black`

Then, when writing a document or designing a button, the background color of the button is set to the `$background` token instead of specifying black or white.

This way, if they later decide not to use white for the background in light theme anymore, they don't need to revise the document or design in every place that uses the button. They only need to change the value of `$background` in one place.

Sounds a lot like how we use variables, right? Exactly, it's just called something different outside the programming field.

So, if we also define a variable as `background` in the code and assign it a value of `white` or `black` depending on the theme mode, we can achieve a similar benefit in decoupling the use of color from its definition.

My job is to bring all the color tokens from each theme in the Carbon Design System into the code.

## How To Automatically Generate Code For Color Tokens?

The list of color tokens is quite long, and they are divided into 4 themes as shown. I don't want to declare each variable manually, as it's time-consuming and seems a bit dull.

Therefore, I will find a way to automatically transform all the definitions of color tokens into code in the correct Dart format.

![Carbon colors](images/Pasted%20image%2020240309085121.png)

Using my experience with Figma, I immediately found the Carbon Design System kit. It contains the design of all the components in the system, and of course, it also includes the component I'm looking for - the color tokens.

These tokens are defined as `variables` in Figma. It's the concept of `variables` in code, but displayed as a UI.

https://www.figma.com/file/TXscEfrRKi7XKdjLW9S2tu/(v11)-All-themes---Carbon-Design-System-(Community)?type=design&node-id=169-0&mode=design&t=69bxO1lxCgV4ANV4-0

![Figma variables](images/Pasted%20image%2020240309085844.png)

## How to Convert All Figma Variables Into Text Format

Fortunately, I found a Figma plugin that allows converting all these variables into JSON format. This is great because I can then write code to read the JSON file and generate new code.

https://www.figma.com/community/plugin/1256972111705530093

After exporting all the color variables using the aforementioned plugin, I got a JSON file that looks like this.

![](content/en/posts/generate-color-tokens/images/Pasted%20image%2020240309091600.png)

I need to stay calm. After a simple collapse, the JSON file was reduced to only display 5 top-level information fields like this. I can almost guess their meaning now.

![](content/en/posts/generate-color-tokens/images/Pasted%20image%2020240309091851.png)

The `variables` field contains the most data. Therefore, I filtered out 2 elements in the `variables` list and brought them along with the remaining fields into a new JSON file to create a much simplified version.

With this approach, I can take it as an example to request ChatGPT to write me a code snippet to convert the data in this JSON file into Dart code.

## Turning JSON Into Color Objects

The code from ChatGPT isn't complete. I used it as a base and added some parts to handle missing cases like:
- Variables in the JSON file that are not of type `color` will not be written into the code.
- Some variables in the JSON file are assigned by another variable, which needs to be handled to obtain the actual value.
- The names of the variables in the JSON file must be converted to camelCase according to Dart's convention.

## Result

After modifying the complete code, I was able to generate an abstract class containing the names of all color tokens like this.

```dart
abstract class CDSThemeColors {
  Color get background;
  Color get backgroundHover;
  Color get backgroundActive;
  Color get backgroundSelected;
  Color get backgroundSelectedHover;
  Color get backgroundInverse;
  Color get backgroundInverseHover;
  Color get backgroundBrand;
  Color get layer01;
  Color get layer02;
  Color get layer03;
  Color get layerHover01;
  Color get layerHover02;
  Color get layerHover03;
  Color get layerActive01;
  Color get layerActive02;
  // More ...
}
```

And I also generated 4 derived classes containing the specific values of each color token according to each theme.

```dart
class WhiteThemeColors implements CDSThemeColors {
  @override
  Color get background => Color.fromARGB(255, 255, 255

, 255);
  @override
  Color get backgroundHover => Color.fromARGB(141, 141, 141, 30);
  @override
  Color get backgroundActive => Color.fromARGB(141, 141, 141, 127);
  // More ...
}
```

```dart
class Gray10ThemeColors implements CDSThemeColors {
  // Implementation
}
```

```dart
class Gray90ThemeColors implements CDSThemeColors {
  // Implementation
}
```

```dart
class Gray100ThemeColors implements CDSThemeColors {
  // Implementation
}
```

The entire source code can be viewed here:

[carbon-design-system/tokens_generator at master · code-on-sunday/carbon-design-system (github.com)](https://github.com/code-on-sunday/carbon-design-system/tree/master/tokens_generator)