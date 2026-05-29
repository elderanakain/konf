---
layout: default
title: KotlinConf 2023
nav_order: 3
---

# KotlinConf 2023

Amsterdam, 13-14 April 2023. [Full playlist of recordings](https://www.youtube.com/playlist?list=PLlFc5cFwUnmwcJ7ZXyMmS70A9QFyUu1HI).

---

## Day 1 - 13 April

### Opening Keynote

> Roman Elizarov, Svetlana Isakova, Egor Tolstoy (JetBrains) & Grace Kloba (Google). [Recording](https://www.youtube.com/watch?v=c4f4SCEYA5Q).

- [Press release](https://blog.jetbrains.com/kotlin/2023/04/kotlinconf-2023-opening-keynote).
- K2 is coming in Kotlin `2.0`.
- After `2.0`: static extensions, name-based destructuring, context receivers, explicit fields.
- The Gradle Kotlin DSL will be the default for new builds.
- Kotlin Multiplatform will become **stable** this year.
- Google Workspace is experimenting with KMM.

---

### Kotlin & Functional Programming

> Urs Peter (Xebia). [Recording](https://www.youtube.com/watch?v=Ed3t4WAe0Co).

- Imperative programming → mutability + side-effects.
- Expression-oriented programming: functions + immutability; data transformation and expressiveness (input/output).
- Monads are containers: can be combined (`plus`), mapped (`map`), transformed (`flatMap`). Examples: `Optional`, `Future`, `Observable`, `kotlin.Result`.
- Unchecked vs checked exceptions: Kotlin has no checked exceptions, only `@Throws`; `Result` can signal that an exception is expected.
- [`arrow-kt`](https://github.com/arrow-kt/arrow) provides monads — e.g. [`@optics`](https://arrow-kt.io/learn/immutable-data/intro#meet-optics) for `copy()` of nested fields in immutable data, and `Either<ApiError, Result>` for APIs that have multiple error types.
- Idiomatic and functional Kotlin are getting closer in Kotlin 2.0.

---

### Dissecting Kotlin: Unsealing the Sealed, the SAM, and Other Syntax

> Huyen Tue Dao (Trello, Atlassian). [Recording](https://www.youtube.com/watch?v=qNAWWuY22Mg).

- `sealed interface`, `data object`.
- Unsigned types: not recommended for wide adoption because of `-1` business-logic edge cases; `ARGB` is usually a `Long`, but `UInt` can be used instead.
- `inline`/`value class` to build a foundation type with domain-specific logic.
- `typealias`: useful for verbose types (too many generics, function types); can be parameterized; no type safety (two aliases of the same type are interchangeable).

---

### Kotlin Multiplatform Conversions at Android Jetpack Scale

> Dustin Lam & James Ward (Google). [Recording](https://www.youtube.com/watch?v=PbwFtqmbMuI).

- Google's long-term commitment to KMP: Google Workspace will have KMP parts; feedback and adoption will determine how far Google goes; release stages are reliable; Java interoperability still matters (source compatibility).
- Google contributions to KMP: Jetpack libraries migrated ([`Annotation`](https://developer.android.com/jetpack/androidx/releases/annotation), [`Collection`](https://developer.android.com/jetpack/androidx/releases/collection), [`DataStore`](https://developer.android.com/jetpack/androidx/releases/datastore)); Gradle (Android ↔ KMP plugins); the Kotlin/Native compiler.
- `kotlin-stdlib`/`kotlin.io` is insufficient for `DataStore` → using `Okio` instead.
- `Kruth`, a KMP port of Google's Truth assertion library — **WIP**.

---

### Confetti: building a Kotlin Multiplatform conference app

> John O'Reilly (Neat) & Martin Bonnin (Apollo GraphQL). [Recording](https://www.youtube.com/watch?v=uATlWUBSx8Q).

- [`joreilly/Confetti`](https://github.com/joreilly/Confetti); focused on sharing non-UI code.
- GraphQL backend: [`ExpediaGroup/graphql-kotlin`](https://github.com/ExpediaGroup/graphql-kotlin) uses reflection, a beans DSL, and a dev playground; Kotlin models can add functions to merge data with other models; map specific types (e.g. `String` → `Date`) by augmenting the GraphQL schema.
- KMP clients: [`apollographql/apollo-kotlin`](https://github.com/apollographql/apollo-kotlin); `@NativeCoroutineState` ([`rickclephas/KMP-NativeCoroutines`](https://github.com/rickclephas/KMP-NativeCoroutines)) for iOS consumers; `KMMViewModel` to share a `ViewModel` between iOS and Android.

---

### How we test concurrent algorithms in Kotlin Coroutines

> Nikita Koval (JetBrains). [Recording](https://www.youtube.com/watch?v=jZqkWfa11Js).

- Testing concurrent data structures (e.g. `ConcurrentMap`, `ConcurrentLinkedDeque`).
- [`Kotlin/kotlinx-lincheck`](https://github.com/Kotlin/kotlinx-lincheck) library.
- Correctness can be checked against a sequential history.

---

### Reflections on a Year of Compose

> Christina Lee (Pinterest). [Recording](https://www.youtube.com/watch?v=6lBBpWX1x8Y).

- A story of a difficult Jetpack Compose adoption.
- Assuming benefits is dangerous — "do not adopt things just to adopt things".
- Stability: device-specific crashes, team inexperience; study the worst case, not the safest case.
- Performance: premature optimization is risky; Views are AOT-compiled, Compose is JIT-compiled; Compose was never intended to be faster than Views.

---

### The Changing Grain of Kotlin

> Nat Pryce & Duncan McGregor (independent; authors of *Java to Kotlin*). [Recording](https://www.youtube.com/watch?v=99pqhD_w-os).

- Unchecked exceptions reduce Kotlin's explicitness → use `Result` → that introduces monads → Kotlin has no monad support → flatten nested functional calls with `onFailure { return it }`.
- Warning signs that something is off: `!!`, `lateinit`, `var`, mutable collections, type casts, etc.
- Context receivers may have the same impact on Kotlin as coroutines did.

---

## Day 2 - 14 April

### Coroutines and Loom behind the scenes

> Roman Elizarov (JetBrains). [Recording](https://www.youtube.com/watch?v=zluKcazgkV4).

- Project Loom is releasing in 2023:
  - Introduces virtual threads in Java; a virtual thread uses a carrier thread to execute on a platform thread.
  - On `yield`, the virtual thread moves to the heap and frees the platform thread (mounting/unmounting) — same idea as coroutines.
  - Focused on the server side and RPC: one platform thread per request is expensive because of blocking operations; Loom lets you use a small pool of platform threads.
- Loom won't make coroutines obsolete:
  - Coroutines target highly concurrent code (e.g. many animation streams) and event handling.
  - The sync/async distinction matters for coroutines.
  - No structured concurrency in Loom yet (incubating).
  - In Loom the VM mounts threads; in coroutines the Kotlin compiler does.
- Loom vs coroutines: Loom has expensive `yield`, cheap `call`; coroutines have cheap `yield`, an expensive `suspend` call, but a cheap regular `call`. Loom has low memory usage; coroutines even lower.
- `Dispatchers.IO` creates physical threads — bad for blocking operations, but it is optimized to share threads. A Loom executor can serve as a coroutine dispatcher, and `Dispatchers.IO` may use virtual threads in the future (a specific Loom API is still missing).

---

### Kotlin/Multiplatform for iOS developers: state & future

> Salomon Brys (Kodein Koders). [Recording](https://www.youtube.com/watch?v=j-zEAMcMcjA).

- Share behaviour; don't share platform integrations that can't be cleanly abstracted.
- Kotlin → Objective-C/Swift:
  - Generics are lost — Objective-C generics can be bounded only by classes, not interfaces.
  - `@ObjCName` makes parameter names more semantic for Swift; `@ObjCName(swiftName = "_")` drops required parameter labels in Swift.
  - JetBrains is focusing on Swift interoperability.
  - `suspend` → `async` is supported; Flows are not properly supported → [`rickclephas/KMP-NativeCoroutines`](https://github.com/rickclephas/KMP-NativeCoroutines).
- Code distribution:
  - SPM (Swift Package Manager) is not officially supported yet — WIP.
  - SPM downloads and builds Swift source, and supports binaries too (the KMM case).
  - [`touchlab/KMMBridge`](https://github.com/touchlab/KMMBridge) manages publishing via SPM; ship a Swift package with Swift source alongside the binaries.
  - [`jpsim/SourceKitten`](https://github.com/jpsim/SourceKitten) generates Swift code from shared Kotlin instead of Objective-C classes with headers.
- Team collaboration:
  - Decouple KMM from Android and build mutual engagement with iOS devs.
  - Don't treat iOS developers as second-class citizens, and don't force every iOS developer to learn Kotlin.
  - iOS pain points: no proper IDE support, hard-to-read Xcode stack traces, a complex code-test-debug loop.
  - KMM developers should understand Objective-C and learn Swift.

---

### Kotlin Mobile for Teams

> Kevin Galligan (Touchlab). [Recording](https://www.youtube.com/watch?v=-tJvCOfJesk).

- Share common Kotlin with iOS as an Xcode framework → [`touchlab/KMMBridge`](https://github.com/touchlab/KMMBridge).
- Shared UI == failure:
  - Compose UI on iOS isn't great or fast yet.
  - Shared UI is hard and should still look native.
  - Sharing everything == Flutter.
- Publish via SPM.
- Everyone can share business logic: minimal disruption to the iOS team and versioning helps, but the feature and bug-fix cycle is slower and efficiency is limited.
- Move from platform-specific engineers to mobile engineers; everyone builds locally (close to feature dev, but can break the other platform); embrace breaking changes.
- Interoperability problem: iOS devs don't love Objective-C; the path is Kotlin → Objective-C → Swift; Swift interop is being discussed and probably coming; Kotlin ≠ Swift; assume Objective-C for a while; generics are painful.
- Work around output quirks; write wrappers for iOS/Swift.
- `SKIE` (Swift/Kotlin Interface Extender): translates enums, sealed classes, and default parameters; reactive interface for `suspend` and Flow; handles packaging; not open-source at the time.
- See also [`rickclephas/KMP-NativeCoroutines`](https://github.com/rickclephas/KMP-NativeCoroutines), [`icerockdev/moko-kswift`](https://github.com/icerockdev/moko-kswift).
- Collaboration with iOS: start with empathy — the problems they raise are real; it depends on iOS team size and acceptance; issues include coordination, inertia, specialized focus, and politically entrenched positions.

---

### Level up on Kotlin Multiplatform

> Pamela Hill (JetBrains). [Recording](https://www.youtube.com/watch?v=EKFdYgjCNOs).

- Networking: [`ktorio/ktor`](https://github.com/ktorio/ktor), [`Kotlin/kotlinx.serialization`](https://github.com/Kotlin/kotlinx.serialization).
- Persistence: [`russhwolf/multiplatform-settings`](https://github.com/russhwolf/multiplatform-settings), [`DataStore`](https://developer.android.com/jetpack/androidx/releases/datastore), [`cashapp/sqldelight`](https://github.com/cashapp/sqldelight), [`realm/realm-kotlin`](https://github.com/realm/realm-kotlin), [`xxfast/KStore`](https://github.com/xxfast/KStore).
- Coroutines on iOS → `async`/`await` with no cancellation support out of the box.
- DI: [`InsertKoinIO/koin`](https://github.com/InsertKoinIO/koin), [`kosi-libs/Kodein`](https://github.com/kosi-libs/Kodein).
- Use `@Throws` in common code for Swift.
- Exception/error reporting: `NSExceptionKt`, `CrashKiOS`.
- Logging: [`touchlab/Kermit`](https://github.com/touchlab/Kermit), [`AAkira/Napier`](https://github.com/AAkira/Napier).

---

### Playing in the Treehouse with Redwood and Zipline

> Jake Wharton (Cash App). [Recording](https://www.youtube.com/watch?v=G4LK_euTadU).

- "Reinventing browsers" for iOS and Android inside an app.
- Native widgets defined via Compose.
- Compose layouts distributed to the platforms from the backend.

---

### Modern Compose Architecture with Circuit

> Zac Sweers & Kieran Elliott (Slack). [Recording](https://www.youtube.com/watch?v=ZIr_uuN8FEw).

- The currently popular architecture is unidirectional flow via `StateFlow`.
- `@Composable` presenters expose state; Compose all the way down → UDF-first → multiplatform.
- [`slackhq/circuit`](https://github.com/slackhq/circuit): supports navigation; combines state and events into one model; easy testing via [`cashapp/turbine`](https://github.com/cashapp/turbine).
- `rememberRetained` survives config changes; `rememberSaveable` survives process death.

---

### Closing panel

> Moderated by Hadi Hariri (JetBrains). [Recording](https://www.youtube.com/watch?v=8XmzVTUkXoE).

- AppCode is being shut down — it lacked financial success.
- The [Kodee](https://blog.jetbrains.com/kotlin/2023/04/the-kotlin-mascot-returns) mascot was introduced.
- They struggled not to name Kotlin coroutines "Koroutines".

---

### Summary

- A lot of backend devs, some iOS devs.
- The majority already use KMM in production.
- Heavy focus on KMP (e.g. `Kotlin/Wasm`).
- Kotlin 2.0 is a big step.
- Google is on board with KMP.
