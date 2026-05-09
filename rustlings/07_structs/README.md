# 07 — Structs

> **Section goal:** To get familiar with ```struct```  and ```impl```


## Exercise 1 — `structs1.rs `

### 📋 The Original Code

```
struct ColorRegularStruct {
    // TODO: Add the fields that the test `regular_structs` expects.
    // What types should the fields have? What are the minimum and maximum values for RGB colors?
}

struct ColorTupleStruct(/* TODO: Add the fields that the test `tuple_structs` expects */);

#[derive(Debug)]
struct UnitStruct;

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn regular_structs() {
        // TODO: Instantiate a regular struct.
        // let green =

        assert_eq!(green.red, 0);
        assert_eq!(green.green, 255);
        assert_eq!(green.blue, 0);
    }

    #[test]
    fn tuple_structs() {
        // TODO: Instantiate a tuple struct.
        // let green =

        assert_eq!(green.0, 0);
        assert_eq!(green.1, 255);
        assert_eq!(green.2, 0);
    }

    #[test]
    fn unit_structs() {
        // TODO: Instantiate a unit struct.
        // let unit_struct =
        let message = format!("{unit_struct:?}s are fun!");

        assert_eq!(message, "UnitStructs are fun!");
    }
}
```


### ❌ Why It Needs Fixing



### ✅ Solution
```
struct ColorRegularStruct {
    red: u8,
    green: u8,
    blue: u8,
}

struct ColorTupleStruct(u8, u8, u8);

#[derive(Debug)]
struct UnitStruct;

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn regular_structs() {
        let green = ColorRegularStruct {
            red: 0,
            green: 255,
            blue: 0,
        };

        assert_eq!(green.red, 0);
        assert_eq!(green.green, 255);
        assert_eq!(green.blue, 0);
    }

    #[test]
    fn tuple_structs() {
        let green = ColorTupleStruct(0, 255, 0);

        assert_eq!(green.0, 0);
        assert_eq!(green.1, 255);
        assert_eq!(green.2, 0);
    }

    #[test]
    fn unit_structs() {
        let unit_struct = UnitStruct;
        let message = format!("{unit_struct:?}s are fun!");

        assert_eq!(message, "UnitStructs are fun!");
    }
}
```

### 🧠 Concepts Introduced
**Tuple Structure** -These are hybrid between a struct and a tuple. You give the struct tname, but you leave the fields unnamed. You instantiate them with parenthesis () and access their data using indices like ```.0``` , ```.1``` and ```.2```. 



## Exercise 2 — `structs2.rs `

### 📋 The Original Code

```
#[derive(Debug)]
struct Order {
    name: String,
    year: u32,
    made_by_phone: bool,
    made_by_mobile: bool,
    made_by_email: bool,
    item_number: u32,
    count: u32,
}

fn create_order_template() -> Order {
    Order {
        name: String::from("Bob"),
        year: 2019,
        made_by_phone: false,
        made_by_mobile: false,
        made_by_email: true,
        item_number: 123,
        count: 0,
    }
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn your_order() {
        let order_template = create_order_template();

        // TODO: Create your own order using the update syntax and template above!
        // let your_order =

        assert_eq!(your_order.name, "Hacker in Rust");
        assert_eq!(your_order.year, order_template.year);
        assert_eq!(your_order.made_by_phone, order_template.made_by_phone);
        assert_eq!(your_order.made_by_mobile, order_template.made_by_mobile);
        assert_eq!(your_order.made_by_email, order_template.made_by_email);
        assert_eq!(your_order.item_number, order_template.item_number);
        assert_eq!(your_order.count, 1);
    }
}
```


### ❌ Why It Needs Fixing
Compiler throws error of `UnitStruct` never constructed 


### ✅ Solution
```
#[derive(Debug)]
struct Order {
    name: String,
    year: u32,
    made_by_phone: bool,
    made_by_mobile: bool,
    made_by_email: bool,
    item_number: u32,
    count: u32,
}

fn create_order_template() -> Order {
    Order {
        name: String::from("Bob"),
        year: 2019,
        made_by_phone: false,
        made_by_mobile: false,
        made_by_email: true,
        item_number: 123,
        count: 0,
    }
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn your_order() {
        let order_template = create_order_template();

        let your_order = Order {
            name: String::from("Hacker in Rust"),
            count: 1,
            // Struct update syntax
            ..order_template
        };

        assert_eq!(your_order.name, "Hacker in Rust");
        assert_eq!(your_order.year, order_template.year);
        assert_eq!(your_order.made_by_phone, order_template.made_by_phone);
        assert_eq!(your_order.made_by_mobile, order_template.made_by_mobile);
        assert_eq!(your_order.made_by_email, order_template.made_by_email);
        assert_eq!(your_order.item_number, order_template.item_number);
        assert_eq!(your_order.count, 1);
    }
}
```

