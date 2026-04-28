# 01 - Variables

> **Section goal:** Introduce to variables in Rust. By default Rust stores the variables immutable. Once saved you cannot change them
## Exercise 01 - variables01.rs

```
fn main() {
    // TODO: Add the missing keyword.
    x = 5;

    println!("x has the value {x}");
}


### ❌ Why It Needs Fixing


```
### ✅ Solution
```
let x = 5;
println!("x has the value of {X}");

```
### 🧠 Concepts Introduced
**`let`** - Let is a keyword in Rust. It let's you bind a value to variable or a statement. It covers the conept of shadowing.  

## Exercise 02 variables02.rs

```
fn main() {
    // TODO: Change the line below to fix the compiler error.
    let x;

    if x == 10 {
        println!("x is ten!");
    } else {
        println!("x is not ten!");
    }
}


### ❌ Why It Needs Fixing
When you run the code you get an compile error. In the code variable has been declared without a value. So to avoid compiling error we declare the variable with a value. 


```
### ✅ Solution
```
fn main()
{
let x = 10;
// or x :u10 = 10;

if x == 10 {
    println!("x is ten!");
} else {
    println!("X is not ten!);
    }
}

```

### 🧠 Concepts Introduced
**Initialization** It is when you declare a variable and assign a value to it. When you declare a variable it creates a memory space on the RAM. This causes compiler to panic. So instead of reading the value from the memory address Rust compiler forces you to assign a value to the decalred variable. This security feature helps to not crash the program.

## Exercise 03 variables03.rs
```
fn main() {
    // TODO: Change the line below to fix the compiler error.
    let x: i32;

    println!("Number {x}");
}
```
### ❌ Why It Needs Fixing
As discussed earlier there are 2 ways to initialize a variable explicitly and implicitly. When you declare a variable along with it's value  Rust knows the data type and will store. There are different types available in Rust Integers, Float, Strings ..... . In this exercise i32 stands for signed integer. Signed integers represents both positive and negative integers. When a signed integer is declared it takes 4 bytes in memory. The range of signed integer varies from -2,147,483,648 to 2,147,483,647.


### ✅ Solution
```
fn main() {
    // TODO: Change the line below to fix the compiler error.
    let x: i32 = 100;

    println!("Number {x}");
}
```

### 🧠 Concepts Introduced
**Explicit Initialization** - You can use multiple data types to intialize a variable. Variables should be initialized before they perfom a function or else compiler will throw a error.

## Exercise 04 variables04.rs
```
// TODO: Fix the compiler error.
fn main() {
    let x = 3;
    println!("Number {x}");

    x = 5; // Don't change this line
    println!("Number {x}");
}
```
### ❌ Why It Needs Fixing
x variable is twice. So we need to use ```mut``` before x = 3. To resolve comiling errors.

### ✅ Solution
```
fn main() {
    let mut x = 3;
    println!("Number {x}");

    x = 5; // Don't change this line
    println!("Number {x}");
}
```


### 🧠 Concepts Introduced
**```mut```** - By default the variables decalred in the Rust immutable. Once they are declared values cannot be changed. If user wants to use a same variable multiple times ```mut``` comes to play. 

## Exercise 05 variables5.rs
```
fn main() {
    let number = "T-H-R-E-E"; // Don't change this line
    println!("Spell a number: {number}");

    // TODO: Fix the compiler error by changing the line below without renaming the variable.
    number = 3;
    println!("Number plus two is: {}", number + 2);
}

```
### ❌ Why It Needs Fixing


### ✅ Solution

```
fn main() {
    let number = "T-H-R-E-E"; // Don't change this line
    println!("Spell a number: {number}");

    // TODO: Fix the compiler error by changing the line below without renaming the variable.
    let  number = 3;
    println!("Number plus two is: {}", number + 2);
}

```
### 🧠 Concepts Introduced
**Shadowing**  - Shadowing lets you use a variable twice even with different data types.  Shadowing and Mutability operatos at different level and are used for different purposes. In order to use Mutability we need to use ```let mut ``` keyword. Let's say a variable is score is declared with value 10 and in next line we declare score with 5 it works. But if we change the data type as "15" it will not work because  once declared data type is fixed it cannot be changed. In case of shadowing it's completely different variables can have different values. Once declared they are overshadowed by previous variables. Shadowing is useful when we want to use a variable with same name temporarily inside a scope. Variables must be used ```let``` keyword. 


## Exercise 06 variables06.rs

```
// TODO: Change the line below to fix the compiler error.
const NUMBER = 3;

fn main() {
    println!("Number: {NUMBER}");
}

```

### ❌ Why It Needs Fixing
Constants that are declared are needs to implicity declared


### ✅ Solution

```
// TODO: Change the line below to fix the compiler error.
const NUMBER:u8 = 3;

fn main() {
    println!("Number: {NUMBER}");
}
```
### 🧠 Concepts Introduced
**const** - Till now we have came across variables and different types. Now we are moving to constants ```const```. To declare a constant type annotation is mandatory. Once a constant is declared the values cannot be changed. There ar types of constants Global and local constants. Global constants are mostly decalred at the start of the program. Value can be accessed throughout the program. Local constants can be within the scope of the function block.
