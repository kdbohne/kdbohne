---
layout: single
author_profile: true
permalink: /klang

classes: wide

title: K Programming Language

header:
  overlay_image: /assets/images/klang.png
  overlay_filter: "0.5"
---

{% comment %}
HACK: Override the default CSS to change the height of the header.
TODO: is there a better way of doing this?
{% endcomment %}
<style type="text/css">
.page__hero--overlay { height: 400px; background-position: 0 10%; }
</style>

<div class="no_margin_top">
<h2>Overview</h2>
</div>

*K* is an experimental low-level programming language compiled by [klang](https://github.com/kdbohne/klang).
It features static type checking, compile time code execution, polymorphic functions, and basic modules with a
[Rust](https://www.rust-lang.org)-inspired syntax. The compiler can either generate simple C code or leverage [LLVM](https://llvm.org)
for more optimized code generation. Basic test cases and examples of *K* programs can be found in
the [test directory](https://github.com/kdbohne/klang/tree/master/test) of the `klang` repository.

## Polymorphic Functions

Function parameters can be specified as generic types, represented here by the letter `T`:

```rust
fn add(a $T, b T) -> T {
    a + b
}
```

At each of the function's call sites, the compiler generates or reuses a specialized definition of
the generic function. For example, adding two 64-bit integers (i64) together looks like this:

```rust
let x = add(3, 4);
let y = add(x, 5);
```

The specialized function that is generated for this case:

```rust
fn add(a i64, b i64) -> i64 {
    a + b
}
```

Attempting to use two different types for this example function results in an error:

```rust
let z = add(y, 3.14); // Error: types y (i64), 3.14 (f64) do not match
```

## Modules

Types and functions can be defined inside **modules**, which are similar to namespaces in C++. For
example, consider a type `Thing` defined inside module `foo`:

```rust
module foo {
    struct Thing {
        x i64;
        y i64;
    };

    fn make_thing(x i64, y i64) -> Thing {
        let t Thing;
        t.x = x;
        t.y = y;
        t
    }

    fn do_something(t *Thing) -> i64 {
        t.x + t.y
    }
}
```

The `foo` module can be imported and used by external code:

```rust
import "foo.k";

fn main() {
    let t0 foo::Thing;
    t0.x = 3;
    t0.y = 5;

    let t1 = foo::make_thing(4, 8);

    let x = foo::do_something(&t0); // x is 8
    let y = foo::do_something(&t1); // y is 12
}
```

## OpenGL Bindings

*K* provides OpenGL bindings, which are defined and loaded into the `gl` module in
[gl.k](https://github.com/kdbohne/klang/blob/master/lib/gl.k). An example usage looks something
like this:

```rust
import "gl.k";

fn main() {
    // Create the OpenGL context and OS window
    let window = gl::create_window("Title", 1280, 720);

    // Load OpenGL function pointers
    gl::load_funcs();

    // Initialize and clear input state
    let input Input;
    for i in 0..256 {
        input.keys[i] = 0;
    };

    gl::ClearColor(1.0, 0.0, 1.0, 1.0);

    // Main loop
    loop {
        gl::poll_events(window, &input);

        // Handle events, simulate...

        gl::Clear(gl::COLOR_BUFFER_BIT);
        // Draw things...
        gl::swap_buffers(window);
    };

    gl::destroy_window(window);
}
```
