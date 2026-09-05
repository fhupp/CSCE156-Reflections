# Question 7 & 8

*What is the final value of `vi`? And what is the final value of `vp`?*

```java
int vi = 0;
int vp = ++vi;
```

Since this is the result of the preincrement opperator, we know that `vi` is increased in place and the expression `++vi` will evaluate to the increased value of `vi`. Therefore `vi = 1` and `vp = 1`.

# Question 15

*Evaluating a Ternary Expression*

```java
int x = 10;
int y = (x >= 10) ? 1 : 0;
```

Ternary expressions in Java `P ? T : F`, where `P` is a `bool`, evaluate to `T` if `P` and `F` otherwise. So in our case, where `P = (x >= 10)`, `T = 1`, and `F = 0`, we first evaluate `P` to be `true` and therefore evaluate `(x >= 10) ? 1 : 0` to `1`.
