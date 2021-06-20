# Rust Notes

### What is r2d2?

A generic connection pool.

### What is a connection pool?

A connection pool it's a object used to mantain a set of open connections to a
database. Opening a new database connection every time one is needed is both
inefficient and can lead to resource exhaustion under high traffic conditions.

### `usize` and `isize` vs `u32` and `i32`

**i32**: The 32-bit signed integer type.

**u32**: The 32-bit unsigned integer type.

**isize**: The pointer-sized signed integer type.

**usize**: The pointer-sized unsigned integer type.

Use usize and isize when it’s related to memory size – the size of an object,
or indexing a vector, for instance. It will be a 32-bit number on 32-bit
platforms, as that’s the limit of memory they can address, and likewise for 64-bit.
Use u32 and i32 when you just want numbers.

So the use cases are:

**isize/usize for:** memory size, index, offset related (heap)
**i32/u32/i64/u64 for:** integers (stack)

### What are lifetimes?

A variable's lifetime begins when it is created and ends when it is destroyed.
Rust normally will implicitly understand how much a variable has to live.

an example will be a borrow inside a new scope:

```rust
fn main() {
    let borrow_me = '😀'; // <-- start of borrow_me lifetime

    {
        //new scope
        let borrowed = &borrow_me; // <-- start of borrowed lifetime

        println!("borrow1: {}", borrow1); // <-- end of borrowed lifetime
        // end of new scope
    }
    // <-- end of borrow_me lifetime
}
```

In other cases Rust can't infer the lifetime that we need. In those situations
we can explicitly annotate a lifetime using an apostrophe character as follows:

```rust
foo<'a>
// `foo` has a lifetime parameter `'a`
```

This lifetime syntax indicates that the lifetime of foo may not exceed that of 'a.
This will be generic lifetimes annotations, that describe the relationship between
the lifetimes of multiple references.

### Diferences between `Option` and `Result`

# Rocket Notes

### What are request handlers?

```rust
#[get("/")] // <-- any get request to the ("/") root will trigger
fn index() -> &'static str { // this handler
    "Hello, world!"
}

#[post("/login", data = "<user_form>")] // <-- any post request to "/login"
fn login(user_form: Form<UserLogin>) -> String { // will trigger this handler
    format!("Hello, {}!", user_form.name)
// in the case we are getting data ⤴
}
```
