# Case Study: The Laggy To-Do App (TaskEase)

## Overview

TaskEase is a Flutter-based cross-platform productivity app. While performance is smooth on Android, users noticed UI lag on iOS when adding or removing tasks.

This case study explains the cause of the issue and how proper **state management**, **reactive rendering**, and **Dart’s async model** help achieve smooth performance across platforms.

---

## 🐢 Why the App Felt Laggy

### 1. Improper State Management

The app used `setState()` high in the widget tree.

**Result:**

* Entire screen rebuilt for small task updates
* Higher CPU usage and dropped frames on iOS

---

### 2. Deep Widget Nesting

Multiple nested layout widgets caused expensive rebuilds.

**Result:**

* Slower layout and paint phases
* Reduced frame rate

---

### 3. Rebuilding Static Widgets

Static widgets (AppBar, headers) were rebuilt repeatedly instead of using `const`.

**Result:**

* Missed Flutter optimizations
* Extra object creation

---

## ⚙️ Flutter’s Reactive Rendering

Flutter follows a reactive model:

* UI = function(state)
* State change → widget rebuild

Flutter efficiently updates only **changed subtrees**, not the whole screen. When state is localized, only affected widgets repaint, maintaining ~**60 FPS**.

---

## ⚡ Dart Async Model

Dart uses a single-threaded event loop with non-blocking `async/await`.

**Benefits:**

* UI runs on the main isolate
* Heavy work runs asynchronously
* No UI freezing during task updates

---

## ✅ Optimized Solution

* Localized state inside task list widgets
* Each task item rebuilds independently
* Used `ValueNotifier` / `ChangeNotifier`
* Reduced widget nesting
* Applied `const` constructors

Only the task list updates when tasks change; the rest of the UI remains untouched.

---

## 🎥 Video Demo Highlights

* Lag-free task add/remove
* DevTools rebuild counter
* Only task widgets rebuild
* Smooth iOS performance

---

## 📈 Result

* Smooth scrolling
* Instant task updates
* No dropped frames on iOS
* Consistent Android & iOS performance

---

## 🏁 Conclusion

The lag was caused by **unnecessary rebuilds**, not Flutter itself. By managing state correctly, optimizing the widget tree, and using Dart’s async model properly, TaskEase delivers a fast and responsive UI across platforms.

**Author:** TaskEase Mobile Engineering Team
