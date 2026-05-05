# 06 - Move-Semantics

> **Section goal:** - Covers the concepts of Ownership, Reference and Borrowing.


## Exercise 1 — `move_semantics1.rs`


### 📋 The Original Code

```
// TODO: Fix the compiler error in this function.
fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let vec = vec;

    vec.push(88);

    vec
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn move_semantics1() {
        let vec0 = vec![22, 44, 66];
        let vec1 = fill_vec(vec0);
        assert_eq!(vec1, vec![22, 44, 66, 88]);
    }
}

```

### ❌ Why It Needs Fixing
The compiler will throw an error saying cannot borrow as mutable. 

### ✅ Solution

```
fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;
    vec.push(88);
    vec
}

fn main() {
}
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn move_semantics1() {
        let vec0 = vec![22, 44, 66];
        let vec1 = fill_vec(vec0);
        assert_eq!(vec1, vec![22, 44, 66, 88]);
    }
}

```

### 🧠 Concepts Introduced
**Move Semantics** - When you ```fill_vec(vec0)``` in the teest, ```vec0``` is moved into the function. It is not copied. The function ```fill_vec``` takes complete ownership of that data in memory. If you tried using ```vec0``` again after calling ```fill_vec(vec0)```, the compiler would stop you because ```vec0``` no longer owns that data.

**Shadowing** - Writing ```let mut vec=vec;``` is called shadowing. You are creating a brand new variable named ```vec``` and assigning it the value of old , immuable ```vec``` parameters. The new ```vec``` shadows(hides) the old one for the4 rest of the function.


## Exercise 2 — `move_semantics2.rs`


### 📋 The Original Code

```
fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;

    vec.push(88);

    vec
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    // TODO: Make both vectors `vec0` and `vec1` accessible at the same time to
    // fix the compiler error in the test.
    #[test]
    fn move_semantics2() {
        let vec0 = vec![22, 44, 66];

        let vec1 = fill_vec(vec0);

        assert_eq!(vec0, [22, 44, 66]);
        assert_eq!(vec1, [22, 44, 66, 88]);
    }
}

```

### ❌ Why It Needs Fixing
Comiler thorw's a "borrow of moved value" error. You cannot read from or assert against a variable that has already given up its data.


### ✅ Solution

```
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn move_semantics2() {
        let vec0 = vec![22, 44, 66];
        let vec1 = fill_vec(vec0.clone());
        assert_eq!(vec0, [22, 44, 66]);
        assert_eq!(vec1, [22, 44, 66, 88]);
    }
}
```

### 🧠 Concepts Introduced

**```clone```** - In Rust, simple types like integers i32 or booleans are entirely stored on the stack and implements a trait called ```Copy```. When you pass them to a function , they copy automatically. Complex types like ```Vec``` or ```String``` store data on the heap and do not implement ```Copy```. To duplicate them , they implmenet the ```Clone``` trait , which requires you to explicitly call ```.clone()```.

**Deep Copying** -   Using .clone() is an easy way to solve ownership errors, but it comes with a cost. It allocates new memory on the heap and copies every single element over. While it is the correct answer for this specific exercise, in real-world Rust programs with massive vectors, excessive cloning can slow down your code.

## Exercise 3 — `move_semantics3.rs` 


### 📋 The Original Code 

```
// TODO: Fix the compiler error in the function without adding any new line.
fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    vec.push(88);

    vec
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn move_semantics3() {
        let vec0 = vec![22, 44, 66];
        let vec1 = fill_vec(vec0);
        assert_eq!(vec1, [22, 44, 66, 88]);
    }
}

```

### ❌ Why It Needs Fixing
Compiler throws an error cannot borrow ```vec``` as mutable, as it is not declared as mutable.

### ✅ Solution

```
fn fill_vec(mut vec: Vec<i32>) -> Vec<i32> {
    vec.push(88);
    vec
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;
    #[test]
    fn move_semantics3() {
        let  vec0 = vec![22, 44, 66];
        let vec1 = fill_vec(vec0);
        assert_eq!(vec1, [22, 44, 66, 88]);
    }
}


```

### 🧠 Concepts Introduced
**Mutable Parameters in Signatures** - When you write mut vec: Vec<i32>, you are telling the compiler: "Take ownership of the data passed into this function, and also allow me to modify that data while I own it."

This is structurally doing the exact same thing as let mut vec = vec; inside the function body, but it is much cleaner and saves you a line of code. It is a very common pattern in Rust when a function takes ownership of a variable and immediately needs to alter it.


## Exercise 4 — `move_semantics4.rs`


### 📋 The Original Code

```
fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    // TODO: Fix the compiler errors only by reordering the lines in the test.
    // Don't add, change or remove any line.
    #[test]
    fn move_semantics4() {
        let mut x = Vec::new();
        let y = &mut x;
        let z = &mut x;
        y.push(42);
        z.push(13);
        assert_eq!(x, [42, 13]);
    }
}
```

### ❌ Why It Needs Fixing
Compiler thorws and error cannot borrow ```x``` as mutable more than once at a time

### ✅ Solution

```
fn main() {
}

#[cfg(test)]
mod tests {
    #[test]
    fn move_semantics4() {
        let mut x = Vec::new();
        
        let y = &mut x;
        y.push(42);       
        
        let z = &mut x;   
        z.push(13);     
        
        assert_eq!(x, [42, 13]); 
    }
}

```

### 🧠 Concepts Introduced
**Non-Lexical Lifetimes(NLL)** - You might wonder: "Wait, both y and z are still inside the curly braces of the function. Aren't they both alive until the function ends?"
In older versions of Rust, the answer was yes, and this code would have required you to put y inside its own set of curly braces {} to force it to drop.
However, modern Rust uses Non-Lexical Lifetimes. The compiler is smart enough to look at your code and say, "I see that y.push(42) is the absolute last time y is ever used. I will automatically end y's borrow right there." This allows you to immediately create z on the very next line.

## Exercise 5 — `move_semantics5.rs`


### 📋 The Original Code

```
#![allow(clippy::ptr_arg)]

// TODO: Fix the compiler errors without changing anything except adding or
// removing references (the character `&`).

// Shouldn't take ownership
fn get_char(data: String) -> char {
    data.chars().last().unwrap()
}

// Should take ownership
fn string_uppercase(mut data: &String) {
    data = data.to_uppercase();

    println!("{data}");
}

fn main() {
    let data = "Rust is great!".to_string();

    get_char(data);

    string_uppercase(&data);
}
```

### ❌ Why It Needs Fixing
Comiler throws multiple errors

### ✅ Solution

```
#![allow(clippy::ptr_arg)]
fn get_char(data: &String) -> char {
    data.chars().last().unwrap()
}

// Takes ownership instead of borrowing.
fn string_uppercase(mut data: String) {
    data = data.to_uppercase();

    println!("{data}");
}

fn main() {
    let data = "Rust is great!".to_string();

    get_char(&data);

    string_uppercase(data);
}

```

### 🧠 Concepts Introduced
**Borrowing```&```** -  When get_char(&data) is called, it passes an immutable reference to the data. Think of it like giving the function a pair of binoculars to look at the string. It can read the characters, but it doesn't own the string, so it doesn't destroy it when the function ends. This allows main to keep using data afterward.

**Taking Ownership (Moving)** - When string_uppercase(data) is called, main completely hands over the data variable. string_uppercase now owns it. Because it owns it (and it was marked mut), it is free to completely overwrite it with a new uppercase version. Once string_uppercase finishes executing, that memory is cleaned up and dropped forever.


