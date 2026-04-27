# 00 — Intro

> **Section goal:** Get comfortable with how Rustlings works, what the Rust compiler
> feels like, and the most fundamental building block — the `println!` macro.
> No ownership, no types, no borrow checker yet. Just you, the terminal, and your first Rust output.

---

## Exercise 1 — `intro1.rs`

### 📋 The Original Code

```rust
fn main() {
    println!(r#"      Welcome to...                      "#);
    println!(r#"                 _   _ _                  "#);
    println!(r#"   _ __ _   _ __| |_| (_)_ __   __ _ ___ "#);
    println!(r#"  | '__| | | / _` | __| | | '_ \ / _` / __|"#);
    println!(r#"  | |  | |_| \__ \ |_| | | | | | (_| \__ \"#);
    println!(r#"  |_|   \__,_|___/\__|_|_|_| |_|\__, |___/"#);
    println!(r#"                                 |___/     "#);
    println!();
    println!("This exercise compiles successfully. The remaining exercises contain a compiler");
    println!("or logic error. The central concept behind Rustlings is to fix these errors and");
    println!("solve the exercises. Good luck!");
}
```

### 🎯 What the Exercise Asks

This one **already compiles**. Nothing is broken. The goal is simply to:

1. Understand how Rustlings works (edit file → compiler runs → progress advances)
2. Explore `println!` by adding your own lines
3. Remove the `// I AM NOT DONE` comment when you're ready to move on

### ✅ Solution

No code change required. Just delete the marker comment and move on. But before you
do — try adding a line like this and watch the output change live:

```rust
println!("My name is [your name]");
```

That's the whole point of intro1: prove to yourself that the edit-compile-run loop works.

### 🧠 Concepts Introduced

**`fn main()`** — Every Rust executable starts here. It's the entry point, identical
in purpose to `main()` in C or Go's `func main()`. 

**`println!`** — The `!` means this is a **macro**, not a regular function. Macros in
Rust are code that writes code at compile time. `println!` expands into code that
formats a string and writes it to stdout with a newline. You'll use its sibling
`eprintln!` to write to stderr — useful for logging errors without polluting structured
output in a sensor.

**Raw string literals `r#"..."#`** — The `r#` prefix means the string is taken
completely literally — no escape sequences are processed. This is why the ASCII art
backslashes don't need to be doubled. In security tooling, raw strings are extremely
useful for embedding regex patterns, YARA rules, or Sigma rule fragments directly in
source code without a forest of `\\`.

```rust
// Normal string — backslash must be escaped
let path = "C:\\Windows\\System32";

// Raw string — no escaping needed
let path = r"C:\Windows\System32";

// Raw string with # delimiters (used when the string itself contains quotes)
let rule = r#"rule suspicious { strings: $a = "malware" condition: $a }"#;
```

**`println!()` with no arguments** — Prints an empty line. Equivalent to `print!("\n")`.

### 🔗 Connects To

- Rust Book Ch. 1.2 — *Hello, World!*
- Next: `intro2.rs` — your first actual fix

---

## Exercise 2 — `intro2.rs`

### 📋 The Original Code

```rust
fn main() {
    // TODO: Fix the code to print "Hello world!".
    println!("Hello {}!", "world");
}
```

### ❌ Why It Needs Fixing

The original broken version has a format placeholder `{}` but the argument passed
does not satisfy what the exercise expects — the output must be exactly `Hello world!`
using the simplest possible form.

### ✅ Solution

```rust
fn main() {
    println!("Hello world!");
}
```

### 🧠 Concepts Introduced

**`println!` format strings** — The `{}` is a placeholder — a slot where a value
gets inserted at runtime.

```rust
// {} — default Display formatting
println!("PID: {}", 1234);
// Output: PID: 1234

// {:#?} — pretty-printed Debug formatting (great for structs during development)
println!("{:#?}", my_struct);

// Multiple placeholders
println!("Process: {} | PID: {} | Parent: {}", name, pid, ppid);

// Padding and alignment — useful for tabular CLI output in your EDR console
println!("{:<20} {:>8} {:>8}", "PROCESS", "PID", "PPID");
println!("{:<20} {:>8} {:>8}", "lsass.exe", 640, 512);
```

**`format!`** — Same syntax as `println!` but returns a `String` instead of printing.
This is what you'll use to build log messages inside your EDR pipeline:

```rust
let log_entry = format!("[ALERT] Process {} (PID {}) created", name, pid);
```

### 🔗 Connects To

- Rust Book Ch. 1.2 — *Hello, World!*
- Rust Docs — [std::fmt formatting syntax](https://doc.rust-lang.org/std/fmt/)
- Next section: `01_variables` — where the real learning begins

---

## 📦 Repo Files Reference

| File | Purpose |
|---|---|
| `book.toml` | mdBook configuration |
| `src/SUMMARY.md` | Table of contents for the site |
| `src/README.md` | Home page |
| `src/rustlings/XX/README.md` | Each exercise section |
| `.github/workflows/deploy.yml` | Auto-deploy to GitHub Pages |

---

*Next up → `01_variables` — where you'll meet `let`, mutability, shadowing,
and Rust's type inference for the first time.*
