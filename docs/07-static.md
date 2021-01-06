# Unit 7: Class Instance and Methods

## Class Fields and Methods

Let's look at the implementation of `getArea()` above.  We use the constant $\pi$ but hardcoded it as 3.1415926.  Hardcoding such a magic number is a _no-no_ in terms of coding style.  This constant can appear in more than one places. If we hardcode such a number and want to change its precision later, we would need to trace down and change every occurrence.  Every time we need to use $\pi$, we have to remember or look up what is the precision that we use.  Not only does this practice introduce more work, it is also likely to introduce bugs.  

In C, we define $\pi$ as a macro constant `M_PI`.  But how should we do this in Java?  This is where the ideal that a program consists of only objects with internal states that communicate with each other feel a bit constraining.  The constant $\pi$ is universal, and does not really belong to any object (the value of $\pi$ is the same for every circle!).  Another example: if we define a method `sqrt()` that computes the square root of a given number, this is a general function that is not associated with any object as well.

A solution to this is to associate these _global_ values and functions with a _class_ instead of with an _object_.  For instance. Java predefines a [`Math`](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html) class[^7] that is populated with constants `PI` and `E` (for Euler's number $e$), along with a long list of mathematical functions.  To associate a method or a field with a class in Java, we declare them with the `static` keyword.  We can additionally add a keyword `final` to indicate that the value of the field will not change[^8].

[^7]: The class `Math` is provided by the package `java.lang` in Java.  A package is simply a set of related classes (and interfaces, but I have not told you what is an interface yet).  To use this class, we need to add the line `import java.lang.Math` at the beginning of our program.

[^8]: A `final` method refers to a method that cannot be _overridden_.  Do not worry for now if you don't understand what overriding a method means.

```Java
class Math {
  :
  public static final double PI = 3.141592653589793;
  :
  :
}
```

We call these fields and methods that are associated with a class as _class fields_ and _class methods_, and fields and methods that are associated with an object as _instance fields_ and _instance methods_.

!!! note "Class Fields and Methods in Python"
    Note that, in Python, any variable declared within a `class` block is a class field:
    ```Python
    class Circle:
      x = 0
      y = 0
    ```
    In the above example, `x` and `y` are class fields, not instance fields.

## Example: The Circle class

Now, let revise our `Circle` class to improve the code and make it a little more complete:

```Java
import java.lang.Math;

/**
 * A Circle object encapsulates a circle on a 2D plane.  
 */
class Circle {
  private double x;  // x-coordinate of the center
  private double y;  // y-coordinate of the center
  private double r;  // the length of the radius

  /**
   * Create a circle centered on (centerX, centerY) with given radius
  */
  public Circle(double centerX, double centerY, double radius) {
    x = centerX;
    y = centerY;
    r = radius;
  }

  /**
   * Return the area of the circle.
   */
  public double getArea() {
    return Math.PI*r*r;
  }

  /**
   * Return the circumference of the circle.
   */
  public double getCircumference() {
    return Math.PI*2*r;
  }

  /**
   * Move the center of the circle to the new position (newX, newY)
   */
  public void moveTo(double newX, double newY) {
    x = newX;
    y = newY;
  }

  /**
   * Return true if the given point (testX, testY) is within the circle.
   */
  public boolean contains(double testX, double testY) {
    return false;
    // TODO: left as an exercise  
  }
}
```

## Creating and Interacting with `Circle` objects

To use the `Circle` class, we can either:

* create a `main()` function, compile and link with the `Circle` class, and create an executable program, just like we usually do with a C program, OR
* use `jshell`, which is part of Java 9 (but not earlier versions). `jshell` provides a _read-evaluate-print loop_ (REPL) to help us quickly try out various features of Java.

We will write a complete Java program with `main()` later in this class, but for now, we will use `jshell` to demonstrate the various language features of Java[^8].

[^8]: You can download and install `jshell` yourself, as part of [Java Development Kit version 9 (JDK 9)](http://www.oracle.com/technetwork/java/javase/downloads/jdk9-downloads-3848520.html)

The demonstration below loads the `Circle` class written above (with the `contains` method completed) from a file named `Circle.java`[^9], and creates two `Circle` objects, `c1` and `c2`.  We use the `new` keyword to tell Java to create an object of type `Circle` here, passing in the center and the radius.

```Java
Circle c1 = new Circle(0, 0, 100);
```

[^9]: We use the convention of one public class per file, name the file with the exact name of the class (including capitalization), and include the extension `.java` to the filename.

<script type="text/javascript" src="https://asciinema.org/a/132906.js" id="asciicast-132906" async></script>

## Reference Type vs. Primitive Type

The variable `c1` actually stores an abstraction over a _reference_ to the Circle object, instead of the object itself.
_All objects are stored as references in Java_.

The other variable type supported in Java is _primitive_ type.  A variable of primitive type stores the _value_ instead of a reference to the value.
Java supports eight _primitive_ data types: `byte`, `short`, `int`, `long`, `float`, `double`, `boolean` and `char`.  If you are familiar with C, these data types should not be foreign to you.  One important difference is that a `char` variable stores a 16-bit Unicode character, not an 8-bit character like in C.  Java uses the type `byte` for that.  The other notable difference is that Java defines `true` and `false` as possible value to a `boolean`, unlike C which uses `0` for false and non-`0` for true.  

You can read all about Java [variables](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html) and [primitive data types](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html) in Oracle's Java Tutorial.

## Type Safety

Some languages are stricter in terms of type "compatibility" than others.  C compilers, however, are not very strict.  If it detects something strange with the type you used, it will issue a warning, but still let your code compiles and runs.

Take:

```C
#include <stdio.h>
int main()
{
	printf("%d\n", "cs2030");
}
```

In Line 4, we treat the address to a string as integer.  This generates a compiler's warning.

In C, you can _type cast_ a variable from one type into another, i.e., force the compiler to treat a variable of one type as another type.  The compiler would listen and do that for you.  The following code would print out gibberish and would compile perfectly without error.

```C
#include <stdio.h>
int main()
{
	printf("%d\n", (int)"cs2030");
}
```

Such flexibility and loose rules for type compatibility could be useful, if you know what you are doing, but for most programmers, it could be a major source of unintentional bugs, especially if one does not pay attention to compiler's warning or one forces the warning to go away without fully understanding what is going on.

Java is very strict when it comes to type checking, and is one of the _type-safe_ languages. Java ensures that basic operations (such as `+`, `-`, etc) and method calls apply to values in a way that makes sense.  If you try to pull the same trick as above, you will receive an error:

<script type="text/javascript" src="https://asciinema.org/a/133995.js" id="asciicast-133995" async></script>

## Exercise

1.  In the example above, we implemented a class `Circle`.  There, we store and pass around two `double` variables that correspond to the x-coordinate and y-coordinate of a point.  The code would be neater if we create a second class `Point` that encapsulates the concept of a point on a 2D plane and the operations on points.

    Implement a new class `Point` and modify the class `Circle` to use the class `Point`.  Pay attention to what methods and fields (if any) you expose as `public` outside of the abstraction barrier of a `Point` object.

    You will need to use `jshell` from Java 1.9 (or JDK 9) to interact with your new classes.

2.  Use `jshell` to try out the following.

    ```Java
    class A {
      public static int x = 1;
      public int y = 5;

      void incrX() {
        x = x + 1;
      }

      void incrY() {
        y = y + 1;
      }
    }

    A a1 = new A();
    A a2 = new A();
    ```

    After executing `a1.x = 10`, what is the value of `a2.x`?

    After executing `a1.y = 10`, what is the value of `a2.y`?

    Is `A.x = 3` a valid statement?  Is `A.y = 3` a valid statement?  

    Note: Even though `a1.x` is valid, it is considered a bad programming practice to access a class field through an instance variable (e.g., `a1.x`).  The proper way to do it is to use the class name `A.x`).

3.  Consider the following two classes:

    ```Java
    class A {
      private int x;
      public void changeSelf() {
        x = 1;
      }
      public void changeAnother(A a) {
        a.x = 1;
      }

    }

    class B {
      public void changeAnother(A a) {
        a.x = 1;
      }
    }
    ```

    Which line(s) above violate the `private` access modifier of `x`?

