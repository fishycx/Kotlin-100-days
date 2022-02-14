@[TOC](Kotlin Example)
# Introduction
## Hello Word
Kotlin code is usually defined in packages. Package specification is optional: If you don't specify a package in a source file, its content goes to the default package.
An entry point to a Kotlin application is the main function. Since Kotlin 1.3, you can declare main without any parameters. The return type is not specified, which means that the function returns nothing.
println writes a line to the standard output. It is imported implicitly. Also note that semicolons are optional.

## Functions
### Infix Functions
Member functions and extensions with a single parameter can be turned into infix functions.
```
fun main() {

  infix fun Int.times(str: String) = str.repeat(this)        // 1
  println(2 times "Bye ")                                    // 2

  val pair = "Ferrari" to "Katrina"                          // 3
  println(pair)

  infix fun String.onto(other: String) = Pair(this, other)   // 4
  val myPair = "McLaren" onto "Lucas"
  println(myPair)

  val sophia = Person("Sophia")
  val claudia = Person("Claudia")
  sophia likes claudia                                       // 5
}

class Person(val name: String) {
  val likedPeople = mutableListOf<Person>()
  infix fun likes(other: Person) { likedPeople.add(other) }  // 6
}

```

 1. Defines an infix extension function on Int.
 2. Calls the infix function.
 3. Creates a Pair by calling the infix function to from the standard
    library.
 4. Here's your own implementation of to creatively called onto.
 5. Infix notation also works on members functions (methods).
 6. The containing class becomes the first parameter.

### Operator Functions
Certain functions can be **upgraded** to operators, allowing their calls with the corresponding operator symbol.
### Functions with vararg Parameters
Varargs allow you to pass any number of arguments by separating them with commas.

## Varibales
You're free to choose when you initialize a variable, however, it must be initialized before the first read.

## Null Safety
In an effort to rid the world of NullPointerException, variable types in Kotlin don't allow the assignment of null. If you need a variable that can be null, declare it nullable by adding ? at the end of its type.
```
fun describeString(maybeString: String?): String {              // 1
    if (maybeString != null && maybeString.length > 0) {        // 2
        return "String of length ${maybeString.length}"
    } else {
        return "Empty or null string"                           // 3
    }
}
```
## Classes
The class declaration consists of the class name, the class header (specifying its type parameters, the primary constructor etc.) and the class body, surrounded by curly braces. Both the header and the body are optional; if the class has no body, curly braces can be omitted.

## Generics
```
class MutableStack<E>(vararg items: E) {              // 1

  private val elements = items.toMutableList()

  fun push(element: E) = elements.add(element)        // 2

  fun peek(): E = elements.last()                     // 3

  fun pop(): E = elements.removeAt(elements.size - 1)

  fun isEmpty() = elements.isEmpty()

  fun size() = elements.size

  override fun toString() = "MutableStack(${elements.joinToString()})"
}
```
## Inheritance
Kotlin fully supports the traditional object-oriented inheritance mechanism.
**Passing Constructor Arguments to Superclass**
```
open class Lion(val name: String, val origin: String) {
    fun sayHello() {
        println("$name, the lion from $origin says: graoh!")
    }
}

class Asiatic(name: String) : Lion(name = name, origin = "India") // 1

fun main() {
    val lion: Lion = Asiatic("Rufo")                              // 2
    lion.sayHello()
}
```

# Control Flow
## When
### When Statement
```
fun main() {
    cases("Hello")
    cases(1)
    cases(0L)
    cases(MyClass())
    cases("hello")
}

fun cases(obj: Any) {                                
    when (obj) {                                     // 1   
        1 -> println("One")                          // 2
        "Hello" -> println("Greeting")               // 3
        is Long -> println("Long")                   // 4
        !is String -> println("Not a string")        // 5
        else -> println("Unknown")                   // 6
    }   
}
```
### When Expression
```
fun main() {
    println(whenAssign("Hello"))
    println(whenAssign(3.4))
    println(whenAssign(1))
    println(whenAssign(MyClass()))
}

fun whenAssign(obj: Any): Any {
    val result = when (obj) {                   // 1
        1 -> "one"                              // 2
        "Hello" -> 1                            // 3
        is Long -> false                        // 4
        else -> 42                              // 5
    }
    return result
}

```

