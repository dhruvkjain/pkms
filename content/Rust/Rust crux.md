[[crux_index]]

Rust  ->  statically typed (knows types of all variables at compile time)
shadowing  ->  reusing same variable
Integers  
- isize, usize -> 64 or 32 depends on computer architecture
- has Ordering trait

When you’re compiling in release mode with the `--release` flag, Rust does _not_ include checks for integer overflow that cause panics. Instead, if overflow occurs, Rust performs _two’s complement wrapping_. In short, values greater than the maximum value the type can hold “wrap around” to the minimum of the values the type can hold. In the case of a `u8`, the value 256 becomes 0, the value 257 becomes 1, and so on.

Boolean  ->  1 byte in size
Char  ->  4 byte in size

Tuple
- compound type  ->  `(int, bool, string ...)`
- unit type  ->  `()`  ->  for example `Ok(())`

Statement  ->  returns nothing
Expression  ->  returns a value

Expression + ";"  ->  Statement

`(1..n).rev()`  ->  reverse iterator

string literals  ->  static memory  ->  stack allocated  ->  cant be changed

vector literals  -> macros `vec![]`  ->  this expands to normal heap allocation 

Ownership
- each value has a owner
- one owner at a time
- if owner goes out of scope value is dropped

double free error  ->  rust uses "move" operation, so passing variable to a function changes its owner  ->  transfer ownership  ->  there no double free error

passing variables is shallow copy
for deep copy use clone

Borrowing
- at any point in code, a variable can have:
	- one mutable reference
	- infinite immutable reference

impl block
- associated functions  ->  have `self` as parameter  ->  `::` is used to call them
- method  ->  does not have `self` as parameter  ->  `.method()` is used to call them

