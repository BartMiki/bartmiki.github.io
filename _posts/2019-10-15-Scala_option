---
layout: post
title: "Scala Option"
date: 2019-10-10
---
## Some theory
Options can hold only one element or none. To simply put they inform about possible absence of value. So why are they handy to use, if it only wraps value providing additional overhead? For long time we use null for informing about absence of value in languages like C# and Java and we were happy… unless we got a NullPointerException, when we tried to use a value, that happens to be actually null…

Ok, we can check for nulls, so no problem. C# even provides handy operators like: .? or ?? to improve null handling (if you want to know more about these C# operators, let me know in comment section).

But what if we want to return value that possible can return null. Or event better, what if the result is not-defined like dividing by zero? We can explain method in documentation, we can throw Exception or return null. But that will enforce consumer of our method to write boilerplate. This is not informative enough, the method definition should be more precise. That’s where Option come into play.

## Scala Option[+A]
Scala Option[+A] is an abstract class that informs us that type A is optional (don’t be bothered about + sign, it is not important for us now). Despite being abstract Option[+A] can be constructed using following syntax:

```scala
val myOpt1 = Option("My string value") // produces Some(My string value)
val myOpt2: Option[String] = None // produces None
val nullStr: String = null
val myOpt3 = Option(nullStr) // produces  None
val myOpt4 = Some(42) // produces Some(42)
```

Why when we creates Options, we get Some(…) or None?
Some(…) is sub type of Option, that holds actual value.
None is another sub type of Option, but it inform us that there is no value.

Now with some theory in mind we can write function for dividing two numbers using Option. We can write it like this:

```scala
def div(x: Int, y: Int): Option[Int] = {
  if (y == 0) None else Option(x / y)
}
```

Note that return type is Option[Int]. Therefore our method definition inform us, that this operation can return None. That way we created pure function – function that don’t have any side effects (don’t throw exception). Now let’s see results of using it:

```scala
val a = div(4,2)
val b = div(4,0)
println(a) // Prints: "Some(2)"
println(b) // Prints: "None"
```
We have beautiful results, but they are no longer simple Ints. There is no way to perform further operation on them, right?

## Performing operations on Option
That’s right that we can’t do a + 3, but we can treat Option as collection. (In fact it derives from Scala Product that returns iterator, that’s why we can iterate over Option). Since we can iterate over Option[+A], we can perform .map() operation. For now let’s create method that will do some calculations using .map() with lambda functions. This example shows, how well the Option fits into functional paradigm.

```scala
def calc(x: Option[Int]): Option[Int] = x
  .map(_ + 3) // add 3 to x
  .map(_ * 5) // multiply x by 5
  .map(_ - 11) // subtract 11 from x
val a2 = calc(a) // Some(14)
val b2 = calc(b) // None
```
We successfully perform operation on both values. For a it is clear. It contain number so it can be calculated. But what about None? It doesn’t have value! Why it worked? Well it simply worked, because None is treated like empty collection. No operation was performed. And this is core of this approach – we don’t care if it is Some or None, it simply works!

## Unpacking Option
Let’s define some consumer method. It will take Option as input and it will print output only, if it has value. We can use .foreach, but I will present two other approaches: for-loop and patter matching.

```scala
def printWithLoop(opt: Option[Int]): Unit = {
  for(value <- opt) println(s"Result: $value")
}
def printWithPatterMach(opt: Option[Int]): Unit = {
  opt match {
    case Some(value) => println(s"Result: $value")
    case None =>
  }
}
printWithLoop(a2) // Prints: "Result: 14"
printWithLoop(b2) // Prints nothing
printWithPatterMach(a2) // Prints: "Result: 14"
printWithPatterMach(b2) // Prints nothing
```
For-loop is straightforward, it iterate over collection, so it will run 0 times in case of None and exactly one time for Some.
Pattern Matching is more powerful. It matches incoming Option against Some(value), where value is immutable unpacked value held by Option. If this match fails, then match against None will be done. In this example it doesn’t do a thing, but it can for example log this event.

## When to use For loop to unpack Option

In case you have some sort of nested Option for-loop is better for handling them. Example bellow is purely synthetic to demonstrate the concept, but it is useful in real time scenario as well. For example configuration file with nested attributes that are optional can be unpacked this way very easily.

```scala
val nestedOption = Some(Some(Some("localhost:5000")))
for{
  opt1 <- nestedOption
  opt2 <- opt1
  address <- opt2
} println(s"Starting server at $address ...")
```
## When to use Pattern Matching to unpack Option

Use Patter Matching to the rest of use cases. It is simply most powerful tool for unpacking Options (and others types like `Try` or `Either`) and can be used to handle None options as well. Think about an example that database query returns None for given ID. That means there is no record with following ID. Thanks to Patter-Match we can act it we get None from database, like for example insert default record with not found ID.

## Other methods of extracting values

We have event shorter way of extracting value. As you see from above examples all methods have some purpose. The last option is to use `.getOrElse(…)` method. In order to work you need to provide default value. So it is only useful if you have default value. See example bellow:

```scala
val name = Some("Bart").getOrElse("Max")
val age = None.getOrElse(21)
println(s"$name is $age years old")
```

There is also another method called `.get`, but I highly discourage you to use it. Using it is not only defeating whole concept of using Option, but it will also throw exception when used on None. Use it only if you are 100% sure that you are using it on type Some.

If you want to check if Option contains value use .isDefined, and if you want to check for absence of value use `.isEmpty`

Thank you for reading!

*Bartosz Mikulski*