## Loops
### for
```
val cakes = listOf("carrot", "cheese", "chocolate")

for (cake in cakes) {                               // 1
    println("Yummy, it's a $cake cake!")
}

```
### while and do-while
while and do-while constructs work similarly to most languages as well.

### Iterators
```
class Animal(val name: String)

class Zoo(val animals: List<Animal>) {

    operator fun iterator(): Iterator<Animal> {             // 1
        return animals.iterator()                           // 2
    }
}

fun main() {

    val zoo = Zoo(listOf(Animal("zebra"), Animal("lion")))

    for (animal in zoo) {                                   // 3
        println("Watch out, it's a ${animal.name}")
    }

}
```
## Ranges

```
for(i in 0..3) {             // 1
    print(i)
}
print(" ")

for(i in 0 until 3) {        // 2
    print(i)
}
print(" ")

for(i in 2..8 step 2) {      // 3
    print(i)
}
print(" ")

for (i in 3 downTo 0) {      // 4
    print(i)
}
print(" ")
```
## Equality Checks
Kotlin uses == for structural comparison and === for referential comparison.
More precisely, a == b compiles down to if (a == null) b == null else a.equals(b).

## Conditional Expression
没有三目啊
```
fun max(a: Int, b: Int) = if (a > b) a else b         // 1

println(max(99, -42))
```
# Special Classes
## Data Classes
Data classes make it easy to create classes that are used to store values. Such classes are automatically provided with methods for copying, getting a string representation, and using instances in collections. You can override these methods with your own implementations in the class declaration.

## Enum Classes
```
enum class State {
    IDLE, RUNNING, FINISHED                           // 1
}

fun main() {
    val state = State.RUNNING                         // 2
    val message = when (state) {                      // 3
        State.IDLE -> "It's idle"
        State.RUNNING -> "It's running"
        State.FINISHED -> "It's finished"
    }
    println(message)
}
```

```
enum class Color(val rgb: Int) {                      // 1
    RED(0xFF0000),                                    // 2
    GREEN(0x00FF00),
    BLUE(0x0000FF),
    YELLOW(0xFFFF00);

    fun containsRed() = (this.rgb and 0xFF0000 != 0)  // 3
}

fun main() {
    val red = Color.RED
    println(red)                                      // 4
    println(red.containsRed())                        // 5
    println(Color.BLUE.containsRed())                 // 6
    println(Color.YELLOW.containsRed())               // 7
}
```

## Sealed Classes
Sealed classes let you restrict the use of inheritance. Once you declare a class sealed, it can only be subclassed from inside the same package where the sealed class is declared. It cannot be subclassed outside of the package where the sealed class is declared.

```
xsealed class Mammal(val name: String)                                                   // 1

class Cat(val catName: String) : Mammal(catName)                                        // 2
class Human(val humanName: String, val job: String) : Mammal(humanName)
```
## Object Keyword
### object Expression
```
fun rentPrice(standardDays: Int, festivityDays: Int, specialDays: Int): Unit {  //1

    val dayRates = object {                                                     //2
        var standard: Int = 30 * standardDays
        var festivity: Int = 50 * festivityDays
        var special: Int = 100 * specialDays
    }

    val total = dayRates.standard + dayRates.festivity + dayRates.special       //3

    print("Total price: $$total")                                               //4

}

fun main() {
    rentPrice(10, 2, 1)                                                         //5
}
```
### object Declaration

