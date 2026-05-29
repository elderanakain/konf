---
layout: default
title: KotlinConf 2025
nav_order: 2
---

# KotlinConf 2025

![KotlinConf 2025 opening keynote — Kirill Skrygan]({{ "/assets/kotlinconf-2025.jpg" | relative_url }})

Copenhagen, 21-23 May 2025.

---

### Opening Keynote

> JetBrains Kotlin team (Sebastian Aigner, Márton Braun, Hadi Hariri, Svetlana Isakova, Kirill Skrygan, Ekaterina Petrova, Vsevolod Tolstopyatov, Michail Zarečenskij). [Recording](https://www.youtube.com/watch?v=F5NaqGF9oT4).

- **Junie**, JetBrains' AI assistant for Kotlin, can be used as a tool for GitHub issues.
- Name-based destructuring (`val (param1, ...)`) is targeted for Kotlin 2.4.
- **Rich errors**: a union-type-style return shape using a new `error` classifier (`object` or `class`); targeted for Kotlin 2.4.
- Must-use return values via annotations, landing in Kotlin 2.3.
- Power-assert will become part of the Kotlin language.
- All built-in Kotlin tools to form an ecosystem with automatic version alignment, multi-version releases (documentation, abi-validation, kover, ...).
- New KMP plugin for IntelliJ-based IDEs, with a KMP wizard and Compose Multiplatform common Previews.
- **Swift Export** lands as experimental in Kotlin 2.2 with only core language features supported initially.
- Compose for iOS is stable, with compatibility guarantees, native navigation, native scrolling, ~10 MB of additional binary size, and Hot Reload for previews.
- **Kotlin/JS** is getting better interop and support for the latest JS standards.
- A DataFrame compiler plugin.
- A Kotlin compiler server (e.g. for VS Code support).

---

### Code Quality at Scale: Future-Proof Your Android Codebase with KtLint and Detekt

> Tristan Hamilton (JetBrains). [Recording](https://www.youtube.com/watch?v=WTWW7OOHQIw).

- ktlint, detekt, and kover are the core toolchain.
- R8 with `kotlinx.serialization` removes the `@Keep` problem that affects Gson.
- Detekt can be extended to help with R8 keep rules for network models.
- VM-specific rules.

---

### Rich Errors in Kotlin

> Michail Zarečenskij (Lead Language Designer, JetBrains). [Recording](https://www.youtube.com/watch?v=IUrA3mDSWZQ).

- Union types like `` `User | FetchError` ``.
- Chained calls are supported; errors accumulate.
- Checked exceptions in Java (`throws X`): can't be skipped, only re-thrown; no support for higher-order functions; you end up with try/catch everywhere, but at least no silent failures.
- Unchecked exceptions in Kotlin: no organic support for errors.
- `null` as a solution: supports chain calls and smart casts.
- Sealed hierarchies as a solution: multiple clear errors, but force nesting instead of chain calls.
- A `Result`-like class? Produces a non-exhaustive error structure.
- There's a huge gap between returning a `null` and a proper error.
- Rich errors: a new `error` classifier; `` `fun getUser(): User | NetworkError | ParsingError` ``; multiple errors can be joined in one return type.
- Return-type checks with type-casting support give type safety.
- Exhaustive sealed structure that includes all errors.
- Main actor + error attributes.
- Errors vs exceptions: unrecoverable cases stay with exceptions.
- An `error` can be either an `object` or a `class`; it must be final; no generics allowed.
- Use `typealias` to bundle error types.
- An `error` is not `Any` — it inherits from `Error`.
- `` `var last: T | NotFound = NotFound` ``.
- Chain calls (`a?.b?.c`): which step is returning `null`?
- `!!` will throw the error as an exception; it's an overloadable operator.
- No `?:` operator for rich errors.

---

### From 0 to h-AI-ro: high-speed track to AI for Kotlin developers

> Urs Peter. Recording not yet found.

- **Koog** is JetBrains' framework for building LLM-based applications and agents in Kotlin.
- Temperature of 0 produces more factual outputs but doesn't prevent hallucinations.
- Spring AI can produce a structured response model (e.g. a `Dish` model).
- LLM fine-tuning: re-training on a specific dataset; costly.
- **RAG** (Retrieval-Augmented Generation) retrieves similar data from a database and augments the prompt with it.

---

### Compose Multiplatform for iOS: Ready for Production Use

> Sebastian Aigner (JetBrains). [Recording](https://www.youtube.com/watch?v=YKTlW8Qkj0w).

- Stable and production-ready for mobile.
- Embeddable into SwiftUI.
- API compatibility guarantees.
- Feels native — scrolling with overscroll, navigation, text input with magnifying glass.
- Drag-and-drop support, using native clipboard logic; compatible with the system.
- Looks good at 60 Hz and 120 Hz.
- Experimental parallel rendering thread to offload calculations from the main thread.
- Out-of-the-box accessibility.
- New KMP plugin.
- `Res` plays the role of Android's `R` for resources.
- Compose is exposed as a `ViewController` to iOS.
- Hot Reload preserves the current state; works well with AI agents; JVM only; good for a UI components library — an interactive playground in a separate module.

---

### Blueprints for Scale: What AWS Learned Building a Massive Multiplatform Project

> Matas Lauzadis & Ian Botsford (AWS). Recording not yet found.

- AWS KMP SDK at scale: 4 repos, 500+ modules, daily releases.
- HTTP engine setup: a common interface that more specific config classes inherit.
- Always wrap third-party APIs so they're not exposed in your public API — prevents leakage and shields callers from upstream changes.
- Consider code generation: KSP, KotlinPoet, Smithy, for your own library solution.
- `smithy-kotlin` enables reusability of white-label SDKs.
- `aws-kotlin-repo-tools` provides lint rules, GitHub Actions, and Gradle scripts.
- Start multiplatform if possible, so you're aware of each target's limitations.
- Prefer `kotlin.*` packages over `java.*`.
- Builders are better than data classes for backwards-compatibility (e.g. a config class) — DSL syntax is preferred over the direct builder API.
- Avoid `data class` in public APIs: adding new parameters breaks calling code; use a builder with defaults instead.
- Artifact-size check in CI/CD compares the current release against the latest stable.
- Use GitHub labels to acknowledge known CI issues.
- Use binary-compatibility checks to verify API stability.

---

### Multiplatform Settings: A case study in Multiplatform library development

> Russell Wolf. [Recording](https://www.youtube.com/watch?v=vMNCAryfJys).

- Key-value storage.
- Evolved from `expect`/`actual` to an interface/class structure, then to a builder for creating settings.
- Read-write property delegate.
- Operator overloading.

---

### Making native SDKs Multiplatform at RevenueCat

> Joop Korteweg (RevenueCat). [Recording](https://www.youtube.com/watch?v=5Sc3Qdb0XoQ).

- SDK for subscriptions and in-app purchases.
- Wraps Apple and Google libraries.
- Migrate the network and persistence layer first — usually the same business models across mobile platforms.
- Use `typealias` for minimal runtime overhead as a first step: `expect class` → `actual typealias`. The KMP layer can vanish at runtime but it exposes the platform API. They ended up with wrapper classes.
- A native transitive dependency can be added as a static library to KMP.
- Another way: a cinterop definition file in `src/nativeInterop` with `compilations { cinterops { } }`; run `xcrun swift build` to build the static library locally.
- Another way: the `swift-klib-plugin` adds a static library to `klib/included`.
- `kmp-tor-binary` uses `resource-cli` for resources in libraries.
- Another way: ship as a Gradle plugin (e.g. `sentry-kotlin-multiplatform`).
- Project: `RevenueCat/purchases-kmp`.

---

### Dependencies and Kotlin/Native

> Tadeas Kriz. [Recording](https://www.youtube.com/watch?v=n0LpCCv3VEY).

- A native dependency can be a framework or a source package.
- Header files are part of the package.
- Dynamic libraries: cachable at build time (they're immutable), cachable on the client (no re-deploy needed), share memory space, but have slower start-up.
- A static `.a` library is part of the executable; a `.dylib` is shipped alongside it.
- A better SPM approach for KMP is to make the KMP library static.
- Don't depend on static libraries from multiple dynamic libraries.
- Static linking is your friend.

---

### Duolingo + KMP: A Case Study in Developer Productivity

> John Rodriguez & Johnny Ye (Duolingo). [Recording](https://www.youtube.com/watch?v=RJtiFt5pbfs).

- Statically link into a single XCFramework via an umbrella project.
- Calculate coordinates on the KMP layer and pass bounding boxes and points down to the native side.
- Kotlin/JS target.
- Rive animations library.
- KMP-first strategy; moving toward Compose Multiplatform.

---

### Don't forget your values!

> Leonid Startsev (Kotlin Libraries, JetBrains). [Slides](https://resources.jetbrains.com/storage/products/kotlinconf-2025/may-23/Don%E2%80%99t%20forget%20your%20values!%20_%20Leonid%20Startsev.pdf). Recording not yet found.

- We need a way to mark return values as "should use".
- Most function results should be used (~85% per JetBrains and Google data).
- The goal is to report every useful value that is returned.
- A possible `@IgnoreableReturnValue` in Kotlin, similar to Swift's `@discardableResult`.

---

### Swift Export — a peek under the hood

> Artem Olkov (JetBrains). [Recording](https://www.youtube.com/watch?v=KEsVNrzPf24).

- First experimental release in Kotlin 2.2.20.
- Multi-module support via separate Swift modules (no name clashes).
- Supports exporting libraries transitively.
- Flatten packages into enum structures: extensions on the last package entry plus `typealias` to call directly.
- `swift-export-sample` repo.
- Objective-C interop is not deprecated.
- Swift calls Kotlin/Native code under the hood — binary compatibility is preserved.
- Pipeline: Swift Intermediate Representation → Kotlin bridges → C headers → Swift wrappers → Swift call sites.
- Direct coroutines interoperability.

---

### Fast inner dev loops for Kotlin Gradle builds

> Alex Semin & Rodrigo Oliveira (Gradle). [Recording](https://www.youtube.com/watch?v=eryPIdJjBgk).

- Three phases: init (`settings.gradle.kts`), configuration (`build.gradle.kts`), execution.
- Configuration cache produces an HTML report on issues.
- `org.gradle.configuration-cache.parallel=true` is experimental.
- Try `./gradlew --configuration-cache` first, then enable `org.gradle.configuration-cache=true` permanently.
- Providers are the Gradle equivalent of "lazy".
- Track progress with Develocity (compatible with Grafana).
- Isolated projects (configuration cache enabled by default).

---

### 47 Refactorings in 45 minutes

> Duncan McGregor & Dmitry Kandalov. Recording not yet found.

*No notes captured.*
