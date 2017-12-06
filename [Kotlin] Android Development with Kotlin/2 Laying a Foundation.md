
# Android Development with Kotlin

## 2. Laying a Foundation

### Variables

1. var = mutable variable
2. val = immutable variable

```kotlin
fun main(args: Array<String>) {
  var fruit:String = "orange"
  fruit = "banana"
}

fun main(args: Array<String>) {
  val fruit:String = "orange"
  fruit = "banana" // error
}
```

```kotlin
val list = mutableListOf("a", "b", "c")
list = mutableListOf("d", "e") // error
list.remove("a")
```

| Definition                   | Reference can change | Object state can change |
|:-----------------------------|:---------------------|:------------------------|
| val = listOf(1, 2, 3)        | No                   | No                      |
| val = mutableListOf(1, 2, 3) | No                   | Yes                     |
| var = listOf(1, 2, 3)        | Yes                  | No                      |
| var = mutableListOf(1, 2, 3) | Yes                  | Yes                     |

### Type inference

```kotlin
var title: String
title = "Kotlin"

var title: String = "Kotlin"

var title = "Kotlin" // inferred type: String

title = 12 // error
```

```kotlin
var title: Any = "Kotlin" // explicitly assign type
title = 12
```

```kotlin
var persons = listOf<personInstance1, personInstance2) // inferred type: List<Person> ()

var pair = "Everest" to 8848 // inferred type: Pair<String, Int>
var pair2 = Pair("Everest", 8848) // same as above

var map = mapOf("Mount Everest" to 8848, "K2" to 4017) // inferred type: Map<String, Int>

var map2 = mapOf("Mount Everest" to 8848, "K2" to "4017") // inferred type: Map<String, Any>
```

```kotlin
// explicitly assign type if needed
var age: Int = 18
var age: Short = 18
var age: Long = 18
var age = 18L // same as above
```

```kotlin
val title // error: type cannot be inferred
```

### Strict null safety

```kotlin
val age: Int = null // error
val name: String? = null
```

```kotlin
val name: String? = null
name.toUpperCase() // error: reference may be null
```

```kotlin
var nullableVehicle: Vehicle?
var vehicle: Vehicle

nullableVehicle = vehicle
vehicle = nullableVehicle // error
```

| Declaration      | List can be null | Element can be null |
|:-----------------|:-----------------|:--------------------|
| ArrayList<Int>   | No               | No                  |
| ArrayList<Int>?  | Yes              | No                  |
| ArrayList<Int?>  | No               | Yes                 |
| ArrayList<Int?>? | Yes              | Yes                 |

Accessing value before declaration
```java
// Java -> runtime error (NullPointerExceptions)
@Override
public void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  savedInstanceState.getBoolean("locked");
}
```

```kotlin
// Kotlin -> compile error
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)
  savedInstanceState.getBoolean("key")
}
```

Fix (as in Java)
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)

  val locked: Boolean
  if(savedInstanceState != null)
    locked = savedInstanceState.getBoolean("locked")
  else
    locked = false
}
```

### Safe call

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)
  val locked: Boolean? = savedInstanceState?.getBoolean("locked")
}
```

```kotlin
// Java idiomatic - multiple checks
val quiz: Quiz = Quiz()
val correct: Boolean?

if(quiz.currentQuestion != null) {
  if(quiz.currentQuestion.answer != null) {
    // do something
  }
}

// Kotlin idiomatic - multiple call of save call operator
val quiz: Quiz = Quiz()

val correct = quiz.currentQuestion?.answer?.correct
// inferred type: Boolean?
```

### Elvis operator

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)

  val locked: Boolean = savedInstanceState?.getBoolean("locked") ?: false
}
```

```kotlin
val correct = quiz.currentQuestion?.answer?.correct ?: false
```

### Not null assertion

```kotlin
var y: String? = "foo"
var size: Int = y!!.length
// !! cast nullable to non-nullable
// if wrong, NullPointerException
```

### Let

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)

  savedInstanceState?.let {
    println(it.getBoolean("isLocked"))
  }
}
```

### Nullability and java

### Casts

#### Safe/unsafe cast operator

```java
// java
Fragment fragment = new ProductFragment();
ProductFragment productFragment = (ProductFragment) fragment;
```

**as** = unsafe cast operator
: Throws ClassCastException when casting impossible
```kotlin
val fragment: Fragment = ProductFragment()
val productFragment: ProductFragment = fragment as ProductFragment
```

```kotlin
val fragment: String = "ProductFragment"
val productFragment: ProductFragment = fragment as ProductFragment // error: ClassCastException
```

**as?** = safe cast operator
: Returns null when casting impossible
```kotlin
val fragment: String = "ProductFragment"
val productFragment: ProductFragment? = fragment as? ProductFragment
val productFragment: ProductFragment? = fragment as ProductFragment? // same as above

val productFragment: ProductFragment? = fragment as? ProductFragment ?: ProductFragment() // make sure value is assigned
```

### Smart casts

#### Type smart casts

```java
// java
if (animal instanceof Fish) {
  Fish fish = (Fish) animal;
  fish.isHungry();

  // or
  ((Fish) animal).isHungry();
}
```

Smart cast in scope

```kotlin
if (animal is Fish) {
  animal.isHungry() // already casted
}

animal.isHungry() // error: casting scope is limited
```

Smart cast outside scope

```kotlin
val fish: Fish?  = ...
if (animal !is Fish)
  return

animal.isHungry() // already casted
```

```kotlin
if (animal is Fish && animal.isHungry()) {
  println("Fish is hungry")
}
```

