# 02 - Functions

**Section goal:** Here, you'll learn how to write functions and how the Rust compiler can help you debug errors even in more complex code.

## Exercise 01 - functions1.rs
```
// TODO: Add some function with the name `call_me` without arguments or a return value.

fn main() {
    call_me(); // Don't change this line
}
```

### ❌ Why It Needs Fixing
When you run the code the compiler throws an error call_me(); function is out of scope. 


### ✅ Solution
```
fn main() {
    call_me(); // Don't change this line
}

fn call_me()
{
    //println!("Hello world"); //Function can be empty as well
}
```

### 🧠 Concepts Introduced
**Function Definition** - In Rust functions are declared using ```fn``` keyword. In this exercise ```call_me()``` function is called inside main function. Execution of program starts from main function. There's no ```call_me()``` function in the Rust code. So the compiler throws an errors. ```fn call_me()``` is declared outisde main function it can be empty or can have parameters.  


## Exercise 02 - functions2.rs
```
// TODO: Add the missing type of the argument `num` after the colon `:`.
fn call_me(num:) {
    for i in 0..num {
        println!("Ring! Call number {}", i + 1);
    }
}

fn main() {
    call_me(3);
}
```


### ❌ Why It Needs Fixing



### ✅ Solution
```
fn call_me(num:u8) {
    for i in 0..num {
        println!("Ring! Call number {}", i + 1);
    }
}

fn main() {
    call_me(3);
}
```

### 🧠 Concepts Introduced
**Function Parameter** 
When you define a function that takes input you must explicitly define the data type. This allows the compiler to ensure that you are passing the correct kind of data. Example : ```fn example(num: u32)``` This tell the compiler the input data will be unsigned integer 

**Range-Based Iteration** 
```for``` loop in rust starts with a number a and it will keep running till the condition is met. In this function the loop starts from 1 and it will end at 3. In main function ```call_me(3)``` passes to ```fn call_me(num:u8)``` . After that for loop is initited it starts 1 and ends at 3.  

## Exercise 03 - functions3.rs
```
fn call_me(num: u8) {
    for i in 0..num {
        println!("Ring! Call number {}", i + 1);
    }
}

fn main() {
    // TODO: Fix the function call.
    call_me();
}
```

### ❌ Why It Needs Fixing
Function is with usigned 8 integer. Without giving input so compiler throws error.

### ✅ Solution
```
fn call_me(num:u8) {
    for i in 0..num {
        println!("Ring! Call number {}", i + 1);
    }
}

fn main() {
    call_me(100);
}
```

### 🧠 Concepts Introduced
**Argument Passing** - ```fn call_me(num:u8)``` was declared with input parameter num with usigned 8 data type. The data type takes only 0-255 numbers. Compiler will throw an error if a number is above 255. So if we input a number let's say 10 the loop will start from 1 and will end at 10.

## Exercise 04 - functions4.rs
```
// This store is having a sale where if the price is an even number, you get 10
// Rustbucks off, but if it's an odd number, it's 3 Rustbucks off.
// Don't worry about the function bodies themselves, we are only interested in
// the signatures for now.

fn is_even(num: i64) -> bool {
    num % 2 == 0
}

// TODO: Fix the function signature.
fn sale_price(price: i64) -> {
    if is_even(price) {
        price - 10
    } else {
        price - 3
    }
}

fn main() {
    let original_price = 51;
    println!("Your sale price is {}", sale_price(original_price));
}
```

### ❌ Why It Needs Fixing
Code will not return because there's no return type to the function. 

### ✅ Solution
```

fn is_even(num: i64) -> bool {
    num % 2 == 0
}

// TODO: Fix the function signature.
fn sale_price(price: i64) -> i64 {
    if is_even(price) {
        price - 10
    } else {
        price - 3
    }
}

fn main() {
    let original_price = 51;
    println!("Your sale price is {}", sale_price(original_price));
}

```

### 🧠 Concepts Introduced
**Function Return Types** - In Rust functions that return a value must explicitly declare the return type using the "arrow" ->. Additionally, because Rust is an expression-based language, the result of an ```if/else``` block can be used as a return value without using ```return``` keyword.
**Implicit Returns** - There's no ```return```  and semicolon at the end of if and else blocks. Let's deep dive ```price - 10 ;``` is statement (returns nothing) ```price - 10 ``` is an expression(returns the value). 

## Exercise 05 - functions5.rs
```
// TODO: Fix the function body without changing the signature.
fn square(num: i32) -> i32 {
    num * num;
}

fn main() {
    let answer = square(3);
    println!("The square of 3 is {answer}");
}
```


### ❌ Why It Needs Fixing
Compiler will not execute the program as fn square is supposed to take value of 3 will multiple num * num and returns the value. 

### ✅ Solution

```
fn square(num: i32) -> i32 {
    num * num
  /return num * num;
}

fn main() {
    let answer = square(3);
    println!("The square of 3 is {answer}");
}
```
### 🧠 Concepts Introduced
**Statemet** - Statement will not return a value

**Expression** - Returns a value after execution the function. Can use ```return``` keyword explicitly for statement to return the value

