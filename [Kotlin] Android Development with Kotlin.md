
# Android Development with Kotlin

## Beginning Your Kotlin Adventure

## Laying a Foundation

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
| ---------------------------- | -------------------- | ----------------------- |
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
