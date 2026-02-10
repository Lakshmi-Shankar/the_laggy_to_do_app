# Case Study: The Laggy To‑Do App (TaskEase)

## Overview

TaskEase is a cross‑platform productivity app built using **Flutter**. While the app performs smoothly on Android, users experience noticeable UI lag on **iOS** when adding or removing tasks.

This case study analyzes the root cause of the performance issue and explains how Flutter’s **state management**, **reactive rendering**, and **Dart async model** can be used correctly to ensure smooth, consistent performance across platforms.

---

## 🐢 Problem Analysis: Why the App Feels Laggy

### 1. Improper State Management

The app relies heavily on `setState()` at high levels of the widget tree.

**What’s happening:**

* A single task addition/removal triggers `setState()` on the parent widget.
* This forces **the entire widget tree to rebuild**, including widgets that did not change.

**Impact:**

* Unnecessary widget rebuilds
* Increased CPU usage
* Dropped frames, especially noticeable on iOS

---

### 2. Deeply Nested Widgets

The UI is composed of multiple nested layout widgets (`Column → ListView → Row → Container → Text`).

**What’s happening:**

* Every rebuild walks through the entire nested tree.
* Flutter has to re‑evaluate layout and paint phases repeatedly.

**Impact:**

* Slower frame rendering
* Layout recalculations become expensive

---

### 3. Rebuilding Static Widgets

Widgets that never change (AppBar, headers, icons) are rebuilt repeatedly.

**What’s happening:**

* Widgets that could be `const` are recreated every frame.

**Impact:**

* Missed optimizations in Flutter’s widget comparison
* Extra object allocations

---

## ⚙️ How Flutter’s Reactive Rendering Helps

Flutter uses a **reactive UI model**:

* UI = function(state)
* When state changes → framework rebuilds widgets

### Efficient Rebuilds (When Done Right)

Flutter does **not** redraw the entire screen automatically.

Instead:

1. Widgets are rebuilt (cheap)
2. Elements are compared
3. Only **dirty render objects** are repainted

✅ When state is localized, Flutter updates **only the affected subtree**, keeping frame rendering close to **60 FPS**.

---

## ⚡ Dart’s Async Model & Smooth Performance

Dart uses a **single‑threaded event loop** with:

* Event queue
* Microtask queue
* Non‑blocking `async/await`

### Why This Matters

* UI rendering runs on the **main isolate**
* Heavy operations (DB, network, file IO) run asynchronously

**Best Practices Used:**

* `Future`‑based task operations
* No blocking code inside `build()`
* Smooth UI even during task updates

---

## ✅ Optimized Solution Used in TaskEase

### 1. Localized State Updates

Instead of calling `setState()` at the screen level:

* Task list state is managed inside a dedicated widget
* Each task item rebuilds independently

Examples:

* `ValueNotifier + ValueListenableBuilder`
* `ChangeNotifier + Consumer`

---

### 2. Efficient Widget Tree Design

* Split large widgets into **small reusable widgets**
* Used `const` constructors wherever possible
* Avoided deep nesting

---

### 3. Targeted Rebuilds

Only the task list updates when:

* A task is added
* A task is removed

Other UI elements (AppBar, filters, footer) remain untouched.

---

## 🎥 Video Demonstration (What We Show)

In the demo video, we demonstrate:

* Adding/removing tasks with **no visible lag**
* Flutter DevTools rebuild counter
* Only task items rebuilding, not the full screen
* Stable frame rendering on iOS

---

## 📈 Result

After optimization:

* ✅ Smooth scrolling
* ✅ Instant task updates
* ✅ No dropped frames on iOS
* ✅ Consistent performance across Android & iOS

---

## 🏁 Conclusion

UI lag in Flutter apps is rarely due to the framework itself. In TaskEase, the root cause was **poor state management and unnecessary widget rebuilds**.

By:

* Localizing state
* Leveraging Flutter’s reactive rendering correctly
* Using Dart’s async model responsibly

We achieved a responsive, high‑performance to‑do app across platforms.

---

**Author:** TaskEase Mobile Engineering Team