#### Non-nullable smart cast

```kotlin
val view: View? = findViewById(R.layout.activity_shop)
```

```kotlin
val view: View?

if (view != null) {
  view.isShown()
}

view.isShown() // error if view is nullable
```

Using if
```kotlin
fun setView(view: View?) {
  if (view == null)
    return

  view.isShown() // already casted
}
```

Using Elvis operator
```kotlin
fun verifyView(view: View?) {
  view ?: return

  view.isShown() // already casted
}
```

Using Elvis operator + throw an error
```kotlin
fun setView(view: View?) {
  view ?: throw RuntimeException("View is empty")

  view.isShown() // already casted
}
```

### Primitive data types
In Kotlin, everything is an object.

```kotlin
var weight: Int = 12 // primitive type
var weight: Int? = null // boxed integer (composite type)
```

```kotlin
var a: Int = 1 // primitive
var b: Int? = null // boxed
b = 12 // still boxed
```

#### Numbers

Table - number data types (same as java)

| Type   | Bit width |
|:-------|:----------|
| Double | 64        |
| Float  | 32        |
| Long   | 64        |
| Int    | 32        |
| Short  | 16        |
| Byte   | 8         |

No implicit conversion to bigger types
```kotlin
var weight: Int = 12
var truckWeight: Long = weight // error
```

Call type conversion methods (since everything is an object)
```kotlin
var weight: Int = 12
var truckWeight: Long = weight.toLong()
```

* toByte()
* toShort()
* toInt()
* toLong()
* toFloat()
* toDouble()
* toChar()

Using type inference
```kotlin
val a: Int = 1
val b = a + 1 // inferred type: Int
val b = a + 1L // inferred type: Long
```

Number literals
```kotlin
27 // Decimal by default
27L // Long
0x1B // Hexadecimal
0b11011 // Binary
27.5 // Double by default
27.5F // Float
```

#### Char

```kotlin
val char = 'a' // Char
val string = "a" // String
```

Escape sequences
* \t: Tabulator
* \b: Backspace
* \n: New line
* \r: New line
* \': Quote
* \": Double quote
* \\: Slash
* \$: Dollar character
* \u: Unicode escape sequence

```kotlin
var yinyang = '\u262F'
```

#### Arrays

Array<Int>: numbers array by default
```kotlin
val array = arrayOf(1, 2, 3) // inferred type: Array<Int>
```

Array containing *Short* or *Long*
```kotlin
val array2: Array<Short> = arrayOf(1, 2, 3)
val array3: Array<Long> = arrayOf(1, 2, 3)
```

Using specialized classes for faster performance
```kotlin
val array = shortArrayOf(1, 2, 3)
val array = intArrayOf(1, 2, 3)
val array = longArrayOf(1, 2, 3)
```

Difference between arrayOf and longArrayOf
```kotlin
val array = arrayOf(1, 2, 3) // Generic(boxed) = slower
val array = longArrayOf(1, 2, 3) // primitive = faster
```

Array of null
```kotlin
val array = arrayOfNulls(3) // [null, null, null]
```

Predefined size + lambda(anonymous function)
```kotlin
val array = Array (5) { it * 2 } // [0, 2, 4, 8 ,10]
```

Accessing element
```Kotlin
val array = arrayOf(1, 2, 3)
val element = array[1] // 2
```

#### The Boolean type

```kotlin
val isGrowing: Boolean = true
val isGrowing: Boolean? = null
```
Standard operations
* ||: Logical OR (lazily evaluated)
* &&: Logical AND (lazily evaluated)
* !: Negation operator

### Composite Data Types

#### Strings

```kotlin
val str = "abcd"
str[1] // b
```

Extension examples
```kotlin
val str = "abcd"
str.reversed() // "dcba"
str.takeLast(2) // "cd"
"john@test.com".substringBefore("@") // "john"
"john@test.com".startsWith("@") // false
```

String templates
```java
// java
String name = "Eva";
Int age = 27;
String message = "My name is" + name + "and I am" + age + "years old";
```

```kotlin
// kotlin
val name = "Eva"
val age = 27
val message = "My name is $name and I am $age years old"
val message2 = "My name has ${name.length} characters"
```

#### Ranges
: A sequence of values (under the hood, created using *rangeTo* operator)

```kotlin
val intRange = 1..4 // equivalent of i >= 1 && i <= 4
val charRange = 'b'..'g' // equivalent of letters 'b' to 'g'
```

Iterate over ranges
```kotlin
for (i in 1..5) print(i) // 1234
for (c in 'b'..'g') print(c) // bcdefg
```

Using with if
```kotlin
val weight = 52
val healthy = 50..75

if (weight in healthy)
  println("$weight is in $healthy range")
```

```kotlin
val c = 'k'
val alphabet = 'a'..'z'

if (c in alphabet)
  println("$c is character")
```

Ranges are closed (end inclusive)
```kotlin
for (i in 1..3) print(i) // 123
```

Incremental by default
```kotlin
for (i in 5..1) print(i) // nothing
```

Reverse order
```kotlin
for (i in 5 downTo 1) print(i) // 54321
```

Step
```kotlin
for (i in 3..6 step 2) print(i) // 35
```

Reverse order with step
```kotlin
for (i in 9 downTo 1 step 3) print(i) // 963
```

#### Collections

### Statements versus expressions

### Control flow

### The if statement

### When when expression

### Loops

#### The for loop

#### The while loop

#### Other iterations

#### Break and continue

### Exceptions

#### The try... catch block

### Compile-time constants
