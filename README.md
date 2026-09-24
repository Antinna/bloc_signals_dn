# Port Of `bloc_signals_flutter` → `bloc_signals_dn`

`bloc_signals_dn` is the **DartNative port of [`bloc_signals_flutter`](https://pub.dev/packages/bloc_signals_flutter)**.

The goal of this package is to bring the `BlocSignal` widget APIs to DartNative while keeping the existing `BlocSignal` architecture and behavior as close as possible to the original Flutter implementation.

## Why this package?

DartNative provides its own reactive and state-related primitives, including features such as signals, computed values, effects, and context-based reactive APIs.

This package intentionally **does not replace `BlocSignal` with DartNative's state-management approach**.

Instead, `bloc_signals_dn` continues to use the existing `BlocSignal` APIs and uses DartNative primarily as the UI/framework layer required to run those APIs outside Flutter.

In other words:

```text
BlocSignal
    ↓
signals_core
    ↓
DartNative UI layer
```

rather than introducing DartNative-specific state abstractions throughout the application.

## Extra Care: Avoid DartNative's Default State APIs

`signals_core` and `signals_core_extended` expose compatible reactive primitives such as:

```text
signal<T>()
computed()
effect()
```

These APIs may look similar to DartNative's own reactive primitives, but they are **not the same implementation or state-management layer**.

Likewise, `bloc_signals_flutter` uses the corresponding signal APIs from `signals_flutter`.

For `bloc_signals_dn`, the `BlocSignal` state-management layer should continue to use the `signals_core` ecosystem rather than DartNative's built-in reactive state paradigm.

In particular, avoid using DartNative's state-management APIs as an alternative to `BlocSignal`, such as:

```text
signal<T>()
Signal
effect()
Effect
Computed
computed()
context.watch(...)
context.read(...)
```

when those APIs are being used to manage application state.

Instead, use the `signals_core` APIs exposed by this package:

```dart
// For signals_core APIs:
import 'package:bloc_signals_dn/signals_core.dart';

// For signals_core_extended APIs:
import 'package:bloc_signals_dn/signals_core_extended.dart';
```

This keeps the application's state-management model based on:

```text
BlocSignal
    ↓
signals_core / signals_core_extended
```

while DartNative remains responsible for the UI and framework integration.

> **Note:** `bloc_signals_dn` may internally use reactive primitives such as `computed()` and `effect()` where required to implement selectors, listeners, and widget updates. This is an implementation detail of the package and does not mean that applications should adopt DartNative's separate state-management model.

## Compatibility

This package is intended to provide an API that is as close as practical to `bloc_signals_flutter`.

Where DartNative exposes a different framework API, the implementation uses the corresponding DartNative API while preserving the original semantics wherever possible.

## Design principle

The core principle of this port is:

> **Port the framework integration, not the state-management architecture.**

`bloc_signals_dn` exists so applications can continue using `BlocSignal` while targeting DartNative, without having to rewrite their state-management layer around DartNative-specific state abstractions.
