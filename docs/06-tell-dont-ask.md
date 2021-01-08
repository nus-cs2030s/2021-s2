# Unit 6: Tell, Don't Ask

After taking this unit, students should:

- understand what accessor and mutator are used for, and why not to use them
- understand the principle of "Tell, Don't Ask"

## Accessors and Mutators

Similar to providing constructors, a class should also provide methods to retrieve or modify the properties of the object.  These methods are called the _accessor_ (or _getter_) or _mutator_ (or _setter_).

The use of accessor and mutator methods is a bit controversial.   Suppose that we provide an accessor method and a mutator method for every private field, then we are exposing the internal representation, therefore breaking the encapsulation.  For instance:

```Java
// Circle v0.4
class Circle {
	private double x;
	private double y;
	private double r;

	public Circle(double x, double y, double r) {
		this.x = x;
		this.y = y;
		this.r = r;
	}

	public double getX() {
		return this.x;
	}

	public void setX(double x) {
		this.x = x;
	}

	public double getY() {
		return this.y;
	}

	public void setY(double y) {
		this.y = y;
	}

	public double getR() {
		return this.r;
	}

	public void setR(double r) {
		this.r = r;
	}

	public double getArea() {
		return 3.141592653589793 * this.r * this.r;
	};
}
```

## The "Tell Don't Ask" Principle

The mutators and accessors above are pretty pointless.  If we need to know the internal and do something with it, then we are breaking the abstraction barrier.  The right approach is to implement a method within the class that does whatever we want the class to do.   For instance, suppose that we want to check if a given point (x,y) calls within the circle, one approach would be:

```Java
   double cX = c.getX();
   double cY = c.getY();
   double r = c.getR();
   boolean isInCircle = ((x - cX) * (x - cX) + (y - cY) * (y - cY)) <= r * r;
```

where `c` is a `Circle` object.

A better approach would be to add a new `boolean` method in the `Circle` class, and call it instead:
```Java
   boolean isInCircle = c.contain(x, y);
```

The better approach involves writing a few more lines of code to implement the method, but it keeps the encapsulation intact.  If one fine day, the implementer of `Circle` decided to change the representation of the circle and remove the direct accessors to the fields, then only the implementer needs to change the implementation of `contain`.  The client does not have to change anything.  

The principle around which we can think about this is the "Tell, Don't Ask" principle.  The client should tell a `Circle` object what to do (compute the circumference), instead of asking "what is your radius?" to get the value of a field then perform the computation on the object's behalf.

While there are situations where we can't avoid using accessor or modifier in a class, for beginner OO programmers like yourself, it is better to not define classes with any accessor and modifier to the private fields, and forces yourselves to think in the OO way -- to tell an object what task to perform as a client, and then implement this task within the class as a method as the implementer.

## Further Reading

- [Tell Don't Ask](https://martinfowler.com/bliki/TellDontAsk.html) by Martin Fowler
- [Why getters and setters are evil](https://www.infoworld.com/article/2073723/why-getter-and-setter-methods-are-evil.html), by Allen Holub, JavaWorld
- [Getters and setters are evil. Period](https://www.yegor256.com/2014/09/16/getters-and-setters-are-evil.html), by Yegor Bygayenko.
