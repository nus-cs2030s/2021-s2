# Unit 13: Liskov Substitution Principle

After this unit, the student should:

- understand the type of bugs that reckless developers can introduce when using inheritance and polymorphism
- understand the Liskov Substitution Principle and thus be aware that not all IS-A relationships should be modeled with inheritance
- know how to explicitly disallow inheritance when writing a class or disallow overriding with the `final` keyword

## The Responsibility When Using Inheritance

As you have seen in [Unit 12](12-polymorphism), polymorphism is a powerful tool that allows a client to change the behavior of existing code written by the implementer, behind the abstraction barrier.

As Ben Parker (aka Uncle Ben) said, "With great power, comes great responsibility."   The client must use overriding and inheritance carefully.  Since they can affect how existing code behaves, they can easily break existing code and introduce bugs.  Since the client may not have access to the existing code behind the abstraction barrier, it is often tricky to trace and debug.  Furthermore, the implementer would not appreciate it if their code was working perfectly until one day, someone overriding a method causes their code to fail, even without the implementer changing anything in their code.

Ensuring this responsibility cannot be done by the compiler, unfortunately.  
It thus becomes a developer's responsibility to ensure that any inheritance with method overriding does not introduce bugs to existing code.  This brings us to the _Liskov Substitution Principle_ (LSP), which says: "Let $\phi(x)$ be a property provable about objects $x$ of type $T$. Then $\phi(y)$ should be true for objects $y$ of type $S$ where $S <: T$."   

This is consistent with the definition of subtyping, $S <: T$, but spelled out more formally.

Let's consider the following example method, `Module::marksToGrade`, which takes in the marks of a student and returns the grade 'A', 'B', 'C', or 'F' as a `char`.  How `Module::marksToGrade` is implemented is not important.  Let's look at how it is used.

```Java
void displayGrade(Module m, double marks) {
  char grade = m.marksToGrade(marks);
  if (grade == 'A')) {
	  System.out.println("well done");
  else if (grade == 'B') {
	  System.out.println("good");
  else if (grade == 'C') {
	  System.out.println("ok");
  } else {
	  System.out.println("retake again");
  }
}
```

Now, suppose that one day, someone comes along and create a new class `CSCUModule` that inherits from `Module`, and overrides `marksToGrade`
s that it now returns only 'S' and 'U'.  Since `CSCUModule` is a subclass of `Module`, we can pass an instance to `displayGrade`:

```
displayGrade(new CSCUModule("GEQ1000", 100));
```

and suddenly `displayGrade` is displaying `retake again` even if the student is scoring 100 marks.

We are violating the LSP here.  The object `m` has the following property: `m.marksToGrade` always returns something from the set { `'A'`, `'B'`, `'C'`, `'F'` }, that the method `displayGrade` depends on explicitly.  The subclass `CSCUModule` violated that and makes `m.marksToGrade` returns `'S'` or `'U'`, sabotaging `displayGrade` and causing it to fail.

LSP cannot be enforced by the compiler[^1]. The properties of an object have to be managed and agreed upon among programmers.  A common way is to document these properties as part of the code documentation.

## Preventing Inheritance and Method Overriding

Sometimes, it is useful for a developer to explicitly prevent a class to be inherited.  Not allowing inheritance would make it much easier to argue for the correctness of programs, something that is important when it comes to writing secure programs.  Both the two java classes you have seen, `java.lang.Math` and `java.lang.String`, cannot be inherited from.  In Java, we use the keyword `final` when declaring a class to tell Java that we ban this class from being inherited.

```Java
final class Circle {
	:
}
```

Alternatively, we can allow inheritance but still prevent a specific method from being overridden, by declaring a method as `final`.  Usually, we do this on methods that are critical for the correctness of the class.

For instance,
```Java
class Circle {
   :
  final public boolean contains(Point p) {
	:
  }
}
```

[^1]: We can use `assert` to check some of the properties though.
