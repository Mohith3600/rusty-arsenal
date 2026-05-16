# 09 — Strings

## Exercise 1 — `strings1.rs`

### 📋 The Original Code

```
// TODO: Fix the compiler error without changing the function signature.
fn current_favorite_color() -> String {
    "blue"
}

fn main() {
    let answer = current_favorite_color();
    println!("My current favorite color is {answer}");
}

```

### ❌ Why It Needs Fixing
When you run the code the compiler throws an error. Compiler expects to return a String return type "blue" is &str

### ✅ Solution


```
fn current_favorite_color() -> String {
   "blue".to_string()
  //String::from("blue")
}

fn main() {
    let answer = current_favorite_color();
    println!("My current favorite color is {answer}");
}

```


### 🧠 Concepts Introduced
**&str(String slice)** - Think of string slice as a borrowed view from the memory. 
WHen you write ```"blue```, it is hardcoded directly into your compiled program's binary. It is fast, fixed in size and immutable. You are just borrowing a reference to that specific spot in the binary memory.

**String** - Think of it as owned, growable text buffer. 
It lives on the heap.Because it is on heap, you can modify it, append to it and pass ownership of it around your program. 

When you use String::from("blue"), Rust asks the computer for some heap memory, copies the word "blue" from the binary into that new memory space, and hands you the "ownership" of that new space.


## Exercise 2 — `strings2.rs`

### 📋 The Original Code

```
// TODO: Fix the compiler error in the `main` function without changing this function.
fn is_a_color_word(attempt: &str) -> bool {
    attempt == "green" || attempt == "blue" || attempt == "red"
}

fn main() {
    let word = String::from("green"); // Don't change this line.

    if is_a_color_word(word) {
        println!("That is a color word I know!");
    } else {
        println!("That is not a color word I know.");
    }
}
```

### ❌ Why It Needs Fixing
Compiler expects ```&word``` and found String so program will not run.

### ✅ Solution


```
fn is_a_color_word(attempt: &str) -> bool {
    attempt == "green" || attempt == "blue" || attempt == "red"
}

fn main() {
    let word = String::from("green");

    if is_a_color_word(&word) {
        //             ^ added to have `&String` which is automatically
        //               coerced to `&str` by the compiler.
        println!("That is a color word I know!");
    } else {
        println!("That is not a color word I know.");
    }
}

```

### 🧠 Concepts Introduced
**Deref Corecion**  
When you call is_a_color_word(&word), the type you are passing is &String.
The function expects &str.
Rust automatically performs Deref Coercion to convert your reference to a String (&String) into a string slice (&str). It knows how to cleanly slice the whole string to make the types match

## Exercise 3 — `strings3.rs`

### 📋 The Original Code

```
fn trim_me(input: &str) -> &str {
    // TODO: Remove whitespace from both ends of a string.
}

fn compose_me(input: &str) -> String {
    // TODO: Add " world!" to the string! There are multiple ways to do this.
}

fn replace_me(input: &str) -> String {
    // TODO: Replace "cars" in the string with "balloons".
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn trim_a_string() {
        assert_eq!(trim_me("Hello!     "), "Hello!");
        assert_eq!(trim_me("  What's up!"), "What's up!");
        assert_eq!(trim_me("   Hola!  "), "Hola!");
        assert_eq!(trim_me("Hi!"), "Hi!");
    }

    #[test]
    fn compose_a_string() {
        assert_eq!(compose_me("Hello"), "Hello world!");
        assert_eq!(compose_me("Goodbye"), "Goodbye world!");
    }

    #[test]
    fn replace_a_string() {
        assert_eq!(
            replace_me("I think cars are cool"),
            "I think balloons are cool",
        );
        assert_eq!(
            replace_me("I love to look at cars"),
            "I love to look at balloons",
        );
    }
}

```

### ❌ Why It Needs Fixing
Multiple compiler errors

### ✅ Solution

```
fn trim_me(input: &str) -> &str {
    input.trim()
}

fn compose_me(input: &str) -> String {
    // The macro `format!` has the same syntax as `println!`, but it returns a
    // string instead of printing it to the terminal.
    // Equivalent to `input.to_string() + " world!"`
    format!("{input} world!")
}

fn replace_me(input: &str) -> String {
    input.replace("cars", "balloons")
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn trim_a_string() {
        assert_eq!(trim_me("Hello!     "), "Hello!");
        assert_eq!(trim_me("  What's up!"), "What's up!");
        assert_eq!(trim_me("   Hola!  "), "Hola!");
        assert_eq!(trim_me("Hi!"), "Hi!");
    }

    #[test]
    fn compose_a_string() {
        assert_eq!(compose_me("Hello"), "Hello world!");
        assert_eq!(compose_me("Goodbye"), "Goodbye world!");
    }

    #[test]
    fn replace_a_string() {
        assert_eq!(
            replace_me("I think cars are cool"),
            "I think balloons are cool",
        );
        assert_eq!(
            replace_me("I love to look at cars"),
            "I love to look at balloons",
        );
    }
}

```