```
object DoAuth {                                                 //1 
    fun takeParams(username: String, password: String) {        //2 
        println("input Auth parameters = $username:$password")
    }
}

fun main(){
    DoAuth.takeParams("foo", "qwerty")                          //3
}
```
### Companion Onjects
```
class BigBen {                                  //1 
    companion object Bonger {                   //2
        fun getBongs(nTimes: Int) {             //3
            for (i in 1 .. nTimes) {
                print("BONG ")
            }
        }
    }
}

fun main() {
    BigBen.getBongs(12)                         //4
}
```
# Functional
## Higher-Order Functions
### Taking Functions as Parameters
```
fun calculate(x: Int, y: Int, operation: (Int, Int) -> Int): Int {  // 1
    return operation(x, y)                                          // 2
}

fun sum(x: Int, y: Int) = x + y                                     // 3

fun main() {
    val sumResult = calculate(4, 5, ::sum)                          // 4
    val mulResult = calculate(4, 5) { a, b -> a * b }               // 5
    println("sumResult $sumResult, mulResult $mulResult")
}
```

Invokes the higher-order function passing in two integer values and the function argument ::sum. :: is the notation that references a function by name in Kotlin.
Invokes the higher-order function passing in a lambda as a function argument. Looks clearer, doesn't it?

### Returning Functions
```
fun operation(): (Int) -> Int {                                     // 1
    return ::square
}

fun square(x: Int) = x * x                                          // 2

fun main() {
    val func = operation()                                          // 3
    println(func(2))                                                // 4
}
```

## Lambda Functions

```
val upperCase1: (String) -> String = { str: String -> str.uppercase() } // 1

val upperCase2: (String) -> String = { str -> str.uppercase() }         // 2

val upperCase3 = { str: String -> str.uppercase() }                     // 3

// val upperCase4 = { str -> str.uppercase() }                          // 4

val upperCase5: (String) -> String = { it.uppercase() }                 // 5

val upperCase6: (String) -> String = String::uppercase                  // 6

println(upperCase1("hello"))
println(upperCase2("hello"))
println(upperCase3("hello"))
println(upperCase5("hello"))
println(upperCase6("hello"
```

### Extension Functions and Properties


Kotlin lets you add new members to any class with the extensions mechanism. Namely, there are two types of extensions: extension functions and extension properties. They look a lot like normal functions and properties but with one important difference: you need to specify the type that you extend.
```
data class Item(val name: String, val price: Float)                                         // 1  

data class Order(val items: Collection<Item>)  

fun Order.maxPricedItemValue(): Float = this.items.maxByOrNull { it.price }?.price ?: 0F    // 2  
fun Order.maxPricedItemName() = this.items.maxByOrNull { it.price }?.name ?: "NO_PRODUCTS"

val Order.commaDelimitedItemNames: String                                                   // 3
    get() = items.map { it.name }.joinToString()

fun main() {

    val order = Order(listOf(Item("Bread", 25.0F), Item("Wine", 29.0F), Item("Water", 12.0F)))
    
    println("Max priced item name: ${order.maxPricedItemName()}")                           // 4
    println("Max priced item value: ${order.maxPricedItemValue()}")
    println("Items: ${order.commaDelimitedItemNames}")                                      // 5

}
```
# Collections
## List

```
val systemUsers: MutableList<Int> = mutableListOf(1, 2, 3)        // 1
val sudoers: List<Int> = systemUsers                              // 2

fun addSystemUser(newUser: Int) {                                 // 3
    systemUsers.add(newUser)                      
}

fun getSysSudoers(): List<Int> {                                  // 4
    return sudoers
}

fun main() {
    addSystemUser(4)                                              // 5 
    println("Tot sudoers: ${getSysSudoers().size}")               // 6
    getSysSudoers().forEach {                                     // 7
        i -> println("Some useful info on user $i")
    }
    // getSysSudoers().add(5) <- Error!                           // 8
}
```
## Set

A set is an unordered collection that does not support duplicates. For creating sets, there are functions setOf() and mutableSetOf(). A read-only view of a mutable set can be obtained by casting it to Set.

## Map

