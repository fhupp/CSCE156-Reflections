# Question 6

*For the following code, if a user enters **Foo Bar Baz**, what is the output?*

```java
Scanner input = new Scanner(System.in);
String s = input.next();
System.out.print(s);
```

The implementation of `Scanner.next( )` splits `String`s at spaces which means that it will return Foo.
