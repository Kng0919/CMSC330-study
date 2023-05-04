# Quiz 5 - Rust

## Q1 Scoping & Borrowing
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
	



