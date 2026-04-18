# Languages Vocabulary
# Covers: Go, Node.js, TypeScript, Rust — language-specific key concepts

---

# GO

## goroutine
Lightweight, independently executing function managed by the Go runtime.
Much cheaper than OS threads. Created with `go funcName()`.
Thousands can run concurrently in one program.
Related: [[channel]], [[WaitGroup]], [[concurrency]]

## channel
Typed conduit for goroutines to communicate safely and synchronise execution.
Types: unbuffered (synchronous), buffered (async up to capacity).
`ch <- value` to send, `value := <-ch` to receive.
Related: [[goroutine]], [[select-statement]]

## select-statement
Allows a goroutine to wait on multiple channel operations simultaneously.
Executes the first channel that becomes ready. Foundation of Go concurrency control.
Related: [[channel]], [[goroutine]]

## interface-go
Types satisfy interfaces implicitly by implementing all required methods.
No `implements` keyword. Empty interface `any` is satisfied by all types.
Enables polymorphism and testability.
Related: [[goroutine]]

## WaitGroup
Synchronisation primitive from `sync` package. Waits for a group of goroutines to finish.
`Add(n)` → `Done()` per goroutine → `Wait()` to block until all done.
Related: [[goroutine]], [[channel]]

## context-go
Carries deadlines, cancellation signals, and request-scoped values across goroutines.
Always pass as first argument to functions. Cancel to clean up resources.
Related: [[goroutine]], [[channel]]

## error-handling-go
Errors are values in Go. Functions return `(result, error)`.
Check `if err != nil` after every call. No exceptions.
Related: [[goroutine]]

## defer
Schedules a function call to run when the surrounding function returns.
Used for cleanup: close files, release locks, recover from panics.
Related: [[error-handling-go]]

---

# NODE.JS

## event-loop-node
Single-threaded, non-blocking I/O model. Handles many concurrent connections efficiently.
Phases: timers → I/O callbacks → poll → check (setImmediate) → close.
Related: [[async-await]], [[streams-node]]

## async-await
Syntactic sugar over Promises. `async fn` returns a Promise. `await` pauses until resolved.
Prefer over raw callbacks. Always handle rejections with try/catch or `.catch()`.
Related: [[event-loop-node]], [[promise]]

## promise
Represents an eventual value. States: pending → fulfilled | rejected.
Chain with `.then()` / `.catch()` / `.finally()`.
Related: [[async-await]]

## streams-node
Efficient handling of large data by processing chunks instead of buffering whole dataset.
Types: Readable, Writable, Duplex, Transform.
Related: [[event-loop-node]], [[back-pressure]]

## middleware-node
Functions that have access to request, response, and next() in Express-like frameworks.
Used for: logging, auth, validation, error handling.
Related: [[chain-of-responsibility]]

## cluster-module
Allows Node.js to fork multiple worker processes to use multi-core CPUs.
Each worker runs a copy of the app. The primary distributes connections.
Related: [[event-loop-node]]

---

# TYPESCRIPT

## generics-ts
Type variable that lets components work with various types while maintaining type safety.
`function identity<T>(arg: T): T`. Avoids duplication without `any`.
Related: [[utility-types-ts]], [[conditional-types-ts]]

## utility-types-ts
Built-in type transformations: Partial<T>, Required<T>, Readonly<T>, Pick<T,K>,
Omit<T,K>, Record<K,T>, Exclude<T,U>, Extract<T,U>, ReturnType<T>.
Related: [[generics-ts]], [[mapped-types-ts]]

## type-guard-ts
Narrows a variable's type at runtime using `typeof`, `instanceof`, `in`, or custom predicates.
`function isFish(pet: Fish | Bird): pet is Fish { ... }`
Related: [[generics-ts]]

## decorators-ts
Functions that wrap classes, methods, properties, or parameters.
Use for: dependency injection, logging, validation, metadata.
Requires `"experimentalDecorators": true` in tsconfig.
Related: [[generics-ts]]

## mapped-types-ts
Transform properties of an existing type: `{ [K in keyof T]: ... }`.
Foundation of utility types like `Partial` and `Readonly`.
Related: [[utility-types-ts]], [[generics-ts]]

## conditional-types-ts
Type-level if/else: `T extends U ? X : Y`.
Used with `infer` to extract type information from other types.
Related: [[generics-ts]], [[mapped-types-ts]]

## discriminated-union-ts
Union type where each member has a literal type field for narrowing.
`type Shape = { kind: 'circle'; radius: number } | { kind: 'rect'; width: number }`.
Related: [[type-guard-ts]]

## declaration-merging-ts
Interfaces can be declared multiple times and TypeScript merges them.
Useful for extending third-party library types.
Related: [[generics-ts]]

---

# RUST

## ownership-rust
Every value has exactly one owner. When owner goes out of scope, value is dropped.
No garbage collector. Memory safety guaranteed at compile time.
Related: [[borrowing-rust]], [[lifetimes-rust]]

## borrowing-rust
Access data without taking ownership via references (`&T` immutable, `&mut T` mutable).
Rule: either many immutable refs OR one mutable ref — never both simultaneously.
Prevents data races at compile time.
Related: [[ownership-rust]], [[lifetimes-rust]]

## lifetimes-rust
Annotations (`'a`) that tell the borrow checker how long references are valid.
Prevents dangling references. Often inferred; required when returning references.
Related: [[borrowing-rust]], [[ownership-rust]]

## traits-rust
Defines shared behaviour across types. Similar to interfaces. Enable polymorphism.
Trait bounds restrict generics: `fn print<T: Display>(item: T)`.
Related: [[ownership-rust]]

## smart-pointers-rust
`Box<T>`: heap allocation. `Rc<T>`: reference counting (single-threaded).
`Arc<T>`: atomic reference counting (multi-threaded). `RefCell<T>`: interior mutability.
Related: [[ownership-rust]], [[borrowing-rust]]

## async-rust
`async fn` returns a Future. `.await` suspends until Future is ready.
No built-in runtime — use Tokio or async-std crates.
Related: [[traits-rust]], [[ownership-rust]]

## error-handling-rust
`Result<T, E>` for recoverable errors. `Option<T>` for optional values.
`?` operator propagates errors up the call stack. No exceptions.
Related: [[ownership-rust]]

## pattern-matching-rust
`match` statement exhaustively handles all cases. Works on enums, structs, tuples.
Used with `Option`, `Result`, and discriminated types.
Related: [[error-handling-rust]]
