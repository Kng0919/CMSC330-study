# Quiz 5 - Rust

## Q1 Scoping & Borrowing

### Q1.1 Ownership
```rust
pub fn add_bye(a: &mut String) {
	a.push_str("bye");
}
pub fn add_period(mut a: String)->String {
	a.push_str(".");
	return a;
}
fn main() {
	let mut s = String::from("hi ");
	let b = add_bye(&mut s);
	let a = add_period(s);
	// HERE
}
```

	Q1.1: Which variable owns the data originally assigned to 's' at HERE?
	Circle the answer:	s	b	a	Data is out of scope
	
### Q1.2 Lifetimes

The following code takes two strings and returns the shorter of the two strings.
```rust
fn shortest (x:&str, y:&str) -> &str {
 	if x.len() < y.len() { x } else { y }
}
```

It works fine until we have code like this:
```rust
fn main () {
 let x = String::from("there");
 let z;
 	{
 	let y = String::from("hi");
 	z = shortest(&x,&y); //will be &y
 	} //drop y, and thereby z

 	println!("z = {}",z); //yikes!
}
```

We can help mitigate this by having the function definition to be:
```rust
fn shortest<'a>(x:&'a str, y:&'a str) -> &'a str {...}
```

	Select the statements that are true about the updated function definition:
	
	A: This is an example of implicit lifetimes
	
	B: This is an example of explicit lifetimes
	
	C: x and y must have the same lifetime
	
	D: The returned reference must have the same lifetime as the
	
	E: shortest living parameter
	
	F: The main code now runs with this change

## Q2 Struct, Traits, Enums

Refer to the following enum and struct definition for the next questions.

```rust
#[derive(Debug)]
enum Languages {
 OCaml,
 Ruby,
 Rust
}

struct Project { name: &'static str, language: Languages, grades: &'static [u32], }

fn main () {
 const project1: &Project = &Project {
 
 	name: _____BLANK 1_____,
 	language: Languages::Rust,
 	grades: &[48, 52, 0]
 	};
	
	 _____BLANK 2_____
 
}
```

## Q2.1

What would go in place for BLANK 1 so that the project name is "Stark Suit Repair":
	
	A: String::from("Stark Suit Repair")
	
	B: "Stark Suit Repair"

	C: &String::from("Stark Suit Repair")
	
	D: &"Stark Suit Repair"
	
## Q2.2

Which of the following code options would print Rust at BLANK 2 ? Select all that apply:

	A: println!("{:?}", project1.language);
	
	B: println!("{}", project1.language);
	
	C: println!("{:#}", project1.language);
	
	C: println!("{:#?}", project1.language);
	
	D: println!("{:?}", language);

	
##Q2.3 Traits

Let's say we wanted to implement the Assignment trait to the struct Project . It is defined as the following:

```rust	
trait Assignment {
 fn maxPoints(&self) -> i32;
 fn quiz() -> Project { // returns a project
 	Project {
 	name:"Quiz",
 	language: Languages::Rust,
 	grades: &[4,8,8]
 	}
    }
}
```

Complete the following code so that Project is an Assignment by filling in the blanks,
```rust	
___Blank 3 ____{
 fn maxPoints(&self) -> i32 { // sums up points
 	let mut total = 0;
 	for &i in self.grades {
 	total += i;
 	}
 	return total;
    }
}
fn main () {
 let q = ___Blank 4____;
 println!("The total possible points for {} is {}.", q.name, ___Bla //Should print out the sum of q's grades
}
```

Blank 3 (extending project to implement assignment)

	Answer: 
	
Blank 4 (getting the quiz assignment)

	Answer: 
	
Blank 5 (calling the method, you will not receive points for hardcoding this)

	Answer: 
	
## Q3

```rust	
fn shortest(x: &str, y: &str) -> &str {
 	if x.bytes().len() < y.bytes().len() { x } 
	else { y.bytes() }
}
fn main() {
 let hi = String::from("HelloCMSC330");
 let hello = "WinterBreakLoading";
 
 let l = shortest(hi, hello);

 println!("{}", hi);
}
```

There exists three errors/bugs to the code above. State 2 of the bugs and provide a fix to them.

Bug 1:

	Answer: 
	
Bug 2:

	Answer: 
