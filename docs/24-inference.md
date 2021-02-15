# Unit 24: Type Inference

After this unit, students should:

- be familiar how Java infers missing type arguments


We have seen in the past units the importance of types in preventing run-time errors.  Utilizing types properly can help programmers catch type mismatch errors that could have caused a program to fail during run-time, possibly after it is released and shipped.

By including type information everywhere in the code, we make the code explicit in communicating the intention of the programmers to the readers.  Although it makes the code more verbose and cluttered -- it is a small price to pay for ensuring the type correctness of the code and reducing the likelihood of bugs as the code complexity increases.

Java, however, allows the programmer to skip some of the type annotations and try to infer the type argument of a generic method and a generic type, through the _type inference_ process.

The basic idea of type inference is simple: Java will looking among the matching types that would lead to successful type checks, and pick the most specific ones.

## Diamond Operator

One example of type inference is the diamond operator `<>` when we `new` an instance of a generic type:
```Java
Pair<String,Integer> p = new Pair<>();
```

Java can infer that `p` should be an instance of `Pair<String,Integer>` since the compile-time type of `p` is `Pair<String,Integer>`.  The line above is equivalent to:
```Java
Pair<String,Integer> p = new Pair<String,Integer>();
```

## Type Inferencing

We have been invoking 
```Java
class A {
  // version 0.7 (with wild cards array)
  public static <S> boolean contains(Array<? extends S> array, S obj) {
    for (int i = 0; i < array.getLength(); i++) {
      S curr = array.get(i);
      if (curr.equals(obj)) {
        return true;
      }
    }
    return false;
  }
}
```

by explicitly passing in the type argument `Shape` (also called _type witness_ in the context of type inference).
```Java
     A.<Shape>contains(circleArray, shape);
```

We could remove the type argument `<Shape>` so that we can call `contains` just like a non-generic method:
```Java
     A.contains(circleArray, shape);
```

and Java could still infer that `S` should be `Shape`.  The type inference process looks for all possible types that match.  In this example, the type of the two parameters must match.  Let's consider each individually first:

- An object of type `Shape` is passed as an argument to the parameter `obj`.  So `S` might be `Shape` or, if widening type conversion has occurred, one of the other supertypes of `Shape`.
- An `Array<Circle>` has been passed into `Array<? extends S>`.  A widening type conversion occurred here, so we need to find all possible `S` such that `Array<Circle>` <: `Array<? extends S>`.  This is true only if `S` is `Circle`, or another supertype of `Circle`.

Intersecting the two lists, we know that `S` could be `Shape` or one of its supertypes: `GetAreable` and `Object`.   The most specific type among these is `Shape`.  So, `S` is inferred to be `Shape`.

Type inferencing can have unexpected consequences.  Let's consider an [older version of `contains` that we wrote](20-generics.md):

```Java
class A {
    // version 0.4 (with generics)
    public static <T> boolean contains(T[] array, T obj) {
      for (T curr : array) {
        if (curr.equals(obj)) {
          return true;
        }
      }
      return false;
    }
}
```

Recall that we want to prevent nonsensical calls where we are searching for an integer in an array of strings.
```Java
String[] strArray = new String[] { "hello", "world" };
A.<String>contains(strArray, 123); // type mismatch error
```

But, if we write:
```Java
A.contains(strArray, 123); // ok!  (huh?)
```

The code compiles!  Let's go through the type inferencing steps to understand what happened.  Again, we have two parameters:

- `strArray` has the type `String[]` and is passed to `T[]`.  So `T` must be `String` or its superclass `Object`.  The latter is possible since Java array is covariant.
- `123` is passed as type `T`.  The value is treated as `Integer` and, therefore, `T` must be either `Integer` or its superclass `Object`. 

The intersection between the two is the type `Object`, which is also the most specific type.  So, Java infers `T` to be `Object`.  The code above is equivalent to:

```Java
A.<Object>contains(strArray, 123);
```

And our version 0.4 of `contains` actually is quite fragile and does not work as intended.  We were bitten by the fact that the Java array is covariant, again.

## Target Typing

The example above performs type inferencing on the parameters of the generic methods.  Type inferencing can involve the type of the expression as well.  This is known as _target typing_.  Take the following upgraded version of `findLargest`:

```Java
// version 0.6 (with Array<T>)
public static <T extends GetAreable> T findLargest(Array<? extends T> array) {
  double maxArea = 0;
  T maxObj = null;
  for (int i = 0; i < array.getLength(); i++) {
    T curr = array.get(i);
    double area = curr.getArea();
    if (area > maxArea) {
      maxArea = area;
      maxObj = curr;
    }
  }
  return maxObj;
}
```

and we call
```
Shape o = A.findLargest(new Array<Circle>(0));
```

We have a few more constraints to check:

- Due to target typing, the returning type of `T` must be a subtype of `Shape` (including `Shape`)
- Due to the bound of the type parameter, `T` must be a subtype of `GetAreable` (including `GetAreable`)
- `Array<Circle>` must be a subtype of `Array<? extends T>`, so `T` must be a supertype of `Circle` (including `Circle`)

Intersecting all these possibilities, only two possibilities emerge: `Shape` and `Circle`.  The most specific one is `Circle`, so the call above is equivalent to:
```
Shape o = A.<Circle>findLargest(new Array<Circle>(0));
```
