# Functions

### Default Parameter Values and Named Arguments

```run-kotlin
fun printMessage(message: String): Unit {                               // 1
    println(message)
}

fun printMessageWithPrefix(message: String, prefix: String = "Info") {  // 2
    println("[$prefix] $message")
}

fun sum(x: Int, y: Int): Int {                                          // 3
    return x + y
}

fun multiply(x: Int, y: Int) = x * y                                    // 4

fun main() {
    printMessage("Hello")                                               // 5                    
    printMessageWithPrefix("Hello", "Log")                              // 6
    printMessageWithPrefix("Hello")                                     // 7
    printMessageWithPrefix(prefix = "Log", message = "Hello")           // 8
    println(sum(1, 2))                                                  // 9
    println(multiply(2, 4))                                             // 10
}
```

1. 하나의 파라미터 message: `String` 을 받고 `Unit` 타입을 리턴하는 함수 (i.e., no return value).
2. 함수의 두번쨰 인자는 [함수인자에 기본값 주기](https://kotlinlang.org/docs/reference/functions.html#default-arguments) `Info` 를 준 함수. 리턴 타입은 Unit으로, 코드엔 작성되어 있지 않으나 생략되어있다. [Unit-returning functions](https://kotlinlang.org/docs/functions.html#unit-returning-functions)
3. Int 타입을 리턴하는 함수이다.
4. 한줄로 표현한 표현식(single-expression), Int 타입을 리턴한다 (추론식)
5. 1번 함수 호출 `Hello`.
6. 2번 함수 호출, 두개의 값을 넘기고있다.
7. 2번 함수 호출, 두번쨰 인자를 제외하고 첫번째 인자만 넣어 실행한다. 두번째 인자의 기본값은 `Info` 이다.
8. 2번 함수 호출, 파라미터의 이름에 값을 넣어 실행하고 있다. 순서가 바뀌었으나 이름을 지정해 값을 넘기기 때문에 문제가없다. [named arguments](https://kotlinlang.org/docs/reference/functions.html#named-arguments)
9. `sum` 함수의 결과를 출력한다.
10. `multiply` 함수의 결과를 출력한다.

### Infix 함수

Member functions and extensions with a single parameter can be turned into [infix functions](https://kotlinlang.org/docs/reference/functions.html#infix-notation).

```run-kotlin
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

1. Defines an infix extension function on `Int`.
2. Calls the infix function.
3. Creates a `Pair` by calling the infix function `to` from the standard library.
4. Here's your own implementation of `to` creatively called `onto`.
5. Infix notation also works on members functions (methods).
6. The containing class becomes the first parameter.

Note that the example uses [local functions](https://kotlinlang.org/docs/reference/functions.html#local-functions) (functions nested within another function).

### Operator Functions

Certain functions can be "upgraded" to [operators](https://kotlinlang.org/docs/reference/operator-overloading.html), allowing their calls with the corresponding operator symbol.

```run-kotlin
fun main() {
//sampleStart
  operator fun Int.times(str: String) = str.repeat(this)       // 1
  println(2 * "Bye ")                                          // 2

  operator fun String.get(range: IntRange) = substring(range)  // 3
  val str = "Always forgive your enemies; nothing annoys them so much."
  println(str[0..14])                                          // 4
//sampleEnd
}
```

1. This takes the infix function from above one step further using the `operator` modifier.
2. The operator symbol for `times()` is `*` so that you can call the function using `2 * "Bye"`.
3. An operator function allows easy range access on strings.
4. The `get()` operator enables [bracket-access syntax](https://kotlinlang.org/docs/reference/operator-overloading.html#indexed).

### Functions with `vararg` Parameters

[Varargs](https://kotlinlang.org/docs/reference/functions.html#variable-number-of-arguments-varargs) allow you to pass any number of arguments by separating them with commas.

```run-kotlin
fun main() {
//sampleStart
    fun printAll(vararg messages: String) {                            // 1
        for (m in messages) println(m)
    }
    printAll("Hello", "Hallo", "Salut", "Hola", "你好")                 // 2
    
    fun printAllWithPrefix(vararg messages: String, prefix: String) {  // 3
        for (m in messages) println(prefix + m)
    }
    printAllWithPrefix(
            "Hello", "Hallo", "Salut", "Hola", "你好",
            prefix = "Greeting: "                                          // 4
    )

    fun log(vararg entries: String) {
        printAll(*entries)                                             // 5
    }
//sampleEnd
}
```

1. The `vararg` modifier turns a parameter into a vararg.
2. This allows calling `printAll` with any number of string arguments.
3. Thanks to named parameters, you can even add another parameter of the same type after the vararg. This wouldn't be allowed in Java because there's no way to pass a value.
4. Using named parameters, you can set a value to `prefix` separately from the vararg.
5. At runtime, a vararg is just an array. To pass it along into a vararg parameter, use the special spread operator `*` that lets you pass in `*entries` (a vararg of `String`) instead of `entries` (an `Array<String>`).
