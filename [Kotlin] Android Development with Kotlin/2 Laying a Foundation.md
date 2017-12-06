
# Android Development with Kotlin

## 2. Laying a Foundation
___

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

Smart cast in if scope

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
