# Unit 33: Logger

This note is inspired by [The Best Introduction to Monad](https://blog.jcoglan.com/2011/03/05/translation-from-haskell-to-javascript-of-selected-portions-of-the-best-introduction-to-monads-ive-ever-read/#), but is adapted to the OO-context with Java.

So far in the class, we have seen very general abstractions that support the `flatMap` operation.  But, it is not clear where this operation comes from, why is it fundamental, nor why is it useful.

In this unit, we are going to build a general abstraction step-by-step, get stuck at some point, and see how `flatMap` comes to our rescue, and hopefully, through this exercise, you will get some appreciation of `flatMap`.

Let's start with some methods that we wish to operate over `int`.  Let's use some trivial functions so that we don't get distracted by its details.

```
int incr(int x) {
  return x + 1;
}

int abs(int x) {
  return x > 0 ? x : -x;
}
```

These methods are pure functions without side effects, they take in one argument and produce a result. 

Just like mathematical functions, we can compose them together in arbitrary order to form more complex operations.

```
incr(abs(-4));
abs(incr(incr(5)));
```

But, suppose now we want to return not only an `int`, but some additional information related to the operation on `int`.  For instance, let's suppose we want to return a string describing the operation (for logging).  Java does not support returning multiple values, so let's returns a `Pair`.

```
Pair<Integer,String> incrWithLog(int x) {
  return Pair.of(incr(x), "incr " + x);
}

Pair<Integer,String> absWithLog(int x) {
  return Pair.of(abs(x), "abs " + x);
}
```

Now, we can't compose the methods as cleanly as before.

```
incrWithLog(absWithLog(-4));  // error
```

We will need to change our methods to take in `Pair<Integer,String>` as the argument.

```
Pair<Integer,String> incrWithLog(Pair<Integer,String> p) {
  return Pair.of(incr(p.first), p.second + " incr " + p.first);
}

Pair<Integer,String> absWithLog(Pair<Integer,String> p) {
  return Pair.of(abs(p.first), p.second + " abs " + p.first);
}
```

We can now compose the methods.
```
incrWithLog(absWithLog(Pair.of(-4, "")));  // error
```

Let's do it in a more OO-way, by writing a class to replace `Pair`.

```Java
// version 0.1
class Logger {
  private final int value;
  private final String log;

  private Logger(int value, String log) {
    this.value = value;
	this.log = log;
  }

  public static Logger of(int value) {
	return new Logger(value, "");
  }

  Logger incrWithLog() {
    return Logger.of(incr(this.value), this.log + "incr " + this.value + "; ");
  }

  Logger absWithLog() {
    return Logger.of(abs(this.value), this.log + "abs " + this.value + "; ");
  }

  String toString() {
    return "value: " + this.value + ", log: " + this.log;
  }
}
```

We can use the class above as:
```
Logger x = Logger.of(4);
Logger z = x.incrWithLog().absWithLog();
```

Note that we can now chain the methods together to compose them.  Additionally, the log messages get passed from one call to another and get "composed" as well.

## Making `Logger` general

There are many possible operations on `int`, and we do not want to add a method `fooWithLog` for every function `foo`.  One way to make `Logger` general is to abstract out the `int` operation and provide that as a lambda expression to `Logger`.  This is what the `map` method does. 

```
  Logger map(Transformer<Integer,Integer> transformer) {
	return Logger.of(transformer.transform(this.value), this.log); 
  }
```

We can use it like:
```
Logger.of(4).map(x -> incr(x)).map(x -> abs(x))
```

We can still chain the methods together to compose them.

But, `map` allows us to only apply the function to the value.  What should we do to the log messages?  Since the given lambda returns an int, it is not sufficient to tell us what message we want to add to the log.

To fix this, we will need to pass in a lambda expression that takes in an integer, but return us a pair of integer and a string, in other words, return us a `Logger`.  We call our new method `flatMap`.

```
  Logger flatMap(Transformer<Integer,Logger> transformer) {
    Logger logger = transformer.transform(this.value)
	return Logger.of(logger.value, logger.log + this.log); 
  }
```

By making `flatMap` takes in a lambda that returns a pair of integer and string, `Logger` can rely on these lambda to tell it how to update the log messages.  Now, if we have methods like this:

```
Logger incrWithLog(int x) {
  return Logger.of(incr(x), "incr " + x + "; ");
}

Logger absWithLog(int x) {
  return Logger.of(abs(x), "abs " + x + "; ");
}
```

We can write:
```
Logger.of(4)
      .flatMap(x -> incrWithLog(x))
      .flatMap(x -> absWithlog(x))
```

to now compose the methods `incr` and `abs` together, along with the log messages!

## Making Logger More General

We started with operation on `int`, but our `Logger` class is fairly general and should be able to add a log message to any operation of any type.  We can make it so by making `Logger` a generic class.

```Java
// version 0.2
class Logger<T> {
  private final T value;
  private final String log;

  private Logger(T value, String log) {
    this.value = value;
	this.log = log;
  }

  public static Logger of(T value) {
	return new Logger(value, "");
  }

  public <R> Logger<R> flatMap(Transformer<? super T, ? extends Logger<? extends R>> transformer) {
    Logger<R> logger = transformer.transform(this.value)
	return new Logger(logger.value, logger.log + this.log);
  }

  String toString() {
    return "value: " + this.value + ", log: " + this.log;
  }
}
```
