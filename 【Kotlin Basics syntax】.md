
## Package definition and imports﻿

```
package my.demo

import kotlin.text.*
//...
```

## Print to the standard output

```
println("Hello world!")
println(42
```

## Functions﻿

```

fun sum(a: Int, b: Int): Int {
    return a + b
}

fun sum(a: Int, b: Int) = a + b

fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}


```

## Variables﻿

Read-only local variables are defined using the keyword val. They can be assigned a value only once.


Variables that can be reassigned use the var keyword.


You can declare variables at the top level.


## Creating classes and instances﻿

```
class Rectangle(var height: Double, var length: Double) {
    var perimeter = (height + length) * 2
}

val rectangle = Rectangle(5.0, 2.0)
println("The perimeter is ${rectangle.perimeter}")
```

Inheritance between classes is declared by a colon （:）. Classes are final by default; to make a class inheritable, mark it as open.

## Comments﻿

```
// This is an end-of-line comment

/* This is a block comment
   on multiple lines. */
   
/* The comment starts here
/* contains a nested comment *⁠/
and ends here. */

```

## String templates

```
var a = 1
// simple name in template:
val s1 = "a is $a" 

a = 2
// arbitrary expression in template:
val s2 = "${s1.replace("is", "was")}, but now is $a"
```

## Conditional expressions﻿

```
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```

## for loop

```
val items = listOf("apple", "banana", "kiwifruit")
for (item in items) {
    println(item)
}
val items = listOf("apple", "banana", "kiwifruit")
for (index in items.indices) {
    println("item at $index is ${items[index]}")
}
```


## while loop
```
val items = listOf("apple", "banana", "kiwifruit")
var index = 0
while (index < items.size) {
    println("item at $index is ${items[index]}")
    index++
}
```

## when expression﻿

```
fun describe(obj: Any): String =
    when (obj) {
        1          -> "One"
        "Hello"    -> "Greeting"
        is Long    -> "Long"
        !is String -> "Not a string"
        else       -> "Unknown"
    }
```

## Ranges

Check if a number is within a range using in operator.
```
val x = 9
val y = 9
if (x in 1..y) {
    println("fits in range")
}

```

```
val list = listOf("a", "b", "c")

if (-1 !in 0..list.lastIndex) {
    println("-1 is out of range")
}
if (list.size !in list.indices) {
    println("list size is out of valid list indices range, too")
}
```

over a progeression.

```
for (x in 1..10 step 2) {
    print(x)
}
println()
for (x in 9 downTo 0 step 3) {
    print(x)
}
```

## Collections

Check if a collection contains an object using in operator.

```
when {
    "orange" in items -> println("juicy")
    "apple" in items -> println("apple is fine too")
}
```

## Nullable values and null checks﻿

```
// ...
if (x == null) {
    println("Wrong number format in arg1: '$arg1'")
    return
}
if (y == null) {
    println("Wrong number format in arg2: '$arg2'")
    return
}

// x and y are automatically cast to non-nullable after null check

```

## Type checks and automatic casts﻿
The is operator checks if an expression is an instance of a type. If an immutable local variable or property is checked for a specific type, there's no need to cast it explicitly:


```
fun getStringLength(obj: Any): Int? {
    if (obj is String) {
        // `obj` is automatically cast to `String` in this branch
        return obj.length
    }

    // `obj` is still of type `Any` outside of the type-checked branch
    return null
}
```


