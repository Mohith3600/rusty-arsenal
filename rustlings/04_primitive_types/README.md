# 04 - Primitive Types

**Section goal:** - Rust has a couple of basic types that are directly implemented into the compiler. In this section, we'll go through the most important ones

## Exercise 01 primitive_types1.rs

```
// Booleans (`bool`)

fn main() {
    let is_morning = true;
    if is_morning {
        println!("Good morning!");
    }

    // TODO: Define a boolean variable with the name `is_evening` before the `if` statement below.
    // The value of the variable should be the negation (opposite) of `is_morning`.
    // let …
    if is_evening {
        println!("Good evening!");
    }
}
```

### ❌ Why It Needs Fixing
Compiler throws an error ```is_evening``` is not declared

### ✅ Solution

```
fn main() {
    let is_morning = true;
    if is_morning {
        println!("Good morning!");
    }

    let is_evening = false;
    if is_evening {
        println!("Good evening!");
    }
}

```

### 🧠 Concepts Introduced
**Boolean** - Rust has multiple data types scalar and compound. Boolean is a scalar data type. The value of Boolean will always be True of False.

## Exercise 02 primitive_types2.rs

```
// Characters (`char`)

fn main() {
    // Note the _single_ quotes, these are different from the double quotes
    // you've been seeing around.
    let my_first_initial = 'C';
    if my_first_initial.is_alphabetic() {
        println!("Alphabetical!");
    } else if my_first_initial.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }

    // TODO: Analogous to the example before, declare a variable called `your_character`
    // below with your favorite character.
    // Try a letter, try a digit (in single quotes), try a special character, try a character
    // from a different language than your own, try an emoji 😉
    // let your_character = '';

    if your_character.is_alphabetic() {
        println!("Alphabetical!");
    } else if your_character.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }
}
```

### ❌ Why It Needs Fixing
The code needs fixing because it's trying to check the condition of the variable ```your_character``` that hasn't been actually created yet!

### ✅ Solution

```
fn main() {
    // Note the _single_ quotes, these are different from the double quotes
    // you've been seeing around.
    let my_first_initial = 'C';
    if my_first_initial.is_alphabetic() {
        println!("Alphabetical!");
    } else if my_first_initial.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }
    let your_character = '😀';
    if your_character.is_alphabetic() {
        println!("Alphabetical!");
    } else if your_character.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }
}

```

### 🧠 Concepts Introduced
**Single Quotes** - ```char``` uses single quote to denote. Unlike strings which uses "" to denote. 
**Unicode** - Rusts ```char``` type is 4 bytes in size and represents a "Unicode Scalar Value". This means it isn't limited to Just Standard ASCII letter number. It can hold accented, characters, lettters from any languages and even emojis.
**Built-in-Methods** - ```is.alphabetic()```, ```.is_numeric()```, ```.is_ascii()```


## Exercise 03 primitive_types3.rs

```
fn main() {
    // TODO: Create an array called `a` with at least 100 elements in it.
    // let a = ???

    if a.len() >= 100 {
        println!("Wow, that's a big array!");
    } else {
        println!("Meh, I eat arrays like that for breakfast.");
        panic!("Array not big enough, more elements needed");
    }
}
```

### ❌ Why It Needs Fixing
Throw's an error because there's no variable named as ```a```. We need to create an array with ```a```.

### ✅ Solution

```
fn main() {
    let a = [0; 100]; 

    if a.len() >= 100 {
        println!("Wow, that's a big array!");
    } else {
        println!("Meh, I eat arrays like that for breakfast.");
        panic!("Array not big enough, more elements needed");
    }
}
```

### 🧠 Concepts Introduced
**Repeat Expression Syntax** - Rust provides a handy shortand syntax for initializing an array with a repeated value.  The first part ```0``` is the value and the second part ```100``` sepearted by ; is the length of the array. This tell the compiler exactly how many times to repeat the intial value. 

## Exercise 04 primitive_types4.rs

```
fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    #[test]
    fn slice_out_of_array() {
        let a = [1, 2, 3, 4, 5];

        // TODO: Get a slice called `nice_slice` out of the array `a` so that the test passes.
        // let nice_slice = ???

        assert_eq!([2, 3, 4], nice_slice);
    }
}

```

### ❌ Why It Needs Fixing
Compiler throws an error as ```nice_slice``` is not initialized.
### ✅ Solution
```
fn main(){
}

#[cfg(test)]
mod tests {
  #[test]
  fn slice_out_array() {
      let a = [1,2,3,4,5];
      let nice_slice = &a[1..4];
      assert_eq!([2,3,4],nice_slice);
  }
}
```

### 🧠 Concepts Introduced
**Borrowing** - A slice is a reference to a contigous block of memeory, so you must use ```&``` to borrow from the original array.
**Range** - Rust arrays are zero-indexed. The value ```2``` is at index ```1```. The range syntax ```start..end``` is inclusive on the start and exclusive on the end. 

## Exercise 05 primitive_types5.rs

```
fn main() {
    let cat = ("Furry McFurson", 3.5);

    // TODO: Destructure the `cat` tuple in one statement so that the println works.
    // let /* your pattern here */ = cat;

    println!("{name} is {age} years old");
}

```

### ❌ Why It Needs Fixing
The code needs fixing because ```println!``` trying t print a variable that doesn't exist.

### ✅ Solution

```
fn main() {
  let cat = ("Furry McFurson" , 3.5);

  let (name,age) = cat ;
  println!("{name} is {age} years old");
}

```

### 🧠 Concepts Introduced
**Tuple Destructing** - A tuple packages multiple, potentially different types of values into one single variable(```cat```). Destructing is the exact opposite process: it allows you to break that single package open and immediately assign its intenral values to distinct, named variables. 
By using a pattern that matches the structure of the tuple, you can upack it in a single line.


## Exercise 06 primitive_types6.rs

```
fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    #[test]
    fn indexing_tuple() {
        let numbers = (1, 2, 3);

        // TODO: Use a tuple index to access the second element of `numbers`
        // and assign it to a variable called `second`.
        // let second = ???;

        assert_eq!(second, 2, "This is not the 2nd number in the tuple!");
    }
}

```

### ❌ Why It Needs Fixing
Compiler throws an error as ```second``` is not initialized. We need to declare ```second```.

### ✅ Solution

```
fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    #[test]
    fn indexing_tuple() {
        let numbers = (1, 2, 3);
        let second = numbers.1;    
        assert_eq!(second, 2, "This is not the 2nd number in the tuple!");
    }
}

```

### 🧠 Concepts Introduced
**Indexing** - In rust strict type checking happens at compile time. For arrays indexing is done using index[1] and for tuples index.1. Tuple is of multiple data types so the compiler needs to know exact position at compile time .


