# KotlinConf 2026 — Session Notes

Munich, 20–22 May 2026 · 100 speakers · 108 sessions · ~2,200 attendees.

---

## Day 1 — 21 May

### Opening Keynote

> Officially recapped in [KotlinConf'26 Keynote Highlights](https://blog.jetbrains.com/kotlin/2026/05/kotlinconf26-keynote-highlights/) — "Advances in Language Design, Tooling, AI-Driven Workflows, and Multiplatform Development."

**Kotlin 2.4**
- Context parameters are now stable.
- Explicit backing fields are now stable.
- Multi-field value classes: name-based destructuring; no identity (defined by fields); no actual object creation.
- Still experimental in 2.4 (per the official recap): auto-generated `equals`/`hashCode`/`toString` for value classes, collection-literal updates, locality as a first-class language concept, "rich errors" for recoverable failures.

**Ecosystem & tooling**
- Kotlin CLI consolidated under the **Kotlin Toolchain** (`kotlintoolchain`) — a unified entry point for build, run, test, format, docs, and agent integration. Amper is the core component.
- **Kotlin Docs Model (KDM)** — machine-readable documentation shipped as `kdoc.jar` alongside libraries.
- **Kotlin Language Server** promoted to Alpha, "with the full power of the IntelliJ engine"; official Kotlin extension on the VS Code Marketplace.
- `ktfmt` is moving into the Kotlin org (a JetBrains Foundation / Meta collaboration). Bazel `rules_kotlin` is getting first-class support too.

**Kotlin at Google**
- R8 rewrote coroutine locks, giving up to 50% on composed performance benchmarks.
- Kotlin is used on the backend at Google.
- Google Docs is 100% KMP on web and iOS.
- Android is Compose-first; Views are in maintenance mode; migration is aided by AI skills.
- The Android CLI is built for both humans and agents.
- Other keynote stats: 92% of professional Android developers use Kotlin; KSP cuts ~17% from complex builds; K2 has near-universal adoption in Android Studio.

**AI tooling**
- **Agent Client Protocol (ACP)** — an open standard, co-led by JetBrains, for IDE-to-coding-agent communication.
- **JetBrains Air** — a multi-agent dev environment running OpenAI Codex, Claude Agent, Gemini CLI, and Junie in parallel.
- **Kotlin SWE-bench** introduced — 110 real engineering tasks; Claude Code with Opus 4.7 achieved an 86.4% resolution rate.

**Anthropic**
- The Anthropic Java SDK is written in Kotlin.
- The official MCP Kotlin SDK comes from JetBrains.
- Claude is a native integration in IntelliJ IDEA / Android Studio, a first-party model in Junie and Air; the Claude Code plugin uses the Kotlin LSP.

**Ktor & backend**
- Ktor integrates with Koog (including local models).
- First-party gRPC support (via `kotlinx-rpc`), with auto-generated sources for all platforms.
- **Exposed 1.0** is stable, with vector types for similarity search and a Gradle plugin for migration scripts.
- `kotlin-stdlib` now has an 18-month security support policy, with backports across active release lines.

**KMP**
- Perk and Alipay are using KMP. The keynote also named PayPal, Booking.com, Sony, and Duolingo as production users. Top-app KMP usage more than doubled year over year.
- Swift Export now covers `suspend` and `Flow`, moving to Alpha. SPM support in the KMP Gradle plugin (lets you depend on Objective-C-compatible code via Swift Package Manager). New interop APIs enable native Liquid Glass components with Compose UI.
- Kotlin/Native: build times are 25% faster while using less than half the RAM compared to last year.
- Kotlin and TypeScript interop: `suspend` functions map to `Promise`.
- Compose Multiplatform is stable on mobile and desktop; Web reached Beta (September 2025). Navigation 3 is stable and Compose-first. `klibs.io` now lists 3,500+ community libraries.

---

### Keeping Tabs on Your Data: AndroidX Storage for the Web

- Not `localStorage`. Not IndexedDB.
- **OPFS is the choice**: file-system storage, byte-level access, async (or sync inside a Web Worker).
  - *OPFS = Origin Private File System*, a W3C File System API for per-origin sandboxed storage. Sync handles are only available from a dedicated Worker.
- `kotlinx.browser` is used for direct interoperability.
- IndexedDB access via external dependencies using `js("")` leads to callback hell.
- OPFS has no Kotlin API yet but is promise-based.
- **DataStore on Web**: reactivity comes via `StorageEvent`; OPFS lacks it, solved with a custom solution.
  - *DataStore* is the AndroidX persistence library; Android/iOS KMP support exists, web is newer.
- **Room 3.0** for web: everything sits in `commonMain`.
- **Web Worker** — isolated, with no shared memory; communication happens via messages.
- A stable release is coming soon.

---

### Metro Under the Hood

> **Speaker:** Zac Sweers.
>
> **Abstract:** "Metro is both a multiplatform DI framework and a sophisticated Kotlin compiler plugin. This advanced session breaks down how Metro works inside the compiler, what code it generates, and how its 'magic' actually happens."

- Handles private declarations.
- Generated code is hidden from Objective-C.
- `lazy` is allowed in cyclic dependencies.
- Dynamic graphs for swapping dependencies in tests.

---

### How google.com/search Builds on Kotlin Coroutines for Highly Scalable, Streaming, Concurrent Servers

> **Speakers:** Sam Berlin and Alessio Della Motta, Senior Staff Software Engineers, Search Infra, Google.
>
> **Abstract:** "Discover how Google Search uses server-side Kotlin and coroutines to enable low-latency, highly asynchronous streaming code paths at massive scale." Introduces *Qflow* — a data-graph interface language connecting async definitions with Kotlin business logic — and coroutine instrumentation for latency tracking and critical-path analysis.

- Scale is a problem; legacy code is anchored in C++ and Java.
- The JVM was chosen for its reliability and memory model.
- A global lock is used with a custom executor.
- Debuggability is hard — call chains can be lost.
- Cancellation: a `CancellationException` can be caught and swallowed; there is no way to know which cancellation happened.
- The Flow API is extension-based — you can't keep a custom `Flow` type after another operator is applied.
- Some APIs were banned for ambiguity (e.g. `merge`, `toList()`).
- A new language to solve this: **Qflow**, with multiple backends (Kotlin, C++ code generation).
- Tracing via **Perfetto** (the Android / Chromium production-grade tracing system).

---

### Kotlin/JS: Past, Present and Future

> Presented by the JetBrains team; individual speaker not confirmed in public sources. Touchlab summary: "goes over the history of KotlinJs while also talking about what is next."

- ES5-only support; bundle size is not great.
- Deliverable to native code as TypeScript.
- Publishable via npm (community solution) or via Maven publish.
- Two-way interoperability via export. Usable with React.
- Recommendation: **Kotlin/Wasm with Compose Multiplatform**; share web source between Wasm-capable and older JS browsers for compatibility.
- Community frameworks: Kilua, Kobweb, Summon.
- Future plans: stable, unlimited TypeScript export (no `Flow` today); better bundle size (JS that looks hand-written for better on-site optimisation, SWC).
- Dropping webpack for Vite (and you can choose your bundler); Playwright for tests.
- Roadmap horizon is ~1 year out.
- Further out: better SSR hydration.

---

### Expedited Shipping: Accelerating iOS Development with KMP at Amazon

> Touchlab summary: covers **App Platform**, a library by Amazon for state management, dependency injection, and decoupling logic layers; includes a deep dive into iOS integration challenges and architectural decisions. Library currently at v0.0.8.

- Used in internal delivery products.
- Shares business logic, data, and state.
- Architecture: **App Platform** (open-source on GitHub).
- Bridging to iOS: **Molecule** for UDF presenters; a custom converter from `Flow` to throwing `Stream`.
  - *Molecule* (Cash App / Jake Wharton) builds Compose-driven presenters that emit `StateFlow`/`Flow`. UDF = Unidirectional Data Flow.
- Model-based business logic with UI wiring.
- Navigation is treated as business logic — and is shared.
- View integration is shared.

---

### Talking to Terminals (and How They Talk Back)

> **Speaker:** Jake Wharton (Android Developer, Skylight).
>
> **Abstract:** "Modern terminals can do far more than print text. In this deep dive, Jake explores how command-line apps communicate with terminals — from colors and sizing to advanced features like frame sync, images, and keyboard events. Using Kotlin, he covers OS-specific APIs, JVM vs. Kotlin/Native challenges, and reusable libraries that help you unlock the full power of the terminal."

- Communication is bi-directional.
- stdin / stdout / stderr flow between the terminal and the executable.
- ANSI control sequences are used.
- The `/dev/tty` device gives access to the enclosing terminal to capture output.
- **Mosaic** (Jake Wharton) — Compose for CLI; React-style declarative UI for the terminal.
- It can track the mouse position.

---

### The Lord of Collection Functions — The Fellowship of Kotlin

- `forEach` is evil.

---

## Day 2 — 22 May

### Advanced Kotlin Native Integration

> **Speaker:** Tadeas Kriz (Senior Kotlin Developer, Touchlab).
>
> **Abstract:** "Kotlin Multiplatform native builds come with a key constraint: one native binary per project. This session explores what happens when multiple binaries enter the picture, the architectural impact on large systems, and strategies for splitting compilation into manageable parts. It's a practical look at scaling Kotlin/Native in complex, multi-repository environments."

- Avoid multiple KMP frameworks: larger binary size; no static linking (stdlib conflicts on link); incompatible types.
- Higher memory and CPU usage — additional GC threads; possible thread deadlocks.
- Single-framework downsides: a single namespace, a single umbrella, and higher build times (limits of incremental compilation).
- Workaround: use multiple frameworks via dummy frameworks; build a framework in each module.
- **Kotlin Package Manager** — similar to Swift Package Manager; a new tool by Touchlab.

---

### How I Learned to Stop Worrying and Love Value Semantics (in Kotlin)

- Value semantics: only the data of a model matters.
- Immutable collections are used.
- Immutability is automatically enforced via value classes.
- Use `copy var` to explicitly mark value-semantics copying.
- Implicit value semantics are optional.
- Do more copies mean worse performance? The JVM is good at scalarization. **Project Valhalla** brings fewer allocations, and the JIT erases allocations; Java 26/27 introduces heap flattening. Performance is worse for generic containers.
  - *Project Valhalla* is the OpenJDK project introducing value classes and primitive-class semantics; heap flattening is one of its scheduled deliverables.
- On other platforms, there are custom optimizations.
- Coming in Kotlin 2.5.

---

### Sony's KMP Journey: Scaling BLE & Hardware with Kotlin Multiplatform

> **Speaker:** Sergio Carrilho (TechLead, Sony).
>
> **Abstract:** "Go behind the scenes of Sony's six-year journey from an early, risky experiment with Kotlin Multiplatform to the global success of the Sony | Sound Connect app. From high-speed BLE and background execution to migrating from React Native to Compose Multiplatform, this talk explores technical trade-offs, stakeholder skepticism, and hard-earned architectural lessons."

- KMP first, then Compose Multiplatform.

---

### What Nobody Told Us About Shipping Kotlin to iOS

- The Sentry SDK is built with KMP.
- Kotlin 2.4.0 — Swift import.

---

### How Kotlin Powers Functional Design: MCP Edition

- Composable behaviours.
- MCP is bi-directional.

---

### Spec-Driven Development with AI Agents: From High-Level Requirements to Working Software

> **Speaker:** Anton Arhipov ([slides](https://speakerdeck.com/antonarhipov/spec-driven-development-with-ai-agents-from-high-level-requirements-to-working-software-e397ad2e-3484-4bfa-a80b-6c47c593ed11) · [video](https://www.youtube.com/watch?v=cTJorhnxrFI)).
>
> Premise: "AI coding agents are powerful, but they often feel unpredictable." Talk presents a structured spec-driven workflow to make agent output predictable.

- Distill requirements, then create a plan, break it into tasks, and let the AI execute.
- The context window matters — "harness engineering."
- Develop intuition for SDD.
- Use a task list to track progress.
- The SDD process: spec, then criteria, then rules, then review, then plan.
- Save decision traces, so you can ask "why" later.
- Criteria define how to validate the result, using **EARS**.
  - *EARS = Easy Approach to Requirements Syntax* — a controlled-language template ("When …, the system shall …") for unambiguous, testable requirements.
- `grill-me` (interactive plan-grilling).
- Use rules to capture technical constraints; link each rule to a reason.
- Open question: what to do with specs afterwards — they could be synced with code via an agent.