A map is a collection of key/value pairs, where each key is unique and is used to retrieve the corresponding value. For creating maps, there are functions mapOf() and mutableMapOf(). Using the to infix function makes initialization less noisy. A read-only view of a mutable map can be obtained by casting it to Map.
```
const val POINTS_X_PASS: Int = 15
val EZPassAccounts: MutableMap<Int, Int> = mutableMapOf(1 to 100, 2 to 100, 3 to 100)   // 1
val EZPassReport: Map<Int, Int> = EZPassAccounts                                        // 2

fun updatePointsCredit(accountId: Int) {
    if (EZPassAccounts.containsKey(accountId)) {                                        // 3
        println("Updating $accountId...")                                               
        EZPassAccounts[accountId] = EZPassAccounts.getValue(accountId) + POINTS_X_PASS  // 4
    } else {
        println("Error: Trying to update a non-existing account (id: $accountId)")
    } 
}

fun accountsReport() {
    println("EZ-Pass report:")
    EZPassReport.forEach {                                                              // 5
        k, v -> println("ID $k: credit $v")
    }
}

fun main() {
    accountsReport()                                                                    // 6
    updatePointsCredit(1)                                                               // 7
    updatePointsCredit(1)                                                               
    updatePointsCredit(5)                                                               // 8 
    accountsReport()                                                                    // 9
}
```

## fliter

```
val numbers = listOf(1, -2, 3, -4, 5, -6)      // 1

val positives = numbers.filter { x -> x > 0 }  // 2

val negatives = n
```
## map 

```
val numbers = listOf(1, -2, 3, -4, 5, -6)     // 1

val doubled = numbers.map { x -> x * 2 }      // 2

val tripled = numbers.map { it * 3 }          // 3
```
## any ,all, none


### Function any
Function any returns true if the collection contains at least one element that matches the given predicate.

### Function all
Function all returns true if all elements in collection match the given predicate.

### Function none

Function none returns true if there are no elements that match the given predicate in the collection.
```
val numbers = listOf(1, -2, 3, -4, 5, -6)            // 1

val allEven = numbers.none { it % 2 == 1 }           // 2

val allLess6 = numbers.none { it > 6 }               // 3
```
##  find, findLast
find and findLast functions return the first or the last collection element that matches the given predicate. If there are no such elements, functions return null.

## first , last 

These functions return the first and the last element of the collection correspondingly. You can also use them with a predicate; in this case, they return the first or the last element that matches the given predicate.
If a collection is empty or doesn't contain elements matching the predicate, the functions throw NoSuchElementException.


## count 
count functions returns either the total number of elements in a collection or the number of elements matching the given predicate.
```
val numbers = listOf(1, -2, 3, -4, 5, -6)            // 1

val totalCount = numbers.count()                     // 2
val evenCount = numbers.count { it % 2 == 0 }  
```

## associateBy, groupBy
```
data class Person(val name: String, val city: String, val phone: String) // 1

val people = listOf(                                                     // 2
    Person("John", "Boston", "+1-888-123456"),
    Person("Sarah", "Munich", "+49-777-789123"),
    Person("Svyatoslav", "Saint-Petersburg", "+7-999-456789"),
    Person("Vasilisa", "Saint-Petersburg", "+7-999-123456"))

val phoneBook = people.associateBy { it.phone }                          // 3
val cityBook = people.associateBy(Person::phone, Person::city)           // 4
val peopleCities = people.groupBy(Person::city, Person::name)            // 5
val lastPersonCity = people.associateBy(Person::city, Person::name)      // 6

```
 1. Defines a data class that describes a person.
 2.  Defines a collection of people.
 3. Builds a map from phone numbers to their owners' information. it.phone is the keySelector here. The valueSelector is not provided, so the values of the map are Person objects themselves.
 4. Builds a map from phone numbers to the cities where owners live. Person::city is the valueSelector here, so the values of the map contain only cities.
 5. Builds a map that contains cities and people living there. The values of the map are lists of person names.
 6. Builds a map that contains cities and the last person living there. The values of the map are names of the last person living in each city.