### 🧠 Concepts Introduced
**trim_me** 
Trimming a string does not require creating any new text or allocating new heap memory. Rust simply looks at the original borrowed string and returns a smaller borrowed view that ignores the spaces on the ends. It's incredibly fast and efficient because no data is copied.

**compose_me**
Why it returns an owned String: You are adding brand-new data (" world!") to the existing text. The original string slice cannot grow because it is fixed in size. Therefore, you must allocate new memory on the heap to hold the combined text, which means returning a brand new String. We used format! here because it cleanly allocates that new String for you.

**replace_me**
Similar to compose_me, when you replace "cars" (4 bytes) with "balloons" (8 bytes), the resulting text is physically larger than the original text. You cannot squeeze 8 bytes into the 4 bytes of memory that the original slice was looking at. Because the size changes, Rust must build a brand new heap-allocated String to hold the result.


## Exercise 4 — `strings4.rs`

### 📋 The Original Code

```
// Calls of this function should be replaced with calls of `string_slice` or `string`.
fn placeholder() {}

fn string_slice(arg: &str) {
    println!("{arg}");
}

fn string(arg: String) {
    println!("{arg}");
}

// TODO: Here are a bunch of values - some are `String`, some are `&str`.
// Your task is to replace `placeholder(…)` with either `string_slice(…)`
// or `string(…)` depending on what you think each value is.
fn main() {
    placeholder("blue");

    placeholder("red".to_string());

    placeholder(String::from("hi"));

    placeholder("rust is fun!".to_owned());

    placeholder("nice weather".into());

    placeholder(format!("Interpolation {}", "Station"));

    // WARNING: This is byte indexing, not character indexing.
    // Character indexing can be done using `s.chars().nth(INDEX)`.
    placeholder(&String::from("abc")[0..1]);

    placeholder("  hello there ".trim());

    placeholder("Happy Monday!".replace("Mon", "Tues"));

    placeholder("mY sHiFt KeY iS sTiCkY".to_lowercase());
}
```

### ❌ Why It Needs Fixing
Multiple compile errors

### ✅ Solution


```
fn string_slice(arg: &str) {
    println!("{arg}");
}

fn string(arg: String) {
    println!("{arg}");
}

fn main() {
    // 1. String literal
    string_slice("blue");

    // 2. Converted to String
    string("red".to_string());

    // 3. Explicit String creation
    string(String::from("hi"));

    // 4. Owned copy of a slice
    string("rust is fun!".to_owned());

    // 5. Converted into a String via the Into trait
    string("nice weather".into());

    // 6. format! macro creates a new String
    string(format!("Interpolation {}", "Station"));

    // 7. Slicing a String
    string_slice(&String::from("abc")[0..1]);

    // 8. trim() returns a smaller view (slice)
    string_slice("  hello there ".trim());

    // 9. replace() allocates new memory
    string("Happy Monday!".replace("Mon", "Tues"));

    // 10. to_lowercase() allocates new memory
    string("mY sHiFt KeY iS sTiCkY".to_lowercase());
}

```


### 🧠 Concepts Introduced

**The ToString and ToOwned Traits**
Code: "red".to_string() and "rust is fun!".to_owned()

Concept: In Rust, traits are like shared behaviors that different types can agree to use.

The ToString trait allows any type that can be formatted as text to turn itself into a String.

The ToOwned trait is slightly more general; it allows any borrowed data (like a slice) to be cloned into owned data on the heap. For strings, they do exactly the same thing.

**The Into Trait and Type Inference**
Code: "nice weather".into()

Concept: This is one of Rust's most powerful features. The Into trait is used for automatic type conversions. Notice how .into() doesn't actually say what it is converting into.

Because you passed it to string(arg: String), the Rust compiler looks at the function signature, sees it needs a String, and says, "Ah, I will automatically use the &str -> String conversion implementation." It figures it out from the context!

**String Slicing (Byte Ranges)**
Code: &String::from("abc")[0..1]

Concept: You can borrow a specific chunk of a string using range syntax [start..end].

The Danger: As the warning in the code mentioned, this operates on raw bytes, not characters. If you slice a multi-byte character (like an emoji or an accented letter) right down the middle, your Rust program will literally panic and crash. Slicing with [..] is powerful but must be used carefully.

**The "Cost" of String Methods**
This exercise taught you to look at a method and intuitively guess if it costs memory (Heap Allocation) or is "free" (Pointer adjustment).

Zero-Cost Views (trim): Because spaces exist at the ends, Rust can just give you a smaller slice (&str) that starts after the leading spaces and ends before the trailing spaces. It doesn't move or copy any underlying data.

Allocating Mutations (replace, to_lowercase): String slices (&str) are immutable. You cannot change uppercase to lowercase inside a &str. Therefore, methods that alter the text must ask the OS for new memory, copy the altered text into it, and return a brand new, owned String.

** Macros vs. Functions**
Code: format!(...)

Concept: You used a macro (denoted by the !). Unlike normal functions which take fixed types, macros in Rust actually generate new Rust code behind the scenes before your program compiles. format! takes all your references, calculates exactly how much memory it needs, asks for that memory once, and builds a brand new String for you.

