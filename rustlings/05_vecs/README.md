# 05 — Vecs

**Section goal:** - Vectors are one of the most-used Rust data structures. In other programming languages, they'd simply be called Arrays, but since Rust operates on a bit of a lower level, an array in Rust is stored on the stack (meaning it can't grow or shrink, and the size needs to be known at compile time), and a Vector is stored in the heap (where these restrictions do not apply).

Vectors are a bit of a later chapter in the book, but we think that they're useful enough to talk about them a bit earlier. We shall be talking about the other useful data structure, hash maps, later.

## Exercise 01 vecs1.rs

```
fn array_and_vec() -> ([i32; 4], Vec<i32>) {
    let a = [10, 20, 30, 40]; // Array

    // TODO: Create a vector called `v` which contains the exact same elements as in the array `a`.
    // Use the vector macro.
    // let v = ???;

    (a, v)
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_array_and_vec_similarity() {
        let (a, v) = array_and_vec();
        assert_eq!(a, *v);
    }
}
```


### ❌ Why It Needs Fixing
The code needs fixing because it's currently incomplete. The function ```test_array_vec```  promises to return a tupe containing a vector ```([i32;4],Vec<i21>)```, but the variable ```v``` hasn't been defined yet. The Rust compiler will complain that it cannot find the value ```v``` in the scope

### ✅ Solution

```
fn array_and_vec() -> ([i32; 4], Vec<i32>) {
    let a = [10, 20, 30, 40]; // Array
    let v = vec![10, 20, 30, 40];
    (a, v)
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_array_and_vec_similarity() {
        let (a, v) = array_and_vec();
        assert_eq!(a, *v);
    }
}

```
### 🧠 Concepts Introduced
**Vectors** -  While arrays (like ```a```) have a fixed, unchangeable size determined at compile time, Vectors are dynamic. They can grow and shrink while your program is running. Because their size can change, Vectors store their data on the heap (which is flexible), whereas standard arrays store their data on the stack (which is fast but inflexible).

**The ```vec!``` Macro**- Creating a Vector from scratch and pushing elements into it one by one can be tedious. Rust provides the handy ```vec!``` macro to initialize a Vector with starting values. Notice how similar the syntax ```vec![10, 20, 30, 40]``` is to the array syntax ```[10, 20, 30, 40]```.

**Dereferencing for Comparison ```(*v)```** - In the test block, you'll see assert_eq!(a, *v);. The asterisk (*) is the dereference operator. Because a is a fixed array [i32; 4] and v is a Vec<i32>, they are fundamentally different types and can't be compared directly. By writing *v, the test looks at the actual underlying contiguous list of numbers (a slice) that the Vector is pointing to on the heap, allowing it to safely compare those numbers against the numbers in the array.


## Exercise 02 vecs2.rs
```
fn vec_loop(input: &[i32]) -> Vec<i32> {
    let mut output = Vec::new();

    for element in input {
        // TODO: Multiply each element in the `input` slice by 2 and push it to
        // the `output` vector.
    }

    output
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_vec_loop() {
        let input = [2, 4, 6, 8, 10];
        let ans = vec_loop(&input);
        assert_eq!(ans, [4, 8, 12, 16, 20]);
    }
}
```

### ❌ Why It Needs Fixing
The code is incomplete

### ✅ Solution


```
fn vec_loop(input: &[i32]) -> Vec<i32> {
    let mut output = Vec::new();

    for element in input {
        output.push(2 * element);
    }

    output
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_vec_loop() {
        let input = [2, 4, 6, 8, 10];
        let ans = vec_loop(&input);
        assert_eq!(ans, [4, 8, 12, 16, 20]);
    }

```

### 🧠 Concepts Introduced
**```.push()```** -  This is the standard method for adding a new element to the end of a ```Vec```. Notice that ```output``` was declared as ```mut``` (mutable) at the top of the function so that we are allowed to change it.

**```*element```** - The for loop iterates over ```&[i32],``` which means element has the type &i32 (a reference to an integer). You can't directly multiply a reference by an integer. The asterisk ```*``` "dereferences" it, essentially telling Rust, "Go to the memory address this reference points to and grab the actual integer value." Once you have the raw value, you can multiply it by 2


