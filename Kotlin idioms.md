
## Map nullable value if not null﻿

```
val value = ...

val mapped = value?.let { transformValue(it) } ?: defaultValue
// defaultValue is returned if the value or the transform result is null.
//...
```

## if expression﻿
```
val y = if (x == 1) {
    "one"
} else if (x == 2) {
    "two"
} else {
    "other"
}

```


## Single-expression functions﻿

```
fun theAnswer() = 42

```
## Call multiple methods on an object instance (with)﻿

```
class Turtle {
    fun penDown()
    fun penUp()
    fun turn(degrees: Double)
    fun forward(pixels: Double)
}

val myTurtle = Turtle()
with(myTurtle) { //draw a 100 pix square
    penDown()
    for (i in 1..4) {
        forward(100.0)
        turn(90.0)
    }
    penUp()
}
```

## Configure properties of an object (apply)﻿

```
val myRectangle = Rectangle().apply {
    length = 4
    breadth = 5
    color = 0xFAFAFA
}
```


## Java 7's try-with-resources﻿
```
val stream = Files.newInputStream(Paths.get("/some/file.txt"))
stream.buffered().reader().use { reader ->
    println(reader.readText())
}

```

## Generic function that requires the generic type information﻿

```
//  public final class Gson {
//     ...
//     public <T> T fromJson(JsonElement json, Class<T> classOfT) throws JsonSyntaxException {
//     ...

inline fun <reified T: Any> Gson.fromJson(json: JsonElement): T = this.fromJson(json, T::class.java)

```
## Nullable Boolean﻿
```
val b: Boolean? = ...
if (b == true) {
    ...
} else {
    // `b` is false or null
}

```

## Swap two variables﻿
```
var a = 1
var b = 2
a = b.also { b = a }

```
## Mark code as incomplete (TODO)﻿

```
fun calcTaxes(): BigDecimal = TODO("Waiting for feedback from accounting")
```

