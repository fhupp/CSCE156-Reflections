# Question <TODO>

*What is the output of the following code*

```java
String message = "Computer Science II";
String s = message.substring(1, 5);
System.out.print(s);
```

The output of the above is `ompu`. 

This is because the Java `String.substring(n, m)` method returns a String the
chars whose indices in the original String were in the range `[n, m)`.

From this definition we end up with a very nice property, that being that the
length of the returned String is always `m - n`, provided the indices were
valid.

