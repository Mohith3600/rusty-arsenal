# 08 — Enums

> **Section goal:** Getting familiar to pattern matching and enum.

## Exercise 1 — `enums1.rs`

### 📋 The Original Code

```
#[derive(Debug)]
enum Message {
    // TODO: Define a few types of messages as used below.
}

fn main() {
    println!("{:?}", Message::Resize);
    println!("{:?}", Message::Move);
    println!("{:?}", Message::Echo);
    println!("{:?}", Message::ChangeColor);
    println!("{:?}", Message::Quit);
}

```


### ❌ Why It Needs Fixing
In main function multiple variants are declared but nothing inside Message block


### ✅ Solution
```
#[derive(Debug)]
enum Message {
    Resize,
    Move,
    Echo,
    ChangeColor,
    Quit,
}

fn main() {
    println!("{:?}", Message::Resize);
    println!("{:?}", Message::Move);
    println!("{:?}", Message::Echo);
    println!("{:?}", Message::ChangeColor);
    println!("{:?}", Message::Quit);
}

```


### 🧠 Concepts Introduced
**```enum```** - An ```enum``` is a way to define a custom data type by explicitly listing out all of its possible variants. It is Rust's way of saying, "A Message  can be a Resize, OR a Move, OR an Echo.. but it can only ever be exactly one of those things at a time.

## Exercise 2 — `enums2.rs`

### 📋 The Original Code

```
#[derive(Debug)]
struct Point {
    x: u64,
    y: u64,
}

#[derive(Debug)]
enum Message {
    // TODO: Define the different variants used below.
}

impl Message {
    fn call(&self) {
        println!("{self:?}");
    }
}

fn main() {
    let messages = [
        Message::Resize {
            width: 10,
            height: 30,
        },
        Message::Move(Point { x: 10, y: 15 }),
        Message::Echo(String::from("hello world")),
        Message::ChangeColor(200, 255, 255),
        Message::Quit,
    ];

    for message in &messages {
        message.call();
    }
}

```


### ❌ Why It Needs Fixing
The enum is empty and the compiler doesn't know what these variables are or what data they are allowed to hold


### ✅ Solution
```
#[derive(Debug)]
struct Point {
    x: u64,
    y: u64,
}

#[derive(Debug)]
enum Message {
    Resize { width: u64, height: u64 },
    Move(Point),
    Echo(String),
    ChangeColor(u8, u8, u8),
    Quit,
}

impl Message {
    fn call(&self) {
        println!("{self:?}");
    }
}

fn main() {
    let messages = [
        Message::Resize {
            width: 10,
            height: 30,
        },
        Message::Move(Point { x: 10, y: 15 }),
        Message::Echo(String::from("hello world")),
        Message::ChangeColor(200, 255, 255),
        Message::Quit,
    ];

    for message in &messages {
        message.call();
    }
}
```

### 🧠 Concepts Introduced
**Enums with Data** - Unlike many other languages where an enum is just an integer under the hood, Rust enums can pack complex data directly inside the variant. This means you don't need a bunch of separate structs to handle different types of message; you can bundle them all under a single ```Message``` umbrella type