## partition
The partition function splits the original collection into a pair of lists using a given predicate:
Elements for which the predicate is true.
Elements for which the predicate is false.

```
val numbers = listOf(1, -2, 3, -4, 5, -6)                // 1

val evenOdd = numbers.partition { it % 2 == 0 }           // 2
val (positives, negatives) = numbers.partition { it > 0 } // 3
```
## flatMap
flatMap transforms each element of a collection into an iterable object and builds a single list of the transformation results. The transformation is user-defined.

##  minOrNull, maxOrNull
minOrNull and maxOrNull functions return the smallest and the largest element of a collection. If the collection is empty, they return null.

## sorted
sorted returns a list of collection elements sorted according to their natural sort order (ascending).
sortedBy sorts elements according to natural sort order of the values returned by specified selector function.

```
val shuffled = listOf(5, 4, 2, 1, 3, -10)                   // 1
val natural = shuffled.sorted()                             // 2
val inverted = shuffled.sortedBy { -it }                    // 3
val descending = shuffled.sortedDescending()                // 4
val descendingBy = shuffled.sortedByDescending { abs(it)  } // 5
```
## Map Element Access
When applied to a map, [] operator returns the value corresponding to the given key, or null if there is no such key in the map.
getValue function returns an existing value corresponding to the given key or throws an exception if the key wasn't found. For maps created with withDefault, getValue returns the default value instead of throwing an exception.

## Zip 

```
val A = listOf("a", "b", "c")                  // 1
val B = listOf(1, 2, 3, 4)                     // 1

val resultPairs = A zip B                      // 2
val resultReduce = A.zip(B) { a, b -> "$a$b" } // 3
```
## getOrElse
getOrElse

```
    val list = listOf(0, 10, 20)
    println(list.getOrElse(1) { 42 })    // 1
    
    val map = mutableMapOf<String, Int?>()
    println(map.getOrElse("x") { 1 })       // 1


```
# Scope Functions
## let 
The Kotlin standard library function let can be used for scoping and null-checks. When called on an object, let executes the given block of code and returns the result of its last expression. The object is accessible inside the block by the reference it (by default) or a custom name.

##  run 

Like let, run is another scoping function from the standard library. Basically, it does the same: executes a code block and returns its result. The difference is that inside run the object is accessed by this. This is useful when you want to call the object's methods rather than pass it as an argument.

## with 
with is a non-extension function that can access members of its argument concisely: you can omit the instance name when referring to its members.

```
with(configuration) {
    println("$host:$port")
}

// instead of:
println("${configuration.host}:${configuration.port}")
```
## apply
apply executes a block of code on an object and returns the object itself. Inside the block, the object is referenced by this. This function is handy for initializing objects.

## also

also works like apply: it executes a given block and returns the object called. Inside the block, the object is referenced by it, so it's easier to pass it as an argument. This function is handy for embedding additional actions, such as logging in call chains.

val jake = Person("Jake", 30, "Android developer")   // 1
    .also {                                          // 2 
        writeCreationLog(it)                         // 3
    }
# Delegation

## Delegation pattern 
```
interface SoundBehavior {                                                          // 1
    fun makeSound()
}

class ScreamBehavior(val n:String): SoundBehavior {                                // 2
    override fun makeSound() = println("${n.toUpperCase()} !!!")
}

class RockAndRollBehavior(val n:String): SoundBehavior {                           // 2
    override fun makeSound() = println("I'm The King of Rock 'N' Roll: $n")
}

// Tom Araya is the "singer" of Slayer
class TomAraya(n:String): SoundBehavior by ScreamBehavior(n)                       // 3

// You should know ;)
class ElvisPresley(n:String): SoundBehavior by RockAndRollBehavior(n)              // 3

fun main() {
    val tomAraya = TomAraya("Thrash Metal")
    tomAraya.makeSound()                                                           // 4
    val elvisPresley = ElvisPresley("Dancin' to the Jailhouse Rock.")
    elvisPresley.makeSound()
}
```

