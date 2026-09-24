# Port Of `bloc_signals_flutter` → `bloc_signals_dn`

`bloc_signals_dn` is the **DartNative port of [`bloc_signals_flutter`](https://pub.dev/packages/bloc_signals_flutter)**.

The goal of this package is to bring the `BlocSignal` widget APIs to DartNative while keeping the existing `BlocSignal` architecture and behavior as close as possible to the original Flutter implementation.

## Why this package?

DartNative provides its own reactive and state-related primitives, including features such as signals, computed values, and effects.

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
# Extra Care
- `signals_core` and `signals_core_extended` use the same primitive types for signals, computeds and effects. But they are not interchangeable with DartNative implementation.
- `bloc_signals_flutter` uses the same API with `signals_flutter`.
- So the only way to use **BlocSignal** with DartNative is to use `signals_core` and `signals_core_extended` in place of DartNative state related reactive paradigm like  `signal` (Signal), `effect` (Effect), `Computed` (Computed), `computed` (Computed), and so, 
```dart
// for `signals_core` API's
import 'package:bloc_signals_dn/signals_core.dart';
// for `signals_core_extended` API's
import 'package:bloc_signals_dn/signals_core_extended.dart';
```
## Compatibility

This package is intended to provide an API that is as close as practical to `bloc_signals_flutter`.

Where DartNative exposes a different framework API, the implementation uses the DartNative equivalent while preserving the original semantics where possible.

## Design principle

The core principle of this port is:

> **Port the framework integration, not the state-management architecture.**

`bloc_signals_dn` exists so applications can continue using `BlocSignal` while targeting DartNative, without having to rewrite their state-management layer around DartNative-specific state abstractions.
