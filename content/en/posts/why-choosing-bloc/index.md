---
title: "After Much Deliberation, Finally Choosing Bloc for Flutter Project at Company"
date: 2024-02-08T06:31:25+07:00
description: "When starting a Flutter project for the company, I invested a considerable amount of effort in deciding which state management tool to use. In this article, I will share the criteria I used to evaluate the three worthy candidates: Provider, Riverpod, and Bloc."
---

When starting a Flutter project for the company, I invested a considerable amount of effort in deciding which **state management** tool to use.

**State Management** is a crucial component of any front-end project. State management libraries have functions that we must call directly in the UI code.

For me, this meant that the project would deeply integrate with the chosen state management library.

This also implies that the project would be _dependent_ on that library to a large extent (**coupling**), and switching to another library later should be avoided as it would require a significant amount of time and effort.

Therefore, choosing a state management library is a one-time decision and long-lasting, leading to careful consideration.

In Flutter, there are three worthy candidates for this position: **Provider**, **Riverpod**, and **Bloc**.

I have researched and experimented with all three libraries based on the criteria below. Let's delve into each criterion to understand why I chose **Bloc** for this project.

A small note: the analysis below is based on a specific project. When dealing with a different project, analyze its requirements and carefully consider each criterion to choose the most suitable library.

## 1. State Management Candidates: Provider, Riverpod, Bloc

Firstly, why did I choose these three libraries?

### a. Provider:

Despite the fact that the author of Provider (**Remi Rousselet**) is also the author of **Riverpod**, and currently Remi is more focused on developing Riverpod, which could lead to the risk of Provider being **abandoned** in the future, I still include it in the list due to its simplicity and "orthodox" nature.

In Flutter, the built-in mechanism in the SDK to serve the purpose of sharing **state** between widgets on the tree is **InheritedWidget**.

Flutter SDK provides some functions for any widget at any position in the tree to retrieve an InheritedWidget from its ancestor on the tree. At the same time, it can also register to receive notifications when data in the InheritedWidget changes, updating the interface to display the new data.

These are the basic features of a State Management tool, and InheritedWidget fulfills this well.

However, every time we implement an InheritedWidget, we have to write a lot of boilerplate code. Moreover, it is too simplistic, requiring developers to add necessary features themselves.

That is why **Provider** was created. It wraps InheritedWidget, providing a simpler approach and additional features that InheritedWidget lacks.

Thus, in the future, even if Provider is abandoned, we can still fork its code to maintain it ourselves. Furthermore, due to its simple operating principle, Provider has reached a high level of stability, with a low likelihood of errors.

### b. Riverpod:

As Remi Rousselet stated, Riverpod is a new state management library built with the goal of **addressing the issues that Provider encounters**. For example, the issue of having to depend on BuildContext to access shared state.

Fundamentally, Riverpod creates a new _element tree_, different from Flutter's element tree. This is why, when using Riverpod, we have to create widgets by inheriting from new types of widgets, not the usual **StatelessWidget** or **StatefulWidget**.

It is precisely because of this mechanism that Riverpod allows access to shared state from anywhere without depending on BuildContext.

Because of this powerful capability, I include Riverpod in the list of candidates.

### c. Bloc:

Bloc is a widely used state management library in the Flutter community. It is built based on the **BLoC** pattern (Business Logic Component) suggested by Google in a Flutter presentation.

Bloc not only provides state management tools but also suggests structuring the project according to the BLoC pattern with three separate layers: **Presentation**, **Business Logic**, and **Data**, helping the project have a clear and maintainable structure.

In general, Bloc is more complex than Provider, but it is still closely integrated with Flutter as it does not require creating a new mechanism like Riverpod. Additionally, it is extremely popular. These are the reasons I include Bloc in the list of candidates.

## 2. Is It Easy to Learn? (Learning Curve)

To assess the choices, I rely on the actual situation of the project. In the team, except for me, everyone has no prior experience with Flutter. Although initially only two people in the team, including myself, are starting this project, after a short period, more people are likely to join to accelerate development.

Therefore, the ease of learning the state management library is an important criterion. People not only need time to learn Dart and Flutter but also have to learn about the state management library. Spending too much time learning will be a significant drawback.

- **Provider**: 5/5

  Provider is the easiest to learn. Typically, we combine Provider with **ChangeNotifier** to manage state. So, to use Provider, developers only need to grasp these two basic concepts.

