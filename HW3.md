# Question 14

*What is the final value of `y`*

```java
int[] array1 = {71, 2, 3};
int[] array2;
array2 = array1;
array2[0] = 158;
int y = array1[0];
```

The final value of `y` is 158

When `array1` gets initialized it loads something similar to the following onto
the heap, and gives us a pointer / reference to it.

 0 | 1 | 2
 - | - | -
71 | 2 | 3

So when `array2` is set to `array1`, we merely are copying the pointer rather
than the underlying data. So by modifying `array2` we are modifying the
underlying memory owned by both `array1` and `array2`, which means the read into
`y` is that of the changed index of our array.

## Aside on Rust

I'm including this as I think the way this works in Rust is very neat by comparison.

First let's try a direct translation of Java to Rust.

[Rust Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=04c82d729243f3f502cc98b79c892fac)

```rust
let array1: [isize; 3] = [71, 2, 3];
let array2: [isize; 3];
array2 = array1;
// [E0594]: cannot assign to `array2[_]`, as `array2` is not declared as mutable
array2[0] = 158;
let y: isize = array1[0];
```

This immediately fails to compile as Rust's variables are immutable by default,
which isn't the case in Java. Annotating `array2` with `mut` to make our code
compile ends we have the following 

[Rust Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=104f79a2ea7d117d3a9d2bf0a680bfd7)

```rust
let array1: [isize; 3] = [71, 2, 3];
let mut array2: [isize; 3];
array2 = array1;
array2[0] = 158;
let y: isize = array1[0];
```

Unlike Java though we end with `y = 71`. This is because when the line `array2 =
array1;` is run Rust doesn't copy the pointer to `array1` into `array2` as it
did in Java. It instead copied the contents of `array1` as `isize` (and `[isize;
N]` by extension) implements [`Copy`](https://doc.rust-lang.org/std/marker/trait.Copy.html). 
Additionally, the [array](https://doc.rust-lang.org/std/primitive.array.html)
type we are using here is statically sized and stack allocated.

To get around all of the above we will try this again, but using a heap allocated type, [`Vec`](https://doc.rust-lang.org/std/vec/struct.Vec.html).

[Rust Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=a19ffdf3eb754634736cba6aaee29403)

```rust
let array1: Vec<isize> = vec![71, 2, 3];
let mut array2: Vec<isize>;
array2 = array1;
array2[0] = 158;
let y: isize = array1[0];
```

This code fails to compile, with the following error

```rust
// error[E0382]: borrow of moved value: `array1`

let array1: Vec<isize> = vec![71, 2, 3];
//  ------ move occurs because `array1` has type `Vec<isize>`, which does not implement the `Copy` trait
let mut array2: Vec<isize>;
array2 = array1;
//       ------ value moved here
array2[0] = 158;
let y: isize = array1[0];
//             ^^^^^^ value borrowed here after move
```

And now, as with all talks about Rust, the borrow checker will be talked about.

If this is your first time hearing of the borrow checker the *very* basic idea
is to ensure that we handle memory and ownership in such a way that we can't
have undefined behavior. This is enforced through "borrowing" pointers, known as
references. References can be either immutable or mutable, and for any variable
we can either have as many immutable references as we want (providing lifetimes
allow it), *or* one mutable reference.

As a small aside within an aside of an aside of this reflection; by default Rust
types are [affine](https://en.wikipedia.org/wiki/Substructural_type_system), but
when they implement `Copy` they are "weakened" to normal types (Additionally,
affine types in rust can be *sort of* strengthened into linear types).

Either way, if we were to use references to recreate the behavior of Java we end up with this

[Rust Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=7f26bb013a19acbc4674333ecd6410ba)

```rust
// Note the swapped mutablilty
let mut array1: Vec<isize> = vec![71, 2, 3];
let array2: &mut Vec<isize>;
// Here we set array2 to be a mutable reference to array1
array2 = &mut array1;
array2[0] = 158;
let y: isize = array1[0];
```

Finally, `y = 158`. 

There were quite a few things I brushed over, especially to do with the borrow
checker and references. For example, Rust has "normal" pointers too, but to
explain how to use them requires covering `unsafe` Rust, which is just a pain to
work with.