***Struct-like(```Resize```)*** - You can define named fields directly inside the enum variant 
***Tuple-like(```Move```,```Echo ```, ```ChangeColor)```***- You can just list the data types in paraentheses. It can be a single built in type like String  or a custom Point struct



## Exercise 3 — `enums3.rs`

### 📋 The Original Code

```
struct Point {
    x: u64,
    y: u64,
}

enum Message {
    Resize { width: u64, height: u64 },
    Move(Point),
    Echo(String),
    ChangeColor(u8, u8, u8),
    Quit,
}

struct State {
    width: u64,
    height: u64,
    position: Point,
    message: String,
    // RGB color composed of red, green and blue.
    color: (u8, u8, u8),
    quit: bool,
}

impl State {
    fn resize(&mut self, width: u64, height: u64) {
        self.width = width;
        self.height = height;
    }

    fn move_position(&mut self, point: Point) {
        self.position = point;
    }

    fn echo(&mut self, s: String) {
        self.message = s;
    }

    fn change_color(&mut self, red: u8, green: u8, blue: u8) {
        self.color = (red, green, blue);
    }

    fn quit(&mut self) {
        self.quit = true;
    }

    fn process(&mut self, message: Message) {
        // TODO: Create a match expression to process the different message
        // variants using the methods defined above.
    }
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_match_message_call() {
        let mut state = State {
            width: 0,
            height: 0,
            position: Point { x: 0, y: 0 },
            message: String::from("hello world"),
            color: (0, 0, 0),
            quit: false,
        };

        state.process(Message::Resize {
            width: 10,
            height: 30,
        });
        state.process(Message::Move(Point { x: 10, y: 15 }));
        state.process(Message::Echo(String::from("Hello world!")));
        state.process(Message::ChangeColor(255, 0, 255));
        state.process(Message::Quit);

        assert_eq!(state.width, 10);
        assert_eq!(state.height, 30);
        assert_eq!(state.position.x, 10);
        assert_eq!(state.position.y, 15);
        assert_eq!(state.message, "Hello world!");
        assert_eq!(state.color, (255, 0, 255));
        assert!(state.quit);
    }
}

```


### ❌ Why It Needs Fixing
The ```process``` method received a ```Message``` enum , but it currently does nothing. You need to inspect which variant of ```Message``` was passed in, extract the data hiding inside the variant, and pass that data to the correct state-updating method


### ✅ Solution

```
struct Point {
    x: u64,
    y: u64,
}

enum Message {
    Resize { width: u64, height: u64 },
    Move(Point),
    Echo(String),
    ChangeColor(u8, u8, u8),
    Quit,
}

struct State {
    width: u64,
    height: u64,
    position: Point,
    message: String,
    // RGB color composed of red, green and blue.
    color: (u8, u8, u8),
    quit: bool,
}

impl State {
    fn resize(&mut self, width: u64, height: u64) {
        self.width = width;
        self.height = height;
    }

    fn move_position(&mut self, point: Point) {
        self.position = point;
    }

    fn echo(&mut self, s: String) {
        self.message = s;
    }

    fn change_color(&mut self, red: u8, green: u8, blue: u8) {
        self.color = (red, green, blue);
    }

    fn quit(&mut self) {
        self.quit = true;
    }

    fn process(&mut self, message: Message) {
        fn process(&mut self, message: Message) {
        // We match on the incoming message enum
        match message {
            // Destructuring a struct-like variant
            Message::Resize { width, height } => self.resize(width, height),
            
            // Destructuring a tuple-like variant with a custom struct
            Message::Move(point) => self.move_position(point),
            
            // Destructuring a tuple-like variant with a String
            Message::Echo(s) => self.echo(s),
            
            // Destructuring a tuple-like variant with multiple values
            Message::ChangeColor(r, g, b) => self.change_color(r, g, b),
            
            // Matching a unit-like variant (no data to extract)
            Message::Quit => self.quit(),
        }
    }
    }
}

fn main() {
    // You can optionally experiment here.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_match_message_call() {
        let mut state = State {
            width: 0,
            height: 0,
            position: Point { x: 0, y: 0 },
            message: String::from("hello world"),
            color: (0, 0, 0),
            quit: false,
        };

        state.process(Message::Resize {
            width: 10,
            height: 30,
        });
        state.process(Message::Move(Point { x: 10, y: 15 }));
        state.process(Message::Echo(String::from("Hello world!")));
        state.process(Message::ChangeColor(255, 0, 255));
        state.process(Message::Quit);

        assert_eq!(state.width, 10);
        assert_eq!(state.height, 30);
        assert_eq!(state.position.x, 10);
        assert_eq!(state.position.y, 15);
        assert_eq!(state.message, "Hello world!");
        assert_eq!(state.color, (255, 0, 255));
        assert!(state.quit);
    }
}

```

### 🧠 Concepts Introduced

**Pattern Matching & Exhaustiveness** 
The match keyword in Rust is incredibly powerful. Unlike a switch statement in other languages, Rust's match is exhaustive. This means the compiler forces you to handle every single possible variant of the Message enum. If you forget to add a line for Message::Quit, your code will simply refuse to compile. This eliminates entire classes of runtime bugs where an event is accidentally ignored.

**Destructuring Data**
Notice how we wrote Message::Resize { width, height }.
When the match statement sees that the incoming message is a Resize variant, it automatically cracks open the variant and creates two new, temporary variables (width and height) containing the data that was trapped inside. The fat arrow => then passes those newly freed variables directly into your self.resize(width, height) method.