- **Riverpod**: 2/5

  Riverpod is slightly more challenging because it has more concepts. However, the reason I rate it 2 points is that, in the context where most people do not know Flutter, loading both Flutter and Riverpod at the same time will become more difficult, doubling the challenge of learning both in a short period.

- **Bloc**: 4/5

  Bloc operates based on Streams. Developers need to learn about Streams, then about the layered structure and the concepts of Event and State, making it harder than Provider but still not as difficult as Riverpod. Moreover, Bloc has a large community, has been around for a long time, so there are many documents and examples available on the internet, making it easier for developers to learn. It also does not change the essence of Flutter, so it does not put pressure on newcomers.

## 3. Does It Have a Structure?

Project structure is another important criterion that I consider. When most people have no experience with Flutter, having a clear project structure, even if it is familiar to them from another framework, will help them catch up much faster, reducing the amount of knowledge they have to absorb.

Sometimes, just knowing which layer a class belongs to allows developers to guess its functionality and usage, even if they do not understand the specific concepts inside, they can still write, use, and maintain it.

- **Provider**: 2/5

  Provider does not suggest any project structure. Developers can place ChangeNotifier anywhere in the project, and there are no rules. This can lead to a messy project if not managed carefully. Especially in a context with tight deadlines, no time and resources for thorough reviews and guidance for team members, the lack of a predefined structure is a significant drawback.

- **Riverpod**: 4/5

  Riverpod separates **Provider** and **Notifier** from the widget and can be easily accessed from within the widget. This creates a separate layer to manage state. However, on the Riverpod homepage, no specific structure model is suggested. Andrea Bizzotto, a blogger deeply knowledgeable about Flutter, also wrote in [his blog post](https://codewithandrea.com/articles/flutter-app-architecture-riverpod-introduction/#riverpod-is-not-very-opinionated) that Riverpod does not have any opinionated structure.

  Of course, having a little is better than none, so I rate 4 points for Riverpod.

- **Bloc**: 5/5

  Right on the Bloc homepage, it introduces the BLoC model along with detailed explanations. Moreover, both the homepage and the community have created many examples and guides on how to structure the project according to the BLoC model.

  Thus, if there are any uncertainties, developers can easily learn by themselves, saving a lot of time. I give it a full 5 points, well-deserved.

## 4. Is It Powerful?

In reality, even without libraries, we can code components to meet our needs. But if the library provides these tools, why bother doing it from scratch?

- **Provider**: 4/5

  Due to its simplicity, Provider does not have many supporting tools.

  For example, **ChangeNotifier** is just a mechanism to call the callback functions of listeners to notify about state changes. It cannot provide powerful operators for handling data like **Stream**. Of course, we can totally place a **StreamController** inside ChangeNotifier to create a Stream, but here I am evaluating the availability of supporting tools.

  Moreover, when wanting to trigger an action when the state changes, we have to create a **StatefulWidget** to call the **addListener** and **removeListener** functions, or create a separate widget for reuse.

  Certainly, Provider comes equipped with many tools for testing and debugging, which is a strength that boosts its score.

- **Riverpod**: 5/5

  Needless to say, Riverpod is truly powerful. Right on its homepage, we see many cases where Riverpod can be used to solve problems quickly.

  For example, the error handling mechanism is clear, the caching mechanism is pre-equipped, and it can debounce and cancel network requests easily. Integration with Stream is also straightforward. Moreover, the testing and debugging tools are very powerful.

- **Bloc**: 3/5

  Bloc is also strong, integrating Stream and providing many powerful operators for data processing. It also has many ready-to-use widgets for reuse when needing to trigger an action when the state changes. The error handling mechanism is also convenient. Additionally, there are many tools for testing and debugging.

  However, it lacks a caching mechanism and pre-equipped cancel network requests like Riverpod. Also, Bloc is known for requiring a lot of boilerplate, although this can be mitigated by using extensions for VSCode and creating some base classes for reuse, or using a code generator.

  That is a drawback, so I rate it only 3 points in this section.

## 5. Summary

After totaling the scores, I have the ranking as follows:

- **Provider**: 11/15
- **Riverpod**: 11/15
- **Bloc**: 12/15

Thus, with the highest score, I have chosen **Bloc** for this project. This does not mean that Provider and Riverpod are not good, but simply that for this specific project, Bloc is the most suitable.

I hope you can find the most suitable library for your project after reading this article. Good luck!