## Delegated Properties

Kotlin provides a mechanism of delegated properties that allows delegating the calls of the property set and get methods to a certain object. The delegate object in this case should have the method getValue. For mutable properties, you'll also need setValue.

```
import kotlin.reflect.KProperty

class Example {
    var p: String by Delegate()                                               // 1

    override fun toString() = "Example Class"
}

class Delegate() {
    operator fun getValue(thisRef: Any?, prop: KProperty<*>): String {        // 2     
        return "$thisRef, thank you for delegating '${prop.name}' to me!"
    }

    operator fun setValue(thisRef: Any?, prop: KProperty<*>, value: String) { // 2
        println("$value has been assigned to ${prop.name} in $thisRef")
    }
}

fun main() {
    val e = Example()
    println(e.p)
    e.p = "NEW"
}
```

Delegates property p of type String to the instance of class Delegate. The delegate object is defined after the by keyword.
Delegation methods. The signatures of these methods are always as shown in the example. Implementations may contain any steps you need. For immutable properties only getValue is required.


# Productivity Boosters

## Named Arguments
As with most other programming languages (Java, C++, etc.), Kotlin supports passing arguments to methods and constructors according to the order they are defined. Kotlin also supports named arguments to allow clearer invocations and avoid mistakes with the order of arguments. Such mistakes are hard to find because they are not detected by the compiler, for example, when two sequential arguments have the same type.

## String Templates
String templates allow you to include variable references and expressions into strings. When the value of a string is requested (for example, by println), all references and expressions are substituted with actual values.

## Restructuring Declarations

```
fun findMinMax(list: List<Int>): Pair<Int, Int> { 
    // do the math
    return Pair(50, 100) 
}

fun main() {
    val (x, y, z) = arrayOf(5, 10, 15)                              // 1

    val map = mapOf("Alice" to 21, "Bob" to 25)
    for ((name, age) in map) {                                      // 2
        println("$name is $age years old")          
    }

    val (min, max) = findMinMax(listOf(100, 90, 50, 98, 76, 83))    // 3

}


```
```
data class User(val username: String, val email: String)    // 1

fun getUser() = User("Mary", "mary@somewhere.com")

fun main() {
    val user = getUser()
    val (username, email) = user                            // 2
    println(username == user.component1())                  // 3

    val (_, emailAddress) = getUser()                       // 4
    
}

```

```
class Pair<K, V>(val first: K, val second: V) {  // 1
    operator fun component1(): K {              
        return first
    }

    operator fun component2(): V {              
        return second
    }
}

fun main() {
    val (num, name) = Pair(1, "one")             // 2

    println("num = $num, name = $name")
}
```
## Smart Casts

The Kotlin compiler is smart enough to perform type casts automatically in most cases, including:
Casts from nullable types to their non-nullable counterparts.
Casts from a supertype to a subtype.

# Kotlin/JS﻿

## dynamic

```
val a: dynamic = "abc"                                               // 1
val b: String = a                                                    // 2

fun firstChar(s: String) = s[0]

println("${firstChar(a)} == ${firstChar(b)}")                        // 3

println("${a.charCodeAt(0, "dummy argument")} == ${b[0].toInt()}")   // 4

println(a.charAt(1).repeat(3))                                       // 5

fun plus(v: dynamic) = v + 2

println("2 + 2 = ${plus(2)}")                                        // 6
println("'2' + 2 = ${plus("2")}")
```

## JS function

You can inline JavaScript code into your Kotlin code using the js("…") function. This should be used with extreme care.

```
js("alert(\"alert from Kotlin!\")") // 1
```

```
fun main(){
    val json = js("{}")               // 1
    json.name = "Jane"                // 2
    json.hobby = "movies"

    println(JSON.stringify(json))     // 3
}
```
## External declarations
external keyword allows to declare existing JavaScript API in a type-safe way.

```
external fun alert(msg: String)   // 1

fun main() {
  alert("Hi!")                    // 2
}
```
## Canvas

## Html Builder

