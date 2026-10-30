# Question 16
*What is the output of the following program? Assuming that each class is in its
corresponding file and all classes are in the same package.*

```java
public class Main {
 public static void main(String[] args) {
      A a1 = new A();
      A a2 = new B();
      B b2 = (B)a2;
      B b3 = new B();
      A a3 = b3;
      a3.instancePrint();
      b3.instancePrint();
 }
}

public class A {
 private int instanceAVar;
 private static int staticAVar = 3;
 public A() {
   instanceAVar = 3;
   staticAVar++;
 }
 public void instancePrint() {
   System.out.print(instanceAVar);
 }
 public static void staticPrint() {
   System.out.print(staticAVar); 
 }
}

public class B extends A {
 private int instanceBVar;
 private static int staticBVar = 1;
 public B() {
   instanceBVar = 4;
   staticBVar++;
 }
 @Override
 public void instancePrint() {
   System.out.print(instanceBVar);
 }
 public static void staticPrint() {
   System.out.print(staticBVar);
 }
}
```

To produce our answer we need to know 2 things, Java's call resolution and Java's handling of static / instance vars.

We will approach this by going in reverse, what we are concerned about is the return values of `a3.instancePrint()` and `b3.instancePrint()`, notably both `a3` and `b3` point at the same object, `a3` merely says that what it points to is at least an `A` whilst `b3` says that whatever it points to is at least a `B` (considering inheritance to be a partial order). This does not change resolution of instance variables and instance methods, so the results of `a3.instancePrint()` should be the same as `b3.instancePrint()`. We can see that the only code in `main` that matters is 

```java
B b3 = new B();
A a3 = b3;
a3.instancePrint();
b3.instancePrint();
```

And the only methods we call out from `main` are `instancePrint` for `B` and `new B()`. Upon review, we know that `b3.instanceVar = 4`, and thus `instancePrint` prints `4`. Therefore, the output is `44`.