### 🧠 Concepts Introduced
**Struct Update Syntax(..)**
When you are creating a new instance of a struct that shares most of its data with an existing instance, writing out every single field again is tedious.

Rust allows you to use the .. syntax to say: "For any fields I haven't explicitly named here, just grab the values from this other instance."

A couple of important rules about this syntax:

It must come last: The ..order_template must be placed at the very end of your struct initialization. You cannot put a comma after it.

It respects Ownership: Because the name field in this struct is a String (which does not implement the Copy trait), if you had used ..order_template to copy the name over, the original order_template would lose ownership of that string and you wouldn't be able to use it anymore! However, because you explicitly overrode name with a brand new string, and all the copied fields (u32 and bool) are simple stack-allocated values that copy automatically, your order_template actually remains fully valid and usable after this line.



## Exercise 3 — `structs3.rs `

### 📋 The Original Code

```
// Structs contain data, but can also have logic. In this exercise, we have
// defined the `Fireworks` struct and a couple of functions that work with it.
// Turn these free-standing functions into methods and associated functions
// to express that relationship more clearly in the code.

#![deny(clippy::use_self)] // practice using the `Self` type

#[derive(Debug)]
struct Fireworks {
    rockets: usize,
}

// TODO: Turn this function into an associated function on `Fireworks`.
fn new_fireworks() -> Fireworks {
    Fireworks { rockets: 0 }
}

// TODO: Turn this function into a method on `Fireworks`.
fn add_rockets(fireworks: &mut Fireworks, rockets: usize) {
    fireworks.rockets += rockets
}

// TODO: Turn this function into a method on `Fireworks`.
fn start(fireworks: Fireworks) -> String {
    "🚀".repeat(fireworks.rockets)
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn start_some_fireworks() {
        let f = Fireworks::new();
        assert_eq!(f.start(), "");

        let mut f = Fireworks::new();
        f.add_rockets(3);
        assert_eq!(f.start(), "🚀🚀🚀");

        let mut f = Fireworks::new();
        f.add_rockets(7);
        // We don't use method syntax in the last test to ensure the `start`
        // function takes ownership of the fireworks.
        assert_eq!(Fireworks::start(f), "🚀🚀🚀🚀🚀🚀🚀");
    }
}
```


### ❌ Why It Needs Fixing
Compiler throws multiple errors

### ✅ Solution
```
#![deny(clippy::use_self)] // practice using the `Self` type

#[derive(Debug)]
struct Fireworks {
    rockets: usize,
}

// Create an implementation block for the Fireworks struct
impl Fireworks {
    // Associated function: Doesn't take `self`. Acts like a constructor.
    fn new() -> Self {
        Self { rockets: 0 }
    }

    // Method: Takes a mutable reference to `self` to modify the struct's data.
    fn add_rockets(&mut self, rockets: usize) {
        self.rockets += rockets;
    }

    // Method: Takes ownership of `self`, consuming the struct.
    fn start(self) -> String {
        "🚀".repeat(self.rockets)
    }
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn start_some_fireworks() {
        let f = Fireworks::new();
        assert_eq!(f.start(), "");

        let mut f = Fireworks::new();
        f.add_rockets(3);
        assert_eq!(f.start(), "🚀🚀🚀");

        let mut f = Fireworks::new();
        f.add_rockets(7);
        assert_eq!(Fireworks::start(f), "🚀🚀🚀🚀🚀🚀🚀");
    }


```

### 🧠 Concepts Introduced

**1. The impl Block**
Everything related to the behavior of a struct goes inside an impl StructName {} block. This tells the compiler that these functions belong specifically to that struct.

**2. Associated Functions (Self) vs. Methods (self)**

Associated Functions: Notice that new() doesn't have self as its first parameter. Because it doesn't take a specific instance to run, you call it using double colons on the type itself: Fireworks::new(). This is exactly how String::new() or Vec::new() works!

Methods: add_rockets and start both take some form of self as their first parameter. This allows you to call them on a specific instance of the struct using dot notation: f.start().

**3. The Three Types of self**
Notice how we handled the self parameter in the methods based on the ownership rules you learned earlier:

&mut self (in add_rockets): We need to change the data inside the struct, so we take a mutable borrow.

self (in start): We take full ownership of the struct. Once f.start() is called, f is consumed and destroyed. You can't use it again!

(Not used here, but for completeness) &self: Used when you just want to read the data inside the struct without changing it or destroying it.

**4. The Capital Self Alias**
At the top of the file, #![deny(clippy::use_self)] is a linter rule that yells at you if you type out the word Fireworks inside the impl block. Instead, Rust prefers you use the capital-S Self keyword. Self is just an alias for "whatever type this impl block is for." If you rename the struct later, you don't have to rewrite the return types inside the impl block!
